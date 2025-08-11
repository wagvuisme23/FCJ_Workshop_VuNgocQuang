---
title : "Tạo VPG"
displayDate :  "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.2.1 </b> "
---

# Tạo Virtual Private Gateway

#### Tạo Virtual Private Gateway (VGW)

ℹ️ **Tổng quan**

- Virtual Private Gateway (VGW) là thành phần quan trọng cho kết nối Site-to-Site VPN
- Đóng vai trò là điểm cuối VPN phía AWS
- Cần được gắn (attach) vào VPC đích để thiết lập kết nối

---

#### Các bước thực hiện

1. Truy cập **AWS VPC Console**
    - Điều hướng đến **Virtual Private Gateways**
    - Click Create **Virtual Private Gateway**

![Tạo VPG](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-1/0001.png?featherlight=false&width=90pc)

2. Cấu hình **Virtual Private Gateway**
    - **Name tag:** Nhập `VPN Gateway`
    - **ASN:** Chọn **Amazon default ASN**
    - Click **Create virtual private gateway**

![Tạo VPG](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-1/0002.png?featherlight=false&width=90pc)

{{% notice tip %}}
💡 **Pro Tip**

- Sử dụng **Amazon default ASN** là lựa chọn phù hợp cho hầu hết trường hợp
- Custom ASN chỉ cần thiết khi có yêu cầu đặc biệt về BGP routing
{{% /notice %}}

3. Gắn **VGW** vào **VPC**
    - Chọn **Actions**
    - Click **Attach to VPC**

![Tạo VPG](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-1/0003.png?featherlight=false&width=90pc)

4. Chọn **VPC** đích
    - Trong dropdown, chọn **VPC ASG**
    - Click **Attach to VPC**

![Tạo VPG](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-1/0004.png?featherlight=false&width=90pc)

{{% notice info %}}
⚠️ **Lưu ý quan trọng**

Đảm bảo VGW chuyển sang trạng thái **Attached** trước khi tiếp tục
Quá trình attach có thể mất vài phút để hoàn tất
{{% /notice %}}

5. Xác nhận trạng thái
    - Kiểm tra **State** hiển thị là **Attached**
    - VGW đã sẵn sàng cho cấu hình VPN tiếp theo

![Tạo VPG](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-1/0005.png?featherlight=false&width=90pc)

---