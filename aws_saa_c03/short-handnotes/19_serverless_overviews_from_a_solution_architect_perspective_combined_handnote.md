# AWS Lambda & Serverless - Handnote 📝

## ⚡ AWS LAMBDA

### What is Lambda?
- **Serverless compute** service
- **Run code** without provisioning servers
- **Pay per request** & compute time
- **Automatic scaling**

### Supported Runtimes
- **Node.js, Python, Ruby, Java, Go, .NET, Custom Runtime**
- **Container images** (up to 10 GB)

### Lambda Limits
- **Memory**: 128 MB - 10 GB (in 1 MB increments)
- **Timeout**: 15 minutes (900 seconds)
- **Ephemeral storage**: 512 MB - 10 GB
- **Deployment package**: 50 MB (zipped), 250 MB (unzipped)
- **Container images**: Up to 10 GB

### Lambda Pricing
- **Requests**: First 1M requests/month FREE
- **Compute**: $0.0000166667 per GB-second
- **Free tier**: 1M requests, 400,000 GB-seconds/month

---

## 🔧 LAMBDA FEATURES

### Environment Variables
- **Key-value pairs** for configuration
- **Encrypted** with KMS (optional)
- ✅ **Use for**: API keys, database URLs, feature flags

### Layers
- **Shared code** & libraries
- **Reusable** across functions
- **Up to 5 layers** per function
- ✅ **Use for**: Common libraries, custom runtimes

### Versions & Aliases
- **Versions**: Immutable snapshots
- **Aliases**: Pointers to versions
- ✅ **Use for**: Blue/green deployments, canary releases

### Reserved Concurrency
- **Reserve** concurrent executions
- **Prevents** other functions from using capacity
- ✅ **No extra charge** to configure (but it leaves less concurrency for other functions)

### Provisioned Concurrency
- **Pre-warm** functions (no cold start)
- **Guaranteed** capacity
- 💰 **Additional cost**

---

## 🔗 LAMBDA INTEGRATIONS

### API Gateway
- **REST APIs** & **HTTP APIs**
- **Request/response** transformation
- ✅ **Use for**: Serverless APIs

### Application Load Balancer
- **Target type**: Lambda function
- **HTTP/HTTPS** requests
- ✅ **Use for**: Serverless web applications

### S3
- **Event notifications** trigger Lambda
- **Object created/deleted** events
- ✅ **Use case**: Image processing, file validation

### DynamoDB
- **Streams** trigger Lambda
- **Real-time processing**
- ✅ **Use case**: Data transformation, analytics

### SQS
- **Message queue** triggers Lambda
- **Batch processing**
- ✅ **Use case**: Async processing, decoupling

### SNS
- **Topic notifications** trigger Lambda
- **Pub/sub** messaging
- ✅ **Use case**: Event-driven architectures

### EventBridge
- **Event bus** triggers Lambda
- **Custom events**
- ✅ **Use case**: Event-driven workflows

### CloudWatch Events/EventBridge
- **Scheduled** execution (cron)
- **Event-based** triggers
- ✅ **Use case**: Scheduled tasks, automation

---

## 🛡️ LAMBDA SECURITY

### IAM Roles
- ✅ **Execution role**: Permissions Lambda needs
- ✅ **Resource-based policies**: Who can invoke function
- ⚠️ **Never use IAM users** for Lambda

### VPC Access
- ✅ **Deploy in VPC** for private resource access
- ℹ️ **Cold starts** in a VPC are no longer much slower (shared Hyperplane ENIs since Sep 2019; older material says they are)
- ✅ **Use VPC endpoints** to avoid NAT Gateway

### Encryption
- ✅ **Encrypt environment variables** with KMS
- ✅ **Encrypt code** at rest (automatic)
- ✅ **Encrypt in transit** (HTTPS)

---

## 📊 LAMBDA MONITORING

### CloudWatch Logs
- ✅ **Automatic logging** to CloudWatch Logs
- ✅ **Log groups** created automatically
- ✅ **Retention**: 1 day - Never (configurable)

### CloudWatch Metrics
- **Invocations**: Number of times function invoked
- **Duration**: Execution time
- **Errors**: Number of errors
- **Throttles**: Number of throttled requests
- **Concurrent Executions**: Current concurrent executions

### X-Ray Integration
- ✅ **Distributed tracing**
- ✅ **Performance analysis**
- ✅ **Debugging** complex applications

---

## ⚡ LAMBDA BEST PRACTICES

### Performance
- ✅ **Choose right memory** (CPU scales with memory)
- ✅ **Use layers** for common dependencies
- ✅ **Optimize package size** (faster cold starts)
- ✅ **Use provisioned concurrency** for low latency needs
- ✅ **Avoid VPC** unless necessary (since Sep 2019 the VPC cold-start penalty is mostly gone, but you need NAT or VPC endpoints for internet/AWS access)

### Security
- ✅ **Least privilege** IAM roles
- ✅ **Encrypt** environment variables
- ✅ **Use VPC endpoints** instead of NAT Gateway
- ✅ **Scan code** for vulnerabilities

### Cost Optimization
- ✅ **Right-size memory** allocation
- ✅ **Optimize code** (faster execution = lower cost)
- ✅ **Use reserved concurrency** wisely
- ✅ **Monitor** unused functions

---

## 🚀 SERVERLESS ARCHITECTURE

### Benefits
- ✅ **No server management**
- ✅ **Automatic scaling**
- ✅ **Pay per use**
- ✅ **High availability**

### Common Patterns
- **API Backend**: API Gateway + Lambda
- **Event Processing**: S3 → Lambda → DynamoDB
- **Scheduled Tasks**: EventBridge → Lambda
- **Data Processing**: Kinesis → Lambda → S3

---

## ⚠️ CRITICAL EXAM POINTS

1. **Lambda timeout**: 15 minutes max
2. **Lambda memory**: 128 MB - 10 GB (CPU scales with memory)
3. **Cold starts**: First invocation slower (use provisioned concurrency)
4. **IAM roles**: Use for Lambda (not IAM users)
5. **VPC access**: Use VPC endpoints or NAT for outbound access (VPC cold starts are no longer much slower since Sep 2019)
6. **Layers**: Shared code/libraries (up to 5 per function)
7. **Reserved concurrency**: Prevents other functions from using capacity
8. **Environment variables**: Encrypted with KMS (optional)
9. **API Gateway**: REST/HTTP APIs with Lambda
10. **Event sources**: S3, DynamoDB, SQS, SNS, EventBridge, etc.

---

## 📋 QUICK REFERENCE

### Lambda Handler
```python
def lambda_handler(event, context):
    # event: Input data
    # context: Runtime information
    return {
        'statusCode': 200,
        'body': 'Hello from Lambda!'
    }
```

### Common Integrations
- **API Gateway** → REST/HTTP APIs
- **S3** → Object processing
- **DynamoDB** → Stream processing
- **SQS** → Message processing
- **EventBridge** → Scheduled/event-driven

---

*Last Updated: Based on AWS SAA-C03 Exam Guide*

