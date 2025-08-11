---
title : "Create VPC for VPN"
displayDate :  "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---

#### Set up VPC for Site-to-Site VPN

⚠️ Prerequisites

- Access to AWS Console with sufficient permissions to create VPC resources  
- Basic understanding of CIDR and subnet planning  

---

#### Create VPC and Subnet

1. Access **VPC Dashboard**  
    - Select **Your VPCs**  
    - Click **Create VPC**  

![Create VPC for VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0001.png?featherlight=false&width=90pc)

2. Configure the new **VPC**  
    - **Resource type:** Select **VPC only**  
    - **Name:** Enter `ASG VPN`  
    - IPv4 CIDR: Enter `10.11.0.0/16`  
    - Click **Create VPC**  

![Create VPC for VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0002.png?featherlight=false&width=90pc)

![Create VPC for VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0003.png?featherlight=false&width=90pc)

![Create VPC for VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0004.png?featherlight=false&width=90pc)

3. Create **Public Subnet**  
    - Go to **Subnets**  
    - Click **Create subnet**  
    - Select **VPC ASG VPN**  

![Create VPC for VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0005.png?featherlight=false&width=90pc)

![Create VPC for VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0006.png?featherlight=false&width=90pc)

4. Configure **Subnet**  
    - Name: Enter `VPN Public`  
    - **Availability Zone:** Select **ap-southeast-1a**  
    - IPv4 CIDR: Enter `10.11.1.0/24`  
    - Click **Create subnet**  

![Create VPC for VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0007.png?featherlight=false&width=90pc)

![Create VPC for VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0008.png?featherlight=false&width=90pc)

---

#### Configure Internet Connectivity

1. **Enable Auto-assign Public IP**  
    - Select subnet **VPN Public**  
    - Click **Actions** > **Edit subnet settings**  
    - Select **Enable auto-assign public IPv4 address**  
    - Click **Save**  

![Create VPC for VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0009.png?featherlight=false&width=90pc)

![Create VPC for VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0010.png?featherlight=false&width=90pc)

![Create VPC for VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0011.png?featherlight=false&width=90pc)

2. Create **Internet Gateway**  
    - Go to **Internet Gateways**  
    - Click **Create internet gateway**  
    - **Name:** Enter `Internet Gateway VPN`  
    - Click **Create**  

![Create VPC for VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0012.png?featherlight=false&width=90pc)

![Create VPC for VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0013.png?featherlight=false&width=90pc)

2. **Attach Internet Gateway to VPC**  
    - Select the newly created **IGW**  
    - Click **Actions > Attach to VPC**  
    - Select **VPC ASG VPN**  
    - Click **Attach**  

![Create VPC for VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0014.png?featherlight=false&width=90pc)

![Create VPC for VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0015.png?featherlight=false&width=90pc)

![Create VPC for VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0016.png?featherlight=false&width=90pc)

---

#### Configure Route Table

1. Create a new **Route Table**  
    - Go to **Route Tables**  
    - Click **Create route table**  
    - **Name:** Enter `Route table VPN - Public`  
    - **VPC:** Select **ASG VPN**  
    - Click **Create**  

![Create VPC for VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0017.png?featherlight=false&width=90pc)

![Create VPC for VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0018.png?featherlight=false&width=90pc)

2. Add **Route** for **Internet Access**  
    - Select the **Routes** tab  
    - Click **Edit routes**  
    - Click **Add route**  
    - Destination: Enter `0.0.0.0/0`  
    - Target: Select **Internet Gateway VPN**  
    - Click **Save changes**  

![Create VPC for VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0019.png?featherlight=false&width=90pc)

![Create VPC for VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0020.png?featherlight=false&width=90pc)

3. Associate **Subnet**  
    - Select the **Subnet associations** tab  
    - Click **Edit subnet associations**  
    - Select **subnet VPN Public**  
    - Click **Save associations**  

![Create VPC for VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0021.png?featherlight=false&width=90pc)

![Create VPC for VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0022.png?featherlight=false&width=90pc)

![Create VPC for VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0023.png?featherlight=false&width=90pc)

{{% notice tip %}}
💡 **Pro Tip**

- Make sure to review all route and security group configurations  
- Use tags for efficient resource management  
- Keep a record of CIDR ranges for future reference  
{{% /notice %}}

---