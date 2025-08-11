---
title : "Create VPN Connection"
displayDate :  "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3.2.3 </b> "
---

# Create VPN Connection

#### Configure AWS Site-to-Site VPN Connection

---

#### Create VPN Connection

1. Access **AWS VPC Console**
    - Navigate to **Site-to-Site VPN Connections**
    - Click **Create VPN Connection**

![Create VPN Connection](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-3/0001.png?featherlight=false&width=90pc)

2. Configure basic **VPN Connection** settings
    - Name tag: Enter `VPN Connection`
    - **Target Gateway Type**: Select **Virtual Private Gateway**
    - **Virtual Private Gateway**: Select the created **VPN Gateway**
    - **Customer Gateway**: Select **Existing**
    - **Customer Gateway ID**: Select the created **Customer Gateway**

![Create VPN Connection](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-3/0002.png?featherlight=false&width=90pc)

3. Configure **Routing**
    - **Routing Options**: Select **Dynamic**

![Create VPN Connection](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-3/0002-1.png?featherlight=false&width=90pc)

{{% notice note %}}
You must select **Dynamic** for **BGP** configuration; **Static** is more suitable for environments with minimal changes.
{{% /notice %}}

{{% notice info %}}
Note:
- Record, screenshot, or copy the information and store it somewhere safe.
- Go to **VPN** -> **VPN Details**
{{% /notice %}}

![Create VPN Connection](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-3/0004-1.png?featherlight=false&width=90pc)

4. Initialize **VPN Connection**
    - Review configuration
    - Click **Create VPN Connection**

![Create VPN Connection](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-3/0004.png?featherlight=false&width=90pc)

⚠️ Note

The VPN creation process may take 5–10 minutes.  
Wait until the status changes to **Available** before proceeding.

![Create VPN Connection](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-3/0005.png?featherlight=false&width=90pc)

---

#### Configure Route Propagation

1. Configure for **Public Route Table**
    - Access **Route Tables** in **VPC Console**
    - Select the **route table** for the **Public subnet**
    - Go to the **Route Propagation** tab
    - Click **Edit route propagation**

![Create VPN Connection](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-3/0006.png?featherlight=false&width=90pc)

2. **Enable Route Propagation**
    - Select **Enable** for the **Virtual Private Gateway**
    - Click **Save**

![Create VPN Connection](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-3/0007.png?featherlight=false&width=90pc)

3. Verify status
    - Check that **Route Propagation** has changed to **Yes**

![Create VPN Connection](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-3/0008.png?featherlight=false&width=90pc)

4. Repeat the process for **Private Route Table**
    - Follow the same steps as above
    - Ensure **route propagation** is enabled for both **public** and **private subnets**

![Create VPN Connection](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-3/0009.png?featherlight=false&width=90pc)

![Create VPN Connection](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-3/0010.png?featherlight=false&width=90pc)

![Create VPN Connection](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-3/0011.png?featherlight=false&width=90pc)

🔒 **Security Note**

- Route propagation automatically updates route tables when changes occur.
- Ensure it is only enabled for the necessary route tables to avoid security risks.

---
