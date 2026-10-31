# Auto Scaling

## **Question 1**

A commercial bank has a forex trading application. They created an Auto Scaling group of EC2 instances that allow the bank to cope with the current traffic and achieve cost efficiency. They want the Auto Scaling group to behave in such a way that it will follow a predefined set of parameters before it scales down the number of EC2 instances, preventing unintended slowdown or unavailability.

Which of the following statements are true regarding the **cooldown period**? (Select TWO.)

### **Options**

1. It ensures that before the Auto Scaling group scales out, the EC2 instances have an ample time to cooldown.
2. It ensures that the Auto Scaling group launches or terminates additional EC2 instances without any downtime.
3. Its default value is 300 seconds.
4. Its default value is 600 seconds.
5. It ensures that the Auto Scaling group does not launch or terminate additional EC2 instances before the previous scaling activity takes effect.

<details>

<summary><strong>Answer & Explanation</strong> 📝</summary>

In **Auto Scaling**, the following statements are **correct** regarding the **cooldown period**:

✅ **It ensures that the Auto Scaling group does not launch or terminate additional EC2 instances before the previous scaling activity takes effect.**
✅ **Its default value is 300 seconds.**
✅ **It is a configurable setting for your Auto Scaling group.**

### ❌ **Incorrect Options**

<span style="color:red"><strong>🔴 – It ensures that before the Auto Scaling group scales out, the EC2 instances have ample time to cooldown.</strong></span>

<span style="color:red"><strong>🔴 – It ensures that the Auto Scaling group launches or terminates additional EC2 instances without any downtime.</strong></span>

<span style="color:red"><strong>🔴 – Its default value is 600 seconds.</strong></span>

These statements are inaccurate and don't depict what the word "cooldown" actually means for Auto Scaling. The **cooldown period** is a configurable setting for your Auto Scaling group that helps to ensure that it **doesn't launch or terminate additional instances before the previous scaling activity takes effect**. After the Auto Scaling group dynamically scales using a simple scaling policy, it waits for the cooldown period to complete before resuming scaling activities.

</details>

---

## **Question 2**

A tech company is currently using Auto Scaling for their web application. A new AMI now needs to be used for launching a fleet of EC2 instances. Which of the following changes needs to be done?

### **Options**

1. Create a new target group.
2. Create a new launch template.
3. Create a new target group and launch template.
4. Do nothing. You can start directly launching EC2 instances in the Auto Scaling group with the same launch template.

<details>

<summary><strong>Answer & Explanation</strong> 📝</summary>

Hint: Target groups are primarily used in ELBs and not in Auto Scaling. The scenario didn’t mention that the architecture has a load balancer. 

A **launch template** is a template that an **Auto Scaling group** uses to launch **EC2 instances**. When you create a launch template, you specify information for the instances, such as the **ID of the Amazon Machine Image (AMI)**, the **instance type**, a **key pair**, one or more **security groups**, and a **block device mapping**. If you've launched an EC2 instance before, you specified the same information in order to launch the instance.

You can specify your **launch template** with multiple Auto Scaling groups. However, you can only specify **one launch template for an Auto Scaling group at a time**, and **you can't modify a launch template after you've created it**. Therefore, if you want to change the launch template for an Auto Scaling group, you must **create a new template** and then update your Auto Scaling group with the new launch template.

For this scenario, you have to **create a new launch template**. Remember that you **can't modify a launch template** after you've created it.

> ⚠️ Note (2026): With launch templates you can also create a new **version** of the same template with the new AMI. Only launch configurations (now legacy) need a completely new object.

### ✅ **Correct Answer**

🟢 **Create a new launch template.**

### ❌ **Incorrect Options (with reasons)**

<span style="color:red"><strong>🔴 The option that says: Do nothing. You can start directly launching EC2 instances in the Auto Scaling group with the same launch template is incorrect</strong></span> because what you are trying to achieve is to **change the AMI** being used by your fleet of EC2 instances. Therefore, you need to change the launch template to update what your instances are using.

