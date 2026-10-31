# Disaster Recovery & Migrations - Handnote 📝

## ⚠️ DISASTER RECOVERY BASICS

### What is a Disaster?
- **Definition**: Event negatively impacting business continuity/finances
- **Recovery**: Preparing for and recovering from disasters

### Key Terminology

#### RPO (Recovery Point Objective) ⏱️
- **Definition**: Acceptable data loss (in time)
- **Determines**: Backup frequency
- **Example**: Hourly backups = 1 hour RPO

#### RTO (Recovery Time Objective) ⏳
- **Definition**: Acceptable downtime after disaster
- **Represents**: Time to restore services
- **Trade-off**: Lower RPO/RTO = Higher cost

---

## 🛡️ DISASTER RECOVERY STRATEGIES

### 1. Backup and Restore 💾
- **RPO**: High (hours/days)
- **RTO**: High (restoration time)
- **Cost**: Low (storage only)
- **Implementation**:
  - Back up to S3, S3 IA, Glacier
  - EBS/RDS snapshots
  - Restore from snapshots/AMIs
- **Use Case**: Non-critical workloads

### 2. Pilot Light 💡
- **RPO**: Lower than Backup/Restore
- **RTO**: Lower (critical systems running)
- **Cost**: Moderate
- **Implementation**:
  - Replicate critical data to RDS (always running)
  - Route 53 ready for failover
  - Recreate EC2 instances on disaster
- **Use Case**: Critical core systems

### 3. Warm Standby 🔥
- **RPO**: Lower (continuous replication)
- **RTO**: Lower (full system at minimum capacity)
- **Cost**: Higher
- **Implementation**:
  - Replicate to secondary RDS
  - ASG at minimum capacity
  - ELB ready
  - Scale up on disaster
- **Use Case**: Production workloads

### 4. Hot Site/Multi-Site 🚀
- **RPO**: Very Low (near zero)
- **RTO**: Very Low (minutes/seconds)
- **Cost**: Very High
- **Implementation**:
  - Full production in both locations
  - Active-active setup
  - Route 53 routing
- **Use Case**: Mission-critical, zero downtime

---

## 🔄 DATABASE MIGRATION SERVICE (DMS)

### What is DMS?
- **Purpose**: Migrate databases to AWS
- **Features**:
  - ✅ Resilient and self-healing
  - ✅ Source remains available during migration
  - ✅ Homogeneous migrations (same engine)
  - ✅ Heterogeneous migrations (different engines)
  - ✅ CDC (Change Data Capture) for continuous replication

### How DMS Works
- **EC2 Instance**: Runs DMS replication tasks
- **Source Endpoint**: Source database
- **Target Endpoint**: Target database
- **Replication**: One-time or ongoing (CDC)

### Sources
- On-premises: Oracle, SQL Server, MySQL, PostgreSQL, MongoDB, SAP, DB2
- AWS: RDS, Aurora, S3, DocumentDB
- Azure: Azure SQL Database

### Targets
- On-premises: Oracle, SQL Server, MySQL, PostgreSQL, SAP
- AWS: RDS, Redshift, DynamoDB, S3, OpenSearch, Kinesis, DocumentDB, Neptune

### Schema Migration
- **DMS**: Handles data migration (not full schema)
- **SCT (Schema Conversion Tool)**: Converts schema for heterogeneous migrations
- **Native Tools**: Use for homogeneous migrations (mysqldump, pg_dump)

### DMS Task Settings
1. **Do Nothing**: Assume schema exists
2. **Create Tables**: Auto-create basic tables (no keys/indexes)
3. **Drop and Recreate**: Clean load

---

## 🚚 DATA TRANSFER TO AWS

### Options for Large Data

#### AWS Snow Family
- **Snowball**: 80 TB capacity (old model — retired Nov 2025)
- **Snowball Edge**: 100 TB, compute capabilities (current Storage Optimized model: 210 TB)
- **Snowmobile**: Petabytes, truck-mounted (**retired Mar 2024** — may still appear in older exam questions)
- **Use Case**: One-time large transfers
- ⚠️ **Note (2026)**: Snow Family devices are closed to new customers since 7 Nov 2025. New customers: DataSync (online) or AWS Data Transfer Terminal (physical). Support for all Snowball devices in commercial Regions ends 31 Dec 2026

#### AWS DataSync
- **Purpose**: Move data between on-premises and AWS
- **Protocols**: NFS, SMB
- **Targets**: S3, EFS, FSx for Windows
- **Use Case**: Ongoing data synchronization

#### Direct Connect
- **Purpose**: Dedicated network connection
- **Speed**: GB/s
- **Use Case**: High-bandwidth, consistent connectivity

#### Storage Gateway
- **Hybrid Storage**: Bridge on-premises and cloud
- **Types**: File, Volume, Tape Gateway
- **Use Case**: Hybrid storage solutions

---

## 💾 AWS BACKUP

### What is AWS Backup?
- **Centralized Backup**: Unified backup service
- **Supports**: EC2, EBS, RDS, EFS, DynamoDB, Storage Gateway
- **Features**:
  - ✅ Backup plans (schedules, retention)
  - ✅ Cross-region backup
  - ✅ Cross-account backup
  - ✅ Lifecycle policies

### Backup Plans
- **Schedule**: Define backup frequency
- **Retention**: How long to keep backups
- **Lifecycle**: Move to cheaper storage over time

---

## ☁️ VMWARE CLOUD ON AWS

### What is VMware Cloud?
- **Purpose**: Run VMware workloads on AWS
- **Benefits**: 
  - ✅ Same VMware tools
  - ✅ Seamless migration
  - ✅ Hybrid cloud support
- **Use Case**: VMware to AWS migration

---

## ⚠️ CRITICAL EXAM POINTS

1. **RPO**: Data loss tolerance (backup frequency)
2. **RTO**: Downtime tolerance (recovery time)
3. **DR Strategies**: Backup/Restore → Pilot Light → Warm Standby → Hot Site
4. **DMS**: Data migration, not full schema (use SCT for schema)
5. **CDC**: Continuous replication with DMS
6. **Snow Family**: Large one-time transfers (Snowball; Snowmobile retired 2024)
7. **DataSync**: Ongoing file system synchronization
8. **Direct Connect**: Dedicated network connection
9. **AWS Backup**: Centralized backup service
10. **Lower RPO/RTO**: Higher cost

---

## 📋 QUICK REFERENCE

### DR Strategy Selection
- **Non-critical**: Backup and Restore
- **Critical core**: Pilot Light
- **Production**: Warm Standby
- **Mission-critical**: Hot Site/Multi-Site

### Data Transfer Selection
- **One-time, large**: Snowball (Snowmobile retired 2024)
- **Ongoing sync**: DataSync
- **High bandwidth**: Direct Connect
- **Hybrid storage**: Storage Gateway

### Migration Approach
- **Homogeneous**: Native tools + DMS
- **Heterogeneous**: SCT (schema) + DMS (data)
- **Zero downtime**: DMS with CDC

---

*Last Updated: Based on AWS SAA-C03 Exam Guide*

