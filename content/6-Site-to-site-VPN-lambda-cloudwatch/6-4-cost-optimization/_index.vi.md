---
title : "Cost optimization"
displayDate :  "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 6.4 </b> "
---

## Cost optimization

Phần này hướng dẫn cách tối ưu chi phí liên quan tới VPN, data transfer, CloudWatch và EC2 test machines.

### Hiểu chi phí VPN

- **VPN hourly**: AWS tính phí theo **connection-hour** cho mỗi Site-to-Site VPN connection (mỗi connection có 2 tunnels).

   - Ví dụ giả định: 0.05 USD/connection-hour.
   - Nếu duy trì 1 kết nối trong 24 giờ: 24 × 0.05 = **1.2 USD/ngày**.
   - Nếu thử nghiệm 5 ngày/tuần, 4 tuần: 5 × 4 × 1.2 = **24 USD/tháng**.

- **Tối ưu**: trong môi trường lab hay workshop, **tắt VPN khi không cần thiết** để tiết kiệm ~1 USD/ngày.

- **Data transfer**: Data outbound (Internet) có thể bị tính phí theo GB (ví dụ 0.09 USD/GB).

   - Ví dụ chuyển 50 GB dữ liệu: 50 × 0.09 = **4.5 USD**.

> Không đề cập giá cụ thể ở đây — kiểm tra Billing / Cost Explorer để biết giá theo region và tài khoản.

### Theo dõi Data Transfer bằng Cost Explorer

1. Vào **Billing & Cost Management** → **Cost Explorer** → **Launch Cost Explorer**.
2. Tạo báo cáo custom:
   - **Group by**: `Service` → lọc `Amazon VPC` / `AWS VPN` / `Data Transfer`.
   - **Time range**: Last 30 days.
   - **Add filter**: `Usage Type` chứa `DataTransfer-Out`.
3. **Lưu** báo cáo để theo dõi hàng tuần.

### Tối ưu CloudWatch và Logs

- **Retention**: đặt retention hợp lý (ví dụ 30–90 ngày) cho log groups quan trọng.
- **Metric Filters**: chỉ tạo các filter thật sự cần thiết — mỗi filter có thể tính phí ingestion.
- **Log verbosity**: trong Lambda, dùng `INFO` hoặc `WARN` cho production; chỉ bật `DEBUG` khi cần phân tích.

### EC2 và chi phí lab

- Dùng instance nhỏ như **t2.micro** (~0.0104 USD/giờ) hoặc **t3.small** (~0.0208 USD/giờ) cho VM giả lập router.
   - Ví dụ: t2.micro chạy 24 h = 24 × 0.0104 ≈ **0.25 USD/ngày**.
- Trong production, cần tính network throughput — chọn instance có ENA/high throughput.
- **Tắt (stop)** hoặc **terminate** EC2 test khi không sử dụng.

### Budgets & Alerts

1. **AWS Budgets** → **Create budget**:
   - Loại budget: **Cost budget** hoặc **Usage budget** (Data Transfer GB).
   - Ví dụ: đặt threshold ở **80%** và **100%** → gửi cảnh báo qua email/SNS.
2. Tạo **Cost Anomaly Detection** để cảnh báo nếu chi phí phát sinh bất thường.

### Tagging & Chargeback

- Tag rõ các resource (VPN, EC2, VPC) theo dự án hoặc team — giúp phân tích chi phí đúng nhóm.
- Sử dụng **Tag Policy** để đảm bảo tất cả resource mới đều được gắn tag bắt buộc (như `Project`, `Owner`, `Environment`).

---

### Bảng minh họa chi phí (giả định)

| Resource                 | Usage                | Cost unit         | Estimated cost     |
|--------------------------|----------------------|-------------------|---------------------|
| VPN connection           | 24 h × 1 conn        | 0.05 USD/connection-hour | **1.2 USD/day**       |
| EC2 (t2.micro)           | 24 h × 30 days       | ~0.0104 USD/hour     | **~7.5 USD/month**    |
| Data transfer            | 50 GB outbound       | 0.09 USD/GB         | **4.5 USD**           |
| CloudWatch logs retention | reduce to 30 days    | lower log volume → lower cost | savings tùy volume |
| Metric filters           | limit filters       | ingestion cost      | savings ~0.03-0.1 USD/filter/day |
| Total (lab scenario)     | VPN + EC2 + Data     |                    | **~13.2 USD/month**   |

> **Lưu ý**: Đây là bảng ví dụ giả định để hình dung mức chi phí tiềm năng — bạn nên thay bằng số liệu thực tế từ **Cost Explorer** và **Billing Dashboard** của tài khoản bạn.

---
