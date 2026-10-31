# Design Resilient Architectures

## Question 1

**An online registration system hosted in an Amazon EKS cluster stores data to a `db.t4g.medium` Amazon Aurora DB cluster. The database performs well during regular hours but is unable to handle the traffic surge that occurs during flash sales. A solutions architect must move the database to Aurora Serverless while minimizing downtime and the impact on the operation of the application.**

**Which change should be taken to meet the objective?**

- Change the Aurora Instance class to Serverless
- Use AWS Database Migration Service (AWS DMS) to migrate to a new Aurora Serverless database.
- Add an Aurora Replica to the cluster and set its instance class to Serverless. Failover to the read replica and promote it to primary.
- Take a snapshot of the DB cluster. Use the snapshot to create a new Aurora DB cluster.

<details>
<summary><strong>Answer & Explanation</strong> 📝</summary>

### ✅ **Correct Answer**

**Add an Aurora Replica to the cluster and set its instance class to Serverless. Failover to the read replica and promote it to primary.**

> ⚠️ Correction (2026): this file used to give **AWS DMS** as the answer. That was based on Aurora Serverless **v1** (end of life 31 Mar 2025). With Aurora Serverless **v2**, AWS documents "add a Serverless v2 reader, then fail over" as the way to switch a provisioned cluster.

Aurora Serverless v2 is the `db.serverless` **instance class**, and one cluster can mix provisioned and Serverless v2 instances. You add a `db.serverless` reader, fail over to it, and it becomes the writer. The only downtime is the short failover ⏱️.

### ❌ **Incorrect Options (with reasons)**

<span style="color:red"><strong>#### 🔴 Change the Aurora Instance class to Serverless</strong></span>

Possible in Serverless v2, but changing the **writer** in place restarts it, so the database is down during the change. Adding a reader and failing over has less downtime.

<span style="color:red"><strong>#### 🔴 Take a snapshot of the DB cluster and create a new Aurora DB cluster</strong></span>

Although functional, this method causes **long downtime** 😴 because the application must be stopped until the new cluster is created and ready.

<span style="color:red"><strong>#### 🔴 Use AWS DMS to migrate to a new Aurora Serverless database</strong></span>

It works, but it needs a second cluster, a replication task and an application cutover. That is more effort and more risk than a failover inside the same cluster.

### 🟢 Summary

Provisioned Aurora → Aurora Serverless v2: **add a `db.serverless` reader and fail over to it** 🎯.

</details>

---

## Question 2

**A data analytics company, which uses machine learning to collect and analyze consumer data, is using Redshift cluster as their data warehouse. You are instructed to implement a disaster recovery plan for their systems to ensure business continuity even in the event of an AWS region outage.**

**Which of the following is the best approach to meet this requirement?**

- Create a scheduled job that will automatically take the snapshot of your Redshift Cluster and store it to an S3 bucket. Restore the snapshot in case of an AWS region outage.
- Do nothing because Amazon Redshift is a highly available, fully-managed data warehouse which can withstand an outage of an entire AWS region.
- Use Automated snapshots of your Redshift Cluster.
- Enable Cross-Region Snapshots Copy in your Amazon Redshift Cluster.

<details>
<summary><strong>Answer & Explanation</strong> 📝</summary>

### 🟢 **Correct Approach**

Configure **Amazon Redshift Cross-Region Snapshot Copy** to automatically copy **all new automated and manual snapshots** to another AWS Region. This ensures that your data is **protected** and can be **restored** even if an entire region experiences an outage 🌩️.

### ❌ **Incorrect Options (with reasons)**

<span style="color:red"><strong>#### 🔴 Create a scheduled job to take snapshots and store them in S3</strong></span>

While technically possible, this method involves **heavy manual work** 🛠️ and is **not the best practice**. Redshift offers a built-in, automated feature, so manual scripting should be avoided.

<span style="color:red"><strong>#### 🔴 Do nothing because Redshift is highly available</strong></span>

