# Databases in AWS - Handnote 📝

## 🎯 CHOOSING THE RIGHT DATABASE

### Key Considerations
- **Workload**: Write-heavy, read-heavy, or balanced?
- **Data volume**: Total size, growth rate
- **Access patterns**: Frequency, concurrent users
- **Data model**: Structured, semi-structured, unstructured
- **Requirements**: Durability, latency, joins, schema
- **Licensing**: Open source vs proprietary

---

## 📊 DATABASE CATEGORIES

### 1. Relational Databases (RDBMS) 🗃️
- **SQL support**: Yes
- **OLTP**: Online Transaction Processing
- **Services**: RDS, Aurora
- **Use Case**: Joins, ACID transactions, structured data

### 2. NoSQL Databases 🧱
- **Flexibility**: Schema flexibility
- **No joins**: Limited query capabilities
- **Services**:
  - **DynamoDB**: Key-value, document (~JSON)
  - **ElastiCache**: Key-value (in-memory)
  - **Neptune**: Graph database
  - **DocumentDB**: MongoDB compatible
  - **Keyspaces**: Apache Cassandra compatible
- **Use Case**: High scale, specific data models

### 3. Object Stores 🗂️
- **Large objects**: Files, backups
- **Services**: S3, Glacier
- **Use Case**: Big object storage, backups

### 4. Data Warehousing & Analytics 📈
- **OLAP**: Online Analytical Processing
- **Services**: Redshift, Athena, EMR
- **Use Case**: BI, reporting, analytics at scale

### 5. Search Databases 🔍
- **Full-text search**: Indexing unstructured data
- **Service**: OpenSearch Service
- **Use Case**: Fast, flexible searching

### 6. Graph Databases 🧬
- **Relationships**: Model connections
- **Service**: Neptune
- **Use Case**: Social networks, recommendations, fraud detection

### 7. Ledger Databases 📓
- **Immutable**: Cryptographically verifiable
- **Service**: QLDB (Quantum Ledger Database) — ⚠️ end of support 31 Jul 2025
- **Use Case**: Auditable transaction logs

### 8. Time Series Databases ⏱️
- **Time-stamped data**: Metrics, logs, events
- **Service**: Timestream — ⚠️ LiveAnalytics closed to new customers 20 Jun 2025 (InfluxDB edition still open)
- **Use Case**: IoT, DevOps, real-time monitoring

---

## 🗄️ AMAZON RDS SUMMARY

### Supported Engines
- PostgreSQL, MySQL, Oracle, SQL Server, DB2, MariaDB
- **Custom**: Custom versions available

### Provisioning
- **Instance size**: Choose EC2 instance type
- **Storage**: EBS volume type and size
- **Auto-scaling**: Available for storage

### High Availability & Performance
- **Read Replicas**: Scale read capabilities
- **Multi-AZ**: Standby database for DR (not for querying)

### Security
- **IAM**: Authentication (some engines)
- **Security Groups**: Network security
- **KMS**: Encryption at rest
- **SSL/TLS**: Encryption in transit

### Backups
- **Automated backups**: Up to 35 days (point-in-time restore)
- **Manual snapshots**: Long-term retention

### Maintenance
- **Managed maintenance**: Scheduled downtime for updates

### Advanced Features
- **RDS Proxy**: Connection pooling, IAM authentication
- **Secrets Manager**: Credential management
- **RDS Custom**: Access to underlying instance

---

## ⭐ AMAZON AURORA SUMMARY

### Key Features
- **MySQL/PostgreSQL compatible**: Drop-in replacement
- **High performance**: Up to 5x MySQL, 3x PostgreSQL
- **Auto-scaling**: Storage scales automatically (10GB-128TB; 256 TiB on newer engine versions)
- **High availability**: 6 copies across 3 AZs
- **Backup**: Continuous backup to S3

### Aurora Serverless
- **Auto-scaling**: Scales automatically
- **On-demand**: Pay per use
- **Use Case**: Intermittent, unpredictable workloads

### Global Database
- **Multi-region**: Replication across regions
- **Low latency**: Read replicas in multiple regions
- **DR**: Cross-region disaster recovery

### Performance Insights
- **Monitoring**: Database performance metrics
- **Optimization**: Identify bottlenecks

