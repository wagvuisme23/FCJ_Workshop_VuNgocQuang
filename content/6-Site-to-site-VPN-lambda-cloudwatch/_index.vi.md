---
title : "Site-to-site VPN với Lambda giám sát hoạt động CloudWatch"
displayDate :  "`r Sys.Date()`"
weight : 6
chapter : false
pre : " <b> 6. </b> "
---

## Tổng quan các bước (Overview)

Phần này giúp bạn nắm nhanh luồng công việc và các bước chính trong workshop — đọc phần này trước khi thực hiện các bước chi tiết phía dưới.

Mục tiêu: triển khai Site‑to‑Site VPN với BGP và redundancy, thiết lập giám sát, tự động hoá failover bằng Lambda/EventBridge, tối ưu chi phí và cung cấp runbook xử lý sự cố.

Tóm tắt các bước chính (thứ tự khuyến nghị):

1. **Chuẩn bị & thông tin đầu vào** — Xác định VPC, on‑prem CIDR, public IP của router, ASN, EC2 test instance (SSM enabled).
2. **Tạo và cấu hình VPN (trên VPC Console)** — Tạo VGW, Customer Gateway, và VPN Connection (dynamic BGP, 2 tunnels). (Xem các phần chi tiết trước đó trong tài liệu.)
3. **Xác minh BGP & redundancy** — Đảm bảo cả 2 tunnel UP và BGP Established; thử mô phỏng failover để kiểm tra route propagation.
4. **Thiết lập giám sát** — Tạo CloudWatch metrics & Alarms cho `TunnelState`, `TunnelDataIn/Out`; cấu hình dashboard `vpn-dashboard` để trực quan hoá.
5. **Tự động hoá xử lý sự cố** — Tạo IAM Role cho Lambda (phần L), triển khai Lambda diagnostic (phần M) và EventBridge rule (phần E/N) để gọi Lambda khi Alarm ALARM.
6. **Test end‑to‑end** — Mô phỏng tunnel down, quan sát CloudWatch Alarm → EventBridge → Lambda → SSM invocation và kết quả ping/traceroute.
7. **Tối ưu chi phí & logging** — Thiết lập retention cho CloudWatch Logs, theo dõi Data Transfer bằng Cost Explorer, dùng Budgets/Alerts.
8. **Chuẩn bị runbook & troubleshooting** — Sử dụng bảng quick guide (phần 15) để xử lý sự cố nhanh.
9. **Dọn dẹp tài nguyên** — Sau khi hoàn thành lab, thực hiện các bước xóa resource (Lambda, EventBridge rule, VPN, VGW, CGW, VPC nếu cần) để tránh chi phí phát sinh.

Lưu ý an toàn & vận hành:

- Luôn thực hiện test trong maintenance window và thông báo với đội vận hành.
- Không thay đổi cả hai tunnel cùng lúc khi kiểm thử failover.
- Giữ bí mật (PSK, webhooks) trong Secrets Manager — không lưu vào biến môi trường plaintext.

---