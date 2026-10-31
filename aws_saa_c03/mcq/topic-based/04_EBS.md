# EBS

## **Question 1**

**A company is implementing its Business Continuity Plan. As part of this initiative, the IT Director instructed the IT team to set up an automated backup of all the Amazon EBS volumes attached to the company’s Amazon EC2 instances. The solution must be implemented as soon as possible and should be both cost-effective and simple to maintain.**

What is the fastest and most cost-effective solution to automatically back up all of the EBS volumes?

**Options:**

* For an automated solution, create a scheduled job that calls the “create-snapshot” command via the AWS CLI to take a snapshot of production EBS volumes periodically.
* Set your Amazon Storage Gateway with EBS volumes as the data source and store the backups in your on-premises servers through the storage gateway.
* Use an EBS snapshot retention rule in AWS Backup to automatically manage snapshot retention and expiration.
* Use Amazon Data Lifecycle Manager (Amazon DLM) to automate the creation of EBS snapshots.

<details>

<summary><strong>Answer & Explanation</strong> 📝</summary>

🟢 **Amazon Data Lifecycle Manager (DLM)** automates the **creation**, **retention**, and **deletion** of **Amazon Elastic Block Store (EBS) snapshots**.
It simplifies **EBS volume management** by allowing you to define **policies** that govern the **lifecycle of snapshots**, ensuring **regular backups** are created and **obsolete snapshots** are automatically removed.

You can use **Amazon Data Lifecycle Manager (Amazon DLM)** to automate the creation, retention, and deletion of snapshots taken to back up your **Amazon EBS volumes**.
Automating snapshot management helps you to:

- 🟢 **Protect valuable data** by enforcing a regular backup schedule.
- 🟢 **Retain backups** as required by **auditors or internal compliance.**
- 🟢 **Reduce storage costs** by deleting outdated backups.

Combined with the monitoring features of **Amazon EventBridge** and **AWS CloudTrail**, Amazon DLM provides a **complete backup solution** for EBS volumes **at no additional cost.**

✅ **Hence, the correct answer is:**
🟢 **Use Amazon Data Lifecycle Manager (Amazon DLM) to automate the creation of EBS snapshots.**

<img src="https://media.tutorialsdojo.com/public/TD-Data-Lifecycle-Manager-08-22-2025.png"
     alt="EBS Data Lifecycle Manager diagram"
     width="600" />

🔴 **The option that says:**
*For an automated solution, create a scheduled job that calls the "create-snapshot" command via the AWS CLI to take a snapshot of production EBS volumes periodically*
is <span style="color:red"><strong>incorrect ❌</strong></span> because even though it is a **valid approach**, it requires **manual setup** and **scripting effort** to create and schedule snapshot jobs.

Using **Amazon DLM** provides a **faster**, **fully managed**, and **code-free** way to automate **EBS snapshot creation, retention, and deletion** without the need for **custom scripts** or **cron jobs.**

🔴 **The option that says:**
*Set your Amazon Storage Gateway with EBS volumes as the data source and store the backups in your on-premises servers through the storage gateway*
is <span style="color:red"><strong>incorrect ❌</strong></span> because **Amazon Storage Gateway** is used for **backing up data from on-premises servers to AWS**, not for creating backups of **EBS volumes within the AWS Cloud**.
It does **not** serve as a backup tool for **Amazon VPC or EBS** resources.

🔴 **The option that says:**
*Use an EBS snapshot retention rule in AWS Backup to automatically manage snapshot retention and expiration*
is <span style="color:red"><strong>incorrect ❌</strong></span> because while **AWS Backup** can manage **retention rules**, it is designed for **centralized backup** across **multiple AWS services**, not solely for **EBS snapshot automation**.
It is therefore **less cost-effective** and **not the most straightforward** solution for this specific **EBS snapshot automation use case.**

</details>

---

## **Question 2**

**A company plans to migrate all of their applications to AWS. The Solutions Architect suggested storing all the data in EBS volumes. The Chief Technical Officer is worried that EBS volumes are not appropriate for the existing workloads due to compliance requirements, downtime scenarios, and IOPS performance.**

