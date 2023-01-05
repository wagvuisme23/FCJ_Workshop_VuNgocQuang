---
title : "Create Admin Group and Admin User"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3. </b> "
---



#### Create Admin Group

1. **Login to Dashboard** at [AWS Web Service page](https://aws.amazon.com/)

2. Click on the account name in the upper right corner and select **My Security Credentials**

![AWS IAM](/images/01/0001.png?featherlight=false&width=90pc)

{{%notice tip%}}
In case you do not see the **My Security Credentials** menu, you can click on the search icon and enter **IAM**. \
Then click on the IAM service to access the IAM service management interface.
{{%/notice%}}

![AWS IAM](/images/01/0002.png?featherlight=false&width=90pc)

3. In the left sidebar, select **User Groups** then select **Create Group**

![AWS IAM](/images/01/0003.png?featherlight=false&width=90pc)

4. Under **Name the group**, enter a Group name (Example: *AdminGroup*) and scroll down.

![AWS IAM](/images/01/0004.png?featherlight=false&width=90pc)

5. In the **Attach permissions policies** section, type **AdministratorAccesss** in the search bar and click it. Finally, select **Create Group**.

![AWS IAM](/images/01/0005.png?featherlight=false&width=90pc)

6. Finish creating admin group.

![AWS IAM](/images/01/0006.png?featherlight=false&width=90pc)

#### Create Admin User

1. In the left sidebar, select **Users** then select **Add User**

![AWS IAM](/images/02/0001.png?featherlight=false&width=90pc)

2. Enter the User name (Example: *AdminUser*).
    + Click **AWS Management Console access**.
    + Click **Programmatic Access**.
    + Click **Custom password** and then type a password of your choice (note: you must remember this password for future logins).
    + Uncheck the item **User must create a new password...**.
    + Click **Next:Permissions**.

![AWS IAM](/images/02/0002.png?featherlight=false&width=90pc)

{{% notice note %}}
 By selecting **AWS Management Console access**, you have just authorized the IAM User to access AWS through the AWS web console.\
 Removing the **User must create a new password...** entry allows the first user to log in to the IAM User without having to create a new password.
{{% /notice %}}

3. Click the **Add user to group** tab and click the **AdminGroup** that we created earlier.

![AWS IAM](/images/02/0003.png?featherlight=false&width=90pc)

4. Click **Next:Tags**
    - Tags are optional to organize, track, or control user access, so you can add tags or not.

5. Click **Next:Review**.

![AWS IAM](/images/02/0004.png?featherlight=false&width=90pc)

6. Check the information and select **Create user**

![AWS IAM](/images/02/0005.png?featherlight=false&width=90pc)

7. Complete user creation. You can download.csv to store the Access key.

![AWS IAM](/images/02/0006.png?featherlight=false&width=90pc)

8. Create admin user successfully.

![AWS IAM](/images/02/0007.png?featherlight=false&width=90pc)

9. Check user details.


![AWS IAM](/images/02/0008.png?featherlight=false&width=90pc)

{{% notice info %}}
After creating a user, you will see a dialog box to download the access key and secret key information. This is the information used to perform **Programmatic access** to AWS resources through the **AWS CLI** and **AWS SDK**. We will not be using it for the time being.
{{% /notice %}}

#### Login to AdminUser

1. Return to the IAM service, and select **Users** in the left sidebar.
2. Click on the name of the IAM User you just selected.
3. In the **Summary** section, select the **Security credentials** tab. Look at the line **Summary: Console sign-in link** and copy the link next to it. This is the link you use to log in to the IAM User.

![AWS IAM](/images/03/0001.png?featherlight=false&width=90pc)


4. Open an incognito tab of the browser you are using and paste the link into the search bar.

![AWS IAM](/images/03/0002.png?featherlight=false&width=90pc)

{{% notice info %}}
Incognito tab login allows you to log in to AWS with an IAM User without having to log out of the root user in the main tab.
{{% /notice %}}

5. Enter the correct IAM User name and password that you entered in the **create IAM User** section above. Click **sign in**.

![AWS IAM](/images/03/0003.png?featherlight=false&width=90pc)

6. Congratulations, you have successfully accessed your account as an IAM User **AdminUser**.


![AWS IAM](/images/03/0004.png?featherlight=false&width=90pc)


7. In The next step, we will switch to using IAM Role to improve the security of your account.