Even though Redshift is fully managed and offers high availability, it **does NOT automatically replicate snapshots across regions**.  
You **must** configure **cross-region snapshot copy** to ensure disaster recovery in case of **regional outages** 🚨.

<span style="color:red"><strong>#### 🔴 Relying only on Automated Snapshots</strong></span>

Automated snapshots are stored **within the same region** 📍.  
If the region goes down, these snapshots become **unavailable**. Therefore, they are **not sufficient** for cross-region disaster recovery.

### 🟢 Summary

To ensure **high availability**, **disaster recovery**, and **cross-region resilience**, you should enable **Cross-Region Snapshot Copy** on your Redshift cluster.

</details>

---

## Question 3

**A company has a VPC for its Human Resource department and another VPC located in different AWS regions for its Finance department. The Solutions Architect must redesign the architecture to allow the finance department to access all resources that are in the human resource department, and vice versa. An Intrusion Prevention System (IPS) must also be integrated for active traffic flow inspection and to block any vulnerability exploits.**

**Which network architecture design in AWS should the Solutions Architect set up to satisfy the above requirement?**

- Launch an AWS Transit Gateway and add VPC attachments to connect all departments. Set up AWS Network Firewall to secure the application traffic travelling between the VPCs.
- Establish a secure connection between the two VPCs using a NAT Gateway. Manage user sessions via the AWS Systems Manager Session Manager service.
- Create a Traffic Policy in Amazon Route 53 to connect the two VPCs. Configure the Route 53 Resolver DNS Firewall to do active traffic flow inspection and block any vulnerability exploits.
- Create a Direct Connect Gateway and add VPC attachments to connect all departments. Configure AWS Security Hub to secure the application traffic travelling between the VPCs.

<details>
<summary><strong>Answer & Explanation</strong> 📝</summary>

### 🟢 **Correct Answer**

**Launch an AWS Transit Gateway and add VPC attachments to connect all departments. Set up AWS Network Firewall to secure the application traffic traveling between the VPCs.**

A Transit Gateway acts as a **centralized network hub** 🧩 to interconnect multiple VPCs and on-prem networks. It supports **intra-region and inter-region peering**, provides **encrypted traffic**, and allows scalable routing.

AWS Network Firewall adds **stateful inspection**, **IPS**, and **web filtering**, ensuring secure traffic flow 🔐 between VPCs.

A transit gateway is a network transit hub that you can use to interconnect your virtual private clouds (VPCs) and on-premises networks. As your cloud infrastructure expands globally, inter-Region peering connects transit gateways together using the AWS Global Infrastructure. Your data is automatically encrypted and never travels over the public internet.

<img src="https://media.tutorialsdojo.com/aws-transit-gateway-mesh-aec3.png"
     alt="AWS Transit Gateway mesh diagram"
     width="600" />

A transit gateway attachment is both a source and a destination of packets. You can attach the following resources to your transit gateway:

- One or more VPCs.
- One or more VPN connections
- One or more AWS Direct Connect gateways
- One or more Transit Gateway Connect attachments
- One or more transit gateway peering connections

AWS Transit Gateway deploys an elastic network interface within VPC subnets, which is then used by the transit gateway to route traffic to and from the chosen subnets. You must have at least one subnet for each Availability Zone, which then enables traffic to reach resources in every subnet of that zone. During attachment creation, resources within a particular Availability Zone can reach a transit gateway only if a subnet is enabled within the same zone. If a subnet route table includes a route to the transit gateway, traffic is only forwarded to the transit gateway if the transit gateway has an attachment in the subnet of the same Availability Zone.

Intra-region peering connections are supported. You can have different transit gateways in different Regions.

AWS Network Firewall is a managed service that makes it easy to deploy essential network protections for all of your Amazon Virtual Private Clouds (VPCs). The service can be setup with just a few clicks and scales automatically with your network traffic, so you don’t have to worry about deploying and managing any infrastructure. AWS Network Firewall’s flexible rules engine lets you define firewall rules that give you fine-grained control over network traffic, such as blocking outbound Server Message Block (SMB) requests to prevent the spread of malicious activity.