Which of the following are valid points in proving that EBS is the best service to use for migration? (Select TWO.)

**Options:**

* EBS volumes support live configuration changes while in production, which means that you can modify the volume type, volume size, and IOPS capacity without service interruptions.
* An EBS volume is off-instance storage that can persist independently from the life of an instance.
* EBS volumes can be attached to any EC2 instance in any Availability Zone.
* Amazon EBS provides the ability to create snapshots (backups) of any EBS volume and write a copy of the data in the volume to Amazon RDS, where it is stored redundantly in multiple Availability Zones.
* When you create an EBS volume in an Availability Zone, it is automatically replicated on a separate AWS Region to prevent data loss due to a failure of any single hardware component.

<details>

<summary><strong>Answer & Explanation</strong> 📝</summary>

The correct answers are:

- ✅ EBS volumes support live configuration changes while in production, which means that you can modify the volume type, volume size, and IOPS capacity without service interruptions.
- ✅ An EBS volume is off-instance storage that can persist independently from the life of an instance.

🟢 **Amazon EBS (Elastic Block Store)** volume is a **durable, block-level storage device** that you can attach to a **single EC2 instance.**
You can use **EBS volumes** as **primary storage** for data that requires **frequent updates**, such as the **system drive** for an instance or **database application storage**.
They are also useful for **throughput-intensive applications** that perform **continuous disk scans.**
📌 **EBS volumes persist independently** from the running life of an EC2 instance.

#### 🟢 **Important information about EBS Volumes**

✅ **1. Automatic replication within the same Availability Zone**
When you create an **EBS volume** in an **Availability Zone**, it is **automatically replicated within that zone** to prevent data loss due to a failure of any single hardware component.

✅ **2. Same AZ attachment rule**
After you create a volume, you can **attach it to any EC2 instance in the same Availability Zone.**

✅ **3. Multi-Attach support (for io1 and io2 volumes only)**
**Amazon EBS Multi-Attach** enables you to attach a single **Provisioned IOPS SSD (io1 or io2)** volume to **multiple Nitro-based instances** that are in the **same Availability Zone.**
⚠️ Other EBS types **are not supported** for Multi-Attach.

✅ **4. Persistent storage**
An **EBS volume** is **off-instance storage** that can **persist independently** from the life of an instance.
You can specify **not to terminate** the EBS volume when you terminate the EC2 instance during instance creation.

✅ **5. Live configuration changes**
EBS volumes support **live configuration changes** while in production.
You can modify the **volume type**, **volume size**, and **IOPS capacity** **without service interruption.**

✅ **6. Encryption**
**Amazon EBS encryption** uses **256-bit Advanced Encryption Standard (AES-256)** algorithms to secure your data.

✅ **7. SLA**
**EBS SLA:** 99.99% uptime per Region and 99.9% per volume. **Durability:** 99.8%–99.9% for most volume types, and 99.999% for io2 Block Express.

### ✅ **Correct Answer**

When you create an EBS volume in an **Availability Zone**, it is **automatically replicated within that same zone**, ensuring durability and fault tolerance against single-hardware failures.

### ❌ **Incorrect Options**

🚫 **Option:**
"When you create an EBS volume in an Availability Zone, it is automatically replicated on a separate AWS region to prevent data loss..."
<span style="color:red"><strong>**Incorrect ❌**</strong></span> — Replication happens **within the same Availability Zone**, **not across regions.**

🚫 **Option:**
"EBS volumes can be attached to any EC2 instance in any Availability Zone."
<span style="color:red"><strong>**Incorrect ❌**</strong></span> — EBS volumes can only be attached to **EC2 instances in the same Availability Zone.**

🚫 **Option:**
"Amazon EBS provides the ability to create snapshots of any EBS volume and write a copy of the data in the volume to Amazon RDS..."
<span style="color:red"><strong>**Incorrect ❌**</strong></span> — EBS **snapshots** are actually stored in **Amazon S3**, **not** Amazon RDS.

🟢 **Correct statement:**
Amazon EBS provides the ability to **create snapshots (backups)** of any EBS volume and store them **redundantly in Amazon S3 across multiple Availability Zones.**

</details>

---

## **Question 3**

