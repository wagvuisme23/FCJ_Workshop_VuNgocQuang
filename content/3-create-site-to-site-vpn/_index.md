---
title : "Create Site-to-Site-VPN"
displayDate :  "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3. </b> "
---

# Configure Site-to-Site VPN

#### AWS Site-to-Site VPN

ℹ️ **Overview**

- **AWS Site-to-Site VPN** enables you to create a secure connection between an **on-premises data center** or **branch office network** and an **Amazon VPC**.
- Supports both **hardware VPN appliances** and **software-based VPN solutions** depending on requirements.
- Uses **IPSec tunnels** to encrypt data in transit over the Internet.
- Requires configuration of a **Virtual Private Gateway (VPG)** on the AWS side and a **Customer Gateway (CGW)** on the customer side.

---

#### Key Components

🔒 **Virtual Private Gateway (VPG)**

- The **VPN endpoint** deployed within AWS.
- Connects directly to a **VPC** via **VPC attachments**.
- Manages and orchestrates VPN connections.
- Supports both **dynamic routing** (BGP) and **static routing**.
- Each VPC can attach to only one VPG at a time.

🔒 **Customer Gateway (CGW)**

- Represents the VPN device on the customer (**on-premises**) side.
- Can be a hardware device (**Cisco, Juniper, Fortinet, etc.**) or software-based (**StrongSwan, OpenVPN**).
- Requires **a single static public IP address** per AWS Region.
- Must specify an **ASN (Autonomous System Number)** if using BGP.

🔒 **Redundancy & BGP**

- Each AWS VPN connection creates **two independent IPSec tunnels** to ensure redundancy.
- Uses **BGP** to dynamically exchange routes between AWS and the on-premises network.
- **Automatic Failover:** When one tunnel fails, BGP automatically updates routes and shifts traffic to the other tunnel without service interruption.
- Routing can be optimized by configuring **BGP metrics** and **AS_PATH prepending** to prioritize one tunnel as primary and the other as backup.

🔒 **CloudWatch Alarm & Monitoring**

- Use **Amazon CloudWatch Metrics** to monitor VPN tunnel status (`TunnelState`, `TunnelDataIn`, `TunnelDataOut`).
- Create **CloudWatch Alarms** to alert when a tunnel goes down.
- Integrate with **SNS (Simple Notification Service)** to send notifications via email/SMS.

🔒 **Automation with Lambda**

- Use **AWS Lambda** to automate failover processes or incident handling.
- Integrate with **CloudWatch Events** to trigger Lambda when a tunnel connection is lost.
- Automatically adjust routes in **VPC Route Tables** or update **on-premises firewall rules** when changes occur.

---

#### Deployment Process in AWS Management Console

1. **Create a Virtual Private Gateway (VPG)** and attach it to the VPC.
2. **Create a Customer Gateway (CGW)** with the public IP and ASN of the on-premises device.
3. **Create a Site-to-Site VPN Connection**, selecting either **Dynamic (BGP)** or **Static** routing.
4. **Download the VPN configuration file** compatible with your device (Cisco, Juniper, Fortinet, etc.).
5. **Configure the on-premises VPN device** using the downloaded file.
6. **Check tunnel status** in the AWS Console.
7. **Configure CloudWatch Alarm and SNS** for monitoring and alerting.
8. **Test failover** by temporarily disabling one tunnel and verifying traffic continuity.
9. **Verify BGP routes** have been exchanged successfully between both sides.

---