<img src="https://media.tutorialsdojo.com/aws-network-firewall-diagram.png"
     alt="AWS Network Firewall architecture"
     width="600" />

AWS Network Firewall includes features that provide protections from common network threats. AWS Network Firewall’s stateful firewall can incorporate context from traffic flows, like tracking connections and protocol identification, to enforce policies such as preventing your VPCs from accessing domains using an unauthorized protocol. AWS Network Firewall’s intrusion prevention system (IPS) provides active traffic flow inspection so you can identify and block vulnerability exploits using signature-based detection. AWS Network Firewall also offers web filtering that can stop traffic to known bad URLs and monitor fully qualified domain names.

### ❌ **Incorrect Options (with reasons)**

<span style="color:red"><strong>#### 🔴 Create a Route 53 Traffic Policy + DNS Firewall</strong></span>

- Route 53 **Traffic Policies** are mainly for **geoproximity routing** and large-scale DNS record management—not VPC connectivity.  
- Route 53 **Resolver DNS Firewall** only filters **outbound DNS traffic** 🌐.  
- It **cannot inspect flows**, **cannot block exploits**, and **cannot secure VPC-to-VPC application traffic**.

<span style="color:red"><strong>#### 🔴 Use a NAT Gateway to connect the two VPCs + Systems Manager Session Manager</strong></span>

- A **NAT Gateway** only provides **outbound internet/NAT connectivity** for private subnets.  
- It **cannot connect VPCs**, especially **across regions** 🚫.  
- **Session Manager** is only for remote EC2 management (SSH-less access), not for user session handling or VPC connectivity.

<span style="color:red"><strong>#### 🔴 Create a Direct Connect Gateway + VPC attachments + Security Hub</strong></span>

- A **Direct Connect Gateway** is used to connect **on-premises** to AWS—not for **VPC-to-VPC inter-region connectivity**.  
- You still require a **Transit Gateway** to interconnect multiple VPCs.  
- **AWS Security Hub** is a **security posture management** tool. It does **not secure network traffic** on its own.

### 🟢 **Summary**

To connect multiple VPCs across regions and secure traffic between them:  
**Use Transit Gateway (connectivity) + AWS Network Firewall (security).**  
This ensures a scalable, secure, and AWS-recommended architecture ⭐.

</details>

---

## Question 4

**A company is setting up a cloud architecture for an international money transfer service to be deployed in AWS which will have thousands of users around the globe. The service should be available 24/7 to avoid any business disruption and should be resilient enough to handle the outage of an entire AWS region. To meet this requirement, the Solutions Architect has deployed their AWS resources to multiple AWS Regions. He needs to use Route 53 and configure it to set all of the resources to be available all the time as much as possible. When a resource becomes unavailable, Route 53 should detect that it’s unhealthy and stop including it when responding to queries.**

**Which of the following is the most fault-tolerant routing configuration that the Solutions Architect should use in this scenario?**

- Configure an Active–Passive Failover with Multiple Primary and Secondary Resources.
- Configure an Active–Active Failover with Weighted routing policy.
- Configure an Active–Passive Failover with Weighted Records.
- Configure an Active–Active Failover with One Primary and One Secondary Resource.

<details>
<summary><strong>Answer & Explanation</strong> 📝</summary>

You can use Route 53 health checking to configure active-active and active-passive failover configurations. You configure active-active failover using any routing policy (or combination of routing policies) other than failover, and you configure active-passive failover using the failover routing policy.

<img src="https://media.tutorialsdojo.com/AWS-Route53-Weighted.PNG"
     alt="Route 53 weighted routing diagram"
     width="600" />

**Active-Active Failover**

Use this failover configuration when you want all of your resources to be available the majority of the time. When a resource becomes unavailable, Route 53 can detect that it’s unhealthy and stop including it when responding to queries.

In active-active failover, all the records that have the same name, the same type (such as A or AAAA), and the same routing policy (such as weighted or latency) are active unless Route 53 considers them unhealthy. Route 53 can respond to a DNS query using any healthy record.

