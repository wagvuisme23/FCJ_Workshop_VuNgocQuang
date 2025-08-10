---
title : "Cấu hình kết nối VPN"
displayDate :  "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 3.2 </b> "
---

# Cấu hình kết nối VPN

#### Cấu hình AWS Site-to-Site VPN

ℹ️ Tổng quan

- Phần này hướng dẫn thiết lập kết nối AWS Site-to-Site VPN.
- Bao gồm cấu hình Virtual Private Gateway (VGW) và Customer Gateway (CGW).
- Cho phép kết nối bảo mật giữa hai VPC thông qua IPSec tunnels.

🔒 Thành phần chính

- Virtual Private Gateway (VGW): Điểm cuối VPN phía AWS.
- Customer Gateway (CGW): Đại diện cho thiết bị VPN phía khách hàng.
- VPN Connection: Kết nối IPSec giữa VGW và CGW.

---

#### Các bước triển khai

💡 **Quy trình cấu hình**

1. [Tạo VPG](3-2-1-create-VPG/)
2. [Tạo Customer Gateway](3-2-2-create-customer-gateway/)
3. [Tạo Kết nối VPN](3-2-3-create-vpn-connection/)