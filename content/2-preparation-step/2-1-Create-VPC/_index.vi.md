---
title : "Tạo VPC"
displayDate :  "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

# TẠO VPC

#### Tạo Amazon Virtual Private Cloud (VPC)

🔒 **Các bước thực hiện**

1. Truy cập **AWS Management Console**
- Tìm kiếm dịch vụ **VPC**
- Chọn **VPC** từ kết quả tìm kiếm

![Tạo VPC](/FCJ_Workshop_VuNgocQuang/images/2/2-1/0001.png?featherlight=false&width=90pc)

2. Trong giao diện **VPC Dashboard**
- Chọn **Your VPCs** từ menu bên trái
- Click vào **Create VPC**

![Tạo VPC](/FCJ_Workshop_VuNgocQuang/images/2/2-1/0002.png?featherlight=false&width=90pc)

3. Cấu hình thông số **VPC**
- Resources: Chọn **VPC only**
- Name tag: Nhập `ASG`
- IPv4 CIDR: Nhập `10.10.0.0/16`

![Tạo VPC](/FCJ_Workshop_VuNgocQuang/images/2/2-1/0003.png?featherlight=false&width=90pc)

{{% notice info %}}
⚠️ **Lưu ý** về Tenancy Giữ tùy chọn Tenancy ở chế độ mặc định **(Default)**. Việc chuyển sang **Dedicated** có thể giới hạn các loại EC2 Instance được hỗ trợ trong VPC.
{{% /notice %}}

4. Xác nhận tạo **VPC**
- Click **Create VPC** để hoàn tất quá trình

![Tạo VPC](/FCJ_Workshop_VuNgocQuang/images/2/2-1/0004.png?featherlight=false&width=90pc)

5. Kiểm tra trạng thái **VPC** sau khi tạo

![Tạo VPC](/FCJ_Workshop_VuNgocQuang/images/2/2-1/0005.png?featherlight=false&width=90pc)

💡 **Cấu hình DNS** 
6. Kích hoạt tính năng **DNS** cho **VPC**
- Chọn **Edit VPC settings**
- Mở tab **DNS settings**
- Bật **DNS hostnames** và **DNS resolution**
- Lưu thay đổi

![Tạo VPC](/FCJ_Workshop_VuNgocQuang/images/2/2-1/0006.png?featherlight=false&width=90pc)

---