Hence, Configuring an Active-Active Failover with Weighted routing policy is correct.

**Active-Passive Failover**

Use an active-passive failover configuration when you want a primary resource or group of resources to be available the majority of the time and you want a secondary resource or group of resources to be on standby in case all the primary resources become unavailable. When responding to queries, Route 53 includes only the healthy primary resources. If all the primary resources are unhealthy, Route 53 begins to include only the healthy secondary resources in response to DNS queries.

### 🟢 **Correct Answer**

**Configuring an Active-Active Failover with Weighted Routing Policy** is correct.  
In an **Active-Active** setup, **all resources stay available** simultaneously, and traffic is distributed using **weighted routing**, ensuring optimal load sharing and high availability ⭐.


### 🟢 **Summary**

For scenarios requiring **all resources to always be active**,  
➡️ **Active-Active Failover + Weighted Routing** is the correct approach.

</details>

---

## Question 5

**A Solutions Architect is designing a highly available environment for an application. She plans to host the application on EC2 instances within an Auto Scaling Group. One of the conditions requires data stored on root EBS volumes to be preserved if an instance terminates.
What should be done to satisfy the requirement?**

- Enable the Termination Protection option for all EC2 instances.
- Set the value of `DeleteOnTermination` attribute of the EBS volumes to `False`.
- Use AWS DataSync to replicate root volume data to Amazon S3.
- Configure ASG to suspend the health check process for each EC2 instance.

<details>
<summary><strong>Answer & Explanation</strong> 📝</summary>

By default, Amazon EBS root device volumes are automatically deleted when the instance terminates. However, by default, any additional EBS volumes that you attach at launch, or any EBS volumes that you attach to an existing instance persist even after the instance terminates. This behavior is controlled by the volume’s DeleteOnTermination attribute, which you can modify.

To preserve the root volume when an instance terminates, change the DeleteOnTermination attribute for the root volume to False.

### 🟢 **Correct Answer**

Set the value of the **DeleteOnTermination** attribute of the EBS volumes to **False**.  
This ensures that **EBS volumes (including the root volume)** are **preserved** even after the EC2 instance terminates.  
You can modify this attribute during instance launch or later through the **Console**, **CLI**, or **API**.

### ❌ **Incorrect Options (with reasons)**

<span style="color:red"><strong>#### 🔴 Use AWS DataSync to replicate root volume data to Amazon S3</strong></span>

AWS DataSync **does not support EBS volumes**.  
It only works with:

- NFS  
- SMB  
- Self-managed object storage  
- Snowcone (discontinued for new orders in Nov 2024)  
- S3  
- EFS  
- FSx for Windows  

Therefore, this option is **not applicable** for EBS root volumes.

<span style="color:red"><strong>#### 🔴 Configure ASG to suspend the health check process</strong></span>

Suspending health checks stops the Auto Scaling Group from replacing **unhealthy instances**, which can lead to **application downtime** and **reduced availability**.  
It does **not** affect EBS volume termination behavior.

<span style="color:red"><strong>#### 🔴 Enable Termination Protection for all EC2 instances</strong></span>

Termination Protection only blocks **`TerminateInstances` calls** (from the console, CLI or API). It does not stop Auto Scaling from terminating instances.  
It does **not** preserve EBS volumes nor affect **DeleteOnTermination** behavior.

### 🟢 **Summary**

To ensure EBS volumes remain after instance termination,  
➡️ **Set DeleteOnTermination = False** for the volumes.

</details>

---

## Question 6

**Amazon Elastic Kubernetes Service (Amazon EKS) is used by an e-commerce company to deploy and manage its containerized applications. The website experiences a surge in traffic around the holidays, which significantly adds to the effort. The goal is to ensure that its underlying infrastructure automatically scales in and out in response to demand.**

**Which of the following would meet the requirements with the LEAST amount of operational overhead? (Select TWO.)**

