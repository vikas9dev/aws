# CloudFront 

## **Question 1**

A company uses **Amazon CloudFront** to distribute static content stored in an S3 bucket. They recently introduced a **members-only feature** for premium media files. They must provide access to **multiple private media files only to paying subscribers**, **without changing the current URLs**.

**Which of the following is the most suitable solution?**

### **Options:**

1. Configure your CloudFront distribution to use Match Viewer as its Origin Protocol Policy which will automatically match the user request. This will allow access to the private content if the request is a paying member and deny it if it is not a member.
2. Use Signed Cookies to control who can access the private files in your CloudFront distribution by modifying your application to determine whether a user should have access to your content. For members, send the required `Set-Cookie` headers to the viewer which will unlock the content only to them.
3. Configure your CloudFront distribution to use Field-Level Encryption to protect your private data and only allow access to members.
4. Create a Signed URL with a custom policy which only allows the members to see the private files.

<details>
<summary><strong>Answer & Explanation</strong> 📝</summary>

Many companies that distribute content over the internet want to restrict access to documents, business data, media streams, or content that is intended for selected users, for example, users who have paid a fee. To securely serve this private content by using **CloudFront**, you can do the following:

✅ **– Require that your users access your private content by using special CloudFront signed URLs or signed cookies.**

✅ **– Require that your users access your content by using CloudFront URLs, not URLs that access content directly on the origin server (for example, Amazon S3 or a private HTTP server).**
Requiring CloudFront URLs isn't necessary, but **we recommend it** to prevent users from bypassing the restrictions that you specify in **signed URLs or signed cookies**.

**CloudFront signed URLs and signed cookies** provide the same basic functionality: they allow you to **control who can access your content**.

If you want to serve private content through CloudFront and you're trying to decide whether to use **signed URLs or signed cookies**, consider the following:

### ✅ **Use signed URLs for the following cases:**

🟢 **– You want to use an RTMP distribution.**
Signed cookies aren't supported for RTMP distributions.
> ⚠️ Note (2026): CloudFront removed RTMP distributions on Dec 31, 2020. Use HTTP streaming (HLS, DASH) on a normal distribution.

🟢 **– You want to restrict access to individual files**, for example, an installation download for your application.

🟢 **– Your users are using a client (for example, a custom HTTP client) that doesn't support cookies.**

### ✅ **Use signed cookies for the following cases:**

🟢 **– You want to provide access to multiple restricted files**, for example, all of the files for a video in HLS format or all of the files in the subscribers' area of a website.

🟢 **– You don't want to change your current URLs.**

### ✅ **Correct Answer**

**Use Signed Cookies** to control who can access the private files in your CloudFront distribution by modifying your application to determine whether a user should have access to your content. For members, send the required **Set-Cookie** headers to the viewer which will unlock the content only to them.

### ❌ **Incorrect Options (with reasons)**

<span style="color:red"><strong>🔴 The option that says: Configure your CloudFront distribution to use Match Viewer as its Origin Protocol Policy which will automatically match the user request. This will allow access to the private content if the request is a paying member and deny it if it is not a member is incorrect</strong></span> because a **Match Viewer** is an Origin Protocol Policy that configures CloudFront to communicate with your origin using HTTP or HTTPS, depending on the protocol of the viewer request. CloudFront caches the object only once even if viewers make requests using both HTTP and HTTPS protocols.

<span style="color:red"><strong>🔴 The option that says: Create a Signed URL with a custom policy which only allows the members to see the private files is incorrect</strong></span> because **Signed URLs** are primarily used for providing access to **individual files**, as shown in the above explanation. In addition, the scenario explicitly says that they **don't want to change their current URLs** which is why implementing **Signed Cookies** is more suitable than Signed URLs.