---

## ⚡ AMAZON ELASTICACHE

### Purpose
- **In-memory cache**: Sub-millisecond latency
- **Reduce database load**: Cache frequently accessed data

### Engines
- **Redis**: Advanced features, persistence
- **Memcached**: Simple, multi-threaded

### Use Cases
- **Session storage**: Shopping carts, user sessions
- **Database caching**: Reduce RDS load
- **Real-time analytics**: Fast data processing

---

## 🚀 DYNAMODB

### What is DynamoDB?
- **Fully managed NoSQL database** service with single-digit millisecond latency
- **Serverless and auto-scaling** with built-in security, backup, and multi-region replication

### Key Features
- **Serverless**: No server management
- **NoSQL**: Key-value and document database
- **Auto-scaling**: Scales automatically
- **Global Tables**: Multi-region replication
- **Streams**: Real-time data capture

### Performance
- **Single-digit millisecond**: Low latency
- **On-demand**: Auto-scaling capacity
- **Provisioned**: Set read/write capacity

### DAX (DynamoDB Accelerator)
- **In-memory cache**: Sub-millisecond reads
- **Fully managed**: No application changes
- **Use Case**: Read-heavy workloads

---

## 📦 S3 AS DATABASE

### Use Case
- **Object storage**: Large unstructured objects
- **Basic database**: For big object storage
- **Scalable**: Petabyte scale

---

## 📄 DOCUMENTDB

### What is DocumentDB?
- **MongoDB-compatible document database** service built on Aurora architecture
- **Fully managed** with automatic scaling, backups, and multi-AZ support

### Purpose
- **MongoDB compatible**: Aurora for MongoDB
- **Managed**: AWS manages infrastructure
- **Use Case**: MongoDB workloads on AWS

---

## 🔷 NEPTUNE

### What is Neptune?
- **Fully managed graph database** service optimized for highly connected data
- **Supports property graph and RDF models** with high-performance querying

### Purpose
- **Graph database**: Model relationships
- **Use Case**: Social networks, recommendations, fraud detection

---

## 🔑 AMAZON KEYSPACES

### What is Keyspaces?
- **Apache Cassandra-compatible managed database** service
- **Serverless wide-column store** with automatic scaling and pay-per-use pricing

### Purpose
- **Apache Cassandra compatible**: Managed Cassandra
- **Serverless**: Auto-scaling
- **Use Case**: Wide-column store workloads

---

## ⏱️ AMAZON TIMESTREAM

### What is Timestream?
- **Fully managed time-series database** optimized for IoT and DevOps applications
- **Serverless** with automatic scaling and built-in data lifecycle management

### Purpose
- **Time series database**: Optimized for time-stamped data
- **Use Case**: IoT, DevOps, real-time monitoring

---

## ⚠️ CRITICAL EXAM POINTS

1. **RDS**: Relational, SQL, OLTP, managed
2. **Aurora**: High performance, auto-scaling storage, 6 copies
3. **ElastiCache**: In-memory cache, Redis/Memcached
4. **DynamoDB**: Serverless NoSQL, auto-scaling, global tables
5. **DAX**: DynamoDB caching, sub-ms reads
6. **Redshift**: Data warehousing, OLAP, analytics
7. **Neptune**: Graph database, relationships
8. **DocumentDB**: MongoDB compatible
9. **Keyspaces**: Cassandra compatible
10. **Timestream**: Time series, IoT, monitoring
11. **S3**: Object storage, basic database for large objects

---

## 📋 QUICK REFERENCE

### Database Selection Guide
- **SQL/Joins needed**: RDS or Aurora
- **High scale/NoSQL**: DynamoDB
- **Caching**: ElastiCache
- **Analytics**: Redshift
- **Graph data**: Neptune
- **Time series**: Timestream
- **MongoDB**: DocumentDB
- **Cassandra**: Keyspaces

### Performance Optimization
- **Read replicas**: Scale reads (RDS/Aurora)
- **DAX**: Cache DynamoDB reads
- **ElastiCache**: Cache RDS queries
- **Global Tables / Global Database**: Reduce latency (DynamoDB Global Tables / Aurora Global Database)

---

*Last Updated: Based on AWS SAA-C03 Exam Guide*

