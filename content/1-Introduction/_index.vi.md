---
title : "Giới thiệu Site-to-Site VPN với BGP và Redundancy"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 1. </b> "
---

**Nội dung:**
- [Mục tiêu của Workshop](#-mục-tiêu-của-workshop)
- [Tổng quan lý thuyết](#-tổng-quan-lý-thuyết)
- [Mô hình kiến trúc đề xuất](#-mô-hình-kiến-trúc-đề-xuất)
- [Tình huống thực tế](#-tình-huống-thực-tế)
- [Những gì bạn sẽ học được](#-những-gì-bạn-sẽ-học-được)
- [Các công cụ và dịch vụ sử dụng](#-các-công-cụ-và-dịch-vụ-sử-dụng)
- [Kiến thức cần có trước khi bắt đầu](#️-kiến-thức-cần-có-trước-khi-bắt-đầu)

---

#### 🎯 Mục tiêu của Workshop

Workshop này hướng dẫn bạn từng bước triển khai một kết nối **Site-to-Site VPN** giữa hạ tầng **on-premises** và **Amazon VPC**, sử dụng **BGP (Border Gateway Protocol)** để tự động hóa định tuyến và đảm bảo **tính sẵn sàng cao (High Availability)** nhờ vào **nhiều tunnels dự phòng**.

Bạn cũng sẽ học cách:

- Giám sát trạng thái VPN và lưu lượng.
- Tự động hóa cảnh báo và xử lý sự cố.
- Kiểm thử failover giữa các tunnel.
- Bảo vệ tunnel với các best practice bảo mật.
- Ước lượng chi phí và tối ưu hóa vận hành.

---

#### 🧠 Tổng quan lý thuyết

1. 🌐 **Site-to-Site VPN là gì?**

Site-to-Site VPN là một phương thức kết nối an toàn giữa hai mạng riêng biệt thông qua internet bằng giao thức **IPSec**. VPN cho phép các máy chủ, ứng dụng ở hai đầu kết nối với nhau như trong cùng một mạng nội bộ.

{{% notice note %}}
Lợi ích: Không yêu cầu người dùng kết nối thủ công như Client VPN, mã hóa end-to-end, duy trì kết nối ổn định 24/7.
{{% /notice %}}

2. 🧭 **BGP là gì và tại sao nên dùng?**

**BGP (Border Gateway Protocol)** là giao thức định tuyến lớp mạng được thiết kế để trao đổi thông tin giữa các hệ thống mạng tự trị (AS). Trong VPN, BGP mang lại:

- 🧠 Học route tự động giữa on-premises và AWS.
- 🔁 Tự động chuyển đổi đường đi khi tunnel lỗi (failover).
- 📈 Tối ưu hóa đường đi, chọn tuyến tối ưu.
- 🔄 Không cần cập nhật route thủ công – dễ mở rộng.

BGP là bắt buộc nếu bạn cần tự động hóa định tuyến và đảm bảo tính linh hoạt khi mạng mở rộng.

3. 🏗️ **Redundancy với Multi-Tunnel VPN**

AWS cung cấp **2 tunnel IPSec** cho mỗi VPN Connection. Khi sử dụng BGP:

- Nếu 1 tunnel bị lỗi, BGP tự động chuyển sang tunnel còn lại.
- Cả hai tunnel có thể hoạt động đồng thời.
- Mô hình này **đảm bảo độ tin cậy và tính sẵn sàng cao**, phù hợp với các tổ chức yêu cầu kết nối không gián đoạn (ngân hàng, logistics, SaaS...).

---

#### 🧩 Mô hình kiến trúc đề xuất

- Một **VPC trên AWS** (10.0.0.0/16) làm mạng cloud.
- Một thiết bị mô phỏng on-premises router (VyOS hoặc pfSense) có IP tĩnh.
- **Customer Gateway**: đại diện thiết bị on-prem.
- **Virtual Private Gateway**: gắn với VPC.
- **Site-to-Site VPN** với 2 đường hầm hỗ trợ BGP.
- **CloudWatch + CloudTrail** để giám sát trạng thái.
- **IAM Role** để phân quyền quản lý.
- **VPC Flow Logs** để giám sát lưu lượng.

![Site-to-Site VPN Diagram](/images/1/0001.png?featherlight=false&width=90pc)

---

#### 💡 Tình huống thực tế

Công ty XYZ đang chuyển một phần hạ tầng ứng dụng lên AWS nhưng vẫn duy trì hệ thống kiểm soát và dữ liệu tại trung tâm dữ liệu on-premises.

Yêu cầu:

- Luồng dữ liệu giữa hai môi trường phải an toàn và ổn định.
- Hệ thống không được gián đoạn khi một tunnel VPN bị lỗi.
- Cần cơ chế định tuyến động khi địa chỉ IP trên on-prem thay đổi.
- Cần giám sát và xử lý lỗi kịp thời.

Giải pháp: Site-to-Site VPN + BGP + Monitoring + Multi-Tunnel Redundancy.

---

#### 🎓 Những gì bạn sẽ học được

| Năng lực đạt được                    | Mô tả chi tiết                             |
| ------------------------------------ | ------------------------------------------ |
| ✅ Thiết lập VPN với nhiều tunnel     | Tạo Site-to-Site VPN connection redundancy |
| ✅ Cấu hình BGP hai chiều             | Tự động học và quảng bá route              |
| ✅ Failover automation                | Khi tunnel down, traffic tự chuyển đổi     |
| ✅ Giám sát tunnel và định tuyến      | Với CloudWatch, CloudTrail                 |
| ✅ Theo dõi lưu lượng mạng            | Dùng VPC Flow Logs                         |
| ✅ Bảo vệ VPN connection              | Bảo mật PSK, phân quyền IAM                |
| ✅ Ước lượng và tối ưu hóa chi phí    | Dựa trên usage patterns                    |
| ✅ Ghi chú và vận hành hệ thống       | Chuẩn hóa tài liệu cho team ops            |

---

#### 📦 Các công cụ và dịch vụ sử dụng

- Amazon VPC  
- Virtual Private Gateway (VGW)  
- Customer Gateway (CGW)  
- EC2 Instance (router giả lập VyOS/pfSense)  
- Site-to-Site VPN  
- BGP Routing  
- CloudWatch & CloudTrail  
- IAM Roles & Policies  
- VPC Flow Logs  

---

#### 🛠️ Kiến thức cần có trước khi bắt đầu

- Nắm cơ bản về **AWS Networking**: VPC, Subnets, Route Table.
- Biết sử dụng **AWS Console hoặc CLI**.
- Hiểu kiến thức cơ bản về **routing IP, CIDR, IPSec**.
- Có kiến thức nền tảng về **BGP hoặc static routing**.
- Kinh nghiệm với **Linux, VyOS hoặc pfSense** là lợi thế.

---