<span style="color:red"><strong>🔴 The option that says: Configure your CloudFront distribution to use Field-Level Encryption to protect your private data and only allow access to members is incorrect</strong></span> because **Field-Level Encryption** only allows you to securely upload user-submitted sensitive information to your web servers. It does **not** provide access to download multiple private files.

<img src="https://media.tutorialsdojo.com/amazon-cloud-front-signed-URL-signed-Cookies.png"
     alt="Amazon CloudFront signed URLs and signed cookies diagram"
     width="600" />

</details>

---

## **Question 2**

A travel photo-sharing website is using **Amazon S3** to serve high-quality photos to visitors. After a few days, the company discovers that other travel websites are **hotlinking** (directly linking to the images) and using these photos, resulting in **financial losses**.

**What is the MOST effective method to mitigate this issue?**

### **Options:**

1. Configure your S3 bucket to remove public read access and use pre-signed URLs with expiry dates.
2. Use Amazon CloudFront distributions for your photos.
3. Store and privately serve the high-quality photos on Amazon WorkDocs instead.
4. Block the IP addresses of the offending websites using NACL.

<details>
<summary><strong>Answer & Explanation</strong> 📝</summary>

The problem here is **hotlinking** — unauthorized third-party websites directly referencing and displaying your images from your S3 bucket.
To prevent this:

* Remove **public access** to S3 objects
* Use **pre-signed URLs** which grant temporary, controlled access to content only for authorized users

A pre-signed URL:

* Is generated by the S3 object owner
* Contains limited permissions (e.g., GET only)
* Has a custom **expiry time**
* Ensures unauthorized users cannot access your images once expired

> This prevents other websites from freely embedding your photos since the URL will expire or be tied to specific access rules.

In **Amazon S3**, all objects are **private by default**. Only the object owner has permission to access these objects. However, the object owner can optionally share objects with others by creating a **pre-signed URL**, using their own security credentials, to grant **time-limited permission** to download the objects.

When you create a **pre-signed URL** for your object, you must provide your **security credentials**, specify a **bucket name**, an **object key**, specify the **HTTP method (GET to download the object)**, and the **expiration date and time**. The pre-signed URLs are valid only for the specified duration.

Anyone who receives the **pre-signed URL** can then access the object. For example, if you have a video in your bucket and both the bucket and the object are **private**, you can share the video with others by generating a pre-signed URL.

### ✅ **Correct Answer**

🟢 **Configure your S3 bucket to remove public read access and use pre-signed URLs with expiry dates.**

### ❌ **Incorrect Options (with reasons)**

<span style="color:red"><strong>🔴 The option that says: Using Amazon CloudFront distributions for your photos is incorrect.</strong></span>  
CloudFront is primarily a **Content Delivery Network (CDN)** service that speeds up the delivery of content to your customers.

<span style="color:red"><strong>🔴 The option that says: Blocking the IP addresses of the offending websites using NACL is also incorrect.</strong></span>  
Blocking IP addresses using **NACLs** is not a very efficient method because a **quick change in IP address** would easily bypass this configuration.

<span style="color:red"><strong>🔴 The option that says: Storing and privately serving the high-quality photos on Amazon WorkDocs instead is incorrect</strong></span> as **WorkDocs** is simply a fully managed, secure content creation, storage, and collaboration service. It is **not a suitable service for storing static content**. Amazon WorkDocs is more often used to easily create, edit, and share documents for collaboration and **not for serving object data like Amazon S3**.

> ⚠️ Note (2026): Amazon WorkDocs reached end of support on Apr 25, 2025, so it is no longer a real option.

</details>

---

## **Question 3**

A company has clients all across the globe that access product files stored in several Amazon S3 buckets, which are behind multiple Amazon CloudFront web distributions. The company wants to deliver content **only to a specific client**, ensuring the data can only be accessed through CloudFront.

Currently, all clients can directly access the S3 buckets via S3 URLs or CloudFront URLs. The Solutions Architect must ensure that **private content is served only through CloudFront** and access is restricted to authorized clients.

