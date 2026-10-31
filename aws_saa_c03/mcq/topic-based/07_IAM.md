# IAM

## **Question 1**

**A company has an application that continually sends encrypted documents to Amazon S3. The company requires that the configuration for data access is in line with its strict compliance standards. It should also be alerted if there is any risk of unauthorized access or suspicious access patterns.**

Which step is needed to meet the requirements?

**Options:**

- Use AWS CloudTrail to monitor and detect access patterns on S3.
- Use Amazon GuardDuty to monitor malicious activity on S3.
- Use Amazon Inspector to alert whenever a security violation is detected on S3.
- Use Amazon Rekognition to monitor and recognize patterns on S3.

<details>

<summary><strong>Answer & Explanation</strong> 📝</summary>

🟢 **Amazon GuardDuty Overview**

**Amazon GuardDuty** is a **threat detection service** that continuously monitors your AWS environment for **malicious activity**, **unauthorized behavior**, and **potential security threats**.
It helps identify compromised resources and suspicious patterns by analyzing **AWS CloudTrail events**, **VPC Flow Logs**, and **DNS logs**.

### 🟢 **GuardDuty and Amazon S3 Monitoring**

With its **S3 protection feature**, **GuardDuty** can detect a variety of suspicious activities related to **Amazon S3 buckets**, such as:

✅ **Requests from known malicious IP addresses** attempting to access or modify S3 data.
✅ **Unusual or anomalous API call patterns** (e.g., repeated `ListBuckets` or `GetBucketAcl` requests).
✅ **Changes to bucket policies or ACLs** that might **expose S3 buckets publicly**.
✅ **Data exfiltration attempts**, where data is being accessed or transferred outside of expected patterns.

GuardDuty uses a combination of:

- 🤖 **Machine Learning** (for anomaly detection),
- 🔍 **Anomaly Detection algorithms**, and
- ⚔️ **Continuously updated threat intelligence feeds** (from AWS and third-party sources).

These allow it to **detect potential threats in real-time**, helping you **prevent data leaks and compromise**.

### ✅ **Correct Answer**

🟢 **Use Amazon GuardDuty to monitor malicious activity on S3.**

📌 **Why this is correct:**
GuardDuty is purpose-built for **continuous monitoring** and **threat detection** on AWS accounts, including **Amazon S3 buckets**, making it the most suitable and automated solution for identifying malicious access and configuration changes.

### 🔴 **Incorrect Options**

🚫 **Option:** _Use Amazon Rekognition to monitor and recognize patterns on S3._
❌ **Reason:**
**Amazon Rekognition** is a **computer vision service** used for analyzing **images and videos** — it detects objects, people, text, and inappropriate content.
It **cannot monitor access patterns or detect threats** on S3.

🚫 **Option:** _Use AWS CloudTrail to monitor and detect access patterns on S3._
❌ **Reason:**
While **CloudTrail** logs **API calls and account activity**, it is designed for **auditing and compliance**, not real-time **threat detection**.
GuardDuty actually **analyzes CloudTrail data** under the hood to detect suspicious activity.

🚫 **Option:** _Use Amazon Inspector to alert whenever a security violation is detected on S3._
❌ **Reason:**
**Amazon Inspector** performs **vulnerability assessments** and **security configuration checks** on **EC2**, **ECR**, and **Lambda functions**, not on **S3**.
It does not monitor for **malicious activity or behavioral anomalies**.

#### 🧠 **Summary**

| AWS Service               | Primary Purpose                                                | Detects S3 Threats?      |
| ------------------------- | -------------------------------------------------------------- | ------------------------ |
| 🟢 **Amazon GuardDuty**   | Threat detection using ML, anomaly detection, and threat intel | ✅ Yes                   |
| 🔵 **AWS CloudTrail**     | Audit API calls and account activity                           | ⚠️ Indirectly (via logs) |
| 🔵 **Amazon Inspector**   | Automated vulnerability assessment                             | ❌ No                    |
| 🔵 **Amazon Rekognition** | Image and video analysis                                       | ❌ No                    |

✅ **Final Answer:**
**Use Amazon GuardDuty to monitor malicious activity on S3.**

</details>

---

## **Question 2**

