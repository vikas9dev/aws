# RDS

## **Question 1**

A financial application consists of an Auto Scaling group of Amazon EC2 instances, an Application Load Balancer, and a MySQL RDS instance set up in a Multi-AZ deployment.

To protect customers’ confidential data, it must be ensured that the **Amazon RDS database is only accessible using an authentication token specific to the profile credentials of EC2 instances**.

**Which of the following actions should be taken to meet this requirement?**

**Options:**

1. Enable the IAM DB Authentication
2. Configure SSL in your application to encrypt the database connection to RDS
3. Use a combination of IAM and STS to enforce restricted access to your RDS instance using a temporary authentication token
4. Create an IAM Role and assign it to your EC2 instances which will grant exclusive access to your RDS instance

<details>

<summary><strong>Answer & Explanation</strong> 📝</summary>

**IAM Database Authentication** allows you to use **temporary IAM tokens** to connect to Amazon RDS instead of using long-term passwords. This ensures secure access tied to the **IAM role of EC2 instances**, meeting the requirement of authentication based on EC2 instance profile credentials.

#### **Benefits of IAM DB Authentication:**

1. ✅ **Passwordless Authentication** – Uses short-lived IAM authentication tokens instead of DB passwords.
2. 🔐 **Secure Access Control** – Access is granted based on IAM and the EC2 instance's role.
3. 🔄 **Token-Based Authentication** – Tokens expire after 15 minutes, reducing the risk of credential misuse.

✅ **Correct Answer:** **Enable the IAM DB Authentication**

<img src="https://media.tutorialsdojo.com/2019-01-13_07-04-06-a2157247b0fa129795001208504fcb51.png"
     alt="Learn more about IAM DB Authentication"
     width="600" />

### ❌ **Incorrect Options**

| Option                                                         | Why It's Incorrect                                                                                           |
| -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| <span style="color:red"><strong>Configure SSL in your application to encrypt DB connection</strong></span> | SSL only encrypts data _in transit_; it does not provide IAM-based token authentication.                     |
| <span style="color:red"><strong>Use IAM + STS temporary tokens</strong></span>                             | STS provides temp credentials for AWS API calls, but RDS requires **IAM DB Authentication**, not STS tokens. |
| <span style="color:red"><strong>Create an IAM Role for EC2 to access RDS</strong></span>                   | IAM roles alone **cannot authenticate** directly to RDS. You still must enable **IAM DB Authentication**.    |

### 📝 **Summary**

To ensure RDS access is controlled via **EC2 instance IAM role + short-lived authentication tokens**, you must **enable IAM DB Authentication** on the RDS instance.

</details>

---

## **Question 2**

An application is hosted in an Auto Scaling group of EC2 instances and uses a **Microsoft SQL Server on Amazon RDS**. There is a requirement that **all in-flight data between your web servers and RDS must be secured**.

Which of the following options is the **MOST suitable solution** that you should implement?
**(Select TWO)**

### **Options:**

1. Configure the security groups of your EC2 instances and RDS to only allow traffic to and from port 443.
2. Enable the IAM DB authentication in RDS using the AWS Management Console.
3. Force all connections to your DB instance to use SSL by setting the `rds.force_ssl` parameter to true. Once done, reboot your DB instance.
4. Specify the TDE option in an RDS option group that is associated with that DB instance to enable Transparent Data Encryption (TDE).
5. Download the Amazon RDS Root CA certificate. Import the certificate to your servers and configure your application to use SSL to encrypt the connection to RDS.

<details>

<summary><strong>Answer & Explanation</strong> 📝</summary>

The goal is to **secure data in transit** between EC2 and RDS SQL Server.
Encryption in transit for RDS SQL Server is achieved using **SSL/TLS**.

To ensure this:

| Step                       | Description                                                                                 |
| -------------------------- | ------------------------------------------------------------------------------------------- |
| **1. Force SSL**           | Set the RDS parameter `rds.force_ssl = true` to ensure _all_ DB connections are encrypted.  |
| **2. Install RDS Root CA** | Your application needs the RDS SSL certificate to establish a trusted encrypted connection. |

These two steps together ensure that **all DB traffic is encrypted and trusted end-to-end**.

### ✅ **Correct Answers**

- ✔ **Option 3:** Force SSL for all DB connections using `rds.force_ssl = true` and reboot the DB instance
- ✔ **Option 5:** Download the RDS Root CA certificate and configure the application to use SSL

<img src="https://media.tutorialsdojo.com/public/rds_sql_ssl_cert.png"
     alt="RDS SQL Server SSL certificate usage"
     width="600" />

### ❌ **Incorrect Options**