<span style="color:red"><strong>🔴 The option that says: Create a new target group and Create a new target group and launch template are both incorrect</strong></span> because you only want to **change the AMI** being used by your instances, and not the instances themselves. **Target groups** are primarily used in **ELBs** and not in Auto Scaling. The scenario didn't mention that the architecture has a load balancer. Therefore, you should be updating your **launch template**, not the **target group**.

</details>

---

## **Question 3**

A tech company has a CRM application hosted on an Auto Scaling group of On-Demand EC2 instances with different instance types and sizes. The application is heavily used from 9 AM to 5 PM. Users report the application is slow at the start of the day but works normally after a few hours.

Which of the following is the **MOST operationally efficient** solution to ensure the application works properly at the beginning of the day?

### **Options**

1. Configure a Dynamic scaling policy for the Auto Scaling group to launch new instances based on the CPU utilization.
2. Configure a Predictive scaling policy for the Auto Scaling group to automatically adjust the number of Amazon EC2 instances.
3. Configure a Dynamic scaling policy for the Auto Scaling group to launch new instances based on the Memory utilization.
4. Configure a Scheduled scaling policy for the Auto Scaling group to launch new instances before the start of the day.

<details>

<summary><strong>Answer & Explanation</strong> 📝</summary>

**Scaling based on a schedule** allows you to scale your application in response to predictable load changes. For example, every week the traffic to your web application starts to increase on Wednesday, remains high on Thursday, and starts to decrease on Friday. You can plan your scaling activities based on the predictable traffic patterns of your web application.

To configure your Auto Scaling group to scale based on a schedule, you create a **scheduled action**. The scheduled action tells **Amazon EC2 Auto Scaling** to perform a scaling action at specified times. To create a scheduled scaling action, you specify the **start time** when the scaling action should take effect and the **new minimum, maximum, and desired sizes** for the scaling action. At the specified time, Amazon EC2 Auto Scaling updates the group with the values for minimum, maximum, and desired size specified by the scaling action. You can create scheduled actions for scaling **one time only** or for scaling on a **recurring schedule**.

### ✅ **Correct Answer**

🟢 **Hence, configuring a Scheduled scaling policy for the Auto Scaling group to launch new instances before the start of the day is the correct answer.** You need to configure a Scheduled scaling policy. This will ensure that the instances are already scaled up and ready before the start of the day since this is when the application is used the most.

### ❌ **Incorrect Options**

<span style="color:red"><strong>🔴 The following options are both incorrect.</strong></span> Although these are valid solutions, it is still better to configure a Scheduled scaling policy as you already know the exact peak hours of your application. By the time either the CPU or Memory hits a peak, the application already has performance issues, so you need to ensure the scaling is done beforehand using a Scheduled scaling policy:

* <span style="color:red"><strong>Configure a Dynamic scaling policy for the Auto Scaling group to launch new instances based on the CPU utilization</strong></span>
* <span style="color:red"><strong>Configure a Dynamic scaling policy for the Auto Scaling group to launch new instances based on the Memory utilization</strong></span>

<span style="color:red"><strong>🔴 The option that says: Configure a Predictive scaling policy for the Auto Scaling group to automatically adjust the number of Amazon EC2 instances is incorrect.</strong></span> Although this type of scaling policy can be used in this scenario, it is not the most operationally efficient option. Take note that the scenario mentioned that the Auto Scaling group consists of Amazon EC2 instances with different instance types and sizes. Predictive scaling assumes that your Auto Scaling group is homogenous, which means that all EC2 instances are of equal capacity. The forecasted capacity can be inaccurate if you are using a variety of EC2 instance sizes and types on your Auto Scaling group.

</details>

---

## **Question 4**

