---
title : "Tạo kết nối tới VPN"
displayDate :  "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3.2.3 </b> "
---

# Tạo kết nối VPN

#### Thiết lập AWS Site-to-Site VPN Connection

---

#### Tạo VPN Connection

1. Truy cập **AWS VPC Console**
    - Điều hướng đến **Site-to-Site VPN Connections**
    - Click **Create VPN Connection**

![Tạo kết nối VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-3/0001.png?featherlight=false&width=90pc)

2. Cấu hình **VPN Connection** cơ bản
    - Name tag: Nhập `VPN Connection`
    - **Target Gateway Type**: Chọn **Virtual Private Gateway**
    - **Virtual Private Gateway**: Chọn **VPN Gateway** đã tạo
    - **Customer Gateway**: Chọn **Existing**
    - **Customer Gateway ID**: Chọn **Customer Gateway** đã tạo

![Tạo kết nối VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-3/0002.png?featherlight=false&width=90pc)

3. Cấu hình **Routing**
    - **Routing Options**: Chọn **Dynamic**

![Tạo kết nối VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-3/0002-1.png?featherlight=false&width=90pc)

{{% notice note %}}
Phải chọn **Dynamic** vì làm theo **BGP**, chọn **Static** sẽ phù hợp với môi trường ít thay đổi nhiều
{{% /notice %}}

{{% notice info %}}
Lưu ý:
- Ghi nhớ hoặc chụp màn hình hoặc chép các thông tin và lưu vào đâu đó
- Tại **VPN** -> **VPN Details**
{{% /notice %}}

![Tạo kết nối VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-3/0004-1.png?featherlight=false&width=90pc)

4. Khởi tạo **VPN Connection**
    - Review cấu hình
    - Click Create **VPN Connection**

![Tạo kết nối VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-3/0004.png?featherlight=false&width=90pc)

⚠️ Lưu ý

Quá trình khởi tạo VPN có thể mất 5-10 phút
Đợi trạng thái chuyển sang Available trước khi tiếp tục

![Tạo kết nối VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-3/0005.png?featherlight=false&width=90pc)

---

#### Cấu hình Route Propagation

1. Cấu hình cho **Public Route Table**
    - Truy cập **Route Tables** trong **VPC Console**
    - Chọn **route table** của **Public subnet**
    - Chọn tab **Route Propagation**
    - Click **Edit route propagation
**
![Tạo kết nối VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-3/0006.png?featherlight=false&width=90pc)

2. **Enable Route Propagation**
    - Chọn **Enable cho Virtual Private Gateway**
    - Click **Save**

![Tạo kết nối VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-3/0007.png?featherlight=false&width=90pc)

3. Xác nhận trạng thái
    - Kiểm tra **Route Propagation** đã chuyển sang **Yes**

![Tạo kết nối VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-3/0008.png?featherlight=false&width=90pc)

4. Lặp lại quy trình cho **Private Route Table**
    - Thực hiện tương tự các bước trên
    - Đảm bảo **route propagation** được enable cho cả **public** và **private subnets**

![Tạo kết nối VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-3/0009.png?featherlight=false&width=90pc)

![Tạo kết nối VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-3/0010.png?featherlight=false&width=90pc)

![Tạo kết nối VPN](/FCJ_Workshop_VuNgocQuang/images/3/3-2/3-2-3/0011.png?featherlight=false&width=90pc)

🔒 **Security Note**

- Route propagation tự động cập nhật bảng định tuyến khi có thay đổi
- Đảm bảo chỉ enable cho các route tables cần thiết để tránh rủi ro bảo mật

---