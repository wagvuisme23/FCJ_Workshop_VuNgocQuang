---
title : "Create an Alarm to Monitor TunnelState in AWS CloudWatch"
displayDate :  "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 5. </b> "
---

#### Create an Alarm to Monitor TunnelState in AWS CloudWatch

The objective of this step is to configure monitoring for the status of each VPN tunnel using Amazon CloudWatch. When a tunnel encounters an issue (TunnelState = 0), the system will automatically send an alert so that the technical team can immediately perform failover or troubleshooting.

#### Steps in AWS Management Console

1. **Access CloudWatch**
   - Open the **CloudWatch** service in the AWS Management Console.
   - In the left-hand menu, select **Metrics** → **All metrics**.

![Alarm](/FCJ_Workshop_VuNgocQuang/images/5/0001.png?featherlight=false&width=90pc)

2. **Select the TunnelState metric**
   - Select namespace: **AWS/VPN**.
   - Choose filter mode: **By Tunnel**.
   - Find the **TunnelState** metric for the tunnel you want to monitor (e.g., Tunnel 1).
   - Check the box to select this metric.

3. **Create an alarm**
   - Click **Actions** (top right) → **Create alarm**.

![Alarm](/FCJ_Workshop_VuNgocQuang/images/5/0002.png?featherlight=false&width=90pc)

4. **Configure alarm conditions**
   - **Metric name**: `TunnelState`
   - **Statistic**: `Minimum` (ensures that if TunnelState = 0 at any point in the period, the alarm will trigger).
   - **Period**: `1 minute`
   - **Threshold type**: Static
   - **Whenever TunnelState is**: Select `Lower/Equal` → enter value `0`

![Alarm](/FCJ_Workshop_VuNgocQuang/images/5/0003.png?featherlight=false&width=90pc)

5. **Configure actions**
   - **Alarm state trigger**: `In alarm`
   - **Send notification to**: Select an existing SNS topic or create a new one.
   - If creating a new topic:
     - Choose **Create new topic**
     - Enter a topic name (e.g., `vpn-alerts`)
     - Enter the email address to receive notifications
     - AWS will send a confirmation email → open the email and click the confirmation link to activate.

![Alarm](/FCJ_Workshop_VuNgocQuang/images/5/0004.png?featherlight=false&width=90pc)

![Alarm](/FCJ_Workshop_VuNgocQuang/images/5/0005.png?featherlight=false&width=90pc)

6. **Name and create the alarm**
   - **Alarm name**: e.g., `VPN-Tunnel1-Down`
   - **Description**: “Alert when VPN Tunnel 1 is down (TunnelState=0)”
   - Click **Create alarm** to complete.

![Alarm](/FCJ_Workshop_VuNgocQuang/images/5/0006.png?featherlight=false&width=90pc)

![Alarm](/FCJ_Workshop_VuNgocQuang/images/5/0007.png?featherlight=false&width=90pc)

7. **Repeat for Tunnel 2**
   - Go back to **All metrics → AWS/VPN → By Tunnel** and find the **TunnelState** metric for Tunnel 2.
   - Create a similar alarm to ensure both tunnels are monitored.

#### Expected result
- When a tunnel is down, the `TunnelState` value will switch to `0`.
- The CloudWatch Alarm will trigger and send an email alert to the technical team.
- The technical team can monitor the failover process to the standby tunnel and take necessary remediation actions.

---