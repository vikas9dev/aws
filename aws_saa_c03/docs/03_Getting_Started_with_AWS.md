# Getting Started with AWS

Sections:-
- [1. History and Global Infrastructure of AWS](#1-history-and-global-infrastructure-of-aws)
- [2. Exploring the AWS Console Home 🏠](#2-exploring-the-aws-console-home-)

---

## 1. History and Global Infrastructure of AWS

Amazon Web Services (AWS) started as an internal project at Amazon.com in 2002. The company realized its IT infrastructure could be externalized and offered as a service.

![AWS History](./img/AWS_History.png)

- The first public offering was Simple Queue Service (SQS) in 2004.
- In 2006, AWS expanded its offerings with SQS, Simple Storage Service (S3), and Elastic Compute Cloud (EC2). These are core services we'll explore in this course.
- AWS expanded globally beyond the US, starting with Europe.

Today, many prominent applications run on AWS, including:

- Dropbox
- Netflix
- Airbnb
- NASA

AWS is a leader in the cloud market.

- Gartner's Magic Quadrant consistently recognizes AWS as a leader.
- As of 2023, AWS has \$90 billion in revenue.
- In Q1 2024, AWS held approximately 31% of the market share, followed by Microsoft at 25%.
- AWS has been a market pioneer and leader for 13 consecutive years.
- AWS has over 1 million active users.

Learning AWS sets you up for success in the cloud computing world.

### What Can You Build on AWS? 🏗️

Pretty much anything! AWS enables the creation of sophisticated and scalable applications applicable to diverse industries. Every company can find a use case for the cloud.

Companies using AWS include:

- Netflix
- McDonald's
- 21st Century Fox
- Activision

Use cases for AWS include:

1.  Enterprise IT migration
2.  Backup and storage
3.  Big data analytics
4.  Website hosting
5.  Backend for mobile and social applications
6.  Gaming server infrastructure

The applications are endless.

### AWS Global Infrastructure 🌍

AWS is a global service with a specific infrastructure. Key components include:

- AWS Regions
- Availability Zones
- Data Centers
- Edge Locations
- Points of Presence

See the map here: [AWS Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)

AWS has multiple regions around the world, such as Paris, Spain, Ohio, Sao Paulo, Cape Town, and Mumbai. These regions are interconnected through a private AWS network.

Within each region, there are multiple availability zones.

AWS allows you to leverage its global infrastructure to make your applications global.

### Regions 📍

Regions are located around the world. Each region has a name, such as `us-east-1` or `eu-west-3`. You can find the mapping of region names to their codes in the AWS console.

![AWS Regions](./img/AWS_Regions.webp)

A region is a cluster of data centers located near each other, for example, in Ohio, Singapore, Sydney, or Tokyo.

Most AWS services are region-scoped. Using a service in one region is independent of using it in another.

### Choosing an AWS Region 🤔

When launching a new application, how do you choose an AWS region? The answer depends on several factors:

1.  **Compliance:** 🏛️ Some governments require data to reside within the country where the application is deployed. 📌 **Example:** Data in France may need to stay in France, requiring deployment in the French region.
2.  **Latency:** ⏱️ Deploy your application close to your users to reduce latency. If most users are in America, deploy in an American region.
3.  **Service Availability:** ✅ Not all regions offer all AWS services. Ensure the region you choose has the services your application requires.
4.  **Pricing:** 💰 Pricing varies from region to region. Consult the service pricing pages to compare costs.

### Availability Zones (AZs) 🏘️

Availability Zones reside within regions. Each region has multiple AZs: a **minimum of three**, a maximum of six, usually three.

📌 **Example:** The Sydney region (`ap-southeast-2`) has three availability zones: `ap-southeast-2a`, `ap-southeast-2b`, and `ap-southeast-2c`.

- **US East (N. Virginia)**: 6 AZs
- **Asia Pacific (Mumbai)**: 3 AZs
- **Europe (Paris)**: 3 AZs
- **Africa (Cape Town)**: 3 AZs
- **New regions** launch with **at least 3 AZs** (AWS: "Each AWS Region consists of a minimum of three ... AZs").

![AWS AZs](./img/AWS_availability_zones.webp)

You can view the current number of AZs per region on the [official AWS Regional Services List](https://aws.amazon.com/about-aws/global-infrastructure/regions_az/).

Each AZ consists of one or more discrete data centers with redundant power, networking, and connectivity. An AZ might contain multiple data centers.

AZs are isolated from each other to prevent cascading failures. If something happens in `ap-southeast-2a`, it is designed not to affect `ap-southeast-2b` or `ap-southeast-2c`.

AZs are connected with high-bandwidth, ultra-low-latency networking, forming a region.

- AWS builds **a minimum of three AZs** in every region.
- However, **us-west-1** is the only region where **new accounts** are limited to **two AZs**, despite having **three** mapped internally.
- In contrast, all other regions (e.g., us-east-1, us-west-2, ap-south-1, eu-west-1, etc.) provide **a minimum of three AZs** to all accounts .

### Points of Presence (Edge Locations) 🌐

AWS has **750+ points of presence** in 100+ cities across 50+ countries, plus **15 regional edge caches** (older course material says 400+ PoPs in 90 cities). These are used to deliver content to end-users with the lowest possible latency. We'll cover this in more detail later in the course.

### AWS Console Tour 🖥️

AWS has global services, such as:

- Identity and Access Management (IAM)
- Route 53
- CloudFront
- Web Application Firewall (WAF)

Most AWS services are region-scoped, such as:

- EC2
- Elastic Beanstalk
- Lambda
- Rekognition

To check if a service is available in a specific region, see [AWS Services by Region](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/).

---

## 2. Exploring the AWS Console Home 🏠

Welcome to the AWS Console Home! This section will guide you through the main elements of the console.

First, let's configure your region:

- Locate the **region selector** in the top right corner of your screen.
- By default, it might be set to Northern Virginia (US East 1).
- It's recommended to choose a region geographically close to you for lower latency.
  - 📌 **Example:** If you're in Europe, select Ireland (EU West 1). If you're in Africa, consider Cape Town.
- You don't need to be physically located in the region to use it. Choose the region that makes the most sense for your needs.

The console also displays:

- A list of recently visited services (this might be empty initially).
- Information about AWS, including:
  - Health issues
  - Cost and usage information for your account
  - Tutorials for building solutions

### Navigating AWS Services 🧭

There are two primary ways to find AWS services:

1.  **Using the "Services" Menu:**
    - Click on "Services" in the top left corner.
    - You can browse services alphabetically or by category (e.g., Compute, Database).
    - Don't worry about knowing all the services initially; we'll cover them throughout the course.
2.  **Using the Search Bar:**
    - Type the name of the service you're looking for in the search bar.
      - 📌 **Example:** Typing "Route 53" will display matching services.
    - The search results also include features, blogs, knowledge articles, and documentation related to the service.

### Understanding Global vs. Regional Services 🌍

Let's explore Route 53 as an example:

- Navigate to Route 53 using the search bar.
- Notice that the top right corner says "Global."
- This indicates that Route 53 is a **global service**.
- Global services don't require region selection, and you'll see the same view regardless of your location.

Now, let's look at EC2:

- Navigate to the EC2 service.
- The top right corner displays the selected region (e.g., Ireland).
- EC2 is a **regional service**.
- The resources you see will vary depending on the selected region.

⚠️ **Warning:** It's crucial to remain in the same region throughout this course to avoid confusion and ensure consistency.

### AWS Global Infrastructure and Regional Service Availability 🌐

- Search for "AWS global infrastructure" on Google to find detailed information about AWS services and regions.
- Pay close attention to the **[AWS regional services](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)** list.
- This table shows the availability of services in each region.
- If a service discussed in the course isn't available in your current region, consult this list.
- You may need to switch to a different region to access the service.

📌 **Example:** If you're in Cape Town and a service isn't available, check the regional services list and consider switching to a region where it is available.

💡 **Tip:** Not all AWS services are available in every region. Always check the regional service availability list if you encounter issues.

---
