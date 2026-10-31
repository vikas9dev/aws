# AWS Security & Encryption - Handnote 📝

## 🔐 ENCRYPTION MECHANISMS

### Encryption in Flight (TLS/SSL)
- Data encrypted BEFORE sending, decrypted AFTER receiving
- Prevents man-in-the-middle attacks
- HTTPS = TLS encryption

### Server-Side Encryption at Rest
- Data encrypted AFTER received by server
- Decrypted BEFORE sending to client
- Uses data keys (DEK)
- Example: S3 SSE

### Client-Side Encryption
- Encrypt/decrypt on client side
- Server NEVER sees unencrypted data
- Use when you don't trust the server

---

## 🔑 AWS KMS (Key Management Service)

### What is KMS?
- **Managed encryption service** for creating and controlling encryption keys
- **Centralized key management** for AWS services and applications

### Key Types
- **Symmetric (AES-256)**: Single key for encrypt/decrypt (AWS services use this)
- **Asymmetric (RSA/ECC)**: Public key encrypt, private key decrypt

### KMS Key Categories
| Type | Cost | Naming | Usage |
|------|------|--------|-------|
| AWS Owned | FREE | Not visible | SSE-S3, SSE-DynamoDB |
| AWS Managed | FREE | `aws/<service>` | Service-specific only |
| Customer Managed | $1/month | Custom | Full control |

### Key Rotation
- **AWS Managed**: Auto-rotated every 1 year
- **Customer Managed**: Auto-rotation (1 year default, configurable 90-2560 days) OR on-demand
- **Imported Keys**: No auto-rotation; on-demand rotation (after importing new material) or manual
- ⚠️ **Key Point**: Old key versions retained for decryption - rotation NEVER breaks decryption
- Decryption fails ONLY if key is disabled/deleted

### KMS Scope & Pricing
- **Region-scoped**: Keys are per-region
- **Cross-region**: Must re-encrypt with destination region key
- **Pricing**: $1/month per key + $0.03 per 10,000 API calls
- **Audit**: All API calls logged in CloudTrail

### Multi-Region Keys
- Same key ID & material across regions
- Encrypt in one region, decrypt in another
- Use cases: Global DynamoDB, Global Aurora, client-side encryption
- ⚠️ NOT global - primary + replicas, each managed independently
- ⚠️ S3 treats multi-region keys as independent (still re-encrypts)

### Cross-Account Access
1. Modify AMI launch permissions
2. Share KMS key via key policy
3. IAM permissions in target account: `kms:DescribeKey`, `kms:ReEncryptFrom`, `kms:CreateGrant`, `kms:Decrypt`
4. Optional: Re-encrypt with target account's key

---

## 📦 SSM PARAMETER STORE

### What is SSM Parameter Store?
- **Centralized configuration management** service for storing application configuration and secrets
- **Hierarchical storage** with versioning and encryption support

### Features
- Secure storage for config & secrets
- IAM access control
- EventBridge notifications
- Serverless, scalable, durable
- Version tracking
- CloudFormation integration
- **Region-specific** (like KMS)

### Parameter Types
- **String**: Plain text (4KB Standard, 8KB Advanced)
- **SecureString**: Encrypted with KMS

### Parameter Tiers
| Feature | Standard | Advanced |
|---------|----------|----------|
| Max parameters | 10,000 | 100,000 |
| Max size | 4 KB | 8 KB |
| Parameter policies | No | Yes (TTL, expiration) |
| Cost | FREE | $0.05/parameter/month |

### Hierarchy
- Use `/` for hierarchy: `/my-app/dev/db_url`
- Must start with `/`, cannot end with `/`
- Access Secrets Manager: `/aws/reference/secretsmanager/secret_ID`
- Public params: `/aws/service/ami-amazon-linux-latest/...`

