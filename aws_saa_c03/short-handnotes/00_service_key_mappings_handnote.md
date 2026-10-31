# AWS Service Key Mappings - Exam Reference 📋

> Comprehensive quick reference guide to identify AWS services based on keywords, use cases, and exam scenarios

---

## 💻 Compute & EC2

| Key | Service |
|-----|---------|
| General Purpose workloads, web servers, code repositories | **EC2 General Purpose Instances** |
| Compute-intensive tasks, batch processing, media transcoding, HPC, ML, game servers | **EC2 Compute Optimized (C family)** |
| Large datasets in memory, in-memory databases, ElastiCache, real-time processing | **EC2 Memory Optimized (R, X1, Z1)** |
| High-speed local storage, OLTP, databases, data warehousing | **EC2 Storage Optimized (I, D, H1)** |
| Short workloads, predictable pricing | **EC2 On-Demand Instances** |
| Long workloads, running databases for extended periods | **EC2 Reserved Instances (1 & 3 years)** |
| Commit to usage in dollars (not instance type) | **EC2 Savings Plans** |
| Very short workloads, very cheap, can lose instances | **EC2 Spot Instances** |
| Book entire physical server, control instance placements | **EC2 Dedicated Host** |
| No other customers share your hardware | **EC2 Dedicated Instances** |
| Reserve capacity in specific AZ for any duration | **EC2 Capacity Reservations** |
| Low-latency, high performance, computational jobs, single AZ | **EC2 Cluster Placement Group** |
| Maximize high availability, reduce risk, 7 instances per AZ | **EC2 Spread Placement Group** |
| Partition-aware apps, HDFS, HBase, Cassandra, Kafka, 100s of instances | **EC2 Partition Placement Group** |
| Very high-performance hardware attached volume | **EC2 Instance Store** (ephemeral) |
| Multiple EC2 instances need same file system, Linux only | **Amazon EFS** |
| Content management, web serving, data sharing, WordPress | **Amazon EFS** |
| Need to enforce IAM authentication for database | **RDS Proxy** |
| Lambda functions connecting to RDS (connection pooling) | **RDS Proxy** |

---

## ⚖️ Load Balancers & Auto Scaling

| Key | Service |
|-----|---------|
| Layer 7, HTTP/HTTPS, content-based routing | **Application Load Balancer (ALB)** |
| Layer 4, TCP/UDP, high performance, static IP per AZ | **Network Load Balancer (NLB)** |
| Deploy third-party network virtual appliances, firewall, IDPS | **Gateway Load Balancer (GWLB)** |
| GENEVE protocol on port 6081 | **Gateway Load Balancer** |
| Cross-zone load balancing enabled by default, no inter-AZ charges | **ALB** |
| Cross-zone load balancing disabled by default, incurs inter-AZ charges | **NLB, GWLB** |
| Simple to set up, maintain metric around target value | **ASG Target Tracking Scaling** |
| Define CloudWatch alarms, add/remove capacity | **ASG Simple/Step Scaling** |
| Anticipated scaling needs, known usage patterns | **ASG Scheduled Scaling** |
| Historical data to forecast future load, repeating patterns | **ASG Predictive Scaling** |

---

## 🗄️ Databases

