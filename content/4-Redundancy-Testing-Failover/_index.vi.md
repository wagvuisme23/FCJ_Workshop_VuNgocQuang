---
title : "Redundancy Testing & Failover"
displayDate :  "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 4. </b> "
---

### 🎯 Mục tiêu
Kiểm tra khả năng dự phòng (redundancy) và tự động chuyển mạch (automatic failover) của kiến trúc Site-to-Site VPN sử dụng BGP.  
Đảm bảo rằng khi một tunnel gặp sự cố, lưu lượng sẽ tự động được định tuyến qua tunnel còn lại mà không làm gián đoạn dịch vụ.

---

### 📚 Giới thiệu lý thuyết
Trong cấu hình Site-to-Site VPN với BGP, AWS cung cấp **2 IPsec tunnels** để tăng tính sẵn sàng.  
BGP sẽ tự động phát hiện khi một tunnel mất kết nối và rút route BGP tương ứng khỏi bảng định tuyến.  
Lưu lượng sẽ được chuyển sang tunnel còn hoạt động.

Kiểm tra redundancy giúp:
- Xác nhận BGP failover hoạt động đúng.
- Đảm bảo các route động (dynamic routes) được cập nhật kịp thời.
- Xác minh thời gian downtime thực tế khi sự cố xảy ra.

---

### 🛠 Các bước thực hiện trên AWS Management Console

#### **Bước 1 — Xác minh trạng thái ban đầu**
1. Mở **AWS Management Console** → vào **VPC**.
2. Chọn **Site-to-Site VPN Connections**.
3. Chọn VPN connection của bạn.
4. Chuyển sang tab **Tunnel Details**.
5. Xác nhận cả **Tunnel 1** và **Tunnel 2** đang ở trạng thái:
   - **Status**: UP
   - **BGP Status**: UP

---

#### **Bước 2 — Kiểm tra bảng định tuyến (Routing Table)**
1. Trong VPC, vào **Route Tables**.
2. Chọn bảng định tuyến gắn với VPC Subnet đang thử nghiệm.
3. Xem tab **Routes**.
4. Xác nhận các route đi đến mạng on-premises đang có **Target** là:
   - `vgw-xxxxxxxx` hoặc `tgw-xxxxxxxx` (tùy kiến trúc)
5. Ghi chú ASN, prefix, và next hop từ BGP routes.

---

#### **Bước 3 — Mô phỏng sự cố trên Tunnel 1**
> Thao tác này thực hiện ở **phía on-premises** hoặc tạm thời thay đổi cấu hình để ngắt Tunnel 1.

Cách phổ biến:
- **Trên thiết bị on-prem**: shutdown interface hoặc IPsec policy của Tunnel 1.
- **Nếu không can thiệp được on-prem**: sử dụng AWS CLI/Console để **disable** Tunnel 1 bằng cách chỉnh lại Pre-shared Key sai tạm thời (chỉ để test, sau đó revert).

---

#### **Bước 4 — Quan sát failover**
1. Trong tab **Tunnel Details**, xem trạng thái của Tunnel 1 chuyển từ **UP** → **DOWN**.
2. Kiểm tra **Tunnel 2** vẫn **UP**.
3. Trong **Route Tables**, refresh và xác nhận:
   - Route BGP tương ứng Tunnel 1 đã biến mất.
   - Lưu lượng đang đi qua Tunnel 2.
4. Thực hiện ping/traceroute từ EC2 trong VPC tới server on-prem để xác nhận kết nối không bị gián đoạn.

---

#### **Bước 5 — Khôi phục Tunnel 1**
1. Khôi phục cấu hình chính xác cho Tunnel 1 trên thiết bị on-prem.
2. Quan sát Tunnel 1 trở lại trạng thái **UP** và BGP **Established**.
3. Xác nhận lại rằng BGP đã quảng bá route trở lại và có 2 đường active.

---

#### **Bước 6 — Thực hiện tương tự cho Tunnel 2**
- Lặp lại quy trình với Tunnel 2 để đảm bảo failover hai chiều hoạt động đúng.

---

### 📈 Xác minh thời gian failover
- Sử dụng CloudWatch Metrics cho VPN connection:
  1. Vào **CloudWatch** → **Metrics** → **VPN**.
  2. Theo dõi metric `TunnelState`, `BGPStatus`, `TunnelDataIn/Out`.
  3. Ghi nhận thời gian từ lúc tunnel down đến khi lưu lượng đi qua tunnel còn lại.

---

### 💰 Lưu ý về chi phí
- Việc duy trì cả 2 tunnels ở trạng thái UP không phát sinh thêm chi phí VPN cố định, nhưng lưu lượng qua tunnel vẫn tính phí Data Transfer.
- Nếu testing nhiều, có thể phát sinh chi phí CloudWatch Logs/Alarms.

---

### 🔒 Lưu ý bảo mật
- Sau khi testing bằng cách đổi Pre-shared Key hoặc tắt tunnel, phải khôi phục lại đúng thông số ban đầu.
- Chỉ thực hiện test trong khung giờ bảo trì hoặc khi có sự đồng thuận từ team vận hành.

---

✅ **Kết quả mong đợi**:
- Khi một tunnel bị down, lưu lượng sẽ tự động chuyển sang tunnel còn lại.
- Thời gian downtime thực tế chỉ bằng thời gian BGP detect + route propagation (thường 20–30 giây).