A tech company that you are working for has undertaken a Total Cost Of Ownership (TCO) analysis evaluating the use of Amazon S3 versus acquiring more storage hardware. The result was that all 1200 employees would be granted access to use Amazon S3 for the storage of their personal documents.

Which of the following will you need to consider so you can set up a solution that incorporates a single sign-on feature from your corporate AD or LDAP directory and also restricts access for each individual user to a designated user folder in an S3 bucket? (Select TWO.)

**Options:**

- Set up a Federation proxy or an Identity provider, and use AWS Security Token Service to generate temporary tokens.
- Map each individual user to a designated user folder in S3 using Amazon WorkDocs to access their personal documents.
- Configure an IAM role and an IAM Policy to access the bucket.
- Use 3rd party Single Sign-On solutions such as Atlassian Crowd, OKTA, OneLogin and many others.
- Set up a matching IAM user for each of the 1200 users in your corporate directory that needs access to a folder in the S3 bucket.

<details>
  <summary><strong>Answer & Explanation</strong> 📝</summary>

🟢 **Temporary credentials in AWS** are commonly used for scenarios involving:

- **Identity federation**
- **Delegation**
- **Cross-account access**
- **IAM roles**

This scenario specifically requires **enterprise identity federation** with **Single Sign-On (SSO)**, where users authenticate using their **existing corporate directory credentials (AD / LDAP)** and then access AWS resources **without** creating separate IAM users.

### 🟢 **Correct Answers**

### ✅ **1. Set up a federation proxy or an identity provider (IdP), and use AWS STS to generate temporary tokens**

- Use your existing **corporate identity system** (like **Microsoft AD / LDAP**).
- Authenticate users internally.
- Use **AWS Security Token Service (STS)** to issue **temporary credentials** via:

  - `AssumeRole`
  - `AssumeRoleWithSAML`
  - `GetFederationToken`

- This allows users to access AWS securely using **time-limited credentials**, without IAM user proliferation.

### ✅ **2. Configure an IAM Role and an IAM Policy to access the S3 bucket**

- Create an **IAM Role** in the AWS account.
- Attach a policy allowing required S3 permissions (e.g., `s3:GetObject`).
- Federated users **assume this role**, gaining temporary permissions.

### 🟢 **Why This Is the Right Solution**

✔ Users authenticate via **existing corporate credentials**
✔ No need to create separate IAM users for 1,200 employees
✔ Fully supports SSO via **SAML 2.0**
✔ **STS temporary credentials** offer enhanced security
✔ Simplifies permission management using IAM roles
✔ No external systems required—AWS has native support for SAML federation

### 🔴 **Incorrect Options Explained**

<span style="color:red"><strong>### ❌ Using 3rd-party SSO solutions such as OKTA, OneLogin, Atlassian Crowd</strong></span>

- Unnecessary, because **AWS already supports SAML 2.0** natively.
- The scenario doesn't mention any requirement to use external IdPs.

<span style="color:red"><strong>### ❌ Mapping each user to a folder via Amazon WorkDocs</strong></span>

- WorkDocs **does not integrate directly with S3**.
- It is a content collaboration service, not a mechanism for controlling access to S3 buckets.
- Amazon WorkDocs was also shut down on 25 Apr 2025, so it is not a valid choice today.

<span style="color:red"><strong>### ❌ Creating matching IAM users for all 1200 corporate users</strong></span>

- Impractical and unscalable.
- These users should authenticate using **existing AD credentials**, not IAM credentials.
- Violates best practices and increases management overhead.

<img src="https://media.tutorialsdojo.com/saml-based-federation.diagram.png"
     alt="SAML-based federation diagram"
     width="600" />

### 🧠 **Summary**

| Requirement                           | Correct AWS Solution              |
| ------------------------------------- | --------------------------------- |
| SSO using corporate AD / LDAP         | 🟢 SAML 2.0 federation with AD FS |
| Access AWS without creating IAM users | 🟢 STS temporary credentials      |
| Secure access to S3                   | 🟢 IAM Role + IAM Policy          |
| Avoid 3rd-party tools                 | 🟢 Use AWS-native SAML support    |

### ✅ **Final Answer**

**Set up a Federation proxy or Identity Provider and use STS to generate temporary tokens. Configure an IAM role and IAM policy for S3 access.**

</details>

---