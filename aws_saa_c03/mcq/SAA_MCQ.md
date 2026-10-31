# SAA MCQ

## Question 1

**A retail company uses AWS Cloud to manage its IT infrastructure. The company has set up AWS Organizations to manage several departments running their AWS accounts and using resources such as Amazon EC2 instances and Amazon RDS databases. The company wants to provide shared and centrally managed VPCs to all departments using applications that need a high degree of interconnectivity.
As a solutions architect, which of the following options would you choose to facilitate this use case?**

**Options:**

- Use VPC peering to share a VPC with other AWS accounts belonging to the same parent organization from AWS Organizations
- Use VPC sharing to share a VPC with other AWS accounts belonging to the same parent organization from AWS Organizations
- Use VPC sharing to share one or more subnets with other AWS accounts belonging to the same parent organization from AWS Organizations
- Use VPC peering to share one or more subnets with other AWS accounts belonging to the same parent organization from AWS Organizations

<details>
  <summary><strong>Answer & Explanation</strong> 📝</summary>

### ✅ Correct Answer

**Use VPC sharing to share one or more subnets with other AWS accounts belonging to the same parent organization from AWS Organizations**

**Note:** AWS **VPC Sharing** (via AWS Resource Access Manager) allows a **central account** to own and manage the VPC, while **participant accounts** can deploy resources into shared **subnets**. This is the recommended approach for centrally managed networking with high interconnectivity.

### 🧠 Why this is correct

- VPC sharing enables **centralized VPC ownership and control**
- Departments can launch EC2, RDS, etc., directly into shared subnets
- No need for complex routing or multiple VPC connections
- Ideal for organizations using **AWS Organizations** with shared infrastructure

### ❌ Why the other options are incorrect

- <span style="color:red"><strong>Use VPC peering to share a VPC with other AWS accounts belonging to the same parent organization</strong></span>
  VPC peering does **not** share a VPC. It only connects **separate VPCs** and still requires routing, CIDR planning, and scaling considerations.

- <span style="color:red"><strong>Use VPC sharing to share a VPC with other AWS accounts belonging to the same parent organization</strong></span>
  VPC sharing does **not share the entire VPC**. Only **specific subnets** are shared with participant accounts.

- <span style="color:red"><strong>Use VPC peering to share one or more subnets with other AWS accounts belonging to the same parent organization</strong></span>
  VPC peering cannot share subnets or VPC resources. It only provides network connectivity between VPCs.

### 🧠 Summary

- **VPC Sharing** = centralized networking, shared subnets, high interconnectivity ✅
- **VPC Peering** = point-to-point VPC connectivity, not resource sharing ❌

</details>

---

## Question 2

**The business analytics team at a company has been running ad-hoc queries on Oracle and PostgreSQL services on Amazon RDS to prepare daily reports for senior management. To facilitate business analytics reporting, the engineering team now wants to continuously replicate this data and consolidate these databases into a petabyte-scale data warehouse by streaming data to Amazon Redshift.
As a solutions architect, which of the following would you recommend as the MOST resource-efficient solution that requires the LEAST amount of development time without the need to manage the underlying infrastructure?**

**Options:**

- Use AWS EMR to replicate the data from the databases into Amazon Redshift
- Use AWS Database Migration Service (AWS DMS) to replicate the data from the databases into Amazon Redshift
- Use Amazon Kinesis Data Streams to replicate the data from the databases into Amazon Redshift
- Use AWS Glue to replicate the data from the databases into Amazon Redshift

<details>
  <summary><strong>Answer & Explanation</strong> 📝</summary>

### ✅ Correct Answer

**Use AWS Database Migration Service (AWS DMS) to replicate the data from the databases into Amazon Redshift**

**Hint:** Look for a **managed, low-code service** that supports **continuous replication (CDC)** from relational databases directly into Amazon Redshift.

### 🧠 Why this is correct

- **AWS DMS** is a fully managed service that supports **continuous data replication (Change Data Capture)** from Oracle and PostgreSQL
- It can stream data directly into **Amazon Redshift**
- Requires **minimal development effort**
- No need to manage servers or infrastructure
- Designed specifically for **database migration and ongoing replication**

This perfectly matches the requirements of:

- Least development time
- High resource efficiency
- No infrastructure management
- Continuous replication

### ❌ Why the other options are incorrect

- <span style="color:red"><strong>Use AWS EMR to replicate the data from the databases into Amazon Redshift</strong></span>
  EMR requires cluster management, custom Spark/Hive jobs, and higher operational overhead. It is not the fastest or simplest option.

- <span style="color:red"><strong>Use Amazon Kinesis Data Streams to replicate the data from the databases into Amazon Redshift</strong></span>
  Kinesis is designed for streaming event data, not native database replication. You would need custom producers, consumers, and transformation logic.

- <span style="color:red"><strong>Use AWS Glue to replicate the data from the databases into Amazon Redshift</strong></span>
  AWS Glue is mainly used for batch ETL jobs, not continuous CDC-based replication. It also requires job configuration and scheduling.

### 🧠 Summary

- **AWS DMS** → Best for continuous DB replication with minimal effort ✅
- **EMR / Glue / Kinesis** → Higher complexity, more development, and operational overhead ❌

</details>

---

## Question 3

**An Electronic Design Automation (EDA) application produces massive volumes of data that can be divided into two categories. The _hot data_ needs to be both processed and stored quickly in a parallel and distributed fashion. The _cold data_ needs to be kept for reference with quick access for reads and updates at a low cost.
Which of the following AWS services is BEST suited to accelerate the aforementioned chip design process?**

**Options:**

- Amazon FSx for Windows File Server
- Amazon FSx for Lustre
- Amazon EMR
- AWS Glue

