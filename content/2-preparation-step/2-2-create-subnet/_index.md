---
title : "Create Subnet"
displayDate :  "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---


#### Create a Subnet in Amazon VPC

🔒 **Steps to perform**

1. Access the **VPC** interface
- Select **Subnets** from the left-hand menu
- Click **Create subnet**

![Create Subnet](/FCJ_Workshop_VuNgocQuang/images/2/2-2/0001.png?featherlight=false&width=90pc)

2. Select **VPC**
- In the **Create subnet** interface
- Choose the **ASG** VPC created earlier

![Create Subnet](/FCJ_Workshop_VuNgocQuang/images/2/2-2/0002.png?featherlight=false&width=90pc)

3. Configure the first Subnet
- **Subnet name**: Enter `Public Subnet 1`
- **Availability Zone**: Select **ap-southeast-1a**
- IPv4 CIDR block: Enter `10.10.1.0/24`
- Click **Create subnet**

![Create Subnet](/FCJ_Workshop_VuNgocQuang/images/2/2-2/0003.png?featherlight=false&width=90pc)

4. Confirm successful subnet creation

![Create Subnet](/FCJ_Workshop_VuNgocQuang/images/2/2-2/0004.png?featherlight=false&width=90pc)

💡 **Create additional Subnets**

5. Repeat the process to create the following subnets:
- **Public Subnet 2**
    - CIDR: **10.10.2.0/24**
    - AZ: **ap-southeast-1b**

![Create Subnet](/FCJ_Workshop_VuNgocQuang/images/2/2-2/0005.png?featherlight=false&width=90pc)

- **Private Subnet 1**
    - CIDR: **10.10.3.0/24**
    - AZ: **ap-southeast-1a**

![Create Subnet](/FCJ_Workshop_VuNgocQuang/images/2/2-2/0006.png?featherlight=false&width=90pc)

- **Private Subnet 2**
    - CIDR: **10.10.4.0/24**
    - AZ: **ap-southeast-1b**

![Create Subnet](/FCJ_Workshop_VuNgocQuang/images/2/2-2/0007.png?featherlight=false&width=90pc)

⚠️ **Note** about **AWS Availability Zones**:  
AWS uses two related concepts:  

- **Availability Zone (AZ)**: The display name (e.g., ap-southeast-1a)  
- **AZ ID**: The actual internal identifier for the Availability Zone. AWS maps AZ names to AZ IDs differently across accounts to ensure even resource distribution.

![Create Subnet](/FCJ_Workshop_VuNgocQuang/images/2/2-2/0008.png?featherlight=false&width=90pc)

---

#### Configure Auto-assign Public IP

ℹ️ **Purpose**: Allow automatic assignment of public IP addresses to instances launched in the public subnet.

6. Configure **Public Subnet 1**
- Select the **subnet** from the list
- Click **Actions**
- Choose **Edit subnet settings**

![Create Subnet](/FCJ_Workshop_VuNgocQuang/images/2/2-2/0009.png?featherlight=false&width=90pc)

7. Enable **Auto-assign IP**
- Turn on **Enable auto-assign public IPv4 address**
- Click **Save**

![Create Subnet](/FCJ_Workshop_VuNgocQuang/images/2/2-2/0010.png?featherlight=false&width=90pc)

![Create Subnet](/FCJ_Workshop_VuNgocQuang/images/2/2-2/0011.png?featherlight=false&width=90pc)

8. Repeat the configuration for **Public Subnet 2**

![Create Subnet](/FCJ_Workshop_VuNgocQuang/images/2/2-2/0012.png?featherlight=false&width=90pc)

![Create Subnet](/FCJ_Workshop_VuNgocQuang/images/2/2-2/0013.png?featherlight=false&width=90pc)

---
