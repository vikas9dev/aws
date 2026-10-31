# Whitepapers & Architectures - Handnote 📝

## 📚 AWS WELL-ARCHITECTED FRAMEWORK

### Core Principles
- ✅ **Stop guessing capacity**: Use auto-scaling
- ✅ **Test at production scale**: Spin up large infrastructure for testing
- ✅ **Automate**: Use CloudFormation for experimentation
- ✅ **Evolutionary architectures**: Start simple, evolve
- ✅ **Data-driven**: Make decisions based on data
- ✅ **Game days**: Simulate real-world scenarios

### Six Pillars
1. **Operational Excellence** 👷
2. **Security** 🔒
3. **Reliability** ⚡
4. **Performance Efficiency** 🚀
5. **Cost Optimization** 💰
6. **Sustainability** 🌍

### Key Point
- **Not trade-offs**: Pillars are synergistic
- **Improving one**: Often benefits others
- **Mnemonic**: O S R P C S (Optimized Solutions Rely on Performance, Cost, and Sustainability)

---

## 🛠️ AWS WELL-ARCHITECTED TOOL

### What is the Tool?
- **Purpose**: Review architectures against six pillars
- **Process**:
  1. Define workload
  2. Answer pillar questions
  3. Review answers
  4. Get recommendations
  5. View dashboard results

### Lenses
- **Well-Architected Framework Lens**: General best practices
- **FTR Lens**: Foundational Technical Review
- **Serverless Lens**: Serverless-specific
- **SaaS Lens**: SaaS-specific
- **Custom Lenses**: Create your own

### Benefits
- ✅ Identify risks (high, medium, low)
- ✅ Get recommendations
- ✅ Track improvements
- ✅ Milestone tracking

---

## 🎯 AWS TRUSTED ADVISOR

### What is Trusted Advisor?
- **Purpose**: High-level assessment of AWS account
- **No Installation**: Built-in service
- **Checks**: Various aspects of account

### Check Categories
1. **Cost Optimization** 💰
2. **Performance** ⚡
3. **Security** 🔒
4. **Fault Tolerance** 🛡️
5. **Service Limits** 📊
6. **Operational Excellence** ⚙️

### Check Sets
- **Core Checks**: Available to all (limited)
- **Full Checks**: Business/Enterprise Support required (since the 2026 plan change: Business Support+, Enterprise or Unified Operations)

### Core Security Checks (Free)
- ✅ Bucket Permissions
- ✅ Security Group ports
- ✅ EBS Public Snapshots
- ✅ RDS Public Snapshots

### Recommendations Examples
- S3 buckets allowing global access
- Security groups with unrestricted access
- Service limits monitoring

---

## 🏛️ AWS ARCHITECTURE CENTER

### What is Architecture Center?
- **Purpose**: Reference architecture examples
- **Content**: 2,000+ architecture diagrams
- **Features**:
  - ✅ Filter by use case
  - ✅ Detailed documentation
  - ✅ Architecture diagrams
  - ✅ CloudFormation templates

### Use Cases
- WordPress on AWS
- DR solutions
- Multi-tier applications
- Industry-specific architectures

---

## 📚 AWS SOLUTIONS LIBRARY

### What is Solutions Library?
- **Purpose**: Vetted solutions with implementation guides
- **Content**: 
  - ✅ Architecture diagrams
  - ✅ CloudFormation templates
  - ✅ Implementation guides
  - ✅ Source code (GitHub)

### Features
- **Deployable**: Ready-to-use CloudFormation templates
- **Best Practices**: AWS-validated solutions
- **Categories**: Industry, technology, organization type

### Examples
- Live Streaming on AWS
- Serverless Image Handler
- Data Lake solutions

---

## ⚠️ CRITICAL EXAM POINTS

1. **Well-Architected Framework**: Six pillars (O S R P C S)
2. **Pillars are Synergistic**: Not trade-offs
3. **Well-Architected Tool**: Review architectures, get recommendations
4. **Trusted Advisor**: Account assessment, checks in 6 categories
5. **Full Checks**: Require Business/Enterprise Support (now Business Support+, Enterprise or Unified Operations)
6. **Architecture Center**: 2,000+ reference architectures
7. **Solutions Library**: Deployable solutions with templates
8. **Core Principles**: Auto-scaling, testing, automation, evolution

---

## 📋 QUICK REFERENCE

### Well-Architected Framework
- **Six Pillars**: Operational Excellence, Security, Reliability, Performance, Cost, Sustainability
- **Tool**: Review workloads, identify risks, get recommendations
- **Lenses**: Framework, FTR, Serverless, SaaS, Custom

### Trusted Advisor
- **Free**: Core security checks + all Service Limits checks
- **Paid**: Full checks (Business/Enterprise Support; now Business Support+, Enterprise or Unified Operations)
- **Categories**: Cost, Performance, Security, Fault Tolerance, Limits, Operations

### Architecture Resources
- **Architecture Center**: Reference architectures, diagrams
- **Solutions Library**: Deployable solutions, CloudFormation templates

---

*Last Updated: Based on AWS SAA-C03 Exam Guide*