<details>
  <summary><strong>Answer & Explanation</strong> 📝</summary>

### ✅ Correct Answer

**Amazon FSx for Lustre**

**Hint:** EDA workloads are classic **high-performance computing (HPC)** use cases that demand **high throughput, low latency, and parallel file access**.

### 🧠 Why this is correct

- **Amazon FSx for Lustre** is a **high-performance, parallel file system** purpose-built for HPC workloads such as **EDA, machine learning, and scientific simulations**
- Optimized for **very high throughput and low latency**, ideal for _hot data_
- Can integrate seamlessly with **Amazon S3**, allowing _cold data_ to be stored cost-effectively while still being quickly accessible
- Supports **massively parallel read/write operations**, which significantly accelerates chip design workflows

This perfectly matches:

- Parallel & distributed processing of hot data
- Low-cost storage with fast access for cold data
- Acceleration of compute-heavy EDA workloads

### ❌ Why the other options are incorrect

- <span style="color:red"><strong>Amazon FSx for Windows File Server – Incorrect</strong></span>
  Designed for Windows-based applications and SMB file sharing, not for high-throughput, parallel HPC workloads.

- <span style="color:red"><strong>Amazon EMR – Incorrect</strong></span>
  EMR is a big data processing service (Spark, Hadoop). It is compute-focused and **not optimized as a high-performance shared file system** for EDA workloads.

- <span style="color:red"><strong>AWS Glue – Incorrect</strong></span>
  Glue is a serverless ETL service for data integration and analytics, not suitable for low-latency, parallel file access required by chip design applications.

### 🧠 Summary

- **Amazon FSx for Lustre** → Best fit for EDA & HPC workloads ✅
- **FSx for Windows, EMR, Glue** → Not designed for parallel, low-latency file system needs ❌

</details>

---

## Question 4

**The engineering team at an e-commerce company wants to migrate from Amazon Simple Queue Service (Amazon SQS) Standard queues to FIFO (First-In-First-Out) queues with batching.
As a solutions architect, which of the following steps would you have in the migration checklist? (Select THREE)**

**Options:**

- Make sure that the name of the FIFO (First-In-First-Out) queue is the same as the standard queue
- Make sure that the name of the FIFO (First-In-First-Out) queue ends with the `.fifo` suffix
- Make sure that the throughput for the target FIFO (First-In-First-Out) queue does not exceed 300 messages per second
- Delete the existing standard queue and recreate it as a FIFO (First-In-First-Out) queue
- Make sure that the throughput for the target FIFO (First-In-First-Out) queue does not exceed 3,000 messages per second
- Convert the existing standard queue into a FIFO (First-In-First-Out) queue

<details>
  <summary><strong>Answer & Explanation</strong> 📝</summary>

### ✅ Correct Answer

The correct steps are:

- **Make sure that the name of the FIFO (First-In-First-Out) queue ends with the `.fifo` suffix**
- **Delete the existing standard queue and recreate it as a FIFO (First-In-First-Out) queue**
- **Make sure that the throughput for the target FIFO (First-In-First-Out) queue does not exceed 3,000 messages per second**

**Note:** SQS does **not** allow converting a Standard queue into a FIFO queue. FIFO queues have strict naming and throughput rules.

### 🧠 Why these are correct

- **`.fifo` suffix requirement**
  FIFO queues must have names ending with `.fifo`. Without this suffix, AWS will treat the queue as a Standard queue.

- **Recreate instead of convert**
  Standard queues **cannot be converted** to FIFO queues. Migration requires creating a new FIFO queue and updating producers/consumers.

- **3,000 messages per second throughput**
  FIFO queues support:

  - Up to **300 messages/second** _without batching_
  - Up to **3,000 messages/second with batching**
    Since the question explicitly mentions **FIFO with batching**, the correct limit is **3,000 messages per second**.

> ⚠️ Note (2026): 300 / 3,000 is the limit **without** high throughput mode. A FIFO queue with **high throughput mode** turned on goes much higher (up to 70,000 transactions per second in the largest Regions). The exam answer above is still 3,000.

### ❌ Why the other options are incorrect

- <span style="color:red"><strong>Make sure that the name of the FIFO queue is the same as the standard queue</strong></span>
  Queue names must be **unique**, and FIFO queues additionally require the `.fifo` suffix.

- <span style="color:red"><strong>Make sure that the throughput does not exceed 300 messages per second</strong></span>
  This limit applies **only when batching is NOT used**. With batching enabled, FIFO queues can scale to 3,000 messages per second.

- <span style="color:red"><strong>Convert the existing standard queue into a FIFO queue</strong></span>
  SQS does **not** support converting queue types. A new FIFO queue must be created.

### 🧠 Summary

- FIFO queues require a **`.fifo` suffix**
- Standard → FIFO requires **recreation**, not conversion
- With **batching**, FIFO queues support up to **3,000 messages/second** ✅

</details>

---

## Question 5

**A mobile gaming company is experiencing heavy read traffic to its Amazon Relational Database Service (Amazon RDS) database that retrieves player scores and stats. The company is using an Amazon RDS database instance type that is not cost-effective for their budget. The company would like to implement a strategy to deal with the high volume of read traffic, reduce latency, and also downsize the instance size to cut costs.
Which of the following solutions do you recommend?**

**Options:**

- Setup Amazon ElastiCache in front of Amazon RDS
- Move to Amazon Redshift
- Switch application code to AWS Lambda for better performance
- Setup Amazon RDS Read Replicas

<details>
  <summary><strong>Answer & Explanation</strong> 📝</summary>

### ✅ Correct Answer

**Setup Amazon ElastiCache in front of Amazon RDS**

**Hint:** When the workload is **read-heavy**, data is frequently accessed, and **low latency + cost reduction** are primary goals, caching is usually the best first solution.