**A company has several unencrypted Amazon EBS snapshots in its Amazon VPC. The Solutions Architect must ensure that all of the new EBS volumes restored from the unencrypted snapshots are automatically encrypted.**

What should be done to accomplish this requirement?

**Options:**

* Launch new EBS volumes and specify the symmetric encryption AWS KMS keys for encryption.
* Launch new EBS volumes and encrypt them using asymmetric AWS KMS keys.
* Enable the EBS Encryption By Default feature for specific EBS volumes.
* Enable the EBS Encryption By Default feature for the AWS Region.

<details>

<summary><strong>Answer & Explanation</strong> 📝</summary>

🟢 **You can configure your AWS account** to **enforce encryption** of new **EBS volumes** and **snapshot copies** that you create.
For example, **Amazon EBS** encrypts the **EBS volumes** created when you **launch an instance** and the **snapshots** that you copy from an **unencrypted snapshot**.

📌 **Encryption by default** has **no effect on existing EBS volumes or snapshots.**

#### 🟢 **Important considerations in EBS encryption**

✅ **1. Region-specific setting**
**Encryption by default** is a **Region-specific** configuration.
Once enabled for a Region, you **cannot disable** it for **individual volumes or snapshots** in that Region.

✅ **2. Instance type requirement**
When you enable **encryption by default**, you can **launch an instance only if** the **instance type supports EBS encryption.**

✅ **3. KMS key support**
**Amazon EBS does not support asymmetric KMS keys.**

✅ **4. KMS key association**
You **cannot change the KMS key** associated with an existing **snapshot** or **encrypted volume**.
However, you **can specify a different KMS key** during a **snapshot copy operation**, so that the **copied snapshot** is encrypted using the **new KMS key.**

✅ **5. Encrypting existing unencrypted resources**
Although there is **no direct way** to encrypt an existing **unencrypted volume** or **snapshot**, you can do so by **creating a new volume or snapshot** from it.

* If **encryption by default** is enabled, the **new resource** is automatically **encrypted** with the **default EBS key**.
* If **not enabled**, you can **manually enable encryption** when creating the new volume or snapshot.
  In both cases, you can **override the default key** and use **symmetric KMS keys** for encryption.

✅ **Hence, the correct answer is:**
🟢 **Enable the EBS Encryption By Default feature for the AWS Region.**

<img src="https://media.tutorialsdojo.com/EBS_Encryption_By_Default.png"
     alt="EBS encryption by default diagram"
     width="600" />

🔴 **The option that says:**
*Launch new EBS volumes and encrypt them using asymmetric AWS KMS keys*
is <span style="color:red"><strong>incorrect ❌</strong></span> because **Amazon EBS does not support asymmetric KMS keys.**
To encrypt an **EBS snapshot**, you must use **symmetric encryption KMS keys.**

🔴 **The option that says:**
*Launch new EBS volumes and specify the symmetric encryption AWS KMS keys for encryption*
is <span style="color:red"><strong>incorrect ❌</strong></span> because although it enables encryption, this process is **manual** and can lead to **inconsistencies**, leaving some volumes **unencrypted**.
🟢 A better approach is to **enable EBS Encryption By Default**, ensuring **all new EBS volumes and snapshots** are **automatically encrypted.**

🔴 **The option that says:**
*Enable the EBS Encryption By Default feature for specific EBS volumes*
is <span style="color:red"><strong>incorrect ❌</strong></span> because the **Encryption By Default** feature is **Region-wide**.
You **cannot enable it selectively** for individual EBS volumes — it applies to **all EBS resources in that Region.**

</details>

---

## **Question 4**

**A technical lead of the Cloud Infrastructure team was consulted by a software developer regarding the required AWS resources of the web application that he is building. The developer knows that an Instance Store only provides ephemeral storage where the data is automatically deleted when the instance is terminated. To ensure that the data of the web application persists, the app should be launched in an EC2 instance that has a durable, block-level storage volume attached. The developer knows that they need to use an EBS volume, but they are not sure what type they need to use.**

In this scenario, which of the following is true about Amazon EBS volume types and their respective usage? (Select TWO.)

**Options:**