| Option                                       | Why It's Wrong                                                                                    |
| -------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| <span style="color:red"><strong>1. Configure SG to allow only port 443</strong></span>   | SG rules do not enforce SSL between EC2 and RDS. Port 443 is not used for RDS SQL Server traffic. |
| <span style="color:red"><strong>2. Enable IAM DB Authentication</strong></span>          | IAM DB Auth is only supported for **MariaDB, MySQL and PostgreSQL**, not SQL Server.                       |
| <span style="color:red"><strong>4. Use TDE (Transparent Data Encryption)</strong></span> | TDE encrypts data **at rest**, not in transit, so it doesn't meet the requirement.                |

### 📝 **Summary**

To secure **in-flight data for SQL Server on RDS**, you must:

- **Force SSL** for all DB connections
- **Use the RDS Root CA certificate** to enforce an encrypted connection from the client/application

</details>

---

## **Question 3**

A Forex trading platform that processes and stores global financial data every minute is hosted in an on-premises data center using an **Oracle database**. Due to a cooling problem, the company urgently needs to migrate its infrastructure to AWS.

As the Solutions Architect, your responsibility is to ensure that the database is properly migrated and remains **highly available** in case of failure.

**Which combination of actions would meet the requirement? (Select TWO.)**

### **Options:**

1. Create an Oracle database in Amazon RDS with Multi-AZ deployments.
2. Convert the database schema using the AWS Schema Conversion Tool.
3. Migrate the Oracle database to a non-cluster Amazon Aurora with a single instance.
4. Migrate the Oracle database to AWS using the AWS Database Migration Service (DMS).
5. Launch an Oracle database instance in Amazon RDS with Recovery Manager (RMAN) enabled.

<details>

<summary><strong>Answer & Explanation</strong> 📝</summary>

The requirements are:

- **Migrate Oracle to AWS quickly**
- **Ensure high availability** (in case the DB fails)

To meet both:

| Step                    | Why?                                                                                                               |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------ |
| **RDS Oracle Multi-AZ** | Provides automatic failover to a standby DB in another AZ → ensures high availability and zero data loss.          |
| **AWS DMS**             | Allows minimal-downtime migration of Oracle from on-prem to AWS. Supports Oracle-to-Oracle homogeneous migrations. |

### ✅ **Correct Answers**

- ✔ **Create an Oracle database in Amazon RDS with Multi-AZ deployments.**
- ✔ **Migrate the Oracle database to AWS using the AWS Database Migration Service (DMS).**

<img src="https://media.tutorialsdojo.com/con-multi-AZ.png"
     alt="RDS Multi-AZ deployment diagram"
     width="600" />

### ❌ **Incorrect Options**

| Option                                        | Reason                                                                                                                                                 |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <span style="color:red"><strong>Convert the schema using AWS SCT</strong></span>              | SCT is needed only for **heterogeneous** migrations (e.g., Oracle → PostgreSQL). Here, it's **Oracle-to-Oracle**, so no schema conversion is required. |
| <span style="color:red"><strong>Migrate to non-cluster Aurora single instance</strong></span> | Not suitable for critical workloads that require **high availability**; a single instance has a single point of failure.                               |
| <span style="color:red"><strong>Launch RDS Oracle with RMAN enabled</strong></span>           | RMAN does not give high availability. On RDS you get only limited RMAN tasks (backups to disk through `rdsadmin.rdsadmin_rman_util`), and you can't use RMAN to restore an RDS instance. RDS uses automated backups, snapshots, and Multi-AZ instead.                                                         |

### 📝 **Summary**

For migrating Oracle to AWS while ensuring **high availability + minimal downtime**:

- Use **AWS DMS** for data migration
- Use **RDS Oracle Multi-AZ** for HA in production

</details>

---

## **Question 4**

Due to a large volume of query requests, the database performance of an online reporting application has significantly slowed down.

The Solutions Architect wants to convince the client to use **Amazon RDS Read Replicas** instead of **Multi-AZ deployments**.

**What are two benefits of using Read Replicas over Multi-AZ? (Select TWO)**

### **Options:**

1. It enhances the read performance of your primary database by increasing its IOPS and accelerates its query processing via AWS Global Accelerator.
2. It elastically scales out beyond the capacity constraints of a single DB instance for read-heavy database workloads.
3. Provides synchronous replication and automatic failover in the case of Availability Zone service failures.
4. Provides asynchronous replication and improves the performance of the primary database by taking read-heavy database workloads from it.
5. Allows both read and write operations on the read replica to complement the primary database.

<details>

<summary><strong>Answer & Explanation</strong> 📝</summary>

**Amazon RDS Read Replicas** are designed for **scaling read performance**. They allow you to create one or more read-only copies of your database so you can serve high-volume read traffic.

#### Key Benefits

