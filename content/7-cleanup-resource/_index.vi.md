---
title : "Dọn dẹp tài nguyên"
displayDate :  "`r Sys.Date()`"
weight : 7
chapter : false
pre : " <b> 7. </b> "
---

# DỌN DẸP TÀI NGUYÊN

### Xóa VPC Endpoints

1. Truy cập vào giao diện Endpoints trong VPC Console
    - Chọn Action
    - Chọn Delete VPC endpoints
    - Nhập “delete” để xác nhận

![Dọn dẹp tài nguyên](/FCJ_Workshop_VuNgocQuang/images/7/0001.png?featherlight=false&width=90pc)

---

### Xóa các tài nguyên VPN

ℹ️ Thông tin: Các tài nguyên VPN cần được xóa theo thứ tự phù hợp để tránh lỗi phụ thuộc.

1. Xóa VPN Site-to-Site connection

![Dọn dẹp tài nguyên](/FCJ_Workshop_VuNgocQuang/images/7/0002.png?featherlight=false&width=90pc)

![Dọn dẹp tài nguyên](/FCJ_Workshop_VuNgocQuang/images/7/0003.png?featherlight=false&width=90pc)

2. Xóa Virtual Private Gateway
    - Trước tiên, detach Virtual Private Gateway khỏi VPC (nếu đang attached)
    - Sau đó xóa Virtual Private Gateway

![Dọn dẹp tài nguyên](/FCJ_Workshop_VuNgocQuang/images/7/0004.png?featherlight=false&width=90pc)

![Dọn dẹp tài nguyên](/FCJ_Workshop_VuNgocQuang/images/7/0005.png?featherlight=false&width=90pc)

![Dọn dẹp tài nguyên](/FCJ_Workshop_VuNgocQuang/images/7/0006.png?featherlight=false&width=90pc)

![Dọn dẹp tài nguyên](/FCJ_Workshop_VuNgocQuang/images/7/0007.png?featherlight=false&width=90pc)

3. Xóa Customer Gateway

![Dọn dẹp tài nguyên](/FCJ_Workshop_VuNgocQuang/images/7/0008.png?featherlight=false&width=90pc)

![Dọn dẹp tài nguyên](/FCJ_Workshop_VuNgocQuang/images/7/0009.png?featherlight=false&width=90pc)

---

### Xóa VPC

1. Xóa VPC ASG VPN

![Dọn dẹp tài nguyên](/FCJ_Workshop_VuNgocQuang/images/7/0010.png?featherlight=false&width=90pc)

![Dọn dẹp tài nguyên](/FCJ_Workshop_VuNgocQuang/images/7/0011.png?featherlight=false&width=90pc)

2. Xóa VPC ASG

![Dọn dẹp tài nguyên](/FCJ_Workshop_VuNgocQuang/images/7/0012.png?featherlight=false&width=90pc)

![Dọn dẹp tài nguyên](/FCJ_Workshop_VuNgocQuang/images/7/0013.png?featherlight=false&width=90pc)

---

### Xoá Alarm trong Metrics của CloudWatch

1. Chọn All alarms bên trái tại mục Alarms, chọn mục Alarm cần xóa rồi chọn Action, chọn Delete

![Dọn dẹp tài nguyên](/FCJ_Workshop_VuNgocQuang/images/7/0014.png?featherlight=false&width=90pc)

![Dọn dẹp tài nguyên](/FCJ_Workshop_VuNgocQuang/images/7/0015.png?featherlight=false&width=90pc)

---

### Xóa EventBridge Rule

1. Mở **AWS Management Console**.
2. Tìm và chọn dịch vụ **Amazon EventBridge**.
3. Trong menu bên trái, chọn **Rules**.
4. Trong danh sách rules, tìm **Rule** đã tạo trong workshop (ví dụ: `Alarm-to-Lambda-Rule`).
5. Tick chọn vào checkbox của rule đó.
6. Ở góc trên bên phải, chọn **Actions** → **Delete**.
7. Hộp thoại xác nhận sẽ xuất hiện, nhập **Delete** (nếu yêu cầu) và bấm **Delete** để xác nhận xóa rule.

---

### Xóa AWS Lambda Function

1. Từ **AWS Management Console**, tìm và mở dịch vụ **Lambda**.
2. Trong danh sách functions, tìm function đã tạo (ví dụ: `vpn-diagnostic-handler`).
3. Click vào tên function để mở chi tiết.
4. Ở góc trên bên phải, chọn **Actions** → **Delete function**.
5. Hộp thoại xác nhận xuất hiện, nhập **delete** (nếu được yêu cầu) và bấm **Delete** để xóa function.

---

### Xóa IAM Role & Policy

1. Từ **AWS Management Console**, tìm và chọn dịch vụ **IAM**.
2. Trong menu bên trái, chọn **Roles**.
3. Tìm **IAM Role** đã tạo cho Lambda (ví dụ: `lambda-vpn-diagnostic-role`).
4. Click vào tên role để mở chi tiết.
5. Ở góc trên bên phải, chọn **Delete role**.
6. Hộp thoại xác nhận xuất hiện, bấm **Yes, Delete**.
7. Nếu role này gắn với **custom policy** (tự tạo trong workshop):
    - Trong menu bên trái của IAM, chọn **Policies**.
    - Tìm policy đã tạo (ví dụ: `lambda-vpn-diagnostic-policy`).
    - Tick chọn policy → **Actions** → **Delete**.
    - Xác nhận xóa policy.

---

### Kiểm tra tài nguyên khác (nếu có)

- Nếu trong workshop bạn có tạo thêm **CloudWatch Alarms**, có thể xóa chúng:
    1. Mở **Amazon CloudWatch**.
    2. Chọn **Alarms** → tìm alarm đã tạo → chọn **Actions** → **Delete**.
- Kiểm tra lại toàn bộ các dịch vụ AWS đã dùng trong workshop để đảm bảo không còn tài nguyên hoạt động (VPC, VPN, Gateway, EC2, S3, ... nếu có).

---

{{% notice note %}}
🔒 **Security Note:** Khi xóa VPC, tất cả các tài nguyên liên quan như subnets, route tables, network ACLs, và security groups cũng sẽ bị xóa. Tuy nhiên, các tài nguyên như NAT Gateways, VPC Endpoints, và VPN Connections cần được xóa riêng trước khi xóa VPC.
{{% /notice %}}

---

{{% notice warning %}}
**LƯU Ý**: Hãy kiểm tra tất cả các dịch vụ lại 1 lần nữa để chắc chắn rằng, **KHÔNG** còn một dịch vụ nào đang chạy ngầm hoặc chưa xóa để không bị tính phí lâu dài.

Kiểm tra các khu vực của dịch vụ AWS để chắn chắc rằng bạn **KHÔNG** bị bỏ sót dịch vụ khi sử dụng tại khu vực nào khác.
{{% /notice %}}

![Dọn dẹp tài nguyên](/FCJ_Workshop_VuNgocQuang/images/7/0016.png?featherlight=false&width=90pc)

---