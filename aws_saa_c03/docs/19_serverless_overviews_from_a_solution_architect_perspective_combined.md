# Serverless Overviews From A Solution Architect Perspective

Sections:-
- [1. About the Serverless Section](#1-about-the-serverless-section)
- [2. What is Serverless?](#2-what-is-serverless)
- [3. What is AWS Lambda and Why is it Helpful?](#3-what-is-aws-lambda-and-why-is-it-helpful)
- [4. Lambda Practice](#4-lambda-practice)
- [5. Lambda Limits You Need to Know 🚀](#5-lambda-limits-you-need-to-know-🚀)
- [6. Lambda Concurrency and Throttling](#6-lambda-concurrency-and-throttling)
- [7. Concurrency Settings in Lambda](#7-concurrency-settings-in-lambda)
- [8. Lambda SnapStart](#8-lambda-snapstart)
- [9. Customization At The Edge](#9-customization-at-the-edge)
- [10. Networking Fundamentals for Lambda](#10-networking-fundamentals-for-lambda)
- [11. Tight Integration Between RDS, Aurora, and Lambda](#11-tight-integration-between-rds-aurora-and-lambda)
- [12. Amazon DynamoDB: A Deep Dive](#12-amazon-dynamodb-a-deep-dive)
- [13. DynamoDB First Look](#13-dynamodb-first-look)
- [14. Advanced DynamoDB Features](#14-advanced-dynamodb-features)
- [15. API Gateway Overview](#15-api-gateway-overview)
- [16. API Gateway Basics Hands On](#16-api-gateway-basics-hands-on)
- [17. AWS Step Functions: Serverless Visual Workflows](#17-aws-step-functions-serverless-visual-workflows)
- [18. Amazon Cognito](#18-amazon-cognito)
- [19. Q & A](#19-q--a)

---

## 1. About the Serverless Section

This section contains short introductions to the Serverless services in AWS.

These lectures are extracted from the AWS Certified Developer course and are meant to give you an overview of how Lambda, API Gateway, DynamoDB, and Cognito work.

The SA exam is pretty lightweight on the details on Serverless services, so these lectures are enough from an exam perspective.

If you'd like to learn more about Serverless, there are over 8 hours of content on it in my AWS Certified Developer course.

---

## 2. What is Serverless?

Serverless is a relatively new paradigm **where developers don't have to manage servers**. 😲

**It doesn't mean servers are gone; it simply means you don't provision or maintain them. You just deploy your code.**

Initially, serverless equated to **Function as a Service (FaaS)**. However, its scope has broadened to encompass various remotely managed services.

In essence, serverless means you don't see or provision the servers.

Serverless was pioneered by **AWS Lambda**, but now includes anything remotely managed such as:

*   Databases
*   Messaging
*   Storage

...as long as you don't provision servers.

📌 **Example:** Serverless Architecture in AWS

Consider a typical serverless application architecture in AWS:

1.  Users access static content from **S3** buckets via a website or **CloudFront**.
2.  Users log in using **Cognito**, which handles identity management.
3.  The application exposes a **REST API** through **API Gateway**.
4.  **API Gateway** invokes **Lambda** functions.
5.  **Lambda** functions store and retrieve data from **DynamoDB**.

This is a reference architecture for serverless applications.

AWS Serverless Services include:

*   Lambda
*   DynamoDB
*   Cognito
*   API Gateway
*   Amazon S3
*   SNS
*   SQS
*   Kinesis Data Firehose
*   Aurora Serverless
*   Step Functions
*   Fargate

We've already seen **SNS** and **SQS** which scale automatically without server management, fitting the serverless model.

**Kinesis Data Firehose** also fits because it scales based on throughput, with you only paying for what you use, and without provisioning servers.

**Aurora Serverless** scales your database on demand without server management.

**Fargate** is a serverless option for ECS, where you don't provision infrastructure to run Docker containers.

Hopefully, this provides a concise introduction to serverless. The next section will focus on AWS Lambda.

The exam tests heavily on serverless knowledge, so let's get started! 🚀

---

## 3. What is AWS Lambda and Why is it Helpful?

Let's explore AWS Lambda and its benefits. We'll start by comparing it to Amazon EC2.

### EC2 vs. Lambda

*   **Amazon EC2:**
    *   Virtual servers in the cloud.
    *   Requires provisioning, limiting resources (memory, CPU).
    *   Continuously running, even when idle.
    *   Scaling requires Auto Scaling Groups (manual addition/removal of servers).

*   **AWS Lambda:**
    *   **Virtual functions**, no servers to manage. 🎉
    *   Code is provisioned and functions run on demand.
    *   Limited execution time (up to 15 minutes) - **short execution**.
    *   **Runs on-demand** - billed only when the function is running. 💰
    *   **Automated scaling**. AWS provisions more Lambda functions as needed. 🚀

### Benefits of Lambda

*   **Pricing:** Simple and cost-effective.
    *   Pay for the number of requests (invocations) and compute time.
    *   Generous free tier: 1 million Lambda requests and 400,000 GB-seconds of compute time.
*   **Integration:** Integrated with many AWS services.
*   **Languages:** Supports various programming languages.
*   **Monitoring:** Easy monitoring integrations through CloudWatch. 📊
*   **Resources:** Provision up to 10 GB of RAM per function.

💡 **Tip:** Increasing the RAM of your function also improves CPU and network performance.

### Supported Languages and Runtimes

AWS Lambda supports multiple languages:

*   Node.js (JavaScript)
*   Python
*   Java
*   C# (.NET Core or PowerShell)
*   Ruby
*   Custom Runtime API (e.g., Rust, Golang)

You can also use containers on Lambda, implementing the Lambda Runtime API.

⚠️ **Warning:** **For running container images, ECS or Fargate are generally preferred over Lambda from an exam perspective**.

📝 **Note:** While Lambda supports various languages, Node.js and Python are the most commonly used.

### Lambda Integrations

Lambda integrates with numerous AWS services. Here are a few examples:

![AWS Lambda Integrations](./img/AWS_Lambda_Integrations.png)

*   **API Gateway:** Create REST APIs that invoke Lambda functions.
*   **Kinesis:** Perform data transformations on the fly.
*   **DynamoDB:** Trigger Lambda functions based on database events.
*   **S3:** Trigger Lambda functions when files are created in S3.
*   **CloudFront:** Lambda@Edge (dedicated lecture in this section).
*   **CloudWatch Events/EventBridge:** React to infrastructure events and automate tasks.
*   **CloudWatch Logs:** Stream logs to various destinations.
*   **SNS:** React to notifications in SNS topics.
*   **SQS:** Process messages from SQS queues.
*   **Cognito:** React to user login events.

### Use Cases

Here are a couple of common use cases for AWS Lambda:

*   📌 **Example-1:** **Serverless Thumbnail Creation**
    1.  New image uploaded to S3.
    2.  S3 event notification triggers a Lambda function.
    3.  Lambda function generates a thumbnail.
    4.  Thumbnail uploaded to another (or the same) S3 bucket.
    5.  Lambda function inserts metadata into DynamoDB (image name, size, creation date, etc.).

![AWS Lambda Use Cases - Serverless Thumbnail Creation](./img/aws_lambda_use_cases-serverless_thumbnail_creation.png)

*   📌 **Example-2:** **Serverless CRON Job**
    1.  CloudWatch Event/EventBridge rule triggered every hour.
    2.  The rule invokes a Lambda function.
    3.  Lambda function performs the desired task.

### Lambda Pricing

![AWS Lambda Pricing](./img/aws_lambda_pricing.png)

*   Pay per call: First 1 million requests are free, then $0.20 per 1 million requests.
*   Pay per duration: First 400,000 GB-seconds of compute time per month are free, then \$1.00 per 600,000 GB-seconds.

    *   GB-seconds: 400,000 seconds of execution with 1 GB of RAM.
    *   A function with 128 MB of RAM gets 8x more execution seconds.

💡 **Tip:** Lambda pricing is generally very cost-effective.

You can find the most up-to-date pricing information on the [AWS website](https://aws.amazon.com/lambda/pricing/).

```text
# Example: Lambda Pricing Calculation
# (Illustrative, refer to AWS documentation for accurate details)

free_requests = 1000000
price_per_million_requests = 0.20

free_gb_seconds = 400000
price_per_600k_gb_seconds = 1.00
```

---

## 4. Lambda Practice

Let's dive into practicing with AWS Lambda! 🚀

If you're in the Lambda console and see a different screen, navigate to `/begin` in the URL. This UI provides a helpful diagram to visualize how Lambda functions operate.

Here's a breakdown of Lambda's core concepts:

*   Lambda functions can be written in various languages, including:
    *   .NET
    *   Java
    *   Node.js
    *   Python
    *   Ruby
    *   Custom runtimes for other languages

```python
def lambda_handler(event, context):
    print(event)
    return 'Hello from Lambda!'
```

```java
package example;

import com.amazonaws.services.lambda.runtime.Context;
import com.amazonaws.services.lambda.runtime.RequestHandler;

public class Hello implements RequestHandler<Object, String> {
    public String handleRequest(final Object input, final Context context) {
        System.out.println(input);
        return "Hello from Lambda!";
    }
}
```

> You can also develop Lambda functions locally using VS Code Extension:  
> - [AWS Toolkit](https://marketplace.visualstudio.com/items?itemName=amazonwebservices.aws-toolkit-vscode), which provides a local environment for testing and debugging Lambda functions.  
> - AWS boto3  
> - Amazon Q - For Agentic coding experience.

Let's explore some key features:

1.  **Running a Lambda Function:**
    *   Select Python and click "Run."
    *   You'll see the output: "Hello from Lambda." 👋

2.  **Lambda Responds to Events:**
    *   Lambda functions can be triggered by various event sources.
    *   📌 **Example:** Streaming analytics sending data to your Lambda function.
    *   As more events trigger the function, Lambda scales up seamlessly. ⬆️
    *   This provides scalability without server management.

3.  **Event Sources:**
    *   Streaming analytics
    *   Mobile phones sending data to a mobile/IoT backend
    *   Photos being uploaded to an S3 bucket

4.  **Cost Considerations:**
    *   Lambda offers a generous free tier.
    *   After exceeding the free tier, invocations increase and costs accumulate.
    *   It's crucial to estimate your workload to understand potential costs. 💰

5.  **Creating a Function:**
    *   Use a blueprint (e.g., "hello world").
    *   Select Python as the runtime.
    *   Name the function (e.g., "HelloWorld").

6.  **Execution Role:**
    *   Lambda functions require an execution role, similar to roles on EC2 instances.
    *   Select -> Create a new role with basic Lambda permissions.

7.  **Function Code:**
    *   The code is automatically generated.
    *   The handler is invoked when an event is passed.

    ```python
    import json

    print('Loading function')

    def lambda_handler(event, context):
        #print("Received event: " + json.dumps(event, indent=2))
        print("value1 = " + event['key1'])
        print("value2 = " + event['key2'])
        print("value3 = " + event['key3'])
        return event['key1']  # Echo back the first key value
        #raise Exception('Something went wrong')
    ```

    *   The code gets executed whenever the Lambda function is triggered.
    * Create the function.

8.  **Testing the Function:**
    *   Click "Test" to execute the function.
    *   The input JSON is passed to the Lambda function.
    *   The function returns a result.

9.  **Triggering a Failure (Example):**
    *   ⚠️ **Warning:** Removing a key from the input JSON can cause the function to fail if the code doesn't handle the exception.
    *   This helps in understanding error handling.

10. **Saving Test Events:**
    *   Save your test event for repeated testing (like with name "HelloWorldEvent").

11. **Monitoring:**
    *   Monitor invocations from CloudWatch.
    *   View statistics and logs.
    *   Click "View CloudWatch Logs" to access function logs.

12. **Debugging:**
    *   Debug functions directly from CloudWatch logs. 🐞

13. **Configuration:**
    *   General configuration: memory, ephemeral storage, timeout, execution role.
    *   The execution role allows access to CloudWatch.

14. **Permissions:**
    *   The role summary shows allowed actions (e.g., CloudWatch Logs).
    *   You can modify the IAM role to add permissions for interacting with other services like Amazon S3.

15. **Triggers:**
    *   Add triggers to define event sources that trigger the Lambda function.
    *   📌 **Example:** Amazon S3 bucket events.

    *   There are many AWS and partner event sources available.

📝 **Note:** This overview provides a good introduction to Lambda. The service is very complete and offers many options to explore.

---

## 5. Lambda Limits You Need to Know 🚀

Before heading into the exam, it's crucial to understand the limitations of AWS Lambda. These limits are region-specific and fall into two main categories: execution and deployment.

### Execution Limits ⚙️

These limits govern the resources available to your Lambda function during runtime.

*   **Memory Allocation:** 128 MB to 10 GB (10,240 MB) 🧠, in **1 MB** increments (the step was 64 MB before Dec 2020 — older exam questions may still say 64 MB). Increasing memory also increases vCPU allocation.
*   **Maximum Execution Time:** 900 seconds (15 minutes) ⏱️.
    *   Anything exceeding this is not suitable for Lambda.
*   **Environment Variables:** Maximum of 4 KB 🔑.
*   **/tmp Space:** 512 MB to 10 GB 💾. This is useful for temporarily storing large files during function execution.
    *   📌 **Example:** Downloading a large dataset for processing.
*   **Concurrent Executions:** 1000 🚦. This limit can be increased by request.
    *   💡 **Tip:** Utilize reserved concurrency early on for better control.

For **AWS Lambda**, the **memory allocation** is set in **1 MB increments** (it was 64 MB increments before Dec 2020). ✅ So you can pick any value from `128 MB` up to **10,240 MB (10 GB)**, for example `129 MB` or `130 MB`. 💡 When you increase memory, Lambda also proportionally increases the amount of vCPU available. For example, giving more memory means your function may run faster because it gets more CPU.

### Deployment Limits 📦

These limits apply to the size of your Lambda function's deployment package.

*   Lambda function deployment size **Compressed (.zip) Size:** 50 MB 🗜️.
*   Size of **uncompressed deployment** (code + dependencies): 250 MB 📂.
    *   ⚠️ **Warning:** For large files exceeding these limits, use the `/tmp` space instead.
*   **Environment Variables:** Maximum of 4 KB 🔑 (same as execution limit).

### Key Takeaways 📝

Understanding these limits is essential for choosing the right AWS service for your workload.

*   📌 **Example:** If a scenario requires 30 GB of RAM or 30 minutes of execution time, Lambda is not the appropriate choice.
*   📌 **Example:** If you need to process a 3 GB file, don't include it in the deployment package. Instead, download it to the `/tmp` directory during execution.

By keeping these limits in mind, you'll be well-prepared to answer exam questions and design efficient serverless applications.

---

## 6. Lambda Concurrency and Throttling

Understanding Lambda concurrency and throttling is crucial for building scalable and reliable serverless applications. Let's explore how these concepts work and how to manage them effectively.

### Concurrency and Scaling

As you invoke your Lambda functions more frequently, the number of concurrent executions increases. Lambda's ability to scale rapidly means that:

*   Low invocation rates might result in only a few concurrent executions.
*   High invocation rates can lead to up to 1000 concurrent executions (or more, if requested).

### Reserved Concurrency and Throttling

To manage concurrency, you can set a **reserved concurrency** limit at the function level. This limit restricts the maximum number of concurrent executions for a specific Lambda function.

*   When invocations exceed the reserved concurrency limit, **throttling** occurs.
*   Each invocation over the concurrency limits will trigger what's called a throttle.

The behavior of throttling depends on the invocation type:

*   **Synchronous Invocations:** If a synchronous invocation is throttled, it returns a `429` throttle error.
*   **Asynchronous Invocations:** Asynchronous invocations are automatically retried and, if unsuccessful after retries, are sent to a Dead Letter Queue (DLQ).

📝 **Note:** The default concurrent execution limit is 1000 per AWS account per region. If you require a higher limit, you can request an increase by opening a support ticket.

### The Importance of Careful Concurrency Management ⚠️

![Lambda Concurrency Management](./img/Lambda_concurrency_management.png)

Failing to set reserved concurrency limits can lead to unexpected throttling issues. Consider this scenario:

*   You have multiple applications using Lambda functions:
    *   Application Load Balancer (ALB) connected to Lambda function A.
    *   API Gateway connected to Lambda function B.
    *   SDK/CLI invoking Lambda function C.

*   If Lambda function A experiences a surge in traffic (e.g., due to a promotion), it can consume all available concurrent executions.

*   This can result in Lambda functions B and C being throttled, impacting the users of those applications.

**The key takeaway is that the concurrency limit applies to all Lambda functions within your account.** Therefore, it's essential to carefully manage concurrency to prevent one function from starving others.

### Concurrency and Asynchronous Invocations

![Lambda Concurrency and Asynchronous Invocations](./img/Lambda_concurrency_and_asynchronous_invocations.png)

Let's consider S3 event notifications as an example of asynchronous invocations.

*   Uploading multiple files to an S3 bucket triggers multiple Lambda function invocations.
*   If the Lambda function doesn't have enough available concurrency (i.e., it has reached its limit), additional requests are throttled.

Because these are asynchronous requests, Lambda handles throttling errors and system errors (429 and 500-series errors) by:

1.  Returning the event to an internal event queue.
2.  Retrying the function execution for up to six hours.
3.  Increasing the retry interval exponentially, from one second to a maximum of five minutes.

This retry mechanism allows Lambda functions to eventually find available concurrency and execute successfully.

### Cold Starts and Provisioned Concurrency 🥶

A **cold start** occurs when a new Lambda function instance is created. During a cold start, the code must be loaded, and any initialization code outside the handler must be executed.

*   Large initialization processes (e.g., loading many dependencies, connecting to databases) can significantly increase the latency of the first request served by the new instance.
*   This increased latency can negatively impact user experience.

To mitigate cold starts, you can use **provisioned concurrency**. This involves pre-allocating concurrency before the function is invoked.

*   By allocating concurrency in advance, you ensure that instances are already initialized and ready to handle requests, eliminating cold starts.
*   You can manage provisioned concurrency using Application Auto Scaling, which allows you to schedule or target utilization to maintain the desired level of reserved Lambda functions.

📝 **Note:** AWS has made significant improvements to reduce cold starts for Lambda functions in VPCs:- [https://aws.amazon.com/blogs/compute/announcing-improved-vpc-networking-for-aws-lambda-functions/](https://aws.amazon.com/blogs/compute/announcing-improved-vpc-networking-for-aws-lambda-functions/)

### Additional Resources

For a deeper understanding of reserved concurrency and provisioned concurrency, refer to the diagrams provided in the slides. These diagrams visually illustrate how these concepts work:- [https://docs.aws.amazon.com/lambda/latest/dg/configuration-concurrency.html
](https://docs.aws.amazon.com/lambda/latest/dg/configuration-concurrency.html)

![AWS Lambda Concurrency and Provisioned Concurrency](./img/AWS_Lambda_Concurrency_and_Provisioned_Concurrency.png)

### Hands-on Exploration

Now, let's move on to a hands-on demonstration to see how concurrency works in practice.

---

## 7. Concurrency Settings in Lambda

Let's explore the concurrency settings within a Lambda function. You can find these settings under **Configuration** in the left-hand menu, then selecting the **Concurrency** tab.

Initially, your function uses the **unreserved account concurrency**.

*   Your entire AWS account has a default unreserved concurrency limit, often 1000 (now it is much lower 10 only, AWS raises quotas automatically based on the usage, or you can request a quota increase).
*   This limit is shared across all Lambda functions in your account.

> **AWS Lambda** had a **default unreserved account concurrency limit of 1000** per Region.
>
> 👉 But starting in **late 2023–2024**, **AWS reduced the initial default concurrency limit for new accounts** to **10**.
>
> This is part of their **gradual scaling model for new accounts** — AWS automatically raises this limit as your usage increases, or you can request a **Service Quota increase** via the AWS console.

You can edit the concurrency settings for individual Lambda functions.

*   You can reserve a specific amount of concurrency for a function. 📌 **Example:** Reserving 20 concurrency for a Lambda function means that it will always have access to those 20 concurrent executions.
*   The remaining unreserved account concurrency is then available to all other functions. 📌 **Example:** If you reserve 20 concurrency from a 1000 limit, the other functions share 980.

To test concurrency limits and throttling, you can reserve zero concurrency for a function.

*   This effectively disables the function, causing it to always be throttled.
*   This is a useful way to test how your application handles throttling errors.

Here's how to simulate a concurrency limit error:

1.  Set the reserved concurrency to zero.
2.  Invoke the function.

You should receive an error message indicating that you have exceeded the rate.

```text
Calling the invoke API action failed with this message: Rate Exceeded.
```

This error message is what you'll see when you exceed your reserved concurrency. 💡 **Tip:** Setting concurrency to zero is a great way to test your application's resilience to throttling.

To resolve the throttling, revert to using the unreserved account concurrency or reserve a specific amount of concurrency for the function.

Provisioned concurrency helps to mitigate cold starts.

*   Cold starts occur when a Lambda function is invoked for the first time or after a period of inactivity, requiring initialization time.
*   Provisioned concurrency keeps a "warm pool" of function instances ready to serve requests, reducing latency.

To configure provisioned concurrency:

1.  Navigate to the **Provisioned Concurrency Configuration** section.
2.  Add a configuration.
3.  Choose an alias or a version for the configuration. 📝 **Note:** You cannot apply provisioned concurrency to the `$LATEST` version; you must publish a version first. We'll cover aliases and versions in later lectures.

When setting provisioned concurrency, consider the associated costs. ⚠️ **Warning:** Provisioned concurrency is not free, so choose a number that aligns with your needs and budget. 📌 **Example:** Setting provisioned concurrency to 5 will incur costs.

---

## 8. Lambda SnapStart

Lambda SnapStart is a feature designed to significantly improve your Lambda function performance, potentially up to 10x, for Java, Python, and .NET runtimes. For Java it comes at no extra cost. For Python and .NET (added Nov 2024), you pay for caching the snapshot and for each restore. 🚀

The core idea is to enable Lambda functions to execute much faster. Let's examine what happens when SnapStart is *disabled*.

When SnapStart is disabled, a Lambda function invocation goes through three lifecycle phases:

1.  **Initialize:** The function's environment is set up. ⚙️
2.  **Invoke:** The function's code is executed. ⚡
3.  **Shutdown:** The function's resources are released. 🚫

The **Initialize** phase can often be time-consuming, especially for languages like Java that require significant environment setup. ⏳

With SnapStart *enabled*, a key optimization is introduced: **pre-initialization**.

![Lambda SnapStart](./img/Lambda_SnapStart.png)

The lifecycle changes to:

1.  **Pre-initialize:** (Done by AWS Lambda) ⚙️
2.  **Invoke:** The function's code is executed. ⚡
3.  **Shutdown:** The function's resources are released. 🚫

This means the function can bypass the lengthy initialization phase and directly enter the invoke stage, drastically reducing latency. 🏎️

So, how does this pre-initialization magic happen?

When you publish a new version of your Lambda function:

1.  Lambda automatically initializes the function. ⚙️
2.  A snapshot of the memory and disk state is created. 📸
3.  This snapshot is then used for subsequent invocations, allowing low-latency access and immediate entry into the invoke stage. 🪄

That's the essence of Lambda SnapStart! It's a powerful optimization technique to accelerate your Lambda function execution. 🎉

---

## 9. Customization At The Edge

Modern applications often require executing logic at the edge before reaching the application itself. These are called **Edge Functions**, pieces of code attached to your CloudFront distributions. The goal is to minimize latency by running these functions closer to users.

CloudFront offers two types of functions: **CloudFront Functions and Lambda@Edge**. Understanding their differences and when to use each is key.

By using Edge Functions, you avoid managing servers, as they are deployed globally and are fully serverless. You only pay for what you use.

### Use Cases of CloudFront Functions and Edge Functions

Edge Functions can be used for a variety of customizations, including:

*   Website security and privacy 🛡️
*   Dynamic web applications at the Edge 🌐
*   Search engine optimization (SEO) 🔍
*   Intelligent routing across Origins and data centers 🚦
*   Bot mitigation at the Edge 🤖
*   Real-time image transformation at the Edge 🖼️
*   A/B testing 🧪
*   User authentication and authorization 🔑
*   User prioritization 🥇
*   User tracking and analytics 📊

### CloudFront Functions

Let's examine CloudFront Functions and how they operate.

![CloudFront Functions](./img/CloudFront_Functions.png)

A typical request flow in CloudFront looks like this:

1.  **Viewer Request:** A client sends a request to CloudFront.
2.  **Origin Request:** CloudFront forwards the request to your origin server.
3.  **Origin Response:** The server responds to CloudFront.
4.  **Viewer Response:** CloudFront sends the response to the client.

CloudFront Functions are lightweight functions written in JavaScript. They modify the viewer request and response. They are designed for high-scale, latency-sensitive CDN customizations, offering sub-millisecond startup times and scaling to millions of requests per second.

*   CloudFront Functions can only modify the viewer request and response.
    *   **Viewer Request:** After CloudFront receives a request from a viewer.
    *   **Viewer Response:** Before CloudFront forwards the response back to the viewer.

CloudFront Functions are a native feature of CloudFront, and the code is managed directly within CloudFront.

💡 **Tip:** CloudFront Functions are ideal for high-performance, high-scale scenarios focusing solely on viewer requests and responses.

### Lambda@Edge

Lambda@Edge provides more flexibility.

![Lambda@Edge](./img/Lambda@Edge.png)

These functions are written in NodeJS or Python and scale to thousands of requests per second. They can modify all CloudFront requests and responses:

*   **Viewer Request:** After CloudFront receives a request from a viewer.
*   **Origin Request:** Before CloudFront forwards a request to the origin.
*   **Origin Response:** After CloudFront receives the response from the origin.
*   **Viewer Response:** Before CloudFront forwards the response back to the viewer.

You author your function in the `us-east-1` region (the same region where you manage your CloudFront distributions), and CloudFront replicates the function to all its locations.

### CloudFront Functions vs. Lambda@Edge: Key Differences

| Feature                          | CloudFront Functions                              | Lambda@Edge                                    |
|----------------------------------|--------------------------------------------------|------------------------------------------------|
| **Runtime Support**              | JavaScript                                       | Node.js, Python                                |
| **# of Requests**                | **Millions** of requests per second              | **Thousands** of requests per second           |
| **CloudFront Triggers**          | - Viewer Request/Response                        | - Viewer Request/Response<br>- Origin Request/Response |
| **Max. Execution Time**          | < 1 ms                                           | 30 seconds (all events; older material says 5 s viewer / 30 s origin) |
| **Max. Memory**                  | 2 MB                                             | 128 MB up to 10 GB                             |
| **Total Package Size**           | 10 KB                                            | 50 MB (all events; older material says 1 MB for viewer events) |
| **Network Access, File System**  | No                                               | Yes                                            |
| **Access to the Request Body**   | No                                               | Yes                                            |
| **Pricing**                      | Free tier available, 1/6th price of @Edge        | No free tier, charged per request & duration   |

### Use Cases: CloudFront Functions

CloudFront Functions are suitable for:

*   Cache key normalization 🔑: Transforming request attributes (headers, cookies, query strings, URL) to create an optimal cache key.
*   Header manipulation ⚙️: Inserting, modifying, or deleting HTTP headers in the request or response.
*   URL rewrites or redirects ➡️
*   Request authentication & authorization 🔒: Creating and validating user-generated tokens (e.g JWT) to allow or deny requests.

These operations must be executed in less than one millisecond.

### Use Cases: Lambda@Edge

Lambda@Edge, with its longer execution time (up to 30 seconds), adjustable CPU and memory, allows for more complex operations.

*   You can load third-party libraries, such as the AWS SDK, to access other AWS services.
*   You have network access to external services for data processing.
*   Lambda@Edge provides file system access and access to the HTTP request body, enabling more extensive customizations.

📌 **Example:** Using Lambda@Edge to authenticate users against a database before serving content.

📝 **Note:** Choose CloudFront Functions for simple, high-performance tasks at the viewer level. Opt for Lambda@Edge when you need more processing power, access to external resources, or control over origin requests.

---

## 10. Networking Fundamentals for Lambda

By default, Lambda functions are launched outside of your VPC, specifically in an AWS-owned VPC. This means your Lambda functions won't have direct access to resources within your VPC.

*   If you have an RDS database, ElastiCache cluster, or an internal load balancer, your Lambda function won't be able to connect to them.
*   This default deployment works if your Lambda function needs to access public APIs on the internet or services like DynamoDB, which are public AWS resources.
*   However, if you need to access a private RDS database, the connection will fail.

![Default Lambda Deployment](./img/default-lambda-deployment.png)

To enable access to resources within your VPC, you need to launch your Lambda function within your VPC. Here's how:

1.  Specify your VPC ID.
2.  Choose the subnets where you want to launch the Lambda function.
3.  Attach a security group to your Lambda function.

This will create an Elastic Network Interface (ENI) for your Lambda function within your chosen subnets, allowing it to access resources like your Amazon RDS database running in your VPC. This provides private connectivity to anything within your VPC.

![Lambda in VPC](./img/lambda-in-vpc.png)

A major use case for Lambda in a VPC is integrating it with the RDS Proxy.

*   Without a proxy, Lambda functions directly accessing your RDS database can lead to problems.
*   The ephemeral nature of Lambda functions can result in **too many open connections under high load**, causing timeouts and other issues.

To solve this, launch an RDS Proxy. The RDS Proxy pools connections and connects to your RDS database instance with fewer connections. Your Lambda functions then connect to the RDS Proxy, which in turn connects to the RDS database instance.

![Lambda RDS Proxy](./img/lambda-rds-proxy.png)

The RDS Proxy offers several benefits:

1.  **Improves Scalability:** By pooling and sharing database connections. 🚀
2.  **Improves Availability:** Reduces failover time by up to 66% and preserves connections during failover events (for RDS and Aurora). ⏱️
3.  **Enforces IAM Authentication:** You can enforce IAM authentication at the RDS Proxy level and store credentials in Secrets Manager. 🔑

**For Lambda functions to connect to your RDS Proxy, they **must** be launched within your VPC. This is because the RDS Proxy is never publicly accessible. If your Lambda functions are launched publicly, they won't have network connectivity to the RDS Proxy.**

📝 **Note:** Understanding this concept is crucial for answering certain questions on the exam.

---

## 11. Tight Integration Between RDS, Aurora, and Lambda

Yes, there is a tight integration between RDS, Aurora, and Lambda. You can invoke Lambda functions directly from within your database instance in certain scenarios. This allows you to process data events happening within your database.

This integration is supported by:

*   RDS for PostgreSQL
*   Aurora MySQL
*   Aurora PostgreSQL (version 11.9 and higher, using the `aws_lambda` extension)

Here's how it works:

1.  A user inserts event data into a table (e.g., a registration table).
2.  RDS is configured to directly invoke a Lambda function.
3.  The Lambda function processes the data (e.g., sends a welcome email to the user).
4.  The user receives the email. 📧

📝 **Note:** You must set up this integration from within the database by connecting to it, not from the AWS console.

![RDS Lambda Integration](./img/rds-lambda-integration.png)

When the RDS instance invokes the Lambda function, ensure the following:

*   **Network connectivity (outbound traffic) is allowed from the RDS database instance to the Lambda function**. This can be achieved through:
    *   Public internet access
    *   NAT Gateway
    *   VPC Endpoints
*   **The RDS database instance has the necessary IAM permissions to invoke the Lambda function.** 🔑

### RDS Event Notifications

⚠️ **Warning:** This is different from using RDS event notifications. RDS event notifications provide information about the database instance itself (e.g., creation time, start time), not the data within the database. Don't confuse the two!

![RDS Event Notifications](./img/rds-event-notifications.png)

RDS event notifications can provide information about:

*   Database instances
*   Database snapshots
*   Parameter groups
*   Security groups
*   Proxies
*   Custom engine versions

These notifications are near real-time (up to five minutes of delivery). You can send these notifications to:

*   SNS (Simple Notification Service)
    *   From SNS, you can send them to an SQS Queue or a Lambda function.
*   EventBridge
    *   From EventBridge, you can send them to various destinations, including a Lambda function.

💡 **Tip:** If you need information about data events within your database, use the direct Lambda invocation method described earlier, not RDS event notifications.

---

## 12. Amazon DynamoDB: A Deep Dive

DynamoDB is a fully managed (proprietary to AWS), highly available, and Cloud-native NoSQL database service offered by AWS. It replicates data across multiple Availability Zones (AZs) for enhanced durability and availability.

*   **It's not a relational database like RDS or Aurora, but it still supports transactions.**
*   DynamoDB is designed for massive scalability, capable of handling millions of requests per second, trillions of rows, and hundreds of terabytes of storage. 🚀
*   **It provides single-digit millisecond performance, ensuring fast and consistent in performance**. ⚡
*   Security is integrated with IAM for authorization and administration.
*   It offers auto-scaling capabilities and eliminates the need for maintenance or patching.
* It has 2 kind of table classes: 
    - Standard: For Frequently Access data.
    - Infrequent Access (IA): For Less Frequently Access data.

### Key Features

*   **Fully Managed:** No need to provision or manage the underlying infrastructure.
*   **Scalable:** Handles massive workloads with ease.
*   **Fast:** Single-digit millisecond performance.
*   **Secure:** Integrated with IAM for security and access control.
*   **Cost-Effective:** Auto-scaling and on-demand pricing options.

### DynamoDB Basics - Tables, Items, and Attributes

DynamoDB is structured around tables, items (rows), and attributes (columns).

*   You create tables within the DynamoDB service.
*   Each table requires a primary key, defined at creation time.
*   Tables can contain an unlimited number of items (rows).
*   Items consist of attributes, which are similar to columns in relational databases.
*   Unlike relational databases, you can add attributes to items over time without pre-defining a schema. ➕
*   Attributes can be null.
*   The maximum item size in DynamoDB is 400 KB. ⚠️ DynamoDB is not suitable for storing very large objects.

### Data Types

DynamoDB supports various data types:

*   **Scalar Types:** String, Number, Binary, Boolean, Null
*   **Document Types:** List, Map
*   **Set Types:** String Set, Number Set, Binary Set

### Schema Evolution

DynamoDB is an excellent choice when your schema needs to evolve rapidly. 💡 **Tip:** Look for scenarios in the exam where the schema requires frequent changes. In such cases, DynamoDB is often a better option than RDS or Aurora.

### Primary Key

A DynamoDB table's primary key can consist of:

*   **Partition Key:** Used for distributing data across partitions.
*   **Sort Key (Optional):** Used for sorting items within a partition.

📌 **Example:** A table might have `UserID` as the partition key and `Timestamp` as the sort key.

![DynamoDB Primary Key](./img/DynamoDB_primary_key.png)

### Capacity Modes

You need to choose a capacity mode for managing read and write operations on your DynamoDB table. There are two modes:

1.  **Provisioned Mode:** (older material calls this the default; since Nov 2024 AWS calls on-demand the default and recommended mode)

    *   You specify the number of reads and writes per second you expect.
    *   You pay for provisioned Read Capacity Units (RCU) and Write Capacity Units (WCU).
    *   You can use auto-scaling to adjust RCU and WCU based on the table's load.
    *   You can provision them independently or use on-demand mode.
    *   Best suited for predictable workloads with smooth traffic patterns. 📈
2.  **On-Demand Mode:**

    *   Read and write capacity scales automatically with your workload.
    *   No capacity planning is required.
    *   You pay for each read and write operation you perform.
    *   More expensive per request than well-used provisioned mode (AWS cut on-demand prices by 50% in Nov 2024), but ideal for unpredictable workloads or those with sudden spikes. 💥
    *   No concept of RCU and WCU.

💡 **Tip:** Choose the right capacity mode based on your workload's characteristics.

### Choosing Between Provisioned and On-Demand Mode

Consider the following scenarios when selecting a capacity mode:

*   If your application scales from 1,000 to 1 million transactions in under a minute, On-Demand Mode is the better choice because Provisioned Mode may not scale fast enough. 🚀
*   If your workload has very few transactions (e.g., four or five times a day), On-Demand Mode can be cost-effective because you only pay for the transactions you use. 💰

📝 **Note:** Pay attention to keywords in the exam questions that indicate unpredictable workloads or the need for rapid scaling. These scenarios often point to On-Demand Mode.

---

## 13. DynamoDB First Look

DynamoDB allows you to create tables. When creating a table, there is no database creation step, making it a serverless database.

To create a table:
1.  Click on "Create table".
2.  Enter a table name. 📌 **Example:** `DemoTable`.
3.  Choose a **partition key** (required) and optionally a **sort key**. The combination of these two columns forms the primary key. 📌 **Example:** `user_id` as the partition key.

### Read/Write Capacity Settings

For read and write capacity settings, you have two options:

*   On-demand
*   Provisioned

#### On-Demand

*   Pay for the actual reads and writes your application performs.
*   Great for applications with unpredictable workloads.
*   ⚠️ **Warning:** Older material says it can be two to three times more expensive than provisioned capacity. Since AWS cut on-demand prices by 50% in Nov 2024, AWS says most workloads cost less on on-demand. Provisioned is still cheaper for steady, well-used tables.
*   💡 **Tip:** The exam will test you on when to use on-demand vs provisioned.

#### Provisioned

*   Manage and optimize costs by allocating read and write capacity in advance.
*   You can configure read capacity units (RCU) and write capacity units (WCU).
*   Auto-scaling can be enabled or disabled for both RCU and WCU.

    *   **Auto-scaling Off:** You specify a fixed number of capacity units. 📌 **Example:** 10 RCU and 5 WCU.
    *   **Auto-scaling On:** DynamoDB automatically adjusts capacity based on target utilization. You define a minimum, maximum, and target utilization percentage. 📌 **Example:** Min 1, Max 100, Target Utilization 70%.

### Indexes

Indexes will be discussed in a later lecture.

💡 **Tip:** There is a capacity calculator to help determine the appropriate read and write capacity, but it's more relevant for the developer exam.

### Pricing

The estimated cost is displayed based on your RCU and WCU settings. 📌 **Example:** 71 cents per month with 1 RCU and 1 WCU.

### Encryption

Enable encryption at rest for data security (AWS owned key).

### Inserting Data

After creating the table, you can insert items:

1.  Click on the table.
2.  Select "View items".
3.  Click "Create item".
4.  Specify the partition key value. 📌 **Example:** `stephane_123`.
5.  Add attributes to the item. 📌 **Example:** `name: Stephane Maarek`, `favorite_movie: Memento`, `favorite_number: 42`.
6.  Click "Create item" to insert the data.

![DynamoDB Insert Table Data - Sample](./img/DynamoDB_Insert_Table_Sample_Data.png)

📌 **Example:**
```json
{
  "userID": "stephane_123",
  "name": "Stephane Maarek",
  "favorite_movie": "Memento",
  "favorite_number": 42
}
```

📌 **Example:**
```json
{
  "userID": "Alice_456",
  "name": "Alice Doe",
  "favorite_movie": "Pocahontas",
  "age": 23
}
```

📝 **Note:** DynamoDB is a NoSQL database, so items don't need to have the same attributes. Each item can have different attributes.

---

## 14. Advanced DynamoDB Features

Let's explore some advanced features of DynamoDB that are important to understand.

### a. DynamoDB Accelerator (DAX) 🚀

![DAX](./img/DAX.png)

DAX is a fully-managed, highly available, and seamless **in-memory cache for DynamoDB**. It helps solve read congestion by caching data, providing **microseconds** (look for this keyword in exam) latency for cached data.

*   DAX is compatible with existing DynamoDB APIs, so no application logic changes are required.
*   You create a DAX cluster made of cache nodes and connect to it. The DAX cluster then connects to your DynamoDB table behind the scenes.
*   The cache has a default TTL of five minutes, but this can be changed.

![DAX vs ElastiCache](./img/DAX_vs_ElastiCache.png)

Why use DAX instead of ElastiCache?

*   DAX is specifically designed for DynamoDB and is helpful for caching individual objects, queries, and scanned queries.
*   ElastiCache is better for storing aggregation results or large computations performed on top of DynamoDB.
*   They are complementary, but DAX is often the preferred caching solution for DynamoDB.

### b. DynamoDB Streams 🌊

DynamoDB Streams capture a stream of all modifications (create, update, delete) that happen on your table.

Use cases:

*   Reacting to changes in real-time (e.g., sending a welcome email for new users).
*   Real-time usage analytics.
*   Inserting data into a derivative table.
*   Cross-region replication.
*   Invoking Lambda functions on changes to your DynamoDB table.

Two main approaches to stream processing:

1.  **DynamoDB Streams:**
    *   24-hour retention.
    *   Limited number of consumers.
    *   Great for use with Lambda triggers.
    *   DynamoDB Stream Kinesis Adapter is available if you want to read it yourself.
2.  **Kinesis Data Streams:**
    *   Up to one year of retention.
    *   Higher number of consumers.
    *   More ways to process data (Lambda, Kinesis Data Analytics, Kinesis Data Firehose, Glue Streaming ETLs, etc.).

![DynamoDB Streams vs Kinesis Data Streams](./img/DynamoDB_Streams_vs_Kinesis_Data_Streams.png)

### c. Global Tables 🌍

![DynamoDB Global Tables](./img/DynamoDB_Global_Tables.png)

A global table is replicated across multiple regions (e.g., US-East-1 and AP-Southeast-2).

*   There is two-way replication between the tables.
*   Applications can read and write to the table in any region (Active-Active replication).
*   The goal is to provide low-latency access to DynamoDB in multiple regions.
*   Global tables (eventual-consistency mode) use DynamoDB Streams to replicate the table across regions. In the current version (2019.11.21), DynamoDB turns Streams on for the replicas by default. Older material says you must enable Streams first (that was the 2017 legacy version). The strong-consistency mode (MRSC, Jun 2025) does not use Streams.

### d. Time To Live (TTL) ⏳

![DynamoDB TTL](./img/DynamoDB_TTL.png)

TTL automatically deletes items after an expiry timestamp.

*   You define a TTL attribute (e.g., `ExpTime`) with a timestamp.
*   When the current time exceeds the `ExpTime`, the item is automatically expired and eventually deleted.

Use cases:

*   Storing only the most current items.
*   Adhering to regulatory obligations (e.g., deleting data after two years).
*   Web session handling: store session data in DynamoDB with a TTL (e.g., two hours). -> Very common in the exam.

> A user logs into your website and a session is created. Instead of storing the session locally, you keep it in a central store such as DynamoDB, with a time-to-live (TTL) of two hours. When the session starts, the data is written to DynamoDB, making it accessible to any of your applications that need it. If the session isn’t renewed within two hours, DynamoDB automatically expires it, and the record is removed from the table.

📌 **Example:** A `SessionData` table with an `ExpTime` attribute.

### e. Disaster Recovery 🛡️

Backup options:

1.  **Continuous Backups with Point-in-Time Recovery (PITR):**
    *   Optionally enabled.
    *   Allows recovery to any point in time within the recovery period. The period is configurable from 1 to 35 days (default 35).
    *   Recovery creates a new table.
2.  **On-Demand Backups:**
    *   Retained until explicitly deleted.
    *   Do not affect performance or latency.
    * **AWS Backup Service:**
        *   Enables lifecycle policies for backups.
        *   Allows copying backups across regions for disaster recovery.
        *   Recovery creates a new table.

### f. DynamoDB and S3 Integration 🗄️

![DynamoDB and S3 Integration](./img/DynamoDB_and_S3_Integration.png)

*   **Export to S3 (Must enable PITR [Point-in-Time Recovery]):**
    *   Requires enabling point-in-time recovery.
    *   Allows exporting a DynamoDB table to S3 for querying (e.g., using Amazon Athena).
    *   Does not affect read capacity or performance.
    *   Export Format: DynamoDB JSON or ION.
    *   Use cases: data analysis, auditing, ETL.
        * Perform **data analysis** on top of DynamoDB table.
        * Retain snapshots for **auditing**.
        * ETL on top of S3 data before importing back into DynamoDB.
*   **Import from S3:**
    *   Allows importing data from S3 (CSV, JSON, or ION format) into a new DynamoDB table.
    *   Does not consume write capacity.
    *   Creates a new table.
    *   Import errors are logged in CloudWatch Logs.

---

## 15. API Gateway Overview

### Building a Serverless API

So far in our serverless journey, we’ve learned how to:

* Create **Lambda functions**
* Use **DynamoDB** as a database for our API (supporting Create, Read, Update, Delete operations).

Now, we need a way for clients to **invoke our Lambda functions**. There are multiple options:

1. **Direct Invocation**

   * The client can directly invoke the Lambda function.
   * ⚠️ However, this requires the client to have **IAM permissions**, which isn’t ideal for public-facing applications.

2. **Application Load Balancer (ALB)**

   * We can place an **ALB** between the client and the Lambda.
   * This exposes the Lambda function as an **HTTP endpoint**.

3. **API Gateway (Recommended)**

   * **Amazon API Gateway** is a **serverless service** that allows us to create **REST APIs**.
   * The client communicates with the **API Gateway**, which then proxies the request to our Lambda function.
   * ✅ The big advantage: API Gateway offers **more than just an HTTP endpoint**. It provides features such as:

     * Authentication & authorization
     * Request validation
     * Rate limiting & throttling
     * Caching
     * Monitoring & logging

![API Gateway](./img/API_Gateway_Serverless.png)

In short, while direct invocation and ALB are possible, **API Gateway** is the most powerful and flexible option for exposing Lambda functions to clients.

### Deep Dive into Amazon API Gateway 🚀

Amazon API Gateway is a powerful serverless service that allows you to build, manage, and secure APIs at scale. It goes far beyond simply exposing Lambda functions as HTTP endpoints. Let’s explore its features, integrations, deployment models, and security options.

### 🌐 Protocol and API Management

* **WebSocket Protocol** support for real-time streaming.
* **API versioning**: Move from v1 → v2 → v3 without breaking existing clients.
* **Environment support**: Manage separate **dev, test, and prod** environments.

### 🔒 Security Features

API Gateway provides multiple layers of security for both **authentication** and **authorization**:

* Create **API keys** for client access.
* Implement **request throttling** to prevent abuse.
* Integrate with standards like **Swagger** or **OpenAPI 3.0** for easy API import/export.
* Enable **request and response transformation/validation** to enforce correct invocations.
* Generate **SDKs and API specifications** for client developers.
* Add **caching** to improve performance.

### 🔗 Integrations

API Gateway can integrate with different backends:

1. **Lambda Functions** (most common)

   * Proxy client requests to Lambda functions (Invoke Lambda Functions).
   * Ideal for creating full **serverless REST APIs** (Easy way to expose REST API backed by AWS Lambda).

2. **HTTP Endpoints**

   *  Expose any HTTP endpoint in the backend.
   * Connect to on-premises APIs or cloud-based endpoints (including Application Load Balancers).
   * Leverage API Gateway for **caching, authentication, rate limiting, and API key usage**.

3. **AWS Services**

   * Start a **Step Functions workflow**.
   * Send a message to **SQS**.
   * Write data to **Kinesis Data Streams** securely without exposing AWS credentials.

📌 **Example: Kinesis Data Streams**
Clients send HTTP requests → API Gateway → Kinesis Data Streams → Firehose → Amazon S3 (JSON format).
This flow provides a **secure, fully managed, serverless pipeline** for data ingestion.

![API Gateway Integrations example with Kinesis Data Streams](./img/API_Gateway_Integrations_example_with_Kinesis_Data_Streams.png)

### 🌍 Deployment Models (Endpoint Types)

API Gateway supports three types of endpoints:

1. **Edge-Optimized (Default)**

   * Best for **global clients**.
   * Requests are routed through **CloudFront Edge locations** for low latency.
   * API itself resides in a single AWS Region.

2. **Regional**

   * Best when clients are within the **same AWS Region**.
   * Optionally, you can front it with your own CloudFront distribution for custom caching or routing strategies.

3. **Private**

   * Accessible **only within your VPC** via **interface VPC endpoints (ENIs)**.
   * Access controlled with **resource policies**.

### 🛡️ Security & Access Control

1. **User Authentication through**:
    * **IAM Roles**: Best for internal apps (e.g., EC2 → API Gateway).
    * **Amazon Cognito**: Manage user authentication for web or mobile apps.
    * **Custom Authorizers**: Implement your own logic with a Lambda function.
2. **Custom Domains + TLS**: Use **AWS Certificate Manager (ACM)** to secure your APIs with HTTPS.
    * Edge-optimized endpoints require certificates in **us-east-1**.
    * Regional endpoints can use certificates in the same region as the API stage.
    * Configure **Route 53 DNS records (CNAME or Alias A)** to map your custom domain to API Gateway.

✅ In summary, API Gateway is not just a gateway—it’s a **feature-rich API management layer** that supports **real-time communication, versioning, security, caching, monitoring, and deep AWS integrations**. This makes it one of the core building blocks of serverless architectures.

---

## 16. API Gateway Basics Hands On

Let's explore the basics of API Gateway. We'll focus on creating a REST API and integrating it with Lambda functions.

First, navigate to the API Gateway console. You'll see options for different API types: HTTP APIs, WebSocket APIs, and REST APIs (public or private). For this example, we'll focus on **REST API**.

When building a REST API, you have several options:

*   Create a new API.
*   Import one from an OpenAPI definition file.
*   Clone an existing API.
*   Start from an example API.

We'll start with creating a new API.

1.  Name the API: `MyFirstAPI`.
2.  Choose the API endpoint type. You have three options:
    *   **Regional:** Deployed in one region.
    *   **Edge-optimized:** Deployed in many regions at the edge, but the API still lives in one region.
    *   **Private:** Not exposed to the web.

For simplicity, choose **Regional** API type.

Now, let's create our first method in this API.

1.  Click on "Create Method".
2.  Choose the method type. We'll use **GET** to retrieve a page.
3.  Select the integration type. We have five options:
    *   Lambda function
    *   HTTP
    *   Mock
    *   AWS service
    *   VPC link

We'll use **Lambda function** integration. You can also integrate with any AWS service in any region, exposing them as an API through API Gateway.

Since we're using a Lambda function, we need to create one.

1.  Go to Lambda and create a new function.
2.  Name it: `api-gateway-route-gets`.
3.  Choose the runtime: Python 3.11 (or any Python 3 version).

While the function is creating, let's look at the code we'll use.

📌 **Example:** Here's a simple Python Lambda function:

```python
import json

def lambda_handler(event, context):
    body = "Hello from Lambda!"
    statusCode = 200
    return {
        'statusCode': statusCode,
        'body': json.dumps(body),
        'headers': {
            'Content-Type': 'application/json'
        }
    }
```

This function responds to an event with a body containing "Hello from Lambda!", a status code of 200, and a Content-Type header set to application/json.

1.  Copy the code to the Lambda function editor.
2.  Click on "Deploy" to deploy the function.
3.  Test the function by creating a test event (e.g., "DemoTest") and clicking "Test". You should see a result with status code 200 and the message "Hello from Lambda!".

Now, let's integrate this Lambda function with our API Gateway.

1.  Copy the Lambda function ARN.
2.  Go back to the API Gateway console.
3.  Paste the ARN into the Lambda function integration settings.
4.  Select **Lambda Proxy Integration** to pass the full request to Lambda and interpret the response.

⚠️ **Warning:** API Gateway has a default integration timeout of 29 seconds, regardless of the Lambda function's timeout setting. Since Jun 2024, you can request a higher limit for Regional and private REST APIs (not edge-optimized); this may lower your account throttle quota.

Click "Create Method". This will automatically grant API Gateway the right to invoke our Lambda function. You can verify this by checking the resource-based policy statement in the Lambda function's configuration > "Permissions".

Now, let's test our API.

1.  In the API Gateway console, click on "Test" at the bottom right.
2.  Click "Test" without specifying any query strings or headers.

You should see a response with a status code of 200 and the body "Hello from Lambda!". The response headers should include "Content-Type: application/json".

You can also view the execution log from the API Gateway for debugging purposes.

```python
def lambda_handler(event, context):
    print(event) # Print the event - Add this line
    body = "Hello from Lambda!"
    statusCode = 200
    return {
        'statusCode': statusCode,
        'body': json.dumps(body),
        'headers': {
            'Content-Type': 'application/json'
        }
    }
```

To see what's being sent to our Lambda function, add `print(event)` to the Lambda function code, deploy it, and invoke it again from the API Gateway. Then, check the CloudWatch logs to see the event data.

Now, let's create a new resource.

1.  Click on "Create Resource".
2.  Set the resource path to `/houses`.
3.  Create a GET method for the `/houses` resource, integrating it with a new Lambda function.

Let's create the new Lambda function quickly.

1.  Create a new function named `api-gateway-route-houses`.
2.  Use the Python runtime.
3.  Copy the code from the previous Lambda function and modify the message to "Hello from my pretty house!".

📌 **Example:**

```python
import json

def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': json.dumps('Hello from my pretty house!'),
        'headers': {
            'Content-Type': 'application/json'
        }
    }
```

1.  Deploy the new Lambda function.
2.  Copy the function ARN and paste it into the API Gateway integration settings for the `/houses` GET method.
3.  Test the `/houses` GET method. You should see a response with a status code of 200 and the body "Hello from my pretty house!".

Now, let's deploy the API to make it accessible from a web browser.

1.  Click on "Deploy API".
2.  Select a stage (e.g., create a new stage named "dev").
3.  Copy the invoke URL.

Open a web browser and paste the invoke URL.

*   Visiting `/dev` should return "Hello from Lambda!".
*   Visiting `/dev/houses` should return "Hello from my pretty house!".
*   Visiting `/dev/wrong` should return an error message, such as "Missing Authentication Token".

Congratulations! You've deployed your API to API Gateway with two Lambda functions responding to different routes.

📝 **Note:** This is a basic overview. Further customization and security configurations are possible.

---

## 17. AWS Step Functions: Serverless Visual Workflows

AWS Step Functions allow you to build serverless visual workflows for orchestration, primarily of your Lambda functions. You design a graph specifying what happens at each step based on success or failure. This enables complex workflows within AWS.

![AWS Step Functions](./img/AWS_step_functions.png)

Step Functions offer internal features such as:

*   Sequencing
*   Parallel functions
*   Conditions
*   Timeouts
*   Error handling

Step Functions integrate with more than just Lambda functions. They also work with:

*   EC2 instances
*   ECS tasks
*   On-premises servers
*   API Gateway
*   SQS queues
*   Many other AWS services

You can implement a human approval feature within a Step Function workflow. For instance, the workflow can pause for human review at a certain point. If approved, it continues; otherwise, it fails.

Use cases for Step Functions include:

*   Order fulfillment
*   Data processing
*   Web applications
*   Any complex workflow that benefits from a visual graph representation

---

## 18. Amazon Cognito

Amazon Cognito provides identities for users to interact with web and mobile applications. These users are typically outside your AWS account (Cognito -> Users we don't know about yet).

Cognito has two main sub-services:

*   Cognito User Pools (CUP)
*   Cognito Identity Pools (formerly Federated Identity)

### Cognito User Pools (CUP)

Cognito User Pools provide sign-in functionality for app users and integrate well with API Gateway and Application Load Balancer (ALB).

*   Provides a serverless user database for web and mobile applications.
*   Supports simple logins (username/email + password).
*   Offers password reset features.
*   Includes email and phone number verification.
*   Supports multi-factor authentication (MFA).
*   Integrates with social identity providers like Facebook and Google.

![Cognito User Pools](./img/AWS_Cognito_user_pools.png)

**Integration with API Gateway:**

1.  User connects to Cognito User Pool and retrieves a token.
2.  User passes the token to API Gateway.
3.  API Gateway verifies the token.
4.  If valid, API Gateway translates the token into a user identity.
5.  The user identity is passed to the backend Lambda function.
6.  The Lambda function now knows the authenticated user.

**Integration with Application Load Balancer (ALB):**

1.  Application connects to Cognito User Pool.
2.  Application passes the token to the ALB.
3.  ALB verifies the login.
4.  If valid, ALB redirects the request to the backend.
5.  ALB passes additional headers with the user's identity.

This setup allows you to verify user logins at the API Gateway or ALB level, offloading that responsibility from your backend.

### Cognito Identity Pools (Federated Identities)

Cognito Identity Pools provide temporary AWS credentials to users registered with your application, allowing them to directly access AWS resources.

*   User sources can be Cognito User Pools or third-party logins.
*   Users can access AWS services directly or through API Gateway.
*   IAM policies applied to the credentials are defined within the Cognito Identity Pool service.
*   Policies can be customized based on the user ID for fine-grained control.
*   A default IAM role can be defined for guest users or authenticated users without specific roles.

**How Identity Pools Work:**

![How Cognito Identity Pools Works](./img/how-cognito-identity-pools-work.png)

Let's say you have a web and mobile application and you want to allow users to access AWS resources (e.g., private S3 buckets. and DynamoDB tables) directly. Flow:-
1.  Web and mobile applications log in and get a token (e.g., from Cognito User Pools, social identity providers, SAML, or OpenID Connect).
2.  The application passes the token to the Cognito Identity Pool service.
3.  Cognito Identity Pool exchanges the token for temporary AWS credentials.
4.  The Identity Pool evaluates the token's validity.
5.  The Identity Pool crafts an IAM policy specific to the user.
6.  The temporary credentials, with the associated IAM policy, allow access to AWS resources (e.g., S3 buckets, DynamoDB tables) without going through an API Gateway or ALB.

📌 **Example:** Enabling row-level security in DynamoDB.

1.  The policy from Cognito Identity Pools includes a condition.
2.  The condition specifies that the leading key for DynamoDB must equal the Cognito identity user ID.
3.  The user can only read and write items in the DynamoDB table that they have access to through this condition.

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "dynamodb:GetItem",
        "dynamodb:PutItem",
        "dynamodb:UpdateItem",
        "dynamodb:DeleteItem"
      ],
      "Resource": "arn:aws:dynamodb:REGION:ACCOUNT_ID:table/TABLE_NAME",
      "Condition": {
        "ForAllValues:StringEquals": {
          "dynamodb:LeadingKeys": [
            "${cognito-identity.amazonaws.com:sub}"
          ]
        }
      }
    }
  ]
}
```

📝 **Note:** Cognito is a complex service, but understanding the high-level concepts is key.

**Key Takeaways:**

*   Create a user base for your web and mobile applications.
*   Enable row-level security in DynamoDB for fine-grained access.
*   Remember the integration between Cognito User Pools and API Gateway/ALB.

**IAM vs Cognito:**

IAM is for users within your AWS account. Cognito is for web and mobile application users outside of AWS. Look for keywords like "hundreds of users," "mobile users," or "authentication with SAML" to identify Cognito use cases.

---

## 19. Q & A

### ❓ Question 3

You have provisioned a **DynamoDB** table with **10 RCUs** (Read Capacity Units) and **10 WCUs** (Write Capacity Units).

A month later, you want to **increase the RCU** to handle more read traffic. What should you do?

* You need to increase both RCU and WCU
* Increase RCU and decrease WCU
* Increase RCU and keep WCU the same

<details>

<summary>Explanation</summary>

In **Amazon DynamoDB**, capacity is provisioned separately for reads and writes:

* **RCU (Read Capacity Units):** Defines how many strongly consistent or eventually consistent read operations per second the table can handle.
* **WCU (Write Capacity Units):** Defines how many write operations per second the table can handle.

📌 **Key Point:**

* **RCUs and WCUs are independent**.
* You can scale them **up or down separately** depending on your workload.

⚠️ **What this means for the question:**

* Since you only need to handle **more read traffic**, you just **increase RCUs**.
* WCUs stay the same unless your write workload also increases.

✅ Answer: **Increase RCU and keep WCU the same**

#### 📌 Example

If your app's read traffic doubles but the write traffic stays constant:

* Old: **10 RCU, 10 WCU**
* New: **20 RCU, 10 WCU**

This ensures you only pay for what you need.

✅ **Takeaway:** DynamoDB allows **independent scaling of read and write capacity**, making it flexible and cost-efficient.

</details>

### ❓ Question 13

You are a DevOps engineer in a football company that has a website that is backed by a DynamoDB table.

The table stores viewers’ feedback for football matches. You have been tasked to work with the analytics team to generate reports on the viewers’ feedback.

The analytics team wants the data in DynamoDB **JSON format** and hosted in an **S3 bucket** to start working on it and create the reports.

**What is the best and most cost-effective way you can achieve this task?**

#### **Options**

1. Select DynamoDB table then select Export to S3
2. Create a Lambda function to read DynamoDB data, convert them to JSON files, then store the files in S3 bucket
3. Use AWS Transfer Family
4. Use AWS DataSync

<details>

<summary>Explanation</summary>

* **Export to S3** is a built-in DynamoDB feature that allows you to directly export data to **Amazon S3** in **DynamoDB JSON or Amazon Ion** format (Parquet is not an export format).
* This method is **serverless** and **cost-effective**, because you only pay for the exported data and S3 storage. No extra infrastructure or custom code is required.
* Other options:

  * **Lambda function** → Involves writing custom code, higher costs (Lambda execution + S3 PUT requests), and maintenance overhead.
  * **AWS Transfer Family** → Used for transferring files via SFTP/FTPS/FTP, not for DynamoDB export.
  * **AWS DataSync** → Designed for migrating large datasets between on-premises and cloud, not for DynamoDB export.

✅ Hence, **Export to S3** is the best and most cost-effective solution.

**Correct Answer**: Select DynamoDB table then select Export to S3

</details>

---
