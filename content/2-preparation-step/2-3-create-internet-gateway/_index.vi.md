---
title : "Tạo Internet GateWay"
displayDate :  "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
---

# TẠO INTERNET GATEWAY

#### Tạo Internet Gateway trong Amazon VPC

🔒 **Các bước thực hiện**

1. Truy cập giao diện **VPC**
    - Chọn **Internet Gateways** từ menu bên trái
    - Click vào **Create internet gateway**

![Tạo Internet Gateway](/FCJ_Workshop_VuNgocQuang/images/2/2-3/0001.png?featherlight=false&width=90pc)

2. Cấu hình **Internet Gateway**
    - Tại **Name tag**, nhập `Internet Gateway`
    - Click **Create internet gateway**

![Tạo Internet Gateway](/FCJ_Workshop_VuNgocQuang/images/2/2-3/0002.png?featherlight=false&width=90pc)

3. Xác nhận tạo **Internet Gateway** thành công

![Tạo Internet Gateway](/FCJ_Workshop_VuNgocQuang/images/2/2-3/0003.png?featherlight=false&width=90pc)

💡 **Kết nối với VPC**

4. Gắn **Internet Gateway** vào **VPC**
    - Click **Actions**
    - Chọn **Attach to VPC**
    - Chọn **VPC ASG** từ danh sách (VPC ID sẽ tự động điền)
    - Click **Attach internet gateway**

![Tạo Internet Gateway](/FCJ_Workshop_VuNgocQuang/images/2/2-3/0004.png?featherlight=false&width=90pc)

⚠️ **Xác nhận trạng thái 5**. Sau khi gắn thành công:

- **State** của **Internet Gateway** sẽ chuyển sang **Attached**
- **IGW** đã sẵn sàng định tuyến lưu lượng internet cho VPC

![Tạo Internet Gateway](/FCJ_Workshop_VuNgocQuang/images/2/2-3/0005.png?featherlight=false&width=90pc)

---