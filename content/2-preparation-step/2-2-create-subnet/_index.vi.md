---
title : "Tạo Subnet"
displayDate :  "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---

# TẠO SUBNET

#### Tạo Subnet trong Amazon VPC

🔒 Các bước thực hiện

1. Truy cập giao diện **VPC**
- Chọn **Subnets** từ menu bên trái
- Click vào **Create subnet**

![Tạo Subnet](/FCJ_Workshop_VuNgocQuang/images/2/2-2/0001.png?featherlight=false&width=90pc)

2. Chọn **VPC**
- Trong giao diện **Create subnet**
- Chọn VPC **ASG** đã tạo trước đó

![Tạo Subnet](/FCJ_Workshop_VuNgocQuang/images/2/2-2/0002.png?featherlight=false&width=90pc)

3. Cấu hình Subnet đầu tiên
- **Subnet name**: Nhập `Public Subnet 1`
- **Availability Zone**: Chọn **ap-southeast-1a**
- IPv4 CIDR: Nhập `10.10.1.0/24`
- Click **Create subnet**

![Tạo Subnet](/FCJ_Workshop_VuNgocQuang/images/2/2-2/0003.png?featherlight=false&width=90pc)

4. Xác nhận tạo subnet thành công

![Tạo Subnet](/FCJ_Workshop_VuNgocQuang/images/2/2-2/0004.png?featherlight=false&width=90pc)

💡 **Tạo các Subnet bổ sung**

5. Lặp lại quy trình để tạo thêm các subnet sau:
- **Public Subnet 2**
    - CIDR: **10.10.2.0/24**
    - AZ: **ap-southeast-1b**

![Tạo Subnet](/FCJ_Workshop_VuNgocQuang/images/2/2-2/0005.png?featherlight=false&width=90pc)

- **Private Subnet 1**
    - CIDR: **10.10.3.0/24**
    - AZ: **ap-southeast-1a**

![Tạo Subnet](/FCJ_Workshop_VuNgocQuang/images/2/2-2/0006.png?featherlight=false&width=90pc)

- **Private Subnet 2**
    - CIDR: **10.10.4.0/24**
    - AZ: **ap-southeast-1b**

![Tạo Subnet](/FCJ_Workshop_VuNgocQuang/images/2/2-2/0007.png?featherlight=false&width=90pc)

⚠️ Lưu ý về **Availability Zone AWS** sử dụng hai khái niệm:

- **Availability Zone (AZ)**: Tên hiển thị (ví dụ: ap-southeast-1a)
-** AZ ID**: Định danh thực tế của AZ AWS ánh xạ ngẫu nhiên AZ với AZ ID giữa các tài khoản để đảm bảo phân phối tài nguyên đồng đều.

![Tạo Subnet](/FCJ_Workshop_VuNgocQuang/images/2/2-2/0008.png?featherlight=false&width=90pc)

---

#### Cấu hình Auto-assign Public IP

ℹ️ **Mục đích Cho phép tự động cấp phát địa chỉ IP công cộng cho các instance trong public subnet.**

6. Cấu hình** Public Subnet 1**
- Chọn **subnet** từ danh sách
- Click **Actions**
- Chọn Edit **subnet** settings

![Tạo Subnet](/FCJ_Workshop_VuNgocQuang/images/2/2-2/0009.png?featherlight=false&width=90pc)

7. Kích hoạt tính năng **Auto-assign IP**
- Bật **Enable auto-assign public IPv4 address**
- Click **Save**


![Tạo Subnet](/FCJ_Workshop_VuNgocQuang/images/2/2-2/0010.png?featherlight=false&width=90pc)

![Tạo Subnet](/FCJ_Workshop_VuNgocQuang/images/2/2-2/0011.png?featherlight=false&width=90pc)

8. Lặp lại cấu hình cho **Public Subnet 2**

![Tạo Subnet](/FCJ_Workshop_VuNgocQuang/images/2/2-2/0012.png?featherlight=false&width=90pc)

![Tạo Subnet](/FCJ_Workshop_VuNgocQuang/images/2/2-2/0013.png?featherlight=false&width=90pc)

---