**Which combination of actions should the Architect implement to meet the requirement? (Select TWO.)**

### **Options:**

1. Enable the Origin Shield feature of the CloudFront distribution to protect the files from unauthorized access.
2. Restrict access to files in the origin by creating an Origin Access Control (OAC) and giving it permission to read the files in the bucket.
3. Create a custom CloudFront function to check and ensure that only their clients can access the files.
4. Require the users to access the private content by using special CloudFront signed URLs or signed cookies.
5. Use S3 pre-signed URLs to ensure that only their client can access the files. Remove permission to use S3 URLs to read the files for anyone else.

<details>
<summary><strong>Answer & Explanation</strong> 📝</summary>

Many companies that distribute content over the Internet want to restrict access to documents, business data, media streams, or content that is intended for selected users, for example, users who have paid a fee. To securely serve this private content by using **CloudFront**, you can do the following:

✅ **– Require that your users access your private content by using special CloudFront signed URLs or signed cookies.**

✅ **– Require that your users access your Amazon S3 content by using CloudFront URLs, not Amazon S3 URLs.**
Requiring CloudFront URLs isn't necessary, but it is **recommended** to prevent users from bypassing the restrictions that you specify in signed URLs or signed cookies. You can do this by setting up an **Origin Access Control (OAC)** for your Amazon S3 bucket. You can also configure the custom headers for a private HTTP server or an Amazon S3 bucket configured as a website endpoint.

All objects and buckets, by default, are **private**. The **pre-signed URLs** are useful if you want your user/customer to be able to upload a specific object to your bucket, but you don't require them to have AWS security credentials or permissions.

You can generate a **pre-signed URL** programmatically using the **AWS SDK for Java** or the **AWS SDK for .NET**. If you are using Microsoft Visual Studio, you can also use **AWS Explorer** to generate a pre-signed object URL without writing any code. Anyone who receives a valid pre-signed URL can then programmatically upload an object.

### ✅ **Correct Answers**

🟢 **– Restrict access to files in the origin by creating an Origin Access Control (OAC) and giving it permission to read the files in the bucket.**

🟢 **– Require the users to access the private content by using special CloudFront signed URLs or signed cookies.**

### ❌ **Incorrect Options (with reasons)**

<span style="color:red"><strong>🔴 The option that says: Create a custom CloudFront function to check and ensure that only their clients can access the files is incorrect.</strong></span>  
CloudFront Functions are just lightweight functions in JavaScript for high-scale, latency-sensitive CDN customizations and **not for enforcing security**. A CloudFront Function runtime environment offers submillisecond startup times which allows your application to scale immediately to handle millions of requests per second. But again, this can't be used to restrict access to your files.

<span style="color:red"><strong>🔴 The option that says: Enable the Origin Shield feature of the CloudFront distribution to protect the files from unauthorized access is incorrect</strong></span> because this feature is not primarily used for security but for **improving your origin's load times, improving origin availability, and reducing your overall operating costs** in CloudFront.

<span style="color:red"><strong>🔴 The option that says: Use S3 pre-signed URLs to ensure that only their client can access the files. Remove permission to use S3 URLs to read the files for anyone else is incorrect.</strong></span>  
Although this could be a valid solution, it doesn't satisfy the requirement to **serve the private content via CloudFront only** to secure the distribution of files. A better solution is to set up an **Origin Access Control (OAC)** and then use **Signed URL or Signed Cookies** in your CloudFront web distribution.

### ✅ **Summary**

* 🟢 **Option 2: Create an Origin Access Control (OAC) and give CloudFront permission to read the S3 files**
* 🟢 **Option 4: Require users to access content via CloudFront Signed URLs or Signed Cookies**

</details>

---

## **Question 4**

A solutions architect is instructed to host a website consisting of HTML, CSS, and JavaScript files. The web pages will display several high-resolution images. The website should have **optimal loading times** and be able to respond to **high request rates**.

