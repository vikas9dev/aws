# AWS RDS, Aurora & ElastiCache - Handnote 📝

## 🗄️ AWS RDS (Relational Database Service)

### What is RDS?
- **Managed relational database service** that automates database administration tasks
- **Supports multiple database engines** with automated backups, patching, and monitoring

### Supported Engines
- PostgreSQL
- MySQL
- MariaDB
- Oracle
- Microsoft SQL Server
- IBM DB2
- **Aurora** (AWS proprietary)

### Why Use RDS?
- ✅ Fully automated provisioning
- ✅ Automated OS patching
- ✅ Continuous backups with Point-in-Time Restore
- ✅ Monitoring dashboards
- ✅ Read replicas (up to 15) - A single primary DB instance can have a maximum of 15 read replicas in total — across all Availability Zones and all Regions combined. RDS read replicas are capped at 15 per primary DB instance globally (all AZs + Regions combined).
- ✅ Multi-AZ for disaster recovery
- ✅ Maintenance windows
- ✅ Vertical & horizontal scaling
- ✅ Storage backed by EBS
- ⚠️ **Cannot SSH** into RDS instances

### RDS Storage Auto Scaling
- ✅ Automatically scales storage when running out of space
- ✅ Triggers when:
  - Free storage < 10% of allocated
  - Condition lasts > 5 minutes
  - Last storage change has finished optimizing, and < 4 changes in last 24 hours (old rule: "6 hours since last change")
- ✅ Set **maximum storage threshold**

---

## 📚 READ REPLICAS vs MULTI-AZ

### Read Replicas
- **Purpose**: **Scale read operations**
- **Up to 15 replicas**
- **Same AZ, different AZ, or different regions**
- **Asynchronous replication** (eventually consistent)
- ✅ Can be **promoted** to standalone database
- ✅ **Free** replication within same region, different AZs
- ⚠️ **Cross-region replication** incurs network costs
- ⚠️ **Only for SELECT** (read operations)

### Use Cases
- **Reporting & Analytics**: Offload read-heavy workloads
- **Read scaling**: Distribute read traffic

### Multi-AZ (Disaster Recovery)
- **Purpose**: **Disaster recovery** (NOT read scaling)
- **Synchronous replication** to standby in different AZ
- **Single DNS name** (automatic failover)
- **Failover time**: ~60 seconds
- ⚠️ **Standby is NOT used for reads** (failover only)
- ✅ **Zero-downtime** conversion from Single-AZ to Multi-AZ

### Important Note
- ✅ **Read Replicas CAN be Multi-AZ** for DR
- ⚠️ **Common exam question**

---

## 🚀 AMAZON AURORA

### What is Aurora?
- **AWS-proprietary database engine** compatible with MySQL and PostgreSQL
- **Cloud-native design** with automatic scaling, self-healing storage, and high performance

### Performance
- 🚀 **5x faster** than MySQL on RDS
- 🚀 **3x faster** than Postgres on RDS
- **20% more expensive** than RDS (but often cost-effective at scale)

### Storage
- ✅ **Auto-expands**: 10GB → 128TB (256 TiB on newer engine versions)
- ✅ **6 copies** across 3 AZs
- ✅ **4/6 copies** required for writes
- ✅ **3/6 copies** required for reads
- ✅ **Self-healing** (peer-to-peer replication)
- ✅ **Data distributed** across hundreds of volumes

### High Availability
- ✅ **Up to 15 read replicas** - Aurora supports 1 writer + up to 15 read replicas **per cluster**, and those replicas can be distributed across multiple AZs — but the limit applies per cluster, not across Regions.
- ✅ **Replication lag usually well under 100 ms** (old material: "sub-10 ms")
- ✅ **<30 second failover** (faster than Multi-AZ MySQL)
- ✅ **Any read replica can become master**

### Aurora Endpoints
- **Writer Endpoint**: Always points to master (writes)
- **Reader Endpoint**: Load balances across read replicas (reads)
- **Custom Endpoints**: Subset of instances for specific workloads

### Aurora Features
- ✅ **Backtrack**: Restore to any point in time (no backups needed)
- ✅ **Replica Auto-Scaling**: Automatically scale read replicas (1-15)
- ✅ **Serverless**: Auto-scaling based on usage (pay per second). v1 ended 31 Mar 2025; v2 can scale to 0 ACU
- ✅ **Global Aurora**: Cross-region replication (<1 second lag)
- ✅ **Machine Learning**: Integrate with SageMaker, Comprehend
- ✅ **Babelfish**: Aurora PostgreSQL understands T-SQL (SQL Server migration)

### Global Aurora
- **Primary region**: Read/write
- **Up to 10 secondary regions**: Read-only (was 5 before May 2025)
- **<1 second replication lag**
- **RTO <1 minute** for failover
- **Up to 16 read replicas** per secondary region

Aurora Global Database (Multi-Region)
* 1 **Primary Region**
  * 1 writer + up to 15 readers
* Each **Secondary Region**
  * **Read-only cluster**
  * Up to **16 readers per Region**

---

## 💾 RDS & AURORA BACKUPS

### RDS Automated Backups
- ✅ **Daily full backup** during backup window
- ✅ **Transaction logs** backed up every 5 minutes
- ✅ **Point-in-Time Restore** (up to 5 minutes ago)
- ✅ **Retention**: 1-35 days (configurable)
- ⚠️ **Set to 0 to disable**

### RDS Manual Snapshots
- ✅ **Manually triggered**
- ✅ **Retained indefinitely**
- ✅ **Cost-saving tip**: Snapshot & delete instead of stopping RDS

