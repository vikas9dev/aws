# ELB

## **Question 1**

**A company hosted an e-commerce website on an Auto Scaling group of Amazon EC2 instances behind an Application Load Balancer. The Solutions Architect noticed that the website is receiving a high number of illegitimate external requests from multiple systems with frequently changing IP addresses. To address the performance issues, the Solutions Architect must implement a solution that would block these requests while having minimal impact on legitimate traffic.**

Which of the following options fulfills this requirement?

**Options:**

* Create a regular rule in AWS WAF and associate the web ACL to an Application Load Balancer.
* Create a custom network ACL and associate it with the subnet of the Application Load Balancer to block the offending requests.
* Create a rate-based rule in AWS WAF and associate the web ACL to an Application Load Balancer.
* Create a custom rule in the security group of the Application Load Balancer to block the offending requests.

<details>

<summary><strong>Answer & Explanation</strong> 📝</summary>

🟢 **AWS WAF (Web Application Firewall)** is tightly integrated with:

* **Amazon CloudFront**
* **Application Load Balancer (ALB)**
* **Amazon API Gateway**
* **AWS AppSync**

These services are commonly used by AWS customers to deliver **web content and APIs** securely and efficiently.

When you use **AWS WAF on Amazon CloudFront**, your rules **run globally** across all **AWS Edge Locations**, close to your end users — ensuring that **security does not come at the cost of performance**.
💡 **Blocked requests** are stopped **before they reach your web servers.**

When you use **AWS WAF on regional services** (like ALB, API Gateway, or AppSync), your rules run **within the specific region**, protecting both **Internet-facing** and **internal resources**.

#### 🟢 **Rate-Based Rules**

A **rate-based rule** tracks the **rate of requests** from each originating **IP address** and triggers the rule action when a particular **threshold** is exceeded.

* You can define the limit as the **number of requests per evaluation window** of 1, 2, 5 (default) or 10 minutes. The lowest limit is 10. (Older material says only "per 5-minute period".)
* It is useful for **throttling illegitimate requests** or **mitigating DDoS-like behavior** without affecting genuine users.

📌 Example:
If an IP exceeds 1,000 requests in 5 minutes, AWS WAF automatically blocks further requests from that IP until the rate falls below the limit.

#### 🟢 **Scenario Requirement**

The requirement is to **limit excessive requests** from potentially **malicious IPs** while **allowing legitimate traffic** to continue smoothly.

✅ **Solution:**
Create an **AWS WAF web ACL (Access Control List)** using a **rate-based rule**, and then **associate** that web ACL with your **Application Load Balancer (ALB)**.

When the **rate-based rule** triggers, **AWS WAF** automatically **blocks or throttles** requests from the offending IPs until their request rate returns below the threshold.

### ✅ **Correct Answer**

🟢 **Create a rate-based rule in AWS WAF and associate the web ACL to an Application Load Balancer.**

### ❌ **Incorrect Options**

<span style="color:red"><strong>🚫 Option: Create a regular rule in AWS WAF and associate the web ACL to an Application Load Balancer.</strong></span>  
❌ **Reason:** Regular rules match only the **defined conditions** (e.g., specific IPs, headers, or strings).  
They **do not support rate-limiting behavior**.  
To control excessive requests, you must use a **rate-based rule** instead.

<span style="color:red"><strong>🚫 Option: Create a custom network ACL and associate it with the subnet of the Application Load Balancer to block the offending requests.</strong></span>  
❌ **Reason:**  
While **NACLs** can block **specific IP ranges**, they **cannot dynamically limit requests** based on traffic rate.  
They also lack the intelligence to handle **rapidly changing IPs** used by attackers.

<span style="color:red"><strong>🚫 Option: Create a custom rule in the security group of the Application Load Balancer to block the offending requests.</strong></span>  
❌ **Reason:**  
**Security groups** can only **allow** incoming traffic — they **cannot explicitly deny** or **rate-limit** requests.  
They do not provide the **request-level filtering** or **rate-based control** that **AWS WAF** offers.

✅ **Final Answer:**
🟢 **Create a rate-based rule in AWS WAF and associate the web ACL to an Application Load Balancer.**

</details>

---

## **Question 2**

A DevOps Engineer is required to design a cloud architecture in AWS. The Engineer is planning to develop a highly available and fault-tolerant architecture consisting of an Elastic Load Balancer and an Auto Scaling group of EC2 instances deployed across multiple Availability Zones. This will be used by an online accounting application that requires path-based routing, host-based routing, and bi-directional streaming using Remote Procedure Call (gRPC).

Which configuration will satisfy the given requirement?

**Options:**

