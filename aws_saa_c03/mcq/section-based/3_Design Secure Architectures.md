# Design Secure Architectures

## Question 1

**1. An organization needs to control access to several Amazon S3 buckets. The organization plans to use a gateway endpoint to allow access to trusted buckets.**

**Which of the following could help you achieve this requirement?**

- 🅐 Generate a bucket policy for trusted VPCs.
- 🅑 Generate a bucket policy for trusted S3 buckets.
- 🅒 Generate an endpoint policy for trusted S3 buckets.
- 🅓 Generate an endpoint policy for trusted VPCs.

<details>
<summary><strong>Answer & Explanation</strong> 📝</summary>

✅ Correct Answer: 🅒 Generate an endpoint policy for trusted S3 buckets

### Explanation

An **endpoint policy** is attached to a **VPC endpoint**, not to a VPC or a bucket directly.
For **S3 VPC endpoints**, endpoint policies are used to **control which S3 buckets and actions are allowed through that endpoint**.

In this scenario, the requirement is to:

- Restrict access **via the VPC endpoint**
- Allow access **only to specific (trusted) S3 buckets**

That is exactly what an **S3 VPC endpoint policy** is designed for.

<img src="https://media.tutorialsdojo.com/public/s3-gateway-endpoint-policy-sample.jpg"
     alt="S3 gateway endpoint policy sample"
     width="600" />

### Why the other options are incorrect

- ❌ **Bucket policy for trusted VPCs**
  Bucket policies can restrict access by VPC or VPC endpoint, but they do not control _what the endpoint itself allows_. This is not the best fit when the requirement is endpoint-level control.

- ❌ **Bucket policy for trusted S3 buckets**
  Bucket policies are attached to buckets, not generated _for_ buckets as a trust mechanism.

- ❌ **Endpoint policy for trusted VPCs**
  Endpoint policies do not trust or reference VPCs. They define **which AWS resources (like S3 buckets)** can be accessed through the endpoint.

### Key takeaway 📌

- **Bucket policy** → controls _who/what can access the bucket_
- **Endpoint policy** → controls _what resources can be accessed through the VPC endpoint_

Since the requirement is about allowing access to **trusted S3 buckets via a VPC endpoint**, the correct choice is:

👉 **Generate an endpoint policy for trusted S3 buckets**

</details>

---

## Question 2

**A startup launched a new FTP server using an On-Demand EC2 instance in a newly created VPC with default settings. The server should not be accessible publicly but only through the IP address `175.45.116.100` and nowhere else.**

**Which of the following is the most suitable way to implement this requirement?**

- 🅐 Create a new Network ACL inbound rule in the subnet of the EC2 instance with:

  - Protocol: UDP
  - Port Range: 20–21
  - Source: 175.45.116.100/0
  - Allow/Deny: ALLOW

- 🅑 Create a new inbound rule in the security group of the EC2 instance with:

  - Protocol: TCP
  - Port Range: 20–21
  - Source: 175.45.116.100/32

- 🅒 Create a new inbound rule in the security group of the EC2 instance with:

  - Protocol: UDP
  - Port Range: 20–21
  - Source: 175.45.116.100/32

- 🅓 Create a new Network ACL inbound rule in the subnet of the EC2 instance with:

  - Protocol: TCP
  - Port Range: 20–21
  - Source: 175.45.116.100/0
  - Allow/Deny: ALLOW

<details>
<summary><strong>Answer & Explanation</strong> 📝</summary>

### ✅ Correct Answer: 🅑

**Create a new inbound rule in the security group of the EC2 instance with TCP, ports 20–21, source `175.45.116.100/32`.**

### Why this is correct

- **FTP uses TCP**, not UDP (control channel on port 21, data on port 20).
- **Security Groups are stateful** and are the **recommended first line of defense** for EC2 instances.
- Restricting the source to **`/32`** ensures **only that single IP address** can access the server.
- Default VPCs already allow outbound traffic, so no extra egress rules are needed.

### Why the other options are incorrect

- ❌ **Options using UDP**
  FTP does not work over UDP.

- ❌ **Options using Network ACLs**

  - NACLs are **stateless**, requiring both inbound and outbound rules.
  - They apply at the **subnet level**, not the instance level.
  - Less precise and more complex than security groups for this use case.

- ❌ **Source `175.45.116.100/0`**
  This CIDR is invalid for restricting to a single IP and would not meet the requirement.

### Key takeaway 📌

For **instance-specific, IP-restricted access**, always prefer **Security Groups** over NACLs—especially in a default VPC.