Which of the following architectures can provide the **most cost-effective and fastest loading experience**?

### **Options:**

1. Upload the HTML, CSS, JavaScript, and the images in a single S3 bucket. Enable static website hosting, create a CloudFront distribution, and point the domain to the S3 website endpoint.
2. Host the website in an AWS Elastic Beanstalk environment. Upload the images to S3 and use CloudFront as a CDN.
3. Launch an Auto Scaling Group of EC2 servers with Apache, store images in EBS, and use AWS Global Accelerator.
4. Host the website using an Nginx server on EC2, store images in S3, and use CloudFront as a CDN.

<details>
<summary><strong>Answer & Explanation</strong> 📝</summary>

**Amazon S3** is an object storage service that offers industry-leading scalability, data availability, security, and performance. Additionally, you can use **Amazon S3 to host a static website**. On a static website, individual webpages include static content. **Amazon S3 is highly scalable and you only pay for what you use**, you can start small and grow your application as you wish, with no compromise on performance or reliability.

**Amazon CloudFront** is a fast **Content Delivery Network (CDN)** service that securely delivers data, videos, applications, and APIs to customers globally with **low latency, high transfer speeds**. CloudFront can be integrated with **Amazon S3** for fast delivery of data originating from an S3 bucket to your end-users. By design, delivering data out of CloudFront can be **more cost-effective** than delivering it from S3 directly to your users.

In the scenario, since we are only dealing with **static content**, we can leverage the **web hosting feature of S3**. Then we can improve the architecture further by integrating it with **CloudFront**. This way, users will be able to load both the web pages and images **faster** than if we hosted them on a webserver that we built from scratch.

### ✅ **Correct Answer**

🟢 **Upload the HTML, CSS, Javascript, and the images in a single bucket. Then enable website hosting. Create a CloudFront distribution and point the domain on the S3 website endpoint.**

### ❌ **Incorrect Options (with reasons)**

🔴 **The option that says: Host the website using an Nginx server in an EC2 instance. Upload the images in an S3 bucket. Use CloudFront as a CDN to deliver the images closer to end-users is incorrect.**
Creating your own web server to host a static website in AWS is a **costly solution**. Web Servers on an EC2 instance are usually used for hosting applications that require **server-side processing** (connecting to a database, data validation, etc.). Since static websites contain web pages with fixed content, **we should use S3 website hosting instead**.

🔴 **The option that says: Launch an Auto Scaling Group using an AMI that has a pre-configured Apache web server, then configure the scaling policy accordingly. Store the images in an Elastic Block Store. Then, point your instance's endpoint to AWS Global Accelerator is incorrect.**
This is how we served static websites in the **old days**. Now, with the help of **S3 website hosting**, we can host our static contents from a **durable, high-availability, and highly scalable environment** without managing any servers. Hosting static websites in S3 is **cheaper** than hosting it in an EC2 instance. In addition, using **ASG for scaling instances that host a static website is an over-engineered solution** that carries unnecessary costs. S3 automatically scales to high requests and you only pay for what you use.

🔴 **The option that says: Host the website in an AWS Elastic Beanstalk environment. Upload the images in an S3 bucket. Use CloudFront as a CDN to deliver the images closer to your end-users is incorrect.**
**AWS Elastic Beanstalk** simply sets up the infrastructure (EC2 instance, load balancer, auto-scaling group) for your application. It's a **more expensive and an overkill solution** for hosting a bunch of client-side files.

</details>

---

## **Question 5**

A digital media company shares static content with its premium users worldwide and to partners who syndicate their media files. The company wants to reduce server costs and securely deliver the content globally with low latency.

Which combination of services should be used to provide the **MOST suitable and cost-effective architecture?** (Select TWO.)

### **Options:**

1. AWS Lambda
2. AWS Global Accelerator
3. Amazon CloudFront
4. AWS Fargate
5. Amazon S3