* Configure a Network Load Balancer in front of the auto-scaling group. Use a UDP listener for routing.
* Configure a Network Load Balancer in front of the auto-scaling group. Create an AWS Global Accelerator and set the load balancer as an endpoint.
* Configure a Gateway Load Balancer in front of the auto-scaling group. Ensure that the IP Listener Routing uses the GENEVE protocol on port 6081 to allow gRPC response traffic.
* Configure an Application Load Balancer in front of the auto-scaling group. Select gRPC as the protocol version.

<details>

<summary><strong>Answer & Explanation</strong> 📝</summary>

🟢 **Application Load Balancer (ALB)** operates at the **request level (Layer 7)** of the OSI model and intelligently routes traffic to targets such as:

* 🖥️ **Amazon EC2 instances**
* 🧱 **Containers** (ECS or EKS)
* 🌐 **IP addresses**
* ⚙️ **AWS Lambda functions**

#### 🟢 **Key Features**

✅ **Layer 7 Content-Based Routing**
The **Application Load Balancer** routes requests based on **the content of the request**, enabling precise control for modern application architectures.
You can route traffic based on:

* **Host field** 🏠 (e.g., `api.example.com`)
* **Path URL** 🌐 (e.g., `/orders` or `/users`)
* **HTTP header**
* **HTTP method** (e.g., GET, POST)
* **Query string** parameters
* **Source IP address**

✅ **Support for Modern Architectures**
ALB is ideal for **HTTP** and **HTTPS** traffic in **microservices** or **container-based applications**, supporting advanced routing mechanisms required in **distributed environments**.

✅ **Simplified Security Management**
**ALB** automatically ensures that your application always uses the **latest SSL/TLS ciphers and protocols**, providing:

* Strong **encryption and security** 🔒
* Easier **certificate management**
* Compliance with **modern security standards**

✅ **Support for gRPC Traffic**
**gRPC** is a high-performance, open-source, universal RPC framework that uses **HTTP/2** for transport.
ALB supports **gRPC routing and load balancing**, enabling communication between:

* **Microservices using gRPC**
* **gRPC-enabled clients and backend services**

💡 This allows you to **introduce gRPC traffic management seamlessly** without changing client or server infrastructure.

### ✅ **Correct Answer**

🟢 **Configure an Application Load Balancer in front of the auto-scaling group. Select gRPC as the protocol version.**

📌 **Reason:**
The ALB operates at **Layer 7** and supports **HTTP/2-based gRPC traffic**, making it ideal for **microservice-to-microservice** or **client-to-service** communication scenarios.

### ❌ **Incorrect Options**

<span style="color:red"><strong>🚫 Option: Configure a Network Load Balancer in front of the auto-scaling group. Use a UDP listener for routing.</strong></span>  
❌ **Reason:**  
**Network Load Balancers (NLBs)** operate at **Layer 4 (Transport layer)** and **do not support gRPC**, which requires **Layer 7 (Application layer)** features.

<span style="color:red"><strong>🚫 Option: Configure a Gateway Load Balancer in front of the auto-scaling group. Ensure that the IP Listener Routing uses the GENEVE protocol on port 6081 to allow gRPC response traffic.</strong></span>  
❌ **Reason:**  
**Gateway Load Balancers (GWLBs)** operate at **Layer 3 and Layer 4**, using the **GENEVE protocol** for network appliance traffic, not **Layer 7 protocols** like gRPC.

<span style="color:red"><strong>🚫 Option: Configure a Network Load Balancer in front of the auto-scaling group. Create an AWS Global Accelerator and set the load balancer as an endpoint.</strong></span>  
❌ **Reason:**  
**AWS Global Accelerator** enhances **network routing performance** over the AWS global network but **does not handle gRPC load balancing** or **application-layer routing.**

### 🧠 **Summary**

✅ **Use Case:** Advanced Layer 7 routing with gRPC-enabled microservices
✅ **Solution:** **Application Load Balancer with gRPC protocol**
✅ **Benefits:**

* Layer 7 content-based routing
* HTTP/2 and gRPC support
* SSL/TLS management
* High scalability and availability

</details>

---

## **Question 3**

**A company is hosting its web application in an Auto Scaling group of EC2 instances behind an Application Load Balancer. Recently, the Solutions Architect identified a series of SQL injection attempts and cross-site scripting attacks to the application, which had adversely affected their production data.**

Which of the following should the Architect implement to mitigate this kind of attack?

**Options:**

* Use Amazon GuardDuty to prevent any further SQL injection and cross-site scripting attacks in your application.
* Set up security rules that block SQL injection and cross-site scripting attacks in AWS Web Application Firewall (WAF). Associate the rules to the Application Load Balancer.
* Using AWS Firewall Manager, set up security rules that block SQL injection and cross-site scripting attacks. Associate the rules to the Application Load Balancer.
* Block all the IP addresses where the SQL injection and cross-site scripting attacks originated using the Network Access Control List.

