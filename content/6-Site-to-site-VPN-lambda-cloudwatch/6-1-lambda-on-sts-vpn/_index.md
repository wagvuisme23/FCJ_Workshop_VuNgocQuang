---
title : "Create Lambda"
displayDate :  "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 6.1 </b> "
---

## IAM Role for Lambda (Detailed)

This section provides **very detailed** step-by-step guidance to create an IAM Role for Lambda to perform diagnostics when a VPN tunnel is down. The role will have:

- Managed policy `AWSLambdaBasicExecutionRole` (allows writing to CloudWatch Logs),
- Minimal inline policy allowing `ec2:DescribeVpnConnections`, `ssm:SendCommand`, `ssm:GetCommandInvocation`, and (optional) `sns:Publish`.

> **Important:** In a production environment, restrict the `Resource` in the policy to specific ARNs to follow the *least privilege* principle.

### Open IAM Console and start creating the Role

1. Log in to **AWS Management Console** → search for **IAM** → go to **Roles**.
2. Click **Create role**.
3. Select **AWS service** → **Lambda** → **Next**.

### Attach Managed Policy for logging

1. On the **Attach permissions policies** page, search for and tick **AWSLambdaBasicExecutionRole**.
2. Click **Next**.

### Enter name and create the role

1. **Role name**: `lambda-vpn-diagnostic-role` (or any name you prefer).
2. (Optional) Add tags for management (team, project).
3. Click **Create role**.

### Add inline policy (minimal permissions)

1. In IAM → Roles → open `lambda-vpn-diagnostic-role` → **Permissions** tab → click **Add inline policy**.
2. Select the **JSON** tab and paste the following policy (sample; replace `Resource` with specific ARNs for production):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "EC2DescribeAndVPN",
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeVpnConnections",
        "ec2:DescribeInstances"
      ],
      "Resource": "*"
    },
    {
      "Sid": "SSMRunCommands",
      "Effect": "Allow",
      "Action": [
        "ssm:SendCommand",
        "ssm:GetCommandInvocation",
        "ssm:ListCommandInvocations",
        "ssm:ListCommands"
      ],
      "Resource": [
        "arn:aws:ec2:*:*:instance/*",
        "arn:aws:ssm:*:*:document/AWS-RunShellScript",
        "arn:aws:ssm:*:*:managed-instance/*"
      ]
    },
    {
      "Sid": "OptionalSNSPublish",
      "Effect": "Allow",
      "Action": [
        "sns:Publish"
      ],
      "Resource": "*"
    }
  ]
}
```


3. Click **Review policy** → name it (e.g., `lambda-vpn-diagnostic-inline`) → **Create policy**.

**Note:** If you are not using SNS to forward results, remove the `OptionalSNSPublish` block.

### Check Trust Relationship

1. In IAM → Roles → select `lambda-vpn-diagnostic-role` → **Trust relationships** tab.
2. The trust policy must allow `lambda.amazonaws.com` to assume the role. Example:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": { "Service": "lambda.amazonaws.com" },
      "Action": "sts:AssumeRole"
    }
  ]
}
```


3. If it’s different, click **Edit trust relationship** and update.

### (Optional) Create via CLI

If you prefer scripting:

```bash
# Tạo role
aws iam create-role --role-name lambda-vpn-diagnostic-role --assume-role-policy-document file://trust.json
# Attach managed policy
aws iam attach-role-policy --role-name lambda-vpn-diagnostic-role --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole
# Put inline policy
aws iam put-role-policy --role-name lambda-vpn-diagnostic-role --policy-name lambda-vpn-diagnostic-inline --policy-document file://inline-policy.json
```

---

## Create Lambda function (vpn-diagnostic-handler)

Step-by-step guidance to create a Lambda function, configure environment variables, timeout/memory, paste code, deploy, and test — all within the **AWS Management Console**.

### Preparation

- `VPN_ID` (e.g., `vpn-0abcd1234`) — will be used in `ec2.describe_vpn_connections`.
- `EC2_INSTANCE_ID` (e.g., `i-0123456789abcdef0`) — EC2 must be an **SSM managed instance** (SSM Agent + IAM Instance Profile `AmazonSSMManagedInstanceCore`).
- `ONPREM_PRIVATE_IP` (e.g., `192.168.100.10`) — On-premises internal IP to ping/traceroute.
- `REGION` (e.g., `ap-southeast-1`).
- IAM Role: `lambda-vpn-diagnostic-role` (created in section L).

### Create the function

1. Open **AWS Console** → **Lambda** → **Create function**.
2. Choose **Author from scratch**.
3. Enter:

    - Function name: `vpn-diagnostic-handler`
    - Runtime: `Python 3.11` (or 3.9)
    - Permissions: **Use an existing role** → choose `lambda-vpn-diagnostic-role`.
4. Click **Create function**.