| Read Replicas Provide        | Why It Matters                                                        |
| ---------------------------- | --------------------------------------------------------------------- |
| **Horizontal read scaling**  | You can add multiple replicas to handle increased read traffic.       |
| **Asynchronous replication** | Reduces load on the primary DB by moving read operations to replicas. |

This improves the performance of the **primary database** since it no longer handles heavy read queries.

### ✅ **Correct Answers**

- ✔ **Option 2:** It elastically scales out beyond the capacity constraints of a single DB instance for read-heavy workloads.
- ✔ **Option 4:** Provides asynchronous replication and offloads read-heavy workloads from the primary DB.

<img src="https://media.tutorialsdojo.com/2020-02-28_01-52-40-4fa2635076a98c44c28464d31d793a21.png"
     alt="RDS read replica scaling diagram"
     width="600" />

### ❌ **Incorrect Options**

| Option                                              | Reason                                                                                                                            |
| --------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| <span style="color:red"><strong>1. Increase IOPS via AWS Global Accelerator</strong></span>     | Global Accelerator is unrelated to RDS performance. It routes user traffic to nearest endpoints, not DB IOPS.                     |
| <span style="color:red"><strong>3. Synchronous replication & automatic failover</strong></span> | That is a **Multi-AZ** feature, not Read Replica. Read Replicas use **asynchronous** replication and are for performance, not HA. |
| <span style="color:red"><strong>5. Allows both read & write operations</strong></span>          | Read Replicas are **read-only** and cannot process writes unless promoted to standalone DB.                                       |

### 📝 **Summary**

| Feature      | Multi-AZ                     | Read Replica               |
| ------------ | ---------------------------- | -------------------------- |
| Purpose      | High availability / failover | Performance & read scaling |
| Replication  | Synchronous                  | Asynchronous               |
| Read Scaling | ❌ No                        | ✅ Yes                     |

</details>

---

## **Question 5**

An online events registration system is hosted in AWS and uses ECS to host its front-end tier and an RDS configured with Multi-AZ for its database tier. What are the events that will make Amazon RDS automatically perform a failover to the standby replica? (Select TWO.)

**Options:**

* Storage failure on primary
* In the event of Read Replica failure
* Loss of availability in primary Availability Zone
* Storage failure on secondary DB instance
* Compute unit failure on secondary DB instance

<details>
  <summary><strong>Answer & Explanation</strong> 📝</summary>

Amazon RDS provides high availability and automatic failover support via **Multi-AZ deployments**. In this setup, RDS provisions a **synchronous standby replica** in a **different Availability Zone**, ensuring data redundancy and minimizing downtime.

Amazon RDS provides high availability and failover support for DB instances using Multi-AZ deployments. Amazon RDS uses several different technologies to provide failover support. Multi-AZ deployments for Oracle, PostgreSQL, MySQL, and MariaDB DB instances use Amazon’s failover technology. SQL Server DB instances use SQL Server Database Mirroring (DBM).

In a Multi-AZ deployment, Amazon RDS automatically provisions and maintains a synchronous standby replica in a different Availability Zone. The primary DB instance is synchronously replicated across Availability Zones to a standby replica to provide data redundancy, eliminate I/O freezes, and minimize latency spikes during system backups. Running a DB instance with high availability can enhance availability during planned system maintenance and help protect your databases against DB instance failure and Availability Zone disruption.

Amazon RDS detects and automatically recovers from the most common failure scenarios for Multi-AZ deployments so that you can resume database operations as quickly as possible without administrative intervention.

### 🟢 **How Multi-AZ Works**

* The primary DB instance replicates synchronously to a **standby replica**.
* This protects against:

  * I/O freezes
  * Latency spikes
  * AZ disruptions
  * Primary instance failures

### 🟢 **Automatic Failover Events**

RDS will **automatically perform a failover** if the **primary DB instance** is impacted by:

1. **Loss of availability in the primary Availability Zone**
2. **Storage failure on the primary DB instance**
3. **Loss of network connectivity to the primary**
4. **Compute unit failure on the primary**

### 🛑 **Standby Replica Is NOT Used for Reads**

The standby replica is strictly for **failover only**, not for read scaling.
For read traffic, you must use **Read Replicas**.

### ✅ **Correct Answers**

* **Loss of availability in primary Availability Zone**
* **Storage failure on primary**

### ❌ **Incorrect Options**

These do **not** trigger failover because they do **not affect the primary instance**:

* <span style="color:red"><strong>Storage failure on secondary DB instance</strong></span>
* <span style="color:red"><strong>Failure of a Read Replica</strong></span>
* <span style="color:red"><strong>Compute unit failure on secondary DB instance</strong></span>

</details>

---