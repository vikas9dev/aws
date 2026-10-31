# EC2 SAA Level - Handnote 📝

## 🌐 IP ADDRESSES

### IPv4 vs IPv6
- **IPv4**: Most common (e.g., `192.168.1.1`)
- **IPv6**: Less common, longer format
- **IPv4 Range**: 0-255 per number (~3.7 billion addresses)

### Public IP Addresses
- ✅ **Accessible over internet**
- ✅ **Must be unique** globally
- ✅ **Geolocation** can be found
- ✅ **EC2 instances** can have public IPs

### Private IP Addresses
- ✅ **Internal network only**
- ✅ **NAT device** needed for internet access
- ✅ **Unique within private network** only
- ✅ **Different networks** can use same private IPs

### Elastic IPs
- ✅ **Static public IPv4** address
- ✅ **Owned by you** (until deleted)
- ✅ **Attach to one instance** at a time
- ⚠️ **Limit**: 5 per AWS account (default)
- 💡 **Tip**: Avoid if possible (use DNS/Load Balancer instead)

### EC2 Instance IPs
- **Private IP**: Internal AWS network
- **Public IP**: Internet access
- ⚠️ **Public IP changes** on stop/start (unless Elastic IP)
- ✅ **SSH**: Use public IP from outside AWS

---

## 🔄 IP ADDRESS BEHAVIOR

### Stop/Start Impact
- ⚠️ **Public IP changes** when instance stopped/started
- ✅ **Elastic IP**: Remains same after stop/start
- ✅ **Private IP**: Usually remains same

### Elastic IP Pricing 💰
- **Cost**: ~$0.005/hour (~$3.60/month) for every Elastic IP, attached or not (since 1 Feb 2024)
- ⚠️ Older material says an EIP is free when attached to a running instance — no longer true
- **Free Tier**: 750 hours/month free public IPv4

---

## 📍 EC2 PLACEMENT GROUPS

### Cluster Placement Group
- ✅ **Low latency**: Same AZ, same rack
- ✅ **High throughput**: ~10 Gbps (with enhanced networking)
- ⚠️ **Risk**: All instances fail if AZ fails
- **Use Cases**: Big data, HPC, low-latency apps
- ⚠️ **Limitation**: Not all instance types supported (no T2)

### Spread Placement Group
- ✅ **Maximize availability**: Different hardware
- ✅ **Multiple AZs**: Spans across AZs
- ⚠️ **Limit**: 7 instances per AZ (max)
- **Use Cases**: Critical apps, high availability

### Partition Placement Group
- ✅ **Multiple partitions**: Physical racks
- ✅ **Isolated failures**: Partitions isolated
- ✅ **Scale**: Up to 100s of instances
- ✅ **7 partitions per AZ** (max)
- **Use Cases**: HDFS, HBase, Cassandra, Kafka
- **Metadata**: Access partition info via metadata service

---

## 🔌 ELASTIC NETWORK INTERFACES (ENI)

### What is ENI?
- **Virtual network card** in VPC
- **Network access** for EC2 instances
- **Attributes**:
  1. Primary private IPv4
  2. Secondary IPv4 addresses
  3. One Elastic IP per private IPv4
  4. One Public IPv4
  5. Security groups
  6. MAC address

### ENI Features
- ✅ **Create independently** from EC2
- ✅ **Attach/detach** on the fly
- ✅ **Move between instances** (failover)
- ⚠️ **AZ-bound**: Created in specific AZ, can only use in that AZ

### Use Cases
- **Failover scenarios**: Move IP between instances
- **Network management**: Control private IPs
- **High availability**: Quick network failover

---

## 💤 EC2 HIBERNATE

### What is Hibernate?
- ✅ **Preserves RAM state** to EBS
- ✅ **Fast boot**: OS not restarted, just resumed
- ✅ **RAM dumped** to encrypted EBS volume
- ✅ **Resume**: Loads RAM from EBS back to instance

### Requirements
- ✅ **Instance RAM**: Must be < 150 GB
- ❌ **Bare metal**: Not supported
- ✅ **OS**: Linux and Windows
- ✅ **Root volume**: Must be EBS (not instance store)
- ✅ **Root volume**: Must be encrypted
- ✅ **Root volume**: Large enough for RAM dump
- ✅ **Instance types**: On-Demand, Reserved, Spot

### Use Cases
- ✅ **Long-running processes**: Don't want to stop
- ✅ **Fast boot**: Services take long to initialize
- ✅ **State preservation**: Save RAM state

### Limitations
- ⚠️ **Hibernation period**: Max 60 days
- ⚠️ **Public IP**: Changes after hibernate/start (unless Elastic IP)

---

## ⚠️ CRITICAL EXAM POINTS

1. **Public IP**: Changes on stop/start (unless Elastic IP)
2. **Elastic IP**: Static, but limit of 5 per account
3. **Placement Groups**: Cluster (low latency), Spread (availability), Partition (scale)
4. **Cluster Placement**: Not all instance types supported (no T2)
5. **Spread Placement**: Max 7 instances per AZ
6. **Partition Placement**: Up to 7 partitions per AZ, 100s of instances
7. **ENI**: AZ-bound, can move between instances
8. **Hibernate**: RAM < 150GB, encrypted EBS root, max 60 days
9. **Hibernate IP**: Public IP changes (unless Elastic IP)

---

## 📋 QUICK REFERENCE

### Placement Group Selection
- **Low latency/HPC**: Cluster Placement Group
- **High availability**: Spread Placement Group
- **Large scale (HDFS/Cassandra)**: Partition Placement Group

### IP Address Strategy
- **Static IP needed**: Elastic IP (but prefer DNS/Load Balancer)
- **Temporary access**: Use random public IP with DNS
- **Best practice**: Use Load Balancer instead of Elastic IPs

### Hibernate Checklist
- ✅ RAM < 150 GB
- ✅ EBS root volume
- ✅ Encrypted root volume
- ✅ Root volume large enough for RAM
- ✅ Not bare metal instance

---

*Last Updated: Based on AWS SAA-C03 Exam Guide*