### TCP vs UDP (Quick Comparison)

| Feature            | **TCP (Transmission Control Protocol)**  | **UDP (User Datagram Protocol)**          |
| ------------------ | ---------------------------------------- | ----------------------------------------- |
| Connection         | Connection-oriented (handshake required) | Connectionless                            |
| Reliability        | **Reliable** – guarantees delivery       | **Unreliable** – no delivery guarantee    |
| Ordering           | Maintains packet order                   | No ordering                               |
| Error Handling     | Retransmission, error checking           | Minimal error checking, no retransmission |
| Speed              | Slower (overhead due to checks)          | Faster (low overhead)                     |
| Congestion Control | Yes                                      | No                                        |
| Use Cases          | Web, email, file transfer                | Streaming, gaming, VoIP, DNS              |

#### TCP – When reliability matters

* Ensures **all data arrives correctly and in order**
* Retransmits lost packets
* Examples:

  * 🌐 HTTP/HTTPS
  * 📧 SMTP, IMAP
  * 📁 FTP
  * 🧠 Database connections

#### UDP – When speed matters

* Sends data **without waiting for confirmation**
* Some packet loss is acceptable
* Examples:

  * 🎥 Video/audio streaming
  * 🎮 Online gaming
  * 📞 VoIP
  * 🔍 DNS queries

#### Simple analogy

* **TCP** = Phone call 📞 (confirming every message)
* **UDP** = Radio broadcast 📻 (send fast, no confirmation)

#### One-line summary

* **Use TCP** when you need accuracy
* **Use UDP** when you need speed

</details>

---

## Question 3

**A company has a web application that uses Amazon CloudFront to distribute its images, videos, and other static content stored in its Amazon S3 bucket to users around the world. The company has recently introduced a new member-only access feature for some of its high-quality media files. There is a requirement to provide access to multiple private media files only to paying subscribers without having to change the current URLs.**

**Which of the following is the most suitable solution to implement to satisfy this requirement?**

- 🅐 Use **Signed Cookies** to control who can access the private files in your CloudFront distribution by modifying your application to determine whether a user should have access to your content. For members, send the required `Set-Cookie` headers to the viewer which will unlock the content only to them.

- 🅑 Configure your CloudFront distribution to use **Match Viewer** as its Origin Protocol Policy which will automatically match the user request. This will allow access to the private content if the request is a paying member and deny it if it is not a member.

- 🅒 Configure your CloudFront distribution to use **Field-Level Encryption** to protect your private data and only allow access to members.

- 🅓 Create a **Signed URL** with a custom policy which only allows the members to see the private files.

<details>
<summary><strong>Answer & Explanation</strong> 📝</summary>

### ✅ Correct Answer: 🅐 Use Signed Cookies

### Explanation

The key requirements are:

- Restrict access to **multiple private media files**
- Allow access **only to paying subscribers**
- **Do not change existing URLs**

**Signed Cookies** are specifically designed for this use case.

- Signed cookies allow you to grant access to **multiple objects** using the **same URLs**.
- Your application authenticates the user (member vs non-member).
- For authorized users, the app sends **CloudFront signed cookies** (`CloudFront-Policy`, `CloudFront-Signature`, `CloudFront-Key-Pair-Id`) via `Set-Cookie`.
- CloudFront then allows access to the protected content **without modifying URLs**.

If you want to serve private content through CloudFront and you're trying to decide whether to use signed URLs or signed cookies, consider the following:

1. Signed URLs are best for the following cases:
   – You want to use an RTMP distribution. Signed cookies aren't supported for RTMP distributions. (Note: CloudFront removed RTMP distributions on 31 Dec 2020, so this point is history only.)
   – You want to restrict access to individual files, for example, an installation download for your application.
   – Your users are using a client (for example, a custom HTTP client) that doesn't support cookies.

2. Signed Cookies are best for the following cases:
   – You want to provide access to multiple restricted files, for example, all of the files for a video in HLS format or all of the files in the subscribers' area of a website.
   – You don't want to change your current URLs.

### Why the other options are incorrect

- ❌ **Match Viewer Origin Protocol Policy**

  - This setting only controls whether CloudFront uses HTTP or HTTPS to communicate with the origin. It has **nothing to do with authorization or membership checks**.
  - Match Viewer is an Origin Protocol Policy that configures CloudFront to communicate with your origin using HTTP or HTTPS, depending on the protocol of the viewer request. CloudFront caches the object only once even if viewers make requests using both HTTP and HTTPS protocols.