### 🧠 Why this is correct

- **Amazon ElastiCache (Redis or Memcached)** offloads frequent read requests from RDS
- Provides **microsecond-level latency**, ideal for gaming use cases (player scores, stats)
- Reduces load on the RDS instance, allowing you to **downsize the DB instance** and save cost
- Frequently accessed data can be served directly from memory instead of hitting the database

This directly satisfies all requirements:

- Handle high read traffic ✅
- Reduce latency ✅
- Reduce database instance cost ✅

### ❌ Why the other options are incorrect

- <span style="color:red"><strong>Move to Amazon Redshift – Incorrect</strong></span>
  Amazon Redshift is a **data warehouse** for analytics and OLAP workloads, not for low-latency transactional reads required by gaming applications.

- <span style="color:red"><strong>Switch application code to AWS Lambda – Incorrect</strong></span>
  Lambda does not reduce database read load by itself. The bottleneck remains the RDS reads.

- <span style="color:red"><strong>Setup Amazon RDS Read Replicas – Incorrect</strong></span>
  Read replicas can help scale reads, but they **increase cost** by adding more DB instances and do not reduce latency as effectively as in-memory caching. They also do not help significantly with downsizing the primary instance.

### 🧠 Summary

- **ElastiCache + RDS** → Best for read-heavy, low-latency, cost-optimized workloads ✅
- **Read Replicas** → Scale reads but increase cost ❌
- **Redshift / Lambda** → Not suitable for this use case ❌

</details>

---

## Question 6

**A leading online gaming company is migrating its flagship application to AWS Cloud to deliver online games to users worldwide. The company wants to use a Network Load Balancer (NLB) to handle millions of requests per second. The engineering team has provisioned multiple instances in a public subnet and specified these instance IDs as the targets for the NLB.
As a solutions architect, which routing mechanism does the NLB use for these target instances?**

**Options:**

- Traffic is routed to instances using the primary public IP address specified in the primary network interface for the instance
- Traffic is routed to instances using the primary elastic IP address specified in the primary network interface for the instance
- Traffic is routed to instances using the instance ID specified in the primary network interface for the instance
- Traffic is routed to instances using the primary private IP address specified in the primary network interface for the instance

<details>
  <summary><strong>Answer & Explanation</strong> 📝</summary>

### ✅ Correct Answer

**Traffic is routed to instances using the primary private IP address specified in the primary network interface for the instance**

**Note:** When you register **instance IDs** as targets for a **Network Load Balancer**, AWS internally routes traffic to the **primary private IP address** of the instance’s primary network interface (ENI).

### 🧠 Why this is correct

- NLB operates at **Layer 4 (TCP/UDP)** and routes traffic directly to targets
- When **instance ID** is used as the target type:

  - NLB resolves the instance to its **primary private IP**

- Public IPs or Elastic IPs are **not used** for backend routing
- This works the same whether instances are in **public or private subnets**

### ❌ Why the other options are incorrect

- <span style="color:red"><strong>Traffic is routed using the primary public IP address</strong></span>
  NLB does not forward traffic to public IPs. Public IPs are for internet access, not internal load balancer routing.

- <span style="color:red"><strong>Traffic is routed using the primary elastic IP address</strong></span>
  Elastic IPs are not used by NLB for routing to instance targets.

- <span style="color:red"><strong>Traffic is routed using the instance ID</strong></span>
  Instance ID is only an identifier. The actual network routing is done via the **private IP**.

### 🧠 Summary

- **NLB + instance targets** → Routes traffic using **primary private IP** ✅
- Public IP / Elastic IP → Not used for backend routing ❌
- Instance ID → Identifier only, not the routing address ❌

</details>

---

## Question 7

**A media agency stores its re-creatable assets on Amazon Simple Storage Service (Amazon S3) buckets. The assets are accessed by a large number of users for the first few days, and access frequency drops significantly after a week. The assets are still accessed occasionally afterward but must remain immediately accessible when required. Storage costs on Amazon S3 are becoming expensive, and the agency wants to reduce costs as much as possible.**
**As an AWS Certified Solutions Architect – Associate, which solution would you suggest to lower storage costs while fulfilling the business requirements?**

**Options:**

- Configure a lifecycle policy to transition the objects to Amazon S3 Standard-Infrequent Access (S3 Standard-IA) after 7 days
- Configure a lifecycle policy to transition the objects to Amazon S3 One Zone-Infrequent Access (S3 One Zone-IA) after 30 days
- Configure a lifecycle policy to transition the objects to Amazon S3 One Zone-Infrequent Access (S3 One Zone-IA) after 7 days
- Configure a lifecycle policy to transition the objects to Amazon S3 Standard-Infrequent Access (S3 Standard-IA) after 30 days

<details>
  <summary><strong>Answer & Explanation</strong> 📝</summary>

### ✅ Correct Answer

**Configure a lifecycle policy to transition the objects to Amazon S3 One Zone-Infrequent Access (S3 One Zone-IA) after 30 days**

**Hint:** The assets are **re-creatable**, accessed infrequently after the first week, but must remain **immediately accessible** at a **lower cost**.

### 🧠 Why this is correct

- **S3 One Zone-IA** is significantly cheaper than S3 Standard and S3 Standard-IA
- Designed for **infrequently accessed, re-creatable data**
- Still provides **millisecond access**, meeting the “immediately accessible” requirement
- Lifecycle transitions to IA storage classes are recommended **after 30 days**, aligning with AWS best practices

This option minimizes storage cost while meeting durability and access needs.

