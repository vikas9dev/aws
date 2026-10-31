# AWS Data & Analytics - Handnote 📝

## 🗄️ AMAZON DYNAMODB

### What is DynamoDB?
- **NoSQL database** service
- **Fully managed**
- **Serverless**
- **Single-digit millisecond** latency

### DynamoDB Features
- ✅ **Auto-scaling**
- ✅ **On-demand** or **provisioned** capacity
- ✅ **Global Tables**: Multi-region replication
- ✅ **Point-in-time recovery**
- ✅ **Backup & restore**
- ✅ **Encryption**: At rest & in transit

### DynamoDB Components
- **Tables**: Collections of items
- **Items**: Rows (JSON documents)
- **Attributes**: Fields/columns
- **Primary Key**: Partition key or Partition + Sort key

### DynamoDB Capacity Modes

#### On-Demand
- ✅ **Pay per request**
- ✅ **Automatic scaling**
- ✅ **No capacity planning**
- ✅ **Use case**: Unknown/unpredictable workloads

#### Provisioned
- ✅ **Set read/write capacity**
- ✅ **Auto Scaling** available
- ✅ **Reserved capacity** discounts
- ✅ **Use case**: Predictable workloads

### DynamoDB Streams
- **Time-ordered** sequence of item changes
- **Triggers Lambda** functions
- ✅ **Use case**: Real-time processing, analytics

### DynamoDB Global Tables
- **Multi-region** replication
- **Active-active** setup
- **<1 second** replication lag
- ✅ **Use case**: Global applications, disaster recovery

---

## 📊 AMAZON REDSHIFT

### What is Redshift?
- **Data warehouse** service
- **OLAP** (Online Analytical Processing)
- **Columnar storage**
- **SQL-based** queries

### Redshift Features
- ✅ **Massively Parallel Processing (MPP)**
- ✅ **Columnar storage** (optimized for analytics)
- ✅ **Compression**: Automatic compression
- ✅ **Encryption**: At rest & in transit
- ✅ **Backup**: Automated snapshots

### Redshift Architecture
- **Leader Node**: Query coordination
- **Compute Nodes**: Data storage & processing
- **Node Types**: RA3 (managed storage) and the newer Graviton-based RG. Dense Storage (DS2) is no longer available and Dense Compute (DC2) is deprecated (Apr 2025); older material lists Dense Compute and Dense Storage

### Redshift Spectrum
- **Query S3 data** directly
- **No data loading** required
- ✅ **Use case**: Data lakes, ad-hoc queries

---

## 🔍 AMAZON ATHENA

### What is Athena?
- **Serverless** SQL query service
- **Query S3 data** directly
- **Pay per query**
- **No infrastructure** to manage

### Athena Features
- ✅ **Standard SQL** (Presto; engine version 3 is based on Trino)
- ✅ **Query S3** (CSV, JSON, Parquet, ORC)
- ✅ **Results** stored in S3
- ✅ **Integration** with QuickSight

### Athena Pricing
- **$5 per TB** of data scanned
- ✅ **Optimize**: Use columnar formats (Parquet, ORC), partition data

---

## 📈 AMAZON QUICKSIGHT

### What is QuickSight?
- **Business intelligence** service
- **Dashboards** & **visualizations**
- **Serverless**
- **Pay per user**

### QuickSight Features
- ✅ **Interactive dashboards**
- ✅ **Data sources**: S3, RDS, Redshift, Athena, etc.
- ✅ **SPICE**: In-memory calculation engine
- ✅ **Embedded analytics**

### QuickSight Pricing
- **Author**: $24/month (Enterprise; older material says $18)
- **Reader**: $3/month (Enterprise; older material says $5)
- **SPICE**: 10 GB per author included, then $0.38/GB/month (Enterprise; $0.25/GB in Standard)

---

## 🔄 AMAZON EMR (Elastic MapReduce)

### What is EMR?
- **Big data platform**
- **Hadoop, Spark, HBase, Presto**
- **Process large datasets**
- **Fully managed**

### EMR Use Cases
- **Data processing**: ETL, data transformation
- **Machine learning**: Spark MLlib
- **Real-time streaming**: Spark Streaming
- **Interactive analytics**: Presto, Hive

### EMR Components
- **Master Node**: Coordinates cluster
- **Core Nodes**: Run tasks & store data
- **Task Nodes**: Run tasks only (optional)

---

## 📡 AMAZON KINESIS

### What is Kinesis?
- **Real-time streaming data platform** for collecting, processing, and analyzing streaming data at scale
- **Multiple services**: Data Streams (real-time streaming), Data Firehose (data loading), Data Analytics (real-time analytics), Video Streams (video streaming)

### Kinesis Data Streams
- **Real-time streaming** data
- **Shards**: Throughput units
- **Retention**: 1-365 days
- **Use cases**: Real-time analytics, log processing

### Kinesis Data Firehose
- **Load streaming data** to destinations
- **Fully managed**
- **Automatic scaling**
- **Destinations**: S3, Redshift, OpenSearch, Splunk, etc.

### Kinesis Data Analytics
- **Real-time analytics** on streaming data
- **SQL queries** on streams — ⚠️ Kinesis Data Analytics for SQL was discontinued on 27 Jan 2026. Use Amazon Managed Service for Apache Flink (or Flink Studio for SQL). Older exam questions may still name it.
- **Use cases**: Real-time dashboards, alerts

### Kinesis Video Streams
- **Stream video** from devices
- **Real-time** processing
- **Use cases**: Security, live streaming

---

## 🔍 AMAZON OPENSEARCH (ELASTICSEARCH)

### What is OpenSearch?
- **Search & analytics** engine
- **Full-text search**
- **Real-time** analytics
- **Log analytics**

### OpenSearch Features
- ✅ **RESTful API**
- ✅ **Integration** with Kibana
- ✅ **Encryption**: At rest & in transit
- ✅ **VPC** deployment

### Use Cases
- **Application search**
- **Log analytics**
- **Real-time monitoring**
- **Security analytics**

---

## ⚠️ CRITICAL EXAM POINTS

1. **DynamoDB**: NoSQL, serverless, single-digit ms latency
2. **On-Demand**: Pay per request, auto-scaling
3. **Provisioned**: Set capacity, auto-scaling available
4. **Global Tables**: Multi-region, active-active, <1 second lag
5. **DynamoDB Streams**: Triggers Lambda for real-time processing
6. **Redshift**: Data warehouse, OLAP, columnar storage
7. **Athena**: Serverless SQL on S3, pay per query
8. **QuickSight**: BI dashboards, pay per user
9. **EMR**: Big data platform (Hadoop, Spark)
10. **Kinesis**: Real-time streaming data processing

---

## 📋 QUICK REFERENCE

### Choosing Database
- **NoSQL, low latency**: DynamoDB
- **Data warehouse, analytics**: Redshift
- **Query S3 data**: Athena
- **Search & analytics**: OpenSearch
- **Big data processing**: EMR

### DynamoDB Use Cases
- **High-traffic web apps**
- **Gaming applications**
- **IoT applications**
- **Real-time bidding**

---

*Last Updated: Based on AWS SAA-C03 Exam Guide*