- ❌ **Field-Level Encryption**
  This protects sensitive form data (like credit card fields) in transit. It does **not control access to CloudFront content**.

- ❌ **Signed URLs**
  Signed URLs work well for **individual files**, but they require **changing the URL**, which violates the requirement.

### Key takeaway 📌

- **Signed URLs** → Best for _single, time-limited objects_
- **Signed Cookies** → Best for _multiple private objects without changing URLs_

👉 Since the requirement is **member-only access to multiple files without changing URLs**, **Signed Cookies** are the best solution.

</details>

---

## Question 4

**An application is hosted on an EC2 instance with multiple EBS Volumes attached and uses Amazon Neptune as its database. To improve data security, you encrypted all of the EBS volumes attached to the instance to protect the confidential data stored in the volumes.**

**Which of the following statements are true about encrypted Amazon Elastic Block Store (EBS) volumes? (Select TWO.)**

- 🅐 Only the data in the volume is encrypted and not all the data moving between the volume and the instance.
- 🅑 Snapshots are not automatically encrypted.
- 🅒 All data moving between the volume and the instance are encrypted.
- 🅓 The volumes created from the encrypted snapshot are not encrypted.
- 🅔 Snapshots are automatically encrypted.

<details>
<summary><strong>Answer & Explanation</strong> 📝</summary>

### ✅ Correct Answers: 🅒 and 🅔

#### 🅒 All data moving between the volume and the instance are encrypted

When you enable **EBS encryption**, AWS encrypts:

- Data **at rest** on the volume
- Data **in transit** between the EBS volume and the EC2 instance
- Data **at rest** in snapshots

This encryption is handled transparently by AWS using AWS KMS.

#### 🅔 Snapshots are automatically encrypted

- Any **snapshot taken from an encrypted EBS volume is automatically encrypted**.
- The snapshot uses the **same KMS key** as the source volume unless specified otherwise.

---

### ❌ Why the other options are incorrect

- ❌ **🅐 Only the data in the volume is encrypted**
  Incorrect — EBS encryption also covers **data in transit** between the instance and the volume.

- ❌ **🅑 Snapshots are not automatically encrypted**
  Incorrect — snapshots of encrypted volumes **are always encrypted automatically**.

- ❌ **🅓 Volumes created from encrypted snapshots are not encrypted**
  Incorrect — **any volume created from an encrypted snapshot is also encrypted by default**.

---

### Key takeaway 📌

EBS encryption provides **end-to-end protection**:

- ✅ Data at rest (volume + snapshots)
- ✅ Data in transit (instance ↔ volume)
- ✅ Automatic inheritance of encryption for snapshots and derived volumes

</details>

---

## Question 5

**A solutions architect is writing an AWS Lambda function that will process encrypted documents from an Amazon FSx for NetApp ONTAP file system. The documents are protected by an AWS KMS customer key. After processing the documents, the Lambda function will store the results in an S3 bucket with an Amazon S3 Glacier Flexible Retrieval storage class. The solutions architect must ensure that the files can be decrypted by the Lambda function.**

**Which action accomplishes the requirement?**

- 🅐 Attach the `kms:decrypt` permission to the Lambda function’s execution role. Add a statement to the AWS KMS key’s policy that grants the function’s execution role the `kms:decrypt` permission.
- 🅑 Attach the `kms:decrypt` permission to the Lambda function’s resource policy. Add a statement to the AWS KMS key’s policy that grants the function’s resource policy ARN the `kms:decrypt` permission.
- 🅒 Attach the `kms:decrypt` permission to the Lambda function’s resource policy. Add a statement to the AWS KMS key’s policy that grants the function’s execution role the `kms:decrypt` permission.
- 🅓 Attach the `kms:decrypt` permission to the Lambda function’s execution role. Add a statement to the AWS KMS key’s policy that grants the function’s ARN the `kms:decrypt` permission.

<details>
<summary><strong>Answer & Explanation</strong> 📝</summary>

### ✅ Correct Answer: 🅐

**Attach the `kms:decrypt` permission to the Lambda function’s execution role and grant the same permission to that execution role in the KMS key policy.**

### Explanation

When AWS Lambda needs to **decrypt data protected by a customer-managed KMS key**, two things must be true:

1. **IAM permission**

   - The Lambda **execution role** must allow `kms:Decrypt`.

2. **KMS key policy permission**

   - The **KMS key policy** must explicitly trust the **same execution role** and allow `kms:Decrypt`.

