---
title : "Tạo Customer Gateway"
displayDate :  "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 3.2.2 </b> "
---

# Tạo Customer Gateway

#### Tạo Customer Gateway

1. Truy cập vào **VPC**

    - Chọn **Customer Gateways**
    - Chọn **Create Customer Gateway**

![Tạo CGW](/images/3/3-2/3-2-2/0001.png?featherlight=false&width=90pc)

2. Trong giao diện **Create Customer Gateway**

    - Tại trường **Name tag**, nhập `Customer Gateway`
    - Tại trường **IP address**, nhập địa chỉ **Public IP** của máy chủ **EC2 Customer Gateway**
    - Chọn **Create Customer Gateway**

![Tạo CGW](/images/3/3-2/3-2-2/0002.png?featherlight=false&width=90pc)

![Tạo CGW](/images/3/3-2/3-2-2/0003.png?featherlight=false&width=90pc)

3. Quá trình tạo Customer Gateway sẽ hoàn tất sau khoảng 5 phút

![Tạo CGW](/images/3/3-2/3-2-2/0004.png?featherlight=false&width=90pc)

{{% notice info %}}
ℹ️ Thông tin quan trọng: Theo mô hình kiến trúc, Customer Gateway sẽ được triển khai trong VPC trên môi trường on-premise. Trong bước này, chúng ta đang khai báo với AWS về việc sử dụng một Customer Gateway với địa chỉ IP public của EC2 instance (Customer Gateway) nằm trong Auto Scaling Group của VPN VPC.
{{% /notice %}}

---