### CLI Commands
```bash
# Get parameters
aws ssm get-parameters --names /my-app/dev/db_url /my-app/dev/db_password --with-decryption

# Get by path
aws ssm get-parameters-by-path --path /my-app/dev --recursive --with-decryption
```

---

## 🔐 AWS SECRETS MANAGER

### What is Secrets Manager?
- **Managed service** for storing and rotating secrets (passwords, API keys, database credentials)
- **Automatic rotation** capabilities for RDS, Redshift, and other services

### Key Features
- **Automatic secret rotation** (main advantage over SSM)
- Automated secret generation via Lambda
- Integrations: RDS, Aurora, MySQL, PostgreSQL, SQL Server
- Encryption via KMS
- Multi-region replication

### Pricing
- $0.40 per secret/month
- $0.05 per 10,000 API calls
- 30-day free trial

### Multi-Region Secrets
- Replicated from primary to secondary regions
- Auto-synchronized
- Use cases: DR, multi-region apps, replicated RDS

---

## 🎫 AWS CERTIFICATE MANAGER (ACM)

### What is ACM?
- **Managed service** for provisioning, managing, and deploying SSL/TLS certificates
- **Automatic renewal** for public certificates at no additional cost

### Features
- Provision & manage TLS/SSL certificates
- **Public certificates: FREE**
- Automatic renewal (45 days before expiry for the 198-day certs issued since Feb 2026; was 60 days)
- Integrations: ELB, CloudFront, API Gateway

### ⚠️ Important Limitations
- **Cannot use with EC2 directly** (public certs)
- **Normal public certs cannot be extracted** from ACM (since Jun 2025, paid *exportable* public certs can)

### Certificate Request Process
1. List domain names (FQDN, wildcard `*.example.com`)
2. Validation: DNS (recommended) or Email
3. Route 53 auto-validates if used
4. Auto-renewal for ACM-generated certs
5. **Imported certs: NO auto-renewal**

### Expiration Notifications
- **EventBridge**: Daily expiration events (45 days before, configurable)
- **AWS Config**: Managed rule `ACM-certificate-expiration-check`
- ⚠️ **ACM publishes DAILY events, NOT monthly**

### API Gateway Integration
- **Edge-Optimized**: Cert MUST be in **us-east-1** (CloudFront requirement)
- **Regional**: Cert in same region as API Gateway
- **Private**: VPC endpoint only

---

## 🛡️ AWS WAF (Web Application Firewall)

### What is WAF?
- **Web application firewall** that protects web applications from common web exploits
- **Layer 7 protection** against SQL injection, XSS, and other OWASP Top 10 vulnerabilities

### Deployment Targets
- Application Load Balancer (ALB)
- API Gateway
- CloudFront
- AppSync GraphQL API
- Cognito user pools
- ⚠️ **NOT on NLB** (Layer 4, WAF is Layer 7)

### Web ACL Rules
- **IP addresses**: Up to 10,000 IPs per set
- **HTTP headers/body/URI**: SQL injection, XSS protection
- **Size constraints**: Limit request size (e.g., 2MB)
- **Geo match**: Allow/block countries
- **Rate-based rules**: DDoS protection (e.g., max 1,000 requests per IP in 5 min; window 1/2/5/10 min, min limit 10)
- **Max capacity**: 5,000 WCUs per Web ACL (1,500 included in base price; older questions say 1,500)

### Web ACL Scope
- Regional (ALB, API Gateway) - per region
- Global (CloudFront) - defined in us-east-1

### Fixed IP with WAF + ALB
- Problem: ALB has no fixed IP, WAF doesn't work with NLB
- Solution: **Global Accelerator** (fixed IP) → ALB → WAF

---

## 🛡️ AWS SHIELD

### What is Shield?
- **Managed DDoS protection service** for AWS applications
- **Two tiers**: Standard (free) and Advanced (paid) with enhanced protection

### Shield Standard (FREE)
- Auto-enabled for all AWS customers
- Protects: SYN floods, UDP floods, reflection attacks
- Layer 3 & 4 protection

