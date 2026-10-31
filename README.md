# AWS_SAA-C03

AWS Certified Solutions Architect Associate Certification SAA-C03

## 🏗️ New: AWS for System Design Interviews

> 🌿 **Branch:** [`system_design`](https://github.com/vikas9dev/aws/tree/system_design) — AWS notes for senior backend **system-design rounds**.  
> 🎯 The exam notes on this branch cover everything. The `system_design` branch keeps only what helps in a design interview, in more depth.  
> ✅ Limits and quotas checked against AWS docs on 2026-10-31.

Each service is explained the same way:

| | Every service answers |
|---|---|
| 🧩 | What problem does it solve? |
| ✅ ❌ | When to use it, and when not to |
| 📏 | Which limits and numbers change a design |
| ⚠️ | How does it fail? |
| ⚖️ | What are the trade-offs against the other options? |
| 🎤 | How do I say it in an interview? |

> 💡 **Interview questions sit inside the section they belong to.** Files are numbered in reading order.  
> 🎯 **Relevance:** 🔥🔥 comes up in almost every design round · 🔥 common · ⚡ sometimes

| # | File | Covers | 🎯 Relevance | ❓ Questions |
|---|---|---|---|---|
| 01 | 🏛️ [Reference Architectures](https://github.com/vikas9dev/aws/blob/system_design/system_design/01_reference_architectures.md) | 8 end-to-end designs as interview walk-throughs | 🔥🔥 the full picture every round asks for | 11 |
| 02 | 🗃️ [Databases](https://github.com/vikas9dev/aws/blob/system_design/system_design/02_databases.md) | RDS, RDS Proxy, Aurora, DynamoDB, choosing a database | 🔥🔥 the data model drives the design | 11 |
| 03 | 📨 [Messaging and Streaming](https://github.com/vikas9dev/aws/blob/system_design/system_design/03_messaging_and_streaming.md) | SQS, SNS, EventBridge, Kinesis, MSK, Step Functions | 🔥🔥 async work, decoupling, idempotency | 13 |
| 04 | ⚡ [Caching and Edge](https://github.com/vikas9dev/aws/blob/system_design/system_design/04_caching_and_edge.md) | ElastiCache, CloudFront, Global Accelerator, Route 53 | 🔥🔥 read scaling and latency | 11 |
| 05 | ⚙️ [Compute and Scaling](https://github.com/vikas9dev/aws/blob/system_design/system_design/05_compute_and_scaling.md) | EC2, Auto Scaling, ALB / NLB, ECS / EKS / Fargate, Lambda | 🔥 how the service tier scales | 14 |
| 06 | 🛡️ [Reliability and DR](https://github.com/vikas9dev/aws/blob/system_design/system_design/06_reliability_and_dr.md) | Multi-AZ vs multi-Region, RPO / RTO, DR strategies, cells, retries | 🔥 "what happens when it fails?" | 11 |
| 07 | 🌐 [Networking](https://github.com/vikas9dev/aws/blob/system_design/system_design/07_networking.md) | VPC, subnets, NAT, endpoints, PrivateLink, Transit Gateway | ⚡ when you draw the deployment | 11 |
| 08 | 🔐 [Security](https://github.com/vikas9dev/aws/blob/system_design/system_design/08_security.md) | IAM, Cognito, KMS, Secrets Manager, WAF, Shield | 🔥 auth, secrets and encryption | 12 |
| 09 | 🪣 [Storage](https://github.com/vikas9dev/aws/blob/system_design/system_design/09_storage.md) | S3, EBS, instance store, EFS | 🔥 uploads, files and media | 11 |
| 10 | 🔭 [Observability](https://github.com/vikas9dev/aws/blob/system_design/system_design/10_observability.md) | CloudWatch, X-Ray / OpenTelemetry, CloudTrail, Config | ⚡ the closing "how do you monitor it?" | 9 |
| | | | **Total** | **114** |

### 🧭 Suggested order

1. 🧭 **Start with the map:** 🏛️ [01 Reference Architectures](https://github.com/vikas9dev/aws/blob/system_design/system_design/01_reference_architectures.md). It shows how the pieces fit together.
2. 🔥 **Then the core four** — they come up in almost every design round: 🗃️ [02 Databases](https://github.com/vikas9dev/aws/blob/system_design/system_design/02_databases.md) → 📨 [03 Messaging](https://github.com/vikas9dev/aws/blob/system_design/system_design/03_messaging_and_streaming.md) → ⚡ [04 Caching](https://github.com/vikas9dev/aws/blob/system_design/system_design/04_caching_and_edge.md) → ⚙️ [05 Compute](https://github.com/vikas9dev/aws/blob/system_design/system_design/05_compute_and_scaling.md).
3. 📖 **Then the depth topics:** 🛡️ [06 Reliability](https://github.com/vikas9dev/aws/blob/system_design/system_design/06_reliability_and_dr.md) → 🌐 [07 Networking](https://github.com/vikas9dev/aws/blob/system_design/system_design/07_networking.md) → 🔐 [08 Security](https://github.com/vikas9dev/aws/blob/system_design/system_design/08_security.md) → 🪣 [09 Storage](https://github.com/vikas9dev/aws/blob/system_design/system_design/09_storage.md) → 🔭 [10 Observability](https://github.com/vikas9dev/aws/blob/system_design/system_design/10_observability.md).

```bash
# 📥 Get the system-design notes locally
git clone -b system_design https://github.com/vikas9dev/aws.git
```

---

## 📝 About These Notes

Most of the part of these notes were generated using **[NoteSynth](https://github.com/vikas9dev/NoteSynth)**, an AI-powered tool that transforms Udemy course captions into well-structured markdown notes using Google's Gemini API.

- **GitHub Repository**: [vikas9dev/NoteSynth](https://github.com/vikas9dev/NoteSynth)
- **Docker Image**: [vikas9dev/notesynth](https://hub.docker.com/r/vikas9dev/notesynth)

---

## 📚 Course Content

This repository contains comprehensive notes and resources for the **AWS Certified Solutions Architect Associate (SAA-C03)** certification exam.

### 📖 Documentation (`doc/`)

Complete course notes covering all major AWS services and concepts:

1. **Introduction & Preparation**
   - Introduction
   - PREP
   - Getting Started with AWS

2. **Identity & Access Management**
   - IAM and AWS CLI
   - IAM Advanced

3. **Compute Services**
   - EC2 Fundamentals
   - EC2 SAA Level
   - EC2 Instance Storage
   - ELB and Auto Scaling Groups
   - Containers on AWS (ECS, Fargate, ECR & EKS)

4. **Storage Services**
   - AWS S3 Introduction
   - AWS S3 Advanced
   - AWS S3 Security
   - AWS Storage Extras

5. **Database Services**
   - AWS RDS, Aurora, ElastiCache
   - Databases in AWS

6. **Networking & Content Delivery**
   - Route 53
   - CloudFront and AWS Global Accelerator
   - Networking - VPC

7. **Integration & Messaging**
   - AWS Integration and Messaging

8. **Serverless**
   - Serverless Overviews
   - Serverless Solution Architecture Discussions

9. **Data & Analytics**
   - Data & Analytics
   - Machine Learning

10. **Monitoring & Security**
    - AWS Monitoring & Audit (CloudWatch, CloudTrail & Config)
    - AWS Security & Encryption (KMS, SSM Parameter Store, Shield, WAF)

11. **Architecture & Best Practices**
    - Classic Solutions Architecture Discussions
    - Disaster Recovery & Migrations
    - More Solution Architectures
    - Whitepapers and Architectures

12. **Other Services**
    - Other Services
    - Key Matches

### 💻 Resources (`resources/`)

Practical code examples and scripts for:

- **API Gateway**: Lambda code examples
- **CLI**: EC2 metadata scripts
- **CloudFormation**: Infrastructure as Code templates
- **EBS**: Command references
- **EC2**: User data scripts
- **EFS**: File system scripts
- **Kinesis**: Data streams examples
- **KMS**: Encryption demos
- **Route 53**: DNS configuration
- **S3**: Static website examples, CORS configuration, pre-signed URLs
- **SQS**: Queue examples
- **SSM**: Parameter store scripts

### 🎯 AWS Well-Architected Framework (`w_a_framework/`)

Comprehensive coverage of the AWS Well-Architected Framework:

1. `1_overview/` - Overview
2. `2_how_to_run_a_well_architected_framework_review/` - How to Run a Well-Architected Framework Review
3. `3_aws_well_architected_tool/` - AWS Well-Architected Tool
4. `4_operational_excellence_pillar/` - Operational Excellence Pillar
5. `5_security_pillar/` - Security Pillar
6. `6_reliability_pillar/` - Reliability Pillar
7. `7_performance_efficiency_pillar/` - Performance Efficiency Pillar
8. `8_cost_optimization_pillar/` - Cost Optimization Pillar
9. `9_sustainability_pillar/` - Sustainability Pillar

### 🛠️ Scripts (`scripts/`)

Utility scripts for:
- S3 bucket management
- Playground examples for encryption/decryption

---

## 📝 Study Guide

These notes are organized to help you:

1. **Understand Core Concepts**: Start with fundamentals in the `doc/` directory
2. **Practice Hands-On**: Use resources and scripts to build practical skills
3. **Focus on Exam Domains**: Review section-based notes for exam-specific content
4. **Master Key Services**: Deep dive into topic-based notes for critical services
5. **Apply Best Practices**: Study the Well-Architected Framework content

---

## 🎓 Exam Information

The AWS Certified Solutions Architect Associate (SAA-C03) exam validates your ability to:

- Design cost-effective, fault-tolerant, and scalable distributed systems on AWS
- Identify and gather requirements to define a solution
- Provide implementation guidance based on best practices

**Exam Domains:**
- Design Resilient Architectures (30%)
- Design High-Performing Architectures (26%)
- Design Secure Applications and Architectures (24%)
- Design Cost-Optimized Architectures (20%)

---

## 📚 Additional Resources

- [AWS Certified Solutions Architect - Associate Official Page](https://aws.amazon.com/certification/certified-solutions-architect-associate/)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