* Set up Karpenter to automatically adjust the number of nodes in the EKS cluster when pods fail or are rescheduled onto other nodes.
* Enable the Cluster Autoscaler for Amazon EKS to automatically manage the number of nodes based on the resource needs of the pods.
* Install the Kubernetes Metrics Server to the Amazon EKS cluster and activate Vertical Pod Autoscaler.
* Install the Kubernetes Metrics Server to the Amazon EKS cluster and activate the Horizontal Pod Autoscaling.
* Use CloudWatch Alarms to trigger scaling for containerized applications.

<details>
<summary><strong>Answer & Explanation</strong> 📝</summary>

The Kubernetes Horizontal Pod Autoscaler automatically scales the number of Pods in a deployment, replication controller, or replica set based on that resource’s CPU utilization. This can help your applications scale out to meet increased demand or scale in when resources are not needed, thus freeing up your nodes for other applications. When you set a target CPU utilization percentage, the Horizontal Pod Autoscaler scales your application in or out to try to meet that target.

Autoscaling is a function that automatically scales your resources up or down to meet changing demands. This is a major Kubernetes function that would otherwise require extensive human resources to perform manually.

Amazon EKS supports two autoscaling products:
– Karpenter
– Cluster Autoscaler

The Kubernetes Cluster Autoscaler automatically adjusts the number of nodes in your cluster when pods fail or are rescheduled onto other nodes. The Cluster Autoscaler uses Auto Scaling groups.

Karpenter is a flexible, high-performance Kubernetes cluster autoscaler that launches appropriately sized compute resources, like Amazon EC2 instances, in response to changing application load. It integrates with AWS to provision compute resources that precisely match workload requirements.

### 🟢 **Correct Answers**
- **Install the Kubernetes Metrics Server** in the Amazon EKS cluster and **activate the Horizontal Pod Autoscaler (HPA)**.  
  HPA enables **automatic horizontal scaling** (scale in/out) of pods based on CPU utilization or custom metrics.

- **Set up Karpenter** to dynamically and efficiently **adjust the number of worker nodes** in the EKS cluster.  
  Karpenter provides faster provisioning, optimal instance selection, and reacts quickly to scheduling needs.

Together, HPA + Karpenter enable **full-stack autoscaling**:  
🚀 **Pods scale horizontally** based on workload.  
🧩 **Nodes scale automatically** to meet pod resource requirements.

### ❌ **Incorrect Options (with reasons)**

<span style="color:red"><strong>#### 🔴 Install Metrics Server + Activate Vertical Pod Autoscaler</strong></span>  
Vertical Pod Autoscaler performs **vertical scaling** (scale up/down pod CPU & memory), not horizontal scaling.  
The question explicitly requires **"scale in and out"**, which refers to **horizontal pod autoscaling**, making this approach inappropriate.

<span style="color:red"><strong>#### 🔴 Use CloudWatch Alarms to trigger scaling</strong></span>  
CloudWatch alarms introduce **latency** between metric collection and autoscaling.  
This approach lacks the **real-time responsiveness** needed for rapidly changing traffic patterns and **is not integrated** with Kubernetes scheduling.

<span style="color:red"><strong>#### 🔴 Enable the Kubernetes Cluster Autoscaler</strong></span>  
Cluster Autoscaler **does** adjust node counts, but:  
- It is **slower** than Karpenter ⏳  
- Requires **more manual configuration & tuning**  
- Cannot optimize instance types as efficiently  
Karpenter offers a **more dynamic**, **high-performance**, and **cost-optimized** solution, making it the preferred choice here.

### 🟢 **Summary**
To support efficient, responsive EKS scaling:  
➡️ **Metrics Server + HPA for Pods**  
➡️ **Karpenter for Nodes**

This combination ensures **automatic**, **responsive**, and **high-performance** autoscaling for the entire EKS environment.

</details>

---

## Question 7

**A global company has deployed numerous AWS Outposts servers in various remote locations worldwide. These servers frequently need to download software updates consisting of multiple files from an S3 bucket in the us-west-2 region. The company is experiencing significant delays in distributing these updates across all servers. What solution would most effectively reduce the deployment latency while minimizing operational overhead?**