### Shield Advanced ($3,000/month)
- Enhanced DDoS mitigation
- Protected resources: EC2, ELB, CloudFront, Global Accelerator, Route 53
- **24/7 DDoS Response Team**
- **DDoS Cost Protection**
- **Auto-creates WAF rules** for Layer 7 attacks

---

## 🔥 AWS FIREWALL MANAGER

### Purpose
- Centralized firewall management across AWS Organization
- Manage rules across multiple accounts simultaneously

### Managed Security Rules
- WAF rules (ALB, API Gateway, CloudFront)
- Shield Advanced (ALB, CLB, NLB, Elastic IPs, CloudFront)
- Security Group policies (EC2, ALB, ENI)
- Network Firewall rules (VPC level)
- Route 53 Resolver DNS Firewall

### Features
- Regional policies
- Organization-wide application (including future accounts)
- Auto-applies rules to new resources
- **Cost: $100/month per policy**

---

## 🚨 DDoS PROTECTION BEST PRACTICES

### Edge Location Mitigation
- **CloudFront**: Edge delivery + Shield protection
- **Global Accelerator**: Edge access + Shield protection
- **Route 53**: Edge DNS + built-in DDoS protection

### Application Layer Defense
- **CloudFront**: Static content at edge
- **WAF**: Filter malicious requests (IPs, signatures)
- **WAF Rate-Based Rules**: Auto-block malicious IPs
- **WAF Managed Rules**: IP reputation, anonymous IP blocking
- **CloudFront Geo Restriction**: Block countries
- **Shield Advanced**: Auto-creates WAF rules for Layer 7

### Reducing Attack Surface
- **Obfuscate resources**: CloudFront, API Gateway, ELB hide backend
- **Security Groups & NACLs**: Filter at subnet/ENI level
- **Elastic IPs**: Protected by Shield Advanced
- **API Gateway**: Burst limits, header filtering, API keys

---

## 🔍 AMAZON GUARDDUTY

### Purpose
- Intelligent threat discovery using ML & anomaly detection
- One-click enable, 30-day free trial
- No software installation

### Input Data Sources
**Mandatory:**
- CloudTrail logs (API calls, unauthorized deployments)
- VPC Flow Logs (unusual traffic, IPs)
- DNS Logs (encoded data in DNS queries)

**Optional:**
- S3 data events
- EKS audit logs
- RDS/Aurora login events
- EBS volumes
- Lambda network activity

### Output
- Findings → EventBridge → Lambda/SNS
- ⚠️ **Effective against cryptocurrency attacks** (dedicated finding)

---

## 🔎 AMAZON INSPECTOR

### What is Inspector?
- **Automated security assessment service** that scans for vulnerabilities and deviations from best practices
- **Continuous monitoring** of EC2 instances, container images, and Lambda functions

### Scans
- **EC2 instances**: Network reachability, OS vulnerabilities (via SSM agent, or agentless with EBS snapshots)
- **ECR container images**: Vulnerability scanning on push
- **Lambda functions**: Code vulnerabilities, package dependencies

### Reporting
- AWS Security Hub (centralized view)
- EventBridge (automations)
- Risk scores for prioritization
- Continuous scanning
- Auto-reruns when CVE database updates

---

## 🔐 AWS MACIE

### Purpose
- Discover & protect sensitive data (PII) in S3
- Uses ML & pattern matching
- One-click activation

### How It Works
1. Analyzes S3 bucket data
2. Identifies PII
3. Notifies via EventBridge
4. Integrate with SNS/Lambda

### Identifies
- PII data
- Unencrypted buckets
- Publicly accessible buckets
- Cross-account shared buckets

---

## 📋 EXAM KEY POINTS

