# More Solution Architectures - Handnote 📝

## 🔔 EVENT PROCESSING IN AWS

### SQS and Lambda
- **Pattern**: SQS Queue → Lambda Function
- **Retry**: Messages returned to queue on failure
- ⚠️ **Infinite Loop Risk**: Failed messages can loop
- 💡 **Solution**: Dead Letter Queue (DLQ) after N retries

### SQS FIFO and Lambda
- **Ordering**: First In, First Out processing
- ⚠️ **Blocking**: One failed message blocks queue
- 💡 **Solution**: DLQ for failed messages

### SNS and Lambda
- **Pattern**: SNS Topic → Lambda Function
- **Retries**: Lambda retries 2 times (3 attempts in total)
- **DLQ**: Configure at Lambda level (not SNS)
- **Default**: Failed messages discarded after the retries

### Fan-Out Pattern
- **Problem**: Deliver to multiple SQS queues
- **Solution**: SNS Topic → Multiple SQS Queues
- **Benefits**: 
  - ✅ Higher delivery guarantee
  - ✅ Single message to SNS
  - ✅ SNS fans out to all subscribers

### S3 Event Notifications
- **Events**: Object creation, removal, restoration
- **Destinations**: SNS, SQS, Lambda, EventBridge
- **Filtering**: By object name (e.g., *.jpg)
- **Use Case**: Generate thumbnails on upload

### Amazon EventBridge
- **All S3 Events**: Sent to EventBridge once you turn on EventBridge notifications for the bucket (off by default)
- **Routing**: Rules to 18+ AWS services
- **Features**:
  - ✅ Advanced filtering (metadata, size, name)
  - ✅ Multiple destinations
  - ✅ Event archiving/replay
- **CloudTrail Integration**: Intercept API calls

### External Events
- **Pattern**: API Gateway → Kinesis Data Streams → Firehose → S3
- **Use Case**: Ingest external events into AWS

---

## 💾 CACHING STRATEGIES

### Caching Layers

#### CloudFront 🌐
- **Location**: Edge locations
- **Benefit**: Fast response (cache hit)
- **Drawback**: Stale content
- **Solution**: TTL configuration

#### API Gateway 🚪
- **Location**: Regional cache
- **Scope**: Regional (not global)
- **Benefit**: Reduce backend load

#### Application Logic ⚙️
- **Caches**: Redis, Memcached, DAX
- **Purpose**: Avoid repeated database queries
- **Benefit**: Reduce database pressure

#### Database & S3 🗄️
- **Note**: No built-in caching
- **Solution**: Use external caches

### Considerations
- **Where to cache**: Edge, regional, application
- **What to cache**: Static vs dynamic content
- **TTL**: How long to cache
- **Latency**: Acceptable response time

---

## 🛡️ BLOCKING IP ADDRESSES

### EC2 in Public Subnet
1. **NACL**: First line of defense (allow/deny rules)
2. **Security Group**: Second line (allow rules only)
3. **Firewall Software**: Optional (on instance, CPU cost)

### ALB and EC2
1. **EC2 Security Group**: Allow only from ALB
2. **ALB Security Group**: Filter at ALB level
3. **NACL**: On public subnet
4. **Connection Termination**: ALB terminates, new connection to EC2

### Web Application Firewall (WAF)
- **ALB + WAF**: Application-level filtering
- **CloudFront + WAF**: Edge-level filtering
- **Features**: IP filtering, geo-restriction, rate limiting
- ⚠️ **Cost**: Additional charges

### CloudFront Considerations
- **Client IP**: Not directly visible (CloudFront IPs)
- **Security Group**: Allow CloudFront IP ranges only
- **Geo Restriction**: Block by country
- **WAF**: IP filtering at CloudFront level

---

## ⚡ HIGH PERFORMANCE COMPUTING (HPC)

### Benefits
- ✅ Rapid resource provisioning
- ✅ Pay per use
- ✅ Destroy when done (no ongoing costs)

### Use Cases
- Genomics, chemistry, risk modeling
- Weather prediction, ML/DL
- Autonomous driving

### Data Transfer
- **Direct Connect**: GB/s, private network
- **Snowball/Snowmobile**: Petabytes, physical transport (⚠️ Snowmobile retired 2024; Snowball Edge closed to new customers Nov 2025)
- **DataSync**: File system sync (NFS, SMB)

### Compute & Networking
- **EC2**: CPU/GPU optimized instances
- **Spot Instances**: Cost savings
- **Auto Scaling**: Scale based on demand
- **Placement Groups (Cluster)**: Low latency, same rack

### Enhanced Networking
- **ENA (Elastic Network Adapter)**: Up to 100 Gbps
- **EFA (Elastic Fabric Adapter)**: For HPC, Linux only
  - **MPI**: Message Passing Interface
  - **Use Case**: Tightly coupled workloads

### Storage
- **Instance Store**: High IOPS (ephemeral)
- **EBS**: Persistent, network-attached
- **EFS**: Shared file system
- **FSx for Lustre**: High-performance file system

---

## 🚀 MAKING EC2 HIGHLY AVAILABLE

### Architecture Components
- **Multi-AZ**: Deploy across Availability Zones
- **Load Balancer**: Distribute traffic
- **Auto Scaling**: Maintain desired capacity
- **Health Checks**: Remove unhealthy instances

### Best Practices
- ✅ No single point of failure
- ✅ Automatic failover
- ✅ Health monitoring
- ✅ Auto-recovery

---

## ⚠️ CRITICAL EXAM POINTS

1. **SQS + Lambda**: Use DLQ to prevent infinite loops
2. **SNS + Lambda**: DLQ configured at Lambda level
3. **Fan-Out**: SNS → Multiple SQS queues
4. **EventBridge**: All S3 events (after you turn it on for the bucket)
5. **Caching**: CloudFront (edge), API Gateway (regional), Application (Redis/DAX)
6. **IP Blocking**: NACL → Security Group → WAF
7. **CloudFront**: Client IP not visible, use WAF/Geo Restriction
8. **HPC**: ENA (100 Gbps), EFA (HPC, Linux, MPI)
9. **Placement Groups**: Cluster for low latency
10. **High Availability**: Multi-AZ + ELB + ASG

---

## 📋 QUICK REFERENCE

### Event Processing Patterns
- **Queue Processing**: SQS → Lambda (with DLQ)
- **Pub/Sub**: SNS → Multiple subscribers
- **Fan-Out**: SNS → Multiple SQS queues
- **S3 Events**: S3 → SNS/SQS/Lambda/EventBridge

### Caching Strategy
- **Global Static**: CloudFront
- **Regional API**: API Gateway cache
- **Application**: Redis/Memcached/DAX

### Security Layers
- **Network**: NACL
- **Instance**: Security Group
- **Application**: WAF
- **Edge**: CloudFront + WAF

---

*Last Updated: Based on AWS SAA-C03 Exam Guide*

