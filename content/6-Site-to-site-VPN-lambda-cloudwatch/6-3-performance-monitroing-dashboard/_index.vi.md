---
title : "Performance monitoring & Dashboard"
displayDate :  "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 6.3 </b> "
---

## Performance monitoring & Dashboard (CloudWatch)

Phần này hướng dẫn chi tiết cách cấu hình giám sát hiệu năng và trạng thái Site‑to‑Site VPN bằng Amazon CloudWatch, tạo dashboard trực quan và cấu hình retention cho logs liên quan.

### Metrics cần giám sát (namespace `AWS/VPN`)

Các metric cốt lõi cần theo dõi:

- **TunnelState** (1 = up, 0 = down)

  - Dùng để phát hiện tunnel bị down.
- **TunnelDataIn** / **TunnelDataOut** (bytes)

  - Lưu lượng inbound/outbound qua mỗi tunnel — dùng để phát hiện spike hoặc traffic shift khi failover.
- **TunnelPacketDrop** / **TunnelPacketLoss** (nếu provider cung cấp)

  - Nếu có, dùng để phát hiện mất gói/quality issues.
- **TunnelLatency** (nếu provider cung cấp)

  - Dùng để giám sát độ trễ giữa hai đầu tunnel.

> Ghi chú: metric có thể xuất hiện theo dạng per‑tunnel (tách theo tunnel index) — khi chọn metric, hãy lọc theo `VpnConnectionId` và `TunnelIndex` để đảm bảo đúng tunnel.

---

### Tạo Dashboard chi tiết (CloudWatch)

Hướng dẫn từng bước tạo dashboard `vpn-dashboard` và thêm widgets cần thiết.

#### Tạo dashboard

1. Mở **AWS Console** → **CloudWatch** → **Dashboards** → **Create dashboard**.
2. Đặt tên: `vpn-dashboard` → **Create dashboard**.
3. Chọn layout (ví dụ: `Grid` hoặc `Time series`) — `Grid` linh hoạt hơn để thêm nhiều widget.

#### Thêm widget Line graph cho lưu lượng

1. Trong dashboard, click **Add widget** → chọn **Line** (Line graph).
2. Trong modal **Add metric**, chọn **Browse** → **AWS/VPN** → **By Tunnel** (hoặc By VPN Connection tùy giao diện).
3. Chọn metric **TunnelDataIn** và **TunnelDataOut** cho cả hai tunnel (từ cùng VPN connection) — bạn có thể thêm nhiều series trong cùng widget.
4. Điều chỉnh: Statistic = `Sum` hoặc `Average` (thường dùng `Sum` cho bytes), Period = `1 minute` hoặc `5 minutes` tùy nhu cầu.
5. Optionally, bật **Y axis (left/right)** nếu muốn hiển thị hai metric với scale khác nhau.
6. Click **Create widget**.

#### Thêm Single value widgets cho TunnelState

1. Click **Add widget** → chọn **Single value**.
2. Chọn metric **AWS/VPN → TunnelState** cho **Tunnel 1** (lọc theo `VpnConnectionId` và `TunnelIndex=1`).
3. Statistic: `Minimum` để nếu giá trị có lúc 0 sẽ hiển thị 0.
4. Title: `Tunnel 1 State` → Create widget.
5. Lặp lại cho **Tunnel 2** (TunnelIndex=2) để có 2 widget trạng thái riêng.

#### Thêm cảnh báo trạng thái & annotation

- Bạn có thể thêm **annotations** (text) hoặc widget **Alarm status** (nếu muốn hiển thị trạng thái alarm trên dashboard).
- Thêm widget **Text** để viết runbook ngắn (ví dụ các bước xử lý nhanh khi tunnel down).

#### Metric Math (tùy chọn)

- Để xem tổng lưu lượng qua cả 2 tunnel, khi thêm widget chọn **Add math expression** và dùng expression như: `m1 + m2` (với m1 = TunnelDataOut t1, m2 = TunnelDataOut t2).
- Đặt tên cho expression (ví dụ `TotalDataOut`) để dễ theo dõi.

#### Tùy chỉnh thời gian & refresh

- Ở góc trên dashboard, chọn time range (Last 1 hour / 3 hours / 24 hours) và Refresh interval (Auto / 1 minute / 5 minutes).

#### Lưu & chia sẻ

- Click **Save dashboard**.
- Bạn có thể export link (View in Console) hoặc bật **share** theo IAM để nhóm vận hành truy cập.

---

### CloudWatch Logs retention (đặt retention cho Lambda/SSM logs)

Đảm bảo logs không lưu giữ vô hạn để tránh chi phí tăng.

#### Thiết lập retention cho mỗi Log Group

1. Mở **CloudWatch** → **Logs** → **Log groups**.
2. Tìm log group của Lambda: `/aws/lambda/vpn-diagnostic-handler` → chọn log group.
3. Click **Actions** → **Edit retention** → chọn `90 days` (hoặc policy của bạn) → **Save**.
4. Lặp lại cho log group Systems Manager (SSM) output nếu có (ví dụ `/aws/ssm/commands` hoặc tên custom bạn dùng).

#### Lưu ý về logging volume

- Tránh in quá nhiều logs (verbose) trong Lambda khi chạy trong production. Sử dụng `LOG_LEVEL` để điều chỉnh.
- Dùng CloudWatch Logs Insights để run queries thay vì xuất toàn bộ logs xuống S3.

---