Lambda accesses encrypted data **using its execution role**, not:

- the Lambda function ARN, or
- the Lambda resource policy.

Because the documents in **FSx for NetApp ONTAP** are encrypted with a **customer-managed KMS key**, Lambda must be authorized at **both IAM and KMS policy levels**.

### Why the other options are incorrect

- ❌ **🅑 Resource policy + resource policy ARN**
  Lambda resource policies are for **invocation permissions**, not for service access to KMS.

- ❌ **🅒 Resource policy usage**
  KMS does not evaluate Lambda resource policies for cryptographic operations.

- ❌ **🅓 Granting the function ARN**
  KMS works with **IAM principals (roles/users)**, not Lambda function ARNs.

### Key takeaway 📌

For KMS-encrypted data access from Lambda:

- ✅ Grant `kms:Decrypt` to the **Lambda execution role**
- ✅ Allow the **same execution role** in the **KMS key policy**
- ❌ Do not use Lambda resource policies or function ARNs for KMS access

</details>

---

## Question 6

**A pharmaceutical company has resources hosted on both its on-premises network and in the AWS cloud. The company requires all Software Architects to access resources in both environments using on-premises credentials, which are stored in Active Directory.**

**In this scenario, which of the following can be used to fulfill this requirement?**

- 🅐 Use IAM users
- 🅑 Use Amazon VPC
- 🅒 Set up **SAML 2.0–Based Federation using a Microsoft Active Directory Federation Service (AD FS)**
- 🅓 Set up **SAML 2.0–Based Federation using a Web Identity Federation**

<details>
<summary><strong>Answer & Explanation</strong> 📝</summary>

### ✅ Correct Answer: 🅒

**Set up SAML 2.0–Based Federation using Microsoft Active Directory Federation Services (AD FS)**

### Comparison of the last two options

#### 🅒 SAML 2.0–Based Federation using Microsoft AD FS

- Integrates **directly with on-premises Active Directory**
- Allows users to authenticate using **existing AD credentials**
- Enables **single sign-on (SSO)** for both on-premises and AWS resources
- Designed for **enterprise identity federation**

➡️ **Correct choice** when credentials are stored in **Active Directory**

#### 🅓 SAML 2.0–Based Federation using Web Identity Federation

- Intended for **public identity providers** (such as Google, Facebook, Amazon Cognito)
- **Does not integrate natively** with on-premises Active Directory
- Commonly used for **external or consumer-facing applications**

➡️ **Not suitable** for enterprise AD-based authentication

### Key takeaway 📌

When users must access AWS using **existing on-premises Active Directory credentials**, the correct solution is:

👉 **SAML 2.0 federation with Microsoft AD FS**

</details>

---

## Question 7

**A company is designing a banking portal that uses Amazon ElastiCache for Redis as its distributed session management component. To secure session data and ensure that Cloud Engineers must authenticate before executing Redis commands, specifically `MULTI` / `EXEC` commands, the system should enforce strong authentication by requiring users to enter a password. Additionally, access should be managed with long-lived credentials while supporting robust security practices.**

**Which of the following actions should be taken to meet the above requirement?**

- 🅐 Enable the in-transit encryption for Redis replication groups.
- 🅑 Set up a Redis replication group and enable the `AtRestEncryptionEnabled` parameter.
- 🅒 Authenticate the users using Redis AUTH by creating a new Redis cluster with both the `--transit-encryption-enabled` and `--auth-token` parameters enabled.
- 🅓 Generate an IAM authentication token using AWS credentials and provide this token as a password.

<details>
<summary><strong>Answer & Explanation</strong> 📝</summary>

### ✅ Correct Answer: 🅒

**Authenticate the users using Redis AUTH by creating a new Redis cluster with both the `--transit-encryption-enabled` and `--auth-token` parameters enabled.**

### Explanation

The requirements explicitly state:

- Redis commands (including `MULTI` / `EXEC`) must require **password-based authentication**
- Credentials should be **long-lived**
- Strong security practices must be enforced

**Redis AUTH** in Amazon ElastiCache directly satisfies these needs:

- The **AUTH token** acts as a **password** required before executing Redis commands.
- The token is **long-lived** (unlike temporary IAM credentials).
- **In-transit encryption** ensures the password and data are protected over the network.
- Redis AUTH is enforced **before transactional commands** (`MULTI` / `EXEC`) can be used.

<img src="https://media.tutorialsdojo.com/ElastiCache-Redis-Secure-Compliant.png"
     alt="ElastiCache Redis AUTH secure architecture"
     width="600" />

