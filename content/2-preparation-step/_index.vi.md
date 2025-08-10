---
title : "Các bước chuẩn bị"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

# Các bước chuẩn bị

#### Tổng quan về Môi trường AWS VPC

ℹ️ **Mục tiêu Lab**

- Xây dựng môi trường VPC hoàn chỉnh từ đầu
- Triển khai các thành phần mạng cơ bản của AWS
- Thiết lập cấu trúc mạng an toàn và có khả năng mở rộng

⚠️ Kiến trúc Tổng thể Trong bài thực hành này, chúng ta sẽ xây dựng một mô hình VPC theo sơ đồ bên dưới:

![Diagram](/images/2/0001.png?featherlight=false&width=90pc)

🔒 **Các Thành phần Chính**

1. **VPC** - Môi trường mạng ảo riêng biệt
2. **Subnet** - Phân đoạn mạng cho các tài nguyên
3. **Internet Gateway** - Cổng kết nối internet
4. **Route Table** - Bảng định tuyến lưu lượng mạng

---

#### Các Bước Triển khai

💡 **Quy trình Thực hiện**

- [Tạo VPC](2-1-Create-VPC/)
- [Tạo Subnet](2-2-create-subnet/)
- [Tạo Internet Gateway](2-3-create-internet-gateway/)
- [Tạo Route Table](2-4-create-route-table/)

---