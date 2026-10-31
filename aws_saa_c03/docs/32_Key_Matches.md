# Key Matches & Short Notes

Sections:-

- [5. EC2 Fundamentals](#5-ec2-fundamentals)
- [6. ELB & ASG](#6-elb--asg)
- [9. AWS RDS Aurora Elasticache](#9-aws-rds-aurora-elasticache)
- [10. Route 53](#10-route-53)
- [11. EC2 Instance Storage](#11-ec2-instance-storage)
- [12. Amazon S3](#12-amazon-s3)
- [13. S3 Advance](#13-s3-advance)
- [17. AWS Integration and Messaging Services](#17-aws-integration-and-messaging-services)
- [18. ECS ECR EKS](#18-ecs-ecr-eks)
- [Serverless](#serverless)
- [Databases](#databases)
- [22. Data & Analytics](#22-data--analytics)
- [23. Machine Learning](#23-machine-learning)
- [24. AWS Monitoring & Auditing](#24-aws-monitoring--auditing)
- [25. IAM Advance](#25-iam-advance)
- [27. Networking & VPC](#27-networking--vpc)
- [26. AWS Security & Encryption](#26-aws-security--encryption)
- [28. DR & Migration](#28-dr--migration)
- [30. Other Services](#30-other-services)

---

## 5. EC2 Fundamentals

### Instance Classes

Here's a breakdown of the main EC2 instance classes:

1.  **General Purpose:**
    - Ideal for a variety of workloads like web servers and code repositories.
    - Offer a balance of compute, memory, and networking resources.
2.  **Compute Optimized (C):**
    - Optimized for compute-intensive tasks requiring high processor performance.
    - Suitable for:
      - Batch processing
      - Media transcoding
      - High-performance web servers
      - High-performance computing (HPC)
      - Machine learning
      - Dedicated game servers
    - Typically denoted by the `C` family (e.g., `C5`, `C6`).
3.  **Memory Optimized (R):**
    - Designed for workloads that process large datasets in memory (RAM).
    - Use cases include:
      - High-performance relational and non-relational databases (especially in-memory databases)
      - Distributed web-scale cache stores (e.g., Elasticache)
      - In-memory databases optimized for business intelligence (BI)
      - Applications performing real-time processing of big, unstructured data.
    - Often identified by the `R` series (for RAM), as well as `X1` and `Z1` instances.
4.  **Storage Optimized:**
    - Excellent for applications that require high-speed access to large datasets on local storage.
    - Suitable for:
      - High-frequency online transactional processing (OLTP) systems
      - Relational and NoSQL databases
      - Caching for in-memory databases (e.g., Redis)
      - Data warehousing applications
      - Distributed file systems
    - Instance names often start with `I`, `D`, or `H1`.

### EC2 Instances Purchasing Options

#### On-Demand Instances

On-demand instances are the most common and cost-effective way to use EC2. Shprt workloads, predictable pricing, and pay by the seconds.

*   Benefit: Flexible pricing.
*   Payment: Pay for what you use.
*   Ideal for: Short workloads, predictable pricing.

#### Reserved Instances (1 & 3 Years)

Reserved Instances are suitable for long workloads.

*   Term: One year or three years.
*   Ideal for: Running databases for extended periods.
*   Convertible Reserved Instances: Allow you to change the instance type over time, providing flexibility.

#### Savings Plans (1 & 3 Years)

Savings Plans are a more modern approach for long workloads.

*   Term: One year or three years.
*   Commitment: Commit to a specific amount of usage in dollars, rather than a specific instance type.

#### Spot Instances

Spot Instances are designed for very short workloads.

*   Benefit: Very cheap.
*   ⚠️ **Warning:** You can lose these instances at any time, making them less reliable.

#### Dedicated Host

Dedicated Hosts allow you to book an entire physical server.

*   Benefit: Control instance placements.

#### Dedicated Instances

Dedicated Instances ensure that no other customers share your hardware.

#### Capacity Reservations

Capacity Reservations allow you to reserve capacity in a specific Availability Zone (AZ) for any duration.

### Placement Groups

When you create a placement group, you specify one of the following strategies for the group:
  * **Cluster** — Instances are grouped together in a low-latency hardware setup within a single Availability Zone (AZ). Provides high performance but also carries a higher risk. Ideal for computational jobs. ⚠️ Warning: If the AZ fails, all instances in the cluster placement group will fail simultaneously. Use Cases:
    * Big data jobs requiring fast completion with high networking.
    * Applications needing extremely low latency and high throughput between instances.
  * **Spread** — Instances are spread across different hardware. Minimizes failure risk. Spans across multiple AZs. Reduces the risk of simultaneous failure because instances are on separate hardware. ⚠️ **Warning:** Limited to 7 EC2 instances per placement group _per_ AZ. Use Cases:
    * Applications requiring maximized high availability and reduced risk.
    * Critical applications where instance failures must be isolated.
  * **Partition** — Similar to Spread, but instances are spread across multiple partitions (physical racks). Partitions rely on different sets of hardware racks within an AZ. Partitions are isolated from each other's failures. Allows scaling to hundreds of EC2 instances per group. Up to 7 partitions per AZ. Partitions can span multiple AZs in the same region. Up to 100s of EC2 instances. Instances in one partition do not share the same physical rack as instances in other partitions. Each partition is isolated from failure. If one partition goes down, others should remain operational. Information about which partition an EC2 instance belongs to can be accessed via the metadata service. Use Cases:
    * Applications that are partition-aware and can distribute data and servers across partitions.
    * Big data applications like HDFS, HBase, Cassandra, and Apache Kafka.

---

## 6. ELB & ASG

* **Vertical ( | ) = Up** (make one machine taller/stronger).
* **Horizontal ( __ ) = Out** (add more machines side by side).

Vertical scalability involves increasing the resources of a single instance.

Horizontal scalability involves increasing the number of instances or systems. This is also known as elasticity.

### Network Load Balancer (NLB)

The Network Load Balancer (NLB) operates at **layer 4** of the OSI model, dealing with TCP and UDP traffic. 🌐 Unlike Application Load Balancers (ALB) which handle HTTP (layer 7), NLBs work at a lower level (Layer 4 -> TCP, UDP).

*   NLBs are known for their **high performance**, capable of handling millions of requests per second with **ultra-low latency**. 🚀
*   Each NLB has **one static IP per Availability Zone (AZ)**, and supports assigning Elastic IP (helpful for whitelisting specific IP). You can assign an Elastic IP to each AZ. 📍

### Gateway Load Balancer

The Gateway Load Balancer is the newest type of load balancer. It's used to deploy, scale, and manage your fleet of third-party network virtual appliances in AWS. Let's explore what that means.

You would use a Gateway Load Balancer if you want all traffic of your network to go through:

*   A firewall.
*   An intrusion detection and prevention system (IDPS).
*   A deep packet inspection system.
*   A system to modify payloads at the network level.

The Gateway Load Balancer has two primary functions:

1.  Transparent Network Gateway: All traffic in your VPC goes through a single entry and exit point – the Gateway Load Balancer.
2.  Load Balancer: It distributes traffic across a set of virtual appliances in your target group.

So, remember these two functions.

If you see the **GENEVE** protocol on port **6081** in an exam question, it's likely related to the Gateway Load Balancer.

What can be target groups for Gateway Load Balancers? These are your third-party appliances. They can be:

*   EC2 instances (registered by instance ID).
*   IP addresses (must be private IPs). 📌 **Example:** If you're running virtual appliances on your own network/data center, you can register them by IP manually.
   
### Sticky Sessions

Sticky sessions, also known as session affinity, allow you to direct client requests to the same backend instance behind a load balancer.

The core idea is that if a client makes multiple requests to a load balancer, all those requests will be handled by the same EC2 instance.

Sticky sessions rely on cookies. When a client makes a request, the load balancer sends a cookie back to the client. This cookie contains information about the stickiness and an expiration date.

*   When the cookie expires, the client may be redirected to a different EC2 instance.

> Note: NLB works without cookies. 

There are two main types of cookies used for sticky sessions:

1.  Application-Based Cookies
2.  Duration-Based Cookies

#### 1. Application-Based Cookies 🍪

* Custom Cookie
  *   Generated by your application/target itself.
  *   You can include any custom attributes required by your application.
  *   The cookie name must be specified individually for each target group.
  *   You **must not** use the following reserved names: `AWSALB`, `AWSALBAPP`, or `AWSALBTG`.

      📌 **Example:**
      Your application sets a cookie named `MYCUSTOMCOOKIE`.
* Application Cookie
  *   Generated by the load balancer.
  *   The cookie name is `AWSALBAPP` for the ALB.

#### 2. Duration-Based Cookies ⏳

*   These cookies are generated by the load balancer.
*   The cookie name is `AWSALB` for the ALB and `AWSELB` for the CLB.
*   They have an expiry based on a specific duration set by the load balancer.
*   With application-based cookies, the duration can be specified by the application itself.

📝 **Note:** While you don't need to memorize the exact cookie names, remember the distinction between application-based and duration-based cookies, as this becomes relevant when working with CloudFront.

### Cross Zone Load Balancing

*   Cross-zone load balancing distributes traffic across multiple AZs.
*   ALBs have it enabled by default (and no inter-AZ charges).
*   NLBs and Gateway Load Balancers have it disabled by default (and incur inter-AZ charges if enabled).
*   The choice to enable or disable cross-zone load balancing depends on your specific use case and cost considerations. 💡 **Tip:** Consider the distribution of your instances across AZs and the potential cost implications when making your decision.


### Auto Scaling Group Scaling Policies

#### a. Dynamic Scaling

This category includes policies that automatically adjust capacity based on real-time conditions.

*   **Target Tracking Scaling:** 🎯
    *   Simple to set up.
    *   Define a metric for your ASG (e.g., CPU utilization).
    *   Define a target value (e.g., 40%).
    *   The ASG automatically scales out or in to maintain the metric around the target value.
*   **Simple or Step Scaling:** 🪜
    *   Define CloudWatch alarms.
    *   Alarms trigger when you want to add or remove capacity from the ASG. Example: CPU utilization is > 70% then add 2 units. When CPU utilization is < 30%, remove 1 unit.

#### b. Scheduled Scaling

This is based on anticipated scaling needs.

*   You predict scaling based on known usage patterns.
*   📌 **Example:** Increase minimum capacity to 10 every Friday at 5:00 PM to handle increased user traffic.
   
#### c. Predictive Scaling

This uses historical data to forecast future load.

*   Continuously forecasts load and schedules scaling actions ahead of time.
*   Ideal for repeating patterns.
*   The ASG analyzes historical load, generates a forecast, and schedules scaling actions based on the forecast.
*   Very useful for cyclical data.

---

## 9. AWS RDS Aurora Elasticache

### RDS

#### RDS Storage Auto Scaling

RDS Storage Auto Scaling is a feature that automatically scales your database storage when you're about to run out of space. When creating an RDS database, you specify the initial storage (e.g., 20 GB). If your database usage increases and you're running out of space, RDS Storage Auto Scaling will automatically increase the storage without requiring manual intervention.

To use this feature, you need to set a maximum storage threshold to limit how much the storage can grow.

Automatic storage modification occurs if:

*   Free storage is less than 10% of allocated storage.
*   Low storage condition lasts for more than five minutes.
*   Six hours have passed since the last modification.

If these conditions are met, the storage will auto-increase when enabled.

#### RDS Read Replicas vs. Multi AZ

**Read Replicas** are designed to scale read operations. 
*   You can create up to 15 Read Replicas.
*   These replicas can reside within the same Availability Zone (AZ), across different AZs, or even across different regions. 🌍
*   The replication between the main RDS database instance and the Read Replicas is **asynchronous** (multitasking). This means the data on the Read Replicas is **eventually consistent**. 
*   If a Read Replica is queried before it has fully replicated the latest data, it might return stale information.

Read Replicas can be promoted to become standalone databases:-
*   Once promoted, they are no longer part of the replication mechanism.
*   They have their own lifecycle.

📝 **Note:** When using Read Replicas, the application needs to be configured to connect to the list of available Read Replicas. This usually involves updating the connection string.

Networking Costs 🌐
*   Within the same region but different AZs, replication traffic for RDS Read Replicas is **free**.
*   Cross-region replication incurs network costs.

**RDS Multi-AZ (Disaster Recovery)**

Multi-AZ deployments are primarily used for **disaster recovery**.

*   Your application reads and writes to a master database instance in one AZ (e.g., AZ A).
*   There is **synchronous** replication to a standby instance in another AZ (e.g., AZ B).
*   Every change to the master is immediately replicated to the standby.

The application connects to a single DNS name.

*   In case of a failure of the master database, there is an automatic failover to the standby database (thanks to the single DNS name).
*   This increases availability.

The standby database becomes the new master automatically.

*   No manual intervention is required from the application, as long as it is configured to automatically reconnect to the database using the same DNS name.
*   Multi-AZ is **not** used for scaling read operations. The standby database is solely for failover purposes.

📝 **Note:** Can we configure the read replicas to be Multi-AZ? Yes, we can configure Read Replicas to be Multi-AZ for Disaster Recovery (DR). > **Common Exam Question.**

### Amazon Aurora

Aurora is cloud-optimized, offering significant performance improvements:

*   🚀 Up to 5x performance improvement over MySQL on RDS.
*   🚀 Up to 3x performance improvement over Postgres on RDS.

Aurora's storage automatically grows, which is a key feature:

*   Starts at 10GB and automatically scales up to **256 TiB** (raised from 128 TiB in Jul 2025 — older exam questions may still say 128 TB).
*   This eliminates the need for manual disk monitoring and management.

Aurora offers robust read scaling and high availability:

*   Supports up to 15 read replicas.
*   Replication lag is usually well under 100 ms (older course notes say sub-10 ms).
*   Failover is near instantaneous, much faster than Multi-AZ MySQL RDS.
*   High availability is built-in due to its cloud-native design.

#### High Availability and Read Scaling

Aurora stores six copies of your data across three Availability Zones (AZs).

*   For writes, Aurora requires only 4 out of 6 copies to be available.
*   For reads, it requires only 3 out of 6 copies.
*   This ensures high availability even if an AZ is down.

Aurora features a self-healing process:

*   Data corruption is automatically corrected using peer-to-peer replication.
*   Data is distributed across hundreds of volumes, reducing risk.

#### Aurora as Multi-AZ for RDS

Aurora operates with a single master instance for writes, similar to Multi-AZ for RDS.

*   Failover typically occurs in less than 30 seconds.
*   Up to 15 read replicas can serve read traffic.
*   Any read replica can become the master in case of a failover.
*   Read replicas support cross-region replication.

Key things to remember: One master, multiple read replicas, and replicated, self-healing, auto-expanding storage.

#### Aurora Cluster Architecture

When clients interact with an Aurora cluster, they connect to specific endpoints.

*   Shared storage volume: Auto-expands from 10GB to 256 TiB (128 TiB before Jul 2025).
*   Master instance: Handles all write operations.

Aurora provides two key endpoints:

1.  **Writer Endpoint**:
    *   A DNS name that always points to the current master instance.
    *   Clients use this endpoint for write operations.
    *   Automatically redirects connections to the new master after a failover.
2.  **Reader Endpoint**:
    *   Helps with connection load balancing across read replicas.
    *   Automatically connects clients to available read replicas.
    *   Load balancing occurs at the connection level, not the statement level.

Read replicas can be configured with auto-scaling, allowing you to dynamically adjust the number of read replicas (from 1 to 15) based on workload.

Aurora also includes a feature called **backtrack**, which allows you to restore data to any point in time without relying on backups.

### Advanced Concepts of Aurora

- **Replica Auto Scaling**: If the Reader Endpoint experiences high read request volume, leading to increased CPU usage on the Aurora databases, replica auto-scaling comes to the rescue! 
  * Replica auto-scaling automatically adds more Aurora Replicas.
  *   The Reader Endpoint is extended to include these new replicas.
  *   The new replicas share the read traffic, distributing the load and reducing overall CPU usage.
- **Custom Endpoints**: This is useful when you want to run specific workloads on more powerful instances, such as analytical queries.
  *   **When a Custom Endpoint is defined, the original Reader Endpoint is generally not used anymore.**
  *   Multiple Custom Endpoints can be set up for different workload types.
  *   This allows you to query only a subset of your Aurora Replicas.
- **Serverless**: Aurora Serverless provides automated database instantiation and auto-scaling based on actual usage.
  *   Ideal for infrequent, intermittent, or unpredictable workloads.
  *   Eliminates the need for capacity planning.
  *   You pay per second of Aurora instance usage, potentially making it more cost-effective.
- **Global Aurora**: Add AWS regions to create a global Aurora database.
  *   This feature requires a compatible instance size.
  *   Go to Action and select "Add AWS Region".
  *   One primary region handles all reads and writes.
  *   Up to **10** secondary read-only regions can be configured (raised from 5 in May 2025 — older exam questions may still say 5).
  *   Replication lag is typically less than one second.
  *   Up to 16 Read Replicas are supported per secondary region.
  *   📝 **Note:** Replication across regions for Aurora Global Database takes, on average, less than one second. This is a key indicator for using Global Aurora in exam scenarios.
- **Aurora Machine Learning**: Aurora integrates with AWS machine learning services, enabling ML-based predictions via SQL. 
  - Use Cases:
    *   Fraud detection
    *   Ads targeting
    *   Sentiment analysis
    *   Product recommendation
  - Architecture:
    1.  Your application runs a SQL query (e.g., "What are the recommended products?").
    2.  Aurora sends data (user profile, shopping history, etc.) to the machine learning service.
    3.  The machine learning service returns a prediction (e.g., "The user should buy a red shirt and blue pants").
    4.  Aurora returns the query results to the application.
- **Babelfish for Aurora PostgreSQL**: Babelfish allows Aurora PostgreSQL to understand commands targeted for Microsoft SQL Server using T-SQL.

### RDS & Aurora - Backup & Monitoring

#### RDS Automated Backups 💾

*   The RDS service automatically performs a daily full backup of the database during a defined backup window.
*   Transaction logs are backed up every 5 minutes. This means you can restore to any point in time up to 5 minutes prior to the current time.
*   Retention period: Configurable from 1 to 35 days.
*   To disable automated backups, set the retention period to 0.

#### RDS Manual DB Snapshots 📸

*   These are manually triggered by the user.
*   The key benefit is that you can retain these snapshots for as long as you need.
*   **Automated backups expire, but manual snapshots do not.**

💡 **Tip:** Manual DB Snapshots can be used to save costs.

Trick: in a stopped RDS database, you will still pay for storage. If you plan on stopping it for a short time, you should snapshot & restore instead.

#### Aurora Backups 🌟

*   Automated backups are similar to RDS, with a retention period of 1 to 35 days.
*   ⚠️ **Warning:** Automated backups **cannot** be disabled in Aurora.
*   Point-in-time recovery is available within the retention timeframe.
*   Manual DB Snapshots are also supported, manually triggered, and can be retained indefinitely for as long as you need.

#### Restore Options ⚙️

*   RDS or Aurora backups (snapshots) can be restored into a **new** database instance. Restoring always creates a new database.
*   **Restoring MySQL RDS databases from Amazon S3**

    *   Create a backup of your on-premises database.
    *   Upload the backup to Amazon S3.
    *   Restore the backup file to a new RDS instance running MySQL.

*   **Restoring MySQL Aurora Cluster from Amazon S3**

    1.  Take a backup of your on-premises database using **Percona XtraBackup**.
    2.  Upload the Percona XtraBackup file to Amazon S3.
    3.  Restore the backup file to a new Aurora cluster running MySQL.

📝 **Note:** Restoring to Aurora MySQL requires using Percona XtraBackup.

### Aurora Database Cloning 🧬

*   Allows you to create a new Aurora database cluster from an existing one. Useful for creating staging environments for testing. Faster than snapshot and restore.

How Cloning Works:
*   Cloning uses a copy-on-write protocol.
*   Initially, the clone shares the same data volume as the original database cluster. This is fast and efficient.
*   As updates are made to either the production or staging database, new storage is allocated, and data is copied and separated.

Benefits:
*   Fast and cost-effective.
*   Enables **creating staging databases from production databases without impacting the production environment**.
*   Avoids the need for snapshot and restore procedures.

### RDS and Aurora Security

*   The master database and any read replicas are encrypted using KMS.
*   Encryption is defined at launch time during the initial database creation.

⚠️ **Warning:** If the master database is not encrypted initially, read replicas cannot be encrypted.

To encrypt an already existing unencrypted database:

1.  Take a database snapshot from the unencrypted database. 📸
2.  Restore the database snapshot as an encrypted database. 🔄

**Audit Logs**: To track queries and database activity over time, you can enable Audit Logs. 🕵️‍♀️

📝 **Note:** Audit Logs are retained for a limited time.

To preserve Audit Logs for longer periods:
*   Send them to AWS CloudWatch Logs. ☁️

### Amazon RDS Proxy

Using an RDS Proxy allows your application to pool and share database connections established with the database. Instead of each application connecting directly to your RDS database instance, they connect to the proxy. The proxy then pools these connections into fewer connections to the RDS database instance.

Why is this beneficial?

*   **It improves database efficiency by reducing stress on database resources (CPU, RAM).** 🚀
*   **It minimizes open connections and timeouts**. ⏳

From an exam perspective, remember these key benefits.

The RDS Proxy is fully serverless and auto-scaling, so you don't need to manage its capacity. It's also highly available across multiple Availability Zones (AZs).

Another advantage of using an RDS Proxy is that it **enforces IAM authentication for your database**. This ensures that **users can only connect to your RDS database instance using IAM**. These credentials can be **securely stored in AWS Secrets Manager**. 🔐

**If you need to enforce IAM authentication for your database, consider using RDS Proxy**.

The **RDS Proxy is never publicly accessible; it's only accessible from within your VPC**, enhancing security. 🛡️

**Lambda functions can greatly benefit from the RDS Proxy**. Lambda functions execute pieces of code and can appear and disappear rapidly. If you have hundreds or thousands of Lambda functions opening connections to your RDS database instance, it can lead to open connections, timeouts, and a general mess.

### Amazon ElastiCache

Amazon ElastiCache helps you manage Redis or Memcached, which are caching technologies, similar to how RDS manages relational databases.

⚠️ **Warning:** Using Amazon ElastiCache requires **significant application code changes**. You need to modify your application to query the cache before or after querying the database.

Redis Security: IAM policies on ElastiCache are used for AWS API-level security.

Memcached Security: Memcached supports SASL-based authentication. 📝 Note: Just remember the name.

A key use case for Redis, especially for exam preparation, is creating a gaming leaderboard.

Redis Sorted Sets guarantee both uniqueness and element ordering.
---

## 10. Route 53

- Except for alias records, TTL is mandatory for each DNS record.

### CNAME vs. Alias Records in Route 53

#### CNAME Records

*   CNAME (Canonical Name) records point a hostname to another hostname.
    📌 **Example:** `app.mydomain.com` points to `blabla.anything.com`.
*   ⚠️ **Warning:** CNAME records only work for **non-root** domain names (e.g., `something.mydomain.com`). They **cannot** be used for the root domain itself (e.g., `mydomain.com`).

#### Alias Records

*   Alias records are specific to Route 53.
*   They point a hostname to a specific AWS resource.
    📌 **Example:** `app.mydomain.com` points to `blabla.amazonaws.com`.
*   Alias records work for **both root domains** and **non-root domains**. This is a key advantage.
*   Alias records are **free of charge**. 💰
*   They have **native health check** capabilities. ✅

Major Difference between CNAME and Alias Records (Exam may test on this)
- CNAME records only work for non-root domain names.
- Alias records work for both root and non-root domain names.

*   You **cannot set the TTL (Time To Live)** for alias records; Route 53 manages it automatically.

Alias records can target the following AWS resources:

*   Elastic Load Balancers (ELB)
*   CloudFront Distributions
*   API Gateway
*   Elastic Beanstalk environments
*   S3 Websites (when buckets are enabled as websites)
*   VPC Interface Endpoints
*   Global Accelerator accelerators
*   Route 53 records in the same hosted zone

⚠️ **Warning:** You **cannot** set an alias for an EC2 DNS name.

Route 53 supports several routing policies:

- **Simple**: Route traffic to a single resource. 
  - It's possible to specify multiple values in the same record. If multiple values are returned by the DNS, the client randomly chooses one.
  - If you enable an alias record alongside the simple policy, you can only specify one AWS resource as a target.
- **Weighted**: The weighted routing policy allows you to direct a percentage of your traffic to specific resources based on assigned weights.
- **Latency Based**: The latency-based routing policy redirects users to the resource with the lowest latency, effectively directing them to the closest AWS region. This is particularly useful when latency is a primary concern for your websites or applications. Latency is measured by how quickly users can connect to the nearest identified AWS region for a given record.
- **Failover**: The goal is to automatically switch traffic from a primary EC2 instance to a secondary (disaster recovery) EC2 instance if the primary instance becomes unhealthy. The setup involves:
  *   Route 53 acting as the DNS service.
  *   A primary EC2 instance.
  *   A secondary (disaster recovery) EC2 instance.
- **Geolocation**: Geolocation routing policy directs traffic to different resources based on the geographic location of the user. This is distinct from Latency-based routing.
  *   🌍 It determines the user's location by continent, country, or even U.S. state.
  *   📍 The most precise location match is selected first.
  *   ⚠️ **Warning:** Always create a default record to handle requests from locations that don't match any specific rule.
- **Geoproximity**: Geoproximity Routing allows you to route traffic to your resources based on the geographic location of your users and resources. With this policy, you can use a bias to shift more traffic to resources based on specific locations.
  - To change the size of a geographic location, you need to specify a **bias** value.
    *   If you want more traffic to go to a specific resource, expand the bias value by increasing it. 📈
    *   If you want less traffic to go to your resource, shrink it by decreasing the bias values to a negative number. 📉
  - In short:-   
    * To expand (1 to 99) - more traffic to the resource
    * To shrink (-1 to -99) - less traffic to the resource
  - Resources can be:
    *   AWS resources: Specify the region, and AWS will compute the correct routing.
    *   Non-AWS resources (e.g., on-premises data center): Specify the latitude and longitude.
  - 📝 **Note:** To leverage the bias feature, you need to use the advanced Route 53 Traffic Flow.
  - 💡 **Tip:** Geoproximity Routing is helpful when you need to shift traffic from one region to another by increasing the bias.
- **IP-based Routing**: IP-based routing is an intuitive routing policy that defines routing based on client IP addresses. In Route 53, you define a list of CIDRs (IP ranges for your clients) and specify which location the traffic should be sent to based on the CIDR. The use cases for IP-based routing include:
  *   Optimizing performance 🚀 because you know the IP addresses in advance.
  *   Reducing network costs 💰 because you know where the IPs are coming from.
- **Multi-Value Answer**: The Multi-Value routing policy is used to route traffic to multiple resources. Route 53 will return multiple values or resources in response to a query.
  *   You can associate these resources with Health Checks.
  *   Only resources associated with a healthy Health Check will be returned.
  *   Up to 8 healthy records are returned for each Multi-Value query.
  * 📝 **Note:** While it might seem similar, Multi-Value routing is **not** a substitute for an ELB (Elastic Load Balancer). It provides client-side load balancing.

### Route 53 Resolvers & Hybrid DNS

To achieve hybrid DNS, AWS provides **Resolver Endpoints**:

* **Inbound Endpoint** → Allows on-premises DNS resolvers to query and resolve AWS resource domain names.
* **Outbound Endpoint** → Allows AWS resources (like EC2 instances) to query and resolve on-premises DNS names.

This setup allows **two-way DNS resolution** between AWS and on-premises.

**Key Takeaway**

💡 If you want **bidirectional DNS resolution** between your **on-premises data center** and **AWS**, you must configure both:

* 🔹 **Inbound Resolver Endpoint** (on-premises → AWS)
* 🔹 **Outbound Resolver Endpoint** (AWS → on-premises)

That’s the essence of Route 53 Resolver in a hybrid DNS setup!

---

## 11. EC2 Instance Storage

*   EBS Volumes can only be mounted to **one instance at a time** (at the CCP level), but at associated level (Solutions Architect, Developer, SysOps) "multi-attach" feature for some EBS.
*   When you create an EBS Volume, it is bound to a specific **Availability Zone (AZ)**.

📌 **Example:** You cannot attach an EBS Volume created in `us-east-1a` to an instance in `us-east-1b`.

📌 **Example:** Transferring an EBS volume from one AZ to another:

1.  You have an EC2 instance with an EBS volume in `US-EAST-1A`.
2.  You have another EC2 instance in `US-EAST-1B`.
3.  Take a snapshot of the EBS volume in `US-EAST-1A`.
4.  Restore the snapshot in `US-EAST-1B`. This effectively moves the EBS volume.

Here are some important EBS Snapshot features:

*   **EBS Snapshot Archive:** 📦
    *   Allows you to move snapshots to an "archive tier" that is up to 75% cheaper.
    *   Restoring from the archive tier takes 24 to 72 hours. It's not immediate.

*   **Recycle Bin for EBS Snapshots:** 🗑️
    *   If you delete an EBS Snapshot, it's moved to the Recycle Bin instead of being permanently deleted.
    *   This allows you to recover from accidental deletions.
    *   You can set the retention period for the Recycle Bin from 1 day to 1 year.

*   **Fast Snapshot Restore (FSR):** ⚡
    *   Forces a full initialization of your snapshot.
    *   Ensures no latency on the first use of the restored volume.
    *   Helpful for large snapshots that need to be initialized quickly.
    *   ⚠️ **Warning:** This feature is expensive, so use it judiciously.

### AMIs

AMIs are region-specific but can be copied across regions to leverage AWS's global infrastructure.

### EC2 Instance Store

EC2 Instance Stores provide high-performance storage directly attached to the physical server hosting your EC2 instance. While EBS volumes offer good performance, Instance Stores can provide even better I/O for specific use cases.

#### Key Benefits:

*   🚀 **Better I/O Performance:** Instance Stores are optimized for high throughput and low latency.
*   ⚡ **High Disk Performance:** Ideal when you need extremely fast disk access.

#### Important Considerations:

*   ⚠️ **Ephemeral Storage:** Data on an Instance Store is lost when the EC2 instance is stopped or terminated. It is not a durable, long-term storage solution.
*   ⚠️ **Data Loss Risk:** If the underlying server fails, data on the Instance Store will be lost.
*   🛡️ **Backup Responsibility:** If you use an Instance Store, you are responsible for backing up and replicating the data based on your needs.

#### Use Cases:

Instance Stores are well-suited for:

*   Buffers
*   Caches
*   Scratch data
*   Temporary content

**If you see a question about very high-performance hardware attached volume for EC2 instances, think local EC2 Instance Store.**

### EBS Volume Types

Volume Type Categories

*   **General Purpose SSD (gp2 and gp3):** Balances price and performance. These are cost-effective storage options with low latency. 
    *   They are suitable for: System boot volumes, Virtual desktops, Development and test environments.
    *   Sizes: gp2 1 GiB to 16 TiB; gp3 1 GiB to **64 TiB** (raised from 16 TiB in Sep 2025, along with 80,000 IOPS and 2,000 MiB/s).
    *   💡 Tip: Remember that with gp3, you can independently set the IOPS and throughput, while with gp2, they are linked.
*   **Highest-Performance SSD (io1 and io2):** For mission-critical, low-latency, and high-throughput workloads.
    *   **io1**: Size: 4 GB to 16 TB. Provisioned IOPS can be increased independently of storage size.
    *   **io2**: Size: 4 GB to 64 TB. Sub-millisecond latency. Max IOPS: 256,000. IOPS to GB ratio: 1,000:1. Very high-performance I/O. Supports EBS multi-attach.
*   **Low-Cost HDD (st1 and sc1):** Designed for frequently and infrequently accessed throughput-intensive workloads.
    * These volume types cannot be used as boot volumes.
125 GB to 16 TB.
    *   **Throughput Optimized HDD (st1):** Great for big data, data warehousing, and log processing. Max throughput: 500 MB/s. Max IOPS: 500.
    *   **Cold HDD (sc1):** For archive data (infrequently accessed). Max throughput: 250 MiB/s. Max IOPS: 250.   

**Boot Volumes**: Only the following volume types can be used as boot volumes for EC2 instances (where the root OS runs): `gp2`, `gp3`, `io1`, and `io2`.

Key Differences to Remember

*   **General Purpose SSD (gp2/gp3) vs. Provisioned IOPS SSD (io1/io2):** Use Provisioned IOPS for databases.
*   **st1 and sc1:** Use for high throughput and lowest cost.

📝 **Note:** If you need more than 32,000 IOPS, you need EC2 Nitro instances with io1 or io2 volumes. (Since Sep 2025, gp3 also goes up to 80,000 IOPS, so io1/io2 is no longer the only choice above 32,000 IOPS.)

### Multi-Attach Feature of EBS Volumes

The Multi-Attach feature allows you to attach the same EBS volume to multiple EC2 instances within the same Availability Zone. 

This feature is exclusively available for the `io1` and `io2` families of EBS volumes. Each instance will have full read and write permissions to the high-performance volume, allowing concurrent read and write operations.

**Use Cases:**

*   Higher application availability in case of a clustered Linux application (📌 **Example:** Teradata).
*   Applications that must manage concurrent write operations.

**Limitations and Important Considerations:**

*   The Multi-Attach feature is only available within a single Availability Zone. You cannot attach an EBS volume from one AZ to another.
*   ⚠️ **Warning:** A maximum of **16 EC2 instances** can be attached to the same volume at a time. **Remember this number for the exam!**
*   To use Multi-Attach, you must use a cluster-aware file system. This is different from standard file systems like XFS or EXT4. 📝 **Note:** This is an important detail to consider when implementing this feature.

### Encrypting EBS Volumes

Encrypting an Unencrypted EBS Volume

Here's how to encrypt an existing unencrypted EBS volume:

1.  📸 Create an EBS snapshot of the unencrypted volume.
2.  🔑 Encrypt the EBS snapshot using the copy function.
3.  💾 Create a new EBS volume from the encrypted snapshot. This new volume will be encrypted.
4.  🔗 Attach the encrypted volume to the original instance.  

**Shortcut:** Encrypting Directly from an Unencrypted Snapshot

You can directly create an encrypted EBS volume from an unencrypted snapshot:

1.  Select the unencrypted snapshot.
2.  Choose "Action" and then "Create Volume from Snapshot".
3.  Enable encryption on the fly in the volume creation settings.
4.  Select a KMS key.
5.  Create the encrypted EBS volume.

### Amazon EFS - Elastic File System

Amazon EFS (Elastic File System) is a managed NFS (Network File System). Because it's a network file system, it can be mounted on many EC2 instances, even those in different Availability Zones. This is the core strength of EFS.

*   Highly available ✅
*   Very scalable ✅
*   Expensive (approximately three times the cost of a GP2 EBS volume) 💰
*   Pay-per-use (no need to provision capacity in advance) 💸

EFS allows multiple EC2 instances across different Availability Zones to connect to the same network file system.

EFS is well-suited for:

*   Content management ✍️
*   Web serving 🌐
*   Data sharing 🤝
*   WordPress 🚀

#### Key Features

*   Uses the NFS protocol internally.
*   Access is controlled via security groups.
*   **Only compatible with Linux-based AMIs (not Windows)**. 🐧
*   Encryption at rest can be enabled using KMS. 🔑
*   Standard file system on Linux, using the POSIX system and a standard file API.
*   No need to plan capacity in advance; the file system scales automatically. ⬆️
*   Pay-per-use for each gigabyte of data used. 💸

---

## 12. Amazon S3

- A common mistake for beginners is assuming S3 is a global service, but buckets are region-specific. Bucket names must be globally unique but Buckets are defined at the region level.
- The maximum object size is **50 TB** (raised from 5 TB in Dec 2025 — older exam questions may still say 5 TB). One GET still returns at most 5 TB.
- If a file is larger than 5 GB, you must use the "multi-part upload" feature.
-   Any file uploaded *before* versioning was enabled will have a version ID of "null".
-   Suspending versioning does *not* delete any existing versions. It simply stops creating new versions. This is a safe operation. ✅

**S3 Replication**
- To configure replication first enable **Versioning** in both the source and destination buckets. 🔄 
- S3 Replication comes in 2 flavors- **CRR** (Cross-Region Replication) and **SRR** (Same-Region Replication). 
- The buckets can reside in different AWS accounts. 
- Replication occurs asynchronously in the background. 
- ⚙️ To ensure replication functions correctly, grant appropriate IAM permissions to the S3 service. This allows it to read from and write to the specified buckets. 🔑
- After enabling replication, only **new objects** will be replicated. 🆕
- To replicate existing objects, you must use the **S3 Batch Replication** feature. 🔄 This feature also replicates objects that have failed replication.
- You can replicate delete markers from the source bucket to the target bucket. 🗑️ This is an optional setting.
- ⚠️ **Warning:** If a deletion occurs with a specific version ID, it will **not** be replicated. This is to prevent malicious or accidental permanent deletions from propagating across buckets.
- There is no chaining of replications. 🔗 If Bucket 1 replicates to Bucket 2, and Bucket 2 replicates to Bucket 3, it does not mean that objects from Bucket 1 will be automatically replicated to Bucket 3.

**S3 Storage Classes**
- Amazon S3 Standard - General Purpose:- Default storage class. Big data analytics, mobile and gaming applications, content distribution. 🚀
- Amazon S3 Standard Infrequent Access (IA):- For data accessed less frequently but requiring rapid access when needed. Disaster recovery and backups. 💾
- Amazon S3 One Zone Infrequent Access:- High durability within a single Availability Zone (AZ). ⚠️ Data loss can occur if the AZ is destroyed. Storing secondary copies of backups (e.g., on-premises data) or data that can be recreated. ♻️
- Amazon S3 Glacier Instant Retrieval:- Milliseconds retrieval times. ⚡ Suitable for data accessed once a quarter. Minimum storage duration: 90 days. 
- Amazon S3 Glacier Flexible Retrieval:- Offers flexibility in retrieval times. Minimum storage duration: 90 days. Retrieval Options:
    *   Expedited: 1-5 minutes.
    *   Standard: 3-5 hours.
    *   Bulk: 5-12 hours (free).
- Amazon S3 Glacier Deep Archive:- Lowest-cost storage option for long-term archiving. ⏳ Minimum storage duration: 180 days. Retrieval Options:
    *   Standard: 12 hours.
    *   Bulk: 48 hours.
- Amazon S3 Intelligent-Tiering:- Automatically moves objects between access tiers based on usage patterns. 🧠 Incurs a small monthly monitoring and auto-tiering fee.  

**S3 Express One Zone Storage Class**
- Unlike standard S3 buckets, S3 Express One Zone uses a **directory bucket**.
- Objects are stored in a **single Availability Zone (AZ)**, not replicated across multiple AZs.
- Handles **hundreds of thousands of requests per second**.
- Achieves **single-digit millisecond latency**.
- Offers up to **10x the performance of S3 Standard**.
- **Request** costs are lower than S3 Standard (the course said about 50% lower; AWS cut them again in Apr 2025). **Storage** per GB costs more than S3 Standard ($0.11 vs $0.023 per GB-month in us-east-1).

S3 Express One Zone is ideal when **latency and performance** are top priorities:
* 🔹 **AI & ML training** (e.g., SageMaker model training)
* 🔹 **Financial modeling**
* 🔹 **Media processing**
* 🔹 **High-performance computing (HPC)**
* 🔹 **Data-intensive applications**
* 🔹 **Latency-sensitive apps**

It integrates seamlessly with services like **SageMaker, Athena, EMR, and Glue**, making it a strong choice for big data and analytics workloads.

📝 **Note:** This storage class is best when you need **co-location of compute and storage** within the same AZ, helping reduce latency and even networking costs.

---

## 13. S3 Advance

**1. S3 Analytics**: Amazon S3 Analytics can help determine the optimal number of days to transition objects between storage classes.
  *   It provides recommendations for Standard and Standard IA.
  *   It does not work with One-Zone IA or Glacier.

**2. S3 Event Notifications**: S3 Event Notifications allow you to react automatically to events happening in your Amazon S3 buckets. You can send S3 Event Notifications to the following destinations:
  *   SNS Topic
  *   SQS Queue
  *   Lambda Function
  *   Amazon EventBridge

**3. S3 Baseline Performance**: Amazon S3 is designed to automatically scale to handle a very high number of requests with low latency. Here's a breakdown of its baseline performance and optimization techniques.
  *   Latency: Expect between 100 and 200 milliseconds to get the first byte from S3.
  *   Request Limits
      *   3,500 PUT/COPY/POST/DELETE requests per second per prefix in a bucket OR
      *   5,500 GET/HEAD requests per second per prefix in a bucket.

It's crucial to understand what "per prefix" means in the context of S3 performance. A prefix is essentially the path within your bucket leading to your object. There are no limits to the number of prefixes in your bucket.

### Optimizing S3 Performance

1.  **Multi-Part Upload 🚀**
    *   💡 **Tip:** Recommended for files over 100 MB and *required* for files over 5 GB.
    *   How it works: Divides the file into smaller parts and uploads them in parallel.
    *   Benefit: Speeds up transfers by maximizing bandwidth utilization.

2.  **S3 Transfer Acceleration 🚀**
    *   Use case: Speeds up uploads and downloads.
    *   How it works: Transfers files to an AWS edge location, which then forwards the data to the S3 bucket in the target region over the AWS private network.
    *   Benefit: Minimizes the use of the public internet, leveraging the faster AWS private network.
    *   📝 **Note:** Compatible with multi-part upload.

3.  **S3 Byte Range Fetches 🚀**
    *   Use case: Parallelizes GET requests by retrieving specific byte ranges of a file.
    *   Benefits:
        *   Speeds up downloads.
        *   Improves resilience: If a byte range request fails, you can retry with a smaller range.
        *   Allows retrieving only a portion of a file (e.g., headers).
---

## 14. S3 Security

### Object Encryption in Amazon S3

You can encrypt objects in S3 buckets using one of the following four methods:

1.  Server-Side Encryption (SSE) -> having 3 flavors
2.  Client-Side Encryption

#### 1. Server-Side Encryption (SSE)

There are three flavors of SSE:

##### a. SSE-S3

*   Encryption uses a key that's handled, managed, and owned by AWS. You never have access to this key.
*   Object is encrypted server-side by AWS using AES-256.
*   To request Amazon S3 to encrypt the object using SSE-S3, set the header:

    ```
    "x-amz-server-side-encryption": "AES256"
    ```

*   SSE-S3 is **enabled by default** for new buckets and new objects.

What if you don’t send the `x-amz-server-side-encryption: AES256` header?

* **If the bucket has default SSE-S3 encryption enabled (default for new buckets since Jan 5, 2023)**:
  → **Object will still be encrypted automatically** with SSE-S3.

* **Older buckets (created before Jan 5, 2023)**:
  → Since Jan 5, 2023, S3 applies SSE-S3 as the base encryption to **every** bucket, so new uploads are encrypted there too. An unencrypted upload is no longer possible.

**Best Practice:**
Either send the header or enable default encryption on the bucket to ensure all objects are encrypted.

##### b. SSE-KMS

*   You manage your own keys using the KMS (Key Management Service).
*   Advantages of using KMS:
    *   User control over the key. You can create keys yourself within KMS.
    *   You can audit key usage using CloudTrail. Every key usage is logged.
*   To use SSE-KMS, set the header:

    ```
    "x-amz-server-side-encryption": "aws:kms"
    ```

⚠️ **Warning:** SSE-KMS has limitations. 
- Each API call to KMS counts towards KMS quotas (API calls per second) (**10,000, 20,000 or 100,000** req/s for symmetric keys, based on region — older course notes say 5,500 / 10,000 / 30,000). 
- When you upload, it calls the GenerateDataKey KMS API. 
- When you download, it calls the Decrypt KMS API.
- If you have a very high throughput S3 bucket with everything encrypted using KMS keys, you may encounter throttling. 
- Check your region's limits and consider using the Service Quotas Console to request increases if needed.

##### c. SSE-C

*   Keys are managed outside of AWS (by the client), but encryption is still server-side.
*   Amazon S3 never stores the encryption key you provide; it's discarded after use.
*   **Must use HTTPS.**
*   ⚠️ Since Apr 2026, SSE-C is **disabled by default** on new buckets. You must turn it on (`BlockedEncryptionTypes` = `NONE`) before you can use it.
*   And **must pass the encryption key as part of HTTPS headers for every request.**
*   To read the file, you must provide the same key used for encryption.

#### 2. Client-Side Encryption

*   Clients encrypt data themselves before sending it to Amazon S3.
*   Decryption happens on the client outside of Amazon S3.
*   Clients fully manage the keys and the encryption cycle.
*   💡 **Tip:** Consider using a client library like the Client-Side Encryption Library for easier implementation.

#### Encryption in Transit

Encryption in transit (also called SSL/TLS or "in flight" encryption) secures data while it's being transmitted.

*   Amazon S3 buckets have two endpoints:
    *   HTTP (not encrypted)
    *   HTTPS (encrypted)
*   It's highly recommended to use HTTPS for secure data transmission.
*   If using SSE-C, you *must* use HTTPS.

Most clients use the HTTPS endpoint by default.

#### How to force encryption in transit?

Use a bucket policy to deny any `GetObject` operation if the connection is not secure (i.e., using HTTP).

### Default Encryption vs. Bucket Policies

By default, all new S3 buckets now have default encryption enabled using SSE-S3. This means that new objects added to these buckets are automatically encrypted.

However, you have the flexibility to change the default encryption type. For instance, you could switch to SSE-KMS.

You can also enforce encryption using bucket policies. This involves configuring the bucket policy to reject any API calls that attempt to upload an S3 object without the required encryption headers (e.g., SSE-KMS or SSE-C).

📝 **Note:** Bucket policies are always evaluated *before* default encryption settings. This means that if a bucket policy denies an unencrypted upload, the default encryption setting will not be applied.

In summary:

*   Default encryption is enabled by default using SSE-S3. 🛡️
*   You can change the default encryption type. ⚙️
*   You can use bucket policies to proactively enforce specific encryption types. 🔒

### MFA Delete in S3

MFA Delete is a security feature in Amazon S3 that leverages multi-factor authentication (MFA) to protect against accidental or malicious permanent data loss.

When is MFA required? MFA is mandatory for the following actions:

*   🗑️ Permanently deleting an object version. This provides a safeguard against unintended or malicious permanent deletions.
*   🚫 Suspending Versioning on an S3 bucket. This is also considered a destructive operation.

MFA is **not** required for:

*   ✅ Enabling Versioning.
*   📜 Listing deleted versions.

These actions are not considered dangerous.

To use MFA Delete:

1.  Enable Versioning on the S3 bucket. MFA Delete is intrinsically linked to Versioning.
2.  **Only the bucket owner (root account) can enable or disable MFA Delete.**

⚠️ **Warning:** Using the root account should be minimized, but it's necessary for managing MFA Delete.

📝 **Note:** MFA Delete provides an extra layer of protection against the permanent deletion of specific object versions. It's a crucial security measure for data protection.

### S3 Access Logs

For audit purposes, you might need to log all access attempts to your S3 buckets. This includes every request made to your S3 bucket from any account, regardless of whether the request was authorized or denied. These logs are saved as files in another S3 bucket. You can then analyze this data using tools like Amazon Athena.

📝 **Note:** **The target logging bucket must be in the same AWS region as the bucket you are monitoring.**

How does it work?

1.  Requests are made against your S3 bucket.
2.  You enable access logs on your S3 bucket.
3.  All requests are then logged into the designated logging bucket.

There's a specific format for these logs. You can find the details at the AWS documentation.

⚠️ **Warning:** **Never set the logging bucket to be the same as the bucket you are monitoring!** This will create a logging loop, leading to exponential growth of your bucket size and unexpected costs.

### Amazon S3 Pre-Signed URLs

- Pre-signed URLs are URLs that you can generate using the S3 console, the CLI, or the SDK. The key feature is that the URL has an expiration date.
- Pre-signed URLs are a common solution for providing temporary access to a specific file for either download or upload.

*   Allow only logged-in users to download a premium video from your S3 bucket. 🎬
*   Enable a dynamically changing list of users to download files by generating URLs on the fly. ⚙️
*   Temporarily allow a user to upload a file to a specific location in your S3 bucket while keeping the bucket private. ⬆️

This approach is useful for maintaining your S3 bucket's privacy while providing controlled, temporary access to specific resources.

### S3 Glacier Vault Lock and S3 Object Lock

#### S3 Glacier Vault Lock 🔒

> ⚠️ **Note (2026):** The original standalone Amazon Glacier service (vaults) no longer accepts new customers. Existing vaults keep working. AWS recommends the S3 Glacier storage classes, with **S3 Object Lock** for WORM. Older exam questions may still use Vault Lock.

The primary goal of S3 Glacier Vault Lock is to lock your Glacier Vault to adhere to a **WORM** (Write Once Read Many) model.

*   You place an object into your S3 Glacier Vault.
*   You then lock the vault to prevent any modifications or deletions.

To achieve this:

1.  Create a Vault Lock Policy on your Glacier vault.
2.  Lock the policy itself to prevent future edits.

Once a Vault Lock Policy is set and locked:

*   It cannot be changed or deleted by anyone, including administrators or AWS itself. 🛡️
*   This is extremely useful for compliance and data retention purposes.

If an object is in a Glacier vault with a Vault Lock Policy, the object can never be deleted. This is particularly helpful for legal and compliance requirements.

#### S3 Object Lock 🗄️

S3 Object Lock offers similar functionality to Glacier Vault Lock but at the object level within S3 buckets.

📝 **Note:** To enable S3 Object Lock, you must first enable **versioning** on your S3 bucket.

S3 Object Lock allows you to adopt a WORM model, but the lock is applied to individual objects within the bucket, not the entire bucket. This allows you to block specific object versions from being deleted for a defined period.

##### Retention Modes ⏳

There are two retention modes available:

1.  **Compliance Mode:**

    *   This mode is similar to S3 Glacier Vault Lock.
    *   Object versions cannot be overwritten or deleted by any user, including the root user. 🚫
    *   Retention modes and periods cannot be changed or shortened.
    *   This is the strictest mode for compliance.
2.  **Governance Mode:**

    *   Most users cannot override or delete object versions or alter their lock settings. 🛡️
    *   Admin users with special IAM permissions can change the retention or delete objects directly.
    *   This mode offers more flexibility than Compliance Mode.

In both modes, you must set a **retention period** to specify how long the object should be protected. This period can be extended if needed.

##### Legal Hold ⚖️

In addition to retention modes, you can also place a **legal hold** on an object.

*   A legal hold protects an object indefinitely, regardless of the retention period or mode.
*   Think of it as marking an object as crucial for legal proceedings.
*   Users with the `S3:PutObjectLegalHold` IAM permission can place or remove legal holds.

📌 **Example:** If an object is needed for a trial, a legal hold can be placed on it to ensure it is protected indefinitely.

This provides a flexible way to protect specific objects when needed. Once the legal investigation is over, the legal hold can be removed.

##### Key Differences Summarized 🔑

*   **Glacier Vault Lock:** Applies to the entire Glacier vault.
*   **S3 Object Lock:** Applies to individual objects within an S3 bucket.
*   **Compliance Mode:** Strict, no one can override.
*   **Governance Mode:** More lenient, admins can override.
*   **Legal Hold:** Indefinite protection, independent of retention settings.

### S3 Access Points

With proper IAM permissions, users can access only the access points relevant to their needs. Finance users access the finance access point, sales users access the sales access point, and the analytics group can access both finance and sales through the analytics access point.

Benefits of using access points:

*   Simplified security management. ✅
*   Policies attached to each access point. ✅
*   Simplified bucket policy on Amazon S3. ✅
*   Scalable access to S3 buckets. ✅

To summarize, access points simplify security management for S3 buckets. Each access point has its own DNS name, allowing you to connect to it. You can configure it to be connected to the internet or a VPC for private traffic.

S3 access points can be privately accessible through a VPC origin. An EC2 instance within a VPC can access the S3 bucket without traversing the internet, using a VPC access point and a VPC origin.

To enable this private access, you need to create a **VPC endpoint** to access the access point. This VPC endpoint acts as a secure connection point within your VPC, allowing private access to the access point through the VPC origin.

The VPC endpoint also has a policy that must allow access to the target buckets and the access points. This policy ensures that your EC2 instance can connect to both the access points and the S3 buckets.

Security layers involved in VPC access points:

1.  VPC endpoint policy for security.
2.  Access point policy for security.
3.  S3 bucket level security.

### S3 Object Lambda

> ⚠️ **Note (2026):** S3 Object Lambda is closed to new customers since 7 Nov 2025. Existing customers can still use it.

Another use case for S3 access points is with S3 Object Lambda. The core idea is to modify an object in an S3 bucket through a Lambda function just before it's retrieved by an application. This avoids duplicating buckets for different versions of the same object.

Only one S3 bucket is needed, on top of which we create S3 Access Point and S3 Object Lambda access points.

Use cases for S3 Object Lambda include:

*   Redacting PII (Personally Identifiable Information) for analytics or non-production environments. 🛡️
*   Converting data formats (e.g., XML to JSON).
*   Performing any kind of data transformation.
*   Resizing and watermarking images on the fly, where the watermark is specific to the user requesting the object. 🖼️

---

## 15. CloudFront & AWS Global Accelerator

- If you see "CDN" then think about CloudFront.

### CloudFront Origins

CloudFront supports multiple **origin types** (i.e., backends):

- **Amazon S3 buckets**:
  - Used for **static files distribution** and **Edge caching**.
  - Supports **file uploads** to S3 via CloudFront.
  - Secured using **Origin Access Control (OAC)**.

- **VPC-based origins**:
  - For applications hosted in **private subnets**.
  - Can include:
    - **Application Load Balancer (ALB)**
    - **Network Load Balancer (NLB)**
    - **EC2 instances**

- **Custom HTTP origins**:
  - Any backend accessible via HTTP.
  - 💡 **Tip:** For S3 websites, ensure the **S3 bucket is configured as a static site**.

💡 **Tip:** Use **CloudFront** for **fast, cached delivery**, and **S3 Replication** for **real-time content syncing** between regions.

#### ✅ Summary

- CloudFront is AWS's **CDN** that caches and distributes content globally.
- It improves **performance**, **reduces latency**, and enhances **security**.
- Supports multiple **origin types** (S3, VPC, Custom HTTP).
- **CloudFront ≠ S3 replication**—they solve different problems.

### Connecting CloudFront to Application Load Balancers or EC2 Instances

**VPC Origins (Recommended) 🚀**

This is the preferred and more modern approach. VPC origins allow you to deliver content directly from applications hosted in your private subnets within your Virtual Private Cloud (VPC). This ensures that your backend infrastructure remains private and doesn't need to be exposed to the public internet.

With VPC origins, you can deliver traffic to:
*   Private Application Load Balancers (ALBs)
*   Network Load Balancers (NLBs)
*   EC2 instances

Here's how it works:

1.  A user accesses your content through a CloudFront distribution, which utilizes a network of edge locations.
2.  From CloudFront, you create a VPC origin.
3.  This VPC origin is then connected to your backend resources (ALB, NLB, or EC2 instance).
4.  CloudFront uses the VPC origin to route traffic to your private subnets and applications.

From a network security standpoint, this is a highly secure setup. Your applications remain hosted privately, and you control what is exposed through CloudFront.

### CloudFront Geo Restriction

You can restrict access to your CloudFront distribution based on the user's country of origin. This allows you to control who can access your content based on geographic location.

You have two options:

*   **Allowlist:** Define a list of approved countries. Only users from these countries can access your distribution. ✅
*   **Blocklist:** Define a list of banned countries. Users from these countries will be blocked from accessing your distribution. 🚫

The country is determined by matching the user's IP address to a country using a third-party Geo-IP database. 🌍

A common use case for geo restriction is to comply with copyright laws and control access to content based on regional restrictions. 📝 **Note:** Geo-restrictions are not foolproof and can be bypassed by sophisticated users.

### CloudFront Price Classes

To reduce costs, you can limit the number of edge locations used by your CloudFront distribution by using Price Classes. There are three options:

1.  **Price Class All:** 🌐
    *   Includes all regions.
    *   Offers the best performance.
    *   Most expensive option.
2.  **Price Class 200:** 🌍+
    *   Includes most regions.
    *   Excludes the most expensive regions.
    *   A balance between cost and performance.
3.  **Price Class 100:** 🌎
    *   Includes only the least expensive regions (North America and Europe).
    *   Least expensive option.
    *   May result in higher latency for users outside of these regions.

### Cache Invalidations in CloudFront

CloudFront uses a backend origin to serve content. When you update the content in your origin, the CloudFront edge locations won't automatically reflect these changes until the Time-To-Live (TTL) of the cached content expires. This delay might be undesirable if you need the updated content to be served immediately.

To address this, you can force a full or partial cache refresh using **CloudFront invalidations**. This eliminates the existing TTL and ensures the latest content is served.

Here's how it works:

You specify the file paths to invalidate. You can invalidate:
  *  All files using a wildcard (`*`).
  *  Specific paths, like `/images/*`.

### AWS Global Accelerator

**AWS Global Accelerator** is a powerful service designed to improve the performance and availability of your global applications by using AWS's global network infrastructure.

#### ⚙️ Key Features

- 🧠 **Intelligent Routing**: Traffic automatically directed to the lowest latency endpoint.
- 🔁 **Fast Failover**:
  - Built-in **health checks** for backends.
  - Automatic failover to healthy regions in **under 1 minute**.
- 🔐 **Security**:
  - Only **two static IPs** to whitelist.
  - Built-in **DDoS protection** via **AWS Shield**.
- 📈 **Consistent Performance**: Leverages AWS’s optimized global network.

📝 **Note:** Since the IPs are static, there's no issue with client-side caching. Clients always connect to the same IPs.

#### ✅ Summary

- **AWS Global Accelerator** is a **global traffic manager** that uses **Anycast IPs** and **AWS’s global network** to route traffic to the **nearest healthy application endpoint**.
- It is ideal for:
  - 🔹 Real-time applications (e.g., gaming, IoT, VoIP)
  - 🔹 Applications needing static IPs globally
  - 🔹 Scenarios requiring fast, reliable **regional failover**
- Provides consistent, low-latency performance and high availability.

🛡️ **Security** is built-in via AWS Shield, and only two static IPs are exposed to clients.

---

## 16. AWS Storage Extra Options

### AWS Snowball 

> ⚠️ **Note (2026):** Snowmobile was retired in 2024. Snowball Edge is closed to new customers since 7 Nov 2025 (existing customers can still order it). AWS points new customers to DataSync, AWS Data Transfer Terminal, or Outposts for edge compute. Older exam questions may still use Snowball.

AWS Snowball is a highly secure and portable device designed for two primary use cases:

*   Collecting and processing data at the edge.
*   Migrating data in and out of AWS.

If you're dealing with petabytes of data for migration, Snowball is worth considering.

#### Snowball Edge Devices 📦

There are two main types of Snowball Edge devices:

1.  **Edge Storage Optimized:** Primarily for storage. Capacity: 210 TB
2.  **Edge Compute Optimized:** Primarily for compute. Capacity: 28 TB

Snowball:
  1.  Receive a physical Snowball device.
  2.  Load the device with your data.
  3.  Ship the device back to AWS.
  4.  AWS imports the data from the Snowball to, for example, an Amazon S3 bucket.

#### Edge Computing 🖥️

Snowball can also be used for edge computing, processing data where it's created. 📌 **Example:** Trucks, ships, or mining stations with limited or no internet access.

In these scenarios, you can order a Snowball Edge device to perform computations locally. The **Edge Compute Optimized** device is designed for this.

**Capabilities:**

*   Run EC2 instances directly on the device.
*   Execute Lambda functions directly on the device.

**Benefits:**

*   Pre-process data at the edge.
*   Perform machine learning at the edge.
*   Transcode media at the edge.

Once processed, the data can be sent back to AWS.

📝 **Note:** The Snowball service is primarily for data migrations and edge computing.

#### Snowball to Glacier Data Import

Unfortunately, Snowball cannot directly import data into Glacier. ⚠️ **Warning:** This is a key limitation to remember.

The solution involves a two-step process using Amazon S3 as an intermediary:

1.  **Snowball Import:** Use Snowball to import the data into Amazon S3. 📦
2.  **Lifecycle Policy:** Create an S3 lifecycle policy to transition the objects from S3 into Amazon Glacier. 🔄

---

## 17. AWS Integration and Messaging Services

| **Key** | **Service** | **Description** |
| --- | --- | --- |
| Decouple Applications, Sudden Spikes load, Timeouts, The need for rapid scaling | **SQS** | Applications that handle unpredictable traffic patterns or sudden spikes in load. |
| Real-time Streaming | **Kinesis Data Streams** | Real-time data is data created and used immediately. |
| Near real-time | **Amazon Data Firehose** | Data is accumulated in the buffer and then flushed to the destination. |

**💡Horizontal vs Vertical - Easy way to remember**
* **Vertical ( | ) = Up** (make one machine taller/stronger).
* **Horizontal ( __ ) = Out** (add more machines side by side).

### a. SQS

| **Description**                 | **Value**                                                                                                      |
| ------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| **ChangeMessageVisibility**     | API action to change the visibility timeout of a specific message in a queue.                                  |
| **WaitTimeSeconds**             | Duration (0–20 seconds) for long polling to wait for a message before returning a response.                    |
| **ApproximateNumberOfMessages** | An attribute that returns the approximate count of available (not delayed or in-flight) messages in the queue. |
| **Default Retention Period**    | 4 days (minimum: 1 minute, maximum: 14 days).                                                                  |
| **Max Message Size**            | 1024 KB (1 MB).                                                                                                |
| **Default Visibility Timeout**  | 30 seconds (can be set from 0 seconds up to 12 hours).                                                         |

### b. FIFO SQS

| **Description**                                            | **Value**                                                                                                |
| ---------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| **Standard FIFO throughput (per queue, without batching)** | Up to **300 messages per second** (send, receive, or delete).                                            |
| **With batching (up to 10 messages per batch)**            | Up to **3,000 messages per second** (send, receive, or delete).                                          |
| **High Throughput Mode (enabled)**                         | Up to **70,000 transactions per second** per API action without batching in us-east-1, us-west-2 and eu-west-1 (lower in other Regions; the course says 3,000). |
| **High Throughput Mode with batching**                     | Up to **700,000 messages per second** in those Regions (the course says 30,000).                         |
| **Message Grouping**                                       | Ordering is guaranteed within a **MessageGroupId**. Throughput scales with the number of message groups. |

⚠️ **Note:**

* High throughput mode must be enabled explicitly, when you create or edit the FIFO queue.
* Deduplication and ordering features still apply in FIFO.

| **Description**        | **Value**                                                                                                                                                                                 |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Message Group ID**   | A tag that specifies a group of related messages. Ensures that messages with the same **MessageGroupId** are processed in strict order, one at a time.                                    |
| **.fifo (Queue Name)** | All FIFO queue names must end with the suffix **`.fifo`**.                                                                                                                                |
| **Deduplication ID**   | Used to avoid duplicate message delivery. It can be explicitly provided with each message, or if **content-based deduplication** is enabled, SQS uses a SHA-256 hash of the message body. |
| **Deduplication Interval** | The fixed **5-minute window** during which SQS uses the **Deduplication ID** (or message body hash if content-based deduplication is enabled) to prevent duplicate messages from being accepted. |

### c. Kinesis Data Streams

| **Description**                  | **Value**                                                                                                              |
| -------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **Producers**                    | Applications, Kinesis Agent                                                                                            |
| **Consumers**                    | Applications, AWS Lambda, Amazon Kinesis Data Firehose, Amazon Managed Service for Apache Flink                        |
| **Data Retention**               | Up to **365 days**                                                                                                     |
| **Data Deletion**                | Once data is ingested, it **cannot be deleted** manually — you must wait until it expires based on retention settings. |
| **Max Record Size**              | **1 MiB per record** by default; up to **10 MiB** if you opt in (since Oct 2025)                                       |
| **High-Throughput Producers**    | Use the **Kinesis Producer Library (KPL)**                                                                             |
| **Optimized Consumers**          | Use the **Kinesis Client Library (KCL)**                                                                               |
| **Capacity Modes**               | **Provisioned Mode** and **On-Demand Mode**                                                                            |
| **Provisioned Mode (per shard)** | - **Inbound:** 1 MB/sec or 1,000 records/sec<br> - **Outbound:** 2 MB/sec                                              |
| **📌 Example (Provisioned)**     | To send **10,000 records/sec** or **10 MB/sec**, you need **10 shards**                                                |

### d. Amazon Data Firehose

| **Description**                    | **Value**                                                                                                                                                                                                                                          |
| ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Data Ingestion**                 | - Producers (applications, clients, custom code) use the **AWS SDK**.<br> - **Kinesis Agent** can also be used.<br> - Firehose can pull data directly from:<br>   • Kinesis Data Streams<br>   • Amazon CloudWatch Logs and Events<br>   • AWS IoT |
| **Data Transformation (Optional)** | - Records can be transformed using a **Lambda function**.<br> - Useful for data conversion or formatting.<br> 📌 **Example:** Converting CSV to JSON.                                                                                              |
| **Buffering and Batch Writing**    | - Records are accumulated into a **buffer**.<br> - Buffer is flushed periodically to write data in **batches** to destinations.                                                                                                                    |
| **Destinations**                   | - **AWS:** Amazon S3 🗄️, Amazon Redshift 📊, Amazon OpenSearch 🔍<br> - **Third-Party Partners:** Datadog, Splunk, New Relic, MongoDB<br> - **Custom:** HTTP Endpoint Integration (any destination)                                               |
| **Backup**                         | - Firehose can write **all data** or only **failed data** to an **S3 bucket** for backup purposes. |

**Comparison: Kinesis Data Streams vs. Amazon Data Firehose**

| Feature             | Kinesis Data Streams                               | Amazon Data Firehose                                  |
| ------------------- | -------------------------------------------------- | ----------------------------------------------------- |
| Purpose             | Streaming data collection                            | Loading streaming data into target destinations        |
| Code                | Requires writing producer and consumer code        | Fully managed                                         |
| Real-time           | Real-time                                          | Near real-time                                        |
| Scaling             | Provisioned and On-Demand Modes                      | Automatic Scaling                                     |
| Data Storage        | Up to one year                                     | No data storage                                       |
| Replay Capability   | Yes                                                | No                                                    |

---

## 18. ECS ECR EKS

| **Key** | **Service** | **Description** |
| --- | --- | --- |
| Microservices, Lift-and-shift app from on-premise to cloud | **Docker** | Package application and its dependencies into a single container that can be run anywhere. |
| **Container Storage Interface** (CSI) | **Amazon EKS** | An interface for exposing storage systems to containerized orchestration systems. |
| Migrate and modernize Java and .NET web applications | **AWS App2Container (A2C)** | A command-line interface (CLI) tool designed to migrate and modernize Java and .NET web applications into Docker containers. |
---

## Serverless

| Key | Service | Description |
| --- | --- | --- |
| Schema needs to evolve rapidly | **DynamoDB** | DynamoDB is an excellent choice when your schema needs to evolve rapidly. |
| Unpredictable workloads or sudden spikes on DynamoDB | **On-Demand Mode of DynamoDB** | On-Demand Mode of DynamoDB is a great choice for unpredictable workloads or sudden spikes on DynamoDB. |
| Very few transactions (e.g 4-5 times a day) on DynamoDB | **On-Demand Mode of DynamoDB** | On-Demand Mode of DynamoDB is a great choice for very few transactions (e.g 4-5 times a day). |
| Microseconds latency for Cached Data | **DynamoDB Accelerator (DAX)** | DynamoDB Accelerator (DAX) is a fully-managed, highly available, and seamless in-memory cache for DynamoDB. |
| "Hundreds of Users", "Mobile Users", "Authenticate with SAML" | **Cognito** | Cognito is a great choice for "Hundreds of Users", "Mobile Users", "Authenticate with SAML". |

## Databases

| Key | Service | Description |
| --- | --- | --- |
| RDBMS databases, OLTP (Online Transaction Processing) databases, SQL queries, transactions | **Amazon RDS** | Amazon RDS is a fully managed relational database service that makes it easy to build and run applications that work with highly connected datasets. |
| Caching solution does not require code changes | **Don't Use ElastiCache** | You must modify your application code to leverage ElastiCache. |
| Flexible and Evolving Schema | **DynamoDB** | DynamoDB is an excellent choice when your schema needs to evolve rapidly. |
| **MongoDB** | **DocumentDB** | DocumentDB is AWS's answer to a cloud-native version of MongoDB, similar to how Aurora is for PostgreSQL and MySQL. |
| **Graph Database** | **Amazon Neptune** | Amazon Neptune is a fully managed graph database service that makes it easy to build and run applications that work with highly connected datasets. |
| Social Networks, Knowledge Graphs, Fraud Detection, Recommendation Engines | **Amazon Neptune** | Amazon Neptune is usually used for Social Networks, Knowledge Graphs, Fraud Detection, Recommendation Engines. |
| **Apache Cassandra** | **Amazon Keyspaces** | Amazon Keyspaces is a fully managed, serverless, Apache Cassandra-compatible database service. You use the same Cassandra (CQL) code and tools. |
| IOT Device Information Storage, Time series data storage | **Amazon Keyspaces** | Amazon Keyspaces is usually used for IOT Device Information Storage, Time series data storage. |
| Time series data storage | **Amazon Timestream** | Amazon Timestream is a fully managed, fast, scalable, and serverless time series database. ⚠️ Timestream for LiveAnalytics is closed to new customers since 20 Jun 2025; Timestream for InfluxDB is still open. |
| Secure, immutable (once data is written, it can't be altered/deleted), cryptographically verifiable financial records | **Amazon QLDB** | Amazon QLDB is a ledger database purpose-built to provide a transparent, immutable, and cryptographically verifiable transaction log. ⚠️ QLDB reached end of support on 31 Jul 2025; AWS points to Aurora PostgreSQL. |

## 22. Data & Analytics

| Key | Service | Description |
| --- | --- | --- |
| Analyze data in S3 using serverless SQL | **Amazon Athena** | Amazon Athena is a serverless query service that enables you to analyze data stored in Amazon S3 buckets. |
| OLAP (Online Analytical Processing) | **Amazon Redshift** | Amazon Redshift is a fully managed, serverless data warehouse service that makes it easy to build and run analytics workloads. |
| Query data directly from S3 without loading it to Redshift | **Redshift Spectrum** | Redshift Spectrum is a serverless query service that enables you to query data stored in Amazon S3 buckets directly from your Redshift cluster. |
| Full-text search, partial match, and analytics | **Amazon OpenSearch** | Amazon OpenSearch is a flexible, scalable search and analytics engine that fits into a variety of real-time data processing architectures. |
| Big Data Clusters, Hadoop Clusters | **Amazon EMR** | Amazon EMR (Elastic MapReduce) is AWS’s managed service for creating and running Hadoop clusters to process and analyze large-scale big data. |
| Machine-learning powered business intelligence tool | **Amazon QuickSight** | Amazon QuickSight is a machine-learning powered business intelligence tool that enables users to create interactive dashboards and perform ad-hoc data analysis. |
| In-memory computation in Amazon QuickSight | **SPICE (Super-fast, Parallel, In-memory Calculation Engine)** | SPICE is a super-fast, parallel, in-memory calculation engine for Amazon QuickSight. It only works when data is imported directly into Amazon QuickSight, not when connecting to external databases. | 
| Extract, transform, and load (ETL) Service | **AWS Glue** | AWS Glue is a fully managed data integration service that makes it easy to extract, transform, and load data. |
| Convert data into Parquet Format | **AWS Glue** | AWS Glue can be used to convert data into Parquet format. |
| Prevents reprocessing old data when running a new ETL job | **AWS Glue Job Bookmarks** | AWS Glue Job Bookmarks can be used to prevent reprocessing old data when running a new ETL job. |
| Manage security in one central place | **AWS Lake Formation** | AWS Lake Formation is a fully managed service that simplifies data lake setup, reducing the time from months to just a few days. |
| Processing data streams | **Managed Service for Apache Flink** | Managed Service for Apache Flink is a fully managed Apache Flink service that makes it easy to process data streams. |
| Manage IoT Devices | **AWS IoT Core** | AWS IoT Core is a fully managed service that makes it easy to manage IoT devices. |

📌 **Exam Tip:** 
- Flink can read from Kinesis Data Streams, but it **cannot** read from Amazon Data Firehose. This is a common exam trick! 🚨
- Remember that **Kinesis → shards**, while **Kafka/MSK → topics & partitions**

## 23. Machine Learning

| Key | Service | Description |
| --- | --- | --- |
| Find objects, People, Text, Scenes in Images and Videos | **Amazon Rekognition** | Amazon Rekognition is a fully managed service that makes it easy to detect and recognize faces, labels, text, and custom objects in images and videos. |
| Content Moderation | **Amazon Rekognition** | Amazon Rekognition has content moderation capabilities. |
| Speech to Text | **Amazon Transcribe** | Amazon Transcribe is a fully managed service that makes it easy to transcribe speech to text. |
| PII (Personally Identifiable Information) Redaction, and Automatic Language Identification | **Amazon Transcribe** | Amazon Transcribe has PII (Personally Identifiable Information like age, name, social security number) Redaction, and Automatic Language Identification capabilities. |
| Text to Speech | **Amazon Polly** | Amazon Polly is a fully managed service that makes it easy to convert text to speech. |
| Pronunciation of Stylish Words and Acronyms | **Amazon Polly -> Pronunciation Lexicons Feature** | Amazon Polly has a feature called Pronunciation Lexicons that can be used to customize speech synthesis Like spelling "AWS" to "Amazon Web Services" |
| Customization on how words are pronounced | **Amazon Polly -> SSML (Speech Synthesis Markup Language) Feature** | Amazon Polly has a feature called SSML (Speech Synthesis Markup Language) that can be used to customize speech synthesis. |
| Localize Content | **Amazon Translate** | Amazon Translate is a natural and accurate language translation service. 🌍 It allows you to localize content, such as websites and applications, for your international users. This service efficiently translates large volumes of text. |
| Automatic Speech Recognition (ASR) & Natural Language Understanding (NLU) | **Amazon Lex** | Amazon Lex has capabilities for Automatic Speech Recognition (ASR) [Converts speech into text] and Natural Language Understanding (NLU) [Understands the intent of text and sentences]. Amazon Lex helps you build chatbots or call center bots. 🤖|
| Building Visual contact center | **Amazon Connect** | Speaking of call centers, Amazon Connect is a visual contact center service that allows you to Receive calls 📞, Create contact flows, integrate with CRM. |
| NLP (Natural Language Processing) | **Amazon Comprehend** | Amazon Comprehend is a fully managed service that makes it easy to extract insights and relationships from your text data. It can be used for sentiment analysis, entity detection, and more. |
| Detect and return useful information from unstructured clinical text | **Amazon Comprehend Medical** | Amazon Comprehend Medical uses NLP to identify the useful information from unstructured clinical text. |
| Build ML Models | **Amazon SageMaker** | Amazon SageMaker is a fully managed service that makes it easy to build, train, and deploy ML models. |
| Document Search Service | **Amazon Kendra** | Amazon Kendra allows you to extract answers directly from within your documents |
| Machine learning service for building recommendations and personalized experiences | **Amazon Personalize** | Amazon Personalize is a fully managed ML service that builds real-time personalized recommendations (for example, product recommendations) from your user and item data. |
| Extract text, handwriting, and data from scanned documents | **Amazon Textract** | Amazon Textract is a fully managed service that makes it easy to extract text, handwriting, and data from scanned documents. |

## 24. AWS Monitoring & Auditing

| Key | Service | Description |
| --- | --- | --- |
| Cron Jobs | **AWS CloudWatch Events** | AWS CloudWatch Events allows you to schedule cron jobs in the cloud. |
| Collect, aggregate, and summarize metrics and logs from your containers | **AWS CloudWatch Container Insights** | AWS CloudWatch Container Insights allows you to extract metrics and logs from your containers and visualize them in detailed dashboards within CloudWatch.  It provides metrics and logs from ECS, EKS, Kubernetes on EC2, and Fargate. Requires an agent for Kubernetes |
| Monitoring and troubleshooting solution for serverless applications running on AWS Lambda | **AWS CloudWatch Lambda Insights** | AWS CloudWatch Lambda Insights provides a monitoring and troubleshooting solution for serverless applications running on AWS Lambda. |
| See Metrics about the top-N contributors | **AWS CloudWatch Contributor Insights** | AWS CloudWatch Contributor Insights analyzes logs and creates time series that display contributed data. It helps to find top talkers on the network and understand who is impacting system performance. This allows you to find the top 10 IP addresses generating traffic on your VPC and determine if they are legitimate or malicious. |
| Automated dashboard that shows potential problems with monitored applications (running on Amazon EC2) | **AWS CloudWatch Application Insights** | AWS CloudWatch Application Insights provides an automated dashboard that shows potential problems with monitored applications. |
| Governance, compliance, and auditing capabilities for your AWS accounts | **AWS CloudTrail** | AWS CloudTrail provides governance, compliance, and auditing capabilities for your AWS accounts. |
| Auditing and compliance recording for your AWS resources | **AWS Config** | AWS Config provides auditing and compliance recording for your AWS resources. Record configurations and their changes over time. ⏱️ Quickly roll back to previous configurations. ⏪ Identify what happened in your infrastructure. 🔍 Config helps answer questions like: Is there unrestricted SSH access to my security groups? 🔓 Do my buckets have public access? 🗄️ Has an ALB configuration changed over time? ⚙️ Based on rule compliance, you can receive alerts or SNS notifications for any changes. 🔔|

⚠️ **Warning:** **Config Rules are for compliance monitoring only. They do not prevent actions and do not replace security mechanisms** like IAM. Config provides an overview of your configuration and resource compliance. It does not prevent actions. If you want to prevent actions, use AWS Firewall Manager or AWS WAF.

## 25. IAM Advance

- How to enforce consistent tagging across multiple accounts? Always think of **AWS Organizations Tag Policies**.

IAM conditions:-
- **aws:SourceIP**: Restrict API calls based on the client's IP address.
- **aws:RequestedRegion**: Restrict API calls based on the region the request is sent to.
- **aws:MultiFactorAuthPresent**: This condition enforces multi-factor authentication (MFA).
- **aws:PrincipalOrgID**: This condition restricts resource policies to accounts within an AWS Organization.
- **ec2:ResourceTag** && **aws:PrincipalTag**: `ec2:ResourceTag` applies to tags on EC2 instances, while `aws:PrincipalTag` applies to user tags. The given below policy allows starting and stopping EC2 instances only if the instance has the tag `Project=DataAnalytics` and the user has the tag `Department=Data`. This allows for fine-grained access control based on both resource and user attributes. 🏷️

```json
{
  "Effect": "Allow",
  "Action": [
    "ec2:StartInstances",
    "ec2:StopInstances"
  ],
  "Resource": "*",
  "Condition": {
    "StringEquals": {
      "ec2:ResourceTag/Project": "DataAnalytics",
      "aws:PrincipalTag/Department": "Data"
    }
  }
}
```

📝 **Note:** Bucket-level permissions (e.g., `ListBucket`) require specifying the bucket ARN directly (`arn:aws:s3:::test`). Object-level permissions (e.g., `GetObject`, `PutObject`, `DeleteObject`) require specifying the object ARN with `/*` to represent all objects within the bucket (`arn:aws:s3:::test/*`).

```json
{
  "Effect": "Allow",
  "Action": "s3:ListBucket",
  "Resource": "arn:aws:s3:::test"
},
{
  "Effect": "Allow",
  "Action": [
    "s3:GetObject",
    "s3:PutObject",
    "s3:DeleteObject"
  ],
  "Resource": "arn:aws:s3:::test/*"
}
```

### IAM Roles vs. Resource-Based Policies: Key Differences

When dealing with **cross-account access**, especially for API calls to services like S3, you have two primary options:

*   **IAM roles**:- When you assume an IAM role (user, application or service):
    *   You give up your original permissions.
    *   You inherit all the permissions associated with the assumed role.
    * 💡 **Tip:** Think of it as temporarily becoming the role. You can only do what the role is permitted to do.
*   **Resource-based policies** (e.g., S3 bucket policies):- With resource-based policies:
    *   The principal (e.g., user) does not assume a role.
    *   The principal retains their original permissions.
    * **📌 Example:** A user in Account A needs to scan a DynamoDB table in Account A and then write the data to an S3 bucket in Account B. Using a resource-based policy allows the user to both scan the DynamoDB table (using their original permissions) and write to the S3 bucket in Account B (because the bucket policy grants them permission).

Here's a summary of common services and their typical integration method with EventBridge:

*   **Resource-Based Policies:** S3 buckets, SNS, SQS, Lambda functions, API Gateway
*   **IAM Roles:** Kinesis Data Streams, EC2 Auto Scaling, System Manager Run Command, ECS task

⚠️ **Warning:** While Kinesis Data Streams supports resource-based policies, EventBridge currently uses IAM roles.

### IAM Permission Boundaries

IAM Permission Boundaries are an advanced feature supported for users and roles, but **not** for groups. They allow you to define the maximum permissions an IAM entity can have. 

The evaluation process involves several steps:

1.  **Explicit Deny:** If there's an explicit deny in any policy, the action is denied.
2.  **Organizations SCP:** Is there an allow? If not, the action is denied (implicit deny).
3.  **Resource-Based Policy:** (e.g., S3 bucket policies, SQS policies). Is there an allow? If not, continue to the next step.
4.  **Identity-Based Policy:** (attached to users, groups, or roles). Is there an allow? If not, continue to the next step.
5.  **IAM Permission Boundaries:** Is the action within the boundary? If not, the action is denied.
6.  **Session Policies:** (related to STS).

Only if **all** applicable policies allow the action (and no policy explicitly denies it) will the action be permitted.

### Microsoft Active Directory and AWS Directory Services

AWS Directory Services provides a way to create an Active Directory on AWS. There are three main flavors:

1.  AWS Managed Microsoft AD
2.  AD Connector
3.  Simple AD

📝 **Note:** The exam may contain high-level questions asking which service to use based on specific requirements:

*   Proxy users to on-premise: Use AD Connector.
*   Manage users in AWS with MFA: Use AWS Managed AD.
*   Need a simple AD without on-premise integration: Use Simple AD.

---

## 27. Networking & VPC

| Key | Service | Description |
| --- | --- | --- |
| IP Multicast | Transit Gateway | Transit Gateway is the only AWS service that supports IP multicast. |

- 1 AWS region can have 5 VPCs by default (a soft quota — you can ask for more).
- You can associate up to 5 IPv4 CIDR blocks per VPC by default (one primary + up to 4 secondary).
- You can associate up to 5 IPv6 CIDR blocks per VPC by default.

### VPC Endpoints

VPC endpoints allow you to privately access AWS services without traversing the public internet. Consider a scenario where an EC2 instance in a private subnet needs to access Amazon SNS. Without a VPC endpoint, the traffic would flow through the NAT gateway, then the internet gateway, and finally to the public Amazon SNS endpoint. This is inefficient and potentially costly. By deploying a VPC endpoint within your VPC, the EC2 instance can directly access Amazon SNS without ever leaving the AWS network. 🚀

There are two types of VPC endpoints:

1.  Interface Endpoints:- Powered by PrivateLink. Provision an Elastic Network Interface (ENI) with a private IP address in your VPC. The ENI serves as an entry point to your private AWS service. Requires attaching a security group. Supports most, if not all, AWS services. Incur a cost per hour and per gigabyte of data processed. 💰
2.  Gateway Endpoints:- Provision a gateway that must be used as a target in a route table. Do not use IP addresses or security groups. Limited to Amazon S3 and DynamoDB. Free to use and scale automatically. 💸

### VPC Flow Logs

VPC Flow Logs allow you to capture information about IP traffic going into network interfaces. This can be configured at the:
- VPC level
- Subnet level
- Elastic Network Interface (ENI) level

These logs can be sent to:
- Amazon S3
- CloudWatch Logs
- Kinesis Data Firehose

### Site-to-Site VPN, Virtual Private Gateway (VGW) & Customer Gateway (CGW)

A **Site-to-Site VPN** allows **private, encrypted communication** between: Your AWS VPC, and Your corporate (on-premises) data center. It uses the **public internet** but creates a **private tunnel** between networks.

1. **Virtual Private Gateway (VGW)**:-  
   - Deployed on the AWS side  
   - Acts as a **VPN concentrator**
   - Must be **created and attached** to your VPC from which you want to create the Site-to-Site VPN connection 
   - You can optionally **customize the ASN (Autonomous System Number)**

2. **Customer Gateway (CGW)**  
   - Deployed on the **corporate data center side**
   - Can be a **physical device** or **software appliance**
   - Must be **publicly routable**, or placed **behind a NAT device** with NAT-T enabled.
   - See more: [Customer Gateway Devices](https://docs.aws.amazon.com/vpn/latest/s2svpn/your-cgw.html#DevicesTested)

### AWS VPN CloudHub

You have a **VPC** with a **Virtual Private Gateway (VGW)**. You also have **multiple customer networks** or **data centers**, each with its own **Customer Gateway (CGW)**. **CloudHub** allows these customer networks to communicate securely with one another using multiple VPN connections.

Key Characteristics ✅

* **Public Internet transport:** All traffic flows over the **public internet**, not through a private network.
* **Encryption:** Even though it uses the public internet, all communication is **encrypted** via VPN.
* **Flexibility:** Can be used for **primary** or **secondary** connectivity between different locations.

Setup Process 🛠️

1. Create multiple **site-to-site VPN connections** on the same **Virtual Private Gateway**.
2. Enable **dynamic routing**.
3. Configure your **route tables**.

### Direct Connect (DX)

Direct Connect (DX) provides a dedicated, private connection from your remote network into your VPC.

*   You need to set up a Direct Connect connection.
*   This connection uses an AWS Direct Connect location.
*   You also need to set up a virtual private gateway (VGW) on your VPC side to enable connectivity between your on-premise data center and AWS.

The same connection can access both:

*   Public resources (e.g., Amazon S3) using a public Virtual Interface (VIF).
*   Private resources (e.g., EC2 Instances) using a private VIF.

**Use Cases for Direct Connect**

*   **Increased Bandwidth Throughput:** 🚀 Faster data transfer for large datasets because traffic doesn't traverse the public internet.
*   **Lower Cost:** 💰 Utilizing a private connection can be more cost-effective than public internet.
*   **Consistent Network Experience:** 🌐 More reliable connectivity compared to the public internet, especially beneficial for applications using real-time data feeds.
*   **Hybrid Environments:** ☁️ Supports connectivity between your on-premises data center and the cloud.
*   Supports both IPv4 and IPv6.

**Direct Connect Gateway**

To connect to one or more VPCs in different regions, use a Direct Connect gateway.

1.  Establish a Direct Connect connection.
2.  Use a private VIF to connect to the Direct Connect gateway.
3.  The Direct Connect gateway will have a private virtual interface to a virtual private gateway in each region.

This setup allows you to connect to multiple VPCs across multiple regions.

**Connection Types**
- Dedicated Connection: Capacities: 1 Gbps, 10 Gbps, 100 Gbps, or 400 Gbps (400 Gbps added Jul 2024 at select locations). Provides a physical Ethernet port dedicated to you. Request is made to AWS and completed by an AWS Direct Connect partner.
- Hosted Connection: Capacities: 50 Mbps, 500 Mbps, up to 25 Gbps (raised from 10 Gbps in Apr 2024). Connection requests are made via AWS Direct Connect Partners. Allows adding capacity on demand. Available at select locations. 

⚠️ Warning: Setting up either a dedicated or hosted connection often takes longer than one month.

💡 Tip: In the exam, if a question asks about transferring data within a week and requires high speed, Direct Connect is likely NOT the answer unless a connection is already established. Consider the time required to establish the connection.

By default, Direct Connect does not encrypt data. It is a private connection, but not encrypted. To add encryption: Set up Direct Connect alongside a VPN to provide an IPsec encrypted private connection. This adds an extra layer of security but increases complexity. This encrypts all traffic between your corporate data center and AWS.

Two modes of resiliency for Direct Connect:
*   **High Resiliency for Critical Workloads:**
    *   Set up multiple Direct Connects.
    *   Use two corporate data centers and two different Direct Connect locations.
    *   Each location has a private VIF.
    *   Provides redundancy if one Direct Connect location goes down.
*   **Maximum Resiliency for Critical Workloads:**
    *   Use two Direct Connect locations.
    *   Each Direct Connect location has two independent connections.
    *   This results in four connections across two locations going into AWS.

**Direct Connect with VPN Backup**

* Your primary connection is established via **Direct Connect**. 💰 This offers a dedicated, high-performance link.
* However, Direct Connect can be expensive, and it's susceptible to occasional outages. ⚠️

To ensure continuous connectivity, consider these options:

1.  **Secondary Direct Connect:** Using a second Direct Connect connection as a backup. This is costly. 💸
2.  **Site-to-Site VPN:** Implementing a Site-to-Site VPN connection over the public internet as a backup. This is a more cost-effective solution. 🌐

In summary:
*   **Primary Connection:** Direct Connect (fast, dedicated, expensive)
*   **Backup Connection:** Site-to-Site VPN (reliable, cost-effective)

### Transit Gateway

It provides transitive peering between thousands of VPCs, on-premises data centers, site-to-site VPNs, and Direct Connect connections in a hub-and-spoke (star) configuration.

To control traffic flow, you can create **route tables** for the Transit Gateway. These tables define which VPCs can communicate with each other, providing network security and granular control over routing.

The Transit Gateway supports:
*   Direct Connect Gateway.
*   VPN connections.
*   IP multicast

Increasing Bandwidth with ECMP:- Another key use case for Transit Gateway is increasing the bandwidth of site-to-site VPN connections using **ECMP** (Equal-Cost Multi-Path routing)

**VPN to VGW vs. VPN to Transit Gateway:**

*   VPN to Virtual Private Gateway (VGW): One connection to one VPC, limited to 1.25 Gbps (one tunnel at a time). This connection consists of two tunnels.
*   VPN to Transit Gateway: One site-to-site VPN to many VPCs (due to transitive connectivity).  One site-to-site VPN connection provides 2.5 Gbps thanks to ECMP, utilizing both tunnels (1.25 Gbps each). Since 2025, Large Bandwidth Tunnels (Transit Gateway or Cloud WAN only) give up to 5 Gbps per tunnel.

You can add more site-to-site VPN connections (e.g., two or three) to the Transit Gateway to double or triple your throughput via ECMP. 📝 **Note:** This is a common exam topic.

Keep in mind that using Transit Gateway incurs costs for each GB of data processed. This added cost should be considered when optimizing for performance.

### VPC Traffic Mirroring 🛡️

VPC Traffic Mirroring is a security feature that allows you to capture and inspect network traffic within your VPC in a non-intrusive way. The goal is to route traffic to security appliances that you manage for analysis.

Here's how it works:

- Capture Traffic: Define the source Elastic Network Interfaces (ENIs) from which you want to capture traffic.
- Define Targets: Specify where you want to send the captured traffic. This could be your own ENIs or a Network Load Balancer (NLB).

Use Cases:

*   Content inspection
*   Threat monitoring
*   Troubleshooting network issues

### IPV6

⚠️ Even with many available IPv6 addresses, you can still run out of IPv4 addresses in your subnet. Each EC2 instance in a dual-stack subnet requires an IPv4 address (IPv6-only subnets, available since Nov 2021, do not). If you exhaust your IPv4 addresses, you'll need to assign a new CIDR block to your subnet to create more instances.

### Egress-only Internet Gateway

Egress-only internet gateways are used exclusively for IPv6 traffic. They function similarly to NAT gateways but specifically for IPv6. 🌐 

*   Internet Gateway: Allows bidirectional traffic (inbound and outbound). ↔️
*   NAT Gateway: Enables outbound IPv4 traffic from private subnets. 📤
*   Egress-Only Internet Gateway: Enables outbound IPv6 traffic from private subnets, preventing inbound connections. 📤🛡️

### Networking Costs in AWS

- To reduce costs and improve network performance, prioritize using private IPs for communication between instances within the same region.
- Keep as much internet traffic as possible within AWS to minimize costs.
- Using a VPC Endpoint can be significantly cheaper than using a NAT Gateway for accessing S3.

---

## 26. AWS Security & Encryption

### SSM Parameter Store

The SSM Parameter Store provides secure storage for your configuration data and secrets. You can optionally encrypt these configurations using the KMS service, effectively turning them into secrets.

Here's a breakdown of its key features: Security, Notifications, Serverless, Scalable and Durable, Easy to Use, Version Tracking, and Cloudformation Integration.

**Parameter Tiers**
Systems Manager offers two parameter tiers: Standard (free) and Advanced (paid).

### AWS Secrets Manager

AWS Secrets Manager is a service designed for storing and managing secrets. It offers several advantages over SSM Parameter Store, primarily the ability to enforce and automate secret rotation.

📌 Example: Whenever you see questions about secrets or integration with RDS or Aurora in the exam, consider Secrets Manager as a potential solution.

#### Multi-Region Secrets

Secrets Manager supports replicating secrets across multiple AWS regions.

*   **Replication:** Secrets are replicated from a primary region to secondary regions.
*   **Synchronization:** The Secrets Manager service keeps the replica secrets synchronized with the primary secret.

### AWS Certificate Manager (ACM)

AWS Certificate Manager (ACM) 🔐 simplifies the process of provisioning, managing, and deploying TLS certificates on AWS. TLS certificates (sometimes referred to as SSL certificates) are essential for providing in-flight encryption for websites, ensuring secure communication via HTTPS.

### AWS WAF (Web Application Firewall)

AWS WAF is a Web Application Firewall used to protect your web applications from common web exploits at Layer 7 (HTTP). In comparison, Layer 4 is for TCP or UDP protocols.

WAF can be deployed on:

*   Application Load Balancer (ALB)
*   API Gateway
*   CloudFront
*   AppSync GraphQL API
*   Cognito user pools

⚠️ **Warning:** The exam will try to trick you into deploying WAF on an NLB. This is not possible! WAF doesn't support Network Load Balancers (NLB) because NLBs operate on Layer 4, and WAF is for Layer 7.

You can set rules to filter based on:

*   IP addresses: You can define IP sets, with each set holding up to 10,000 IP addresses. Use multiple rules for more IPs.
*   HTTP headers, HTTP body, URI strings protects from common attacks - like SQL injection and cross-site scripting ( XSS ).
*   Size constraints: To limit request sizes (e.g., up to 2MB).
*   Geo match: To allow or block specific countries.
*   Rate-based rules: To count requests per IP for DDoS protection. 📌 **Example:** Block an IP that sends more than 100 requests in 5 minutes. (The rule counts over a 1, 2, 5 or 10 minute window; the minimum limit is 10. You cannot set a per-second limit.)

Web ACLs are regional, except for CloudFront, where they are defined globally.

📝 **Note:** A "rule group" is a reusable set of rules that can be added to multiple Web ACLs for organization.

### AWS Shield: Protecting Against DDoS Attacks

It is a free service automatically enabled for all AWS customers. It provides baseline protection against common network and transport layer attacks, including:
*   SYN floods
*   UDP floods
*   Reflection attacks
*   Other Layer 3 and Layer 4 attacks

#### AWS Shield Advanced: Enhanced DDoS Mitigation

For more sophisticated DDoS protection, AWS offers Shield Advanced. This is an **optional, paid service** that provides enhanced mitigation capabilities.

*   **Cost:** Approximately \$3,000 per month per organization.
*   **Protected Resources:** Protects against more complex DDoS attacks targeting:
    *   Amazon EC2
    *   Elastic Load Balancing (ELB)
    *   Amazon CloudFront
    *   AWS Global Accelerator
    *   Route 53

### AWS Firewall Manager

AWS Firewall Manager is a service designed to manage firewall rules across all accounts within an AWS Organization. It allows you to centrally manage security policies, ensuring consistent security across your entire AWS environment.

*   The core idea is to manage rules across many accounts simultaneously.
*   You define a **security policy**, which is a common set of security rules.

## 28. DR & Migration

### DR

A disaster is any event that negatively impacts a company's business continuity or finances. Disaster recovery involves preparing for and recovering from such events.

There are four main disaster recovery strategies, each with different RTOs and costs:

1.  Backup and Restore: Backing up data and restoring it when needed.
2.  Pilot Light: A small version of the app is always running in the cloud. Used for critical core (pilot light).
3.  Warm Standby: Running a scaled-down version (minimum size) of the full system in the cloud. Upon Disaster Recovery, scale the application using Auto Scaling to production load.
4.  Hot Site/Multi-Site: Running two full production environments, one on-premise and one in the cloud.

### Database Migration Service (DMS)

DMS is a quick and secure database service that allows you to migrate your database from on-premise to AWS.

*   Resilient and self-healing. 💪
*   Source database remains available during migration. ⏳
*   Supports 🔄: 
    *   Homogeneous (e.g., Oracle to Oracle, Postgres to Postgres).
    *   Heterogeneous migrations (e.g., Microsoft SQL Server to Aurora).
*   Supports continuous data replication using CDC (Change Data Capture). ⏱️

To use DMS, you need to create an EC2 instance. This EC2 instance performs the replication tasks. The DMS software on the EC2 instance pulls data from the source database and puts it into the target database continuously.

⚠️ Important: DMS mainly handles **data migration**, not **schema migration**. For schema changes (tables, indexes, constraints, stored procs, etc.), you use the **AWS Schema Conversion Tool (SCT)**.

### RDS to Aurora Migrations

Here are the options for migrating to Aurora MySQL:

#### RDS MySQL to Aurora MySQL

1.  **Database Snapshot** 📸 Take a database snapshot from your existing RDS MySQL database. Restore this snapshot as a new Aurora MySQL database. ⚠️ **Warning:** This option may involve some downtime, as you might need to stop operations on the original MySQL database before migrating.

2.  **Aurora Read Replica** 🔄 Create an Amazon Aurora Read Replica on top of your RDS MySQL database. Wait for the replica lag to reach zero, indicating that the Aurora Replica has fully synchronized with the MySQL database. Promote the Aurora Read Replica into its own database cluster. 📝 **Note:** This process might take longer than using a database snapshot and could incur network costs due to replication.

#### External MySQL to Aurora MySQL

1.  **Percona XtraBackup** 📦
    *   If your MySQL database is external to RDS, you can use the Percona XtraBackup utility to create a backup.
    *   Upload the backup file to Amazon S3.
    *   Import the backup file directly into a new Aurora MySQL DB cluster using the Aurora import functionality.
    *   📝 **Note:** This method only supports backups created with the Percona XtraBackup utility.

2.  **MySQL Dump Utility** ⚙️
    *   Create an Aurora MySQL DB cluster.
    *   Use the `mysqldump` utility against your MySQL database.
    *   Pipe the output directly into your existing Amazon Aurora database.
    *   ⚠️ **Warning:** This method can be time-consuming and doesn't leverage Amazon S3.
    *   📌 **Example:**
        ```bash
        mysqldump -u [user] -p[password] [database] | mysql -u [user] -p[password] -h [aurora_endpoint] [database]
        ```
#### Both Databases Running

**Amazon DMS (Database Migration Service)** 🚚
    *   If both databases are up and running, use Amazon DMS to perform continuous replication between the two databases.

### On Premise Strategy with AWS Services

AWS offers several services to help with on-premise migration:

*   **VM Import/Export**: This feature allows you to migrate existing VMs and applications into EC2.
*   **AWS Application Discovery Service**: This service helps you gather information about your on-premise servers to plan a migration.
*   **AWS Migration Hub**: You can track your migration progress using the AWS Migration Hub. ⚠️ Closed to new customers since 7 Nov 2025; AWS Transform replaces it.
*   **AWS Database Migration Service (DMS)**: DMS allows you to replicate databases between on-premise and AWS, AWS to AWS, or AWS to on-premise.
*   **AWS Server Migration Service (SMS)**: ⚠️ SMS was discontinued on 31 Mar 2022 and replaced by **AWS Application Migration Service (MGN)**, which does the same job. SMS was used for incremental replication of on-premise live servers to AWS. It replicates volumes directly into AWS. This is suited for ongoing replication (e.g., incremental replication).

### AWS Backup

AWS Backup is a fully managed service that allows you to centrally manage and automate backups across your AWS services. 🚀 The goal is to provide a central view of your backup strategy without the need for custom scripts or manual processes.

#### Vault Lock

Another crucial feature is **Vault Lock**, which enforces a **WORM** (Write Once Read Many) policy. 🔒

*   Backups stored in a Backup Vault with Vault Lock cannot be deleted. 🚫
*   Provides an additional layer of defense against accidental or malicious deletion or modification of retention periods.🛡️
*   Even the root user cannot delete backups when Vault Lock is enabled. 👑

⚠️ **Warning:** Vault Lock provides strong guarantees on the safety of your backups.

## 30. Other Services

- **CloudFormation**: CloudFormation is a crucial technology in AWS because it provides a declarative way of defining your AWS infrastructure for almost all resources. CloudFormation is the foundation of infrastructure as code on AWS. It is an alternative of Terraform.
- **Amazon SES: Simple Email Service**: It is a fully managed service that enables you to send emails securely, globally, and at scale. 📧 Your application interacts with Amazon SES through: The SES API, an SMTP server. Amazon SES then handles sending bulk emails to your users. Common use cases for Amazon SES: Transactional emails 🧾, Marketing emails 📣, Bulk email communications ✉️
- **Amazon Pinpoint**: Amazon Pinpoint is a scalable inbound and outbound marketing communication service. ⚠️ End of support 30 Oct 2026 (closed to new customers 20 May 2025); SMS/voice/push now in AWS End User Messaging, email in SES.
  - It allows you to send messages via multiple channels:
    *   📧 Email
    *   💬 SMS
    *   📱 Push Notifications
    *   🗣️ Voice
    *   ✉️ In-App Messaging
  - One of the primary use cases is sending SMS messages. Customers receive SMS messages sent through Amazon Pinpoint. Use cases for Pinpoint include:
    *   Running marketing campaigns by sending bulk marketing emails.
    *   Sending transactional SMS messages.
- **SSM Session Manager**: SSM Session Manager allows you to start a secure shell on your EC2 instances and on-premises servers without needing SSH access, bastion hosts, or SSH keys. This enhances security by eliminating the need to open port 22 on your EC2 instances.
- **Systems Manager Services**:
  - **Run Command**: The Run Command is used to execute a document (script or single command) on multiple instances using resource groups.
  - **Patch Manager**: Patch Manager automates the process of patching managed instances.
  - **Maintenance Windows**: Maintenance Windows define a schedule for performing actions on your instances.
  - **Automation**: Automation simplifies common maintenance and deployment tasks on EC2 instances or other AWS resources.
- **Cost Explorer**: Cost Explorer is a billing service that helps you visualize, understand, and manage your AWS costs and usage over time. It's a valuable tool for cost optimization and planning.
- **AWS Cost Anomaly Detection**: AWS Cost Anomaly Detection is a service that continuously monitors your cost and usage data. It leverages machine learning to detect unusual spending patterns. 🚀 
  - Here's a breakdown:
    - It learns from your unique historical patterns. 🧠
    - It detects one-time cost spikes and continuous cost increases.
  - It monitors:
    *   AWS services.
    *   Member accounts.
    *   Cost allocation tags.
    *   Cost categories.
- **AWS Batch**: AWS Batch is a fully managed batch processing service that allows you to perform batch processing at any scale. With Batch, you can efficiently run hundreds of thousands of computing batch jobs on AWS very easily.
- **Amazon AppFlow**: Amazon AppFlow is a fully managed integration service that simplifies data transfer between Software-as-a-Service (SaaS) applications and AWS services. 📌 **Example:** Salesforce is a common source and frequently appears in exam scenarios. AppFlow supports various data sources, including:
  *   Salesforce ☁️
  *   SAP 🏢
  *   Zendesk 💬
  *   Slack 🗣️
  *   ServiceNow 🛠️
- **AWS Amplify**: AWS Amplify is a powerful web and mobile application development tool. 🚀 Think of it as a central hub for integrating various AWS services to streamline your development process. It allows developers to build and deploy applications more efficiently. Amplify can be thought of as the Elastic Beanstalk for web and mobile applications. 
- **Instance Scheduler on AWS**: Instance Scheduler is an AWS solution, not a service, that you deploy using CloudFormation. It provides the capability to automatically start and stop your AWS services to reduce costs, potentially by up to 70%.




---