* Use Amazon S3 Transfer Acceleration on the existing S3 bucket and have the Outposts servers use the Transfer Acceleration endpoint for downloads.
* Set up AWS Global Accelerator to route traffic from Outposts servers to the nearest AWS edge location, then use private VIF connections to access the S3 bucket in us-west-2.
* Set up an Amazon CloudFront distribution with the us-west-2 S3 bucket as the primary origin and create a secondary origin in another region, implementing a CachingDisabled cache policy. Use signed URLs for downloads.
* Create an Amazon CloudFront distribution with the us-west-2 S3 bucket as the origin. Use signed URLs for software downloads.

<details>
<summary><strong>Answer & Explanation</strong> 📝</summary>

AWS Outposts is a fully managed service that brings AWS infrastructure, services, APIs, and tools directly to customer locations. It’s tailored for workloads that must remain on-premises due to low latency or the need for local data processing.

Amazon S3 is an object storage service offering industry-leading scalability, data availability, security, and performance. It’s commonly used to store large amounts of unstructured data, including datasets for machine learning.

Amazon CloudFront is a fast CDN service that securely delivers data, videos, applications, and APIs to customers worldwide, ensuring low latency and high transfer speeds. It seamlessly integrates with other AWS services and provides easy-to-use APIs for developers to customize the service to meet their needs. CloudFront utilizes a global network of edge locations to cache content closer to end users, enhancing performance and reducing the load on origin servers.

To address the company’s challenge of distributing large software updates to AWS Outposts servers globally with reduced latency and minimal operational overhead, use Amazon CloudFront with the S3 bucket in us-west-2 as the origin. This solution provides global content delivery through edge locations, reducing latency for all Outposts servers. It also offers caching capabilities, which can significantly speed up access to frequently downloaded files. Next, the Signed URLs add an extra layer of security for software distribution. Lastly, it can potentially reduce overall data transfer costs compared to direct S3 access, and once set up, CloudFront requires minimal ongoing management.

### 🟢 **Correct Answer**
**Create an Amazon CloudFront distribution with the us-west-2 S3 bucket as the origin, and use signed URLs for software downloads.**

CloudFront is the best solution for distributing **large software updates globally** because:

- 🌍 **Global Edge Network** → Reduces latency for all AWS Outposts servers  
- ⚡ **Caching** → Improves download speeds for frequently accessed files  
- 🔐 **Signed URLs** → Secures access to sensitive software updates  
- 🛠️ **Low operational overhead** → Once configured, CloudFront requires minimal maintenance  
- 💸 **Cost-efficient** → Can reduce data transfer costs vs. direct S3 access  

This combination meets all requirements: **global performance**, **security**, and **efficiency**.

### ❌ **Incorrect Options (with reasons)**

#### 🔴 CloudFront with Secondary Origin + CachingDisabled  
- A secondary origin in another region adds **no meaningful benefit**.  
- A **CachingDisabled** policy removes CloudFront’s biggest advantage: **edge caching**.  
- This results in **higher latency** and **unnecessary complexity**.

#### 🔴 AWS Global Accelerator + Private VIF to access S3  
- Global Accelerator is used for **improving application availability and performance**, not optimizing S3 object downloads.  
- **S3 is not directly integrated** with Global Accelerator.  
- **Private VIF** is used with **Direct Connect**, not S3 connections.  
- This setup is **overly complex** and doesn’t meet the use case.

#### 🔴 Use S3 Transfer Acceleration  
- May not give consistent benefits for all Outposts locations.  
- Adds **extra data transfer costs**.  
- Does **not provide caching**, which is essential for distributing large, frequently downloaded files.  
- Does not reduce operational overhead as effectively as CloudFront.

### 🟢 **Summary**
To distribute large software updates to AWS Outposts servers globally with the **lowest latency** and **minimal management**,  
➡️ **Use CloudFront with the S3 bucket (us-west-2) as the origin + Signed URLs.**

</details>

---
