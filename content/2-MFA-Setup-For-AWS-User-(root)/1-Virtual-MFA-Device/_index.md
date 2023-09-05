---
title : "Virtual MFA Device"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

## Enabling Multi-Factor Authentication (MFA) on AWS

**Note:** Before proceeding, ensure you are logged in to AWS using the root user.

## Enable Virtual MFA Device through AWS Management Console

To enhance the security of your AWS account, you can set up Multi-Factor Authentication (MFA). This adds an extra layer of protection by requiring a second form of verification in addition to your password. Follow these steps to set up and activate a virtual MFA device:

1. Sign in to the AWS Management Console.

2. In the upper right corner of the console, you will see your account name. Click on it and select **My Security Credentials**.

   ![My Security Credentials](/images/2/0001.png?featherlight=false&width=90pc)

3. Expand the **Multi-factor authentication (MFA)** section and select **Assign MFA**.

   ![Assign MFA](/images/2/0002.png?featherlight=false&width=90pc)

4. In the **Select MFA Device** interface:

   - Enter a **Device Name**.
   - Select **MFA Device** as the **Authenticator App**.
   - Select **Next**.

   ![Select MFA Device](/images/2/0003.png?featherlight=false&width=90pc)

5. Install a compatible authenticator app on your smartphone. You can find a list of [MFA-compatible apps here](https://aws.amazon.com/iam/features/mfa/?audit=2019q1).

   ![MFA App List](/images/2/0004.png?featherlight=false&width=90pc)

6. Install the authenticator extension for Google Chrome. Select **Add to Chrome**.

   ![Authenticator Extension](/images/2/0005.png?featherlight=false&width=90pc)

7. Use the authenticator app to generate an MFA code and enter it for confirmation.

   ![MFA Code](/images/2/0006.png?featherlight=false&width=90pc)

8. Perform a QR code scan using the authenticator app.

   ![QR Code Scan](/images/2/0007.png?featherlight=false&width=90pc)

9. After scanning the QR code, enter the generated MFA codes into the corresponding fields.

   ![Enter MFA Codes](/images/2/0008.png?featherlight=false&width=90pc)

10. Once the codes are entered, select **Add MFA** to complete the setup.

   ![Add MFA](/images/2/0009.png?featherlight=false&width=90pc)

11. Complete the additional MFA setup steps as prompted.

   ![Additional MFA Setup](/images/2/00010.png?featherlight=false&width=90pc)

By setting up Multi-Factor Authentication, you add an extra layer of security to your AWS account, helping to protect your valuable resources and data.
