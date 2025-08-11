---
title : "Performance monitoring & Dashboard"
displayDate :  "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 6.3 </b> "
---

## Performance monitoring & Dashboard (CloudWatch)

This section provides detailed instructions on configuring performance and Site-to-Site VPN status monitoring with Amazon CloudWatch, creating a visual dashboard, and setting retention policies for related logs.

### Metrics to monitor (namespace `AWS/VPN`)

Core metrics to track:

- **TunnelState** (1 = up, 0 = down)

    - Used to detect when the tunnel is down.
- **TunnelDataIn** / **TunnelDataOut** (bytes)

    - Inbound/outbound traffic through each tunnel — used to detect spikes or traffic shifts during failover.
- **TunnelPacketDrop** / **TunnelPacketLoss** (if provided by the provider)

    - If available, used to detect packet loss/quality issues.
- **TunnelLatency** (if provided by the provider)

  * Used to monitor latency between tunnel endpoints.

> Note: Metrics may appear per-tunnel (separated by tunnel index) — when selecting metrics, filter by `VpnConnectionId` and `TunnelIndex` to ensure you are monitoring the correct tunnel.

---

### Creating a detailed Dashboard (CloudWatch)

Step-by-step guide to create a `vpn-dashboard` and add the necessary widgets.

#### Create the dashboard

1. Open **AWS Console** → **CloudWatch** → **Dashboards** → **Create dashboard**.
2. Name it: `vpn-dashboard` → **Create dashboard**.
3. Choose a layout (e.g., `Grid` or `Time series`) — `Grid` is more flexible for adding multiple widgets.

#### Add a Line graph widget for traffic

1. In the dashboard, click **Add widget** → select **Line** (Line graph).
2. In the **Add metric** modal, select **Browse** → **AWS/VPN** → **By Tunnel** (or By VPN Connection depending on the interface).
3. Select **TunnelDataIn** and **TunnelDataOut** metrics for both tunnels (from the same VPN connection) — you can add multiple series in the same widget.
4. Adjust: Statistic = `Sum` or `Average` (usually `Sum` for bytes), Period = `1 minute` or `5 minutes` as needed.
5. Optionally, enable **Y axis (left/right)** if you want to display two metrics with different scales.
6. Click **Create widget**.

#### Add Single value widgets for TunnelState

1. Click **Add widget** → select **Single value**.
2. Select metric **AWS/VPN → TunnelState** for **Tunnel 1** (filter by `VpnConnectionId` and `TunnelIndex=1`).
3. Statistic: `Minimum` so that if the value is 0 at any point, it will show 0.
4. Title: `Tunnel 1 State` → Create widget.
5. Repeat for **Tunnel 2** (`TunnelIndex=2`) to have separate status widgets.

#### Add state alarms & annotations

- You can add **annotations** (text) or an **Alarm status** widget if you want to display alarm status on the dashboard.
- Add a **Text** widget to include a short runbook (e.g., quick recovery steps when a tunnel is down).

#### Metric Math (optional)

- To view the total traffic across both tunnels, when adding a widget, select **Add math expression** and use an expression such as: `m1 + m2` (where m1 = TunnelDataOut t1, m2 = TunnelDataOut t2).
- Name the expression (e.g., `TotalDataOut`) for easier tracking.

#### Customize time range & refresh

- In the top corner of the dashboard, select a time range (Last 1 hour / 3 hours / 24 hours) and a refresh interval (Auto / 1 minute / 5 minutes).

#### Save & share

- Click **Save dashboard**.
- You can export the link (View in Console) or enable **share** via IAM so the operations team can access it.

---

### CloudWatch Logs retention (set retention for Lambda/SSM logs)

Ensure logs are not stored indefinitely to avoid increasing costs.

#### Configure retention for each Log Group

1. Open **CloudWatch** → **Logs** → **Log groups**.
2. Locate the Lambda log group: `/aws/lambda/vpn-diagnostic-handler` → select the log group.
3. Click **Actions** → **Edit retention** → choose `90 days` (or your policy) → **Save**.
4. Repeat for the Systems Manager (SSM) command output log group if applicable (e.g., `/aws/ssm/commands` or your custom name).

#### Logging volume considerations

- Avoid excessive (verbose) logging in Lambda when running in production. Use `LOG_LEVEL` to adjust.
- Use CloudWatch Logs Insights to run queries instead of exporting all logs to S3.

---