| Key | Service |
|-----|---------|
| RDBMS databases, OLTP, SQL queries, transactions | **Amazon RDS** |
| Scale read operations, up to 15 replicas, eventually consistent | **RDS Read Replicas** |
| Disaster recovery, synchronous replication, automatic failover | **RDS Multi-AZ** |
| Cloud-optimized, 5x MySQL, 3x Postgres performance | **Amazon Aurora** |
| Auto-expanding storage (10 GiB to 256 TiB; was 128 TiB before Jul 2025) | **Amazon Aurora** |
| Replication lag usually well under 100 ms (older notes say sub-10 ms), <30s failover | **Amazon Aurora** |
| 6 copies across 3 AZs, self-healing | **Amazon Aurora** |
| Infrequent, intermittent, unpredictable workloads | **Aurora Serverless** |
| Global database, <1 second replication lag | **Aurora Global Database** |
| ML predictions via SQL, fraud detection, recommendations | **Aurora Machine Learning** |
| Aurora PostgreSQL understands T-SQL (SQL Server) | **Babelfish for Aurora PostgreSQL** |
| Create staging from production, copy-on-write | **Aurora Database Cloning** |
| Flexible and Evolving Schema, Schema needs to evolve rapidly | **DynamoDB** |
| Unpredictable workloads or sudden spikes on DynamoDB | **DynamoDB On-Demand Mode** |
| Very few transactions (e.g 4-5 times a day) | **DynamoDB On-Demand Mode** |
| Microseconds latency for Cached Data | **DynamoDB Accelerator (DAX)** |
| Caching solution does not require code changes | **Don't Use ElastiCache** (requires code changes) |
| **MongoDB** | **DocumentDB** |
| **Graph Database** | **Amazon Neptune** |
| Social Networks, Knowledge Graphs, Fraud Detection, Recommendation Engines | **Amazon Neptune** |
| **Apache Cassandra** | **Amazon Keyspaces** |
| IOT Device Information Storage, Time series data storage | **Amazon Keyspaces** or **Amazon Timestream** |
| Time series data storage | **Amazon Timestream** (⚠️ Timestream for LiveAnalytics closed to new customers 20 Jun 2025; Timestream for InfluxDB is still open) |
| Secure, immutable, cryptographically verifiable financial records | **Amazon QLDB** (⚠️ end of support 31 Jul 2025; AWS points to Aurora PostgreSQL) |
| Gaming leaderboard | **ElastiCache Redis** (Sorted Sets) |
| SASL-based authentication for Memcached | **ElastiCache Memcached** |

---

## 📊 Data & Analytics

| Key | Service |
|-----|---------|
| Analyze data in S3 using serverless SQL | **Amazon Athena** |
| OLAP (Online Analytical Processing) | **Amazon Redshift** |
| Query data directly from S3 without loading it to Redshift | **Redshift Spectrum** |
| Full-text search, partial match, and analytics | **Amazon OpenSearch** |
| Big Data Clusters, Hadoop Clusters | **Amazon EMR** |
| Machine-learning powered business intelligence tool | **Amazon QuickSight** |
| In-memory computation in Amazon QuickSight | **SPICE** (Super-fast, Parallel, In-memory Calculation Engine) |
| Extract, transform, and load (ETL) Service | **AWS Glue** |
| Convert data into Parquet Format | **AWS Glue** |
| Prevents reprocessing old data when running a new ETL job | **AWS Glue Job Bookmarks** |
| Manage security in one central place | **AWS Lake Formation** |
| Processing data streams | **Managed Service for Apache Flink** |
| Manage IoT Devices | **AWS IoT Core** |

**📌 Exam Tip:** 
- Flink can read from Kinesis Data Streams, but **cannot** read from Amazon Data Firehose
- **Kinesis → shards**, while **Kafka/MSK → topics & partitions**

---

## 🤖 Machine Learning

| Key | Service |
|-----|---------|
| Find objects, People, Text, Scenes in Images and Videos | **Amazon Rekognition** |
| Content Moderation | **Amazon Rekognition** |
| Speech to Text | **Amazon Transcribe** |
| PII Redaction, Automatic Language Identification | **Amazon Transcribe** |
| Text to Speech | **Amazon Polly** |
| Pronunciation of Stylish Words and Acronyms | **Amazon Polly → Pronunciation Lexicons** |
| Customization on how words are pronounced | **Amazon Polly → SSML** (Speech Synthesis Markup Language) |
| Localize Content | **Amazon Translate** |
| Automatic Speech Recognition (ASR) & Natural Language Understanding (NLU) | **Amazon Lex** |
| Building Visual contact center | **Amazon Connect** |
| NLP (Natural Language Processing) | **Amazon Comprehend** |
| Detect and return useful information from unstructured clinical text | **Amazon Comprehend Medical** |
| Build ML Models | **Amazon SageMaker** |
| Document Search Service | **Amazon Kendra** |
| Machine learning service for building recommendations and personalized experiences | **Amazon Personalize** |
| Extract text, handwriting, and data from scanned documents | **Amazon Textract** |

---

## 📊 Monitoring & Auditing

