---
title : "Tổng quan"
displayDate :  "`r Sys.Date()`"
weight : 1 
chapter : false
---

#### TỔNG QUAN

**Site-to-Site VPN với BGP và Redundancy** là giải pháp kết nối an toàn, linh hoạt và có khả năng chịu lỗi cao giữa hạ tầng AWS và hệ thống on-premises (hoặc giữa hai hạ tầng cloud khác nhau). Giải pháp này tận dụng sức mạnh của **IPSec VPN**, **Border Gateway Protocol (BGP)** và cơ chế **redundancy** để đảm bảo kết nối luôn hoạt động liên tục ngay cả khi một đường truyền gặp sự cố.

Giải pháp được thiết kế dựa trên các thành phần chính:

- **AWS Virtual Private Gateway (VGW)** hoặc **AWS Transit Gateway (TGW)**: Là đầu mối kết nối phía AWS, chịu trách nhiệm quản lý các kết nối VPN và định tuyến.
- **Customer Gateway (CGW)**: Thiết bị hoặc phần mềm ở phía on-premises, đóng vai trò đối tác kết nối.
- **IPSec Tunnels**: Hai đường hầm bảo mật cho mỗi kết nối VPN, hoạt động song song nhằm cung cấp khả năng dự phòng.
- **BGP**: Giao thức định tuyến động giúp tự động cập nhật bảng định tuyến, giảm thiểu downtime khi xảy ra sự cố.
- **Redundancy & Failover**: Cơ chế phân luồng và chuyển hướng lưu lượng tự động khi một tunnel hoặc đường truyền bị lỗi.

**Lợi ích nổi bật của giải pháp:**
1. 🔒 **Bảo mật cao**: Sử dụng IPSec để mã hóa dữ liệu, đảm bảo an toàn trong quá trình truyền tải.
2. ⚡ **Tự động cập nhật định tuyến**: Nhờ BGP, việc thêm, gỡ bỏ hoặc thay đổi mạng đều được cập nhật tự động.
3. 🔄 **Dự phòng linh hoạt**: Với hai tunnel cho mỗi kết nối và có thể triển khai nhiều kết nối VPN song song, hệ thống luôn duy trì kết nối ổn định.
4. 📈 **Khả năng mở rộng**: Dễ dàng kết nối thêm site mới hoặc mở rộng dải mạng mà không phải thay đổi thủ công nhiều cấu hình.
5. 💰 **Tối ưu chi phí**: Tránh phụ thuộc hoàn toàn vào các đường truyền leased-line đắt đỏ, tận dụng hạ tầng internet sẵn có.

**Kịch bản triển khai điển hình:**
- Kết nối **AWS VPC** với trung tâm dữ liệu on-premises phục vụ hệ thống ERP, CRM.
- Đồng bộ dữ liệu thời gian thực giữa các cơ sở hạ tầng.
- Hỗ trợ mô hình **Hybrid Cloud** hoặc **Multi-Cloud**.
- Tích hợp giải pháp DR (Disaster Recovery) với khả năng chuyển mạch tự động.

Trong workshop này, chúng ta sẽ đi qua toàn bộ quy trình **thiết kế, triển khai, kiểm thử và giám sát** giải pháp Site-to-Site VPN với BGP và Redundancy, bao gồm:
- Thiết kế kiến trúc **highly available**
- Cấu hình **BGP Routing**
- Triển khai **multiple tunnels**
- Thiết lập **automatic failover**
- Tích hợp **monitoring & troubleshooting**
- Thực hiện **cost optimization**
- Kiểm tra **security compliance**
- Xây dựng **operational runbook & documentation**