### KMS
- ✅ Key rotation: 1 year (auto, default; configurable 90–2,560 days), old keys retained for decryption
- ✅ Region-scoped, cross-region requires re-encryption
- ✅ Multi-region keys: Global DynamoDB/Aurora + client-side encryption
- ✅ Cross-account AMI sharing: Modify launch permissions + share KMS key

### SSM Parameter Store
- ✅ Standard: 10K params, 4KB, FREE
- ✅ Advanced: 100K params, 8KB, $0.05/param/month, TTL policies
- ✅ Hierarchy: `/my-app/dev/db_url`
- ✅ Region-specific

### Secrets Manager
- ✅ Auto-rotation (main advantage)
- ✅ RDS/Aurora integration
- ✅ Multi-region replication

### ACM
- ✅ Public certs: FREE, auto-renewal
- ✅ Edge-Optimized API Gateway: Cert MUST be in **us-east-1**
- ✅ Imported certs: NO auto-renewal
- ✅ Expiration: EventBridge DAILY events (NOT monthly)

### WAF
- ✅ Layer 7 only (NOT NLB)
- ✅ 1,500 WCUs in base price; max 5,000 per Web ACL
- ✅ Fixed IP: Global Accelerator + ALB + WAF

### Shield
- ✅ Standard: FREE, auto-enabled
- ✅ Advanced: $3K/month, auto-creates WAF rules

### Firewall Manager
- ✅ Centralized management across Organization
- ✅ $100/month per policy

### GuardDuty
- ✅ Mandatory: CloudTrail, VPC Flow Logs, DNS Logs
- ✅ Effective against crypto attacks

### Inspector
- ✅ EC2, ECR, Lambda only
- ✅ Continuous scanning

### Macie
- ✅ S3 only
- ✅ Finds PII data

---

## 💰 PRICING SUMMARY

| Service | Cost |
|---------|------|
| KMS Customer Managed Key | $1/month + $0.03/10K calls |
| SSM Parameter Store Standard | FREE |
| SSM Parameter Store Advanced | $0.05/param/month |
| Secrets Manager | $0.40/secret/month + $0.05/10K calls |
| ACM Public Certificates | FREE |
| WAF | Pay per use |
| Shield Standard | FREE |
| Shield Advanced | $3,000/month |
| Firewall Manager | $100/policy/month |
| GuardDuty | Pay per use (30-day trial) |
| Inspector | Pay per use |
| Macie | Pay per use |

---

## 🔗 SERVICE INTEGRATIONS

### KMS Integrates With
- EBS, S3, RDS, SSM Parameter Store, Secrets Manager, and many more

### SSM Parameter Store
- Can reference Secrets Manager: `/aws/reference/secretsmanager/secret_ID`
- Public parameters: `/aws/service/...`

### Secrets Manager
- RDS, Aurora, MySQL, PostgreSQL, SQL Server
- Auto-rotation via Lambda

### ACM
- ELB (Classic, ALB, NLB)
- CloudFront
- API Gateway

### WAF
- ALB, API Gateway, CloudFront, AppSync, Cognito
- ⚠️ NOT NLB

### Shield
- EC2, ELB, CloudFront, Global Accelerator, Route 53

---

## ⚠️ CRITICAL EXAM WARNINGS

1. **WAF does NOT work with NLB** (Layer 4 vs Layer 7)
2. **ACM Edge-Optimized API Gateway**: Cert MUST be in **us-east-1**
3. **ACM imported certs**: NO auto-renewal
4. **ACM expiration events**: DAILY, NOT monthly
5. **KMS keys are region-scoped**: Cross-region requires re-encryption
6. **KMS rotation**: Old keys retained, rotation NEVER breaks decryption
7. **S3 multi-region keys**: Still treated as independent (re-encrypts)
8. **GuardDuty**: Effective against crypto attacks
9. **Macie**: S3 buckets only, finds PII
10. **Inspector**: EC2, ECR, Lambda only

---

*Last Updated: Based on AWS SAA-C03 Exam Guide*

