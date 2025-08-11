---
title : "Site-to-Site VPN with Lambda Monitoring CloudWatch Activity"
displayDate :  "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 6. </b> "
---

## Overview of Steps

This section provides a quick understanding of the workflow and the main steps in the workshop — read this part before executing the detailed steps below.

Objective: Deploy a Site-to-Site VPN with BGP and redundancy, set up monitoring, automate failover using Lambda/EventBridge, optimize costs, and provide a troubleshooting runbook.

Summary of the main steps (recommended order):

1. **Preparation & input information** — Identify VPC, on-prem CIDR, router public IP, ASN, and a test EC2 instance (SSM enabled).
2. **Create and configure VPN (in VPC Console)** — Create VGW, Customer Gateway, and VPN Connection (dynamic BGP, 2 tunnels). (See the detailed sections earlier in the documentation.)
3. **Verify BGP & redundancy** — Ensure both tunnels are UP and BGP is Established; simulate failover to check route propagation.
4. **Set up monitoring** — Create CloudWatch metrics & Alarms for `TunnelState`, `TunnelDataIn/Out`; configure the `vpn-dashboard` for visualization.
5. **Automate incident handling** — Create an IAM Role for Lambda (section L), deploy the diagnostic Lambda (section M), and configure the EventBridge rule (section E/N) to invoke Lambda when an Alarm is in ALARM state.
6. **End-to-end testing** — Simulate a tunnel down, observe CloudWatch Alarm → EventBridge → Lambda → SSM invocation and review ping/traceroute results.
7. **Cost optimization & logging** — Set retention for CloudWatch Logs, monitor Data Transfer via Cost Explorer, and use Budgets/Alerts.
8. **Prepare runbook & troubleshooting** — Use the quick guide table (section 15) for rapid incident resolution.
9. **Clean up resources** — After completing the lab, delete resources (Lambda, EventBridge rule, VPN, VGW, CGW, VPC if applicable) to avoid incurring additional costs.

Safety & operational notes:

- Always perform testing during a maintenance window and notify the operations team.
- Do not change both tunnels simultaneously when testing failover.
- Keep secrets (PSK, webhooks) in AWS Secrets Manager — do not store them as plaintext environment variables.

---