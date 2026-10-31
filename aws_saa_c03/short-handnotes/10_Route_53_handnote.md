# AWS Route 53 - Handnote 📝

## 🌐 ROUTE 53 BASICS

### What is Route 53?
- **DNS (Domain Name System)** service
- **Domain registration**
- **Health checking & monitoring**
- **Traffic routing**

### Features
- ✅ **Highly available** & **scalable**
- ✅ **Global service** (not region-specific)
- ✅ **Low latency** (DNS queries answered from edge locations)
- ✅ **Integration** with other AWS services

---

## 📋 DNS RECORD TYPES

### A Record
- **IPv4 address** mapping
- **Example**: `example.com` → `192.0.2.1`

### AAAA Record
- **IPv6 address** mapping
- **Example**: `example.com` → `2001:0db8::1`

### CNAME Record
- **Canonical name** (alias)
- **Points to another domain name**
- ⚠️ **Cannot use for root domain** (use A/AAAA or Alias)

### Alias Record
- ✅ **AWS-specific** record type
- ✅ **Can use for root domain**
- ✅ **Free** (no queries charged)
- ✅ **Works with**: ELB, CloudFront, S3, API Gateway, etc.

### NS Record
- **Name Server** records
- **Delegates subdomain** to other name servers

### SOA Record
- **Start of Authority**
- **Contains admin info** about domain

### MX Record
- **Mail Exchange** records
- **Email routing**

### TXT Record
- **Text records**
- **SPF, DKIM, verification** records

### PTR Record
- **Reverse DNS** lookup
- **IP to domain** mapping

### SRV Record
- **Service records**
- **Define service location** (port, priority, weight)

### CAA Record
- **Certificate Authority Authorization**
- **Controls which CAs** can issue certificates

---

## 🗺️ ROUTING POLICIES

### Simple Routing
- **One record** with multiple values
- **Random selection** from values
- ✅ **Use case**: Single region, multiple IPs

### Weighted Routing
- **Assign weights** to records
- **Traffic distributed** based on weights
- ✅ **Use case**: Testing, gradual migration
- **Example**: 70% to prod, 30% to test

### Latency-Based Routing (LBR)
- **Route to region** with lowest latency
- **Based on user location**
- ✅ **Use case**: Multi-region applications

### Failover Routing
- **Active-Passive** setup
- **Primary** record (healthy)
- **Secondary** record (failover)
- ✅ **Use case**: Disaster recovery

### Geolocation Routing
- **Route based on user's geographic location**
- **Country/continent** level
- ✅ **Use case**: Content localization, compliance

### Geoproximity Routing
- **Route based on geographic location** + **bias**
- **Bias** can shift traffic to/from regions
- Since Jan 2024: create it as a normal record (Traffic Flow no longer required)
- ✅ **Use case**: Fine-grained traffic control

### Multivalue Answer Routing
- **Multiple healthy records** returned
- **Random selection** from healthy records
- ✅ **Use case**: Simple load balancing, high availability

### IP-Based Routing
- **Route based on user's IP address**
- **CIDR blocks** mapped to endpoints
- ✅ **Use case**: Route to specific endpoints based on IP

---

## 🏥 HEALTH CHECKS

### Purpose
- **Monitor endpoint health**
- **Automatic failover**
- **Route only to healthy** resources

### Health Check Types
- **HTTP/HTTPS**: Check HTTP status code
- **TCP**: Check TCP connection
- **CALCULATED**: Combine multiple health checks
- **CLOUDWATCH ALARM**: Based on CloudWatch metric

### Health Check Configuration
- **Interval**: 10 or 30 seconds
- **Failure threshold**: 1-10 consecutive failures
- **Request interval**: 10 or 30 seconds
- **Health check regions**: 3+ regions (recommended)

### Calculated Health Checks
- **Combine multiple** health checks
- **Child health checks** (must pass)
- **Health check regions**: 3+ regions
- ✅ **Use case**: Complex health evaluation

---

## 🔗 ROUTE 53 INTEGRATIONS

### ELB Integration
- **Alias record** points to ELB
- **Automatic health checking**
- ✅ **Free** (no queries charged)

### S3 Static Website
- **Alias record** points to S3 website endpoint
- ✅ **Use case**: Static website hosting
- For **S3 static website hosting**, the bucket name must **exactly match the domain name** (for example, `example.com`). This is required because Route 53 uses an **Alias record** that points to the S3 website endpoint, which is tied to the bucket name.

### CloudFront
- **Alias record** points to CloudFront distribution
- ✅ **Global distribution**

### API Gateway
- **Alias record** points to API Gateway
- ✅ **RESTful APIs**

### EC2 Instance
- **A record** points to Elastic IP
- ⚠️ **Not recommended** (use ELB instead)

---

## 🌍 PRIVATE HOSTED ZONES

### Purpose
- **Private DNS** for VPC
- **Resolve internal** domain names
- **Not accessible** from internet

### Use Cases
- **Internal applications**
- **Service discovery**
- **Private resources**

### Setup
1. Create **private hosted zone**
2. Associate with **VPC**
3. Create **private records**

---

## 💰 ROUTE 53 PRICING

### Hosted Zones
- **$0.50 per hosted zone** per month

### Queries
- **First 1 billion queries/month**: $0.40 per million
- **Over 1 billion**: $0.20 per million

### Health Checks
- **Basic**: $0.50 per health check per month
- **Advanced**: Additional charges

### Alias Records
- ✅ **FREE** (no query charges)

---

## ⚠️ CRITICAL EXAM POINTS

1. **Route 53 is global** - not region-specific
2. **Alias records** - Free, can use for root domain, AWS services only
3. **CNAME** - Cannot use for root domain (use Alias instead)
4. **Health checks** - Monitor endpoint health, automatic failover
5. **Weighted routing** - Distribute traffic by weights (testing, migration)
6. **Latency-based routing** - Route to lowest latency region
7. **Failover routing** - Active-passive disaster recovery
8. **Geolocation routing** - Route by user's geographic location
9. **Multivalue answer** - Multiple healthy records, random selection
10. **Private hosted zones** - Private DNS for VPC

---

## 📋 QUICK REFERENCE

### Choosing Routing Policy
- **Single region**: Simple
- **Testing/Migration**: Weighted
- **Multi-region, low latency**: Latency-Based
- **Disaster recovery**: Failover
- **Content localization**: Geolocation
- **Simple load balancing**: Multivalue Answer

### Health Check Best Practices
- ✅ **3+ health check regions** (recommended)
- ✅ **Monitor from multiple locations**
- ✅ **Set appropriate failure threshold**
- ✅ **Use calculated health checks** for complex scenarios

---

*Last Updated: Based on AWS SAA-C03 Exam Guide*