**An application is hosted in an Auto Scaling group of EC2 instances. To improve the monitoring process, you have to configure the current capacity to increase or decrease based on a set of scaling adjustments. This should be done by specifying the scaling metrics and threshold values for the CloudWatch alarms that trigger the scaling process.**

Which of the following is the most suitable type of scaling policy that you should use?

**Options:**
- A. Scheduled Scaling
- B. Step Scaling
- C. Target Tracking Scaling
- D. Simple Scaling

<details>

<summary><strong>Answer & Explanation</strong> 📝</summary>

🟢 **With step scaling**, you choose **scaling metrics** and **threshold values** for the **CloudWatch alarms** that trigger the scaling process, as well as define how your **scalable target** should be scaled when a threshold is in breach for a specified number of evaluation periods.

Step scaling policies **increase or decrease the current capacity** of a scalable target based on a set of scaling adjustments, known as **step adjustments**. The adjustments vary based on the **size of the alarm breach**.

After a scaling activity is started, the policy continues to respond to additional alarms, even while a scaling activity is in progress. Therefore, all alarms that are breached are evaluated by **Application Auto Scaling** as it receives the alarm messages.

🟢 **When you configure dynamic scaling**, you must define how to scale in response to changing demand.

📌 **Example:**
You have a web application that currently runs on **two instances**, and you want the **CPU utilization** of the **Auto Scaling group** to stay at around **50%** when the load on the application changes.

This gives you **extra capacity** to handle traffic spikes without maintaining an excessive amount of idle resources.
You can configure your **Auto Scaling group** to scale automatically to meet this need.
The **policy type** determines how the scaling action is performed.

**Amazon EC2 Auto Scaling supports the following types of scaling policies:**

1. 🟢 **Target tracking scaling** – Increase or decrease the current capacity of the group based on a **target value** for a specific metric.
   This is similar to the way your **thermostat maintains temperature** – you select a temperature and the thermostat does the rest.

2. 🟢 **Step scaling** – Increase or decrease the current capacity of the group based on a **set of scaling adjustments**, known as **step adjustments**, that vary based on the **size of the alarm breach**.

3. 🟢 **Simple scaling** – Increase or decrease the current capacity of the group based on a **single scaling adjustment**.

📝 **Note:**
If you are scaling based on a **utilization metric** that increases or decreases proportionally to the number of instances in an Auto Scaling group, then it is recommended that you use **target tracking scaling policies**.
Otherwise, it is better to use **step scaling policies** instead.

✅ **Hence, the correct answer in this scenario is:**
🟢 **Step Scaling**

<img src="https://media.tutorialsdojo.com/public/as_create_stepped_group_5.png"
     alt="Auto Scaling step scaling configuration example"
     width="600" />

<span style="color:red"><strong>🔴 Target tracking scaling is incorrect</strong></span> because the target tracking scaling policy increases or decreases the current capacity of the group based on a **target value for a specific metric**, instead of a **set of scaling adjustments**.

<span style="color:red"><strong>🔴 Simple scaling is incorrect</strong></span> because the simple scaling policy increases or decreases the current capacity of the group based on a **single scaling adjustment**, instead of a **set of scaling adjustments**.

<span style="color:red"><strong>🔴 Scheduled Scaling is incorrect</strong></span> because the scheduled scaling policy is based on a **schedule** that allows you to set your own scaling schedule for **predictable load changes**.
This is **not considered** as one of the types of **dynamic scaling**.

#### 🟢 Target Tracking Scaling

Target tracking scaling is one of the simplest and most popular scaling policies in **Amazon EC2 Auto Scaling**. It works like a **thermostat** — you set a target value for a specific metric, and Auto Scaling automatically adjusts capacity to maintain that target.

Here’s how it works:

1. **Define the target metric and value** – For example, keep the average CPU utilization of all instances at **50%**.
2. **Automatic CloudWatch monitoring** – EC2 Auto Scaling continuously tracks the metric using CloudWatch alarms created behind the scenes.
3. **Automatic scaling actions** –

   * If the metric exceeds the target (e.g., CPU > 50%), Auto Scaling **adds instances** to reduce the load.
   * If the metric falls below the target (e.g., CPU < 50%), Auto Scaling **removes instances** to save cost.
4. The system dynamically scales in and out to keep the metric close to the target value — just like a thermostat maintaining room temperature.

📌 **Example:**
A web application uses target tracking to maintain 50% average CPU utilization.

* When CPU spikes to 70%, Auto Scaling automatically adds instances.
* When traffic decreases and CPU drops below 50%, it terminates extra instances.
  This ensures stable performance while minimizing cost.

📝 **Common Metrics Used:**

* **CPU Utilization (%)**
* **Request Count per Target** (for ALB-based apps)
* **Custom metrics** (e.g., queue length or latency)

✅ **Benefits:**

* Simple to configure — no need to define manual thresholds.
* Automatically maintains performance and cost balance.
* Responds quickly to real-time workload changes.
* AWS **recommends** this policy for most use cases.

#### 🟠 Simple Scaling

**Simple scaling** is one of the earliest and most straightforward scaling policies in **Amazon EC2 Auto Scaling**. It uses **CloudWatch alarms** to trigger a **single scaling action** (either scale out or scale in) whenever a metric crosses a defined threshold.

#### 💡 How It Works

1. You create a **CloudWatch alarm** that monitors a metric (e.g., CPU utilization).
2. You define **what action** to take when the alarm is triggered — for example:

   * **Add 2 instances** when CPU > 70% for 2 consecutive periods.
   * **Remove 1 instance** when CPU < 30% for 5 consecutive periods.
3. After a scaling activity happens, Auto Scaling **waits for a cooldown period** (default: 300 seconds) before responding to another alarm.

   * This avoids launching or terminating instances too frequently.

#### 📌 Example

* Metric: **Average CPU Utilization**
* Alarm 1: If CPU > 70% for 2 minutes → **Add 2 instances**
* Alarm 2: If CPU < 30% for 5 minutes → **Remove 1 instance**

So, when your app gets more traffic and CPU goes above 70%, Auto Scaling automatically scales out.
When the traffic drops and CPU falls below 30%, it scales in.

#### 📝 Key Points

* Simple scaling is **event-driven** — it reacts to specific alarm conditions.
* It performs **one scaling action per alarm** and waits for the cooldown period before acting again.
* Best suited for **steady or predictable workloads** where scaling doesn’t need to happen rapidly or frequently.

#### ✅ Benefits

* Easy to configure and understand.
* Good for small applications with **low scaling complexity**.

#### ⚠️ Limitations

* Only one scaling action per alarm — not ideal for fast-changing workloads.
* Cooldown period can make response slower compared to **step** or **target tracking** scaling.

</details>

---

## **Question 5**

**A major TV network has a web application running on eight Amazon T3 EC2 instances behind an application load balancer. The number of requests that the application processes are consistent and do not experience spikes. A Solutions Architect must configure an Auto Scaling group for the instances to ensure that the application is running at all times.**

Which of the following options can satisfy the given requirements?

**Options:**

* Deploy four EC2 instances with Auto Scaling in one region and four in another region behind an Amazon Elastic Load Balancer.
* Deploy eight EC2 instances with Auto Scaling in one Availability Zone behind an Amazon Elastic Load Balancer.
* Deploy two EC2 instances with Auto Scaling in four regions behind an Amazon Elastic Load Balancer.
* Deploy four EC2 instances with Auto Scaling in one Availability Zone and four in another Availability Zone in the same region behind an Amazon Elastic Load Balancer.

<details>

<summary><strong>Answer & Explanation</strong> 📝</summary>

🟢 **The best option to take** is to **deploy four EC2 instances in one Availability Zone and four in another availability zone in the same region behind an Amazon Elastic Load Balancer.** In this way, if one availability zone goes down, there is still another available zone that can accommodate traffic.