### Why the other options are incorrect

- ❌ **🅐 In-transit encryption only**
  Encrypts traffic but **does not enforce authentication**.

- ❌ **🅑 At-rest encryption only**
  Protects stored data but **does not control client authentication**.

- ❌ **🅓 IAM authentication token**
  ElastiCache **does** support IAM authentication (Valkey 7.2+ / Redis OSS 7.0+, with in-transit encryption on). But the IAM token is valid for only **15 minutes**, so it is short-lived. The question asks for **long-lived** credentials, so Redis AUTH is still the answer.

### Key takeaway 📌

For **Amazon ElastiCache for Redis**, when you need:

- Password-based authentication
- Support for Redis transactional commands
- Long-lived credentials

👉 **Use Redis AUTH with in-transit encryption enabled**.

</details>

---

## Question 8

**A Solutions Architect is working for a fast-growing startup that just started operations during the past 3 months. They currently have an on-premises Active Directory and 10 computers. To save costs in procuring physical workstations, they decided to deploy virtual desktops for their new employees in a virtual private cloud (VPC) in AWS. The new cloud infrastructure should leverage the existing security controls in AWS but can still communicate with their on-premises network.**

**Which set of AWS services will the Architect use to meet these requirements?**

- 🅐 AWS Directory Services, VPN connection, and Amazon S3
- 🅑 AWS Directory Services, VPN connection, and ClassicLink
- 🅒 AWS Directory Services, VPN connection, and AWS Identity and Access Management
- 🅓 AWS Directory Services, VPN connection, and Amazon WorkSpaces

<details>
<summary><strong>Answer & Explanation</strong> 📝</summary>

### ✅ Correct Answer: 🅓

**AWS Directory Services, VPN connection, and Amazon WorkSpaces**

### Explanation

The requirements are:

- **Virtual desktops** for new employees
- Integration with **on-premises Active Directory**
- Ability to **communicate with the on-premises network**
- Use of **existing AWS security controls**

This is exactly what **Amazon WorkSpaces** is designed for.

- **Amazon WorkSpaces** provides managed **virtual desktop infrastructure (VDI)** in AWS.
- **AWS Directory Service** integrates WorkSpaces with the existing **on-premises Active Directory**.
- A **VPN connection** allows secure connectivity between AWS and the on-premises network.
- AWS-managed security (network isolation, encryption, IAM integration) is leveraged automatically.

<img src="https://media.tutorialsdojo.com/public/standardedition_usecases_1017_large.png"
     alt="Amazon WorkSpaces architecture"
     width="600" />

### Why the other options are incorrect

- ❌ **Amazon S3**
  Object storage, not a virtual desktop solution.

- ❌ **ClassicLink**
  Deprecated and unrelated to virtual desktops.

- ❌ **IAM instead of WorkSpaces**
  IAM manages access, not user desktops.

### Key takeaway 📌

When the requirement is **cost-effective virtual desktops integrated with on-prem AD**, the correct AWS service is:

👉 **Amazon WorkSpaces**, combined with **AWS Directory Service** and a **VPN connection**.

</details>

---

## Question 9

**A media company needs to configure an Amazon S3 bucket to serve static assets for a public-facing web application. Which methods ensure that all of the objects uploaded to the S3 bucket can be read publicly all over the Internet? (Select TWO.)**

* 🅐 Configure the cross-origin resource sharing (CORS) of the S3 bucket to allow objects to be publicly accessible from all domains.
* 🅑 Grant public read access to the object when uploading it using the S3 Console.
* 🅒 Create an IAM role to set the objects inside the S3 bucket to public read.
* 🅓 Configure the S3 bucket policy to set all objects to public read.
* 🅔 Do nothing. Amazon S3 objects are already public by default.

<details>
<summary><strong>Answer & Explanation</strong> 📝</summary>

### ✅ Correct Answers: 🅑 and 🅓

---

### 🅑 Grant public read access to the object when uploading it using the S3 Console

* When uploading objects, you can explicitly grant **public read (READ for Everyone)**.
* This makes each uploaded object **individually accessible over the Internet**.
* Works immediately for static assets like images, CSS, and JavaScript files.

---

### 🅓 Configure the S3 bucket policy to set all objects to public read

* A **bucket policy** can grant `s3:GetObject` permission to `Principal: *`.
* This ensures **all objects in the bucket** are publicly readable.
* This is the **most scalable and consistent approach** for public static websites.

---

### ❌ Why the other options are incorrect

