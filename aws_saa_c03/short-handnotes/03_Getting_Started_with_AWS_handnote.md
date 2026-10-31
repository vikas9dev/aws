# Getting Started with AWS - Handnote 📝

## 📜 AWS HISTORY

### Timeline
- **2002**: AWS started as internal Amazon.com project
- **2004**: First public offering - **Simple Queue Service (SQS)**
- **2006**: Expanded with SQS, **S3**, and **EC2**
- **Today**: Market leader with $90B revenue (2023)

### Market Position
- ✅ **Market Leader**: 13 consecutive years
- ✅ **Market Share**: ~31% (Q1 2024)
- ✅ **Active Users**: Over 1 million
- ✅ **Gartner Magic Quadrant**: Consistently recognized as leader

### Companies Using AWS
- Netflix, Dropbox, Airbnb, NASA
- McDonald's, 21st Century Fox, Activision

---

## 🌍 AWS GLOBAL INFRASTRUCTURE

### Key Components
1. **Regions** 📍
2. **Availability Zones (AZs)** 🏘️
3. **Data Centers** 🏢
4. **Edge Locations** 🌐
5. **Points of Presence** 📡

### Regions
- **Definition**: Cluster of data centers in geographic area
- **Naming**: e.g., `us-east-1`, `eu-west-3`
- **Independence**: Most services are region-scoped
- **Selection Factors**:
  1. **Compliance** 🏛️: Data residency requirements
  2. **Latency** ⏱️: Deploy close to users
  3. **Service Availability** ✅: Not all services in all regions
  4. **Pricing** 💰: Varies by region

### Availability Zones (AZs)
- **Definition**: One or more discrete data centers
- **Isolation**: AZs isolated to prevent cascading failures
- **Connectivity**: High-bandwidth, ultra-low-latency networking
- **Minimum**: 3 AZs per region (AWS standard for every region)
- **Typical**: 3 AZs per region
- **Maximum**: Up to 6 AZs
- **Example**: `ap-southeast-2a`, `ap-southeast-2b`, `ap-southeast-2c`

### Points of Presence (Edge Locations)
- **Count**: 750+ points of presence + 15 regional edge caches (older material says 400+)
- **Coverage**: 100+ cities, 50+ countries
- **Purpose**: Deliver content with lowest latency
- **Used By**: CloudFront CDN

---

## 🖥️ AWS CONSOLE

### Region Selector
- **Location**: Top right corner
- **Default**: Northern Virginia (US East 1)
- **Recommendation**: Choose region close to you
- **Note**: Don't need to be physically in region

### Service Discovery
1. **Services Menu**: Browse alphabetically or by category
2. **Search Bar**: Type service name (e.g., "Route 53")

### Global vs Regional Services

#### Global Services 🌍
- **IAM** (Identity and Access Management)
- **Route 53** (DNS)
- **CloudFront** (CDN)
- **WAF** (Web Application Firewall)
- **Note**: No region selection needed

#### Regional Services 📍
- **EC2** (Elastic Compute Cloud)
- **Elastic Beanstalk**
- **Lambda**
- **Rekognition**
- **Note**: Resources vary by selected region

---

## 🏗️ WHAT CAN YOU BUILD ON AWS?

### Use Cases
1. **Enterprise IT Migration**
2. **Backup and Storage**
3. **Big Data Analytics**
4. **Website Hosting**
5. **Mobile/Social App Backend**
6. **Gaming Server Infrastructure**

### Industries
- Entertainment (Netflix)
- Retail (McDonald's)
- Media (21st Century Fox)
- Gaming (Activision)

---

## ⚠️ CRITICAL EXAM POINTS

1. **Regions**: Geographic areas with multiple AZs
2. **AZs**: Isolated data centers within a region
3. **Minimum AZs**: 3 per region
4. **Typical AZs**: 3 per region
5. **Region Selection**: Consider compliance, latency, service availability, pricing
6. **Global Services**: IAM, Route 53, CloudFront, WAF
7. **Regional Services**: EC2, Lambda, most others
8. **Edge Locations**: 750+ PoPs for CloudFront CDN
9. **Stay in Same Region**: Important during course to avoid confusion

---

## 📋 QUICK REFERENCE

### Region Selection Checklist
- ✅ Compliance requirements
- ✅ User location (latency)
- ✅ Service availability
- ✅ Pricing comparison

### Global Infrastructure Resources
- [AWS Global Infrastructure Map](https://aws.amazon.com/about-aws/global-infrastructure/)
- [AWS Regional Services List](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)

### Console Navigation
- **Services Menu**: Top left → Browse services
- **Search Bar**: Top center → Type service name
- **Region Selector**: Top right → Choose region

---

*Last Updated: Based on AWS SAA-C03 Exam Guide*

