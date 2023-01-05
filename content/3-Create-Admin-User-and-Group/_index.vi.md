---
title : "Tạo Admin Group và Admin User"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 3. </b> "
---



#### Tạo Admin Group

1.	**Đăng nhập vào Bảng điều khiển** ở trang [AWS Web Service page](https://aws.amazon.com/)

2.	Nhấn vào tên tài khoản ở góc trên bên phải và chọn **My Security Credentials**

![AWS IAM](/images/01/0001.png?featherlight=false&width=90pc)

{{%notice tip%}}
Trong trường hợp không thấy menu **My Security Credentials**, bạn có thể click vào biểu tượng tìm kiếm và điền **IAM**. \
Sau đó click vào dịch vụ IAM để truy cập vào giao diện quản lý dịch vụ IAM.
{{%/notice%}}

![AWS IAM](/images/01/0002.png?featherlight=false&width=90pc)

3.	Ở thanh bên trái, chọn **User Groups** sau đó chọn **Create Group**

![AWS IAM](/images/01/0003.png?featherlight=false&width=90pc)

4.	Dưới mục **Name the group**, nhập tên Group (Ví dụ: *AdminGroup*) và cuộn chuột xuống dưới

![AWS IAM](/images/01/0004.png?featherlight=false&width=90pc)

5.	Ở phần **Attach permissions policies**, gõ **AdministratorAccesss** vào thanh tìm kiếm và nhấn chọn nó. Cuối cùng, chọn **Create Group**.

![AWS IAM](/images/01/0005.png?featherlight=false&width=90pc)

6. Hoàn thành tạo admin group.

![AWS IAM](/images/01/0006.png?featherlight=false&width=90pc)

#### Tạo Admin User

1.	Ở thanh bên trái, chọn **Users** sau đó chọn **Add User**

![AWS IAM](/images/02/0001.png?featherlight=false&width=90pc)

2.	Nhập tên User (Ví dụ: *AdminUser*).
    + Click **AWS Management Console access**. 
    + Click **Programmatic Access**.
    + Click **Custom password** rồi gõ một password tùy ý của bạn (lưu ý: bạn phải ghi nhớ mật khẩu này cho những lần đăng nhập trong tương lai). 
    + Bỏ chọn mục **User must create a new password...**.
    + Click **Next:Permissions**.

![AWS IAM](/images/02/0002.png?featherlight=false&width=90pc)

{{% notice note %}}
 Bằng cách chọn **AWS Management Console access**, bạn vừa cho phép IAM User được truy cập vào AWS thông qua bảng điều khiển AWS trên web.\
 Việc bỏ mục **User must create a new password...** cho phép người dùng khi lần đầu đăng nhập vào IAM User đó không cần phải tạo mật khẩu mới.
{{% /notice %}}

3.	Click tab **Add user to group** và click **AdminGroup** mà chúng ta tạo trước đó.

![AWS IAM](/images/02/0003.png?featherlight=false&width=90pc)

4.	Click **Next:Tags**
    - Tags (thẻ) là một tùy chọn không bắt buộc để tổ chức, theo dõi, hoặc điều khiển truy cập của user, thế nên bạn có thể thêm tags hoặc không.

5.	Click **Next:Review**.

![AWS IAM](/images/02/0004.png?featherlight=false&width=90pc)

6. Kiểm tra thông tin và chọn **Create user**

![AWS IAM](/images/02/0005.png?featherlight=false&width=90pc)

7. Hoàn thành tạo user. Có thể download.csv để lưu trữ Access key.

![AWS IAM](/images/02/0006.png?featherlight=false&width=90pc)

8. Tạo admin user thành công.

![AWS IAM](/images/02/0007.png?featherlight=false&width=90pc)

9.	Kiểm tra thông tin chi tiết user.


![AWS IAM](/images/02/0008.png?featherlight=false&width=90pc)

{{% notice info %}}
Sau khi tạo user, bạn sẽ thấy hiện lên hộp thoại download thông tin access key và secret key. Đây là thông tin dùng để thực hiện **Programmatic access** tới các tài nguyên của AWS thông qua **AWS CLI** và **AWS SDK**. Tạm thời chúng ta sẽ chưa sử dụng tới.
{{% /notice %}}


#### Đăng nhập vào AdminUser

1. Trở về dịch vụ IAM, và chọn **Users** ở thanh bên trái.
2. Nhấn vào tên của IAM User mà bạn vừa chọn.
3. Trong mục **Summary**, chọn tab **Security credentials**. Nhìn vào dòng **Summary: Console sign-in link** và copy đường link bên cạnh nó. Đây là đường link bạn dùng để đăng nhập vào IAM User.

![AWS IAM](/images/03/0001.png?featherlight=false&width=90pc)


4. Mở một tab ẩn danh của trình duyệt bạn đang sử dụng và paste đường link ấy vào thanh tìm kiếm.

![AWS IAM](/images/03/0002.png?featherlight=false&width=90pc)

{{% notice info %}}
Việc đăng nhập bằng tab ẩn danh cho phép bạn đăng nhập vào AWS bằng IAM User mà không cần phải log out khỏi root user trong tab chính.
{{% /notice %}}

5. Nhập đúng tên IAM User và password mà bạn đã nhập ở phần **tạo IAM User** ở trên. Nhấn **sign in**.

![AWS IAM](/images/03/0003.png?featherlight=false&width=90pc)

6. Chúc mừng, bạn đã truy cập thành công vào tài khoản của bạn dưới danh nghĩa của một IAM User **AdminUser**.


![AWS IAM](/images/03/0004.png?featherlight=false&width=90pc)


7. Bước tiếp theo, chúng ta sẽ chuyển sang sử dụng IAM Role để nâng cao tính bảo mật hơn cho account của bạn nhé.