| Key | Service |
|-----|---------|
| Cron Jobs | **AWS CloudWatch Events** (EventBridge) |
| Collect, aggregate, and summarize metrics and logs from containers | **AWS CloudWatch Container Insights** |
| Monitoring and troubleshooting solution for serverless applications (Lambda) | **AWS CloudWatch Lambda Insights** |
| See Metrics about the top-N contributors | **AWS CloudWatch Contributor Insights** |
| Automated dashboard showing potential problems with monitored applications (EC2) | **AWS CloudWatch Application Insights** |
| Governance, compliance, and auditing capabilities for AWS accounts | **AWS CloudTrail** |
| Auditing and compliance recording for AWS resources | **AWS Config** |
| Track queries and database activity | **RDS Audit Logs** → **CloudWatch Logs** |

---

## 🌐 Networking & VPC

| Key | Service |
|-----|---------|
| IP Multicast | **Transit Gateway** (only AWS service that supports IP multicast) |
| Privately access AWS services without internet, most services | **VPC Interface Endpoints** (PrivateLink) |
| Privately access S3 and DynamoDB, free, gateway in route table | **VPC Gateway Endpoints** |
| Capture IP traffic information | **VPC Flow Logs** → S3, CloudWatch Logs, or Kinesis Data Firehose |
| Private encrypted communication between VPC and on-premises | **Site-to-Site VPN** |
| VPN concentrator on AWS side | **Virtual Private Gateway (VGW)** |
| VPN device on corporate side | **Customer Gateway (CGW)** |
| Multiple customer networks communicate securely | **AWS VPN CloudHub** |
| Dedicated private connection from on-premises to VPC | **Direct Connect (DX)** |
| Connect to multiple VPCs in different regions | **Direct Connect Gateway** |
| Connect thousands of VPCs, hub-and-spoke | **Transit Gateway** |
| Increase VPN bandwidth using multiple connections | **Transit Gateway with ECMP** |
| Capture and inspect network traffic for security | **VPC Traffic Mirroring** |
| Outbound IPv6 traffic from private subnets | **Egress-only Internet Gateway** |
| Bidirectional DNS resolution between on-premises and AWS | **Route 53 Resolver** (Inbound + Outbound Endpoints) |

---

## 🔐 Security & Encryption

| Key | Service |
|-----|---------|
| Manage encryption keys, audit via CloudTrail | **AWS KMS** |
| Symmetric encryption (AES-256), integrated services | **KMS Symmetric Keys** |
| Asymmetric encryption, encrypt outside AWS | **KMS Asymmetric Keys (RSA/ECC)** |
| Free, used with SSE-S3, SSE-DynamoDB | **AWS Owned Keys** |
| Free, start with `aws/<service-name>` | **AWS Managed Keys** |
| Custom keys, $1/month | **Customer Managed Keys** |
| Automatic rotation every 1 year | **AWS Managed Keys** |
| Enable automatic rotation or on-demand | **Customer Managed Keys** |
| Manual rotation only | **Imported KMS Keys** |
| Secure storage for configuration data and secrets | **SSM Parameter Store** |
| Store secrets with automatic rotation | **AWS Secrets Manager** |
| Secrets integration with RDS or Aurora | **AWS Secrets Manager** |
| Replicate secrets across multiple regions | **AWS Secrets Manager Multi-Region** |
| Provision, manage, deploy TLS/SSL certificates | **AWS Certificate Manager (ACM)** |
| Protect web applications from Layer 7 exploits | **AWS WAF** |
| Deploy on ALB, API Gateway, CloudFront, AppSync, Cognito | **AWS WAF** |
| ⚠️ **Cannot deploy on NLB** (Layer 4) | **AWS WAF** |
| Filter by IP addresses, HTTP headers/body, Geo match, Rate-based | **AWS WAF** |
| Baseline DDoS protection, free, automatically enabled | **AWS Shield Standard** |
| Enhanced DDoS protection, ~$3,000/month | **AWS Shield Advanced** |
| Manage firewall rules across all accounts in Organization | **AWS Firewall Manager** |
| Detect threats and suspicious activity | **Amazon GuardDuty** |
| Assess application security and compliance | **Amazon Inspector** |
| Discover and protect sensitive data in S3 | **Amazon Macie** |
| Enforce consistent tagging across multiple accounts | **AWS Organizations Tag Policies** |
| Restrict API calls based on client IP | **IAM Condition: aws:SourceIP** |
| Restrict API calls based on region | **IAM Condition: aws:RequestedRegion** |
| Enforce MFA | **IAM Condition: aws:MultiFactorAuthPresent** |
| Restrict to accounts within Organization | **IAM Condition: aws:PrincipalOrgID** |
| Fine-grained access based on resource and user tags | **IAM Conditions: ec2:ResourceTag & aws:PrincipalTag** |
| Define maximum permissions for users/roles | **IAM Permission Boundaries** |
| Proxy users to on-premise AD | **AD Connector** |
| Manage users in AWS with MFA | **AWS Managed Microsoft AD** |
| Simple AD without on-premise integration | **Simple AD** |

