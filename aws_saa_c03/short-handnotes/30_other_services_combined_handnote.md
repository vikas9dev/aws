# Other AWS Services - Handnote 📝

## 🏗️ AWS CLOUDFORMATION

### What is CloudFormation?
- **Infrastructure as Code**: Declarative infrastructure definition
- **Purpose**: Deploy and manage AWS resources
- **Format**: YAML or JSON templates
- **Scope**: AWS-only (native service)

### Benefits
- ✅ **Infrastructure as Code**: Version control, code review
- ✅ **Cost Management**: Tagged resources, easy cost tracking
- ✅ **Automation**: Delete/recreate for cost savings
- ✅ **Dependency Management**: Automatic ordering
- ✅ **Reusability**: Templates across environments/regions

### Key Concepts
- **Stack**: Collection of resources from template
- **Template**: YAML/JSON definition
- **Change Sets**: Preview changes before applying
- **Service Role**: IAM role for CloudFormation operations
- **iam:PassRole**: Required permission to use service roles

### CloudFormation vs Terraform
- **CloudFormation**: AWS-native, deep AWS integration
- **Terraform**: Multi-cloud, more flexible
- **Choose CloudFormation**: AWS-only environments

---

## 📧 AMAZON SES (SIMPLE EMAIL SERVICE)

### What is SES?
- **Email Service**: Send transactional and marketing emails
- **Cost-Effective**: Pay per email sent
- **Features**:
  - ✅ High deliverability
  - ✅ Bounce/complaint handling
  - ✅ Reputation management

### Use Cases
- Transactional emails (order confirmations)
- Marketing campaigns
- Automated notifications

---

## 📱 AMAZON PINPOINT

### What is Pinpoint?
- **Customer Engagement**: Multi-channel messaging
- **Channels**: Email, SMS, push notifications
- **Features**:
  - ✅ User segmentation
  - ✅ Campaign management
  - ✅ Analytics
- ⚠️ **End of support**: 30 Oct 2026 (closed to new customers May 2025). SMS/voice/push → AWS End User Messaging; email → SES

### Use Cases
- Marketing campaigns
- User notifications
- Customer engagement

---

## ⚙️ AWS SYSTEMS MANAGER (SSM)

### SSM Session Manager
- **Purpose**: Secure shell access without SSH keys
- **Benefits**:
  - ✅ No SSH keys needed
  - ✅ No open ports (22)
  - ✅ CloudTrail logging
- **Requirements**: SSM Agent, IAM role

### SSM Run Command
- **Purpose**: Execute commands on EC2 instances
- **Use Case**: Configuration management, patching

### Patch Manager
- **Purpose**: Automate patching across instances
- **Features**: Schedule patches, compliance reporting

### Maintenance Windows
- **Purpose**: Schedule maintenance tasks
- **Use Case**: Patching, updates during off-hours

### Automation
- **Purpose**: Automate operational tasks
- **Use Case**: Instance management, backups

---

## 💰 AWS COST EXPLORER

### What is Cost Explorer?
- **Cost Visualization**: Analyze AWS costs and usage
- **Features**:
  - ✅ Cost trends
  - ✅ Forecasts
  - ✅ Filtering by service, tag, etc.
- **Time Range**: 13 months of history by default (38 months opt-in); forecast up to 18 months (was 12 months before Nov 2025)

---

## 🚨 AWS COST ANOMALY DETECTION

### What is Cost Anomaly Detection?
- **Purpose**: Detect unusual spending patterns
- **Features**:
  - ✅ Automatic detection
  - ✅ Alerts via SNS
  - ✅ Root cause analysis
- **Use Case**: Identify unexpected cost increases

---

## 🏢 AWS OUTPOSTS

### What is Outposts?
- **Purpose**: Extend AWS to on-premises data center
- **Features**:
  - ✅ Same AWS services on-premises
  - ✅ Managed by AWS
  - ✅ Hybrid cloud
- **Use Cases**:
  - Low latency requirements
  - Data residency
  - Migration path

---

## 📦 AWS BATCH

### What is AWS Batch?
- **Purpose**: Run batch computing workloads
- **Features**:
  - ✅ Automatic scaling
  - ✅ Job scheduling
  - ✅ Multiple compute environments
- **Use Cases**:
  - Data processing
  - ETL jobs
  - Scientific computing

---

## 🔄 AMAZON APPFLOW

### What is AppFlow?
- **Purpose**: Integrate SaaS applications with AWS
- **Features**:
  - ✅ No code integration
  - ✅ Bidirectional data flow
  - ✅ Transformations
- **Connectors**: Salesforce, Slack, ServiceNow, etc.

---

## 🚀 AWS AMPLIFY

### What is Amplify?
- **Purpose**: Build and deploy web/mobile applications
- **Features**:
  - ✅ Hosting
  - ✅ CI/CD
  - ✅ Backend services
- **Use Cases**: React, Vue, Angular, mobile apps

---

## ⏰ INSTANCE SCHEDULER ON AWS

### What is Instance Scheduler?
- **Purpose**: Automatically start/stop EC2/RDS instances
- **Features**:
  - ✅ Schedule-based automation
  - ✅ Cost optimization
  - ✅ Tag-based targeting
- **Use Case**: Stop dev/test instances during off-hours

---

## ⚠️ CRITICAL EXAM POINTS

1. **CloudFormation**: Infrastructure as Code, AWS-native
2. **Service Role**: IAM role for CloudFormation, requires iam:PassRole
3. **SES**: Email sending service, cost-effective
4. **Pinpoint**: Multi-channel customer engagement (⚠️ end of support 30 Oct 2026)
5. **SSM Session Manager**: Secure shell without SSH keys
6. **SSM Run Command**: Execute commands remotely
7. **Patch Manager**: Automated patching
8. **Cost Explorer**: Cost visualization and analysis
9. **Cost Anomaly Detection**: Unusual spending alerts
10. **Outposts**: AWS services on-premises
11. **Batch**: Batch computing workloads
12. **AppFlow**: SaaS integration with AWS
13. **Amplify**: Web/mobile app development platform
14. **Instance Scheduler**: Automated start/stop for cost savings

---

## 📋 QUICK REFERENCE

### Infrastructure as Code
- **CloudFormation**: AWS-native, YAML/JSON
- **Terraform**: Multi-cloud, HCL

### Cost Management
- **Cost Explorer**: Analyze costs
- **Cost Anomaly Detection**: Detect unusual spending
- **Instance Scheduler**: Stop instances to save costs

### Operations
- **SSM Session Manager**: Secure access
- **SSM Run Command**: Remote execution
- **Patch Manager**: Automated patching

### Integration
- **AppFlow**: SaaS integration
- **Amplify**: App development platform

---

*Last Updated: Based on AWS SAA-C03 Exam Guide*

