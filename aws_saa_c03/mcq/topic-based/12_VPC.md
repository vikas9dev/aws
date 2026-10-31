# VPC

## **Question 1**

**A large insurance company has an AWS account that contains three VPCs (DEV, UAT and PROD) in the same region. UAT is peered to both PROD and DEV using a VPC peering connection. All VPCs have non-overlapping CIDR blocks. The company wants to push minor code releases from Dev to Prod to speed up time to market.**

**Which of the following options helps the company accomplish this?**

* Change the DEV and PROD VPCs to have overlapping CIDR blocks to be able to connect them.
* Do nothing. Since these two VPCs are already connected via UAT, they already have a connection to each other.
* Create a new VPC peering connection between PROD and DEV with the appropriate routes.
* Create a new entry to PROD in the DEV route table using the VPC peering connection as the target.

<details>
  <summary><strong>Answer & Explanation</strong> 📝</summary>

### 🟢 **Understanding VPC Peering**

A **VPC peering connection** is a **private, point-to-point network connection** between two VPCs.
It allows instances in both VPCs to communicate **as if they were in the same network**.
Key characteristics:

* Works **within the same AWS account**, **across accounts**, and even **across Regions**.
* Uses the **existing VPC infrastructure**—no gateways, VPNs, or additional hardware.
* **No single point of failure** and **no bandwidth bottleneck**.
* **CIDR blocks must NOT overlap**.
* **Transitive peering is not supported** → VPC A ↔ VPC B and VPC B ↔ VPC C does **not** mean VPC A ↔ VPC C.

Correct Option: **Create a new VPC peering connection between PROD and DEV with the appropriate routes.**

### 🛑 **Incorrect Options Explained**

<span style="color:red"><strong>#### ❌ “Add a route entry from DEV to PROD using the existing UAT peering connection”</strong></span>

Even if routes are added, **DEV and PROD cannot communicate** because **no peering exists between them**.  
Routing alone cannot override AWS’s restriction:  
➡️ **VPC peering is NOT transitive.**

<span style="color:red"><strong>#### ❌ “Change DEV and PROD to have overlapping CIDR blocks”</strong></span>

Overlapping CIDRs result in **invalid peering configurations**.  
AWS does **not allow peering** between VPCs with overlapping IP ranges because routing becomes ambiguous.

<span style="color:red"><strong>#### ❌ “Do nothing. They are already connected through UAT.”</strong></span>

This is incorrect due to the same rule:  
➡️ **Transitive VPC peering is not allowed.**  
Even if DEV ↔ UAT and PROD ↔ UAT are connected, **DEV does NOT connect to PROD** automatically.

### 🧠 **Summary**

To enable communication between DEV and PROD, you must create a **direct VPC peering connection** between them. Without it, no routing configuration will make the two VPCs communicate through UAT.

</details>

---

## **Question 2**

**A media company has two VPCs: VPC-1 and VPC-2 with peering connection between each other. VPC-1 only contains private subnets while VPC-2 only contains public subnets. The company uses a single AWS Direct Connect connection and a virtual interface to connect their on-premises network with VPC-1.**

**Which of the following options increase the fault tolerance of the connection to VPC-1? (Select TWO.)**

* Establish a new AWS Direct Connect connection and private virtual interface in the same region as VPC-2.
* Establish a hardware VPN over the Internet between VPC-1 and the on-premises network.
* Use the AWS VPN CloudHub to create a new AWS Direct Connect connection and private virtual interface in the same region as VPC-2.
* Establish another AWS Direct Connect connection and private virtual interface in the same AWS region as VPC-1.
* Establish a hardware VPN over the Internet between VPC-2 and the on-premises network.

<details>
  <summary><strong>Answer & Explanation</strong> 📝</summary>

### 🟢 **Key Concept: VPC Peering Does NOT Support Edge-to-Edge Routing**

VPC peering is a **non-transitive**, **one-to-one** network relationship.
This means **traffic cannot flow through a peered VPC to reach another network** such as:

* On-premises networks (via VPN or Direct Connect)
* Internet via IGW
* Private subnets via NAT Gateways
* AWS services via Gateway Endpoints (e.g., S3)
* ClassicLink (for IPv6) — ClassicLink was retired with EC2-Classic in Aug 2023

So even if **VPC-1 ↔ VPC-2** are peered, and **VPC-2 ↔ On-Prem** are connected (via VPN or Direct Connect),
➡️ **VPC-1 cannot route traffic through VPC-2 to reach on-premises**, and vice versa.

This is strictly disallowed because VPC peering **does not allow edge-to-edge routing**.

### 🛑 **Why the Incorrect Options Do Not Work**

All of these incorrect options assume **transitive routing through VPC-2**, which AWS forbids:

<span style="color:red"><strong>#### ❌ Use AWS VPN CloudHub with Direct Connect in VPC-2’s region</strong></span>

Still involves routing from VPC-1 → VPC-2 → On-Prem, which is NOT allowed.

<span style="color:red"><strong>#### ❌ Establish a hardware VPN between VPC-2 and on-prem</strong></span>

Same issue — VPC-1 traffic would need to route *through* VPC-2.

<span style="color:red"><strong>#### ❌ Create a new Direct Connect in the same region as VPC-2</strong></span>