---

## 🔄 Integration & Messaging

| Key | Service |
|-----|---------|
| Decouple Applications, Sudden Spikes load, Timeouts, Rapid scaling | **SQS** |
| Real-time Streaming | **Kinesis Data Streams** |
| Near real-time | **Amazon Data Firehose** |
| Deliver data to multiple SQS queues | **SNS Fan-Out Pattern** |
| React to S3 events (object creation, removal, restoration) | **S3 Event Notifications** → SNS, SQS, Lambda, EventBridge |
| Route events to multiple destinations, filtering with JSON rules | **Amazon EventBridge** |
| Intercept API calls via CloudTrail | **EventBridge + CloudTrail Integration** |
| Ingest external events | **API Gateway → Kinesis Data Streams** |
| Resource-Based Policies for EventBridge | **S3, SNS, SQS, Lambda, API Gateway** |
| IAM Roles for EventBridge | **Kinesis Data Streams, EC2 Auto Scaling, SSM Run Command, ECS task** |

---

## 🐳 Containers

| Key | Service |
|-----|---------|
| Microservices, Lift-and-shift app from on-premise to cloud | **Docker** |
| **Container Storage Interface** (CSI) | **Amazon EKS** |
| Migrate and modernize Java and .NET web applications | **AWS App2Container (A2C)** |

---

## 🔐 Identity & Access

| Key | Service |
|-----|---------|
| "Hundreds of Users", "Mobile Users", "Authenticate with SAML" | **Cognito** |

---

## 📦 Storage

| Key | Service |
|-----|---------|
| General Purpose, default storage class | **S3 Standard** |
| Data accessed less frequently, rapid access when needed | **S3 Standard-IA** |
| Single AZ, secondary backups, data can be recreated | **S3 One Zone-IA** |
| Milliseconds retrieval, accessed once a quarter | **S3 Glacier Instant Retrieval** |
| Flexible retrieval times (1-5 min, 3-5 hours, 5-12 hours) | **S3 Glacier Flexible Retrieval** |
| Lowest-cost, long-term archiving (12-48 hours retrieval) | **S3 Glacier Deep Archive** |
| Automatically moves objects between tiers | **S3 Intelligent-Tiering** |
| Single-digit millisecond latency, 10x performance, AI/ML training | **S3 Express One Zone** |
| High-performance database workloads, co-location with compute | **S3 Express One Zone** |
| Encryption managed by AWS (AES-256) | **S3 SSE-S3** |
| Encryption with user-managed keys, audit via CloudTrail | **S3 SSE-KMS** |
| Encryption with client-provided keys, must use HTTPS | **S3 SSE-C** (disabled by default on new buckets since Apr 2026) |
| Clients encrypt before sending to S3 | **Client-Side Encryption** |
| Temporary access to S3 objects, expiration date | **S3 Pre-Signed URLs** |
| Lock Glacier vault, WORM model, compliance | **S3 Glacier Vault Lock** (⚠️ standalone Glacier vault service closed to new customers; use S3 Object Lock for new work) |
| Lock individual objects, WORM model | **S3 Object Lock** |
| Strict mode, no one can override | **S3 Object Lock Compliance Mode** |
| Lenient mode, admins can override | **S3 Object Lock Governance Mode** |
| Indefinite protection, independent of retention | **S3 Object Lock Legal Hold** |
| Simplified security management for S3 buckets | **S3 Access Points** |
| Modify objects via Lambda before retrieval | **S3 Object Lambda** (⚠️ closed to new customers since 7 Nov 2025) |
| Redact PII, convert formats, resize images on-the-fly | **S3 Object Lambda** |
| System boot volumes, virtual desktops, dev/test | **EBS gp2/gp3** |
| Mission-critical, low-latency, high-throughput, databases | **EBS io1/io2** |
| Big data, data warehousing, log processing | **EBS st1** (Throughput Optimized HDD) |
| Archive data, infrequently accessed | **EBS sc1** (Cold HDD) |
| Multi-attach to multiple EC2 instances (same AZ) | **EBS io1/io2 Multi-Attach** |
| Move snapshots to archive tier, 75% cheaper, 24-72 hours restore | **EBS Snapshot Archive** |
| Recover from accidental deletions, 1 day to 1 year retention | **EBS Snapshot Recycle Bin** |
| Force full initialization, no latency on first use | **EBS Fast Snapshot Restore (FSR)** |
| Migrate petabytes of data, edge computing | **AWS Snowball** (⚠️ Snowball Edge closed to new customers since 7 Nov 2025) |
| Primarily for storage, 210 TB | **Snowball Edge Storage Optimized** |
| Primarily for compute, 28 TB | **Snowball Edge Compute Optimized** |
| Run EC2 instances and Lambda on device | **Snowball Edge Compute** |
| ⚠️ Cannot directly import to Glacier | **Snowball** → Use S3 + Lifecycle Policy |

