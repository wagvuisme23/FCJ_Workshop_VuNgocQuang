---
title : "Tạo Lambda"
displayDate :  "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 6. </b> "
---

## Tạo IAM Role cho Lambda (Console)

Lambda cần quyền đọc thông tin VPN (`ec2:DescribeVpnConnections`), gọi SSM `SendCommand` tới EC2, và ghi logs lên CloudWatch. Dưới đây là hướng dẫn từng bước chuẩn để tạo Role Lambda và gán policy tối thiểu.

### Chuẩn bị

* Tài khoản AWS có quyền tạo IAM Role, Lambda, CloudWatch, EventBridge, SSM, EC2.
* Biết ID AWS account (không bắt buộc để làm theo hướng dẫn này vì ta sẽ dùng Resource="\*").
* Quyết định tên role: `lambda-vpn-diagnostic-role` (có thể đổi tuỳ bạn).

### Tạo Role (IAM Console)

1. Mở **AWS Management Console** → **IAM** → **Roles** → **Create role**.
2. Chọn **Trusted entity type**: **AWS service** → **Use case**: **Lambda** → **Next**.
3. Ở phần **Attach permissions policies**, tìm và chọn managed policy: `AWSLambdaBasicExecutionRole` (để Lambda có quyền ghi CloudWatch Logs).
4. Click **Next** (bỏ qua phần add permissions optional).
5. Điền **Role name**: `lambda-vpn-diagnostic-role` → (Optional) thêm Tags → **Create role**.

### Thêm inline policy (minimal permissions)

1. Trong IAM → Roles → mở `lambda-vpn-diagnostic-role` → tab **Permissions** → click **Add inline policy**.
2. Chọn tab **JSON** và dán policy sau (ở môi trường production nên thay `Resource: "*"` bằng ARN cụ thể để theo principle of least privilege):

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

3. Click **Review policy** → đặt tên policy (ví dụ `lambda-vpn-diagnostic-inline`) → **Create policy**.

**Ghi chú bảo mật:**

* `sns:Publish` là tuỳ chọn — nếu bạn không dùng SNS để forward kết quả thì có thể bỏ block này.
* Trong production, giới hạn `Resource` tới ARN cụ thể (ví dụ `arn:aws:ec2:ap-southeast-1:123456789012:instance/i-0123...`) để tuân theo least-privilege.

---

## Tạo Lambda Function (Console)

Lambda sẽ nhận sự kiện từ EventBridge/CloudWatch Alarm, gọi SSM để chạy `ping`/`traceroute` từ EC2 test, và ghi kết quả về CloudWatch Logs (và có thể publish tới SNS nếu cần).

### Chuẩn bị

* EC2 test instance đã cài SSM Agent và có **IAM Instance Profile** gắn `AmazonSSMManagedInstanceCore`.
* Ghi ra: `VPN_ID` (Ví dụ `vpn-xxxxxxxx`), `EC2_INSTANCE_ID` (Ví dụ `i-0123456789abcdef0`), `ONPREM_PRIVATE_IP` (ví dụ `192.168.100.10`).

### Tạo function

1. Mở **AWS Console** → **Lambda** → **Create function** → **Author from scratch**.
2. Cấu hình:

   * Function name: `vpn-diagnostic-handler` (hoặc tên bạn chọn)
   * Runtime: **Python 3.11** (hoặc 3.9)
   * Permissions: **Use an existing role** → chọn `lambda-vpn-diagnostic-role` (role đã tạo ở phần C)
3. Click **Create function**.

### Cấu hình Environment variables

1. Vào function → tab **Configuration** → **Environment variables** → **Edit** → **Add environment variables**:

   * `VPN_ID` = `vpn-xxxxxxxx`
   * `EC2_INSTANCE_ID` = `i-0123456789abcdef0`
   * `ONPREM_PRIVATE_IP` = `192.168.100.10`
   * `REGION` = `ap-southeast-1` (hoặc region của bạn)
   * `FORWARD_SNS_ARN` = \`\` (để trống nếu không forward)
2. Save.

**Lưu ý:** không lưu Pre-shared Key (PSK) vào biến môi trường. Nếu cần lưu secret, dùng **AWS Secrets Manager** và đọc trong Lambda.

### Điều chỉnh timeout & memory

* Vào **Configuration → General configuration → Edit**

  * **Timeout**: đặt `120` seconds (tùy test, có thể 180s)
  * **Memory**: `256 MB`
  * Save.

### Dán mã Lambda (full code)

1. Trong tab **Code -> Code source**, chọn file `lambda_function.py` (hoặc inline editor) và dán đoạn code sau:

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

2. Click **Deploy** (nếu cần).

**Lưu ý:** kiểm tra script chạy được bằng cách test Lambda (xem mục Testing bên dưới). Nếu EC2 không có `traceroute`, cân nhắc thay bằng `tracepath` hoặc cài package `traceroute`.

---

## Tạo EventBridge Rule (bắt CloudWatch Alarm → gọi Lambda)

Thay vì dùng SNS, ta dùng **EventBridge** để bắt sự kiện Alarm state change và invoke Lambda.

### Tạo Rule

1. Mở **AWS Console** → **Amazon EventBridge** → **Rules** → **Create rule**.
2. Cấu hình:

   * **Name**: `alarm-vpn-tunnel1-to-lambda`
   * **Description**: `Forward CloudWatch alarm vpn-tunnel1-down to lambda diagnostic`
   * **Rule type**: **Event pattern** → chọn **Custom pattern (JSON)**.
3. Dán event pattern sau (thay `vpn-tunnel1-down` bằng tên alarm của bạn nếu khác):

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

4. **Select targets** → **Target type**: `Lambda function` → chọn `vpn-diagnostic-handler`.
5. Click **Create**.

> Khi bạn chọn Lambda làm target, EventBridge sẽ tự động thêm permission resource-based policy cho Lambda để EventBridge có thể invoke function. Nếu không thấy permission, xem phần troubleshooting.

---

## Testing / Kiểm thử

1. **Test qua EventBridge** (khuyến nghị):

   * EventBridge → Rules → chọn rule `alarm-vpn-tunnel1-to-lambda` → **Test** → **Create test event** → paste sample event JSON (mẫu có trong tài liệu workshop) → Send test event.
   * Kiểm tra Lambda → Monitoring → View logs in CloudWatch.

2. **Test invoke trực tiếp Lambda**:

   * Lambda → chọn function → **Test** → paste `{}` hoặc sample event → Run.

3. **End-to-end (thực tế)**:

   * Kích hoạt alarm thật (ví dụ chặn UDP 500/4500 outbound tới AWS\_T1\_OUT để TunnelState = 0) → khi Alarm chuyển ALARM, EventBridge sẽ invoke Lambda → Lambda gọi SSM → check SSM command output.

---

## Troubleshooting (thường gặp)

* **EventBridge không invoke Lambda**: kiểm tra resource-based policy của Lambda (Permissions → Resource-based policy). Nếu cần, thêm bằng CLI:

```bash
aws lambda add-permission \
  --function-name vpn-diagnostic-handler \
  --statement-id eventbridge-invoke \
  --action "lambda:InvokeFunction" \
  --principal events.amazonaws.com \
  --source-arn arn:aws:events:<region>:<account-id>:rule/alarm-vpn-tunnel1-to-lambda
```

* **SSM không thực thi lệnh**: kiểm tra EC2 có SSM Agent & Instance Profile

---