* <span style="color:red"><strong>❌ 🅐 CORS configuration</strong></span>  
  CORS controls **browser cross-origin requests**, not public access permissions.  
  Objects must already be public for CORS to matter.

* <span style="color:red"><strong>❌ 🅒 IAM role</strong></span>  
  IAM roles control **who can manage AWS resources**, not anonymous public access.

* <span style="color:red"><strong>❌ 🅔 Objects are public by default</strong></span>  
  S3 objects are **private by default**, not public.

---

### Key takeaway 📌

To make S3 objects publicly readable over the Internet:

* ✅ Use **object-level public read permissions**
* ✅ Or apply a **bucket policy granting public `GetObject` access**
* ❌ CORS and IAM alone do **not** make objects public

> ⚠️ Note (2026): Since **April 2023**, every new bucket has **S3 Block Public Access** on and **ACLs disabled** (Object Ownership = bucket owner enforced). So both answers work only after you turn off Block Public Access, and the per-object public-read option also needs ACLs turned back on. The exam answer is still 🅑 and 🅓.

</details>

---

## Question 10

**An organization stores and manages financial records of various companies in its on-premises data center, which is almost out of space. The management decided to move all of their existing records to a cloud storage service. All future financial records will also be stored in the cloud. For additional security, all records must be prevented from being deleted or overwritten.**

**Which of the following should you do to meet the above requirement?**

* 🅐 Use AWS Storage Gateway to establish hybrid cloud storage. Store all of your data in Amazon S3 and enable Object Lock.
* 🅑 Use AWS DataSync to move the data. Store all of your data in Amazon S3 and enable Object Lock.
* 🅒 Use AWS Storage Gateway to establish hybrid cloud storage. Store all of your data in Amazon EBS and enable Object Lock.
* 🅓 Use AWS DataSync to move the data. Store all of your data in Amazon EFS and enable Object Lock.

<details>
<summary><strong>Answer & Explanation</strong> 📝</summary>

### ✅ Correct Answer: 🅑

**Use AWS DataSync to move the data. Store all of your data in Amazon S3 and enable Object Lock.**

### Explanation

The key requirements are:

* Migrate **existing on-premises data** to the cloud
* Store **all future records** in the cloud
* Prevent records from being **deleted or overwritten** (immutability)

**Amazon S3 Object Lock** is specifically designed for this purpose:

* Provides **WORM (Write Once, Read Many)** protection
* Prevents object deletion or overwrite for a defined retention period
* Commonly used for **financial, legal, and compliance data**

**AWS DataSync** is the most suitable service for:

* Efficiently **moving large datasets** from on-premises storage to Amazon S3
* High-performance, secure, and managed data transfer

AWS DataSync allows you to copy large datasets with millions of files without having to build custom solutions with open-source tools or licenses and manage expensive commercial network acceleration software. You can use DataSync to migrate active data to AWS, transfer data to the cloud for analysis and processing, archive data to free up on-premises storage capacity or replicate data to AWS for business continuity.

AWS DataSync enables you to migrate your on-premises data to Amazon S3, Amazon EFS, and Amazon FSx for Windows File Server. You can configure DataSync to make an initial copy of your entire dataset and schedule subsequent incremental transfers of changing data toward Amazon S3. Enabling S3 Object Lock prevents your existing and future records from being deleted or overwritten.

AWS DataSync is primarily used to migrate existing data to Amazon S3. On the other hand, AWS Storage Gateway is more suitable if you still want to retain access to the migrated data and for ongoing updates from your on-premises file-based applications.

### Why the other options are incorrect

* ❌ **🅐 Storage Gateway + S3 Object Lock**
  - Storage Gateway is used when you need **ongoing hybrid access** from on-premises systems. The requirement is a **full migration**, not hybrid access.
  - The scenario requires that all of the existing records must be migrated to AWS. The future records will also be stored in AWS and not in the on-premises network. This means that setting up hybrid cloud storage is not necessary since the on-premises storage will no longer be used.

* ❌ **🅒 Amazon EBS with Object Lock**
  Object Lock is **only supported by Amazon S3**, not EBS.

* ❌ **🅓 Amazon EFS with Object Lock**
  Object Lock is **not supported by EFS**.

### Key takeaway 📌

For **immutable, compliance-grade storage** of financial records in the cloud:

* ✅ Use **Amazon S3 with Object Lock**
* ✅ Use **AWS DataSync** for large-scale on-premises data migration

👉 This combination best satisfies scalability, security, and compliance requirements.

</details>

---

