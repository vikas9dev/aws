# EC2 Instance Storage - Handnote 📝

## 💾 EBS VOLUMES

### What is EBS?
- **Elastic Block Store** - Network-attached storage
- **Persistent**: Survives instance termination (if not deleted)
- **Network drive**: Not physical drive
- **AZ-bound**: Locked to specific Availability Zone

### Key Characteristics
- ✅ **Attach/detach** while instance running
- ✅ **One instance** at a time (CCP level)
- ✅ **Multiple volumes** per instance
- ⚠️ **AZ-bound**: Cannot attach across AZs
- ✅ **Provision capacity**: GB and IOPS
- ✅ **Billed** for provisioned capacity

### Delete on Termination
- **Root Volume**: ✅ Enabled by default
- **Additional Volumes**: ❌ Disabled by default
- **Control**: Can change during launch
- **Use Case**: Preserve root volume data after termination

### Moving Across AZs
1. **Take snapshot** of EBS volume
2. **Restore snapshot** in target AZ
3. **Attach** to instance in new AZ

---

## 📸 EBS SNAPSHOTS

### What are Snapshots?
- **Backup** of EBS volume at point in time
- ✅ **Recommended**: Detach volume before snapshot (not required)
- ✅ **Copy across AZs/Regions**: Via snapshots
- ✅ **Restore**: Create new volume from snapshot

### Snapshot Features

#### EBS Snapshot Archive
- ✅ **75% cheaper** storage tier
- ⚠️ **Restore time**: 24-72 hours
- **Use Case**: Long-term archival

#### Recycle Bin
- ✅ **Accidental deletion protection**
- ✅ **Retention period**: 1 day to 1 year
- ✅ **Recover**: Restore deleted snapshots

#### Fast Snapshot Restore (FSR)
- ✅ **No latency** on first use
- ✅ **Full initialization** forced
- ⚠️ **Expensive**: Use judiciously
- **Use Case**: Large snapshots needing quick initialization

---

## 🖼️ AMAZON MACHINE IMAGES (AMIs)

### What are AMIs?
- **Customization** of EC2 instance
- **Contains**: OS, software, monitoring tools
- **Benefits**:
  - ✅ Faster boot times
  - ✅ Faster configuration
  - ✅ Prepackaged software

### AMI Types
1. **Public AMIs**: AWS-provided (e.g., Amazon Linux 2)
2. **Your AMIs**: Self-created and maintained
3. **AWS Marketplace**: Third-party vendors (may cost)

### AMI Creation Process
1. Start EC2 instance
2. Customize instance
3. **Stop instance** (for data integrity)
4. Build AMI (creates EBS snapshots)
5. Launch new instances from AMI

### AMI Features
- ✅ **Region-specific**: Can copy across regions
- ✅ **Cross-AZ**: Launch in different AZ from same region
- ✅ **Backup**: EBS snapshots created automatically

---

## 💿 EC2 INSTANCE STORE

### What is Instance Store?
- **Ephemeral storage**: Physically attached to server
- **High performance**: Better I/O than EBS
- ⚠️ **Data loss**: Lost on stop/terminate
- **Use Cases**: Buffers, caches, scratch data, temporary content

### Performance Comparison
- **I3 Instance Store**: Millions of IOPS
- **EBS gp2**: Tens of thousands of IOPS
- **Use Case**: Very high-performance hardware-attached storage

---

## 📊 EBS VOLUME TYPES

### General Purpose SSD

#### gp3 (Newer)
- **Baseline**: 3,000 IOPS, 125 MB/s
- **Scalable**: Up to 80,000 IOPS, 2,000 MiB/s, 64 TiB (since Sep 2025; was 16,000 IOPS, 1,000 MB/s, 16 TiB)
- ✅ **Independent**: IOPS and throughput scalable separately

#### gp2 (Older)
- **Burst**: Up to 3,000 IOPS (small volumes)
- **Linked**: 3 IOPS per GB
- **Max**: 16,000 IOPS (at 5,334 GB)
- ⚠️ **Coupled**: IOPS linked to volume size

### Provisioned IOPS SSD

#### io1
- **Size**: 4 GB to 16 TB
- **Max IOPS**: 64,000 (Nitro), 32,000 (others)
- ✅ **Independent**: IOPS independent of size
- **Use Case**: Mission-critical databases

#### io2 Block Express
- **Size**: 4 GB to 64 TB
- **Max IOPS**: 256,000
- **All io2 = Block Express** since Apr 2025 (99.999% durability, 4,000 MiB/s)
- **Latency**: Sub-millisecond
- **Ratio**: 1,000:1 IOPS to GB
- ✅ **Multi-attach**: Supported

### Throughput Optimized HDD (st1)
- **Size**: 125 GB to 16 TB
- **Max Throughput**: 500 MB/s
- **Max IOPS**: 500
- ❌ **Not boot volume**
- **Use Case**: Big data, data warehousing, log processing

### Cold HDD (sc1)
- **Size**: 125 GB to 16 TB
- **Max Throughput**: 250 MB/s
- **Max IOPS**: 250
- ❌ **Not boot volume**
- **Use Case**: Archive data, lowest cost

