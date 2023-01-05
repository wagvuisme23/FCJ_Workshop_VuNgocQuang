---
title : "Hard MFA Device"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.3 </b> "
---

**Content**

- [Enable other hardware MFA device through the Console](#enable-other-hardware-mfa-device-through-the-console)

#### Enable other hardware MFA device through the Console

1. Sign in to the AWS Console.
2. In the upper right corner, you will see your account name, select and select **My Security Credentials** then expand Multi-factor authentication (MFA).

![Image](/images/1-account-setup/MySecurity_v1.png?width=15pc)

3. To manage U2F security keys, you must have permissions from the following set of permissions. in the left sidebar, select **Policies** then select **Create policy**, select **JSON** tab and paste the below:


```js
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowManageOwnUserMFA",
            "Effect": "Allow",
            "Action": [
                "iam:DeactivateMFADevice",
                "iam:EnableMFADevice",
                "iam:GetUser",
                "iam:ListMFADevices",
                "iam:ResyncMFADevice"
            ],
            "Resource": "arn:aws:iam::*:user/${aws:username}"
        },
        {
            "Sid": "DenyAllExceptListedIfNoMFA",
            "Effect": "Deny",
            "NotAction": [
                "iam:EnableMFADevice",
                "iam:GetUser",
                "iam:ListMFADevices",
                "iam:ResyncMFADevice"
            ],
            "Resource": "arn:aws:iam::*:user/${aws:username}",
            "Condition": {
                "BoolIfExists": {
                    "aws:MultiFactorAuthPresent": "false"
                }
            }
        }
    ]
}
```
![MFA](/images/3/0001.png?featherlight=false&width=90pc)

4. Select **Next: Tags**. This is a screen of **Tags**, a tool used to differentiate AWS resources.
5. Select **Next: Review**. This is the screen that allows you to review the permission set you are creating.
6. Enter the permission set name (eg MFAHardDevice) and select **Create policy**.

![MFA](/images/3/0002.png?featherlight=false&width=90pc)

![MFA](/images/3/0003.png?featherlight=false&width=90pc)

7. In the left sidebar, select **Dashboard** and then select **Enable MFA**.

![MFA](/images/3/0004.png?featherlight=false&width=90pc)

8. Expand Multi-factor authentication (MFA) then select **Active MFA**.

9. In **Manage MFA Device**, select **Other Hardware MFA Device** then click **Continue**.
10. Enter **Serial Number** on the back of the device.

![Image](/images/1-account-setup/HardwareMFA.png?featherlight=false&width=90pc)

11. Enter MFA code 1 then wait 30 seconds and enter MFA code 2.
12. Select **Assign MFA**.