![S3 Standard to IA](https://assets-pt.media.datacumulus.com/aws-saa-pt/assets/pt1-q8-i1.jpg)

### ❌ Why the other options are incorrect

- <span style="color:red"><strong>S3 Standard-IA after 7 days – Incorrect</strong></span>
  Transitioning after only 7 days does not align with AWS lifecycle best practices and offers less cost savings than One Zone-IA.

- <span style="color:red"><strong>S3 One Zone-IA after 7 days – Incorrect</strong></span>
  S3 IA storage classes are intended for objects stored for **at least 30 days**. Transitioning earlier can result in higher costs due to minimum storage duration charges.

- <span style="color:red"><strong>S3 Standard-IA after 30 days – Incorrect</strong></span>
  Although valid, **Standard-IA is more expensive** than One Zone-IA and provides multi-AZ durability that is unnecessary for **re-creatable assets**.

### 🧠 Summary

- Re-creatable + infrequently accessed + immediate access → **S3 One Zone-IA** ✅
- Use **30-day lifecycle transition** to avoid minimum storage charges
- Choose the **lowest-cost storage class** that still meets access requirements

</details>

---

## Question 8

**A Big Data analytics company writes data and log files in Amazon S3 buckets. The company now wants to stream the existing data files as well as any ongoing file updates from Amazon S3 to Amazon Kinesis Data Streams.
As a Solutions Architect, which of the following would you suggest as the FASTEST possible way of building a solution for this requirement?**

**Options:**

- Leverage AWS Database Migration Service (AWS DMS) as a bridge between Amazon S3 and Amazon Kinesis Data Streams
- Configure Amazon EventBridge events for the bucket actions on Amazon S3. An AWS Lambda function can then be triggered from the Amazon EventBridge event that will send the necessary data to Amazon Kinesis Data Streams
- Leverage Amazon S3 event notification to trigger an AWS Lambda function for the file create event. The AWS Lambda function will then send the necessary data to Amazon Kinesis Data Streams
- Amazon S3 bucket actions can be directly configured to write data into Amazon Simple Notification Service (Amazon SNS). Amazon SNS can then be used to send the updates to Amazon Kinesis Data Streams

<details>
  <summary><strong>Answer & Explanation</strong> 📝</summary>

### ✅ Correct Answer

**Leverage AWS Database Migration Service (AWS DMS) as a bridge between Amazon S3 and Amazon Kinesis Data Streams**

**Note:** The key requirement is to stream **both existing files and ongoing updates** from Amazon S3 with the **fastest implementation and least custom development**.

### 🧠 Why this is correct

- **AWS DMS** supports **Amazon S3 as a source** and **Amazon Kinesis Data Streams as a target**
- Can migrate **existing objects** and also stream **ongoing changes**
- Fully managed service with **minimal setup and no custom code**
- Fastest way to meet the requirement compared to building event-driven pipelines

This aligns perfectly with:

- Existing data + continuous updates
- Speed of implementation
- Minimal operational and development effort

### ❌ Why the other options are incorrect

- <span style="color:red"><strong>Amazon S3 event notification + AWS Lambda – Incorrect</strong></span>
  S3 event notifications only trigger on **new object events**. They cannot process **existing objects** already stored in the bucket without additional custom logic.

- <span style="color:red"><strong>Amazon EventBridge + AWS Lambda – Incorrect</strong></span>
  Similar to S3 notifications, EventBridge captures **events**, not historical data. Additional scripting would be required to backfill existing files.

- <span style="color:red"><strong>Amazon SNS integration – Incorrect</strong></span>
  SNS cannot directly stream data into Kinesis Data Streams. Extra processing layers would still be needed.

### 🧠 Summary

- **AWS DMS** → Fastest, managed solution for existing + ongoing S3 data streaming ✅
- **Event-driven approaches** → Handle only new events, need extra development ❌
- **SNS-based solutions** → Not designed for direct Kinesis streaming ❌

</details>

---

## Question 9

**You have been hired as a Solutions Architect to advise a company on the various authentication/authorization mechanisms that AWS offers to authorize an API call within Amazon API Gateway. The company prefers a solution that offers built-in user management.
Which of the following solutions would you suggest as the best fit for this use case?**

**Options:**

- Use Amazon Cognito Identity Pools
- Use AWS Lambda authorizer for Amazon API Gateway
- Use AWS IAM authorization
- Use Amazon Cognito User Pools

<details>
  <summary><strong>Answer & Explanation</strong> 📝</summary>

### ✅ Correct Answer

**Use Amazon Cognito User Pools**

**Hint:** The phrase **“built-in user management”** is the most important clue in this question.

### 🧠 Why this is correct

- **Amazon Cognito User Pools** provide:

  - Built-in **user sign-up and sign-in**
  - User directories and profile management
  - Authentication using **JWT tokens**

- API Gateway can **directly integrate** with Cognito User Pools for authorization
- No custom authentication logic or infrastructure is required
- Ideal for securing APIs accessed by **end users** (web or mobile apps)

This exactly matches the requirement of:

- API authorization
- Built-in user management
- Minimal custom development

### ❌ Why the other options are incorrect

- <span style="color:red"><strong>Amazon Cognito Identity Pools – Incorrect</strong></span>
  Identity Pools are used to grant **temporary AWS credentials** to access AWS services, not for managing users or API authentication directly.

- <span style="color:red"><strong>AWS Lambda authorizer – Incorrect</strong></span>
  Lambda authorizers require **custom code** for authentication and user handling. They do not provide built-in user management.

- <span style="color:red"><strong>AWS IAM authorization – Incorrect</strong></span>
  IAM authorization is suited for **service-to-service** or AWS principal access, not end-user authentication with user management.

### 🧠 Summary

- **Cognito User Pools** → Built-in user management + API Gateway integration ✅
- **Identity Pools** → AWS credential federation ❌
- **Lambda authorizer** → Custom auth logic required ❌
- **IAM authorization** → Not suitable for end-user authentication ❌

</details>

---

## Question 10

**A weather forecast agency collects key weather metrics across multiple cities in the US and sends this data as key-value pairs to AWS Cloud every minute.
As a solutions architect, which of the following AWS services would you use to build a solution for processing and then reliably storing this data with high availability? (Select TWO)**

**Options:**

- Amazon Redshift
- Amazon ElastiCache
- Amazon RDS
- AWS Lambda
- Amazon DynamoDB

<details>
  <summary><strong>Answer & Explanation</strong> 📝</summary>

### ✅ Correct Answers

- **AWS Lambda**
- **Amazon DynamoDB**

### 🧠 Why these are correct

- **AWS Lambda**

  - Ideal for **processing incoming data streams** at a one-minute frequency
  - Fully serverless → no infrastructure to manage
  - Automatically scales with incoming requests
  - Highly available by default

- **Amazon DynamoDB**

  - Purpose-built for **key-value data models**
  - Provides **single-digit millisecond latency** at any scale
  - Fully managed and **highly available across multiple Availability Zones**
  - Perfect for storing frequent, small, structured updates like weather metrics

Together, Lambda handles **processing**, and DynamoDB provides **durable, highly available storage**.

### ❌ Why the other options are incorrect

- <span style="color:red"><strong>Amazon Redshift – Incorrect</strong></span>
  Redshift is a data warehouse designed for **analytics and batch querying**, not high-frequency key-value ingestion.

- <span style="color:red"><strong>Amazon ElastiCache – Incorrect</strong></span>
  ElastiCache is an **in-memory cache**, not intended for durable or long-term storage.

- <span style="color:red"><strong>Amazon RDS – Incorrect</strong></span>
  RDS is a relational database and introduces unnecessary schema management and scaling overhead for simple key-value data.

### 🧠 Summary

- **AWS Lambda** → Serverless processing with automatic scaling ✅
- **Amazon DynamoDB** → Highly available, durable key-value storage ✅
- **Redshift / RDS / ElastiCache** → Not suited for this ingestion pattern ❌

</details>

---

## Question 11

**A media company wants to get out of the business of owning and maintaining its IT infrastructure. As part of this digital transformation, the company wants to archive about 5 petabytes of data in its on-premises data center to durable long-term storage.
As a solutions architect, what is your recommendation to migrate this data in the MOST cost-optimal way?**

**Options:**

- Setup AWS Direct Connect between the on-premises data center and AWS Cloud. Use this connection to transfer the data into Amazon S3 Glacier
- Transfer the on-premises data into multiple AWS Snowball Edge Storage Optimized devices. Copy the AWS Snowball Edge data into Amazon S3 and create a lifecycle policy to transition the data into Amazon S3 Glacier
- Setup AWS Site-to-Site VPN connection between the on-premises data center and AWS Cloud. Use this connection to transfer the data into Amazon S3 Glacier
- Transfer the on-premises data into multiple AWS Snowball Edge Storage Optimized devices. Copy the AWS Snowball Edge data into Amazon S3 Glacier

<details>
  <summary><strong>Answer & Explanation</strong> 📝</summary>

### ✅ Correct Answer

**Transfer the on-premises data into multiple AWS Snowball Edge Storage Optimized devices. Copy the AWS Snowball Edge data into Amazon S3 and create a lifecycle policy to transition the data into Amazon S3 Glacier**

**Note:** The keywords **“5 petabytes”**, **“archive”**, and **“most cost-optimal”** are critical here.

### 🧠 Why this is correct

- **AWS Snowball Edge Storage Optimized** is designed for **large-scale offline data transfers** (tens of TBs to PBs)
- Shipping physical devices is **far cheaper and faster** than transferring 5 PB over the network
- Data must first land in **Amazon S3** (Snowball cannot write directly to Glacier)
- An **S3 lifecycle policy** is the recommended and cost-optimal way to transition archived data into **Amazon S3 Glacier** for long-term storage

This approach minimizes:

- Network transfer costs
- Migration time
- Operational complexity

### ❌ Why the other options are incorrect

- <span style="color:red"><strong>AWS Direct Connect to S3 Glacier – Incorrect</strong></span>
  Direct Connect is expensive to provision and operate, and transferring **5 PB** over the network is not cost-optimal for a one-time migration.

- <span style="color:red"><strong>AWS Site-to-Site VPN to S3 Glacier – Incorrect</strong></span>
  VPN has limited bandwidth and would take an impractically long time to transfer petabytes of data.

- <span style="color:red"><strong>Snowball Edge → S3 Glacier directly – Incorrect</strong></span>
  Snowball Edge **cannot write directly to Glacier**. Data must first be copied to **Amazon S3**, then transitioned using a lifecycle policy.

### 🧠 Summary

- **Petabyte-scale migration** → Use **AWS Snowball Edge** ✅
- **Archival storage** → Use **Amazon S3 Glacier via lifecycle policies** ✅
- **Network-based transfers** → Not cost-effective at this scale ❌

> ⚠️ Note (2026): Since **7 Nov 2025**, AWS Snowball Edge is available only to existing customers. New customers should use AWS DataSync (online) or AWS Data Transfer Terminal (physical). Older exam questions may still expect Snowball Edge.

</details>

---

## Question 12

**A company has hired you as an AWS Certified Solutions Architect – Associate to help with redesigning a real-time data processor. The company wants to build custom applications that process and analyze streaming data for its specialized needs.
Which solution will you recommend to address this use case?**

**Options:**

- Use Amazon Simple Notification Service (Amazon SNS) to process the data streams as well as decouple the producers and consumers for the real-time data processor
- Use Amazon Kinesis Data Streams to process the data streams as well as decouple the producers and consumers for the real-time data processor
- Use Amazon Simple Queue Service (Amazon SQS) to process the data streams as well as decouple the producers and consumers for the real-time data processor
- Use Amazon Kinesis Data Firehose to process the data streams as well as decouple the producers and consumers for the real-time data processor

<details>
  <summary><strong>Answer & Explanation</strong> 📝</summary>

### ✅ Correct Answer

**Use Amazon Kinesis Data Streams to process the data streams as well as decouple the producers and consumers for the real-time data processor**

**Hint:** The requirement mentions **custom applications**, **real-time processing**, and **streaming data**.

### 🧠 Why this is correct

- **Amazon Kinesis Data Streams (KDS)** is designed for **real-time streaming data ingestion**
- Allows multiple **custom consumers** to independently read and process the same stream
- Provides **fine-grained control** over shard scaling, data retention, and processing logic
- Naturally **decouples producers and consumers**, enabling specialized real-time analytics

This makes KDS the best fit for building **custom, real-time stream-processing applications**.

### ❌ Why the other options are incorrect

- <span style="color:red"><strong>Amazon SNS – Incorrect</strong></span>
  SNS is a **pub/sub messaging service** for notifications and fan-out, not for ordered, replayable real-time stream processing.

- <span style="color:red"><strong>Amazon SQS – Incorrect</strong></span>
  SQS is a **queueing service** for asynchronous message processing, not true streaming. Messages are consumed once and cannot be replayed by multiple consumers.

- <span style="color:red"><strong>Amazon Kinesis Data Firehose – Incorrect</strong></span>
  Firehose is for **fully managed data delivery** to destinations like S3, Redshift, or OpenSearch. It does **not** support custom, real-time stream processing logic.

### 🧠 Summary

- **Kinesis Data Streams** → Custom, real-time streaming + multiple consumers ✅
- **SNS / SQS** → Messaging, not streaming ❌
- **Kinesis Firehose** → Delivery service, not custom processing ❌

</details>

---

## Question 13

**A retail company wants to roll out and test a blue-green deployment for its global application in the next 48 hours. Most customers use mobile phones, which are prone to Domain Name System (DNS) caching. The company has only two days left before the annual Thanksgiving sale begins.
As a Solutions Architect, which of the following options would you recommend to test the deployment on as many users as possible in the given time frame?**

**Options:**

- Use Amazon Route 53 weighted routing to spread traffic across different deployments
- Use Elastic Load Balancing (ELB) to distribute traffic across deployments
- Use AWS Global Accelerator to distribute a portion of traffic to a particular deployment
- Use AWS CodeDeploy deployment options to choose the right deployment

<details>
  <summary><strong>Answer & Explanation</strong> 📝</summary>

### ✅ Correct Answer

**Use AWS Global Accelerator to distribute a portion of traffic to a particular deployment**

**Hint:** The key constraint here is **DNS caching on mobile devices** combined with a **very short rollout window (48 hours)**.

### 🧠 Why this is correct

- **AWS Global Accelerator** routes traffic using **static anycast IP addresses**, not DNS lookups
- This bypasses DNS caching issues common on mobile devices
- Allows **instant traffic shifting** (blue → green) without waiting for DNS TTLs to expire
- Ensures the **maximum number of users** can be tested within a short timeframe

This makes it ideal for **fast blue-green testing** during critical, time-sensitive launches.

### ❌ Why the other options are incorrect

- <span style="color:red"><strong>Amazon Route 53 weighted routing – Incorrect</strong></span>
  Route 53 relies on DNS. Due to DNS caching (especially on mobile networks), many users may continue hitting the old deployment for hours or days.

- <span style="color:red"><strong>Elastic Load Balancing (ELB) – Incorrect</strong></span>
  ELB can distribute traffic only **after traffic reaches it**. It does not solve DNS caching at the client side.

- <span style="color:red"><strong>AWS CodeDeploy deployment options – Incorrect</strong></span>
  CodeDeploy manages deployment strategies but does **not control client traffic routing** at a global scale.

### 🧠 Summary

- **AWS Global Accelerator** → Immediate traffic shifting, no DNS caching issues ✅
- **Route 53 weighted routing** → Impacted by DNS caching ❌
- **ELB / CodeDeploy** → Do not solve global traffic routing problems ❌

</details>

---

## Question 14

**A big data analytics company is using Amazon Kinesis Data Streams (KDS) to process IoT data from field devices. Multiple consumer applications read from the same stream, and engineers have noticed a performance lag in data delivery speed between producers and consumers.
As a solutions architect, which of the following would you recommend to improve performance for this use case?**

**Options:**

- Swap out Amazon Kinesis Data Streams with Amazon Kinesis Data Firehose
- Swap out Amazon Kinesis Data Streams with Amazon SQS Standard queues
- Use Enhanced Fanout feature of Amazon Kinesis Data Streams
- Swap out Amazon Kinesis Data Streams with Amazon SQS FIFO queues

<details>
  <summary><strong>Answer & Explanation</strong> 📝</summary>

### ✅ Correct Answer

**Use Enhanced Fanout feature of Amazon Kinesis Data Streams**

**Hint:** The key clue is **multiple consumer applications** reading from the same Kinesis stream.

### 🧠 Why this is correct

- **Enhanced Fan-Out (EFO)** provides **dedicated throughput per consumer** (up to 2 MB/sec per shard per consumer)
- Consumers receive data via **push-based delivery**, reducing latency
- Eliminates the need for consumers to compete for shard throughput
- Ideal for **high-performance, multi-consumer** streaming architectures

This directly addresses the observed **lag between producers and consumers**.

### ❌ Why the other options are incorrect

- <span style="color:red"><strong>Swap out with Kinesis Data Firehose – Incorrect</strong></span>
  Firehose is designed for **data delivery** to destinations (S3, Redshift, OpenSearch), not for real-time multi-consumer processing.

- <span style="color:red"><strong>Swap out with Amazon SQS Standard queues – Incorrect</strong></span>
  SQS does not support multiple consumers reading the **same stream of data** independently.

- <span style="color:red"><strong>Swap out with Amazon SQS FIFO queues – Incorrect</strong></span>
  FIFO queues ensure ordering but have **lower throughput** and are not suitable for high-volume streaming data.

### 🧠 Summary

- **Kinesis Enhanced Fan-Out** → Dedicated throughput + low latency per consumer ✅
- **Firehose / SQS** → Different use cases, not streaming fan-out ❌

</details>

---

## Question 15

**A financial services company is looking to move its on-premises IT infrastructure to AWS Cloud. The company has multiple long-term server-bound licenses across the application stack, and the CTO wants to continue using those licenses while moving to AWS.
As a solutions architect, which of the following would you recommend as the MOST cost-effective solution?**

**Options:**

- Use Amazon EC2 Reserved Instances (RI)
- Use Amazon EC2 On-Demand instances
- Use Amazon EC2 Dedicated Hosts
- Use Amazon EC2 Dedicated Instances

<details>
  <summary><strong>Answer & Explanation</strong> 📝</summary>

### ✅ Correct Answer

**Use Amazon EC2 Dedicated Hosts**

**Note:** The critical phrase here is **“server-bound licenses”** (also known as **bring-your-own-license – BYOL**).

### 🧠 Why this is correct

- **Amazon EC2 Dedicated Hosts** provide **physical servers dedicated to a single customer**
- They give you **visibility and control over the underlying hardware**, including:

  - Physical socket
  - Core count
  - Host ID

- This is required by many **server-bound and core-based licensing models** (e.g., Oracle, Windows Server, SQL Server with strict terms)
- Dedicated Hosts allow you to **reuse existing licenses**, making this the **most cost-effective** option in BYOL scenarios

### ❌ Why the other options are incorrect

- <span style="color:red"><strong>Amazon EC2 Reserved Instances (RI) – Incorrect</strong></span>
  Reserved Instances only provide **billing discounts**. They do **not** give control over physical servers and therefore do not satisfy server-bound licensing requirements.

- <span style="color:red"><strong>Amazon EC2 On-Demand instances – Incorrect</strong></span>
  On-Demand instances do not provide any physical host visibility, making them unsuitable for server-bound licenses.

- <span style="color:red"><strong>Amazon EC2 Dedicated Instances – Incorrect</strong></span>
  Dedicated Instances run on hardware dedicated to a single customer, but **you do not get host-level visibility or control**. This makes them insufficient for many server-bound license agreements.

### 🧠 Summary

- **Server-bound / BYOL licenses** → **EC2 Dedicated Hosts** ✅
- **Dedicated Instances** → No host visibility ❌
- **RI / On-Demand** → Pricing options, not licensing solutions ❌

</details>

---

## Question 16

**The engineering team at an e-commerce company is working on cost optimizations for Amazon Elastic Compute Cloud (Amazon EC2) instances. The team wants to manage the workload using a mix of On-Demand and Spot Instances across multiple instance types. They want to create an Auto Scaling group with this mix of instances.
Which of the following options would allow the engineering team to provision the instances for this use case?**

**Options:**

- You can use a launch configuration or a launch template to provision capacity across multiple instance types using both On-Demand Instances and Spot Instances to achieve the desired scale, performance, and cost
- You can neither use a launch configuration nor a launch template to provision capacity across multiple instance types using both On-Demand Instances and Spot Instances to achieve the desired scale, performance, and cost
- You can only use a launch template to provision capacity across multiple instance types using both On-Demand Instances and Spot Instances to achieve the desired scale, performance, and cost
- You can only use a launch configuration to provision capacity across multiple instance types using both On-Demand Instances and Spot Instances to achieve the desired scale, performance, and cost

<details>
  <summary><strong>Answer & Explanation</strong> 📝</summary>

### ✅ Correct Answer

**You can only use a launch template to provision capacity across multiple instance types using both On-Demand Instances and Spot Instances to achieve the desired scale, performance, and cost**

### 🧠 Why this is correct

- **EC2 Auto Scaling with mixed instances policies** supports:

  - Multiple **instance types**
  - A mix of **On-Demand and Spot Instances**

- **Launch templates are mandatory** for this feature
- Launch configurations are **legacy** and do **not support**:

  - Mixed instance types
  - Spot + On-Demand allocation strategies

- Launch templates allow advanced controls such as:

  - Instance type overrides
  - Spot allocation strategies
  - Capacity-optimized scaling

This makes launch templates the **only valid choice** for this cost-optimized Auto Scaling setup.

### ❌ Why the other options are incorrect

- <span style="color:red"><strong>Launch configuration or launch template</strong></span>
  ❌ Launch configurations **do not support** mixed instances policies.

- <span style="color:red"><strong>Neither launch configuration nor launch template</strong></span>
  ❌ Incorrect — launch templates **are required and fully supported**.

- <span style="color:red"><strong>Only launch configuration</strong></span>
  ❌ Launch configurations are deprecated for advanced Auto Scaling use cases.

### 🧠 Summary

- **Mixed On-Demand + Spot across multiple instance types** → **Launch template only** ✅
- **Launch configurations** → Legacy, limited features ❌
- **Launch templates** → Modern, flexible, cost-optimized Auto Scaling ✅

</details>

---

## Question 17

**A startup’s cloud infrastructure consists of a few Amazon EC2 instances, Amazon RDS instances, and Amazon S3 storage. A year into operations, the startup is incurring costs that seem too high for its business requirements.
Which of the following options represents a valid cost-optimization solution?**

**Options:**

* Use AWS Trusted Advisor checks on Amazon EC2 Reserved Instances to automatically renew reserved instances (RI). AWS Trusted Advisor also suggests Amazon RDS idle database instances
* Use AWS Cost Explorer Resource Optimization to get a report of Amazon EC2 instances that are either idle or have low utilization and use AWS Compute Optimizer to look at instance type recommendations
* Use AWS Compute Optimizer recommendations to help you choose the optimal Amazon EC2 purchasing options and help reserve your instance capacities at reduced costs
* Use Amazon S3 Storage Class Analysis to get recommendations for transitions of objects to Amazon S3 Glacier storage classes to reduce storage costs. You can also automate moving these objects into lower-cost storage tiers using lifecycle policies

<details>
  <summary><strong>Answer & Explanation</strong> 📝</summary>

### ✅ Correct Answer

**Use AWS Cost Explorer Resource Optimization to get a report of Amazon EC2 instances that are either idle or have low utilization and use AWS Compute Optimizer to look at instance type recommendations**

**Hint:** The startup wants a **broad, practical cost-optimization approach** across its existing compute usage.

### 🧠 Why this is correct

* **AWS Cost Explorer – Resource Optimization** helps identify:

  * Idle EC2 instances
  * Underutilized EC2 instances
* **AWS Compute Optimizer** analyzes historical utilization and provides:

  * Right-sizing recommendations for EC2 instance types
  * Guidance to reduce over-provisioning and unnecessary spend

Together, these services directly target **wasted compute costs**, which is usually the biggest expense for small startups.

### ❌ Why the other options are incorrect

* <span style="color:red"><strong>AWS Trusted Advisor checks on Reserved Instances – Incorrect</strong></span>
  Trusted Advisor does **not automatically renew Reserved Instances**, and this option misunderstands its capabilities.

* <span style="color:red"><strong>AWS Compute Optimizer for purchasing options – Incorrect</strong></span>
  Compute Optimizer focuses on **right-sizing**, not on selecting or reserving EC2 purchasing options like RIs or Savings Plans.

* <span style="color:red"><strong>Amazon S3 Storage Class Analysis – Incorrect</strong></span>
  While valid for **storage-specific** optimization, it does not address EC2 or RDS costs, making it incomplete for this scenario.

### 🧠 Summary

* **Cost Explorer + Compute Optimizer** → Best first step for identifying and fixing EC2 cost inefficiencies ✅
* **Trusted Advisor** → Advisory only, no automatic RI renewal ❌
* **S3 Storage Class Analysis** → Storage-only optimization ❌

</details>

---

## Question 18

**An e-commerce company has copied 1 petabyte of data from its on-premises data center to an Amazon S3 bucket in the `us-west-1` Region using AWS Direct Connect. The company now wants to create a **one-time copy** of this data to another Amazon S3 bucket in the `us-east-1` Region. The on-premises data center does not allow the use of AWS Snowball.
As a Solutions Architect, which of the following options can be used to accomplish this goal? (Select TWO)**

**Options:**

* Use AWS Snowball Edge device to copy the data from one Region to another Region
* Set up Amazon S3 Transfer Acceleration (Amazon S3TA) to copy objects across Amazon S3 buckets in different Regions using S3 console
* Copy data from the source bucket to the destination bucket using the `aws s3 sync` command
* Set up Amazon S3 batch replication to copy objects across Amazon S3 buckets in another Region using S3 console and then delete the replication configuration
* Copy data from the source Amazon S3 bucket to a target Amazon S3 bucket using the S3 console

<details>
  <summary><strong>Answer & Explanation</strong> 📝</summary>

### ✅ Correct Answers

* **Copy data from the source bucket to the destination bucket using the `aws s3 sync` command**
* **Set up Amazon S3 batch replication to copy objects across Amazon S3 buckets in another Region using S3 console and then delete the replication configuration**

### 🧠 Why these are correct

* **`aws s3 sync`**

  * Supports **cross-Region S3-to-S3 copies**
  * Efficient for **large datasets**
  * Can be resumed and scripted
  * Commonly used for **one-time bulk migrations**
  * `aws s3 sync s3://source-bucket-us-west-1 s3://target-bucket-us-east-1`

* **Amazon S3 Batch Replication**

  * Designed specifically to **replicate existing objects** (not just new ones)
  * Works well for **very large buckets (petabyte scale)**
  * After the one-time copy is complete, the replication rule can be removed
  * Fully managed and scalable

These two options best satisfy:

* One-time copy
* Large data volume (1 PB)
* No Snowball usage
* Reliable cross-Region transfer

### ❌ Why the other options are incorrect

* <span style="color:red"><strong>Use AWS Snowball Edge device</strong></span>
  Snowball usage is explicitly **not allowed** in the scenario.

* <span style="color:red"><strong>Use Amazon S3 Transfer Acceleration</strong></span>
  S3 Transfer Acceleration is meant for **uploading data from clients to S3**, not for **S3-to-S3 cross-Region copies**.

* <span style="color:red"><strong>Copy data using the S3 console</strong></span>
  The S3 console is **not practical or reliable** for copying **1 PB of data**.

### 🧠 Summary

* **One-time, large-scale S3 cross-Region copy** → `aws s3 sync` or **S3 Batch Replication** ✅
* **Snowball** → Not allowed ❌
* **S3 Transfer Acceleration / Console copy** → Not suitable ❌

Note:- Even **if Snowball were allowed**, it is **not the right service** for this scenario.

🔴 **Snowball cannot be used for S3-to-S3 regional copy**
You cannot ship a Snowball device **between AWS Regions**.

📌 **Exam rule:**

> Snowball is **never** used for **AWS-to-AWS** data transfer.

</details>

---
