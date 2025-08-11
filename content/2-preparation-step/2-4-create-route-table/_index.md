---
title : "Create Route Table"
displayDate :  "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 2.4 </b> "
---

#### Create a Route Table in Amazon VPC

🔒 **Steps to perform**

1. Access the **VPC** console
    - Select **Route Tables** from the left menu
    - Click **Create route table**

![Create Route Table](/FCJ_Workshop_VuNgocQuang/images/2/2-4/0001.png?featherlight=false&width=90pc)

2. Configure the **Route Table**
    - Name: Enter `Route table-Public`
    - VPC: Select **VPC ASG** (VPC ID will be auto-filled)
    - Click **Create route table**

![Create Route Table](/FCJ_Workshop_VuNgocQuang/images/2/2-4/0002.png?featherlight=false&width=90pc)

3. Confirm successful creation of the Route Table

![Create Route Table](/FCJ_Workshop_VuNgocQuang/images/2/2-4/0003.png?featherlight=false&width=90pc)

💡 **Configure routing**

4. Add a route for the Internet Gateway
    - Click **Actions**
    - Select **Edit routes**

![Create Route Table](/FCJ_Workshop_VuNgocQuang/images/2/2-4/0004.png?featherlight=false&width=90pc)

5. Configure a new route
    - Click **Add route**
    - Destination: Enter `0.0.0.0/0` (represents the internet)
    - Target: Select **Internet Gateway** and choose the created **IGW**
    - Click **Save changes**

![Create Route Table](/FCJ_Workshop_VuNgocQuang/images/2/2-4/0005.png?featherlight=false&width=90pc)

⚠️ Associate with Subnet

6. Configure **subnet associations**
    - Select the **Subnet associations** tab
    - Click **Edit subnet associations**

![Create Route Table](/FCJ_Workshop_VuNgocQuang/images/2/2-4/0006.png?featherlight=false&width=90pc)

7. Select the **public subnets**
    - Expand the **Subnet ID** column to view details
    - Select both created **public subnets**
    - Click **Save associations**

![Create Route Table](/FCJ_Workshop_VuNgocQuang/images/2/2-4/0007.png?featherlight=false&width=90pc)

8. Confirm successful **subnet associations** configuration

![Create Route Table](/FCJ_Workshop_VuNgocQuang/images/2/2-4/0008.png?featherlight=false&width=90pc)

---