---

## 🌍 Content Delivery & Global Services

| Key | Service |
|-----|---------|
| CDN, edge caching, static files distribution | **CloudFront** |
| Static files distribution, file uploads to S3 | **CloudFront + S3** |
| Applications in private subnets | **CloudFront VPC Origins** |
| Restrict access by country | **CloudFront Geo Restriction** |
| Reduce costs by limiting edge locations | **CloudFront Price Classes** |
| Force cache refresh | **CloudFront Invalidations** |
| Global traffic manager, static IPs, fast failover | **AWS Global Accelerator** |
| Real-time applications, gaming, IoT, VoIP | **AWS Global Accelerator** |
| DNS service, routing policies | **Route 53** |
| Route to single resource | **Route 53 Simple Routing** |
| Direct percentage of traffic | **Route 53 Weighted Routing** |
| Route to lowest latency | **Route 53 Latency-Based Routing** |
| Automatic failover primary to secondary | **Route 53 Failover Routing** |
| Route based on geographic location | **Route 53 Geolocation Routing** |
| Route based on geographic proximity with bias | **Route 53 Geoproximity Routing** |
| Route based on client IP addresses | **Route 53 IP-based Routing** |
| Return multiple healthy records | **Route 53 Multi-Value Answer** |
| Point hostname to another hostname, non-root only | **Route 53 CNAME Records** |
| Point hostname to AWS resource, root and non-root, free | **Route 53 Alias Records** |
| Native health check capabilities | **Route 53 Alias Records** |

---

## 🔄 Disaster Recovery & Migration

| Key | Service |
|-----|---------|
| Backup data and restore when needed, high RPO/RTO | **Backup and Restore** |
| Small version always running, critical core | **Pilot Light** |
| Scaled-down full system, scale on DR | **Warm Standby** |
| Two full production environments | **Hot Site/Multi-Site** |
| Migrate database from on-premise to AWS | **AWS DMS (Database Migration Service)** |
| Homogeneous and heterogeneous migrations | **AWS DMS** |
| Continuous data replication using CDC | **AWS DMS** |
| Schema migration (tables, indexes, constraints) | **AWS Schema Conversion Tool (SCT)** |
| RDS MySQL to Aurora MySQL (with downtime) | **Database Snapshot** |
| RDS MySQL to Aurora MySQL (no downtime) | **Aurora Read Replica** → Promote |
| External MySQL to Aurora MySQL | **Percona XtraBackup** → S3 → Aurora Import |
| Both databases running, continuous replication | **Amazon DMS** |
| Migrate VMs and applications into EC2 | **VM Import/Export** |
| Gather information about on-premise servers | **AWS Application Discovery Service** |
| Track migration progress | **AWS Migration Hub** (⚠️ closed to new customers since 7 Nov 2025; replaced by AWS Transform) |
| Incremental replication of on-premise servers | **AWS Application Migration Service (MGN)** (replaced AWS Server Migration Service (SMS), discontinued 31 Mar 2022) |
| Centrally manage and automate backups | **AWS Backup** |
| WORM policy for backups, cannot be deleted | **AWS Backup Vault Lock** |