> Note: Create the function in the **same region** as your EC2/VPN to avoid cross-region issues.

### Set Environment variables

1. In the function → **Configuration** → **Environment variables** → **Edit** → **Add environment variables**:

   - `VPN_ID` = `vpn-xxxxxxxx`
   - `EC2_INSTANCE_ID` = `i-0123456789abcdef0`
   - `ONPREM_PRIVATE_IP` = `192.168.100.10`
   - `REGION` = `ap-southeast-1`
   - `FORWARD_SNS_ARN` = `` (if you want to publish a summary to SNS, set ARN)
2. Save.

**Security:** Do not store the Pre-Shared Key (PSK) in environment variables. If PSK or webhook secrets are needed, store them in **AWS Secrets Manager**.

### Configure Timeout & Memory

1. Go to **Configuration → General configuration → Edit**:

   - Timeout: `120` seconds (increase if you want longer waits)
   - Memory: `256 MB`
2. Save.

### Paste Lambda code

1. In **Code → Code source**, open `lambda_function.py` (editor) and paste the following code:

```python
import os
import time
import json
import boto3
import logging

logger = logging.getLogger()
logger.setLevel(logging.INFO)

ssm = boto3.client('ssm')
ec2 = boto3.client('ec2')
sns = boto3.client('sns')

# Read env
VPN_ID = os.environ.get('VPN_ID')
EC2_INSTANCE_ID = os.environ.get('EC2_INSTANCE_ID')
ONPREM_PRIVATE_IP = os.environ.get('ONPREM_PRIVATE_IP')
REGION = os.environ.get('REGION') or boto3.session.Session().region_name
FORWARD_SNS_ARN = os.environ.get('FORWARD_SNS_ARN')  # optional


def run_ssm_command(instance_id, commands, timeout=60):
    resp = ssm.send_command(
        InstanceIds=[instance_id],
        DocumentName="AWS-RunShellScript",
        Parameters={"commands": commands},
        TimeoutSeconds=timeout
    )
    cmd_id = resp['Command']['CommandId']
    logger.info(f"SSM command sent: CommandId={cmd_id}")
    end_time = time.time() + timeout
    while time.time() < end_time:
        try:
            inv = ssm.get_command_invocation(CommandId=cmd_id, InstanceId=instance_id)
            status = inv.get('Status')
            logger.info(f"Invocation status: {status}")
            if status in ('Success', 'Failed', 'Cancelled', 'TimedOut'):
                return inv
        except ssm.exceptions.InvocationDoesNotExist:
            pass
        time.sleep(1)
    raise TimeoutError("SSM command invocation timed out")


def describe_vpn(vpn_id):
    resp = ec2.describe_vpn_connections(VpnConnectionIds=[vpn_id])
    return resp.get('VpnConnections', [])


def build_response_summary(event, vpn_info, ssm_result):
    summary = {
        "event": event if isinstance(event, dict) else str(event),
        "vpn_info": vpn_info,
        "ssm_result": {
            "Status": ssm_result.get('Status') if isinstance(ssm_result, dict) else ssm_result,
            "StandardOutputContent": ssm_result.get('StandardOutputContent') if isinstance(ssm_result, dict) else None,
            "StandardErrorContent": ssm_result.get('StandardErrorContent') if isinstance(ssm_result, dict) else None
        }
    }
    return summary


def lambda_handler(event, context):
    logger.info("Received event: %s", json.dumps(event))
    try:
        alarm_name = event.get('detail', {}).get('alarmName') or event.get('AlarmName')
    except Exception:
        alarm_name = None

    logger.info(f"Alarm name: {alarm_name}")

    vpn_info = []
    try:
        if VPN_ID:
            vpn_info = describe_vpn(VPN_ID)
        else:
            vpn_info = {"error": "VPN_ID not set in environment"}
    except Exception as e:
        logger.exception("Error describing VPN: %s", e)
        vpn_info = {"error": str(e)}

    ssm_result = {}
    if EC2_INSTANCE_ID and ONPREM_PRIVATE_IP:
        commands = [
            f"echo '==== ping to {ONPREM_PRIVATE_IP} ===='",
            f"ping -c 10 {ONPREM_PRIVATE_IP} || true",
            f"echo '==== traceroute to {ONPREM_PRIVATE_IP} ===='",
            f"traceroute -n {ONPREM_PRIVATE_IP} || true"
        ]
        try:
            inv = run_ssm_command(EC2_INSTANCE_ID, commands, timeout=120)
            ssm_result = {
                "Status": inv.get('Status'),
                "StandardOutputContent": inv.get('StandardOutputContent'),
                "StandardErrorContent": inv.get('StandardErrorContent')
            }
        except Exception as e:
            logger.exception("SSM command failed: %s", e)
            ssm_result = {"error": str(e)}
    else:
        ssm_result = {"error": "EC2_INSTANCE_ID or ONPREM_PRIVATE_IP not configured"}

    summary = build_response_summary(alarm_name or event, vpn_info, ssm_result)
    logger.info("Diagnostic summary: %s", json.dumps(summary))

    if FORWARD_SNS_ARN:
        try:
            sns.publish(TopicArn=FORWARD_SNS_ARN, Message=json.dumps(summary), Subject=f"VPN Diagnostic: {alarm_name}")
        except Exception as e:
            logger.exception("Failed to publish SNS: %s", e)

    return {
        "statusCode": 200,
        "body": summary
    }
```


