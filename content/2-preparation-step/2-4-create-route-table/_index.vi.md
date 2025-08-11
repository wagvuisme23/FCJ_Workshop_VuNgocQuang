---
title : "Tạo Route Table"
displayDate :  "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 2.4 </b> "
---

#### Tạo Route Table trong Amazon VPC

🔒 **Các bước thực hiện**

1. Truy cập giao diện **VPC**
    - Chọn **Route Tables** từ menu bên trái
    - Click vào **Create route table**

![Tạo Route Table](/FCJ_Workshop_VuNgocQuang/images/2/2-4/0001.png?featherlight=false&width=90pc)

2. Cấu hình **Route Table**
    - Name: Nhập `Route table-Public`
    - VPC: Chọn **VPC ASG** (VPC ID sẽ tự động điền)
    - Click **Create route table**

![Tạo Route Table](/FCJ_Workshop_VuNgocQuang/images/2/2-4/0002.png?featherlight=false&width=90pc)

3. Xác nhận tạo Route Table thành công

![Tạo Route Table](/FCJ_Workshop_VuNgocQuang/images/2/2-4/0003.png?featherlight=false&width=90pc)

💡 **Cấu hình định tuyến**

4. Thêm route cho Internet Gateway
    - Click **Actions**
    - Chọn **Edit routes**

![Tạo Route Table](/FCJ_Workshop_VuNgocQuang/images/2/2-4/0004.png?featherlight=false&width=90pc)

5. Cấu hình route mới
    - Click **Add route**
    - Destination: Nhập `0.0.0.0/0` (đại diện cho internet)
    - Target: Chọn **Internet Gateway** và chọn **IGW** đã tạo
    - Click **Save changes**

![Tạo Route Table](/FCJ_Workshop_VuNgocQuang/images/2/2-4/0005.png?featherlight=false&width=90pc)

⚠️ Liên kết với Subnet

6. Thiết lập **subnet associations**
    - Chọn tab **Subnet associations**
    - Click **Edit subnet associations**

![Tạo Route Table](/FCJ_Workshop_VuNgocQuang/images/2/2-4/0006.png?featherlight=false&width=90pc)

7. Chọn các **public subnet**
    - Mở rộng cột **Subnet ID** để xem chi tiết
    - Chọn cả **2 public subnet** đã tạo
    - Click **Save associations**

![Tạo Route Table](/FCJ_Workshop_VuNgocQuang/images/2/2-4/0007.png?featherlight=false&width=90pc)

8. Xác nhận cấu hình **subnet associations** thành công

![Tạo Route Table](/FCJ_Workshop_VuNgocQuang/images/2/2-4/0008.png?featherlight=false&width=90pc)

---