### Boot Volumes
- ✅ **gp2, gp3, io1, io2**: Can be boot volumes
- ❌ **st1, sc1**: Cannot be boot volumes

---

## 🔗 EBS MULTI-ATTACH

### Features
- ✅ **Same volume** to multiple EC2 instances
- ✅ **Same AZ** only
- ✅ **io1/io2 only**: Exclusive to these families
- ✅ **Concurrent R/W**: All instances have full access
- ⚠️ **Limit**: Max 16 EC2 instances per volume
- ⚠️ **File System**: Must use cluster-aware file system

### Use Cases
- ✅ **High availability**: Clustered Linux apps (e.g., Teradata)
- ✅ **Concurrent writes**: Applications managing concurrent operations

---

## 🔒 EBS ENCRYPTION

### Benefits
- ✅ **Data at rest**: Encrypted in volume
- ✅ **Data in transit**: Encrypted between instance and volume
- ✅ **Snapshots**: Encrypted automatically
- ✅ **Volumes from snapshots**: Encrypted automatically
- ✅ **Transparent**: Handled by EC2/EBS
- ✅ **KMS**: Uses AES-256 keys

### Encrypting Unencrypted Volume
1. Create **snapshot** of unencrypted volume
2. **Copy snapshot** with encryption enabled
3. Create **new volume** from encrypted snapshot
4. **Attach** encrypted volume to instance

---

## 📁 AMAZON EFS (ELASTIC FILE SYSTEM)

### What is EFS?
- **Managed NFS**: Network File System
- ✅ **Multiple instances**: Mount on many EC2 instances
- ✅ **Cross-AZ**: Instances in different AZs
- ✅ **Highly available**: Multi-AZ setup
- ✅ **Scalable**: Auto-scales to petabyte
- ⚠️ **Expensive**: ~3x cost of gp2 EBS
- ✅ **Pay-per-use**: No capacity planning

### Use Cases
- Content management
- Web serving
- Data sharing
- WordPress

### Key Features
- ✅ **NFS protocol**: Standard file system
- ✅ **Security Groups**: Access control
- ✅ **Linux only**: Not Windows compatible
- ✅ **KMS encryption**: At rest encryption
- ✅ **POSIX**: Standard file API
- ✅ **Auto-scaling**: No capacity planning

### Performance Modes
1. **General Purpose** (Default): Low latency (web servers, CMS)
2. **Max I/O**: High throughput, higher latency (big data, media) — previous generation; AWS recommends General Purpose; not with Elastic or One Zone

### Throughput Modes
1. **Bursting**: Baseline + burst (1 TB = 50 MBps + bursts to 100 MBps)
2. **Provisioned**: Set specific throughput (decouples from storage)
3. **Elastic**: Auto-scales (up to 20–60 GiBps reads, 1–5 GiBps writes, by Region; older material says 3 / 1 GBps)

### Storage Classes
1. **Standard**: Frequently accessed
2. **EFS-IA**: Infrequent access (lower storage, retrieval cost)
3. **Archive**: Rarely accessed (cheapest)

### Availability Options
- **Standard**: Multi-AZ (production)
- **One Zone**: Single AZ (development, cheaper)

---

## 📊 EBS vs EFS vs INSTANCE STORE

| Feature | EBS | EFS | Instance Store |
|---------|-----|-----|---------------|
| **Attach to** | One instance | Multiple instances | One instance |
| **AZ-bound** | Yes | No (cross-AZ) | Yes |
| **Persistent** | Yes | Yes | No (ephemeral) |
| **Performance** | Good | Good | Excellent |
| **Cost** | Low | High (~3x EBS) | Free (included) |
| **Use Case** | Single instance | Shared storage | Cache/temp |

---

## ⚠️ CRITICAL EXAM POINTS

1. **EBS**: AZ-bound, one instance (unless multi-attach)
2. **Delete on Termination**: Root = yes, Additional = no (default)
3. **Snapshots**: Copy across AZs/regions
4. **AMIs**: Region-specific, can copy
5. **Instance Store**: Ephemeral, high performance, lost on stop/terminate
6. **gp3**: Independent IOPS/throughput scaling
7. **gp2**: IOPS linked to size (3 IOPS/GB)
8. **io2 Block Express**: 256K IOPS max, multi-attach
9. **Multi-attach**: io1/io2 only, max 16 instances, same AZ
10. **EFS**: NFS, Linux only, cross-AZ, expensive
11. **EFS Performance**: General Purpose (low latency) vs Max I/O (high throughput)

---

## 📋 QUICK REFERENCE

### Volume Type Selection
- **Boot volume**: gp2, gp3, io1, io2
- **Databases**: io1, io2 (high IOPS)
- **Big data**: st1 (high throughput)
- **Archive**: sc1 (lowest cost)
- **General purpose**: gp3 (recommended)

### Storage Selection
- **Single instance**: EBS
- **Multiple instances (Linux)**: EFS
- **High performance cache**: Instance Store
- **Shared files**: EFS

---

*Last Updated: Based on AWS SAA-C03 Exam Guide*