2. Click **Deploy** (top right) to save the code.

### Set resource-based policy for Lambda (EventBridge invoke)

- Normally, EventBridge will automatically add the invoke permission. If EventBridge cannot invoke the Lambda, manually add permission via CLI:

```bash
aws lambda add-permission \
  --function-name vpn-diagnostic-handler \
  --statement-id eventbridge-invoke \
  --action "lambda:InvokeFunction" \
  --principal events.amazonaws.com \
  --source-arn arn:aws:events:<region>:<account-id>:rule/alarm-vpn-tunnel1-to-lambda
```

### Basic Lambda test

1. In Lambda → select `vpn-diagnostic-handler` → **Test**.
2. Create a test event (e.g., `{}` or a sample CloudWatch Alarm event).
3. Run → check **Monitoring → View logs in CloudWatch** to view output and SSM invocation details.

### Quick Troubleshooting

- If SSM invocation returns an error: Check EC2 instance role (AmazonSSMManagedInstanceCore), SSM Agent installation, and VPC access (Internet or VPC endpoint).
- If `ec2.describe_vpn_connections` returns a permission error: Check if the inline policy is attached.

---

## Create Lambda Function (Console)

Lambda will receive events from EventBridge/CloudWatch Alarm, call SSM to run `ping`/`traceroute` from the test EC2, and log the results to CloudWatch Logs (and optionally publish to SNS if needed).

### Preparation

- Test EC2 instance with SSM Agent installed and **IAM Instance Profile** attached with `AmazonSSMManagedInstanceCore`.
- Note: `VPN_ID` (e.g., `vpn-xxxxxxxx`), `EC2_INSTANCE_ID` (e.g., `i-0123456789abcdef0`), `ONPREM_PRIVATE_IP` (e.g., `192.168.100.10`).

### Create the function

1. Open **AWS Console** → **Lambda** → **Create function** → **Author from scratch**.
2. Configure:

   - Function name: `vpn-diagnostic-handler` (or any name you choose)
   - Runtime: **Python 3.11** (or 3.9)
   - Permissions: **Use an existing role** → choose `lambda-vpn-diagnostic-role` (created in section C)
3. Click **Create function**.

### Configure Environment variables

1. In the function → **Configuration** tab → **Environment variables** → **Edit** → **Add environment variables**:

   - `VPN_ID` = `vpn-xxxxxxxx`
   - `EC2_INSTANCE_ID` = `i-0123456789abcdef0`
   - `ONPREM_PRIVATE_IP` = `192.168.100.10`
   - `REGION` = `ap-southeast-1` (or your region)
   - `FORWARD_SNS_ARN` = `` (leave blank if not forwarding)
2. Save.

**Note:** Do not store the Pre-Shared Key (PSK) in environment variables. If secrets are needed, use **AWS Secrets Manager** and read them in Lambda.

### Adjust timeout & memory

- Go to **Configuration → General configuration → Edit**

    - **Timeout**: set `120` seconds (or 180s for longer tests)
    - **Memory**: `256 MB`
    - Save.

### Paste Lambda code (full code)

1. In the **Code -> Code source** tab, open `lambda_function.py` (or inline editor) and paste the following code:


