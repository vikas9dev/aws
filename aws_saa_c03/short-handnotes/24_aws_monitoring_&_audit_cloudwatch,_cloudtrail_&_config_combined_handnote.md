# AWS Monitoring & Audit - Handnote 📝

## 📊 AMAZON CLOUDWATCH

### Purpose
- **Performance monitoring** (metrics, logs, alarms)
- **Real-time visibility** into AWS resources
- **Dashboards** for visualization

### CloudWatch Metrics
- **Namespaces**: One per AWS service (e.g., `AWS/EC2`, `AWS/S3`)
- **Dimensions**: Attributes of metric (up to 30 per metric)
- **Time-based**: Must include timestamp
- **Data Granularity**:
  - **Standard**: 5-minute intervals (default)
  - **Detailed**: 1-minute intervals (enable detailed monitoring)

### Custom Metrics
- ✅ Create your own metrics
- ✅ Example: Memory usage from EC2 instance
- ✅ Use `PutMetricData` API

### CloudWatch Dashboards
- ✅ Visualize multiple metrics
- ✅ Share dashboards
- ✅ Export data as CSV

### Streaming Metrics
- ✅ Stream to **Kinesis Data Firehose**
- ✅ Destinations: S3, Redshift, OpenSearch, third-party (Datadog, Splunk, etc.)

---

## 📝 CLOUDWATCH LOGS

### Components
- **Log Groups**: Applications (e.g., `/aws/lambda/my-function`)
- **Log Streams**: Instances within log group
- **Retention**: Indefinite to 1 day - 10 years

### Sending Logs
- **CloudWatch Unified Agent** (recommended)
- **CloudWatch Logs Agent** (deprecated)
- **SDK**: Direct API calls

### AWS Service Integration
- ✅ **Elastic Beanstalk**: Application logs
- ✅ **ECS**: Container logs
- ✅ **Lambda**: Function logs
- ✅ **VPC Flow Logs**: Network traffic
- ✅ **API Gateway**: Request logs
- ✅ **CloudTrail**: Filtered logs
- ✅ **Route 53**: DNS queries

### Log Destinations
- **S3**: Batch export
- **Kinesis Data Streams**: Streaming
- **Kinesis Data Firehose**: Streaming
- **Lambda**: Processing
- **OpenSearch**: Analysis

### CloudWatch Logs Insights
- ✅ **Query language** for log analysis
- ✅ **Automatic field detection**
- ✅ **Query multiple log groups** (even cross-account)
- ✅ **Save queries** and add to dashboards

### Example Query
```
fields @timestamp, @message, @logStream
| filter @message like /ERROR/
| sort @timestamp desc
| limit 100
```

---

## 🚨 CLOUDWATCH ALARMS

### Purpose
- **Monitor metrics** and trigger actions
- **State Changes**: OK → ALARM → OK

### Alarm States
- **OK**: Metric within threshold
- **ALARM**: Metric breached threshold
- **INSUFFICIENT_DATA**: Not enough data

### Alarm Actions
- **SNS**: Send notifications
- **Auto Scaling**: Scale up/down
- **EC2 Actions**: Stop, terminate, reboot
- **Systems Manager**: Run automation

### Alarm Types
- **Threshold**: Simple threshold
- **Anomaly Detection**: ML-based detection
- **Composite**: Combines the states of other alarms (AND / OR)

---

## 📅 AMAZON EVENTBRIDGE

### Purpose
- **Event-driven architecture**
- **Serverless event bus**
- **Route events** to targets

### Event Sources
- ✅ **AWS Services**: S3, EC2, Lambda, etc.
- ✅ **Custom Applications**: Via API
- ✅ **SaaS Partners**: Datadog, PagerDuty, etc.
- ✅ **EventBridge Schema Registry**

### EventBridge Rules
- **Event Pattern**: Filter events
- **Schedule**: Cron or rate expressions
- **Targets**: Lambda, SNS, SQS, Step Functions, etc.

### Use Cases
- **Event-driven workflows**
- **Scheduled tasks** (cron jobs)
- **Cross-service integration**
- **Custom business logic**

---

## 🕵️ AWS CLOUDTRAIL