<details>

<summary><strong>Answer & Explanation</strong> 📝</summary>

🟢 **AWS WAF (Web Application Firewall)** is a **security service** that lets you **monitor and filter HTTP/HTTPS requests** sent to your:

* **Amazon API Gateway APIs**
* **Amazon CloudFront distributions**
* **Application Load Balancers (ALBs)**

It allows you to **control access to your content** based on custom **conditions**, such as:

* **IP addresses** of incoming requests
* **Query string values**
* **Headers, URI paths**, or other **request properties**

Depending on how you configure it, AWS WAF can **allow**, **block**, or **count** specific requests.

#### 🟢 **Basic Behaviors of AWS WAF**

✅ **1. Allow all requests except the ones you specify**
Useful for **public websites** — you allow general access but **block known attackers or malicious requests**.

✅ **2. Block all requests except the ones you specify**
Useful for **restricted or internal sites**, where you want to allow only certain **IPs or conditions**.

✅ **3. Count requests that match certain properties**
Ideal for **testing WAF rules** before enforcement — allows you to monitor traffic and ensure that legitimate requests aren't accidentally blocked before changing rules to "Block" or "Allow."

#### 🟢 **Security Use Case: Protect Against SQL Injection and XSS**

In this scenario, the goal is to **block SQL Injection** and **Cross-Site Scripting (XSS)** attacks targeting your web application.

✅ **Best Solution:**
**Set up security rules in AWS WAF** that block **SQL Injection** and **Cross-Site Scripting** attack patterns.
Then, **associate those WAF rules** to your **Application Load Balancer (ALB)**.

This ensures that malicious requests are identified and blocked **in the Region, at the ALB**, before reaching your backend servers. (WAF runs at the edge only when it is attached to CloudFront.)

### ✅ **Correct Answer**

🟢 **Set up security rules that block SQL injection and cross-site scripting attacks in AWS Web Application Firewall (WAF). Associate the rules to the Application Load Balancer.**

### ❌ **Incorrect Options**

<span style="color:red"><strong>🚫 Option: Use Amazon GuardDuty to prevent SQL injection and XSS attacks.</strong></span>  
❌ **Reason:**  
**Amazon GuardDuty** is a **threat detection service**, not a web firewall.  
It identifies suspicious activity and compromised resources but **cannot block or filter requests**.

<span style="color:red"><strong>🚫 Option: Use AWS Firewall Manager to set up security rules and associate them with the ALB.</strong></span>  
❌ **Reason:**  
**AWS Firewall Manager** is an **administration tool** that helps you manage **WAF rules**, **Shield Advanced**, and **security policies** across **multiple AWS accounts and resources**.  
It **does not create or enforce** the actual security rules — it just manages them.

<span style="color:red"><strong>🚫 Option: Block the IP addresses of the attackers using a Network ACL (NACL).</strong></span>  
❌ **Reason:**  
**NACLs** control **inbound and outbound traffic** at the **subnet level**.  
They **cannot detect or prevent** **SQL injection** or **XSS attacks**, as these are **application-layer (Layer 7)** threats.

### 🧠 **Summary**

✅ **Best Protection Method:**
Use **AWS WAF** to define and apply **Layer 7 filtering rules** against **SQL Injection** and **XSS attacks**, then **associate the web ACL** with your **Application Load Balancer**.

💡 **Result:**
Malicious requests are blocked **before reaching your application**, improving both **security** and **availability**.

</details>

---

## **Question 4**

A social media company needs to capture the detailed information of all HTTP requests that went through its public-facing Application Load Balancer every five minutes. The client’s IP address and network latencies must also be tracked, while the load balancer’s availability is monitored separately through the ELB health check. The captured data should be used for analyzing traffic patterns and for troubleshooting Docker applications orchestrated by the Amazon ECS Anywhere service.

Which of the following options meets the customer requirements with the LEAST amount of overhead?

**Options:**

* Integrate Amazon EventBridge metrics on the Application Load Balancer to capture the client IP address. Use Amazon CloudWatch `GetMetricData` to retrieve and analyze traffic-related metrics.
* Enable access logs on the Application Load Balancer. Integrate the ECS cluster with Amazon CloudWatch Application Insights to analyze traffic patterns and simplify troubleshooting.
* Install and run the AWS X-Ray daemon on the ECS cluster. Use the Amazon CloudWatch ServiceLens to analyze the traffic that goes through the application.
* Enable AWS CloudTrail for their Application Load Balancer. Use the CloudTrail Lake to analyze and troubleshoot the application traffic.

<details>

<summary><strong>Answer & Explanation</strong> 📝</summary>