<details>
<summary><strong>Answer & Explanation</strong> 📝</summary>

**Amazon CloudFront** is a fast **Content Delivery Network (CDN)** service that securely delivers data, videos, applications, and APIs to customers globally with **low latency, high transfer speeds**, all within a developer-friendly environment.

CloudFront is integrated with **AWS** – both physical locations that are directly connected to the AWS global infrastructure, as well as other AWS services. CloudFront works seamlessly with services, including **AWS Shield** for DDoS mitigation, **Amazon S3**, **Elastic Load Balancing** or **Amazon EC2** as origins for your applications, and **Lambda@Edge** to run custom code closer to customers' users and to customize the user experience. Lastly, if you use AWS origins such as **Amazon S3, Amazon EC2 or Elastic Load Balancing**, **you don't pay for any data transferred between these services and CloudFront.**

**Amazon S3** is object storage built to **store and retrieve any amount of data from anywhere on the Internet**. It's a simple storage service that offers an **extremely durable, highly available, and infinitely scalable** data storage infrastructure at **very low costs**.

**AWS Global Accelerator** and **Amazon CloudFront** are separate services that use the **AWS global network** and its **edge locations** around the world.
CloudFront improves performance for both **cacheable content** (such as images and videos) and **dynamic content** (such as API acceleration and dynamic site delivery).
Global Accelerator improves performance for a wide range of applications over **TCP or UDP** by proxying packets at the edge to applications running in one or more AWS Regions. Global Accelerator is a good fit for **non-HTTP use cases**, such as gaming (UDP), IoT (MQTT), or Voice over IP, as well as for HTTP use cases that specifically require **static IP addresses** or **deterministic, fast regional failover**. Both services integrate with **AWS Shield** for DDoS protection.

### ✅ **Correct Answers**

🟢 **Amazon CloudFront**
🟢 **Amazon S3**

### ❌ **Incorrect Options (with reasons)**

<span style="color:red"><strong>🔴 AWS Fargate is incorrect</strong></span> because this service is just a **serverless compute engine for containers** that works with both Amazon Elastic Container Service (ECS) and Amazon Elastic Kubernetes Service (EKS). Although this service is more cost-effective than its server-based counterpart, **Amazon S3 still costs way less than Fargate**, especially for storing static content.

<span style="color:red"><strong>🔴 AWS Lambda is incorrect</strong></span> because this simply lets you **run your code serverless** without provisioning or managing servers. Although this is also a cost-effective service since you pay only for the compute time you consume, **you can't use this to store static content or as a CDN**. A better combination is **Amazon CloudFront and Amazon S3**.

<span style="color:red"><strong>🔴 AWS Global Accelerator is incorrect</strong></span> because this service is more suitable for **non-HTTP use cases**, such as gaming (UDP), IoT (MQTT), or Voice over IP, as well as for HTTP use cases that specifically require **static IP addresses** or deterministic, fast regional failover. Moreover, there is **no direct way to integrate AWS Global Accelerator with Amazon S3**. It's more suitable to use **Amazon CloudFront** instead in this scenario.

### ✅ **Summary**

**Amazon S3** and **Amazon CloudFront**

</details>

---

## **Question 6**

A company's web app runs on an Auto Scaling group behind an **Application Load Balancer (ALB)**. To strengthen security and **minimize DDoS impact**, which solution is most effective?

### Options

1. Configure CloudFront with **Network** Load Balancer origin; use **VPC Flow Logs** + Lambda + SNS.
2. Configure CloudFront with the ALB as origin; create an AWS WAF *rate-based* web ACL and associate it with CloudFront.
3. Configure CloudFront with ALB; create a security group rule to deny suspicious addresses; use SNS.
4. Configure CloudFront with **Network** Load Balancer origin; use **GuardDuty** + Lambda + SNS.

<details>
<summary><strong>Answer & Explanation</strong> 📝</summary>

