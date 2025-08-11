---
title : "Cost optimization"
displayDate :  "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 6.4 </b> "
---

## Cost optimization

This section provides guidance on optimizing costs related to VPN, data transfer, CloudWatch, and EC2 test machines.

### Understanding VPN costs

- **VPN hourly**: AWS charges by **connection-hour** for each Site-to-Site VPN connection (each connection has 2 tunnels).

   - Example assumption: 0.05 USD/connection-hour.  
   - If maintaining 1 connection for 24 hours: 24 × 0.05 = **1.2 USD/day**.  
   - If testing 5 days/week, 4 weeks: 5 × 4 × 1.2 = **24 USD/month**.

- **Optimization**: In lab or workshop environments, **disable the VPN when not needed** to save ~1 USD/day.

- **Data transfer**: Outbound data (to the Internet) may be charged per GB (e.g., 0.09 USD/GB).

   - Example for transferring 50 GB: 50 × 0.09 = **4.5 USD**.

> No specific prices are listed here — check Billing / Cost Explorer for rates per region and account.

### Monitoring Data Transfer with Cost Explorer

1. Go to **Billing & Cost Management** → **Cost Explorer** → **Launch Cost Explorer**.  
2. Create a custom report:  
   - **Group by**: `Service` → filter `Amazon VPC` / `AWS VPN` / `Data Transfer`.  
   - **Time range**: Last 30 days.  
   - **Add filter**: `Usage Type` contains `DataTransfer-Out`.  
3. **Save** the report for weekly tracking.

### Optimizing CloudWatch and Logs

- **Retention**: Set reasonable retention (e.g., 30–90 days) for important log groups.  
- **Metric Filters**: Create only necessary filters — each filter may incur ingestion costs.  
- **Log verbosity**: In Lambda, use `INFO` or `WARN` for production; enable `DEBUG` only for troubleshooting.

### EC2 and lab costs

- Use small instances such as **t2.micro** (~0.0104 USD/hour) or **t3.small** (~0.0208 USD/hour) for router simulation VMs.  
   - Example: t2.micro running 24 h = 24 × 0.0104 ≈ **0.25 USD/day**.  
- In production, consider network throughput — choose instances with ENA/high throughput.  
- **Stop** or **terminate** EC2 test instances when not in use.

### Budgets & Alerts

1. **AWS Budgets** → **Create budget**:  
   - Budget type: **Cost budget** or **Usage budget** (Data Transfer GB).  
   - Example: set threshold at **80%** and **100%** → send alerts via email/SNS.  
2. Create **Cost Anomaly Detection** to get alerts when unexpected charges occur.

### Tagging & Chargeback

- Tag resources (VPN, EC2, VPC) clearly by project or team — this helps allocate costs to the right group.  
- Use **Tag Policy** to ensure all new resources are tagged with required keys (e.g., `Project`, `Owner`, `Environment`).

---

### Sample cost table (assumptions)

| Resource                  | Usage               | Cost unit                    | Estimated cost         |
|---------------------------|---------------------|------------------------------|------------------------|
| VPN connection            | 24 h × 1 conn       | 0.05 USD/connection-hour     | **1.2 USD/day**        |
| EC2 (t2.micro)            | 24 h × 30 days      | ~0.0104 USD/hour              | **~7.5 USD/month**     |
| Data transfer             | 50 GB outbound      | 0.09 USD/GB                   | **4.5 USD**            |
| CloudWatch logs retention | reduce to 30 days   | lower log volume → lower cost | savings depend on volume |
| Metric filters            | limit filters       | ingestion cost                | savings ~0.03–0.1 USD/filter/day |
| Total (lab scenario)      | VPN + EC2 + Data    |                               | **~13.2 USD/month**    |

> **Note**: This is a hypothetical example to illustrate potential costs — replace with actual figures from your **Cost Explorer** and **Billing Dashboard**.

---