---

## 🛠️ Other Services

| Key | Service |
|-----|---------|
| Infrastructure as Code, declarative infrastructure | **AWS CloudFormation** |
| Alternative to Terraform, AWS-only | **AWS CloudFormation** |
| Send emails securely, globally, at scale | **Amazon SES** |
| Transactional emails, marketing emails, bulk emails | **Amazon SES** |
| Scalable marketing communication, SMS, push notifications | **Amazon Pinpoint** (⚠️ end of support 30 Oct 2026) |
| Secure shell without SSH, bastion hosts, or SSH keys | **SSM Session Manager** |
| Execute scripts on multiple instances | **SSM Run Command** |
| Automate patching managed instances | **SSM Patch Manager** |
| Schedule actions on instances | **SSM Maintenance Windows** |
| Simplify maintenance and deployment tasks | **SSM Automation** |
| Visualize, understand, manage AWS costs | **Cost Explorer** |
| ML-powered cost anomaly detection | **AWS Cost Anomaly Detection** |
| Batch processing at any scale | **AWS Batch** |
| Data transfer between SaaS and AWS (Salesforce, SAP, etc.) | **Amazon AppFlow** |
| Web and mobile application development tool | **AWS Amplify** |
| Automatically start/stop services to reduce costs | **Instance Scheduler on AWS** |

---

## ⚠️ CRITICAL EXAM TIPS

### Service Selection Logic

1. **Real-time vs Near Real-time**
   - Real-time → **Kinesis Data Streams**
   - Near real-time → **Amazon Data Firehose**

2. **Database Selection**
   - SQL/Relational → **RDS**
   - Cloud-optimized SQL → **Aurora**
   - NoSQL/Flexible Schema → **DynamoDB**
   - MongoDB → **DocumentDB**
   - Graph → **Neptune**
   - Cassandra → **Keyspaces**
   - Time Series → **Timestream**
   - Immutable Ledger → **QLDB** (⚠️ end of support 31 Jul 2025)
   - Caching (requires code changes) → **ElastiCache**

3. **Analytics Selection**
   - Query S3 → **Athena**
   - Data Warehouse → **Redshift**
   - Query S3 from Redshift → **Redshift Spectrum**
   - Big Data/Hadoop → **EMR**
   - Search → **OpenSearch**
   - BI Dashboards → **QuickSight**
   - ETL → **Glue**

4. **ML Service Selection**
   - Images/Videos → **Rekognition**
   - Speech to Text → **Transcribe**
   - Text to Speech → **Polly**
   - Translation → **Translate**
   - Chatbots (ASR + NLU) → **Lex**
   - Contact Center → **Connect**
   - NLP → **Comprehend**
   - Custom ML → **SageMaker**
   - Document Search → **Kendra**
   - Recommendations → **Personalize**
   - Document Extraction → **Textract**

5. **Monitoring Selection**
   - Metrics/Logs → **CloudWatch**
   - API Auditing → **CloudTrail**
   - Configuration Compliance → **Config**
   - Scheduled Tasks → **EventBridge**

6. **Storage Selection**
   - Object Storage → **S3**
   - File System (Linux, multiple EC2) → **EFS**
   - Block Storage (single EC2) → **EBS**
   - Ephemeral High Performance → **Instance Store**
   - Data Migration (petabytes) → **Snowball**
   - Edge Computing → **Snowball Edge**

7. **Load Balancer Selection**
   - HTTP/HTTPS, Layer 7 → **ALB**
   - TCP/UDP, Layer 4, high performance → **NLB**
   - Network virtual appliances → **GWLB**

8. **Security Selection**
   - Key Management → **KMS**
   - Secrets with rotation → **Secrets Manager**
   - Configuration/Secrets (no rotation) → **SSM Parameter Store**
   - TLS Certificates → **ACM**
   - Web Application Firewall → **WAF**
   - DDoS Protection → **Shield**
   - Centralized Firewall Management → **Firewall Manager**
   - Threat Detection → **GuardDuty**
   - Security Assessment → **Inspector**
   - Data Discovery → **Macie**