* Provisioned IOPS volumes offer storage with consistent and low-latency performance, and are designed for I/O intensive applications such as large relational or NoSQL databases.
* General Purpose SSD (gp3) volumes with multi-attach enabled offer consistent and low-latency performance, and are designed for applications requiring multi-AZ resiliency.
* Spot volumes provide the lowest cost per gigabyte of all EBS volume types and are ideal for workloads where data is accessed infrequently, and applications where the lowest storage cost is important.
* Magnetic volumes provide the lowest cost per gigabyte of all EBS volume types and are ideal for workloads where data is accessed infrequently, and applications where the lowest storage cost is important.
* Single root I/O virtualization (SR-IOV) volumes are suitable for a broad range of workloads, including small to medium-sized databases, development and test environments, and boot volumes.

<details>

<summary><strong>Answer & Explanation</strong> 📝</summary>

🟢 **Amazon EBS** provides **three volume types** to best meet the needs of your workloads:
**General Purpose (SSD)**, **Provisioned IOPS (SSD)**, and **Magnetic.**

#### 🟢 **1. General Purpose (SSD)**

**General Purpose (SSD)** is the **new, SSD-backed, general-purpose EBS volume type** and is **recommended as the default choice** for most customers.
It is suitable for a **broad range of workloads**, including:

* Small to medium-sized databases
* Development and test environments
* Boot volumes

💡 **Balanced price and performance** make it ideal for general use cases.

#### 🟢 **2. Provisioned IOPS (SSD)**

**Provisioned IOPS (SSD)** volumes offer **consistent and low-latency performance**, designed for **I/O-intensive applications** such as:

* Large **relational databases** (e.g., Oracle, MySQL, PostgreSQL)
* **NoSQL databases** (e.g., MongoDB, Cassandra)

📈 These volumes allow you to **provision a specific number of IOPS**, ensuring **predictable throughput** and **performance consistency**.

#### 🟢 **3. Magnetic**

**Magnetic volumes** provide the **lowest cost per gigabyte** of all EBS volume types.
They are ideal for workloads where:

* Data is **accessed infrequently**
* Applications prioritize **low storage cost** over performance

⚠️ **Note:** Magnetic is a **Previous Generation Volume type**.
The **latest low-cost storage options** are:

* **Cold HDD (sc1)** – lowest-cost storage for infrequently accessed data
* **Throughput Optimized HDD (st1)** – optimized for streaming and sequential I/O workloads

> ⚠️ Note (2026): This answer key comes from an older question. Today AWS lists **sc1 at $0.015 per GB-month**, and Magnetic is a previous-generation type. If sc1 (Cold HDD) is an option, it is the lowest-cost choice.

✅ **Hence, the correct answers are:**

- 🟢 **– Provisioned IOPS (SSD)** volumes offer storage with **consistent and low-latency performance**, and are designed for **I/O-intensive applications** such as **large relational or NoSQL databases.**
- 🟢 **– Magnetic volumes** provide the **lowest cost per gigabyte** of all EBS volume types and are ideal for **workloads where data is accessed infrequently** and **cost efficiency is a priority.**

### ❌ **Incorrect Options**

- 🚫 **Option:** *Spot volumes provide the lowest cost per gigabyte of all EBS volume types...*
**Incorrect** ❌ — There is **no EBS type** called a **"Spot volume."**
However, there is a **Spot Instance purchasing option** for **EC2 instances**, which is unrelated to EBS storage types.
- 🚫 **Option:** *General Purpose SSD (gp3) volumes with multi-attach enabled offer consistent and low-latency performance...*
**Incorrect** ❌ — The **multi-attach** feature is only supported on **Provisioned IOPS (io1 or io2 Block Express)** volumes.
Also, **multi-attach does not provide multi-AZ resiliency**, as it allows attachment to **multiple instances within the same Availability Zone** only.
- 🚫 **Option:** *Single root I/O virtualization (SR-IOV) volumes are suitable for a broad range of workloads...*
**Incorrect** ❌ — **SR-IOV** refers to **Enhanced Networking** on EC2 instances, not to **EBS volumes.**
It improves **network performance**, not **block storage performance.**

</details>

---