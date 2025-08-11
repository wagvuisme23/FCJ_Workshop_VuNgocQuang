---
title : "Tạo VPC cho VPN"
displayDate :  "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---

# Tạo VPC cho VPN

#### Thiết lập VPC cho Site-to-Site VPN

⚠️ Yêu cầu tiên quyết

- Quyền truy cập vào AWS Console với đủ quyền tạo VPC resources
- Hiểu biết cơ bản về CIDR và subnet planning

---

#### Tạo VPC và Subnet

1. Truy cập **VPC Dashboard**
    - Chọn **Your VPCs**
    - Click **Create VPC**

![Tạo VPC Cho VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0001.png?featherlight=false&width=90pc)

2. Cấu hình **VPC** mới
    - **Resource type:** Chọn **VPC only**
    - **Name:** Nhập `ASG VPN`
    - IPv4 CIDR: Nhập `10.11.0.0/16`
    - Click **Create VPC**

![Tạo VPC Cho VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0002.png?featherlight=false&width=90pc)

![Tạo VPC Cho VPN](/images/3/3-1/0003.png?featherlight=false&width=90pc)

![Tạo VPC Cho VPN](/images/3/3-1/0004.png?featherlight=false&width=90pc)

3. Tạo **Public Subnet**
    - Truy cập **Subnets**
    - Click **Create subnet**
    - Chọn **VPC ASG VPN**

![Tạo VPC Cho VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0005.png?featherlight=false&width=90pc)

![Tạo VPC Cho VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0006.png?featherlight=false&width=90pc)

4. Cấu hình **Subnet**
    - Name: Nhập `VPN Public`
    - **Availability Zone:** Chọn **ap-southeast-1a**
    - IPv4 CIDR: Nhập `10.11.1.0/24`
    - Click **Create subnet**

![Tạo VPC Cho VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0007.png?featherlight=false&width=90pc)

![Tạo VPC Cho VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0008.png?featherlight=false&width=90pc)

---

#### Cấu hình Internet Connectivity

1. **Enable Auto-assign Public IP**
    - Chọn subnet **VPN Public**
    - Click **Actions** > **Edit subnet settings**
    - Chọn **Enable auto-assign public IPv4 address**
    - Click **Save**

![Tạo VPC Cho VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0009.png?featherlight=false&width=90pc)

![Tạo VPC Cho VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0010.png?featherlight=false&width=90pc)

![Tạo VPC Cho VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0011.png?featherlight=false&width=90pc)

2. Tạo **Internet Gateway**
    - Truy cập **Internet Gateways**
    - Click **Create internet gateway**
    - **Name:** Nhập `Internet Gateway VPN`
    - Click **Create**

![Tạo VPC Cho VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0012.png?featherlight=false&width=90pc)

![Tạo VPC Cho VPN](/images/3/3-1/0013.png?featherlight=false&width=90pc)

2. **Attach Internet Gateway vào VPC**
    - Chọn **IGW** vừa tạo
    - Click **Actions > Attach to VPC**
    - Chọn **VPC ASG VPN**
    - Click **Attach**

![Tạo VPC Cho VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0014.png?featherlight=false&width=90pc)

![Tạo VPC Cho VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0015.png?featherlight=false&width=90pc)

![Tạo VPC Cho VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0016.png?featherlight=false&width=90pc)

---

#### Cấu hình Route Table

1. Tạo **Route Table** mới
    - Truy cập **Route Tables**
    - Click **Create route table**
    - **Name:** Nhập `Route table VPN - Public`
    - **VPC:** Chọn **ASG VPN**
    - Click **Create**

![Tạo VPC Cho VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0017.png?featherlight=false&width=90pc)

![Tạo VPC Cho VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0018.png?featherlight=false&width=90pc)

2. Thêm **Route** cho **Internet Access**
    - Chọn tab **Routes**
    - Click **Edit routes**
    - Click **Add route**
    - Destination: Nhập `0.0.0.0/0`
    - Target: Chọn **Internet Gateway VPN**
    - Click **Save changes**

![Tạo VPC Cho VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0019.png?featherlight=false&width=90pc)

![Tạo VPC Cho VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0020.png?featherlight=false&width=90pc)

3. Liên kết **Subnet**
    - Chọn tab **Subnet associations**
    - Click **Edit subnet associations**
    - Chọn **subnet VPN Public**
    - Click **Save associations**

![Tạo VPC Cho VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0021.png?featherlight=false&width=90pc)

![Tạo VPC Cho VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0022.png?featherlight=false&width=90pc)

![Tạo VPC Cho VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-1/0023.png?featherlight=false&width=90pc)

{{% notice tip %}}
💡 **Pro Tip**

- Đảm bảo kiểm tra lại tất cả các cấu hình route và security group
- Sử dụng tags để quản lý tài nguyên hiệu quả
- Lưu trữ CIDR ranges trong tài liệu để tham khảo sau này
{{% /notice %}}

---