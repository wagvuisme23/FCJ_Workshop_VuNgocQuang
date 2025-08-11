---
title : "Redundancy Testing & Failover"
displayDate :  "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 4. </b> "
---

# Redundancy Testing & Failover

### 🎯 Objective
Validate the redundancy and automatic failover capabilities of a Site-to-Site VPN architecture using BGP.  
Ensure that when one tunnel experiences a failure, traffic is automatically rerouted through the remaining active tunnel without service disruption.

---

### 📚 Theoretical Overview
In a Site-to-Site VPN configuration with BGP, AWS provides **two IPsec tunnels** to enhance availability.  
BGP automatically detects when a tunnel goes down and withdraws the corresponding BGP route from the routing table.  
Traffic is then rerouted through the remaining active tunnel.

Redundancy testing helps:
- Confirm BGP failover functions as expected.
- Ensure dynamic routes are updated promptly.
- Measure the actual downtime during a failure event.

---

### 🛠 Steps in AWS Management Console

#### **Step 1 — Verify Initial State**
1. Open **AWS Management Console** → navigate to **VPC**.
2. Select **Site-to-Site VPN Connections**.
3. Choose your VPN connection.
4. Switch to the **Tunnel Details** tab.
5. Verify both **Tunnel 1** and **Tunnel 2** show:
   - **Status**: UP
   - **BGP Status**: UP

---

#### **Step 2 — Check Routing Table**
1. In VPC, go to **Route Tables**.
2. Select the route table associated with the VPC subnet under test.
3. View the **Routes** tab.
4. Confirm that routes to the on-premises network have **Target** as:
   - `vgw-xxxxxxxx` or `tgw-xxxxxxxx` (depending on the architecture)
5. Record ASN, prefix, and next hop from the BGP routes.

---

#### **Step 3 — Simulate Failure on Tunnel 1**
> This step is performed **on the on-premises side** or by temporarily altering configuration to bring down Tunnel 1.

Common methods:
- **On on-premises device**: shut down the interface or IPsec policy for Tunnel 1.
- **If on-prem access is not available**: use AWS CLI/Console to **disable** Tunnel 1 by temporarily setting an incorrect Pre-shared Key (for testing only, revert afterwards).

---

#### **Step 4 — Observe Failover**
1. In the **Tunnel Details** tab, watch Tunnel 1 status change from **UP** → **DOWN**.
2. Ensure **Tunnel 2** remains **UP**.
3. In **Route Tables**, refresh and confirm:
   - The BGP route associated with Tunnel 1 is withdrawn.
   - Traffic is now flowing through Tunnel 2.
4. Perform ping/traceroute from an EC2 instance in the VPC to an on-prem server to confirm no service interruption.

---

#### **Step 5 — Restore Tunnel 1**
1. Restore the correct configuration for Tunnel 1 on the on-premises device.
2. Observe Tunnel 1 return to **UP** state with BGP **Established**.
3. Verify that BGP has re-advertised the routes and both tunnels are active.

---

#### **Step 6 — Repeat for Tunnel 2**
- Repeat the procedure for Tunnel 2 to confirm bidirectional failover works as expected.

---

### 📈 Measuring Failover Time
- Use CloudWatch Metrics for the VPN connection:
  1. Go to **CloudWatch** → **Metrics** → **VPN**.
  2. Monitor metrics `TunnelState`, `BGPStatus`, `TunnelDataIn/Out`.
  3. Record the time from tunnel down event to when traffic resumes via the alternate tunnel.

---

### 💰 Cost Considerations
- Keeping both tunnels in an UP state does not incur additional fixed VPN charges, but data transfer through either tunnel is billed.
- Repeated testing may generate additional CloudWatch Logs/Alarms charges.

---

### 🔒 Security Notes
- After testing (e.g., changing Pre-shared Key or disabling a tunnel), restore all original parameters.
- Perform tests during maintenance windows or with explicit approval from the operations team.

---

✅ **Expected Outcome**:
- When one tunnel goes down, traffic automatically switches to the remaining tunnel.
- Actual downtime should be limited to BGP detection time plus route propagation delay (typically 20–30 seconds).
