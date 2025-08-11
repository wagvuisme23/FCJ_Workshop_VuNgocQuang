---
title : "Create VPG"
displayDate :  "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.2.1 </b> "
---

# Create Virtual Private Gateway

#### Create Virtual Private Gateway (VGW)

ℹ️ **Overview**

- The Virtual Private Gateway (VGW) is a critical component for a Site-to-Site VPN connection  
- Acts as the AWS-side VPN endpoint  
- Must be attached to the target VPC to establish the connection  

---

#### Steps to Implement

1. Access the **AWS VPC Console**
    - Navigate to **Virtual Private Gateways**
    - Click **Create Virtual Private Gateway**

![Create VPG](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-1/0001.png?featherlight=false&width=90pc)

2. Configure the **Virtual Private Gateway**
    - **Name tag:** Enter `VPN Gateway`
    - **ASN:** Select **Amazon default ASN**
    - Click **Create virtual private gateway**

![Create VPG](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-1/0002.png?featherlight=false&width=90pc)

{{% notice tip %}}
💡 **Pro Tip**

- Using **Amazon default ASN** is suitable for most cases  
- Custom ASN is only necessary if you have specific BGP routing requirements  
{{% /notice %}}

3. Attach **VGW** to the **VPC**
    - Select **Actions**
    - Click **Attach to VPC**

![Create VPG](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-1/0003.png?featherlight=false&width=90pc)

4. Select the target **VPC**
    - From the dropdown, select **VPC ASG**
    - Click **Attach to VPC**

![Create VPG](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-1/0004.png?featherlight=false&width=90pc)

{{% notice info %}}
⚠️ **Important Note**

Ensure the VGW status changes to **Attached** before proceeding  
The attach process may take a few minutes to complete  
{{% /notice %}}

5. Verify the status
    - Check that **State** shows **Attached**
    - VGW is now ready for the next VPN configuration steps

![Create VPG](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-1/0005.png?featherlight=false&width=90pc)

---