🟢 **Amazon CloudWatch Application Insights** enables **observability** for your **applications and AWS resources**, helping you **detect, analyze, and troubleshoot** issues more efficiently.
It uses **machine learning (via Amazon SageMaker)** and other AWS technologies to automatically **monitor**, **analyze**, and **visualize** your application's health and performance.

#### 🟢 **How CloudWatch Application Insights Works**

✅ When you **add your application**, Application Insights automatically:

* **Scans resources** and identifies components such as **SQL Server databases**, **Microsoft IIS/Web tiers**, or **ECS services**.
* **Configures metrics and logs** in **CloudWatch** for those components.
* **Analyzes metric patterns** using **historical data** to **detect anomalies**.
* **Correlates issues** using built-in rules and ML classification algorithms.
* **Creates automated dashboards** to visualize **problem areas** and their **severity levels**, helping you prioritize troubleshooting.

💡 This reduces **Mean Time To Repair (MTTR)** by allowing you to isolate and resolve issues quickly.

#### 🟢 **Elastic Load Balancing Access Logs**

**Elastic Load Balancing (ELB)** provides **access logs** that capture detailed request-level data such as:

* 🕒 **Timestamp** of the request
* 🌐 **Client IP address**
* ⚙️ **Target response time and latencies**
* 🔗 **Request path and server responses**

📁 These **access logs** are stored as **compressed files in an Amazon S3 bucket** that you specify.
Access logging is **disabled by default**, but you can **enable or disable it anytime**.

🔍 Once enabled, these logs can be used to:

* Analyze **traffic patterns**
* Monitor **user behavior**
* Troubleshoot **performance bottlenecks**

### ✅ **Correct Answer**

🟢 **Enable access logs on the Application Load Balancer. Integrate the ECS cluster with Amazon CloudWatch Application Insights to analyze traffic patterns and simplify troubleshooting.**

📌 **Why this is correct:**

* **Access logs** provide the **client IP address**, **request details**, and **latency data**.
* **CloudWatch Application Insights** provides **automated anomaly detection** and **application health dashboards**.
  Together, these tools give a **comprehensive view** of both **network-level** and **application-level** performance.

### ❌ **Incorrect Options**

<span style="color:red"><strong>🚫 Option: Enable AWS CloudTrail for their Application Load Balancer. Use CloudTrail Lake to analyze and troubleshoot the application traffic.</strong></span>  
❌ **Reason:**  
**AWS CloudTrail** tracks **API calls and account activity**, **not application traffic**.

* It records **resource changes** (like ALB creation/deletion), not **HTTP requests**.  
* **CloudTrail Lake** is designed for **querying API event data**, not **analyzing client request patterns**.

<span style="color:red"><strong>🚫 Option: Install and run the AWS X-Ray daemon on the ECS cluster. Use Amazon CloudWatch ServiceLens to analyze traffic.</strong></span>  
❌ **Reason:**  
**AWS X-Ray** traces requests and latency **within your application**, not **at the load balancer level**.

* It **won't capture the client's IP address** or **ALB network latency**.  
* The question explicitly requires **tracking client IP and latency**, which is only possible through **ALB access logs**.
* Also, the X-Ray SDKs and daemon are in maintenance mode since 25 Feb 2026. AWS now recommends OpenTelemetry (ADOT) for tracing.

<span style="color:red"><strong>🚫 Option: Integrate Amazon EventBridge metrics on the Application Load Balancer to capture the client IP address. Use CloudWatch GetMetricData to retrieve traffic metrics.</strong></span>  
❌ **Reason:**  
**EventBridge** cannot **capture or log HTTP request data** such as client IP or latency.

* It handles **event-driven automation**, not **web traffic analysis**.  
* **CloudWatch GetMetricData** only retrieves existing **aggregated metrics**, not detailed per-request data.

### 🧠 **Summary**

| Feature                                | Purpose                       | Layer         | Tracks Client IP?        | Best Use Case              |
| -------------------------------------- | ----------------------------- | ------------- | ------------------------ | -------------------------- |
| 🟢 **AWS WAF**                         | Protects from attacks         | L7            | Yes                      | Security filtering         |
| 🟢 **ALB Access Logs**                 | Captures request details      | L7            | ✅ Yes                    | Traffic & latency analysis |
| 🟢 **CloudWatch Application Insights** | Application health monitoring | App           | ✅ Yes (via metrics/logs) | Automated troubleshooting  |
| 🔴 **CloudTrail**                      | Tracks API activity           | Control Plane | ❌ No                     | Audit resource changes     |
| 🔴 **EventBridge**                     | Event-driven automation       | Event         | ❌ No                     | Workflow triggers          |

✅ **Final Answer:**
🟢 **Enable access logs on the Application Load Balancer. Integrate the ECS cluster with Amazon CloudWatch Application Insights to analyze traffic patterns and simplify troubleshooting.**

</details>

---