9. **Disaster Recovery Selection**
   - Backup and Restore → **S3, Snapshots**
   - Pilot Light → **Minimal running system**
   - Warm Standby → **Scaled-down system**
   - Hot Site → **Full production**

10. **Networking Selection**
    - Private access to AWS services → **VPC Endpoints**
    - Connect VPCs → **VPC Peering, Transit Gateway**
    - On-premises to AWS → **VPN, Direct Connect**
    - Global traffic routing → **Route 53, Global Accelerator**
    - CDN → **CloudFront**

---

## 📋 QUICK REFERENCE BY CATEGORY

### Compute & Containers
- **EC2** - Virtual servers, various instance types
- **Docker** - Containerization
- **EKS** - Kubernetes on AWS
- **App2Container** - Java/.NET migration

### Storage & Databases
- **S3** - Object storage
- **EBS** - Block storage
- **EFS** - File system
- **RDS** - Relational databases
- **Aurora** - Cloud-optimized databases
- **DynamoDB** - NoSQL database
- **DocumentDB** - MongoDB compatible
- **Neptune** - Graph database
- **Keyspaces** - Cassandra compatible
- **Timestream** - Time series
- **QLDB** - Immutable ledger (⚠️ end of support 31 Jul 2025)
- **ElastiCache** - Caching (Redis/Memcached)
- **Snowball** - Data migration

### Analytics & Data Processing
- **Athena** - Query S3 with SQL
- **Redshift** - Data warehouse
- **Redshift Spectrum** - Query S3 from Redshift
- **EMR** - Big data processing
- **OpenSearch** - Search and analytics
- **QuickSight** - BI dashboards
- **Glue** - ETL service
- **Lake Formation** - Data lake security
- **Kinesis** - Real-time streaming

### Machine Learning
- **Rekognition** - Image/Video analysis
- **Transcribe** - Speech to text
- **Polly** - Text to speech
- **Translate** - Language translation
- **Lex** - Chatbots
- **Connect** - Contact center
- **Comprehend** - NLP
- **SageMaker** - Custom ML
- **Kendra** - Document search
- **Personalize** - Recommendations
- **Textract** - Document extraction

### Integration & Messaging
- **SQS** - Message queues
- **SNS** - Notifications
- **Kinesis Data Streams** - Real-time streaming
- **Kinesis Data Firehose** - Near real-time loading
- **EventBridge** - Event routing

### Monitoring & Auditing
- **CloudWatch** - Metrics, logs, alarms
- **CloudWatch Events/EventBridge** - Scheduled tasks
- **CloudTrail** - API auditing
- **Config** - Configuration compliance

### Networking
- **VPC** - Virtual private cloud
- **VPC Endpoints** - Private AWS access
- **Transit Gateway** - VPC connectivity hub
- **Direct Connect** - Dedicated connection
- **Route 53** - DNS service
- **CloudFront** - CDN
- **Global Accelerator** - Global traffic manager

### Security & Identity
- **IAM** - Identity and access management
- **KMS** - Key management
- **Secrets Manager** - Secrets with rotation
- **SSM Parameter Store** - Configuration storage
- **ACM** - Certificate management
- **WAF** - Web application firewall
- **Shield** - DDoS protection
- **Firewall Manager** - Centralized firewall management
- **GuardDuty** - Threat detection
- **Inspector** - Security assessment
- **Macie** - Data discovery
- **Cognito** - User authentication

### Disaster Recovery & Migration
- **AWS Backup** - Centralized backup
- **DMS** - Database migration
- **SCT** - Schema conversion
- **Snowball** - Data transfer
- **MGN** - Server migration (replaced SMS in 2022)

### Other Services
- **CloudFormation** - Infrastructure as code
- **SES** - Email service
- **Pinpoint** - Marketing communications (⚠️ end of support 30 Oct 2026)
- **SSM** - Systems management
- **Cost Explorer** - Cost management
- **Batch** - Batch processing
- **AppFlow** - SaaS integration
- **Amplify** - App development

---

*Last Updated: Based on AWS SAA-C03 Exam Guide - Comprehensive Mapping*