When the first AZ goes down, the second AZ will only have an initial **4 EC2 instances.** This will eventually be scaled up to **8 instances** since the solution is using **Auto Scaling.**

The **110% compute capacity for the 4 servers** might cause some degradation of the service but not a total outage since there are still some instances that handle the requests. Depending on your **scale-up configuration** in your **Auto Scaling group**, the additional **4 EC2 instances** can be launched in a matter of minutes.

**T3 instances** also have a **Burstable Performance** capability to burst or go beyond the current compute capacity of the instance to higher performance as required by your workload. So your **4 servers** will be able to manage **110% compute capacity** for a short period of time. This is the power of cloud computing versus our on-premises network architecture. It provides **elasticity** and **unparalleled scalability.**

Take note that **Auto Scaling** will launch additional **EC2 instances** to the remaining **Availability Zone/s** in the event of an **Availability Zone outage** in the region. Hence, **the correct answer is the option that says: Deploy four EC2 instances with Auto Scaling in one Availability Zone and four in another availability zone in the same region behind an Amazon Elastic Load Balancer.**

<span style="color:red"><strong>🔴 The option that says: Deploy eight EC2 instances with Auto Scaling in one Availability Zone behind an Amazon Elastic Load Balancer is incorrect</strong></span> because this architecture is **not highly available.** If that Availability Zone goes down, then your web application will be unreachable.

<span style="color:red"><strong>🔴 The options that say: Deploy four EC2 instances with Auto Scaling in one region and four in another region behind an Amazon Elastic Load Balancer and Deploy two EC2 instances with Auto Scaling in four regions behind an Amazon Elastic Load Balancer are incorrect</strong></span> because the **ELB is designed to only run in one region and not across multiple regions.

🟢 **The default termination policy** is designed to help ensure that your **network architecture spans Availability Zones evenly.**
With the **default termination policy**, the behavior of the **Auto Scaling group** is as follows:

1️⃣ **If there are instances in multiple Availability Zones**, choose the **Availability Zone with the most instances** and at least one instance that is **not protected from scale in.**
If there is more than one Availability Zone with this number of instances, choose the **Availability Zone with the instances that use the oldest launch template.**

2️⃣ **Determine which unprotected instances** in the selected Availability Zone use the **oldest launch template.**
If there is one such instance, **terminate it.**

3️⃣ If there are **multiple instances** to terminate based on the above criteria, determine which unprotected instances are **closest to the next billing hour.**
💡 *(This helps you maximize the use of your EC2 instances and manage your Amazon EC2 usage costs.)*
If there is one such instance, **terminate it.**

4️⃣ If there is **more than one unprotected instance** closest to the next billing hour, choose **one of these instances at random.**

<img src="https://media.tutorialsdojo.com/ASG-default-policy-evaluation-flowchart.png"
     alt="Auto Scaling default termination policy evaluation flowchart"
     width="600" />

🟢 **Hence, the correct answer is:**
**The EC2 instance launched from the oldest launch template.**

<span style="color:red"><strong>🔴 The option that says: The EC2 instance which has the least number of user sessions is incorrect</strong></span> because the **number of user sessions** is **not typically a factor** considered by Amazon EC2 Auto Scaling groups when deciding which instances to terminate during a **scale-in event.**

<span style="color:red"><strong>🔴 The option that says: The EC2 instance which has been running for the longest time is incorrect</strong></span> because the **duration** for which an EC2 instance has been running is **not a primary factor** considered by Amazon EC2 Auto Scaling when deciding which instances to terminate.

<span style="color:red"><strong>🔴 The option that says: The instance will be randomly selected by the Auto Scaling group is incorrect</strong></span> because **Amazon EC2 Auto Scaling groups do not randomly select instances** for termination during a scale-in event — random selection only occurs **if all other conditions are equal.**

</details>

---