```python
import os
import time
import json
import boto3
import logging

logger = logging.getLogger()
logger.setLevel(logging.INFO)

ssm = boto3.client('ssm')
ec2 = boto3.client('ec2')
sns = boto3.client('sns')

# Read env
VPN_ID = os.environ.get('VPN_ID')
EC2_INSTANCE_ID = os.environ.get('EC2_INSTANCE_ID')
ONPREM_PRIVATE_IP = os.environ.get('ONPREM_PRIVATE_IP')
REGION = os.environ.get('REGION') or boto3.session.Session().region_name
FORWARD_SNS_ARN = os.environ.get('FORWARD_SNS_ARN')  # optional


def run_ssm_command(instance_id, commands, timeout=60):
    resp = ssm.send_command(
        InstanceIds=[instance_id],
        DocumentName="AWS-RunShellScript",
        Parameters={"commands": commands},
        TimeoutSeconds=timeout
    )
    cmd_id = resp['Command']['CommandId']
    logger.info(f"SSM command sent: CommandId={cmd_id}")
    end_time = time.time() + timeout
    while time.time() < end_time:
        try:
            inv = ssm.get_command_invocation(CommandId=cmd_id, InstanceId=instance_id)
            status = inv.get('Status')
            logger.info(f"Invocation status: {status}")
            if status in ('Success', 'Failed', 'Cancelled', 'TimedOut'):
                return inv
        except ssm.exceptions.InvocationDoesNotExist:
            pass
        time.sleep(1)
    raise TimeoutError("SSM command invocation timed out")


def describe_vpn(vpn_id):
    resp = ec2.describe_vpn_connections(VpnConnectionIds=[vpn_id])
    return resp.get('VpnConnections', [])


def build_response_summary(event, vpn_info, ssm_result):
    summary = {
        "event": event if isinstance(event, dict) else str(event),
        "vpn_info": vpn_info,
        "ssm_result": {
            "Status": ssm_result.get('Status') if isinstance(ssm_result, dict) else ssm_result,
            "StandardOutputContent": ssm_result.get('StandardOutputContent') if isinstance(ssm_result, dict) else None,
            "StandardErrorContent": ssm_result.get('StandardErrorContent') if isinstance(ssm_result, dict) else None
        }
    }
    return summary


def lambda_handler(event, context):
    logger.info("Received event: %s", json.dumps(event))
    try:
        alarm_name = event.get('detail', {}).get('alarmName') or event.get('AlarmName')
    except Exception:
        alarm_name = None

    logger.info(f"Alarm name: {alarm_name}")

    vpn_info = []
    try:
        if VPN_ID:
            vpn_info = describe_vpn(VPN_ID)
        else:
            vpn_info = {"error": "VPN_ID not set in environment"}
    except Exception as e:
        logger.exception("Error describing VPN: %s", e)
        vpn_info = {"error": str(e)}

    ssm_result = {}
    if EC2_INSTANCE_ID and ONPREM_PRIVATE_IP:
        commands = [
            f"echo '==== ping to {ONPREM_PRIVATE_IP} ===='",
            f"ping -c 10 {ONPREM_PRIVATE_IP} || true",
            f"echo '==== traceroute to {ONPREM_PRIVATE_IP} ===='",
            f"traceroute -n {ONPREM_PRIVATE_IP} || true"
        ]
        try:
            inv = run_ssm_command(EC2_INSTANCE_ID, commands, timeout=120)
            ssm_result = {
                "Status": inv.get('Status'),
                "StandardOutputContent": inv.get('StandardOutputContent'),
                "StandardErrorContent": inv.get('StandardErrorContent')
            }
        except Exception as e:
            logger.exception("SSM command failed: %s", e)
            ssm_result = {"error": str(e)}
    else:
        ssm_result = {"error": "EC2_INSTANCE_ID or ONPREM_PRIVATE_IP not configured"}

    summary = build_response_summary(alarm_name or event, vpn_info, ssm_result)
    logger.info("Diagnostic summary: %s", json.dumps(summary))

    if FORWARD_SNS_ARN:
        try:
            sns.publish(TopicArn=FORWARD_SNS_ARN, Message=json.dumps(summary), Subject=f"VPN Diagnostic: {alarm_name}")
        except Exception as e:
            logger.exception("Failed to publish SNS: %s", e)

    return {
        "statusCode": 200,
        "body": summary
    }
```

2. Click **Deploy** (if needed).

**Note:** Verify the script runs by testing Lambda (see Testing section below). If EC2 does not have `traceroute`, consider replacing with `tracepath` or installing the `traceroute` package.

---

## Create EventBridge Rule (capture CloudWatch Alarm → invoke Lambda)

Instead of using SNS, we will use **EventBridge** to capture the Alarm state change event and invoke Lambda.

### Create the Rule

1. Open **AWS Console** → **Amazon EventBridge** → **Rules** → **Create rule**.
2. Configure:

    - **Name**: `alarm-vpn-tunnel1-to-lambda`
    - **Description**: `Forward CloudWatch alarm vpn-tunnel1-down to lambda diagnostic`
    - **Rule type**: **Event pattern** → choose **Custom pattern (JSON)**.
3. Paste the following event pattern (replace `vpn-tunnel1-down` with your alarm name if different):

```json
{
  "source": ["aws.cloudwatch"],
  "detail-type": ["CloudWatch Alarm State Change"],
  "detail": {
    "alarmName": ["vpn-tunnel1-down"],
    "state": {
      "value": ["ALARM"]
    }
  }
}
```

4. **Select targets** → **Target type**: `Lambda function` → select `vpn-diagnostic-handler`.
5. Click **Create**.

> When you select Lambda as the target, EventBridge automatically adds the resource-based policy permission for Lambda to be invoked by EventBridge. If the permission is missing, see troubleshooting section.

---