### Purpose
- **Audit & compliance**
- **Track API activity** (who, what, when, where)
- **Governance & forensic analysis**

### Event Types

#### Management Events
- **Operations on resources** (default logged)
- **Read Events**: Don't modify (e.g., ListUsers, DescribeInstances)
- **Write Events**: May modify (e.g., CreateUser, DeleteTable)

#### Data Events
- **Not logged by default** (high volume)
- **S3 object-level**: GetObject, PutObject, DeleteObject
- **Lambda Invoke**: Function execution

#### CloudTrail Insights Events
- **Anomaly detection** (optional, additional cost)
- **Unusual activity**: Bursts, service limits, gaps in maintenance

### CloudTrail Features
- ✅ **Global service** (all regions)
- ✅ **Multi-region trails** (recommended)
- ✅ **Log file validation** (integrity)
- ✅ **Log file encryption** (KMS)
- ✅ **S3 integration** (long-term storage) - CloudTrail keeps event history for 90 days; long-term retention requires exporting logs to Amazon S3.
- ✅ **CloudWatch Logs integration** (real-time analysis)

### CloudTrail Insights
- ✅ **Baseline establishment** of normal activity
- ✅ **Continuous analysis** of write events
- ✅ **Anomaly detection** (deviations from baseline)

---

## 🛡️ AWS CONFIG

### Purpose
- **Configuration compliance** & history
- **Resource configuration** tracking
- **Configuration drift** detection

### Features
- ✅ **Point-in-time snapshots** of configuration
- ✅ **Timeline** of changes
- ✅ **Compliance checking** against rules
- ✅ **Configuration history** (what changed, when)

### Config Rules
- **Managed Rules**: AWS-provided (e.g., encrypted volumes)
- **Custom Rules**: Lambda-based custom logic
- **Compliance**: Compliant/Non-compliant status

### Use Cases
- **Security compliance**: Ensure encryption enabled
- **Governance**: Track configuration changes
- **Audit**: Historical configuration data

---

## 🔍 CLOUDWATCH vs CLOUDTRAIL vs CONFIG

| Service | Purpose | Data Type | Use Case |
|---------|---------|-----------|----------|
| **CloudWatch** | Performance monitoring | Metrics, logs, alarms | How is system performing? |
| **CloudTrail** | API activity audit | API calls (who, what, when) | Who did what? |
| **Config** | Configuration compliance | Configuration snapshots | What changed? Is it compliant? |

### Example (ELB)
- **CloudWatch**: Monitor request count, 4XX/5XX errors, latency
- **CloudTrail**: Who changed security group? Who modified SSL cert?
- **Config**: Track SSL cert updates, ensure HTTPS only, no HTTP

---

## ⚠️ CRITICAL EXAM POINTS

1. **CloudWatch**: Performance monitoring (metrics, logs, alarms)
2. **CloudTrail**: API activity audit (who, what, when, where)
3. **Config**: Configuration compliance & history
4. **CloudWatch Metrics**: Standard (5 min) vs Detailed (1 min)
5. **CloudWatch Logs**: Log Groups → Log Streams
6. **CloudTrail**: Management events (default), Data events (opt-in)
7. **CloudTrail Insights**: Anomaly detection (additional cost)
8. **Config Rules**: Managed (AWS) or Custom (Lambda)
9. **EventBridge**: Event-driven architecture, scheduled tasks
10. **CloudWatch Alarms**: OK, ALARM, INSUFFICIENT_DATA states

---

## 📋 QUICK REFERENCE

### CloudWatch Logs Metric Filter
- Filter logs for keywords (e.g., "ERROR")
- Create metric from filter
- Create alarm on metric

### CloudTrail Best Practices
- ✅ Enable **multi-region trails**
- ✅ Enable **log file validation**
- ✅ Enable **log file encryption**
- ✅ Send to **S3** for long-term storage
- ✅ Send to **CloudWatch Logs** for real-time analysis

### Config Best Practices
- ✅ Enable **recording** for all resources
- ✅ Use **managed rules** for common compliance
- ✅ Create **custom rules** for specific requirements
- ✅ Set up **SNS notifications** for non-compliance

---

*Last Updated: Based on AWS SAA-C03 Exam Guide*

