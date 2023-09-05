---
title : "Thiết bị MFA ảo"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

## Kích hoạt Multi-Factor Authentication (MFA) trên AWS

{{% notice note %}}
Để kích hoạt MFA, bạn cần đăng nhập vào AWS sử dụng tài khoản root.
{{% /notice %}}

## Kích hoạt thiết bị MFA ảo thông qua Console

Để thiết lập và kích hoạt thiết bị MFA ảo, bạn có thể tuân theo các bước sau:

1. Đăng nhập vào [AWS Console](https://aws.amazon.com/console/).
2. Ở góc trên bên phải, bạn sẽ thấy tên tài khoản của bạn. Nhấp vào tên và chọn **My Security Credentials**.

   ![MFA](/images/2/0001.png?featherlight=false&width=90pc)

3. Mở rộng mục **Multi-factor authentication (MFA)** và chọn **Assign MFA**.

   ![MFA](/images/2/0002.png?featherlight=false&width=90pc)

4. Trong giao diện **Select MFA device**, nhập tên cho thiết bị MFA của bạn:

   - Chọn **MFA device** là **Authenticator app**.
   - Chọn **Next**.

   ![MFA](/images/2/0003.png?featherlight=false&width=90pc)

5. Tiến hành cài đặt ứng dụng xác thực trên điện thoại của bạn. Danh sách [ứng dụng MFA tương thích](https://aws.amazon.com/iam/features/mfa/?audit=2019q1).

   ![MFA](/images/2/0004.png?featherlight=false&width=90pc)

6. Bạn có thể tìm ứng dụng **Authenticator** trên [Chrome Web Store](https://chrome.google.com/webstore/detail/authenticator/bhghoamapcdpbohphigoooaddinpkbai). Sau đó nhấp vào **Add to Chrome** để cài đặt.

   ![MFA](/images/2/0005.png?featherlight=false&width=90pc)

7. Sử dụng mã xác thực MFA để nhập vào xác nhận.

   ![MFA](/images/2/0006.png?featherlight=false&width=90pc)

8. Thực hiện quét mã QR.

   ![MFA](/images/2/0007.png?featherlight=false&width=90pc)

9. Sau khi quét mã QR, bạn cần nhập 2 mã xác thực từ ứng dụng MFA.

   ![MFA](/images/2/0008.png?featherlight=false&width=90pc)

10. Sau khi nhập mã xác thực, chọn **Add MFA** để hoàn thành quá trình thêm MFA.

   ![MFA](/images/2/0009.png?featherlight=false&width=90pc)

11. Quá trình thêm MFA đã hoàn tất.

   ![MFA](/images/2/00010.png?featherlight=false&width=90pc)
