---
title : "Thiết bị MFA ảo"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

{{% notice note %}}
Để kích hoạt MFA, bạn cần đăng nhập vào AWS sử dụng root user. 
{{% /notice %}}

#### Kích hoạt thiết bị MFA ảo thông qua Console

Để thiết lập và kích hoạt thiết bị MFA ảo:

1. Đăng nhập vào AWS Console.
2. Góc trên bên phải, bạn sẽ thấy tên account của bạn, chọn vào và chọn **My Security Credentials**.

![MFA](/images/2/0001.png?featherlight=false&width=90pc)

3. Mở rộng **Multi-factor authentication (MFA)** và chọn **Assign MFA**.

![MFA](/images/2/0002.png?featherlight=false&width=90pc)



4. Trong giao diện **Select MFA device**, nhập **Device name**

- Chọn **MFA device** là **Authenticator app**
- Chọn **Next**

![MFA](/images/2/0003.png?featherlight=false&width=90pc)

5. Cài đặt ứng dụng tương thích trên điện thoại của bạn. [Danh sách ứng dụng MFA](https://aws.amazon.com/iam/features/mfa/?audit=2019q1).

![MFA](/images/2/0004.png?featherlight=false&width=90pc)

6. Bạn có thể tìm **Authenticator** từ extension của Google Chrome. Sau đó chọn **Add to Chrome**

![MFA](/images/2/0005.png?featherlight=false&width=90pc)

7. Bạn sử dụng MFA code để điền vào xác nhận.

![MFA](/images/2/0006.png?featherlight=false&width=90pc)

8. Thực hiện quét QR code.

![MFA](/images/2/0007.png?featherlight=false&width=90pc)

9. Sau khi quét QR code, bạn nhập 2 code của MFA.

![MFA](/images/2/0008.png?featherlight=false&width=90pc)

10. Sau khi nhập code chọn **Add MFA**

![MFA](/images/2/0009.png?featherlight=false&width=90pc)

11. Hoàn thành thêm MFA.

![MFA](/images/2/00010.png?featherlight=false&width=90pc)