This again implies routing to on-prem via VPC-2, which VPC peering cannot support.

### 🟢 **Correct Approach: Connect Directly to VPC-1**

To build a **highly available and fault-tolerant** hybrid connection, and **to ensure VPC-1 can directly reach on-prem**, you must connect **VPC-1 itself** to the corporate network:

1. **Establish a hardware VPN connection** from the on-premises network directly to **VPC-1**

   * Provides encrypted, fault-tolerant connectivity.

2. **Establish a second AWS Direct Connect (DX) connection** and **private VIF** in the same region as **VPC-1**

   * Adds redundancy and reduces dependency on VPN latency.

This configuration ensures:

* No routing through VPC-2
* No violation of VPC peering limitations
* High availability via dual hybrid connections

<img src="https://media.tutorialsdojo.com/edge-to-edge-vpn-diagram.png"
     alt="Edge-to-edge VPN and VPC peering diagram"
     width="600" />


### 🧠 **Summary**

VPC peering **cannot** be used to “pass through” another VPC to reach external networks.
To allow on-premises connectivity to VPC-1:

✔ Create **direct** VPN and/or Direct Connect links **to VPC-1**
❌ Do not route through VPC-2

This meets the requirement for **high availability**, **fault tolerance**, and **AWS peering design constraints**.

</details>

---

## **Question 3**

**A company has multiple VPCs with IPv6 enabled for its suite of web applications. The Solutions Architect attempted to deploy a new Amazon EC2 instance but encountered an error indicating that there were no available IP addresses on the subnet. The VPC has a combination of IPv4 and IPv6 CIDR blocks, but the IPv4 CIDR blocks are nearing exhaustion. The architect needs a solution that will resolve this issue while allowing future scalability.**

**How should the Solutions Architect resolve this problem?**

* Ensure that the VPC has IPv6 CIDRs only. Remove any IPv4 CIDRs associated with the VPC.
* Disable the IPv4 support in the VPC and use the available IPv6 addresses.
* Set up a new IPv4 subnet with a larger CIDR range. Associate the new subnet with the VPC and then launch the instance.
* Set up a new IPv6-only subnet with a large CIDR range. Associate the new subnet with the VPC then launch the instance.

<details>
  <summary><strong>Answer & Explanation</strong> 📝</summary>

### 🟢 **Core Concept: IPv4 Exhaustion & Dual-Stack VPCs**

An Amazon VPC is fundamentally an **IPv4-based network**, and all VPCs must include at least one **IPv4 CIDR block**. However, because IPv4 space is limited—and your company’s IPv4 CIDR blocks are nearly exhausted—the best long-term solution is to leverage **IPv6**, which provides a massive address pool and supports future scalability.

Since the VPC is already **IPv6-enabled**, the architect can avoid using scarce IPv4 addresses by creating an **IPv6-only subnet**.
AWS allows **IPv6-only subnets within a dual-stack VPC**, and EC2 instances launched in these subnets can use IPv6 exclusively.

This resolves the immediate problem (IPv4 exhaustion) **and** provides a scalable path moving forward.

### 🟢 **Correct Answer**

**Set up a new IPv6-only subnet with a large CIDR range, associate it with the VPC, and then launch the instance.**

✔ Uses abundant IPv6 address space
✔ Conserves remaining IPv4 space
✔ Supports long-term growth
✔ Fully supported in a dual-stack VPC


<img src="https://media.tutorialsdojo.com/Amazon_VPC_IPv6.png"
     alt="Amazon VPC IPv6 architecture"
     width="600" />

### 🔴 **Incorrect Options Explained**

<span style="color:red"><strong>#### ❌ Create a new IPv4 subnet with a larger CIDR range</strong></span>

* IPv4 space is limited; this is only a **temporary fix**.  
* Does not address the root issue: **long-term IPv4 exhaustion**.  
* Fails the “future scalability” requirement.  

The option that says: Set up a new IPv4 subnet with a larger CIDR range. Associate the new subnet with the VPC and then launch the instance is incorrect because it is not a scalable, long-term solution. While creating a new IPv4 subnet would temporarily solve the immediate problem of address exhaustion, it does not address the fundamental issue of the limited IPv4 address space. The company would eventually face the same problem again. This approach fails to meet the requirement for future scalability and is a temporary fix rather than a sustainable strategy.

<span style="color:red"><strong>#### ❌ Ensure the VPC has IPv6 CIDRs only and remove IPv4 CIDRs</strong></span>

* This is **not possible**.  
* All VPCs **must include an IPv4 CIDR block**, even when using IPv6.  
* You **cannot remove** all IPv4 CIDRs from a VPC.

<span style="color:red"><strong>#### ❌ Disable IPv4 support and use IPv6 exclusively</strong></span>

* IPv4 **cannot be disabled** on a VPC.  
* Many AWS services still rely on IPv4.  
* Disabling IPv4 would break existing workloads and is not supported by AWS.

### 🧠 **Summary**

Since the VPC is already IPv6-enabled, the architect should leverage IPv6 to overcome IPv4 exhaustion. AWS supports **IPv6-only subnets** in dual-stack VPCs, making this the **most scalable, future-proof, and technically correct solution**.

</details>

---
