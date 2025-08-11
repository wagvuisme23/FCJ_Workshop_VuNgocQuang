---
title : "Clean Up Resources"
displayDate :  "`r Sys.Date()`"
weight : 7
chapter : false
pre : " <b> 7. </b> "
---

### Delete VPC Endpoints

1. Go to the Endpoints section in the VPC Console
    - Select **Action**
    - Select **Delete VPC endpoints**
    - Enter “delete” to confirm

![Clean Up Resources](/FCJ_Workshop_VuNgocQuang/images/7/0001.png?featherlight=false&width=90pc)

---

### Delete VPN Resources

ℹ️ Info: VPN resources need to be deleted in the proper order to avoid dependency errors.

1. Delete VPN Site-to-Site connection

![Clean Up Resources](/FCJ_Workshop_VuNgocQuang/images/7/0002.png?featherlight=false&width=90pc)

![Clean Up Resources](/FCJ_Workshop_VuNgocQuang/images/7/0003.png?featherlight=false&width=90pc)

2. Delete Virtual Private Gateway
    - First, detach the Virtual Private Gateway from the VPC (if attached)
    - Then delete the Virtual Private Gateway

![Clean Up Resources](/FCJ_Workshop_VuNgocQuang/images/7/0004.png?featherlight=false&width=90pc)

![Clean Up Resources](/FCJ_Workshop_VuNgocQuang/images/7/0005.png?featherlight=false&width=90pc)

![Clean Up Resources](/FCJ_Workshop_VuNgocQuang/images/7/0006.png?featherlight=false&width=90pc)

![Clean Up Resources](/FCJ_Workshop_VuNgocQuang/images/7/0007.png?featherlight=false&width=90pc)

3. Delete Customer Gateway

![Clean Up Resources](/FCJ_Workshop_VuNgocQuang/images/7/0008.png?featherlight=false&width=90pc)

![Clean Up Resources](/FCJ_Workshop_VuNgocQuang/images/7/0009.png?featherlight=false&width=90pc)

---

### Delete VPC

1. Delete VPC ASG VPN

![Clean Up Resources](/FCJ_Workshop_VuNgocQuang/images/7/0010.png?featherlight=false&width=90pc)

![Clean Up Resources](/FCJ_Workshop_VuNgocQuang/images/7/0011.png?featherlight=false&width=90pc)

2. Delete VPC ASG

![Clean Up Resources](/FCJ_Workshop_VuNgocQuang/images/7/0012.png?featherlight=false&width=90pc)

![Clean Up Resources](/FCJ_Workshop_VuNgocQuang/images/7/0013.png?featherlight=false&width=90pc)

---

### Delete Alarms in CloudWatch Metrics

1. Select **All alarms** on the left under **Alarms**, choose the Alarm to delete, then select **Action** → **Delete**

![Clean Up Resources](/FCJ_Workshop_VuNgocQuang/images/7/0014.png?featherlight=false&width=90pc)

![Clean Up Resources](/FCJ_Workshop_VuNgocQuang/images/7/0015.png?featherlight=false&width=90pc)

---

### Delete EventBridge Rule

1. Open **AWS Management Console**.
2. Search for and select the **Amazon EventBridge** service.
3. In the left menu, select **Rules**.
4. From the rules list, find the **Rule** created during the workshop (e.g., `Alarm-to-Lambda-Rule`).
5. Check the checkbox for that rule.
6. In the top-right corner, select **Actions** → **Delete**.
7. A confirmation dialog will appear, enter **Delete** (if required) and click **Delete** to confirm rule deletion.

---

### Delete AWS Lambda Function

1. From the **AWS Management Console**, search for and open the **Lambda** service.
2. From the functions list, find the function created (e.g., `vpn-diagnostic-handler`).
3. Click the function name to open details.
4. In the top-right corner, select **Actions** → **Delete function**.
5. A confirmation dialog will appear, enter **delete** (if required) and click **Delete** to remove the function.

---

### Delete IAM Role & Policy

1. From the **AWS Management Console**, search for and select the **IAM** service.
2. In the left menu, select **Roles**.
3. Find the **IAM Role** created for Lambda (e.g., `lambda-vpn-diagnostic-role`).
4. Click the role name to open details.
5. In the top-right corner, select **Delete role**.
6. In the confirmation dialog, click **Yes, Delete**.
7. If this role is associated with a **custom policy** (created in the workshop):
    - In the IAM left menu, select **Policies**.
    - Find the policy created (e.g., `lambda-vpn-diagnostic-policy`).
    - Check the policy → **Actions** → **Delete**.
    - Confirm policy deletion.

---

### Check Other Resources (if any)

- If you created additional **CloudWatch Alarms** during the workshop, you can delete them:
    1. Open **Amazon CloudWatch**.
    2. Select **Alarms** → find the alarm created → select **Actions** → **Delete**.
- Review all AWS services used in the workshop to ensure there are no active resources remaining (VPC, VPN, Gateway, EC2, S3, ... if applicable).

---

{{% notice note %}}
🔒 **Security Note:** When deleting a VPC, all related resources such as subnets, route tables, network ACLs, and security groups will also be deleted. However, resources such as NAT Gateways, VPC Endpoints, and VPN Connections must be deleted separately before deleting the VPC.
{{% /notice %}}

---

{{% notice warning %}}
**NOTE**: Double-check all services to ensure that there are **NO** running or undeleted services to avoid incurring long-term charges.

Check all AWS service regions to ensure that you have **NOT** missed any services in other regions.
{{% /notice %}}

![Clean Up Resources](/FCJ_Workshop_VuNgocQuang/images/7/0016.png?featherlight=false&width=90pc)

---