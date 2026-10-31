# AWS EC2 Fundamentals - Handnote 📝

## 💻 EC2 BASICS

### What is EC2?
- **Elastic Compute Cloud** - Virtual servers in AWS
- **Infrastructure as a Service (IaaS)**
- Collection of services: Instances, EBS, ELB, ASG

### EC2 Instance Options
- **Operating System**: Linux, Windows, macOS
- **CPU**: Number of vCPUs
- **Memory (RAM)**: Amount of memory
- **Storage**: EBS, EFS, Instance Store
- **Network**: Network card, public IP
- **Firewall**: Security Groups
- **Bootstrap**: EC2 User Data script

---

## 🚀 EC2 USER DATA

### Purpose
- **Bootstrap script** runs on first launch
- **Runs only once** at instance start
- **Root privileges**

### Common Tasks
- Install updates
- Install software
- Download files
- Configure applications

### Example
```bash
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Hello World</h1>" > /var/www/html/index.html
```

### Logs Location
- **Linux**: `/var/log/cloud-init-output.log`
- **Windows**: `C:\ProgramData\Amazon\EC2-Windows\Launch\Log\UserdataExecution.log`
- **Console**: EC2 → Instances → Actions → Get system log

---

## 🖥️ EC2 INSTANCE TYPES

### Naming Convention
- **Format**: `m5.2xlarge`
- **m** = Instance class
- **5** = Generation
- **2xlarge** = Size

### Instance Classes

#### General Purpose (M, T, Mac)
- **Balanced** compute, memory, networking
- **Use cases**: Web servers, code repositories
- **Examples**: `t2.micro`, `m5.large`

#### Compute Optimized (C)
- **High CPU** performance
- **Use cases**: Batch processing, media transcoding, HPC, ML, game servers
- **Examples**: `c5.xlarge`, `c6i.2xlarge`

#### Memory Optimized (R, X, Z)
- **High RAM** for large datasets in memory
- **Use cases**: Databases, in-memory caches, BI, real-time big data
- **Examples**: `r5.xlarge`, `x1e.32xlarge`

#### Storage Optimized (I, D, H)
- **High IOPS** for local storage
- **Use cases**: NoSQL databases, data warehousing, file systems
- **Examples**: `i3.xlarge`, `d2.2xlarge`

#### Accelerated Computing (P, G, F, Inf)
- **GPUs** or specialized hardware
- **Use cases**: ML, graphics, video processing
- **Examples**: `p3.2xlarge`, `g4dn.xlarge`

### Burstable Performance (T)
- **Baseline performance** with ability to burst
- **CPU Credits**: Earn when idle, spend when busy
- **Unlimited mode**: Pay extra for sustained high CPU

---

## 🔐 SECURITY GROUPS

### Features
- **Virtual firewall** for EC2 instances
- **Stateful**: Return traffic automatically allowed
- **Default**: Deny all inbound, Allow all outbound
- **Can attach multiple** security groups to instance

### Rules
- **Type**: Protocol (TCP, UDP, ICMP, etc.)
- **Port Range**: Specific port or range
- **Source**: IP (CIDR) or Security Group
- **Allow/Deny**: Allow or Deny traffic

### Best Practices
- ✅ **Least privilege** - only necessary ports
- ✅ **Reference security groups** instead of IPs when possible
- ✅ **Separate security groups** for different tiers (web, app, DB)

---

## 🔑 EC2 INSTANCE CONNECT

### Purpose
- **Browser-based SSH** access
- **No SSH key** needed
- **Temporary credentials**

### How It Works
1. Click "Connect" in EC2 Console
2. Select "EC2 Instance Connect"
3. Click "Connect" (opens browser-based terminal)

### Requirements
- ✅ **SSM Agent** must be running
- ✅ **IAM role** with `AmazonEC2RoleforSSM` policy
- ✅ **Security Group** allows SSH (port 22)

---

## 💰 EC2 PURCHASING OPTIONS

### On-Demand Instances
- ✅ **Pay per second** (no commitment)
- ✅ **No upfront costs**
- ✅ **Use cases**: Short-term, unpredictable workloads
- ⚠️ **Most expensive** option

### Reserved Instances
- ✅ **Up to 72% discount** vs On-Demand
- ✅ **1 or 3 year** commitment
- ✅ **Payment options**: All Upfront, Partial Upfront, No Upfront
- ✅ **Types**: Standard, Convertible (Scheduled RIs can no longer be bought — AWS has no plans to offer them again)

### Savings Plans
- ✅ **Up to 72% discount**
- ✅ **Flexible** (EC2, Lambda, Fargate)
- ✅ **1 or 3 year** commitment
- ✅ **Compute Savings Plans**: Most flexible