**AWS WAF** is a **web application firewall** that helps protect your web applications or APIs against common web exploits that may affect availability, compromise security, or consume excessive resources. **AWS WAF gives you control** over how traffic reaches your applications by enabling you to create security rules that block common attack patterns, such as **SQL injection** or **cross-site scripting (XSS)**, and rules that filter out specific traffic patterns you define. You can deploy AWS WAF on **Amazon CloudFront** as part of your CDN solution, the **Application Load Balancer** that fronts your web servers or origin servers running on EC2, or **Amazon API Gateway** for your APIs.

To detect and mitigate **DDoS attacks**, you can use **AWS WAF** in addition to **AWS Shield**. AWS WAF is a web application firewall that helps detect and mitigate **web application layer DDoS attacks** by inspecting traffic inline. **Application layer DDoS attacks** use well-formed but malicious requests to evade mitigation and consume application resources. You can define custom security rules that contain a set of conditions, rules, and actions to **block attacking traffic**. After you define **web ACLs**, you can apply them to CloudFront distributions, and web ACLs are evaluated in the priority order you specified when you configured them.

By using **AWS WAF**, you can configure **web access control lists (Web ACLs)** on your CloudFront distributions or Application Load Balancers to filter and block requests based on request signatures. Each Web ACL consists of rules that you can configure to **string match or regex match** one or more request attributes, such as the **URI**, **query-string**, **HTTP method**, or **header key**. In addition, by using **AWS WAF's rate-based rules**, you can automatically **block the IP addresses of bad actors** when requests matching a rule **exceed a threshold** that you define. Requests from offending client IP addresses will receive **403 Forbidden** error responses and will remain blocked until request rates drop below the threshold. This is useful for mitigating **HTTP flood attacks disguised as regular web traffic**.

It is recommended that you add **web ACLs with rate-based rules** as part of your **AWS Shield Advanced** protection. These rules can alert you to sudden spikes in traffic that might indicate a potential DDoS event. A **rate-based rule** counts the requests that arrive from any individual address in a time window you choose: 1, 2, 5 or 10 minutes (it used to be fixed at five minutes). If the number of requests exceeds the limit that you define, the rule can trigger an action such as sending you a notification.

### ✅ **Correct Answer**

🟢 **Configure Amazon CloudFront distribution and set Application Load Balancer as the origin. Create a rate-based web ACL rule using AWS WAF and associate it with Amazon CloudFront.**

### ❌ **Incorrect Options (with reasons)**

🔴 **The option that says: Configure Amazon CloudFront distribution and set a Network Load Balancer as the origin. Use VPC Flow Logs to monitor abnormal traffic patterns. Set up a custom AWS Lambda function that processes the flow logs and invokes Amazon SNS for notification is incorrect** because this option only allows you to **monitor the traffic** that is reaching your instance. You **can't use VPC Flow Logs to mitigate DDoS attacks**.

🔴 **The option that says: Configure Amazon CloudFront distribution and set an Application Load Balancer as the origin. Create a security group rule and deny all the suspicious addresses. Use Amazon SNS for notification is incorrect.**
To deny suspicious addresses, you must **manually insert the IP addresses** of these hosts. This is a manual task which is **not a sustainable solution**. Take note that attackers generate large volumes of packets or requests to overwhelm the target system. Using a **security group** in this scenario **won't help you mitigate DDoS attacks**.

🔴 **The option that says: Configure Amazon CloudFront distribution and set a Network Load Balancer as the origin. Use Amazon GuardDuty to block suspicious hosts based on its security findings. Set up a custom AWS Lambda function that processes the security logs and invokes Amazon SNS for notification is incorrect** because **Amazon GuardDuty is just a threat detection service**. You should use **AWS WAF and create your own AWS WAF rate-based rules** for mitigating HTTP flood attacks that are disguised as regular web traffic.

</details>

---
