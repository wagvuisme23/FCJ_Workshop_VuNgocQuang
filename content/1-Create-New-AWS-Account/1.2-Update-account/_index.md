---
title : "Update the AWS account"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 1.2 </b> "
---

#### Update the AWS account name, email address, or password for the root user

#### Editing AWS Account Information

To make changes to your AWS account's name, root user's password, or root user's email address, follow the steps below. Your email address and password serve as credentials for signing in as the AWS account root user.

> **Note:** Changes made to an AWS account may take up to four hours to propagate across all services.


#### Editing Account Name, Root User Password, or Email Address

**Minimum Permissions:**
To proceed with the following steps, you need to have at least the following IAM permissions:

- You must log in as the AWS account root user. No additional IAM permissions are required. These steps cannot be performed by an IAM user or role.

1. Sign in to the AWS Management Console using your AWS account's email address and password.

2. In the upper right corner of the console, click on your account name or account number, then select "Account."

3. On the Account page, locate "Account settings" and click "Edit." You will be prompted to re-authenticate for security reasons.

   > **Note:** If the "Edit" option is not visible, it indicates that you are not logged in as the root user of your AWS account. Account settings cannot be modified while signed in as an IAM user or role.

4. On the "Update Account Settings" page, choose "Edit" next to the field you wish to update.

   - For **Name**: On the "Update Your Account Name" page, enter the new account name in the "New account name" field, then click "Save changes."

     > **Note:** If you encounter issues modifying the AWS account name, check if there's a Service Control Policy (SCP) within AWS Organizations that restricts access to "aws-portal" or denies the "iam:UpdateAccountName" action.

   - For **Email**: On the "Update Your Email Address" page, provide the required information for "New email address," "Confirm new email address," and confirm your current password. Afterward, click "Save changes." A verification code will be sent to your new email address from "no-reply@verify.signin.aws." On the "Verify Your New Email Address" page, enter the verification code you received via email, then click "Save changes."

     > **Note:** The verification code may take up to 5 minutes to arrive. If you don't find the email in your inbox, remember to check your spam and junk folders.

   - For **Password**: On the "Update Your Password" page, fill in the fields for "Current password," "New password," and "Confirm new password." Then, click "Save changes." For additional guidance and best practices regarding root user password management, refer to [Change the Password for the AWS Account Root User](link-to-documentation).

5. Once you have completed all the desired changes, select "Done."

That's it! You've successfully updated your AWS account information.