### Spot Instances
- ✅ **Up to 90% discount**
- ⚠️ **Can be terminated** by AWS with 2-minute notice
- ✅ **Use cases**: Fault-tolerant, flexible workloads
- ✅ **Spot Fleet**: Automatically launch best Spot instances

### Dedicated Instances
- ✅ **Physical server** dedicated to you
- ⚠️ **No instance placement** control (placement control is a Dedicated Host feature)
- ✅ **Compliance** requirements

### Dedicated Hosts
- ✅ **Physical server** with full control
- ✅ **Bring Your Own License (BYOL)**
- ✅ **Compliance** requirements
- ⚠️ **Most expensive** option

---

## ⚡ SPOT INSTANCES

### How It Works
1. Optionally set a **maximum price** (default = On-Demand price)
2. Instance runs while AWS has spare capacity and spot price < your max price
3. AWS can interrupt it (2-min notice) — mainly when it **needs the capacity back**, or if spot price > your max price

### Spot Instance Interruption
- **2-minute warning** via instance metadata or CloudWatch Events
- **Spot Instance Request** can be stopped/terminated

### Spot Fleet
- **Automatically launch** best Spot instances
- **Multiple instance types** & AZs
- **Target capacity** maintained automatically
- **Strategies**: LowestPrice, Diversified, CapacityOptimized

### Use Cases
- ✅ **Fault-tolerant** workloads
- ✅ **Flexible** start/end times
- ✅ **Cost-sensitive** applications
- ✅ **Big data**, containerized workloads

---

## 💾 EC2 INSTANCE STORAGE

### EBS Volumes
- **Network-attached** storage
- **Persistent** (survives instance termination if not deleted)
- **Types**: gp2, gp3, io1, io2, st1, sc1
- **Can attach/detach** while instance running

### Instance Store
- **Ephemeral** storage (lost on stop/terminate)
- **Physically attached** to host
- **Higher IOPS** than EBS
- **Use cases**: Cache, temporary data, buffers

### EFS
- **Network file system** (NFS)
- **Shared** across multiple instances
- **Scales automatically**
- **Use cases**: Content management, web serving, data sharing

---

## 🌐 ELASTIC IP ADDRESSES

### Features
- **Static public IP** address
- **Can remap** to different instances
- **$0.005/hour** for every Elastic IP, attached or not (since 1 Feb 2024)
- ⚠️ **Not free** when attached to a running instance any more (older material says it is)

### Use Cases
- **Fixed IP** for applications
- **Failover** scenarios
- **DNS** pointing to static IP

---

## 📊 EC2 PLACEMENT GROUPS

### Cluster Placement Group
- **Low latency** (same AZ, same rack)
- **High network throughput**
- **Use cases**: HPC, big data

### Spread Placement Group
- **Maximize availability** (different hardware)
- **7 instances per AZ** (max)
- **Use cases**: Critical applications

### Partition Placement Group
- **Multiple partitions** per AZ
- **Up to 7 partitions per AZ**
- **Use cases**: Large distributed systems (HDFS, Cassandra)

---

## ⚠️ CRITICAL EXAM POINTS

1. **EC2 User Data** - Runs once on first launch, root privileges
2. **Security Groups** - Stateful, default deny inbound, allow outbound
3. **IAM Roles** - Use for EC2 (not IAM users), auto-rotate credentials
4. **Instance Types** - M (general), C (compute), R (memory), I/D (storage)
5. **Spot Instances** - Up to 90% discount, 2-min termination notice
6. **Reserved Instances** - Up to 72% discount, 1-3 year commitment
7. **Instance Store** - Ephemeral, lost on stop/terminate
8. **EBS Volumes** - Persistent, can attach/detach
9. **Elastic IP** - $0.005/hour, attached or not (since Feb 2024)
10. **Placement Groups** - Cluster (low latency), Spread (availability), Partition (large scale)

---

## 📋 QUICK REFERENCE

### Instance Type Selection
- **Web servers**: General Purpose (M, T)
- **Databases**: Memory Optimized (R)
- **HPC/ML**: Compute Optimized (C) or Accelerated (P, G)
- **Big Data**: Storage Optimized (I, D)

### Purchasing Strategy
- **Predictable workload**: Reserved Instances
- **Fault-tolerant**: Spot Instances
- **Short-term**: On-Demand
- **Compliance**: Dedicated Hosts

### Security Best Practice
```
Internet → Security Group (Allow 80, 443)
         ↓
    EC2 Instance (IAM Role for S3 access)
```

---

*Last Updated: Based on AWS SAA-C03 Exam Guide*

