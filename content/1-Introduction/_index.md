---
title : "Introduction to Site-to-Site VPN with BGP and Redundancy"
displayDate :  "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 1. </b> "
---

**Contents:**
- [Workshop Objectives](#-workshop-objectives)
- [Theoretical Overview](#-theoretical-overview)
- [Proposed Architecture](#-proposed-architecture)
- [Real-World Scenario](#-real-world-scenario)
- [What You Will Learn](#-what-you-will-learn)
- [Tools and AWS Services Used](#-tools-and-aws-services-used)
- [Prerequisites](#️-prerequisites)

---

#### 🎯 Workshop Objectives

This workshop walks you through setting up a **Site-to-Site VPN** connection between an **on-premises infrastructure** and an **Amazon VPC**, using **BGP (Border Gateway Protocol)** for dynamic routing and ensuring **high availability** through **redundant tunnels**.

You will also learn to:

- Monitor VPN health and metrics.
- Automate alerting and failure detection.
- Perform failover testing.
- Secure tunnels using IAM and best practices.
- Estimate cost and optimize performance.

---

#### 🧠 Theoretical Overview

1. 🌐 **What is Site-to-Site VPN?**

Site-to-Site VPN enables secure communication between two private networks over the internet using **IPSec encryption**. It allows resources on both ends to communicate as if they were on the same local network.

{{% notice note %}}
Benefits: Always-on connection, end-to-end encryption, no manual connection required.
{{% /notice %}}

2. 🧭 **Why Use BGP?**

**BGP (Border Gateway Protocol)** is a Layer 3 dynamic routing protocol that exchanges routes between Autonomous Systems (AS). In VPN:

- 🧠 Learns and advertises routes automatically.
- 🔁 Supports automatic failover when a tunnel fails.
- 📈 Optimizes routing based on metrics and reachability.
- 🔄 No manual static route updates – scales better.

3. 🏗️ **Redundancy with Multi-Tunnel VPN**

AWS provides **two IPSec tunnels** per VPN connection. With BGP:

- If one tunnel fails, traffic is rerouted through the second.
- Both tunnels can be active simultaneously.
- Ideal for **mission-critical environments** needing high availability.

---

#### 🧩 Proposed Architecture

- An **AWS VPC** (10.0.0.0/16) with public/private subnets.
- A simulated on-prem router using **VyOS or pfSense**.
- **Customer Gateway**: represents the on-prem router.
- **Virtual Private Gateway** attached to the VPC.
- **Site-to-Site VPN** with two IPSec tunnels and BGP.
- **CloudWatch & CloudTrail** for monitoring and audit.
- **IAM Role** for access control.
- **VPC Flow Logs** for traffic visibility.

![Site-to-Site VPN Diagram](/images/1/0001.png?featherlight=false&width=90pc)

---

#### 💡 Real-World Scenario

XYZ Corp is migrating part of its workload to AWS, while retaining legacy systems and data centers on-prem.

Requirements:

- Secure and stable connection between AWS and on-prem.
- No downtime if a tunnel fails.
- Dynamic route updates when on-prem IP changes.
- Monitoring and alerting on VPN health.

Solution: Site-to-Site VPN + BGP + Monitoring + Redundancy.

---

#### 🎓 What You Will Learn

| Skill Acquired                    | Description                               |
| ----------------------------------|-------------------------------------------|
| ✅ Create a VPN with redundancy    | Deploy Site-to-Site VPN with two tunnels  |
| ✅ Configure bidirectional BGP     | Automate routing between AWS and on-prem  |
| ✅ Test automatic failover         | Simulate tunnel failure, validate reroute |
| ✅ Monitor VPN and routing state   | Use CloudWatch, CloudTrail                |
| ✅ Inspect traffic flow            | Enable VPC Flow Logs                      |
| ✅ Secure VPN with IAM             | Least privilege access and strong PSKs   |
| ✅ Estimate and optimize costs     | Understand billing patterns               |
| ✅ Document and operationalize     | Build SOPs for networking team            |

---

#### 📦 Tools and AWS Services Used

- Amazon VPC  
- Virtual Private Gateway (VGW)  
- Customer Gateway (CGW)  
- EC2 Instance (VyOS/pfSense simulator)  
- Site-to-Site VPN  
- BGP Routing  
- CloudWatch & CloudTrail  
- IAM Roles & Policies  
- VPC Flow Logs  

---

#### 🛠️ Prerequisites

- Basic knowledge of **AWS networking** (VPC, Subnets, EC2).
- Familiarity with **AWS Console or CLI**.
- Understanding of **routing, CIDR, and IPSec concepts**.
- Prior exposure to **BGP or static routing** is helpful.
- Experience with **Linux or router OS (VyOS/pfSense)** is a plus.

---
