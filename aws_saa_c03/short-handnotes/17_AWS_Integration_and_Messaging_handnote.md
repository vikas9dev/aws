# AWS Integration & Messaging - Handnote 📝

## 📨 AMAZON SQS (Simple Queue Service)

### What is SQS?
- **Message queue** service
- **Decouple** application components
- **Fully managed**
- **Scalable** & **reliable**

### Queue Types

#### Standard Queue
- ✅ **Unlimited throughput**
- ✅ **At-least-once delivery**
- ✅ **Best-effort ordering**
- ⚠️ **Messages may be duplicated**

#### FIFO Queue
- ✅ **Exactly-once delivery**
- ✅ **First-In-First-Out** ordering
- ⚠️ **300 messages/second** (with batching: 3,000/second)
- ✅ **Message groups**: Parallel processing

### SQS Features
- **Visibility Timeout**: How long message hidden after receive
- **Message Retention**: 1 minute - 14 days (default 4 days)
- **Dead Letter Queue (DLQ)**: Failed messages
- **Long Polling**: Wait up to 20 seconds for messages
- **Short Polling**: Immediate return (may be empty)

### SQS Pricing
- **First 1M requests/month**: FREE
- **Standard**: $0.40 per million requests
- **FIFO**: $0.50 per million requests

---

## 📢 AMAZON SNS (Simple Notification Service)

### What is SNS?
- **Pub/sub** messaging service
- **Push notifications**
- **Fan-out** pattern
- **Fully managed**

### SNS Features
- ✅ **Topics**: Communication channels
- ✅ **Subscriptions**: Email, SMS, HTTP/HTTPS, SQS, Lambda, etc.
- ✅ **Message filtering**: Filter by attributes
- ✅ **FIFO Topics**: Exactly-once delivery, ordering (subscribers must be SQS queues)

### SNS Integrations
- **Email/Email-JSON**: Email notifications
- **SMS**: Text messages
- **HTTP/HTTPS**: Webhook endpoints
- **SQS**: Queue messages
- **Lambda**: Invoke functions
- **Mobile Push**: APNS, FCM, etc.

### SNS Pricing
- **First 1M requests/month**: FREE
- **$0.50 per million** requests
- **SMS**: Varies by country

---

## 🔄 SQS vs SNS

| Feature | SQS | SNS |
|---------|-----|-----|
| **Pattern** | Point-to-point | Pub/sub |
| **Delivery** | Pull (polling) | Push (notifications) |
| **Consumers** | One consumer per message | Multiple subscribers |
| **Use Case** | Decoupling, async processing | Notifications, fan-out |

---

## 🔗 AWS EVENTBRIDGE

### What is EventBridge?
- **Serverless event bus**
- **Event-driven** architecture
- **Route events** to targets
- **Schema registry**

### Event Sources
- **AWS Services**: S3, EC2, Lambda, etc.
- **Custom Applications**: Via API
- **SaaS Partners**: Datadog, PagerDuty, etc.
- **EventBridge Schema Registry**: Discover & manage schemas

### EventBridge Rules
- **Event Pattern**: Filter events
- **Schedule**: Cron or rate expressions
- **Targets**: Lambda, SNS, SQS, Step Functions, etc.

### EventBridge vs CloudWatch Events
- **EventBridge**: Enhanced version of CloudWatch Events
- **More features**: Schema registry, custom buses, etc.
- **CloudWatch Events**: Legacy name (still works)

---

## 🔀 AWS STEP FUNCTIONS

### What is Step Functions?
- **Visual workflow** service
- **Orchestrate** Lambda functions & AWS services
- **State machines**
- **Error handling** & **retries**

### State Types
- **Task**: Work unit (Lambda, ECS, etc.)
- **Choice**: Conditional branching
- **Parallel**: Parallel execution
- **Wait**: Delay execution
- **Succeed/Fail**: End states
- **Pass**: Transform data

### Use Cases
- **Workflow orchestration**
- **Microservices** coordination
- **ETL pipelines**
- **Human approval** workflows

---

## 📡 AMAZON MQ

### What is Amazon MQ?
- **Managed message broker**
- **Apache ActiveMQ** & **RabbitMQ**
- **Protocol support**: MQTT, AMQP, STOMP, OpenWire
- **Use case**: Migrate existing messaging systems

### When to Use
- ✅ **Existing messaging** applications
- ✅ **Protocol requirements** (MQTT, AMQP)
- ✅ **Migration** from on-premises

### When NOT to Use
- ❌ **New applications** → Use SQS/SNS instead
- ❌ **Simple use cases** → SQS/SNS simpler

---

## 🔄 AMAZON KINESIS

### What is Kinesis?
- **Real-time streaming data platform** for collecting, processing, and analyzing streaming data
- **Multiple services**: Data Streams (real-time streaming), Data Firehose (data loading), Data Analytics (real-time analytics — now Amazon Managed Service for Apache Flink)

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
- **SQL queries** on streams — ⚠️ Kinesis Data Analytics for SQL was discontinued on 27 Jan 2026. Use Amazon Managed Service for Apache Flink (renamed from Kinesis Data Analytics for Apache Flink in Aug 2023) or Flink Studio for SQL. Older exam questions may still name it.
- **Use cases**: Real-time dashboards, alerts

---

## ⚠️ CRITICAL EXAM POINTS

1. **SQS**: Message queue, decoupling, pull model
2. **SNS**: Pub/sub, notifications, push model
3. **Standard SQS**: Unlimited throughput, may duplicate
4. **FIFO SQS**: Exactly-once, 300 msg/sec, ordered
5. **SNS Fan-out**: One message to multiple subscribers
6. **EventBridge**: Event-driven architecture, serverless
7. **Step Functions**: Workflow orchestration, visual
8. **Amazon MQ**: Managed message broker (migration use case)
9. **Kinesis**: Real-time streaming data processing
10. **SQS Long Polling**: Wait up to 20 seconds (reduces costs)

---

## 📋 QUICK REFERENCE

### Choosing Messaging Service
- **Decoupling**: SQS
- **Notifications**: SNS
- **Event-driven**: EventBridge
- **Workflows**: Step Functions
- **Existing messaging**: Amazon MQ
- **Streaming**: Kinesis

### Common Patterns
- **S3 → SNS → SQS → Lambda**: Event processing
- **API Gateway → Lambda → SNS → Multiple subscribers**: Fan-out
- **EventBridge → Step Functions → Lambda**: Workflow orchestration

---

*Last Updated: Based on AWS SAA-C03 Exam Guide*

