# 26 AWS Security & Encryption KMS, SSM Parameter Store, Shield, WAF

Sections:-
- [1. AWS Security - Section Introduction](#1-aws-security---section-introduction)
- [2. Encryption Mechanisms in the Cloud](#2-encryption-mechanisms-in-the-cloud)
- [3. AWS KMS: Key Management Service 🔑](#3-aws-kms-key-management-service-)
- [4. KMS Service Hands On with AWS CLI](#4-kms-service-hands-on-with-aws-cli)
- [5. KMS Multi-Region Keys](#5-kms-multi-region-keys)
- [6. S3 Replication and Encrypted Objects](#6-s3-replication-and-encrypted-objects)
- [7. Sharing Encrypted AMIs Across AWS Accounts](#7-sharing-encrypted-amis-across-aws-accounts)
- [8. SSM Parameter Store](#8-ssm-parameter-store)
- [9. Systems Manager Parameter Store - Hands On](#9-systems-manager-parameter-store---hands-on)
- [10. Using Lambda with SSM Parameter Store](#10-using-lambda-with-ssm-parameter-store)
- [11. AWS Secrets Manager](#11-aws-secrets-manager)
- [12. AWS Secrets Manager - Hands On](#12-aws-secrets-manager---hands-on)
- [13. AWS Certificate Manager (ACM)](#13-aws-certificate-manager-acm)
- [14. AWS WAF (Web Application Firewall)](#14-aws-waf-web-application-firewall)
- [15. AWS Shield: Protecting Against DDoS Attacks 🛡️](#15-aws-shield-protecting-against-ddos-attacks-️)
- [16. AWS Firewall Manager](#16-aws-firewall-manager)
- [17. WAF, Shield, and Firewall Manager - Hands On](#17-waf-shield-and-firewall-manager---hands-on)
- [18. DDoS Protection and Best Practices](#18-ddos-protection-and-best-practices)
- [19. Amazon GuardDuty](#19-amazon-guardduty)
- [20. Amazon Inspector 🛡️](#20-amazon-inspector-️)
- [21. AWS Macie](#21-aws-macie)
- [22. Q & A](#22-q--a)

---

## 1. AWS Security - Section Introduction

Welcome to one of the **most important sections** of this course—**AWS Security**.

### 🔐 Why This Section Matters

- The **AWS certification exam** will ask **many questions related to security**.
- Security is foundational to **all AWS services** and **real-world architecture**.

### 📝 What You’ve Already Seen

Throughout previous lectures:
- We've discussed how different AWS services integrate with **security features**.
- You've been introduced to concepts like:
  - Identity and access management
  - Encryption
  - Secure service configurations

Now, it’s time to **go deeper**.

### 🎯 What You’ll Learn in This Section

This dedicated security section will help you:
- **Master AWS Key Management Service (KMS)**.
- Understand **encryption mechanisms** clearly.
- Learn about **AWS Systems Manager Parameter Store** and its role in managing secrets.
- Tie all your security knowledge together in a **practical, cohesive way**.

### ✅ What Makes This Section Unique

- It's **real-world oriented** — not just theory.
- You'll get **hands-on experience** with services like **AWS Lambda** in a secure context.
- This section helps reinforce your security knowledge across **multiple AWS services**.

Let’s get started and dive into the world of **AWS Security**! 🔒🚀

---

## 2. Encryption Mechanisms in the Cloud

This note provides an overview of encryption mechanisms, including encryption in flight (TLS/SSL), server-side encryption at rest, and client-side encryption.

### Encryption in Flight (TLS/SSL) ✈️

Encryption in flight, often referred to as TLS (Transport Layer Security) or SSL (Secure Sockets Layer), ensures data is encrypted while being transmitted over a network. TLS is the newer version of SSL.

*   Data is encrypted **before** being sent.
*   Data is decrypted **after** being received.
*   This is crucial for secure communication between a client and a server, especially over public networks.
*   TLS certificates are used to encrypt the data.
*   You'll see "HTTPS" in your browser, indicating that the connection is encrypted using TLS certificates.

Why is encryption in flight important?

*   It prevents "man-in-the-middle" attacks, where malicious actors intercept and observe data packets.
*   With HTTPS/TLS/SSL, only the intended target server can decrypt the encrypted data.

![Secure Login](./img/SecureLogin.png)

📌 **Example:** Secure Login

1.  A client wants to securely log in to a server with a username and password.
2.  The client-side automatically applies TLS encryption to the username and password.
3.  The encrypted data is sent over the network.
4.  Intermediate servers cannot decrypt the data.
5.  Only the target server can use TLS decryption to retrieve the username and password and authenticate the user.

### Server-Side Encryption at Rest 💾

![Server-Side Encryption at Rest](./img/ServerSideEncryptionAtRest.png)

Server-side encryption at rest focuses on encrypting data after it has been received by the server, ensuring secure storage.

*   Data is encrypted **after** being received by the server.
*   Data is decrypted **before** being sent back to clients.
*   Data is stored in an encrypted form using a **data key**.
*   Key management is crucial and must be handled securely by the server.

📌 **Example:** Amazon S3

1.  A client sends an object to Amazon S3 over HTTP/HTTPS.
2.  The service receives the object (initially decrypted).
3.  The service uses a data key to encrypt the object at rest.
4.  When the client requests the object, the service uses the data key to decrypt it.
5.  The decrypted object is sent back to the client over HTTPS.

📝 **Note:** In this scenario, the encryption and decryption processes happen on the server-side.

### Client-Side Encryption 🔒

![Client-Side Encryption](./img/ClientSideEncryption.png)

Client-side encryption involves encrypting and decrypting data on the client's side, ensuring the server never has access to the unencrypted data.

*   Data is encrypted and decrypted on the client-side.
*   The server should **never** be able to decrypt the data.
*   This is useful when you don't fully trust the server.

📌 **Example:**

1.  The client has an object and a data key (stored client-side).
2.  The client encrypts the object using the data key, resulting in an encrypted object.
3.  The encrypted object is sent to a storage service (e.g., Amazon S3, FTP server).
4.  The storage service stores the object in its encrypted form.
5.  When the client retrieves the object, it receives the encrypted object.
6.  The client uses its client-side data key to decrypt the object.

📝 **Note:** The server never has access to the data key or the decrypted content.

---

## 3. AWS KMS: Key Management Service 🔑

AWS KMS (Key Management Service) is AWS's key management solution. It handles encryption keys for various AWS services, simplifying data protection.

*   KMS is fully integrated with IAM for authorization, providing easy access control to encrypted data.
*   🔑 You can audit every API call made to use your keys through CloudTrail. This is a key point for the AWS exam.

KMS integrates seamlessly with most AWS services:

*   EBS volumes
*   S3 buckets
*   RDS databases
*   SSM Parameter Store
*   And many more!

You can also use KMS directly via API calls, AWS CLI, or SDKs to encrypt your own secrets.

*   Never store secrets in plain text, especially in code.
*   Encrypt secrets with a KMS key and store the encrypted secrets in code or environment variables. This is a much more secure pattern.

![symmetrical-vs-asymmetrical-min.webp](./img/symmetrical-vs-asymmetrical-min.webp)

### KMS Key Types

There are two main types of KMS keys:

1.  **Symmetric Keys (AES-256 Keys):**
    *   Use a single encrypted key for both encryption and decryption.
    *   Integrated AWS services use symmetric keys.
    *   You never have direct access to the key itself; you use KMS API calls to leverage the key.
2.  **Asymmetric Keys (RSA & ECC Key Pairs):**
    *   Use a public key for encryption and a private key for decryption.
    *   Used for encrypt/decrypt or sign/verify operations.
    *   You can download the public key, but you can only access the private key through API calls.
    *   📌 **Example:** Use case: Encryption done outside of AWS by users without KMS API access. They encrypt with the public key, and you decrypt within your account using the private key.

![symmetric-encryption-vs-asymmetric-encryption.jpg](./img/symmetric-encryption-vs-asymmetric-encryption.jpg)

### Types of KMS Keys within AWS

*   **AWS Owned Keys:**
    *   Free.
    *   Used with SSE-S3 or SSE-DynamoDB when you choose a key owned by the service.
    *   You don't directly see these keys.
*   **AWS Managed Keys:**
    *   Free.
    *   Start with `aws/<service-name>` (e.g., `aws/rds`, `aws/ebs`, `aws/dynamodb`).
    *   Usable only from within the service they are assigned to.
*   **Customer Managed Keys:**
    *   Custom keys that cost \$1 per month.
    *   Imported keys also cost \$1 per month.

### KMS Pricing

*   You pay for each API call made to the KMS service (approximately 3 cents per 10,000 API calls).

### Key Rotation

*   **AWS Managed Keys:** Automatic rotation every one year.
*   **Customer Managed Keys:** You can enable automatic rotation and set the period, or perform on-demand rotation.
*   **Imported KMS Keys:** No automatic rotation. You can rotate them on demand (import new key material, then rotate), or rotate manually.
*   📝 **Note:** Leverage an alias for key rotation.

### KMS Key Scope

*   KMS keys are scoped per region.
*   To copy an EBS volume encrypted with a KMS key to another region:
    1.  Take a snapshot of the EBS volume. The snapshot will also be encrypted with the same KMS key.
    2.  Copy the snapshot to another region, re-encrypting it with a different KMS key in that region.
    3.  Restore the snapshot into a new EBS volume in the target region, encrypted with the new KMS key.

![Copy EBS Volume to Different Region via KMS](./img/CopyEBSVolumeToDifferentRegionViaKMS.png)

When you **copy a snapshot across regions**, AWS does this behind the scenes:

1. **Decrypts** the snapshot data using the **data key (DEK)** that was wrapped by the **source region’s KMS key**.
2. **Generates a new DEK** in the **destination region**.
3. **Encrypts** the snapshot data with this new DEK, which is itself protected by the **destination region’s KMS key**.

So the data is never stored unencrypted — the decrypt → re-encrypt happens inside AWS’s secure systems, not visible to you. At the end, you just get a snapshot in the target region encrypted with that region’s KMS key.

⚠️ That’s why KMS keys are **region-scoped** — AWS won’t let one region’s CMK protect data in another region.

### KMS Key Policies

KMS key policies control access to your KMS keys.

*   If you don't have a KMS key policy, no one can access the key.
*   Similar to S3 bucket policies.

Types of KMS Key Policies:

1.  **Default:**
    *   Created if you don't provide a custom policy.
    *   Allows everyone in your account to access the key, provided they have an IAM policy granting access.
2.  **Custom:**
    *   You define the users and roles that can access the key.
    *   You define who can administer the key.
    *   Especially helpful for cross-account access.

### Cross-Account Access

To allow another account to use your KMS key:

1.  Create a snapshot encrypted with your own KMS Key (customer-managed key).
2.  Attach a custom KMS key policy to authorize cross-account access.
    📌 **Example:** The policy will look something like this:

```json
{
  "Sid": "Allow use of the key with destination account",
  "Effect": "Allow",
  "Principal": {
    "AWS": "arn:aws:iam::TARGET-ACCOUNT-ID:role/ROLENAME"
  },
  "Action": [
    "kms:Decrypt",
    "kms:CreateGrant"
  ],
  "Resource": "*",
  "Condition": {
    "StringEquals": {
      "kms:ViaService": "ec2.REGION.amazonaws.com",
      "kms:CallerAccount": "TARGET-ACCOUNT-ID"
    }
  }
}
```
3.  Share the encrypted snapshot with the target account.
4.  In the target account, create a copy of the snapshot and encrypt it with a different customer-managed key in that account.
5.  Create a volume from the snapshot in the target account.

### ❓ Q1: If I encrypt data today and the KMS key is rotated after 90 days, will I still be able to decrypt the data after 100 days?

✅ **Answer:** Yes, you can still decrypt it.

* KMS keeps all previous key versions (backing keys) active for decryption.
* When decrypting, KMS automatically uses the correct key version that was originally used.
* Key rotation only changes which version is used for **new encryptions**, not for old decryptions.

### ❓ Q2: What happens to old key versions after rotation?

✅ **Answer:** Old key versions are **retained by KMS** and remain usable for decryption.

* They are never automatically deleted.
* They ensure that any data encrypted in the past can always be decrypted, even after multiple rotations.

### ❓ Q3: In which case would decryption fail after rotation?

⚠️ **Answer:** Decryption will fail only if:

* The KMS key (CMK) itself is **disabled**.
* Or if it is **scheduled for deletion** and then deleted after the waiting period.

Rotation alone will **never** cause decryption failure.

---

## 4. KMS Service Hands On with AWS CLI

Let's explore the KMS (Key Management Service) service. 🔐

### AWS Managed Keys

First, let's examine AWS managed keys. If you've been using KMS encryption, these keys will appear here.

📌 **Example:** The AWS EBS key is an AWS managed key because it belongs to the EBS service.

![AWS Managed KMS Keys](./img/AWSManagedKMSKeys.png)

We can inspect how the key is being used.

*   **Key Policy:** This complex policy defines who can access the key.
*   For an EBS AWS key, the policy specifies actions that can originate from anywhere.
*   **Condition:** The caller account must be yours, and the `ViaService` must be the EC2 service (which sits above EBS).

📌 **Example:** Looking at the SNS AWS managed key:

*   The key policy's `ViaService` condition is the SNS service.
*   This allows only access from SNS to this key.

We can also view the cryptographic configuration. This shows that the key is **symmetric**, originates from KMS, and is used to encrypt and decrypt data.

### Customer Managed Keys

Besides AWS managed keys, we have customer managed keys and custom key stores.

📝 **Note:** Custom key stores involve CloudHSM, but that's outside the scope of this discussion. We'll focus on customer managed keys.

Customer managed keys are keys that you create within KMS, instead of using the ones managed by AWS.

⚠️ **Warning:** Creating a customer managed key costs $1 per month. If you don't want to incur charges, avoid creating one.

To create a key:

1.  **Key Type:** You have options like symmetric or asymmetric keys.
    *   Asymmetric keys can be used for encrypt/decrypt or sign/verify operations.
    *   For this example, we'll use a symmetric key for encrypt/decrypt.
    * Choose:-
        - Key type: **Symmetric**
        - Key usage: **Encrypt and Decrypt**
2.  **Advanced Options:**
    *   **Key Origin:** We'll use KMS, so KMS creates the key for us.
        *   External key origin is for importing keys.
        *   Custom key store is for CloudHSM (out of scope).
    *   **Regionality:** We'll use a single-region key, the most common option.
3.  **Key Alias:** Give the key an alias, like "tutorial".
4.  **Key Administrators:** You can define key administrators. If you don't, the default KMS key policy is used.
5.  **Key Users:** Specify who can use the key.
    *   You can allow everyone with the right IAM permissions to use it.
    *   You can restrict access to specific users, creating a custom KMS key policy.
    *   You can allow other AWS accounts to access your key. 📌 **Example:** This is useful for sharing encrypted EBS snapshots.
6.  **Review:** Summarize the key configuration. The default key policy enables IAM user permissions, allowing any action on any KMS resource, provided they have the necessary IAM permissions.

After creation, you can view the key.

*   **Key Policy:** It's an IAM policy for your key. You can switch to the default view for a better summary of key administrators, key deletion permissions, key users, and cross-account access.
*   **Cryptographic Configuration:** Review the cryptographic settings.
*   **Key Rotation:** You can enable automatic key rotation.
    *   Set the rotation period from 90 to 2,560 days.
    *   You can also initiate on-demand key rotation.
    *   Key rotation history will show all rotations.
*   **Alias:** The alias for the key is "tutorial," so you can refer to it using the alias ARN.
*   **Key Actions:** You can disable the key or schedule key deletion.

### Commands for Encryption and Decryption

```bash
# 1) encryption
aws kms encrypt --key-id alias/tutorial --plaintext fileb://ExampleSecretFile.txt --output text --query CiphertextBlob  --region eu-west-2 > ExampleSecretFileEncrypted.base64

# base64 decode for Linux or Mac OS 
cat ExampleSecretFileEncrypted.base64 | base64 --decode > ExampleSecretFileEncrypted

# base64 decode for Windows
certutil -decode .\ExampleSecretFileEncrypted.base64 .\ExampleSecretFileEncrypted

# 2) decryption
aws kms decrypt --ciphertext-blob fileb://ExampleSecretFileEncrypted   --output text --query Plaintext > ExampleFileDecrypted.base64  --region eu-west-2

# base64 decode for Linux or Mac OS 
cat ExampleFileDecrypted.base64 | base64 --decode > ExampleFileDecrypted.txt

# base64 decode for Windows
certutil -decode .\ExampleFileDecrypted.base64 .\ExampleFileDecrypted.txt
```

### Using the CLI for Encryption and Decryption

Now, let's use the CLI to encrypt and decrypt data.

We'll use the `kms-demo-cli.sh` script to demonstrate the `encrypt` and `decrypt` calls of KMS.

```bash
mkdir -p c/workspace/AWS_SAA-C03/scripts/playground
cd c/workspace/AWS_SAA-C03/scripts/playground
```

1.  **Create a File:** Create a file called `ExampleSecretFile.txt`.
    ```bash
    echo "SuperSecretPassword" > ExampleSecretFile.txt
    ```
    This file contains the secret you want to encrypt.
2.  **KMS Encryption:** Use the `encrypt` command.
    ```bash
    aws kms encrypt \
        --key-id alias/tutorial \
        --plaintext fileb://ExampleSecretFile.txt \
        --output text \
        --query CiphertextBlob \
        --region us-east-1 \
        > ExampleSecretFileEncrypted.base64
    ```
    *   `--key-id`: Specify the key ID (alias, key ID, or full ARN).
    *   `--plaintext`: Pass the address of your file.
    *   `--query`: Query for the CiphertextBlob.
    *   `--output`: Output the text as is.
    *   `--region`: Specify the region your key is in.
    This command generates a base64 file containing the encrypted content (`ExampleSecretFileEncrypted.base64`).
3.  **Base64 Decode:** Decode the base64 file to get the binary encrypted value. This creates a file called `ExampleSecretFileEncrypted`, which contains the binary encrypted data.
    *   **Linux/Mac:**
        ```bash
        cat ExampleSecretFileEncrypted.base64 | base64 --decode > ExampleSecretFileEncrypted
        ```
    *   **Windows:** 
        ```bash
        certutil -decode ExampleSecretFileEncrypted.base64 ExampleSecretFileEncrypted
        ```
    **⚠️ Why convert Base64 back to binary?** Because KMS expects the binary ciphertext when you want to decrypt it later. The Base64 output is just an encoding (not extra encryption). To actually use the encrypted value, you need the binary form (what KMS understands as CiphertextBlob).

    ✅ In short:

    - Base64 = transport-friendly text form.
    - Binary = what KMS actually encrypts/decrypts.

4.  **KMS Decryption:** Use the `decrypt` command to decrypt the binary file.
    ```bash
    aws kms decrypt \
        --ciphertext-blob fileb://ExampleSecretFileEncrypted \
        --output text \
        --query Plaintext \
        --region us-east-1 \
        > ExampleFileDecrypted.base64
    ```
    *   `--ciphertext-blob`: Pass the encrypted file.
    *   `--query`: Query for the Plaintext value.
    *   `--output`: Output to base64 encoded file.
    *   `--region`: Specify the region.
    KMS automatically knows which key to use for decryption because it's included in the encrypted blob.
5.  **Base64 Decode (Decrypted):** Decode the base64 output to get the original text value. This will output the decrypted text, revealing your "SuperSecretPassword".
    *   **Linux/Mac:**
        ```bash
        base64 -d ExampleSecretFileDecrypted.txt
        ```
    *   **Windows:** 
        ```bash
        certutil -decode ExampleFileDecrypted.base64 ExampleFileDecrypted.txt
        ```

💡 **Tip:** The SDK abstracts some of these low-level commands, but this example demonstrates how to use the `encrypt` and `decrypt` commands of KMS with your own customer master key.

---

## 5. KMS Multi-Region Keys

Let's explore KMS Multi-Region keys and their use cases.

🔑 With KMS, you can create a Multi-Region key, meaning you have a primary key in one AWS Region (📌 **Example:** `us-east-1`) and it's replicated to other Regions (📌 **Example:** `us-west-2`, `eu-west-1`, `ap-southeast-2`).

![KMS Multi-Region Key](./img/KMS-Multi-Region-Key.png)

*   The key material is replicated across Regions.
*   The same key exists in multiple Regions.
*   The key ID is identical across all Regions (📌 **Example:** `key ID/mrk/`).

This allows you to use the same key interchangeably across different AWS Regions. You can encrypt data in one Region and decrypt it in another.

🔑 Multi-Region keys are possible because they share the same key ID and key material. If automatic key rotation is enabled for the primary key, rotations are replicated to other Regions.

With a Multi-Region key, you can encrypt in one Region and decrypt in another without re-encrypting data when moving between Regions or making cross-Region API calls.

⚠️ **Warning:** KMS Multi-Region keys are *not* global. You have a primary key and replicas, and each Multi-Region key is managed independently with its own key policy.

💡 **Tip:** It's generally not recommended to use Multi-Region keys unless you have specific use cases. KMS prefers keys to be bound to a single Region.

Use cases for Multi-Region keys:

*   Global client-side encryption: Encrypt client-side in one Region and decrypt client-side in another.
*   Encryption on Global DynamoDB tables.
*   Encryption on Global Aurora databases.

### DynamoDB Global Tables and KMS Multi-Region Keys with Client-Side Encryption

How do Global Tables in DynamoDB work with KMS Multi-Region keys and client-side encryption?

The goal is to encrypt specific attributes in your DynamoDB table, not just the entire table (which is at-rest encryption). This ensures that only specific clients can access the data, not even database administrators.

We'll use the Amazon DynamoDB Encryption Client.

📌 **Example:**

1.  Assume you have `us-east-1` and `ap-southeast-2` Regions.
2.  KMS has a Multi-Region key replicated to `ap-southeast-2`.
3.  A client application wants to insert data into a DynamoDB table.

The client application encrypts the attribute that needs to be encrypted using the primary Multi-Region key.

📝 **Note:** Most fields in the DynamoDB table will *not* be encrypted client-side. Only sensitive fields, like a Social Security number, will be encrypted.

This protects the data even from database administrators who have access to the DynamoDB table but not the KMS key used to encrypt the Social Security number.

If the DynamoDB table is a Global Table, the data is replicated to another Region (📌 **Example:** `ap-southeast-2`).

A client application in `ap-southeast-2` can retrieve the row, detect that the attribute is encrypted, and make a local API call to KMS to decrypt the attribute using the replica Multi-Region key.

This is why Multi-Region keys are useful here: client applications in `ap-southeast-2` can make local API calls to KMS for decryption.

![KMS Multi-Region Key With DynamoDB](./img/KMS-Multi-Region-Key-With-DynamoDB.png)

Using client-side encryption, you can protect specific fields or attributes and ensure decryption only when the client has access to the API key. Global Tables ensure that data and encryption keys are replicated together.

### Global Aurora and KMS Multi-Region Keys

The same concept applies to Global Aurora. We'll use the AWS Encryption SDK.

1.  Two Regions are in use.
2.  A Multi-Region key in KMS is replicated across these Regions.
3.  The client application wants to encrypt a column (📌 **Example:** `SSN` i.e. Social Security Number).
4.  The data is stored in an Amazon Aurora database table.

All data in the row is unencrypted except for the `SSN` column, which is encrypted with the Multi-Region key.

Since this is a Global Database, the tables are replicated globally. The same data exists in `ap-southeast-2`.

Clients in `ap-southeast-2` retrieve the encrypted data from the table. Because a Multi-Region key is used, they can make a local API call to KMS to decrypt the attribute, achieving lower latency.

**Client-side encryption protects this data even from database administrators.** If a database admin accesses the Amazon Aurora database and tries to access the `SSN` column, they won't be able to read the data without access to the KMS key.

---

## 6. S3 Replication and Encrypted Objects

When setting up S3 Replication, it's crucial to understand how it interacts with different encryption methods. Here's a breakdown:

*   By default, if you enable S3 Replication from one bucket to another:
    *   Unencrypted objects will be replicated by default.
    *   Objects encrypted with SSE-S3 will be replicated by default.
    *   Objects encrypted with SSE-C (customer-provided key) can also be replicated.
    *   Objects encrypted with SSE-KMS require additional configuration for replication. 🔑

Here's how to enable replication for **SSE-KMS** encrypted objects:

1.  Enable the option to replicate these objects.
2.  Specify the KMS Key you want to use to encrypt the objects in the target bucket.
3.  Adapt the KMS Key Policy for the target key.
4.  Create an IAM Role that allows the S3 Replication service to:
    *   Decrypt the data in the source bucket.
    *   Re-encrypt the data in the target bucket with the target KMS Key.

Because of all the encryption and decryption involved, you might encounter KMS throttling errors. ⚠️ In this case, you'll need to request a service quota increase.

A common question is whether to use multi-region keys with S3 Replication. 🤔

*   The documentation states that you *can* use multi-region keys.
*   However, Amazon S3 currently treats them as independent keys.
*   This means the object will still be decrypted and then re-encrypted using the *same* key, even if it's a multi-region key. 📝

---

## 7. Sharing Encrypted AMIs Across AWS Accounts

Here's a breakdown of the process for sharing an AMI encrypted with a KMS key from one AWS account (Account A) to another (Account B), allowing Account B to launch EC2 instances from it. This is a common exam topic! 📝

The scenario:

*   AMI resides in your source account (Account A).
*   AMI is encrypted with a KMS key owned by Account A.
*   Goal: Launch an EC2 instance in Account B using the AMI from Account A.

Here's how to do it:

1.  **Modify AMI Launch Permissions:** 🔑
    *   You must modify the AMI's properties to grant launch permissions to Account B.
    *   This allows Account B to launch instances from the AMI.
    *   Effectively, this is how you share the AMI.
    *   Modify the launch permissions and add the target account ID (Account B).

2.  **Share the KMS Key:** 🤝
    *   Account B needs access to the KMS key used to encrypt the AMI.
    *   This is typically achieved by modifying the KMS key policy.

3.  **Configure IAM Permissions in Account B:** 🛡️
    *   In Account B, create an IAM role or user.
    *   This IAM entity needs sufficient permissions to use both the KMS key and the AMI.
    *   Specifically, the IAM role/user needs the following KMS permissions:
        *   `kms:DescribeKey`
        *   `kms:ReEncryptFrom`
        *   `kms:CreateGrant`
        *   `kms:Decrypt`

    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                    "kms:DescribeKey",
                    "kms:ReEncryptFrom",
                    "kms:CreateGrant",
                    "kms:Decrypt"
                ],
                "Resource": "arn:aws:kms:REGION:ACCOUNT_A_ID:key/YOUR_KMS_KEY_ID"
            }
        ]
    }
    ```

4.  **Launch the EC2 Instance:** 🚀
    *   Once the permissions are correctly configured, Account B can launch an EC2 instance from the shared AMI.

5.  **Optional: Re-encryption:** 🔄
    *   Account B can optionally re-encrypt the volumes with a KMS key that it owns in its own account. This provides further isolation and control.

By following these steps, you can successfully share encrypted AMIs across AWS accounts. Understanding this process is crucial for the exam! 💡

---

## 8. SSM Parameter Store

The SSM Parameter Store provides secure storage for your configuration data and secrets. You can optionally encrypt these configurations using the KMS service, effectively turning them into secrets.

Here's a breakdown of its key features:

*   🛡️ **Security:** Leverages IAM for access control.
*   ✉️ **Notifications:** Integrates with Amazon EventBridge for notifications.
*   ☁️ **Serverless:** No servers to manage.
*   📈 **Scalable & Durable:** Designed for reliability and growth.
*   💻 **Easy to Use:** Simple SDK for interaction.
*   📜 **Version Tracking:** Keeps track of parameter updates. If you update your parameters then you have version tracking of them.
*   🤝 **CloudFormation Integration:** Use parameters as inputs for CloudFormation stacks.

![SSM Parameter Store](./img/SSM_Parameter_Store.png)

📌 **Example:**

You can store plain text configurations. Your application's IAM permissions (e.g., EC2 instance role) will be checked. For encrypted configurations, the Parameter Store uses KMS for encryption and decryption.  Make sure your applications have access to the underlying KMS key.

### Parameter Hierarchy

You can organize parameters in a hierarchical structure.

📌 **Example:**

Here’s the same hierarchy written as a nested list:

* /my-department/
  * my-app/
    * dev/
      * db_url
      * db_password
    * prod/
      * db_url
      * db_password
  * other-app/
* /other-department/


This structure allows you to:

*   Organize parameters in a structured way.
*   Simplify IAM policies by granting access to entire departments, apps, or specific environment paths.

📝 **Note:** You can access Secrets Manager secrets through the Parameter Store using a specific reference:- `/aws/reference/secretsmanager/secret_ID_in_Secrets_Manager`

💡 **Tip:** AWS provides Public Parameters, such as the latest AMI for Amazon Linux 2 in a specific region:- `/aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2` (public)

📌 **Example:**

A Dev Lambda function can have an IAM role to access `/my-app/dev/db_url` and `/my-app/dev/db_password`. A Prod Lambda function, with a different IAM policy and environment variables, can access `/my-app/prod/db_url` and `/my-app/prod/db_password`.

![SSM Parameter Store Hierarchy](./img/SSM_Parameter_Store_Hierarchy.png)

### Parameter Tiers

Systems Manager offers two parameter tiers: Standard and Advanced.

| Feature                                   | Standard           | Advanced                                |
|-------------------------------------------|--------------------|-----------------------------------------|
| Total number of parameters allowed (per AWS account and Region) | 10,000             | 100,000                                 |
| Maximum size of a parameter value          | 4 KB               | 8 KB                                    |
| Parameter policies available               | No                 | Yes                                     |
| Cost                                       | No additional charge | Charges apply                           |
| Storage Pricing                            | Free               | $0.05 per advanced parameter per month  |

### Parameter Policies (Advanced Tier Only)

Parameter policies allow you to manage the lifecycle of your parameters.

*   **Time to Live (TTL):** Set an expiration date for a parameter. This forces users to update or delete sensitive data like passwords. You can assign multiple policies at a time.

![SSM Parameter Store TTL](./img/SSM_Parameter_Store_TTL.png)

📌 **Example:** Expiration policy to delete a parameter:

EventBridge integration provides notifications.

📌 **Example:** 15 days before a parameter expires, you can receive a notification in EventBridge, giving you time to update it.

You can also configure notifications if a parameter hasn't been updated for a certain period.

📌 **Example:** If a parameter hasn't been updated for 20 days, you can receive a notification.

This allows for creative management of parameters.

---

## 9. Systems Manager Parameter Store - Hands On

The Parameter Store, a feature of **AWS Systems Manager**, is used for secrets and configuration data management. It centralizes parameters within your AWS accounts.

Note:- It is region specific (also the KMS).

- Search for "Parameter Store" in the AWS console (part of AWS Systems Manager).
- Inside Application Tools, you can find the Parameter store.

Here's how it works:

1.  Create a new parameter. ➕
2.  Specify the parameter type and value. ✍️
3.  Reference the parameter from within your commands or code. 🔗

Let's walk through creating parameters:
1.  Click on "Create parameter". 
* Parameter named `/my-app/dev/db_url`. (Must start with `/` and should not end with `/`).
*   Description: "Database URL for my app in development."
* Tier: Standard
*   Type: String
* Data Type: Text
*   Value: `dev.database.knowledge.com:3306`

Click "Create parameter". You will see the parameter created successfully, including its description, type, and version.  The last modified date and user are also tracked.

Now, let's create another parameter: `/my-app/dev/db_password`.

*   Description: "Database password from my app in development."
*   Type: SecureString
*   KMS Key Source: Current Account
*   KMS Key ID: `alias/tutorial` (a KMS key created beforehand)
*   Value: "devpassword"
* Click "Create parameter".  The value is encrypted using KMS.

Next, create parameters for the production environment:

*   `my-app/prod/database-url`
    *   Description: "URL for my app in prod."
    *   Type: String
    *   Value: `prod.database.knowledge.com:3306`
*   `my-app/prod/db_password`
    *   Description: "Database password in production."
    *   Type: SecureString
    *   KMS Key ID: `alias/tutorial`
    *   Value: "prod password"

Now you have four parameters stored in the Parameter Store.

### Accessing Parameters via AWS CLI

You can access these parameters using the AWS CLI.

To get specific parameters:

```bash
aws ssm get-parameters --names /my-app/dev/db_url /my-app/dev/db_password
```

The output will show the `database-url` with its decrypted string value. However, the `db_password` (SecureString) will be encrypted.

Output:-

```json
{
    "Parameters": [
        {
            "Name": "/my-app/dev/db_password",
            "Type": "SecureString",
            "Value": "AQICAHhiwxO4nsfXhPEIty73ZbsFp94xmHTJGHzmxU9PkzuKGAEwLTKkbaEVAHsxDQ+eBve1AAAAaTBnBgkqhkiG9w0BBwagWjBYAgEAMFMGCSqGSIb3DQEHATAeBglghkgBZQMEAS4wEQQMxj+vU7zsY1ucngDKAgEQgCYQKyH+cYcmeYOzmobKn/A+91/aVYHqo7yz0V5Qg5owFsv45dogOg==",
            "Version": 1,
            "LastModifiedDate": "2025-09-20T11:19:59.752000+05:30",
            "ARN": "arn:aws:ssm:us-east-1:xyz:parameter/my-app/dev/db_password",
            "DataType": "text"
        },
        {
            "Name": "/my-app/dev/db_url",
            "Type": "String",
            "Value": "dev.database.knowledge.com:3306",
            "Version": 1,
            "LastModifiedDate": "2025-09-20T11:17:57.212000+05:30",
            "ARN": "arn:aws:ssm:us-east-1:xyz:parameter/my-app/dev/db_url",
            "DataType": "text"
        }
    ],
    "InvalidParameters": []
}
```

To decrypt the SecureString, use the `--with-decryption` flag:

```bash
aws ssm get-parameters --names /my-app/dev/db_url /my-app/dev/db_password --with-decryption
```

This command decrypts the value, provided you have the necessary KMS permissions.

To retrieve parameters by path:

```bash
aws ssm get-parameters-by-path --path /my-app/dev
```

This retrieves all parameters under the `/my-app/dev/` path.

To retrieve all parameters recursively under `/my-app`:

```bash
aws ssm get-parameters-by-path --path "/my-app" --recursive
```

This retrieves all four parameters (dev and prod).

You can also use the `--with-decryption` flag with `get-parameters-by-path` to decrypt SecureStrings.

💡 **Tip:** Using a tree structure helps organize secrets and retrieve them efficiently.

⚠️ **Warning:** Ensure you have the correct KMS permissions when using `--with-decryption`. Without the necessary permissions, the decryption will fail.

---

## 10. Using Lambda with SSM Parameter Store

**Note:- This section had been removed from the tutorial.**

This section demonstrates how to use AWS Lambda to retrieve parameters from the SSM Parameter Store using the SDK. This is a practical example that showcases the integration between Lambda and SSM.

### Creating the Lambda Function

1.  Create a new Lambda function.
2.  Name the function: `helloWorldSSM`.
3.  Choose the runtime: any supported Python 3.x version (the course used Python 3.7, which Lambda deprecated in Dec 2023 — you can no longer create functions with it).
4.  For permissions, create a new role with basic Lambda permissions. This role will allow the function to write to CloudWatch Logs.
    *   The role will be named `helloWorldSSM-role-<8 character random string>`.
5.  Click "Create function".

### Modifying the IAM Role

The initial IAM role only grants basic Lambda permissions. We need to add permissions to allow the Lambda function to read and decrypt parameters from the SSM Parameter Store.

1.  Navigate to the IAM Roles console.
2.  Find the `helloWorldSSM-role-<8 character random string>`.
3.  Add an inline policy with the following configurations (to get and retrieve the parameters):
    *   Service: Systems Manager
    *   Actions (Read):
        *   `GetParameters` (Not `GetParameter`)
        *   `GetParametersByPath`
    *   Resources: Specific resources
        *   ARN:
            *   Region: Any
            *   Account: Any
            *   Fully qualified parameter name: `my-app/*` (This allows access to any parameter under the `my-app` path.)
4.  Include another for the KMS Decrypt action:
    *   Service: KMS
    *   Actions (Write): `Decrypt`
    *   Resources: Specific key
        *   Region: Any
        *   Account: Any
        *   Key ID: `<your-kms-key-id>` (Find this in the KMS console under "Encryption keys".)
5.  Review and name the policy `SSMAccessForMyApp`.
6.  Create the policy.

### Writing the Lambda Function Code

Now, let's modify the Lambda function code to retrieve parameters from SSM.

1.  Import the `boto3` library:

    ```python
    import boto3
    import os
    ```

2.  Create an SSM client outside the handler function for reuse:

    ```python
    ssm = boto3.client('ssm', region_name='eu-west-3') # Replace with your region
    ```

3.  Define the Lambda handler function:

    ```python
    def lambda_handler(event, context):
        # Get parameters from SSM
        db_url = ssm.get_parameters(Names=['/my-app/dev/db_url'])['Parameters'][0]['Value']
        db_password = ssm.get_parameters(Names=['/my-app/dev/db_password'], WithDecryption=True)['Parameters'][0]['Value']

        print("DB URL:", db_url)
        print("DB Password:", db_password)

        return "worked"
    ```

    📝 **Note:** Replace `/my-app/dev/db_url` and `/my-app/dev/db_password` with the actual names of your parameters in SSM.

### Testing the Lambda Function

1.  Create a new test event (e.g., named "helloworld").
2.  Click "Test".

Initially, the function will likely fail due to permission issues. This is expected, as IAM role changes might take a few minutes to propagate.

### Troubleshooting IAM Permissions

If you encounter an "Access Denied" error, it indicates that the IAM role doesn't have the necessary permissions.

1.  Double-check the IAM policy attached to the Lambda function's role.
2.  Ensure that the policy allows `GetParameters` and `GetParametersByPath` actions on the correct SSM parameter paths.
3.  Wait a few minutes and test the function again. IAM changes are not always immediate.

### Decrypting Secure Strings

To decrypt secure string parameters, you need to add KMS decrypt permissions to the Lambda function's IAM role.

1.  Add another inline policy to the `hello world SSM role`.
2.  Service: KMS
3.  Action (Write): `Decrypt`
4.  Resources: Specific key
    *   Region: Any
    *   Account: Any
    *   Key ID: `<your-kms-key-id>` (Find this in the KMS console under "Encryption keys".)
5.  Review and name the policy `KMS decrypt tutorial key`.
6.  Create the policy.

⚠️ **Warning:** Granting KMS decrypt permissions should be done with caution. Limit access to only the necessary keys.

### Using Environment Variables for Dynamic Configuration

You can use environment variables to switch between different parameter sets (e.g., dev and prod).

1.  Add an environment variable to the Lambda function:
    *   Key: `dev_or_prod`
    *   Value: `dev` (or `prod`)

2.  Modify the Lambda function code to use the environment variable:

    ```python
    import boto3
    import os

    ssm = boto3.client('ssm', region_name='eu-west-3')

    def lambda_handler(event, context):
        env = os.environ['dev_or_prod']
        db_url_param_name = f'/my-app/{env}/db_url'
        db_password_param_name = f'/my-app/{env}/db_password'

        db_url = ssm.get_parameters(Names=[db_url_param_name])['Parameters'][0]['Value']
        db_password = ssm.get_parameters(Names=[db_password_param_name], WithDecryption=True)['Parameters'][0]['Value']

        print("DB URL:", db_url)
        print("DB Password:", db_password)

        return "worked"
    ```

🔧 Steps to Set Environment Variable via AWS Lambda Console:

1. Go to the [AWS Lambda Console](https://console.aws.amazon.com/lambda/)
2. Click on your Lambda function name (e.g., `helloWorldSSM`)
3. Scroll down to the **"Environment variables"** section
4. Click **"Edit"**
5. Under **"Environment variables"**, add:

   * **Key:** `dev_or_prod`
   * **Value:** `dev` (or `prod`, depending on what you want)
6. Click **"Save"**

```
Key: dev_or_prod
Value: dev
```

    📌 **Example:** If `dev_or_prod` is set to `prod`, the function will retrieve parameters from `/my-app/prod/db_url` and `/my-app/prod/db_password`.

### Conclusion

This tutorial demonstrated how to integrate AWS Lambda with SSM Parameter Store to retrieve and decrypt configuration data. By using IAM roles and environment variables, you can securely manage and dynamically configure your Lambda functions.

---

## 11. AWS Secrets Manager

AWS Secrets Manager is a service designed for storing and managing secrets. It offers several advantages over SSM Parameter Store, primarily the ability to enforce and automate secret rotation.

Here's a breakdown of its key features:

*   🔄 **Secret Rotation:** You can configure Secrets Manager to automatically rotate secrets at specified intervals. This enhances security by regularly updating credentials.
*   🔑 **Automated Secret Generation:** Secrets Manager can automatically generate new secrets during rotation using Lambda functions. You define a Lambda function to handle the secret generation process.
*   🤝 **Integration with AWS Services:** Secrets Manager integrates seamlessly with various AWS services, including:
    *   Amazon RDS (MySQL, PostgreSQL, SQL Server, Aurora)
    *   Other databases (check for specific integrations)

    This integration allows you to store database credentials directly in Secrets Manager and automate their rotation.
*   🔒 **Encryption:** Secrets are encrypted using the KMS service, providing an additional layer of security.

📌 **Example:** When you see questions about secrets or integration with RDS or Aurora in the exam, consider Secrets Manager as a potential solution.

### Multi-Region Secrets

Secrets Manager supports replicating secrets across multiple AWS regions.

*   **Replication:** Secrets are replicated from a primary region to secondary regions.
*   **Synchronization:** The Secrets Manager service keeps the replica secrets synchronized with the primary secret.

![Multi-Region Secrets](./img/Multi-Region-Secrets.png)

Why use multi-region secrets?

1.  🛡️ **Disaster Recovery:** If the primary region (e.g., US East 1) experiences issues, you can promote a replica secret to a standalone secret in another region.
2.  🌍 **Multi-Region Applications:** Enables building applications that span multiple AWS regions.
3.  🚑 **Disaster Recovery Strategies:** Supports disaster recovery scenarios by ensuring secrets are available in multiple locations.
4.  🔗 **Replicated RDS Databases:** If you have an RDS database replicated across regions, you can use the same secret to access the corresponding database instance in each region.

📝 **Note:** Multi-region secrets help improve the resilience and availability of your applications.

---

## 12. AWS Secrets Manager - Hands On

Secrets Manager helps you easily rotate, manage, and retrieve secrets throughout their lifecycle. It's similar to the SSM Parameter Store for storing secret information, but differs by offering rotation, management, and tight integrations with databases.

**Database Integrations:**

*   MySQL
*   PostgreSQL
*   Amazon Aurora
*   RDS

**Pricing:**

*   30-day free trial available.
*   \$0.40 per secret, per month.
*   \$0.05 per 10,000 API calls.

💡 **Tip:** To stay within the free tier, create a secret and then delete it after use.

### Storing a New Secret

To store a new secret:

1.  Choose a secret type. The available integrations may increase over time.
2.  Available secret types:
    *   Amazon RDS
    *   Amazon DocumentDB
    *   Amazon Redshift
    *   Other databases
    *   Other types of secrets

📌 **Example:**  For "Other type of secrets," you can store key-value pairs:

```text
MySecretKey: MyVerySecretValue
API_KEY: <secret API key>
```

You can enter secrets through the UI as key-value pairs, in plain text, or as a JSON document.

📝 **Note:** Only users with the correct IAM permissions can access these secrets.

### Encryption Key

Specify an encryption key. You can use the default key or your own KMS key.

### Naming and Permissions

1.  Name your secret (e.g., `prod/my-secret`).
2.  Optionally add a description.
3.  Resource Permissions: Define a policy that restricts who can access the secret. This can be across AWS accounts using a resource policy (similar to an S3 bucket policy).

### Multi-Region Replication

You can replicate the secret across regions for multi-region setups (e.g., multi-region apps or databases).

1.  Select the regions to replicate to (e.g., `us-west-2`, `ap-southeast-1`).
2.  Specify the encryption key for each region.

### Automatic Secret Rotation

You can enable automatic secret rotation.

1.  Choose "yes" to enable rotation.
2.  Specify the rotation frequency.
3.  If enabled, you need to specify a rotation function. This is a Lambda function that performs the rotation.

### Retrieving Secrets

After creating a secret, you'll be provided with code snippets to retrieve the secrets from your applications.

### RDS Database Credentials

You can store credentials for an Amazon RDS database.

1.  Create a username and password.
2.  Specify the database.

The integration between RDS and Secrets Manager allows the username and password to be used to log in to the database. If you enable rotation, the database is automatically updated with the new credentials.

---

## 13. AWS Certificate Manager (ACM)

AWS Certificate Manager (ACM) 🔐 simplifies the process of provisioning, managing, and deploying TLS certificates on AWS.

TLS certificates (sometimes referred to as SSL certificates) are essential for providing in-flight encryption for websites, ensuring secure communication via HTTPS.

### How ACM Works

Imagine you have an Application Load Balancer (ALB) connected to an Auto Scaling group, and you want to expose your application balancer as an HTTPS endpoint.

1.  Integrate the ALB with ACM.
2.  ACM provisions and maintains TLS certificates directly on your ALB.
3.  Users access your website or API using the HTTPS protocol.

### Key Features

*   ACM supports both public and private TLS certificates.
*   Public TLS certificates are free of charge.
*   Automatic renewal of certificates is supported.
*   Integrates with various AWS services:
    *   Elastic Load Balancers (Classic, Application, and Network)
    *   CloudFront Distributions
    *   APIs on API Gateway

⚠️ **Warning:** A normal (free) ACM public certificate cannot be used directly on EC2 instances, and its private key cannot be extracted. Since Jun 2025, ACM also offers **exportable** public certificates (paid, per domain) that you can install on EC2 or on-premises. Older exam questions assume public certificates can never be exported.

### Requesting a Public Certificate

Here's the process to request a public certificate:

1.  List the domain names to be included in the certificate. This can be:
    *   A fully qualified domain name (FQDN), 📌 **Example:** `corp.example.com`
    *   A wildcard domain, 📌 **Example:** `*.example.com`
    *   You can include multiple domains.
2.  Select a validation method:
    *   DNS validation (recommended for automation)
    *   Email validation
3.  If using DNS validation, create a CNAME record in your DNS configuration to verify domain ownership. If you use Route 53, this process is automated.
4.  Wait a few hours for verification.
5.  The certificate is issued and automatically enrolled for renewal.

### Automatic Renewal

ACM automatically renews ACM-generated certificates before they expire. Since Feb 2026, new public certificates are valid for at most 198 days and are renewed **45 days** before expiry (older 395-day certificates renew 60 days before expiry — older exam questions may say 60 days).

### Importing Certificates

If you import a certificate generated outside of ACM, automatic renewal is **not** supported. You must manually import a new certificate before the existing one expires.

### Expiration Notifications

![ACM Expiration Notifications](./img/ACM_Expiration_Notifications.png)

ACM provides two methods for receiving expiration notifications:

1.  **EventBridge:** ACM sends daily expiration events to EventBridge, starting 45 days (configurable) prior to expiration. You can then trigger Lambda functions, SNS topics, or SQS queues from EventBridge.
2.  **AWS Config:** Use the managed rule `ACM-certificate-expiration-check` to check for expiring certificates (configurable number of days). Non-compliance events are sent to EventBridge, allowing you to trigger Lambda, SNS, or SQS.

### ACM integration with ALB (Application Load Balancers)

*   Provision and maintain TLS certificates through ACM for your ALB.
*   Set up a redirect rule on your ALB to redirect HTTP traffic to HTTPS. This ensures users always access your application securely.

### ACM and API Gateway

To integrate ACM with API Gateway, you need to understand the different endpoint types:

*   **Edge-optimized:** 
*   * Clients are global, and requests are routed through CloudFront Edge locations (improves latency). 
    * The API Gateway still lives in only one region.
*   **Regional:** 
    * Clients are within the same region as your API Gateway.
    * Cloud manually combine with CloudFront (more control over the caching strategies and the distribution).
*   **Private:** 
    * Accessible only from within your VPC using an interface VPC endpoint (ENI).
    * Use a resource policy to define access.

ACM is relevant for Edge-optimized and Regional endpoints.

#### Integration Steps

1.  Create a Custom Domain Name resource in API Gateway.
2.  Configure the custom domain name based on the endpoint type:

    *   **Edge-optimized:**
        *   TLS certificates must be attached to your CloudFront distribution.
        *   Certificates must be created in the `us-east-1` region (where CloudFront is located).
        *   Set up a CNAME or alias record in Route 53.
    *   **Regional:**
        * For clients within the same region.  
        * TLS certificates must be imported into API Gateway in the same region as the API stage.
        *   Set up a CNAME or (better) A-alias record in Route 53 to point to your DNS.

💡 **Tip:** For Edge-optimized API Gateways, ensure your ACM certificate is in the `us-east-1` region.

📝 **Note:** ACM simplifies TLS certificate management for AWS services, but understanding the nuances of each integration is crucial for a secure and efficient setup.

---

## 14. AWS WAF (Web Application Firewall)

AWS WAF is a Web Application Firewall used to protect your web applications from common web exploits at Layer 7 (HTTP). In comparison, Layer 4 is for TCP or UDP protocols.

WAF can be deployed on:

*   Application Load Balancer (ALB)
*   API Gateway
*   CloudFront
*   AppSync GraphQL API
*   Cognito user pools

⚠️ **Warning:** The exam will try to trick you into deploying WAF on an NLB. This is not possible!

Once deployed, you can define Web ACLs (Web Access Control Lists) and their rules.

You can set rules to filter based on:

*   IP addresses: You can define IP sets, with each set holding up to 10,000 IP addresses. Use multiple rules for more IPs.
*   HTTP headers, HTTP body, URI strings protects from common attacks - like SQL injection and cross-site scripting ( XSS ).
*   Size constraints: To limit request sizes (e.g., up to 2MB).
*   Geo match: To allow or block specific countries.
*   Rate-based rules: To count requests per IP for DDoS protection. 📌 **Example:** Block an IP that sends more than 1,000 requests in 5 minutes. The rate is counted over an evaluation window of 1, 2, 5 or 10 minutes (5 is the default), not per second. The lowest limit you can set is 10.

Web ACLs are regional, except for CloudFront, where they are defined globally.

📝 **Note:** A "rule group" is a reusable set of rules that can be added to multiple Web ACLs for organization.

### Use Case: Fixed IP with WAF and ALB

Problem: How to get a fixed IP for your application while using WAF with an Application Load Balancer?

*   WAF doesn't support Network Load Balancers (NLB) because NLBs operate on Layer 4, and WAF is for Layer 7.
*   ALBs don't have fixed IPs.

Solution: Use a Global Accelerator to get a fixed IP for the application and then enable WAF on the ALB.

![AWS WAF And ALB](./img/AWS_WAF_and_ALB.png)

Architecture:

1.  One region with an ALB and EC2 instances.
2.  Front the ALB with a Global Accelerator to get a fixed IP.
3.  Attach a Web Application Firewall with a WebACL in the same region as the ALB.

This achieves the target architecture of having a fixed IP address while using WAF to protect the application. 🛡️

---

## 15. AWS Shield: Protecting Against DDoS Attacks 🛡️

AWS Shield is a service designed to protect your infrastructure from Distributed Denial of Service (DDoS) attacks. Let's break down what that means and the protection AWS offers.

### What is a DDoS Attack? 💥

A DDoS attack, or Distributed Denial of Service attack, aims to overwhelm your infrastructure by flooding it with a massive number of requests simultaneously. These requests originate from numerous computers across the globe.

The goal is to overload your systems to the point where they become unable to serve legitimate users. This effectively denies service to your intended audience.

### AWS Shield Standard: Free DDoS Protection 🆓

AWS Shield Standard is a free service automatically enabled for all AWS customers. It provides baseline protection against common network and transport layer attacks, including:

*   SYN floods
*   UDP floods
*   Reflection attacks
*   Other Layer 3 and Layer 4 attacks

### AWS Shield Advanced: Enhanced DDoS Mitigation 🛡️

For more sophisticated DDoS protection, AWS offers Shield Advanced. This is an **optional, paid service** that provides enhanced mitigation capabilities.

*   **Cost:** Approximately \$3,000 per month per organization.
*   **Protected Resources:** Protects against more complex DDoS attacks targeting:
    *   Amazon EC2
    *   Elastic Load Balancing (ELB)
    *   Amazon CloudFront
    *   AWS Global Accelerator
    *   Route 53

#### Key Features of Shield Advanced:

1.  **24/7 DDoS Response Team Access:** 🧑‍💻 Provides round-the-clock access to AWS DDoS experts who can assist you during an attack.
2.  **DDoS Cost Protection:** 💰 Shields you from increased AWS usage fees resulting from a DDoS attack.
3.  **Automatic Application Layer DDoS Mitigation:** 🤖 Automatically creates, evaluates, and deploys Web Application Firewall (WAF) rules to mitigate Layer 7 attacks. This is a huge benefit!

    📌 **Example:** Shield Advanced can automatically update your WAF rules to block malicious traffic patterns observed during a Layer 7 DDoS attack.

    📝 **Note:** Layer 7 attacks target the application layer, often exploiting vulnerabilities in your web applications.

    💡 **Tip:** Regularly review your WAF rules and Shield Advanced configurations to ensure optimal protection.

    ```
    # Example WAF rule (hypothetical)
    {
      "name": "BlockHighRequestRate",
      "priority": 10,
      "action": "BLOCK",
      "statement": {
        "rateBasedStatement": {
          "aggregateKeyType": "IP",
          "scopeDownStatement": {
            "geoMatchStatement": {
              "countryCodes": ["CN", "RU"]
            }
          },
          "limit": 1000
        }
      }
    }
    ```

---

## 16. AWS Firewall Manager

AWS Firewall Manager is a service designed to manage firewall rules across all accounts within an AWS Organization. It allows you to centrally manage security policies, ensuring consistent security across your entire AWS environment.

*   The core idea is to manage rules across many accounts simultaneously.
*   You define a **security policy**, which is a common set of security rules.

Firewall Manager can manage several types of security rules:

*   Web Application Firewall (WAF) rules: Applied to Application Load Balancers (ALB), API Gateways, CloudFront distributions, and more.
*   Shield Advanced rules: Protect ALBs, Classic Load Balancers (CLB), Network Load Balancers (NLB), Elastic IPs, and CloudFront distributions.
*   Security policies to standardize security groups: Applied to EC2 instances, Application Load Balancers, and Elastic Network Interfaces (ENI) resources in your VPC.
*   AWS Network Firewall rules: Configured at the VPC level.
*   Amazon Route 53 Resolver DNS Firewall rules.

Key features of Firewall Manager:

*   Centralized Management: Manage all your firewalls from a single place. 🛡️
*   Regional Policies: Policies are created at the region level. 🌍
*   Organization-Wide Application: Policies are applied to all accounts within your organization (including future accounts). 🏢
*   Automatic Rule Application: If a new resource (e.g., an ALB) is created, Firewall Manager automatically applies the relevant rules to it. ⚙️

### WAF, Shield, and Firewall Manager: Working Together

WAF, Shield, and Firewall Manager are used together to provide comprehensive protection for your AWS accounts.

1.  WAF: You define your Web ACL rules in WAF. If you need one-time protection, WAF is the right choice.
2.  Firewall Manager: If you want to use WAF across multiple accounts, accelerate WAF configuration, and automate the protection of new resources, you manage your WAF rules within Firewall Manager. It applies these rules automatically to all your accounts and resources.
3.  Shield Advanced: Protects against DDoS attacks and offers additional features on top of WAF, such as:
    *   Dedicated support from the Shield Response Team (SRT). 🧑‍💻
    *   Advanced reporting. 📊
    *   Automatic WAF rule creation. 🤖

If you are prone to frequent DDoS attacks, consider purchasing Shield Advanced. Firewall Manager can also help you deploy Shield Advanced across all your accounts.

---

## 17. WAF, Shield, and Firewall Manager - Hands On

This section provides an overview of WAF (Web Application Firewall), Shield, and Firewall Manager, highlighting their functionalities and how they complement each other. In AWS console, search for WAF & Shield.

### Web Application Firewall (WAF) 🛡️

WAF helps protect your web applications from common web exploits at layer seven. It works with:

*   Amazon API Gateway
*   CloudFront distributions
*   Application Load Balancer (ALB)

To use WAF, you create a **Web ACL (Access Control List)**. Here's how:

1.  **Name the Web ACL:** 📌 **Example:** 'DemoWebACL'.
2.  **Choose the Resource Type:** Options are:-
    *   Regional resources (ALB, API Gateway, etc.). Choose this for our case.
    *   CloudFront distribution (global, only in US East 1)
3.  **Associate Resources:** Select the AWS resources you want to protect with the Web ACL.
4.  **Add Rules:** You can add managed rule groups or create your own rules.

    *   **Managed Rules:** These come from AWS and partners. 📌 **Example:**
        * Paid rules in AWS managed rule groups:-
            *   Bot Control Rule: Prevents bots from accessing your application.
        * Free rules in AWS Managed rule groups:-
            *   Amazon IP Reputation List: Allows access only to reputable IPs.
            *   Anonymous IP List: Blocks IPs from VPNs, proxies, etc.
            *   Known Bad Inputs: Blocks known malicious inputs.
5.  **Rule Capacity:** Each rule has a capacity (WCUs). The basic Web ACL price includes up to 1,500 WCUs. You can go up to 5,000 WCUs per Web ACL, but each WCU above 1,500 costs extra. Older exam questions may say the maximum is 1,500. This limits the number of rules you can have.
6.  **Default Action:** If a request doesn't match any rule, the default action is to allow it. Otherwise, it will be blocked.
7.  **Rule Priorities:** Set the order in which rules are evaluated (Move the rules Up and Down).
8.  **Metrics:** Configure metrics for monitoring.
9.  **Create Web ACL:** Once created, you can associate it with your ALB, API Gateway, etc.
10. **IP Sets:** You can set up IP sets to define which IP addresses to block or allow. 📝 **Note:** Enter one IP address per line.

### Shield 🛡️

Shield provides DDoS (Distributed Denial of Service) protection.

*   Shield Advanced offers enhanced protection but comes with a cost ($3,000 per month). ⚠️ **Warning:** Be mindful of the cost before subscribing.
*   The primary purpose of Shield is to mitigate DDoS attacks.

### Firewall Manager 🛡️

Firewall Manager offers centralized security management of your rules across your organization.

*   It allows you to create policies that apply to all accounts.
*   Creating a policy incurs a monthly cost ($100 per month). ⚠️ **Warning:** Be mindful of the cost before creating a policy.
*   You can apply policies for WAF, Shield Advanced, security groups, network firewalls, and more.
*   📌 **Example:** You can define a WAF policy for all your accounts in the Ireland region.

---

## 18. DDoS Protection and Best Practices

Let's explore DDoS protection strategies and best practices within a typical AWS architecture. We'll consider scenarios involving Auto Scaling Groups with EC2 instances, Elastic Load Balancers (ELB), Global Accelerator, CloudFront, WAF (Web Application Firewall), and Route 53.

### Architecture Overview

Common architectural patterns include:

*   Auto Scaling Group ➡️ ELB ➡️ Global Accelerator (fixed IPs)
*   Auto Scaling Group ➡️ ELB ➡️ CloudFront ➡️ WAF
*   CloudFront ➡️ API Gateway

The key is understanding how these components contribute to DDoS resilience.

![DDoS Protection and Best Practices Architecture](./img/DDoS_Protection_and_Best_Practices_Architecture.png)

### Edge Location Mitigation

By leveraging AWS's Edge locations, we can significantly enhance DDoS protection.

*   **CloudFront:** Delivers web applications from the Edge, mitigating common DDoS attacks through Shield. This includes protection against common attacks like SYN floods and UDP reflection attacks.
*   **Global Accelerator:** Provides global access to your application at the Edge, also integrated with Shield for DDoS protection. This is useful when your backend isn't compatible with CloudFront.
*   Using either CloudFront or Global Accelerator ensures your application is distributed at the Edge, benefiting from AWS's DDoS protection.
*   **Route 53:** Offers global domain name resolution at the Edge, with built-in DDoS protection for your DNS.

💡 **Tip:** Being at the Edge provides a strong foundation for DDoS mitigation.

- BP1 means Best Practices 1, and BP2 means Best Practices 2, and similar.

### AWS Best Practices for DDoS Resiliency Best Practices for DDoS Mitigation

#### Infrastructure Layer Defense (BP1, BP3, BP6)

* The goal is to protect EC2 instances from high traffic volumes.
* CloudFront, Global Accelerator, Route 53, and ELB work together to handle traffic before it reaches your EC2 instances.

#### Amazon EC2 with Auto Scaling (BP7)
* Enables automatic scaling to accommodate increased load during an attack. It helps scale in case of sudden traffic surges including a flash crowd or a DDoD attack.

#### Elastic Load Balancing (BP6)
* ELB scales with the traffic increases and will distributes traffic across multiple EC2 instances, ensuring each instance receives a manageable load.

### AWS Best Practices for DDoS Resiliency - Application Layer Defense

#### Detect and filter malicious web requests (BP1, BP2)

*   **CloudFront:** Serves static content from Edge locations, shielding your backend.
*   **WAF:** AWS WAF is used on top of CloudFront and Application Load Balancers to filters and blocks requests based on signatures.
    *   Block specific IPs.
    *   Block specific request types.
*   **WAF Rate-Based Rules:** Automatically block IPs of malicious actors.
*   **WAF Managed Rules:** Utilize pre-configured rules to block IPs based on reputation or anonymous access.
*   **CloudFront Geo Restriction:** Block traffic from specific geographic locations.

💡 **Tip:** CloudFront and WAF are managed services that handle request filtering, protecting against DDoS attacks.

#### Shield Advanced (BP2, BP2, BP6)

*   **Shield Advanced:** Automatically creates WAF rules to mitigate layer seven attacks. It automatically creates, evaluates, and deploys AWS WAF rules to mitigate Layer 7 attacks.

### Reducing the Attack Surface

#### Obfuscating AWS Resources (BP1, BP4, BP6)


* Hiding backend AWS resources is crucial.
*   Using CloudFront, API Gateway, or ELB hides your backend infrastructure. Attackers won't know if you're using Lambda, EC2, or ECS.

#### Security Groups and Network ACLs (BP5)

*  Use Security Groups and Network ACLs to filter traffic based on specific IPs at the subnet or ENI level.
*  Elastic IP are protected by AWS Shield Advanced

#### Protecting API Endpoints (BP4)

*   **API Gateway:**
    *   Hides the backend (EC2, Lambda, etc.).
    *   **Edge-Optimized Mode:** Provides global distribution.
    *   **CloudFront + Regional Mode:** Offers more control for DDoS protection.
*   **WAF on API Gateway:** Filters HTTP requests.
*   **API Gateway Configuration:**
    *   Burst limits.
    *   Header filtering.
    *   API keys.

💡 **Tip:** Properly configuring the API Gateway is essential for DDoS protection.

By implementing these strategies, you can build a resilient architecture that effectively mitigates DDoS attacks.

---

## 19. Amazon GuardDuty

Amazon GuardDuty is a service that helps you with intelligent threat discovery to protect your AWS accounts. It uses machine learning algorithms, anomaly detection, and third-party data to identify these threats.

Here's how it works:

*   🛡️ **Easy Setup:** Enabling GuardDuty is as simple as a single click.
*   💰 **Free Trial:** You get a 30-day trial period to evaluate the service.
*   🛠️ **No Software Installation:** You don't need to install any software.

GuardDuty analyzes various input data sources to detect threats:

*   **CloudTrail Event Logs:** Looks for unusual API calls and unauthorized deployments. This includes:
    *   Management events (e.g., create VPC Subnet).
    *   Data events on S3 (e.g., get object, list objects, delete objects) — only when you turn on the optional **S3 Protection** feature. They are not a foundational source.
*   **VPC Flow Logs:** Monitors for unusual internet traffic and IP addresses.
*   **DNS Logs:** Detects EC2 instances sending encoded data within DNS queries, which could indicate a compromise.

GuardDuty also offers **optional features** to analyze additional data sources:

*   EKS audit logs
*   RDS and Aurora login events
*   EBS volumes
*   Lambda network activity
*   S3 data events

You can set up EventBridge rules to receive automatic notifications when findings are detected. These rules can target various services, such as AWS Lambda or SNS topics.

📌 **Example:**

```text
EventBridge Rule -> Lambda Function (Automated Response)
EventBridge Rule -> SNS Topic (Notification)
```

⚠️ **Warning:** **GuardDuty is particularly effective at protecting against cryptocurrency attacks, as it has a dedicated finding for this type of threat.** (Important for Exam)

![AWS GuardDuty](./img/AWS_GuardDuty.png)

In summary, GuardDuty utilizes several input data sources:

1.  **Mandatory Input Data:**
    *   VPC Flow Logs
    *   CloudTrail Logs
    *   DNS Logs
2.  **Optional Features (Input Data):**
    *   S3 Logs
    *   EBS Volumes
    *   Lambda Network Activity
    *   RDS and Aurora Login Activity
    *   EKS Logs and Runtime Monitoring

Based on this data, GuardDuty generates findings. When a finding is detected, an event is created in Amazon EventBridge.

From EventBridge, you can use rules to trigger automations (e.g., using Lambda functions) or send notifications (e.g., using SNS).

---

## 20. Amazon Inspector 🛡️

Amazon Inspector is a service that allows you to run automated security assessments on several resources:

*   EC2 instances
*   Container Images pushed to Amazon ECR
*   Lambda functions

Let's break down each of these.

### EC2 Instances 💻

Amazon Inspector leverages the AWS Systems Manager (SSM) agent on your EC2 instances to assess their security. (Since 2024 it can also scan EC2 instances without the agent, using EBS snapshots — called agentless or hybrid scanning.) It analyzes:

*   Unintended network accessibility
*   The running operating system for known vulnerabilities

This analysis is performed continuously.

### Container Images (Amazon ECR) 🐳

As your Container Images (e.g., Docker images) are pushed to Amazon ECR, they are analyzed by Amazon Inspector against known vulnerabilities.

### Lambda Functions ƛ

When Lambda functions are deployed, they are analyzed by Inspector for:

*   Software vulnerabilities in the function code
*   Package dependencies

This assessment happens as the functions are being deployed.

### Reporting and Integration 📣

Once Amazon Inspector completes its assessment, it can report its findings to:

*   AWS Security Hub: This gives you a central view of vulnerabilities in your infrastructure.
*   Amazon EventBridge: You can send findings and events to EventBridge to run automations.

![AWS Inspector](./img/AWS_Inspector.png)

### What Amazon Inspector Evaluates 🤔

Remember, Amazon Inspector ONLY focuses on:

*   Running EC2 instances
*   Container Images on Amazon ECR
*   Lambda functions

It performs continuous scanning of the infrastructure only when needed.

Inspector checks:

* Package vulnerabilities (EC2, ECR, and Lambda) -  A database of vulnerabilities (CVE).
*   Network reachability on Amazon EC2.

If the CVE database is updated, Amazon Inspector automatically reruns to ensure your infrastructure is retested.

Each time Inspector runs, a risk score is associated with all vulnerabilities for prioritization.

---

## 21. AWS Macie

AWS Macie is a fully managed data security and data privacy service. It leverages machine learning and pattern matching to discover and protect your sensitive data within AWS. 🛡️

Macie's primary function is to alert you to the presence of sensitive data, specifically **personally identifiable information (PII)**.

![AWS Macie](./img/AWS_Macie.png)

Here's a breakdown of how Macie works:

1.  Your PII data resides in your S3 buckets. 🗄️
2.  Macie analyzes the data within those S3 buckets. 🔍
3.  Macie identifies and classifies data as PII. 🏷️
4.  Macie notifies you of its discoveries through EventBridge. 📢
5.  You can then integrate these notifications with other AWS services, such as SNS topics or Lambda functions. 🔗

In essence, Macie is designed to find sensitive data within your S3 buckets. That is its sole purpose. 🎯

Enabling Macie is straightforward:

*   It's a one-click activation. 🖱️
*   You simply specify the S3 buckets you want Macie to monitor. ✅

That's a concise overview of AWS Macie!

---

## 22. Q & A

### Question 1

When you enable **Automatic Rotation** on your **KMS Key**, the backing key is rotated every _____________ **Options**

1. 90 days
2. 1 year
3. 2 years
4. 3 years

<details>
<summary>Explanation</summary>

✅ **Correct Answer**: 1 year

AWS Key Management Service (KMS) supports automatic key rotation for customer-managed KMS keys. When enabled, AWS automatically generates a new cryptographic backing key every 12 months (1 year). The key ID remains the same, but a new underlying key material is created and used for future encrypt operations. Older backing keys are retained to decrypt data encrypted in the past, ensuring backward compatibility and seamless access.

Therefore, the backing key rotation occurs every 1 year by default.

> ⚠️ Note (2026): Since Apr 2024 you can set a custom rotation period from 90 to 2,560 days. 1 year (365 days) is still the default, so "1 year" stays the correct answer here.

</details>

---

### Question 2

You have created a **Customer-managed CMK** in KMS that you use to encrypt both S3 buckets and EBS snapshots. Your company policy mandates that your encryption keys be rotated every **6 months**. What should you do?
**Options**

1. Re-configure your KMS CMK and enable Automatic Key Rotation, and configure the Retention Period with 180 days
2. Use AWS Managed Keys as they are automatically rotated by AWS every 3 months
3. Rotate the KMS CMK manually. Create a new KMS CMK and use Key Aliases to reference the new KMS CMK. Keep the old KMS CMK so you can decrypt the old data

<details>
<summary>Explanation</summary>

✅ **Correct Answer**: 1. Re-configure your KMS CMK and enable Automatic Key Rotation, and configure the Retention Period with 180 days

AWS KMS supports **automatic key rotation** for **customer-managed CMKs**. The rotation occurs every **1 year** by default. Since Apr 2024 you can set a custom **rotation period** from 90 to 2,560 days. (The option text says "Retention Period", but the KMS setting is called the **rotation period**.)

By **reconfiguring the CMK** and enabling **automatic key rotation** with a **rotation period of 180 days**, you ensure:

* Keys are rotated **every 6 months** as per policy
* Older backing keys are kept until the KMS key itself is deleted, so you can always **decrypt previously encrypted data**
* No need for manual CMK creation or alias management

This solution meets compliance while maintaining operational efficiency and seamless decryption.

> ⚠️ Note (2026): Before Apr 2024 the rotation period was fixed at 1 year. So older answer keys mark option 3 (manual rotation: create a new key and move the alias) as correct. With current KMS, option 1 is the better answer.

</details>

---

### Question 3

Which AWS service helps you protect your sensitive data stored in S3 buckets?
**Options**

* Amazon GuardDuty
* Amazon Shield
* Amazon Macie
* AWS KMS

<details>
<summary>Explanation</summary>

✅ **Correct Answer**: Amazon Macie

**Amazon Macie** is a fully managed data security and privacy service that uses **machine learning** to automatically discover, classify, and **protect sensitive data** stored in Amazon S3.

It identifies:

* **Personally Identifiable Information (PII)** and other sensitive content
* **Unencrypted S3 buckets**
* **Publicly accessible buckets**
* **Cross-account shared buckets**

Macie alerts you to potential data exposure risks, helping you prevent unauthorized access or data leakage.

</details>

---

### Question 4

An online-payment company is using AWS to host its infrastructure. The frontend is hosted on S3 and CloudFront, while the backend runs on EC2 with Aurora Global Database for multi-region low-latency access. A new requirement mandates that customers must be able to store data encrypted **on the client side**, and the encrypted data **must not be visible even to company administrators**. The encrypted data must be stored in the database and available across multiple AWS Regions.

What should you implement to meet this requirement? **Options**

* **Using Aurora Client-side Encryption and KMS Multi-region Keys**
* **Using Lambda Client-side Encryption and KMS Multi-region Keys**
* **Using Aurora Client-side Encryption and CloudHSM**
* **Using Lambda Client-side Encryption and CloudHSM**

<details>
<summary>Explanation</summary>

✅ **Correct Answer**: ✨ **Using Aurora Client-side Encryption and KMS Multi-Region Keys** ✅

To ensure that:

* Data is **encrypted on the client side** before reaching the database
* **Administrators cannot read the data** (zero-trust model)
* Keys can be securely and consistently used **across multiple AWS Regions**

You must use:

🔐 **AWS KMS Multi-Region Keys** — allow encryption/decryption operations across regions without re-encrypting or exporting keys.

🗄️ **Aurora Client-Side Encryption** — ensures the application encrypts the data **before** sending it to Aurora, so the database never sees plaintext.

This combination satisfies **multi-region replication**, **client-side confidentiality**, and **compliance** requirements.

</details>

---

### Question 5

You have generated a public SSL certificate using **Let’s Encrypt** and uploaded it to **AWS Certificate Manager (ACM)** so it can be attached to an Application Load Balancer that forwards traffic to EC2 instances. Since the certificate was generated **outside AWS**, ACM **does not support automatic renewal** for it. You want to be **notified 30 days before the certificate expires** so you can manually renew it.

**How can you set up this notification?** **Options**

* Configure ACM to send notifications by linking it to 3rd party certificate provider LetsEncrypt
* Configure EventBridge for Daily Expiration Events from ACM to invoke SNS notifications to your email
* Configure EventBridge for Monthly Expiration Events from ACM to invoke SNS notifications to your email
* Configure CloudWatch Alarms for Daily Expiration Events from ACM to invoke SNS notifications to your email

<details>
<summary>Explanation</summary>

✅ **Correct Answer**: ✔ **Configure EventBridge for Daily Expiration Events from ACM to invoke SNS notifications to your email**

ACM automatically tracks certificate expiration dates—even for **imported certificates** that it cannot auto-renew. To receive alerts before expiration:

* **ACM emits certificate expiration events** to **Amazon EventBridge**.
* You can create an **EventBridge rule** that detects certificates expiring within a configurable window (such as 30 days).
* The rule triggers an **SNS notification**, which sends an **email alert**.

This is the **most suitable** and AWS-recommended method because:

* ACM **cannot** notify you directly for third-party certificates.
* CloudWatch alarms cannot track ACM certificate expiration events.
* Monthly notifications are too infrequent for a 30-day renewal requirement.

Therefore, configuring **EventBridge + SNS** ensures **timely and automated notifications** so you can manually renew the certificate before it expires.

#### Daily Expiration Events vs Monthly Expiration Events

##### **Why Option 2 Is Correct**

ACM publishes **certificate expiration events DAILY** in Amazon EventBridge (formerly CloudWatch Events).
To ensure you never miss the notification — especially when the certificate is 30 days from expiry — you must configure a **daily rule** so EventBridge triggers **every day** and sends an SNS email alert.

This gives you multiple reminders and prevents a missed notification.

##### **Why Option 3 Is Incorrect**

❌ **Configure EventBridge for Monthly Expiration Events from ACM to invoke SNS notifications to your email**

There is **no monthly expiration event** for ACM certificates. Amazon ACM only publishes **daily events**, not monthly.
If you rely on a monthly trigger, you **may not get notified exactly 30 days before expiration**, risking certificate downtime.

##### **Summary Table**

| Option                                  | Works? | Reason                                                              |
| --------------------------------------- | :----: | ------------------------------------------------------------------- |
| **Daily EventBridge Rule (Option 2)**   |    ✅   | ACM emits **daily expiration events**, ensuring timely notification |
| **Monthly EventBridge Rule (Option 3)** |    ❌   | ACM **does not support monthly expiration events**                  |

</details>

---

### Question 6

You have created the main **Edge-Optimized API Gateway** in the **us-west-2** AWS Region. This main Edge-Optimized API Gateway forwards traffic to a second-level API Gateway in **ap-southeast-1**. You want to secure the main API Gateway by attaching an **ACM certificate** to it.

**In which AWS Region must you create the ACM certificate?** **Options**

* **us-west-2**
* **ap-southeast-1**
* **Both us-east-1 and us-west-2 works**
* **us-east-1** 

<details>
<summary>Explanation</summary>

✅ **Correct Answer**: ✔ **us-east-1**

Edge-Optimized API Gateways use a **CloudFront distribution behind the scenes** to route traffic from global edge locations. Since **CloudFront only supports ACM certificates stored in the `us-east-1` Region (N. Virginia)**, any certificate attached to an **Edge-Optimized API Gateway** must be created in **us-east-1**, regardless of where the API Gateway itself is deployed.

Therefore, even though your API Gateway is hosted in **us-west-2**, the ACM certificate **must** be provisioned in **us-east-1** for it to work.

</details>

---