### Aurora Backups
- ✅ **Similar to RDS** (1-35 days retention)
- ⚠️ **Cannot disable** automated backups
- ✅ **Point-in-Time Restore** available
- ✅ **Manual snapshots** supported

### Restore Options
- ✅ Always creates **new database instance**
- ✅ **MySQL RDS**: Restore from S3 backup
- ✅ **Aurora MySQL**: Restore from S3 using **Percona XtraBackup**

### Aurora Database Cloning
- ✅ **Fast cloning** (seconds, not hours)
- ✅ **Point-in-time copy** of database
- ✅ **Independent** from source (changes don't affect each other)
- ✅ **Cost-effective** for testing

---

## 🔒 RDS & AURORA SECURITY

### Encryption
- ✅ **At Rest**: KMS encryption
- ✅ **In Transit**: SSL/TLS certificates
- ✅ **Encrypt snapshots** if source encrypted

### Network Security
- ✅ **VPC**: Deploy in private subnets
- ✅ **Security Groups**: Control access - Both Amazon RDS and Amazon Aurora run in a VPC and are protected by Security Groups.
- ✅ **No public access** (unless explicitly enabled)

### Authentication
- ✅ **IAM Database Authentication**: Use IAM roles (MariaDB/MySQL/PostgreSQL)
- ✅ **Kerberos Authentication**: Microsoft SQL Server

### Automated Security
- ✅ **Automated backups**
- ✅ **Automated patching**
- ✅ **Automated minor version upgrades**

---

## 🔌 AMAZON RDS PROXY

### What is RDS Proxy?
- **Fully managed database proxy** that pools and shares database connections
- **Reduces connection overhead** and improves application scalability and resilience

### Purpose
- **Connection pooling** for RDS/Aurora
- **Reduces database connections**
- **Improves scalability**

### Features
- ✅ **IAM authentication**
- ✅ **Secrets Manager integration**
- ✅ **Enforce SSL/TLS**
- ✅ **Query logging**

### Use Cases
- **Serverless applications** (Lambda)
- **High connection counts**
- **Multi-tenant applications**

---

## 🔥 AMAZON ELASTICACHE

### What is ElastiCache?
- **Managed in-memory caching service** supporting Redis and Memcached
- **Improves application performance** by caching frequently accessed data

### Purpose
- **In-memory caching** for databases
- **Reduce database load**
- **Improve application performance**

### Engines
- **Redis**: Advanced data structures, pub/sub, persistence
- **Memcached**: Simple key-value store, multi-threaded

### Redis vs Memcached

| Feature | Redis | Memcached |
|---------|-------|-----------|
| **Data Types** | Strings, Lists, Sets, Sorted Sets, Hashes | Key-Value only |
| **Persistence** | ✅ Yes (AOF, RDB) | ❌ No |
| **Pub/Sub** | ✅ Yes | ❌ No |
| **Multi-AZ** | ✅ Yes (with replication) | ❌ No |
| **Backup/Restore** | ✅ Yes | ❌ No (✅ only with ElastiCache Serverless for Memcached) |
| **Threading** | Single-threaded | Multi-threaded |
| **Use Case** | Advanced features needed | Simple caching |

### ElastiCache Features
- ✅ **Cluster Mode**: Scale horizontally (Redis)
- ✅ **Multi-AZ**: Automatic failover (Redis)
- ✅ **Backup & Restore**: Snapshots, restored into a new cache (Redis) — no point-in-time restore
- ✅ **Encryption**: At rest & in transit
- ✅ **VPC**: Deploy in private subnets
- ✅ **Security Groups**: Control access

### Use Cases
- **Session Store**: Store user sessions
- **Database Caching**: Cache frequently accessed data
- **Leaderboards**: Real-time rankings (Redis Sorted Sets)
- **Pub/Sub**: Real-time messaging (Redis)

---

## 🔌 PORTS TO REMEMBER

| Service | Port |
|---------|------|
| **SSH** | 22 |
| **HTTP** | 80 |
| **HTTPS** | 443 |
| **MySQL** | 3306 |
| **PostgreSQL** | 5432 |
| **Redis** | 6379 |
| **Memcached** | 11211 |
| **MongoDB** | 27017 |

---

## ⚠️ CRITICAL EXAM POINTS

1. **Read Replicas**: Scale reads, async, eventually consistent
2. **Multi-AZ**: DR only, sync, automatic failover, NOT for reads
3. **Read Replicas CAN be Multi-AZ** (common exam question)
4. **Aurora**: 6 copies, 3 AZs, auto-expands 10GB→128TB (256 TiB on newer versions)
5. **Aurora failover**: <30 seconds (faster than RDS Multi-AZ)
6. **Aurora Global**: <1 second replication lag, RTO <1 minute
7. **RDS backups**: Can be disabled (set retention to 0)
8. **Aurora backups**: Cannot be disabled
9. **ElastiCache Redis**: Advanced features, persistence, Multi-AZ
10. **ElastiCache Memcached**: Simple caching, multi-threaded

---

## 📋 QUICK REFERENCE

### When to Use Read Replicas
- Read-heavy workloads
- Reporting & analytics
- Cross-region read access

### When to Use Multi-AZ
- Disaster recovery
- High availability
- Automatic failover needed

### When to Use Aurora
- High performance needed
- Auto-scaling storage
- Fast failover required
- Global database needed

### When to Use ElastiCache
- Reduce database load
- Improve application performance
- Session storage
- Real-time features (Redis)

---

*Last Updated: Based on AWS SAA-C03 Exam Guide*

