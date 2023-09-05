---
title : "Create or update your AWS account alias"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 1.3 </b> "
---

####  Managing AWS Account Aliases for IAM User Sign-In

The AWS account root user and AWS Identity and Access Management (IAM) users in the account sign in using a web URL.

If you want the URL for your IAM users to contain your company name (or another easy-to-remember identifier) instead of the AWS account ID, you can create an account alias. This section provides information about AWS account aliases and lists the API operations that you can use to create an alias.

The sign-in page URL for your account's IAM users has the following format, by default:

```
https://Your_Account_ID.signin.aws.amazon.com/console/
```


If you create an AWS account alias for your AWS account ID, the IAM user sign-in page URL looks like the following example:

```
https://Your_Account_Alias.signin.aws.amazon.com/console/
```


The original URL containing your AWS account ID remains active and can still be used after you create your AWS account alias.

#### Tip
To create a bookmark for your account sign-in page in your web browser, we recommend that you manually type the sign-in URL in the bookmark entry. Don't use your web browser's "bookmark this page" feature, as it can capture session-specific information that might interfere with future visits to the page.

#### Considerations
- Your AWS account can have only one alias. Creating a new alias will overwrite the previous one, and the URL containing the previous alias will stop working.
- The account alias must be unique across all Amazon Web Services products.
- The alias can only contain lowercase letters, digits, and hyphens.

#### Create or Edit an Account Alias

#### Minimum Permissions
To perform the following steps, you must have at least the following IAM permissions:

- `iam:ListAccountAliases`
- `iam:CreateAccountAlias`

1. Sign in to the AWS Management Console as either the AWS account root user or as an IAM user or role that has the minimum permissions.

2. Open the IAM console at [https://console.aws.amazon.com/iam/](https://console.aws.amazon.com/iam/).

3. In the navigation pane, choose **Dashboard**.

4. In the right-hand pane under AWS account, next to **Account Alias**, choose **Create**. If an alias already exists, then choose **Edit**.

5. In the dialog box, enter the new or updated name you want to use for your alias, then choose **Save changes**.

**Note**: You can have only one alias associated with your AWS account at a time. If you create a new alias, the previous alias is removed, and the sign-in URL that was associated with the previous alias stops working.

---

#### Delete an Account Alias

#### Minimum Permissions
To perform the following steps, you must have at least the following IAM permissions:

- `iam:ListAccountAliases`
- `iam:CreateAccountAlias`
- `iam:DeleteAccountAlias`

1. Sign in to the AWS Management Console as either the AWS account root user or as an IAM user or role that has the minimum permissions.

2. Open the IAM console at [https://console.aws.amazon.com/iam/](https://console.aws.amazon.com/iam/).

3. In the navigation pane, choose **Dashboard**.

4. In the right-hand pane under AWS account, next to **Account Alias**, choose **Delete**.
