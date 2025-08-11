---
title : "Tạo Site-to-Site-VPN"
displayDate :  "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3. </b> "
---

#### AWS Site-to-Site VPN

ℹ️ **Tổng quan**

- **AWS Site-to-Site VPN** cho phép tạo kết nối bảo mật giữa **on-premises data center** hoặc **branch office network** và **Amazon VPC**.
- Hỗ trợ cả **hardware VPN appliances** và **software-based VPN solutions** tùy theo nhu cầu.
- Sử dụng **IPSec tunnels** để mã hóa dữ liệu khi truyền qua Internet.
- Yêu cầu cấu hình **Virtual Private Gateway (VPG)** ở phía AWS và **Customer Gateway (CGW)** ở phía khách hàng.

---

#### Thành phần chính

🔒 **Virtual Private Gateway (VPG)**

- Là **VPN endpoint** được triển khai trong AWS.
- Kết nối trực tiếp với **VPC** thông qua **VPC attachments**.
- Quản lý và điều phối các kết nối VPN.
- Hỗ trợ cả **dynamic routing** (BGP) và **static routing**.
- Mỗi VPC chỉ có thể gắn một VPG tại một thời điểm.

🔒 **Customer Gateway (CGW)**

- Đại diện cho thiết bị VPN phía khách hàng (**on-premises**).
- Có thể là thiết bị phần cứng (**Cisco, Juniper, Fortinet, etc.**) hoặc phần mềm (**StrongSwan, OpenVPN**).
- Yêu cầu **một địa chỉ IP public tĩnh duy nhất** trong mỗi region.
- Phải khai báo **ASN (Autonomous System Number)** nếu dùng BGP.

🔒 **Redundancy & BGP**

- Mỗi kết nối VPN trên AWS tạo **hai tunnel IPSec độc lập** để đảm bảo tính dự phòng.
- Sử dụng **BGP** để trao đổi route động giữa AWS và on-premises.
- **Automatic Failover:** Khi một tunnel gặp sự cố, BGP sẽ tự động cập nhật route và chuyển lưu lượng sang tunnel còn lại mà không gián đoạn dịch vụ.
- Có thể thiết lập **tối ưu hóa định tuyến** bằng cách cấu hình **BGP metrics** và **AS_PATH prepending** để ưu tiên một tunnel làm primary và tunnel còn lại làm backup.

🔒 **CloudWatch Alarm & Monitoring**

- Sử dụng **Amazon CloudWatch Metrics** để giám sát trạng thái VPN tunnels (`TunnelState`, `TunnelDataIn`, `TunnelDataOut`).
- Tạo **CloudWatch Alarms** để cảnh báo khi tunnel down.
- Kết hợp **SNS (Simple Notification Service)** để gửi thông báo qua email/SMS.

🔒 **Automation with Lambda**

- Sử dụng **AWS Lambda** để tự động hóa quy trình failover hoặc xử lý sự cố.
- Có thể tích hợp với **CloudWatch Events** để kích hoạt Lambda khi tunnel bị mất kết nối.
- Tự động điều chỉnh route trong **VPC Route Tables** hoặc cập nhật **on-premises firewall rules** khi có thay đổi.

---

#### Quy trình triển khai trên AWS Management Console

1. **Tạo Virtual Private Gateway (VPG)** và gắn vào VPC.
2. **Tạo Customer Gateway (CGW)** với IP public và ASN của thiết bị on-premises.
3. **Tạo Site-to-Site VPN Connection**, chọn **Dynamic (BGP)** hoặc **Static** routing.
4. **Tải file cấu hình VPN** tương thích với thiết bị (Cisco, Juniper, Fortinet, v.v.).
5. **Cấu hình thiết bị VPN on-premises** dựa trên file đã tải.
6. **Kiểm tra trạng thái tunnel** trong AWS Console.
7. **Cấu hình CloudWatch Alarm và SNS** để giám sát.
8. **Thử nghiệm failover** bằng cách tạm thời tắt một tunnel và kiểm tra lưu lượng.
9. **Xác nhận BGP routes** đã được trao đổi đầy đủ giữa hai phía.

---