---
title : "Create Customer Gateway"
displayDate :  "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 3.2.2 </b> "
---

# Create Customer Gateway

#### Create Customer Gateway

1. Access **VPC**

    - Select **Customer Gateways**
    - Click **Create Customer Gateway**

![Create CGW](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-2/0001.png?featherlight=false&width=90pc)

2. In the **Create Customer Gateway** interface

    - In the **Name tag** field, enter `Customer Gateway`
    - In the **IP address** field, enter the **Public IP** address of the **EC2 Customer Gateway** instance
    - Click **Create Customer Gateway**

![Create CGW](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-2/0002.png?featherlight=false&width=90pc)

![Create CGW](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-2/0003.png?featherlight=false&width=90pc)

3. The creation process for the **Customer Gateway** will complete in approximately 5 minutes

![Create CGW](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-2/0004.png?featherlight=false&width=90pc)

{{% notice info %}}
ℹ️ Important Information: According to the architecture design, the Customer Gateway will be deployed in the on-premises VPC environment. In this step, we are registering with AWS the usage of a Customer Gateway with the public IP address of an EC2 instance (Customer Gateway) located in the Auto Scaling Group of the VPN VPC.
{{% /notice %}}

---
