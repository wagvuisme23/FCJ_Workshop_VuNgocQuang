---
title : "Virtual MFA Device"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

{{% notice note %}}
To enable MFA, you need to log in to AWS using the root user.
{{% /notice %}}

#### Enable virtual MFA device through Console

To set up and activate a virtual MFA device:

1. Sign in to the AWS Console.
2. In the upper right corner, you will see your account name, click on it and select **My Security Credentials**.

![MFA](/images/2/0001.png?featherlight=false&width=90pc)

3. Expand **Multi-factor authentication (MFA)** and select **Assign MFA**.

![MFA](/images/2/0002.png?featherlight=false&width=90pc)



4. In the **Select MFA device** interface, enter **Device name**

- Select **MFA device** as **Authenticator app**
- Select **Next**

![MFA](/images/2/0003.png?featherlight=false&width=90pc)

5. Install a compatible app on your phone. [MFA App List](https://aws.amazon.com/iam/features/mfa/?audit=2019q1).

![MFA](/images/2/0004.png?featherlight=false&width=90pc)

6. You can find **Authenticator** from the Google Chrome extension. Then select **Add to Chrome**

![MFA](/images/2/0005.png?featherlight=false&width=90pc)

7. You use the MFA code to fill in the confirmation.

![MFA](/images/2/0006.png?featherlight=false&width=90pc)

8. Perform a QR code scan.

![MFA](/images/2/0007.png?featherlight=false&width=90pc)

9. After scanning the QR code, enter 2 codes of MFA.

![MFA](/images/2/0008.png?featherlight=false&width=90pc)

10. After entering the code select **Add MFA**

![MFA](/images/2/0009.png?featherlight=false&width=90pc)

11. Complete more MFA.

![MFA](/images/2/00010.png?featherlight=false&width=90pc)