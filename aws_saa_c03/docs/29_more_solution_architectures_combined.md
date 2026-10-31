# 29 More Solution Architectures


Sections-
1. [Event Processing in AWS](#1-event-processing-in-aws)
2. [Caching Strategies on AWS](#2-caching-strategies-on-aws)
3. [Blocking IP Addresses in AWS](#3-blocking-ip-addresses-in-aws)
4. [High Performance Computing (HPC) on AWS](#4-high-performance-computing-hpc-on-aws)
5. [Solution Architecture: Making EC2 Instances Highly Available 🚀](#5-solution-architecture-making-ec2-instances-highly-available-)

---

## 1. Event Processing in AWS

Let's explore event processing in AWS, covering different possibilities and associated constraints.

### SQS and Lambda

*   Using SQS and Lambda involves an SQS queue and a Lambda function.
*   Events are inserted into the SQS queue.
*   The Lambda service pulls messages from the SQS queue.
*   If issues arise, messages are returned to the SQS queue for retries.
*   ⚠️ **Warning:** This can lead to an infinite loop if a message consistently fails.
*   💡 **Tip:** To prevent infinite loops, configure SQS to send problematic messages to a dead-letter queue (DLQ) after a certain number of retries (e.g., five).

![AWS SQS, SQS FIFO and Lambda](./img/AWS_SQS_SQS_FIFO_and_Lambda.png)

### SQS FIFO and Lambda

*   FIFO stands for "First In, First Out," ensuring messages are processed in order.
*   The Lambda function retries to process messages from the queue.
*   ⚠️ **Warning:** If one message fails, it blocks the entire queue processing.
*   💡 **Tip:** Again, use a DLQ to handle failed messages and allow the queue to continue processing.

### SNS and Lambda

![AWS SNS and Lambda](./img/AWS_SNS_and_Lambda.png)

*   SNS (Simple Notification Service) sends messages asynchronously to Lambda.
*   The Lambda function has its own internal retry behavior.
*   The Lambda service retries **two** more times (3 attempts in total).
*   If processing still fails after these retries, the message is discarded by default.
*   💡 **Tip:** Configure a DLQ at the Lambda service level to send failed messages to, for example, an SQS queue for further processing.
*   📝 **Note:** With SQS, the DLQ is configured on the SQS side, while with SNS, it's configured on the Lambda side.

### Fan-Out Pattern

![AWS Fan-Out Pattern](./img/AWS_Fan-Out_Pattern.png)

**How to deliver data to multiple SQS queues?**

*   The fan-out pattern addresses how to deliver data to multiple SQS queues.
*   **Option 1: Application with AWS SDK**
    *   The application sends the same message to each SQS queue individually.
    *   ⚠️ **Warning:** This approach is unreliable. If the application crashes midway, some queues might not receive the message.
*   **Option 2: Fan-Out Pattern** 🚀
    *   Combine SQS queues with an SNS topic.
    *   SQS queues subscribe to the SNS topic.
    *   When a message is sent to the SNS topic, SNS fans it out to all subscribed SQS queues.
    *   💡 **Tip:** This provides a higher guarantee of message delivery.
    *   From the application's perspective, you only need to send the message to the SNS topic.

### S3 Event Notifications

![AWS S3 Event Notifications](./img/AWS_S3_Event_Notifications.png)

*   React to specific events in Amazon S3 buckets:
    *   Object creation
    *   Object removal
    *   Object restoration
    *   Replication events
*   Filter events by name (e.g., *.jpg).
*   📌 **Use-case:** Generate thumbnails of images uploaded to Amazon S3.
*   S3 events can trigger:
    *   SNS notifications
    *   SQS messages
    *   Lambda function invocations
*   You can create multiple S3 event notifications.
*   Delivery typically occurs within seconds, but can sometimes take a minute or longer.

### Amazon EventBridge

![AWS EventBridge](./img/AWS_EventBridge.png)

*   Events in Amazon S3 buckets are sent to Amazon EventBridge after you turn on EventBridge notifications for the bucket (it is off by default).
*   Use rules to route events to over 18 AWS services as destinations.
*   Benefits of using EventBridge:
    *   Filtering options with JSON rules (filter on metadata, object size, name, etc.).
    *   Send events to multiple destinations simultaneously (Step Functions, Kinesis Streams, Data Firehose).
    *   EventBridge capabilities: archiving, replaying, and reliable delivery.

### EventBridge and CloudTrail Integration (Intercept API Calls)

![AWS EventBridge and CloudTrail Integration](./img/AWS_EventBridge_and_CloudTrail_Integration.png)

*   Intercept any API call with Amazon EventBridge using CloudTrail integration.
*   📌 **Example:** React to a user deleting a table from DynamoDB.
    *   The `DeleteTable` API call is logged in CloudTrail.
    *   This triggers an event in Amazon EventBridge.
    *   Create alerts to notify via Amazon SNS.

### External Events onto AWS

![AWS External Events onto AWS](./img/AWS_External_Events_onto_AWS.png)

*   Use API Gateway to ingest external events.
*   Clients send requests to API Gateway.
*   API Gateway sends messages to Kinesis Data Streams.
*   Records end up in Kinesis Data Firehose.
*   Data can then be delivered to Amazon S3.

---

## 2. Caching Strategies on AWS

Let's explore caching strategies on AWS and their implications. This discussion revolves around a typical solution architecture, which may vary based on specific needs.

The architecture includes:

*   CloudFront (for both dynamic and static content)
*   API Gateway
*   Application Logic (EC2, Lambda)
*   Database
*   Internal Cache (Redis, Memcached, DAX)
*   S3

Here's a breakdown of caching at each layer:

![AWS Caching Strategies](./img/AWS_Caching_Strategies.png)

### CloudFront 🌐

CloudFront provides caching at the edge, bringing content as close as possible to users.

*   Benefits: Fast response times for users when the cache is hit. ⚡
*   Drawbacks: Potential for outdated content if changes occur in the backend. ⏳
*   Solution: Use a TTL (Time To Live) to refresh the cache regularly. 🔄
*   Considerations: Balance caching at the edge versus caching in the application logic. 🤔

### API Gateway 🚪

API Gateway also offers caching capabilities, independent of CloudFront.

*   Scope: Regional service, meaning the cache is regional. 🌍
*   Implication: Network latency between clients and the API Gateway if the cache is hit. 📶

### Application Logic ⚙️

Application logic often utilizes an internal cache to avoid repeatedly hitting the database.

*   Common Caches: Redis, Memcached, or DAX (for DynamoDB). 💾
*   Purpose: Store frequent or complex query results for faster access. 🚀
*   Benefits: Reduces pressure on the database and augments read capacity. 💪

### Database and S3 🗄️

📝 **Note:** There is no built-in caching capability in databases or Amazon S3.

### General Caching Considerations 💭

As you move further from the edge, computation costs and latency increase.

*   No one-size-fits-all: There's no single "right" way to implement caching. 🤷‍♀️
*   Key Decisions:
    *   Where to cache content? 📍
    *   How to cache content? ⚙️
    *   How long to cache content? ⏳
    *   Acceptable latency? 🐌
    *   Which content to cache? 📦

This overview highlights the various caching options available on AWS. The optimal strategy depends on the specific scenario and desired behavior. The goal is to determine where caching will be most appropriate and efficient. 🎯

---

## 3. Blocking IP Addresses in AWS

Let's explore how to block IP addresses in AWS, focusing on different architectural scenarios and defense layers.

### EC2 Instance in a Public Subnet

![Blocking IP Addresses in AWS - EC2 Instance in a Public Subnet](./img/Blocking_IP_Addresses_in_AWS_EC2_Instance_in_a_Public_Subnet.png)

Consider an EC2 instance in a public subnet that a client wants to access. Here's how to implement security:

1.  **Network ACL (NACL):** 🛡️ The first line of defense is the NACL associated with your public subnet.
    *   You can explicitly write allow or deny rules to block or permit client traffic.
    *   NACLs are a cost-effective and straightforward way to manage network access.
2.  **Security Group:** 🔒 If the NACL is configured to allow all traffic (e.g., a default NACL), the Security Group acts as the second line of defense.
    *   Security Groups only support allow rules.
    *   If you know the specific IP addresses of your clients, you can configure the Security Group to only allow traffic from those IPs.
3.  **Firewall Software (Optional):** 🔥 If traffic makes it to the EC2 instance, you can optionally run firewall software directly on the instance.
    *   This provides granular control over incoming and outgoing traffic.
    *   ⚠️ **Warning:** Running firewall software incurs a CPU cost on the EC2 instance, potentially impacting performance.

### Application Load Balancer (ALB) and EC2 Instance

![Blocking IP Addresses in AWS - ALB and EC2 Instance](./img/Blocking_IP_Addresses_in_AWS_ALB_and_EC2_Instance.png)

Now, let's examine a scenario with an ALB and an EC2 instance.

1.  **Architecture:** 🏗️ Clients connect to an ALB in a public subnet, which then forwards traffic to an EC2 instance in a private subnet.
    *   This setup enhances security because the EC2 instance is not directly exposed to the internet.
2.  **Security Group for EC2:** 🔒 The EC2 instance's Security Group should only allow connections from the ALB.
3.  **Connection Termination:** 🔌 The ALB performs connection termination. The client connects to the ALB, and the ALB establishes a separate connection to the EC2 instance.
4.  **Security Management at ALB Level:** 🛡️ You can manage security at the ALB level using Security Groups or other ALB features.
5.  **NACL for Public Subnet:** 🌐 You can still use NACLs on the public subnet to allow or deny traffic.

The same security principles apply when using a Network Load Balancer (NLB) instead of an ALB.

### Web Application Firewall (WAF)

You can enhance security further by integrating AWS WAF with your ALB or CloudFront distribution.

1.  **ALB + WAF:** 🛡️ Pairing WAF with an ALB allows you to set up IP address filtering and other security rules at the application level.
    *   ⚠️ **Warning:** WAF incurs additional costs.
    *   WAF provides comprehensive defenses for your infrastructure.

![Blocking IP Addresses in AWS - ALB and WAF](./img/Blocking_IP_Addresses_in_AWS_ALB_and_WAF.png)

2.  **CloudFront + WAF:** ☁️ AWS WAF can also be applied to CloudFront distributions.
    *   If using an ALB in public mode with CloudFront, CloudFront sends traffic from its edge locations (using CloudFront's public IPs) to your ALB.
    *   In this case, NACLs are not effective for filtering client traffic because the client's IP is not directly reaching your infrastructure.
    *   You must configure the ALB's Security Group to allow traffic only from CloudFront's public IP ranges.
    * **Geo Restriction:** 🌍 CloudFront offers a Geo Restriction feature that allows you to block traffic from specific countries.
        *   📌 **Example:** If you detect attacks originating from a particular country, you can block traffic from that country using Geo Restriction.
    * **WAF at CloudFront Level:** 🔥 You can also implement IP address filtering and other security measures using WAF at the CloudFront level.

![Blocking IP Addresses in AWS - CloudFront and WAF](./img/Blocking_IP_Addresses_in_AWS_CloudFront_and_WAF.png)

### General 💡 **Tip** for Network Security

When troubleshooting network security issues, drawing a diagram of the network traffic flow is incredibly helpful. This visual representation clarifies where to apply specific security rules.

```
Client --> (Internet) --> CloudFront --> ALB --> EC2 Instance
```

By visualizing the network path, you can better understand where to implement security measures.

---

## 4. High Performance Computing (HPC) on AWS

The cloud is an ideal environment for **High Performance Computing (HPC)** due to its ability to rapidly provision a large number of resources. 🚀

Benefits of using the cloud for HPC:

*   Speed up time to results by adding more resources. ⏱️
*   Pay only for what you use. 💰
*   Destroy the entire infrastructure when finished and incur no further costs. 💥

This makes the cloud a cost-effective and efficient solution for computationally intensive tasks. AWS actively encourages the use of its services for HPC.

Common use cases for HPC include:

*   Genomics
*   Computational chemistry
*   Financial risk modeling
*   Weather prediction
*   Machine learning & Deep Learning
*   Autonomous driving

Let's explore the AWS services that facilitate HPC.

### Data Management and Transfer

How do we efficiently move large datasets into AWS?

*   **Direct Connect**: Transfer GB/s of data to the cloud, over a private secure network. 🔒
*   **Snowball & Snowmobile**: Move petabytes of data to the cloud via physical transport for large, one-off transfers. 🚚 ⚠️ **Note (2026):** Snowmobile was retired in 2024, and Snowball Edge is closed to new customers since 7 Nov 2025. AWS now points new customers to DataSync or AWS Data Transfer Terminal.
*   **DataSync**: Install DataSync agents to move large amounts of data between on-premises file systems (NFS, SMB) and AWS storage services (S3, EFS, FSx for Windows). ⚙️

### Compute and Networking

These are critical components for HPC.

*   **EC2 Instances**: Choose CPU-optimized or GPU-optimized instances based on your computational needs. 💻
*   **Spot Instances/Fleets**: Leverage Spot Instances or Spot Fleets for significant cost savings. 📉
*   **Auto Scaling**: Automatically scale your compute resources based on workload demands. ⬆️⬇️
*   **EC2 Placement Group (Cluster)**: For distributed computations requiring low latency, use a cluster placement group. This provides low latency (e.g., 10 Gbps) networking, with all instances on the same rack and within the same Availability Zone (AZ). 📍

![Compute and Networking - EC2 Placement Group](./img/Compute_and_Networking_EC2_Placement_Group.png)

### Enhancing EC2 Instance Performance

How can we further optimize the performance of our EC2 instances for HPC?

*   **EC2 Enhanced Networking (SRI-IOV)**: Provides higher bandwidth, higher packets per second (PPS), and lower latency. ⚡️

    * Option-1:   **Elastic Network Adapter (ENA)**: The preferred option, delivering up to 100 Gbps network speed. 🚀 📝 **Note:** This is a key concept for the exam.
    * Option-2:   **Intel 82599 VF**: A legacy option providing up to 10 Gbps. Still relevant for exam awareness. 👴

*   **Elastic Fabric Adapter (EFA)**: An improved ENA specifically designed for HPC workloads on Linux (only works on Linux). It's ideal for **inter-node communication** and **tightly coupled workloads** (e.g., distributed computation). 🔗

    *   EFA leverages the **Message Passing Interface (MPI)** standard, bypassing the underlying Linux OS for even lower latency and more reliable transport. ⚙️
    *   💡 **Tip:** If you have a Linux instance performing tightly coupled workloads, EFA can significantly improve network performance.
    *   ⚠️ **Warning:** Be prepared to differentiate between ENA, EFA, and ENI in the exam.

### Data Storage Options

Where should we store the data for our HPC workloads?

*   **Instance-Attached Storage**:
    *   **EBS**: Scales up to 256,000 IOPS with io2 Block Express. 💽
    *   **Instance Store**: Can scale to millions of IOPS, offering very low latency. However, data is lost if the instance fails. ⚠️
*   **Network Storage**:
    *   **Amazon S3**: Store large blobs of data (object storage). 📦
    *   **EFS**: In Bursting mode, throughput scales with the size of the file system. Elastic and Provisioned **throughput** modes give higher performance (EFS has no "Provisioned IOPS" mode). 📁
    *   **FSx for Lustre**: A file system specifically designed for HPC, offering millions of IOPS. It's backed by S3. 🌟 "Lustre" stands for "Linux cluster".

### Automation and Orchestration

How do we automate and orchestrate HPC workloads?

*   **AWS Batch**: A fully managed service for running multi-node parallel jobs, which enables you to run single jobs that span across multiple EC2 instances. It easily simplifies schedule jobs and launch EC2 instance accordingly. ⚙️
*   **AWS ParallelCluster**: An open-source cluster management tool for deploying HPC clusters on AWS. 🛠️

    *   Configure with text files.
    *   It automates the creation of VPCs, subnets, cluster types, and instance types.
    *   Ability to enable EFA on the cluster (improves network performance).
    *   💡 **Tip:** You can use ParallelCluster with EFA to improve network performance and create higher-performance HPC clusters. There's a parameter in the configuration file to enable Elastic Fabric Adapters.

In summary, HPC on AWS is not a single service but a combination of services and configurations. Understanding these components is crucial for maximizing computational potential within AWS. 🚀

---

## 5. Solution Architecture: Making EC2 Instances Highly Available 🚀

EC2 instances, by default, are launched in a single Availability Zone (AZ) and are not inherently highly available. However, we can engineer solutions to achieve high availability. The approach depends on your specific requirements and the effort you're willing to invest.

### 1. Using a Standby EC2 Instance with Elastic IP 🌐

This approach involves having a primary EC2 instance and a standby instance ready to take over in case of failure.

![Making EC2 Instances Highly Available - Using a Standby EC2 Instance with Elastic IP](./img/Making_EC2_Instances_Highly_Available_Using_a_Standby_EC2_Instance_with_Elastic_IP.png)

1.  **Initial Setup:**
    *   A Public EC2 instance runs a web server.
    *   An Elastic IP is attached to the EC2 instance, allowing users to access the website directly.

2.  **Standby Instance:**
    *   A Standby EC2 instance is created as a backup.

3.  **Failure Detection:**
    *   Monitoring is crucial to detect issues with the primary instance.
    *   Create a CloudWatch Event or Alarm based on relevant events.
        *   📌 **Example:** Monitor CPU usage. If it reaches 100%, trigger an alarm.
        *   📌 **Example:** Monitor for instance termination events.

4.  **Failover Process:**
    *   The CloudWatch Alarm or Event triggers a Lambda function.
    *   The Lambda function performs the following actions:
        *   Starts the Standby EC2 instance (if it's not already running).
        *   Attaches the Elastic IP to the Standby instance.
        *   Detaches the Elastic IP from the failed primary instance.

5.  **Result:**
    *   Users continue to access the website via the Elastic IP, unaware of the failover.
    *   The failed EC2 instance can be terminated.

### 2. Using Auto Scaling Group (ASG) with Elastic IP ⚙️

![Making EC2 Instances Highly Available - Using Auto Scaling Group with Elastic IP](./img/Making_EC2_Instances_Highly_Available_Using_Auto_Scaling_Group_with_Elastic_IP.png)

This method leverages an Auto Scaling Group across multiple Availability Zones to ensure automatic recovery.

1.  **ASG Configuration:**
    *   Configure an ASG spanning two Availability Zones.
    *   Set the minimum, maximum, and desired capacity to 1.
        *   This ensures only one instance runs at any given time.

    ```text
    Minimum instances: 1
    Maximum instances: 1
    Desired instances: 1
    ```

2.  **Elastic IP Attachment:**
    *   Use EC2 User Data to automatically attach the Elastic IP to the launched instance.
    *   The User Data script issues API calls to attach the Elastic IP based on Tags.

3.  **Failure and Recovery:**
    *   If the EC2 instance is terminated, the ASG automatically launches a replacement instance in another AZ.
    *   The new instance executes the EC2 User Data script, attaching the Elastic IP.

4.  **Benefits:**
    *   No need for CloudWatch Alarms or Events. The ASG handles instance replacement automatically.
    *   Guaranteed single instance operation due to the min/max/desired capacity settings.

5.  **IAM Role:**
    *   ⚠️ **Warning:** The EC2 instance requires an IAM role with permissions to call the AWS APIs necessary to attach the Elastic IP.

### 3. Handling Stateful EC2 Instances with EBS Volumes and ASG 💾

![Handling Stateful EC2 Instances with EBS Volumes and ASG](./img/Handling_Stateful_EC2_Instances_with_EBS_Volumes_and_ASG.png)

This approach extends the ASG method to handle stateful EC2 instances with attached EBS volumes. This is useful for databases or applications requiring persistent storage.

1.  **Setup:**
    *   An ASG spans two Availability Zones.
    *   A Public EC2 instance is launched within the ASG.
    *   An Elastic IP is attached to the EC2 instance.
    *   An EBS Volume is attached to the EC2 instance, containing the application's data.

2.  **Lifecycle Hooks:**
    *   Utilize ASG lifecycle hooks to manage the EBS volume during instance termination and launch.

3.  **Termination Lifecycle Hook:**
    *   When the EC2 instance is terminated, a termination lifecycle hook is triggered.
    *   This hook executes a script to create an EBS Snapshot of the attached EBS Volume.
    *   The snapshot is tagged appropriately.

4.  **Launch Lifecycle Hook:**
    *   When the ASG launches a replacement EC2 instance, a launch lifecycle hook is triggered.
    *   This hook executes a script to:
        *   Create a new EBS Volume from the EBS Snapshot in the correct Availability Zone.
        *   Attach the new EBS Volume to the Replacement EC2 instance.
        *   Attach the Elastic IP address to the Replacement EC2 instance using EC2 User Data.

5.  **IAM Role:**
    *   ⚠️ **Warning:** The EC2 instance requires an IAM role with permissions to:
        *   Create EBS Snapshots.
        *   Create EBS Volumes from Snapshots.
        *   Attach EBS Volumes.
        *   Attach Elastic IPs.

6.  **Benefits:**
    *   Ensures data persistence by creating snapshots of the EBS volume before termination.
    *   Restores the data to the new instance by creating a volume from the snapshot.
    *   Automates the entire process of failover and data recovery.

### Conclusion ✅

These are just a few ways to make EC2 instances highly available. The best approach depends on your specific needs and the level of automation you require. While these architectures may involve more initial setup and customization, they can significantly improve the resilience and availability of your applications. 💡 **Tip:** Automation is key to achieving reliable high availability.

---

## Q & A

### ❓ Question 1

You are working on a **Serverless application** where you want to process objects uploaded to an S3 bucket. You have configured **S3 Events** on your S3 bucket to invoke a Lambda function every time an object is uploaded.

You want to ensure that events that **can’t be processed** are sent to a **Dead Letter Queue (DLQ)** for further processing. Which AWS service should you use to set up the DLQ?

Options:

1. S3 Events 
2. SNS Topic 
3. Lambda Function 

<details>

<summary>Explanation</summary>

✅ **Correct Answer:** **Lambda Function**

* **S3 Events**: Used only to trigger notifications (e.g., invoking Lambda, SNS, SQS). They cannot configure a DLQ.
* **SNS Topic**: Can be used as an event destination but **does not handle DLQs directly** in this scenario.
* **Lambda Function**: When a Lambda function is invoked **asynchronously** (such as via S3 Events), you can configure a **DLQ (SQS or SNS)** in the Lambda function settings to capture failed events.

**Key Point:** Since the **invocation is asynchronous**, the DLQ must be configured on the **Lambda side** to handle unprocessed events.

✅ **Summary:** To set up a DLQ for unprocessed S3-triggered events, you must configure it at the **Lambda Function level**, not on S3 Events or SNS.

</details>

---

### ❓ Question 2

As a **Solutions Architect**, you have created an architecture for a company that includes the following AWS services:

* CloudFront
* Web Application Firewall (AWS WAF)
* AWS Shield
* Application Load Balancer
* EC2 instances managed by an Auto Scaling Group

Sometimes the company receives **malicious requests** and wants to **block these IP addresses**.

According to your architecture, **where should you do it?**

**Options:**

1. CloudFront
2. AWS WAF
3. AWS Shield
4. ALB Security Group
5. EC2 Security Group
6. NACL

<details>

<summary>Explanation</summary>

✅ **Correct Answer:** **AWS WAF**

* **AWS WAF (Web Application Firewall)**: Best for filtering and blocking malicious requests (e.g., SQL injection, XSS, IP blacklisting) before they reach your application. Works with **CloudFront, ALB, and API Gateway**.
* **CloudFront**: A CDN service, not designed for request filtering.
* **AWS Shield**: Provides **DDoS protection**, but not fine-grained IP blocking.
* **ALB / EC2 Security Groups**: Control inbound/outbound traffic, but not effective against malicious **application-layer attacks**.
* **NACLs**: Operate at subnet level, not application-level filtering.

📌 **Key Point:** When you need to block **malicious traffic (e.g., IP-based, pattern-based)**, the correct service is **AWS WAF**, which integrates seamlessly with CloudFront or ALB to protect applications at Layer 7.

✅ **Summary:** To block malicious requests and IP addresses in this architecture, configure **AWS WAF** rules.

</details>

---
