# AWS Storage Extras - Handnote 📝

## 💾 EBS (Elastic Block Store)

### What is EBS?
- **Network-attached** storage for EC2
- **Persistent** block storage
- **Can attach/detach** while instance running
- **Scoped to AZ** (cannot attach across AZs)

### EBS Volume Types

#### gp3 (General Purpose SSD)
- ✅ **Latest generation** (recommended)
- ✅ **3,000 IOPS** baseline (up to **80,000 IOPS**; was 16,000 before Sep 2025)
- ✅ **125 MB/s** baseline throughput (up to **2,000 MiB/s**; was 1,000 MB/s)
- ✅ Size up to **64 TiB** (was 16 TiB)
- ✅ **$0.08/GB-month**

#### gp2 (General Purpose SSD)
- ✅ **3 IOPS per GB** (min 100, max 16,000)
- ✅ **Burst to 3,000 IOPS** (credit system)
- ✅ **$0.10/GB-month**

#### io1/io2 (Provisioned IOPS SSD)
- ✅ **io1: up to 64,000 IOPS**. **io2** is Block Express for all volumes since Apr 2025: up to **256,000 IOPS**, 4,000 MiB/s, 64 TiB, 99.999% durability
- ✅ **Consistent low latency**
- ✅ **Use cases**: Databases, I/O-intensive workloads
- ✅ **io2**: More durable, same price as io1

#### st1 (Throughput Optimized HDD)
- ✅ **500 IOPS**, **500 MB/s** throughput
- ✅ **Use cases**: Big data, data warehouses, log processing
- ✅ **Cannot be boot volume**

#### sc1 (Cold HDD)
- ✅ **250 IOPS**, **250 MB/s** throughput
- ✅ **Lowest cost** HDD
- ✅ **Use cases**: Throughput-oriented, infrequently accessed
- ✅ **Cannot be boot volume**

### EBS Features
- **Snapshots**: Point-in-time backups (incremental)
- **Encryption**: KMS encryption (at rest & in transit)
- **Multi-Attach**: Attach to multiple instances (io1/io2 only)
- **Fast Snapshot Restore**: Instant access to snapshots

---

## 📁 EFS (Elastic File System)

### What is EFS?
- **Managed NFS** (Network File System)
- **Shared file storage** across multiple EC2 instances
- **Scales automatically**
- **Multi-AZ** by default

### EFS Performance Modes
- **General Purpose**: Low latency, web servers
- **Max I/O**: Higher latency, higher throughput, big data (AWS now recommends General Purpose; Elastic throughput and One Zone file systems do not support Max I/O)

### EFS Throughput Modes
- **Elastic** (default and recommended): Scales up and down with the workload, pay for what you use
- **Bursting**: Scales with file system size
- **Provisioned**: Set throughput regardless of size

### EFS Storage Classes
- **Standard**: Frequently accessed
- **Infrequent Access (IA)**: 90% cost savings, lifecycle management

### EFS Features
- ✅ **Multi-AZ** (default)
- ✅ **POSIX-compliant**
- ✅ **On-premises access** via VPN/Direct Connect
- ✅ **Encryption**: At rest & in transit

### EFS Pricing
- **Storage**: $0.30/GB-month (Standard), $0.016/GB-month (IA, cut from $0.025 in Nov 2023), $0.008/GB-month (Archive)
- **Throughput**: Bursting (included), Provisioned ($6/MBps-month)

---

## 🗄️ EBS vs EFS vs FSx

| Feature | EBS | EFS | FSx |
|---------|-----|-----|-----|
| **Type** | Block storage | File storage | File storage |
| **Access** | Single EC2 | Multiple EC2 | Multiple EC2/on-prem |
| **Protocol** | Block | NFS | NFS, SMB, Lustre |
| **Use Case** | Boot volumes, databases | Shared files | Windows, Lustre HPC |

---

## 🪣 AWS STORAGE GATEWAY

### What is Storage Gateway?
- **Hybrid cloud storage**
- **Bridge** on-premises & AWS
- **Virtual appliances** or hardware

### Gateway Types

#### File Gateway
- **NFS/SMB** protocol
- **Files stored in S3**
- ✅ **Use case**: File shares in cloud

#### Volume Gateway
- **iSCSI** protocol
- **Stored Volumes**: All data on-premises, async backup to S3
- **Cached Volumes**: Frequently accessed data cached locally, all data in S3
- ✅ **Use case**: Backup, disaster recovery

#### Tape Gateway
- **VTL (Virtual Tape Library)**
- **Backup to S3/Glacier**
- ✅ **Use case**: Replace physical tape libraries

---

## 📦 AWS SNOW FAMILY

### Snowball Edge
- **Physical device** for data transfer
- **210 TB** (Storage Optimized) or **28 TB** NVMe (Compute Optimized). The older 80 TB / 42 TB models were discontinued in Nov 2024
- ⚠️ Closed to new customers since Nov 7, 2025 (AWS suggests DataSync or AWS Data Transfer Terminal)
- **Use cases**: Large data transfers, offline processing

### Snowmobile
- ⚠️ **Retired in 2024** — keep only for older exam questions
- **Exabyte-scale** data transfer
- **45-foot shipping container**
- **Up to 100 PB** per Snowmobile
- ✅ **Use case**: Massive data center migrations

### Use Cases
- **Large data transfers** (faster than internet)
- **Offline processing** (compute on device)
- **Data center migrations**

---

## ⚠️ CRITICAL EXAM POINTS

1. **EBS**: Block storage, single EC2, AZ-scoped
2. **EFS**: File storage, multiple EC2, multi-AZ, NFS
3. **gp3**: Latest, recommended, 3,000 IOPS baseline
4. **io1/io2**: High IOPS, databases, multi-attach support
5. **EBS Snapshots**: Incremental, cross-region copy
6. **EFS**: POSIX-compliant, on-premises access via VPN/DX
7. **Storage Gateway**: Hybrid cloud, on-premises to AWS
8. **Snowball**: Physical data transfer, large datasets
9. **EBS Encryption**: KMS encryption, at rest & in transit
10. **EFS Performance Modes**: General Purpose (low latency) vs Max I/O (high throughput)

---

## 📋 QUICK REFERENCE

### Choosing Storage
- **Boot volumes, databases**: EBS
- **Shared files, web content**: EFS
- **Windows file shares**: FSx for Windows
- **HPC workloads**: FSx for Lustre
- **Hybrid cloud**: Storage Gateway
- **Large data transfer**: Snowball

### EBS Volume Selection
- **General purpose**: gp3 (recommended) or gp2
- **High IOPS databases**: io1/io2
- **Throughput workloads**: st1
- **Infrequent access**: sc1

---

*Last Updated: Based on AWS SAA-C03 Exam Guide*

