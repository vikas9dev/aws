# IAM and AWS CLI

Sections:-
- [1. IAM: Identity and Access Management](#1-iam-identity-and-access-management)
- [2. Creating IAM Users in AWS](#2-creating-iam-users-in-aws)
- [2.1. Multi-Session Support in AWS Console](#21-multi-session-support-in-aws-console)
- [3. IAM Policies In Depth](#3-iam-policies-in-depth)
- [4. IAM Policies Hands On](#4-iam-policies-hands-on)
- [5. Protecting Users and Groups: Password Policies and MFA](#5-protecting-users-and-groups-password-policies-and-mfa)
- [6. Securing Your AWS Account: Password Policies and Multi-Factor Authentication (MFA)](#6-securing-your-aws-account-password-policies-and-multi-factor-authentication-mfa)
- [7. Accessing AWS: Management Console, CLI, and SDK](#7-accessing-aws-management-console-cli-and-sdk)
- [8. Installing the AWS CLI on Windows](#8-installing-the-aws-cli-on-windows)
- [9. Installing the AWS CLI on macOS](#9-installing-the-aws-cli-on-macos)
- [10. Installing the AWS CLI on Linux](#10-installing-the-aws-cli-on-linux)
- [11. Creating Access Keys](#11-creating-access-keys)
- [12. AWS CloudShell](#12-aws-cloudshell)
- [13. IAM Roles for Services](#13-iam-roles-for-services)
- [14. Creating Roles in AWS](#14-creating-roles-in-aws)
- [15. IAM Security Tools](#15-iam-security-tools)
- [16. Generating Credentials Report and Using Last Access](#16-generating-credentials-report-and-using-last-access)
- [17. IAM Guidelines and Best Practices](#17-iam-guidelines-and-best-practices)
- [18. IAM Summary](#18-iam-summary)
- [Q & A](#q--a)

---


## 1. IAM: Identity and Access Management

IAM, or Identity and Access Management, is a global service in AWS that allows you to manage users and their access to AWS resources.

### Key Concepts 🔑

- IAM is used to create users and assign them to groups.
- When you create an AWS account, a root account is automatically created. This root account should only be used for initial setup.
- For day-to-day operations, create individual users within IAM.

### Users and Groups 👥

- A user represents a person within your organization.
- Users can be grouped together based on their roles or responsibilities.
- 📌 **Example:** An organization with six people: Alice, Bob, Charles, David, Edward, and Fred.
  - Alice, Bob, and Charles are developers, so they belong to the "Developers" group.
  - David and Edward are in operations, so they belong to the "Operations" group.
  - Fred might not belong to any group.
- **Groups can only contain users, not other groups.**
- A user can belong to multiple groups. Or user can exist without any group at all (not best practice).
  - 📌 **Example:** Charles and David might also be part of an "Audit" group.

![users-and-groups](/docs//img/IAM_Users_and_Groups.png)

### Permissions and Policies 🛡️

- Users and groups need permissions to use AWS resources.
- Permissions are granted using IAM policies, which are JSON documents.
- IAM policies define what actions a user or group is allowed to perform.
- 📌 **Example:** An IAM policy allowing users to use EC2, Elastic Load Balancing, and CloudWatch:

```json
{
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:Describe*",
        "elasticloadbalancing:Describe*",
        "cloudwatch:*"
      ],
      "Resource": "*"
    }
  ]
}
```

- 📝 **Note:** You don't need to be a programmer to understand IAM policies. They are written in a declarative way.

### Least Privilege Principle ⚠️

- AWS follows the principle of least privilege.
- Only grant users the minimum permissions they need to perform their tasks.
- This helps to prevent accidental misuse of resources and improves security.
- 💡 **Tip:** Avoid giving users more permissions than necessary.

---

## 2. Creating IAM Users in AWS

Let's practice using the IAM service to create users in AWS.

First, navigate to the IAM console:

1.  In the search bar, type `IAM`.
2.  Select IAM to access the IAM Dashboard.

Upon arriving on the IAM Dashboard, you'll see security recommendations. For now, we can ignore these.

Next, let's create a user:

1.  On the left-hand side, click on **Users**. This is where you create users for IAM.

📝 **Note:** IAM is a global service. This means the region selection is not active, and there is no region to be selected. When you create a user in IAM, it will be available everywhere. Other consoles we'll see in this course will be region-specific.

Why create users? Because right now, you are using the **root user**.

- If you click on your account details, you'll see only the account ID.
- Using the root account is not best practice.
- We want to create users, such as admin users, that will allow us to use our accounts more safely.

To create a user:

1.  Click **Add user**.
2.  Provide a username (e.g., Stephane).
3.  Provide the user access to the management console.
4.  You have the option to use Identity Center (recommended) or create an IAM user. Choose the second option (create an IAM user) because it is simpler and the one you need to know about for the exam.
5.  Set the password.
    - For a user that is not you, leave it as **Auto-generated password** and select **User must create a new password at next sign-in**.
    - Because it is you, you can enter a custom password and uncheck the box to change the password at the next login.
6.  Click **Next**.

Now, add permissions to the user. You can attach permissions directly to the user or you can get started with groups. Let's create a group:

1.  Click **Create group**.
2.  The group name will be `admin`.
3.  The policy name will be `AdministratorAccess`.
4.  Add the user to the `admin` group.
5.  Click **Next**.

Review everything. You'll see the username, the permissions on the group, and tags.

Tags are everywhere in AWS. They're optional, but they allow you to give metadata to many of your resources.

📌 **Example:** You could say that the department of Stephane is engineering.

Click **Create user**. The user is now created successfully.

You can email sign-in instructions or download a CSV file. Then you can log in with this user.

Let's return to the user list and have a look at everything.

- You'll see your user in the list.
- You'll also see groups.

If you go to **User groups** on the left-hand side, you'll see `admins`.

- `admins` has one user in it named Stephane.
- If you look at permissions of `admins`, you see that there is `AdministratorAccess` attached to the admin group.

If you go to your user, Stephane, you can look at permission policies and see it also has `AdministratorAccess`, but this one has not been attached directly. It has been attached via the group `admin`.

This means that Stephane inherited any permissions of the group `admin` it is in. This is why we put users in groups. It is a bit simpler to manage permissions this way.

Now let's go back to the dashboard and sign in with your user, Stephane.

First, look at your AWS account. It has an account ID and a Sign-in URL.

You can customize this Sign-in URL very easily by creating what's called an **account alias**.

📌 **Example:** You could create an alias `aws-stephane-v5`. The alias must be unique.

```text
aws-stephane-v5
```

Now, using this alias can simplify your signing URL.

To sign in using your Stephane account, you could use the same browser or create a new browser window in private mode.

The benefit of using a private window is that you can have two windows side by side using AWS.

⚠️ **Warning:** If you log in using the Stephane account on the right-hand side window, then you will be disconnected on the left-hand side.

To use two accounts at the same time (the root account on the left and your IAM account on the right), use a private window in your web browser. Chrome, Firefox, and Safari all have this feature.

By pasting the signing URL, you get the sign-in as an IAM user.

To get to this page:

1.  Go to IAM user sign-in.
2.  Enter either the account ID or the account alias.
3.  You will be taken to the sign-in page.
4.  The IAM user name is Stephane.
5.  The password is the one you set before.
6.  Sign in.

Now, if you look at the top right-hand side, you're logged in using your IAM user. It says the account ID and the IAM user.

If you look on the top right-hand side of the original window, it just says the account ID, which shows you it's the root account.

You now have the root account logged in on the left-hand side through a normal window and the IAM user logged in on the right-hand side through a private window.

⚠️ **Warning:** Please make sure not to lose your root account logins and your admin login. Otherwise, you will be in deep trouble with your account and you'll have to contact AWS for support.

From a course perspective, it is recommended to use your IAM user and not your root user. Sometimes you'll see the instructor using root, sometimes using the IAM user. When you have to use root or when you have to use an IAM user, you will be notified in the course.

For the rest of this section, please keep these two windows open.

---

## 2.1. Multi-Session Support in AWS Console

AWS recently introduced **multi-session support** — a game-changer for anyone managing multiple accounts.

When you enable this feature, you can:

* Turn it on directly from the browser.
* Add new sessions and log in with any of your account IDs or root accounts.
* Run multiple AWS sessions side by side in the same browser. ✅

📌 **Example:**

* In one session, you log in with **Account A** and create an EBS volume in EC2.
* In another session (same browser), you log in with **Account B**.
* The actions in Account A (like the EBS volume) won’t appear in Account B’s session.

This means you can now seamlessly switch between accounts without juggling multiple browsers or constantly signing in and out.

💡 **Tip:** This is especially useful for developers, admins, or organizations managing AWS at scale.

For long-time AWS users, this is a small but revolutionary improvement that makes daily workflows much smoother.

For different sessions, AWS create the url in the following format: `https://<account_id>-<8_chars_random_string>.<region>.console.aws.amazon.com`. Example:- https://123456789-phrglbsj.eu-north-1.console.aws.amazon.com/console/home?region=eu-north-1

Normally (without multi-session support), the url used to be in the following format: `https://<region>.console.aws.amazon.com`. Example:- https://eu-north-1.console.aws.amazon.com/console/home#?region=eu-north-1

---

## 3. IAM Policies In Depth

Let's explore IAM policies and how they apply to users and groups.

### IAM Policy Inheritance

![iam-policy-inheritance](./img/IAM_Policy_Inheritance.png)

- Imagine a group of developers: Alice, Bob, and Charles.
- If you attach a policy at the group level, that policy applies to every member.
- Therefore, Alice, Bob, and Charles all inherit the policy and gain the specified access.

Now consider a second group: Operations, with members David and Edward.

- This group has a different policy.
- David and Edward will have a different policy than Alice, Bob, and Charles.

Individual users can also have policies.

- Fred is a user who doesn't belong to any group.
- You can create an **inline policy** 📝, which is a policy attached directly to a user.
- This allows you to grant specific permissions to individual users, regardless of group membership.

Multiple group memberships can lead to combined policies.

- Charles and David both belong to the Audit team.
- A policy attached to the Audit team will also be inherited by Charles and David.
- Charles inherits policies from both the Developers group and the Audit team.
- David inherits policies from both the Operations group and the Audit team.

This inheritance model will become clearer with hands-on experience.

### Policy Structure

You should understand the high-level structure and naming conventions of IAM policies. This is a common pattern in AWS, so familiarity is key. IAM policies are structured as JSON documents.

```json
{
  "Version": "2012-10-17",
  "Id": "ExamplePolicy",
  "Statement": [
    {
      "Sid": "AllowS3Access",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::123456789012:root"
      },
      "Action": ["s3:ListAllMyBuckets", "s3:ListBucket"],
      "Resource": "arn:aws:s3:::example-bucket"
    }
  ]
}
```

An IAM policy structure consists of:

- **Version:** Specifies the policy language version. Usually `2012-10-17`.
- **ID:** An optional identifier for the policy.
- **Statement(s):** One or more statements defining the policy's permissions.

Each statement contains the following elements:

- **Sid (Statement ID):** An optional identifier for the statement.
- **Effect:** Determines whether the statement `Allow` or `Deny` access.
- **Principal:** Specifies the AWS accounts, users, or roles to which the policy applies. 📌 **Example:** The root account of your AWS account.
- **Action:** A list of API calls that are allowed or denied based on the `Effect`.
- **Resource:** A list of resources to which the actions apply. 📌 **Example:** An S3 bucket.

- **Condition (Optional):** Specifies when the statement should be applied.

For the exam, focus on understanding `Effect`, `Principal`, `Action`, and `Resource`. You'll gain more familiarity with these concepts throughout the course.

---

## 4. IAM Policies Hands On

Let's explore IAM policies in detail.

### a. Removing User Permissions

First, we'll examine user permissions.

Initially, the user "Stephane" is part of the "admin" group, granting administrator access to AWS.

If we log in as "Stephane" and navigate to the IAM console, we can see the "Stephane" user.

However, we'll remove "Stephane" from the "admin" group to revoke these permissions.

After removing the user from the group and refreshing the page, "Stephane" loses the ability to view users and receives an access denied error, specifically for `iamListUsers`.

### b. Adding User Permissions

To rectify this, we'll navigate to the IAM console, find the "Stephane" user, and add permissions.

Instead of re-adding the user to a group, we'll attach a policy directly to the user.

We'll attach the `IAMReadOnlyAccess` policy, allowing "Stephane" to read IAM resources.

After adding the `IAMReadOnlyAccess` policy and refreshing the page, "Stephane" can now view users and user groups.

However, attempting to create a new group, such as "developers", will fail because "Stephane" only has read-only access.

This demonstrates how you can restrict user permissions to specific actions.

To grant group creation access, a broader permission set like `IAMFullAccess` would be required.

Next, we'll create a new user group called "developers" and add the user "Stephane" to this group. We'll attach an arbitrary policy, such as `AlexaForBusiness`, to this group.

Then, we'll re-add "Stephane" to the "admin" group.

Now, if we examine the "Stephane" user, we'll see three permission policies:

1.  Administrator access inherited from the "admin" group.
2.  `AlexaForBusiness` managed policy attached via the "developers" group.
3.  `IAMReadOnlyAccess` attached directly.

![iam-user-permissions](./img/IAM_Permissions_Policies.png)

This illustrates how permissions are inherited and combined from different sources.

### c. Understanding IAM Policies

Now, let's delve into specific policies.

We'll start with the `AdministratorAccess` policy, which grants full administrator access to all AWS services.

Examining the policy summary reveals that it allows access to all services, such as App Mesh, Alexa for Business, and Amplify.

The JSON representation of this policy shows:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "*",
      "Resource": "*"
    }
  ]
}
```

Here, `Action: "*"` and `Resource: "*"` mean any action on any resource, effectively granting administrator access.

Next, we'll examine the `IAMReadOnlyAccess` policy.

This policy authorizes full list and limited read access to IAM resources.

The JSON representation of this policy shows:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "iam:GenerateCredentialReport",
        "iam:GenerateServiceLastAccessedDetails",
        "iam:Get*",
        "iam:List*",
        "iam:SimulateCustomPolicy",
        "iam:SimulatePrincipalPolicy"
      ],
      "Resource": "*"
    }
  ]
}
```

Here, `Get*` allows any API call starting with "Get" (e.g., `GetUser`, `GetGroup`), and `List*` allows any API call starting with "List" (e.g., `ListUsers`, `ListGroups`).

The `Resource: "*"` indicates that these actions are allowed on all resources.

### d. Creating IAM Policies

You can also create custom policies. AWS provides both a visual editor and a JSON editor for policy creation.

Using the visual editor, you can select a service (e.g., IAM) and specify the actions you want to authorize (e.g., `ListUsers`, `GetUser`). You can also define the resources these actions apply to (all resources or specific resources).

📌 **Example:** Creating a policy allowing `iam:ListUsers` and `iam:GetUser` on all resources.

After defining the policy, you can review it and give it a name (e.g., "MyIAMPermissions").

The corresponding JSON for this policy would be:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "VisualEditor0",
      "Effect": "Allow",
      "Action": ["iam:ListUsers", "iam:GetUser"],
      "Resource": "*"
    }
  ]
}
```

This policy can then be attached to groups or users.

Finally, to clean up, we'll delete the "developers" group and remove the `IAMReadOnlyAccess` policy that was directly attached to the "Stephane" user.

Now, "Stephane" only belongs to the "admin" group and has administrator access.

Verifying this by logging in as "Stephane" and accessing the IAM console confirms that everything is working correctly.

---

## 5. Protecting Users and Groups: Password Policies and MFA

Now that we've created users and groups, it's crucial to protect them from being compromised. We'll explore two defense mechanisms: password policies and Multi-Factor Authentication (MFA).

### a. Password Policies

A strong password policy enhances account security. In AWS, you can configure password policies with the following options:

- **Minimum Password Length:** Set the minimum number of characters required for a password.
- **Character Type Requirements:** Require specific character types, such as:
  - Uppercase letters
  - Lowercase letters
  - Numbers
  - Non-alphanumeric characters (e.g., question marks, symbols)
- **IAM User Password Changes:**
  - Allow or disallow IAM users to change their own passwords.
  - Require users to change their passwords after a specified period (e.g., every 90 days).
- **Prevent Password Reuse:** Prevent users from reusing previous passwords.

💡 **Tip:** A well-defined password policy is a strong defense against brute-force attacks.

![password-policies](./img/Password_Policy.png)

### b. Multi-Factor Authentication (MFA)

MFA is a crucial security measure, especially for the root account and IAM users. It adds an extra layer of protection beyond just a password.

MFA combines:

- Something you know (your password)
- Something you own (a security device)

This combination provides significantly greater security than a password alone.

📌 **Example:** Alice knows her password and has an MFA generating token. To log in successfully, she needs both.

Even if Alice's password is stolen or hacked, the account remains secure because the attacker would also need physical access to Alice's MFA device (e.g., her phone).

#### MFA Device Options in AWS

AWS offers several MFA device options:

![mfa-devices](./img/Virtual%20MFA%20Device.png)

1.  **Virtual MFA Device:** 📱
    - Uses apps like Google Authenticator (works on one phone at a time) or Authy.
    - Authy supports multiple tokens on a single device, allowing you to manage multiple accounts (root, IAM users, etc.) easily.
2.  **Universal 2nd Factor (U2F) Security Key:** 🔑 (AWS now calls this a **FIDO security key or passkey**; passkeys are supported since Jun 2024)
    - A physical device, such as a YubiKey by Yubico (a third-party provider).
    - Easy to carry on a key fob.
    - Supports multiple root and IAM users with a single key.
3.  **Hardware Key Fob MFA Device:**
    - Provided by third-party vendors like Gemalto.
4.  **AWS GovCloud Key Fob:**
    - A special key fob provided by SurePassID (a third-party) specifically for AWS GovCloud users.

![mfa-devices](./img/Hardware%20Key%20Fob%20MFA%20Device.png)

📝 **Note:** Yubico, Gemalto, and SurePassID are all third-party providers, not AWS services.

That concludes the theory on protecting your account.

---

## 6. Securing Your AWS Account: Password Policies and Multi-Factor Authentication (MFA)

This section covers two crucial aspects of securing your AWS account: setting up a strong password policy and enabling multi-factor authentication (MFA) for your root account.

### Setting Up a Password Policy 🔐

A robust password policy is the first line of defense against unauthorized access. Here's how to configure it:

1.  Navigate to **Account Settings** in the IAM console.
2.  Select **Password Policy** and click **Edit**.
3.  You have two options:

    - Use the **IAM default password policy**. This policy enforces a set of basic requirements.
    - Customize the password policy to meet your specific needs.

4.  Customization options include:

    - Forcing a **minimum password length**.
    - Requiring **uppercase letters**.
    - Requiring **lowercase letters**.
    - Requiring **numbers**.
    - Requiring **non-alphanumeric characters**.
    - Enabling **password expiration** (e.g., expire after 90 days).
    - Requiring **administrative resets** after expiration.
    - Allowing users to **change their own passwords**.
    - Preventing **password reuse**.

5.  📝 **Note:** Password policies are edited directly within the IAM console.

### Enabling Multi-Factor Authentication (MFA) for the Root Account 📱

MFA adds an extra layer of security to your root account, which is the most privileged account in your AWS environment.

1.  Click on your **account name** and then **Security Credentials**. Make sure you are logged in as the root user.
2.  Locate the section for **MFA**.
3.  ⚠️ **Warning:** Losing access to your MFA device can lock you out of your account. If you're concerned about losing your device, consider the risks carefully before proceeding. You can delete the MFA device after activating it.
    - 📝 **Note (2026):** MFA on the root user is no longer optional. Since Jun 2025 AWS enforces root-user MFA for all account types. To avoid lock-out, register a second MFA device (you can have up to eight).
4.  Click on **Assign MFA device**.
5.  Enter a **name** for your MFA device (e.g., "My iPhone").
6.  Select the **type of MFA device**:

    - **Authenticator App** (recommended): Uses a virtual app on your smartphone or computer.
    - Security Key
    - Hardware TOTP Token

7.  For **Authenticator App**:

    - Choose a compatible application. 📌 **Example:** Twilio Authy, Google Authenticator, Microsoft Authenticator.
    - Launch the authenticator app on your phone.
    - Click on **Show QR Code** in the AWS console.
    - Scan the QR code with your authenticator app.
    - The app will add the account and start generating time-based codes.

8.  Enter **two consecutive MFA codes** generated by your app into the AWS console. AWS requires two codes to ensure the device is set up correctly.
9.  Click **Add MFA**.
10. You can have up to eight MFA devices.
11. You can remove or manage your MFA devices from the security credentials section.

### Using MFA to Log In 🔑

1.  Log out of the AWS Management Console.
2.  Log back in using your root account credentials (email address and password).
3.  After successful authentication, you will be prompted for an **MFA code**.
4.  Open your authenticator app and enter the current code.
5.  Click **Submit**.
6.  You are now logged in with MFA enabled, providing an extra layer of security.

By implementing a strong password policy and enabling MFA, you significantly enhance the security of your AWS account.

---

## 7. Accessing AWS: Management Console, CLI, and SDK

There are three main ways to access AWS:

1.  Management Console
2.  Command Line Interface (CLI)
3.  Software Development Kit (SDK)

Let's explore each of these options in detail.

### Management Console

- This is the web interface we've been using so far.
- It's protected by your username, password, and potentially multi-factor authentication.

### Command Line Interface (CLI)

- The CLI is a tool that allows you to interact with AWS services using commands from your command-line shell.
- You set it up on your computer.
- It's protected by **access keys**. 🔑
- Access keys are credentials that you download and use to authenticate your CLI requests.

  📌 **Example:**

  ```bash
  aws s3 cp my_file.txt s3://my-bucket/
  ```

  This command copies `my_file.txt` to an S3 bucket named `my-bucket`.

- The AWS CLI is open-source and available on GitHub.
- Some users prefer using the CLI exclusively over the Management Console.

### Software Development Kit (SDK)

- SDK stands for Software Development Kit.
- It's a set of language-specific libraries.
- It allows you to access and manage AWS services and APIs programmatically from within your application code.
- Your application embeds the AWS SDK.
- Supports various programming languages like JavaScript, Python, PHP, .NET, Ruby, Java, Go, Node.js, C++, and more.
- There are also mobile SDKs for Android and iOS, and IoT device SDKs.

  📌 **Example:** The AWS CLI is built on the AWS SDK for Python, called `Boto`.

### Generating and Managing Access Keys

- You generate access keys through the Management Console.
- Users are responsible for managing their own access keys.
- Access keys are secret, like passwords.
- Do not share your access keys with colleagues. They can generate their own.

  ⚠️ **Warning:** Treat your access key ID like your username and your secret access key like your password. Do not share them!

- When you create access keys in the Management Console, you can download them immediately.

  📌 **Example:**
  A fake access key ID might look like: `AKIAIOSFODNN7EXAMPLE`
  A fake secret access key might look like: `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY`

  📝 **Note:** These are examples only. Never use these keys in a real environment.

- Loading these keys into your CLI allows you to access the AWS API.

### What is a CLI?

- CLI stands for Command Line Interface.
- The AWS CLI allows you to interact with AWS services using commands from your command-line shell.
- It provides direct access to the public APIs of AWS services.
- You can develop scripts to manage resources and automate tasks.

### Key Takeaways

- 💡 **Tip:** Understand the different ways to access AWS (Management Console, CLI, SDK).
- ⚠️ **Warning:** Protect your access keys. They are private credentials.
- 📝 **Note:** The CLI and SDK provide programmatic access to AWS, while the Management Console offers a graphical interface.

---

## 8. Installing the AWS CLI on Windows

This guide walks you through installing the AWS Command Line Interface (CLI) on a Windows machine.

1.  **Find the Installer:**

    - Search Google for `aws CLI install windows`.
    - Locate the official AWS documentation link for installing the AWS CLI version 2 on Windows. This version offers improved performance and capabilities compared to version 1, while maintaining API compatibility. The installer is also improved.

2.  **Download the MSI Installer:**

    - Scroll down to the "Install on Windows" section.
    - Click the link to download the MSI installer.

3.  **Run the Installer:**

    - Locate the downloaded MSI file and run it.
    - Follow the on-screen instructions:
      - Click "Next".
      - Accept the terms of the license agreement.
      - Click "Next".
      - Click "Install".
      - Grant the installer permission to make changes to your device (if prompted).
    - Wait for the installation to complete.

4.  **Verify the Installation:**

    - Once the installer finishes, click "Finish".
    - Open the Command Prompt (search for "Command Prompt" in the Windows search bar).
    - Type the following command and press Enter:

      ```bash
      aws --version
      ```

    - If the AWS CLI is installed correctly, you should see output similar to the following:

      ```
      aws-cli/2.x.x Python/3.x.x Windows/10 botocore/1.x.x
      ```

    - ✅ The key is to ensure the `aws-cli` version starts with a `2`. This confirms you've installed version 2.

5.  **Upgrade the AWS CLI:**
    - 📝 **Note:** To upgrade your AWS CLI installation in the future, simply re-download the latest MSI installer and re-run it. The installer will automatically upgrade your existing installation.

With the AWS CLI successfully installed, you are now ready to interact with AWS services from your command line. 🎉

---

## 9. Installing the AWS CLI on macOS

Here's how to install the AWS CLI version 2 on macOS.

1.  Go to Google and search for "installing the AWS CLI version 2 on macOS". Choose the official AWS documentation link.

2.  Download the `.pkg` file. This is a graphical installer.

3.  Run the installer:

    - Double-click the downloaded `.pkg` file.
    - Click "Continue" several times.
    - Agree to the license terms.
    - Select "Install for all users on this computer".
    - Click "Continue" and then "Install".
    - Wait for the installation to complete.

4.  Once the installation is successful, you can move the installer to the trash.

5.  Verify the installation:

    - Open a terminal. You can use the built-in "Terminal" application or a third-party terminal like iTerm2.
    - Type the following command:

    ```bash
    aws --version
    ```

    - If the installation was successful, you should see the version of the AWS CLI printed in the terminal. 📌 **Example:** `aws-cli/2.0.10`

6.  🎉 Congratulations! The AWS CLI is now installed.

In case of issues, please refer to the official AWS documentation guide. It should provide solutions to common problems.

---

## 10. Installing the AWS CLI on Linux

Here's how to install the AWS CLI Version 2 on Linux:

1.  Download the AWS CLI installer. 📦
    - Search on Google for "installing the AWS CLI Version 2 on Linux".
    - Navigate to the official AWS documentation page.
    - Scroll down to the installation instructions.
2.  Download the ZIP file using `curl`.
    ```bash
    curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
    ```
3.  Unzip the downloaded file. 🗜️
    ```bash
    unzip awscliv2.zip
    ```
4.  Run the installer as root using `sudo`. 🔑
    ```bash
    sudo ./aws/install
    ```
    - You will be prompted for your password. Enter it to proceed.
5.  Verify the installation. ✅
    - Run the following command to check the AWS CLI version:
      ```bash
      aws --version
      ```
    - Alternatively, if `/usr/local/bin` is in your `$PATH`, you can run:
      ```bash
      /usr/local/bin/aws --version
      ```
    - You should see output similar to:
      ```
      aws-cli/2.<version> Python<version> Linux/<version> botocore/<version>
      ```
      where `<version>` represents the specific version numbers.
6.  Troubleshooting. 🛠️
    - If you encounter any issues, refer to the official AWS documentation for troubleshooting steps.
7.  You are now ready to use the AWS CLI! 🎉
    - You can now proceed with other lectures that utilize the AWS CLI.

📝 **Note:** The specific version numbers displayed will vary depending on when you perform the installation.

---

## 11. Creating Access Keys

Here's how to create and configure access keys for the AWS CLI.

1.  Click on your username.
2.  Go to **Security credentials**.
3.  Scroll down and create an access key.

AWS provides recommendations based on your intended use case. 📌 **Example:** If you want access keys for the CLI, AWS suggests using CloudShell or the CLI V2 with authentication through the IAM Identity Center.

Consider the recommendations based on whether you're running:

- Local code applications outside of AWS
- Applications within AWS

For this example, **we'll use the CLI with access keys**.

1.  Acknowledge the recommendation by checking the box: "I understand the above recommendation."
2.  Click to create the access key.

⚠️ **Warning:** This is the _only_ time you'll have access to the access key and the secret access key. Store them securely!

To configure the AWS CLI:

1.  Open your terminal.
2.  Type `aws configure` and press Enter.

    ```bash
    aws configure
    ```

3.  Enter your access key ID when prompted.
4.  Enter your secret access key when prompted.
5.  Enter the default region name. 💡 **Tip:** Choose a region close to you. 📌 **Example:** `eu-west-1`. You can find the region name and code in the dropdown menu in the AWS Management Console.
6.  Enter the default output format, or just press Enter to accept the default.

Now your AWS CLI is configured. To test it, you can run a command like:

```bash
aws iam list-users
```

This will list all the IAM users in your account. The output will include information like:

- UserId
- ARN
- Creation date
- Last password use

This information is similar to what you'd find in the AWS Management Console.

### Understanding the Configuration Files

When you run `aws configure` on **Windows**, the AWS CLI stores your configuration and credentials in files located in your user directory. Specifically:

1. **Configuration files location**

```plaintext
C:\Users\<YourUsername>\.aws\
```

This directory contains:

- **`config`** – Stores configuration settings such as default region and output format.
- **`credentials`** – Stores your AWS access key ID and secret access key.

Example:

```plaintext
C:\Users\Vks\.aws\config
C:\Users\Vks\.aws\credentials
```

2. **Contents of the files**

`config`

```ini
[default]
region = us-east-1
output = json
```

`credentials`

```ini
[default]
aws_access_key_id = YOUR_ACCESS_KEY
aws_secret_access_key = YOUR_SECRET_KEY
```

3. **Additional Notes**

- You can use **named profiles** by adding more `[profile profile-name]` sections in `config`, and `[profile-name]` sections in `credentials`.
- Environment variables like `AWS_CONFIG_FILE` and `AWS_SHARED_CREDENTIALS_FILE` can override the default location.

### Managing Permissions

Let's examine what happens when a user's permissions are revoked.

1.  Remove the user (📌 **Example:** Stephane) from a group (📌 **Example:** admins).
2.  Attempt to perform an action in the Management Console. You should receive an error indicating insufficient permissions.
3.  Try the same action using the CLI (📌 **Example:** `aws iam list-users`). You will receive a "denied" response.

This demonstrates that CLI permissions mirror those in the IAM console.

The key takeaway is that you can access AWS using either:

- The Management Console
- Access keys and secret access keys configured for use with the CLI

⚠️ **Warning:** Do not forget to add your user back to the appropriate group to restore their permissions!

---

## 12. AWS CloudShell

**Region Availability**

We're going to use demo AWS CloudShell. It is not yet available in all regions, and you can find the region list [here](https://docs.aws.amazon.com/cloudshell/latest/userguide/supported-aws-regions.html)

Please switch to one of these regions if you want to do the next (optional) hands-on.

**Using AWS CloudShell**

Cloud Shell provides an alternative to using the terminal for issuing commands against AWS. It's accessible via an icon in the top right corner of your AWS console.

⚠️ **Warning:** Cloud Shell is not available in all AWS regions. Check the [AWS documentation](https://aws.amazon.com/cloudshell/faqs/) for the most up-to-date list of supported regions. If you don't see the icon, ensure you're in a supported region.

If the terminal is fine for you, there is no need to use Cloud Shell.

```bash
aws iam list-users
```

### Accessing and Using Cloud Shell

- Click the Cloud Shell icon to launch the environment. It may take a minute to initialize.
- You can then issue AWS commands directly.

📌 **Example:**

```bash
aws --version
```

This shows the AWS CLI version installed in Cloud Shell.

Cloud Shell is essentially a terminal in the cloud, provided by AWS and free to use.

### Credentials and Region

When using the AWS CLI within Cloud Shell, the credentials used are those of the AWS account you're currently logged into. This is why API calls work seamlessly.

The default region for API calls in Cloud Shell is the region you're currently logged into. You can still specify a different region using the `--region` argument in your AWS CLI commands.

### File Storage

Cloud Shell provides a persistent storage repository.

📌 **Example:**

```bash
echo "tests" > demo.txt
ls
```

This creates a text file named `demo.txt` containing the word "tests".

📝 **Note:** Files created within your Cloud Shell environment persist even if you restart Cloud Shell.

### Customization

You can configure Cloud Shell to your preferences:

- Font size (Small, Medium, Large)
- Theme (Light, Dark)
- Safe Paste

### Uploading and Downloading Files

Cloud Shell allows you to upload and download files.

1.  To download a file, use the "Actions" menu and select "Download file".
2.  Enter the full path to the file you want to download (use `pwd`).

📌 **Example:** If your file is named `demo.txt` in the home directory, you would enter `demo.txt`.

To upload files, use the "Actions" menu and select "Upload file".

### Multiple Tabs

You can open multiple tabs within Cloud Shell for parallel tasks. You can also split the terminal into columns.

### Key Takeaways

- Cloud Shell is a convenient alternative to a local terminal for interacting with AWS.
- It's pre-configured with the AWS CLI and uses your AWS account credentials.
- It offers persistent storage and customization options.
- It supports uploading and downloading files.

💡 **Tip:** The upload and download features are particularly useful for managing files within your AWS environment.

You can use either Cloud Shell or your configured terminal to perform the commands in this course.

---

## 13. IAM Roles for Services

IAM Roles are a crucial component of AWS Identity and Access Management (IAM). They allow AWS services to perform actions on your behalf within your AWS account. Let's break down what they are and why they're important.

AWS services sometimes need to interact with other AWS services or resources. To grant them the necessary permissions, we use IAM Roles. Think of IAM Roles as identities for AWS services, similar to how IAM Users are identities for people.

Here's a breakdown:

- 🔑 IAM Roles are assigned to AWS services, not individual users.
- 🛡️ They grant permissions to those services, allowing them to perform specific actions.
- 🤝 This enables services to interact with other AWS resources securely.

📌 **Example:**

Let's say you're launching an EC2 instance (a virtual server). This EC2 instance might need to access data from an S3 bucket (object storage). To allow the EC2 instance to do this, you would:

1.  Create an IAM Role.
2.  Grant the IAM Role the necessary permissions to access the S3 bucket.
3.  Assign the IAM Role to the EC2 instance.

Now, when the EC2 instance tries to access the S3 bucket, it will use the permissions defined in the IAM Role.

The process looks like this:

EC2 Instance ➡️ IAM Role ➡️ AWS Permissions ➡️ Access to AWS Resources

Common use cases for IAM Roles include:

- 💻 EC2 Instance Roles: Allow EC2 instances to interact with other AWS services.
- ⚙️ Lambda Function Roles: Grant Lambda functions permissions to access resources.
- ☁️ CloudFormation Roles: Enable CloudFormation to create and manage AWS resources.

📝 **Note:** We'll be creating an IAM Role in the next lecture, but we won't be using it immediately. We'll put it to use in a later section.

---

## 14. Creating Roles in AWS

Let's practice creating roles in AWS. Roles are a fundamental way to grant permissions to AWS entities, allowing them to perform actions within your AWS environment.

1.  Access the Roles section in the IAM console. You may see some roles already created for your account. The number of pre-existing roles doesn't matter. We'll create our own.

2.  Understand Role Types: There are different types of roles you can create. Currently, you can create five types. For this hands-on exercise and for the exam, the most important type is a role for an AWS service.

3.  Choose the AWS Service Role Type: Select the option to create a role for an AWS service.

4.  Select the Service: Choose the specific AWS service for which this role will apply.

    - You'll see a list of commonly used services like EC2 and Lambda, as well as nearly every service on AWS.
    - 📌 **Example:** We will create a role for an EC2 instance.

5.  Select the EC2 Use Case: Choose EC2 as the use case. Disregard any other options presented.

6.  Attach a Policy: Now that you've created a role for an EC2 instance, you need to attach a policy that defines the permissions granted by the role.

    - 📌 **Example:** Attach the `IAMReadOnlyAccess` policy to allow the EC2 instance to read information from IAM.

7.  Configure the Role:

    - Enter a role name.
      - 📌 **Example:** `DemoRoleForEC2`
    - Review the trusted entities. This confirms that the role can be assumed by the EC2 service, defining it as a role for Amazon EC2.
    - Verify the permissions.
      - In our example, confirm that the role has `IAMReadOnlyAccess`.

8.  Create the Role: Click the button to create the role.

9.  Verify the Role: The newly created role will appear in your list of roles. You can verify that the permissions are correctly assigned.

10. Using the Role: 📝 **Note:** We cannot use this role just yet. We will use it later in the EC2 section.

In summary, you've learned how to create a role for Amazon EC2 and attach the appropriate permissions to it. 🎉

---

## 15. IAM Security Tools

Let's explore the security tools available within IAM to enhance your AWS security posture. We'll cover two key tools: IAM Credentials Report and Last Access.

### IAM Credentials Report (account-level) 📊

The IAM Credentials Report is a valuable resource for auditing and managing user credentials across your AWS account.

- This report operates at the **account-level**.
- It provides a comprehensive overview of **all users** within your account.
- The report details the **status of various credentials** for each user. This includes information about passwords, access keys, and MFA devices.
- We will generate and examine a sample report to understand its structure and content.

### Last Access (user-level) 🕵️‍♀️

The Last Access helps you refine user permissions based on actual usage, aligning with the principle of least privilege.

- This tool functions at the **user-level**.
- It displays the **service permissions granted to a user**.
- Crucially, it shows **when those services were last accessed**. This is key to identifying unused permissions.
- By leveraging this information, you can **reduce user permissions** to only those services they actively use. This minimizes the potential attack surface and strengthens your security.

  💡 **Tip:** Regularly review the Last Access to identify and remove unnecessary permissions.

  📌 **Example:** If a user has `S3:ListBucket` permission but hasn't accessed S3 in the last 90 days, consider removing that permission.

We will explore how to use these security tools in the next section.

---

## 16. Generating Credentials Report and Using Last Access

To enhance your AWS security posture, let's explore generating credentials reports and using Last Access.

### Generating a Credentials Report 📝

A credentials report provides a comprehensive overview of your AWS account's users and their security credentials. Here's how to generate one:

1.  Navigate to the "Credential report" section in the IAM console.
2.  Click on "Download credential report". This will create a CSV file. ⬇️

The CSV file contains valuable information, including:

- User creation date
- Password status (enabled/disabled)
- Password last used and last changed dates
- Password rotation details
- MFA activation status
- Access key status (created/not created)
- Access key last rotated and last used dates
- Information about other access keys and certificates

This report is extremely helpful for identifying users who:

- Haven't changed their passwords recently.
- Aren't actively using their accounts.
- May pose a security risk. ⚠️

### Last Access 💡

Last Access helps you understand which services your users are accessing and when. This allows you to fine-tune their permissions and adhere to the principle of least privilege.

1.  Go to the specific IAM user in the IAM console.
2.  Click on "Last Accessed". ➡️

Access Advisor displays a list of AWS services accessed by the user, along with the last access time.

- It also shows services that have _not_ been accessed.
- You can drill down to see the specific permissions granted for each service.

📌 **Example:** If a user has administrator access to Amazon EC2, Access Advisor will indicate this.

Using this information, you can determine if the user has the correct permissions or if their access should be restricted. If a user only needs access to a few services, you can revoke unnecessary permissions.

Access Advisor is particularly useful for:

- Implementing granular user access permissions.
- Ensuring users only have the necessary permissions.
- Reducing the attack surface of your AWS environment.

---

## 17. IAM Guidelines and Best Practices

Here are some general guidelines and best practices for Identity and Access Management (IAM) to help you avoid common mistakes when using AWS.

- 🔑 **Root Account Security:**

  - Do not use the root account except when initially setting up your AWS account.
  - You should have two accounts: the root account and your personal account.

- 👤 **User Management:**

  - One AWS user should correspond to one physical user (One physical user = One AWS user).
  - If someone else needs access to AWS, create a separate user for them instead of sharing your credentials.

- 👥 **Group Permissions:**

  - Assign users to groups.
  - Assign permissions to groups to manage security at the group level.
  - Create a strong password policy.

- 🛡️ **Multi-Factor Authentication (MFA):**

  - Enforce the use of multi-factor authentication (MFA) to enhance account security and protect against unauthorized access.

- ⚙️ **Roles for AWS Services:**

  - Create and use roles when granting permissions to AWS services, including EC2 instances (virtual servers).

- 💻 **Programmatic Access (CLI/SDK):**

  - If using AWS programmatically via the CLI or an SDK, generate access keys.
  - ⚠️ **Warning:** Treat access keys like passwords – they are highly sensitive and must be kept secret.

- 🔍 **Permission Auditing:**

  - Use IAM credentials reports or the Last Access feature to review and adjust account permissions.

- 🔒 **Credential Protection:**
  - ⚠️ **Warning:** Never, ever, ever share your IAM users and access keys. This is a critical security practice.

That concludes this section on IAM. You should now have a solid understanding of IAM principles.

---

## 18. IAM Summary

Here's a recap of Identity and Access Management (IAM) in AWS:

- IAM users represent actual physical users within your company. Each user has a password for accessing the AWS console.
- Users can be organized into groups for easier permission management. 📝 **Note:** Groups contain only users, not other groups.
- Policies are JSON documents that define permissions. These policies can be attached to users or groups.

Roles:

- Roles are identities assumed by AWS services like EC2 instances or other AWS services.
- They grant permissions to these services to perform actions on your behalf.

Security Best Practices:

- Enable Multi-Factor Authentication (MFA) for enhanced security. 🔑
- Implement a strong password policy for your users.

Accessing AWS:

- AWS Command Line Interface (CLI): Use the CLI to manage AWS services from the command line.
- AWS Software Development Kit (SDK): Use the SDK to manage AWS services using a programming language.
- Access Keys: Create access keys to access AWS via the CLI or SDK. ⌨️ **Shortcut:** Store access keys securely and rotate them regularly.

Auditing IAM:

- IAM Credentials Report: Generate a report to audit IAM usage.
- Last Access: Use the Access Advisor service to review and refine IAM permissions. 💡 **Tip:** Regularly review and refine IAM permissions to adhere to the principle of least privilege.

---

## Q & A

An IAM policy consists of one or more statements.
A statement in an IAM Policy consists of the following, **EXCEPT:**

### **Options:**

* 🅐 Effect
* 🅑 Principal
* 🅒 Version
* 🅓 Action
* 🅔 Resource

<details>

<summary>Explanation</summary>

* An **IAM Policy** is a JSON document that defines **permissions**.
* Each **policy statement** can contain the following elements:

  * **Sid** (optional) – statement ID
  * **Effect** – whether to **allow** or **deny** the action
  * **Principal** – the account, user, role, or service receiving the permission
  * **Action** – the operation being allowed or denied
  * **Resource** – the AWS resource(s) the action applies to
  * **Condition** (optional) – specifies when the policy is in effect

⚠️ **Version** is **not part of a statement**.

* Instead, **Version** is at the **top level of the IAM policy document**.
* It defines the **policy language version** (e.g., `"Version": "2012-10-17"`).
* This is important because AWS occasionally updates policy language features.

✅ **Summary:**

* Statements = Sid, Effect, Principal, Action, Resource, Condition
* **Version belongs to the policy itself, not inside the statement.**

**Correct Answer:** ✅ *(Correct)* **Version**

</details>

---