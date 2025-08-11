---
title : "Create VPC"
displayDate :  "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

#### Create an Amazon Virtual Private Cloud (VPC)

🔒 **Steps to perform**

1. Access **AWS Management Console**
- Search for the **VPC** service
- Select **VPC** from the search results

![Create VPC](/FCJ_Workshop_VuNgocQuang/images/2/2-1/0001.png?featherlight=false&width=90pc)

2. In the **VPC Dashboard**
- Select **Your VPCs** from the left-hand menu
- Click **Create VPC**

![Create VPC](/FCJ_Workshop_VuNgocQuang/images/2/2-1/0002.png?featherlight=false&width=90pc)

3. Configure **VPC** parameters
- Resources: Select **VPC only**
- Name tag: Enter `ASG`
- IPv4 CIDR block: Enter `10.10.0.0/16`

![Create VPC](/FCJ_Workshop_VuNgocQuang/images/2/2-1/0003.png?featherlight=false&width=90pc)

{{% notice info %}}
⚠️ **Note** about Tenancy: Keep the Tenancy option at the default setting **(Default)**. Changing it to **Dedicated** may limit the types of EC2 instances supported in the VPC.
{{% /notice %}}

4. Confirm VPC creation
- Click **Create VPC** to complete the process

![Create VPC](/FCJ_Workshop_VuNgocQuang/images/2/2-1/0004.png?featherlight=false&width=90pc)

5. Verify the **VPC** status after creation

![Create VPC](/FCJ_Workshop_VuNgocQuang/images/2/2-1/0005.png?featherlight=false&width=90pc)

💡 **DNS Configuration**
6. Enable **DNS** features for the **VPC**
- Select **Edit VPC settings**
- Open the **DNS settings** tab
- Enable **DNS hostnames** and **DNS resolution**
- Save changes

![Create VPC](/FCJ_Workshop_VuNgocQuang/images/2/2-1/0006.png?featherlight=false&width=90pc)
