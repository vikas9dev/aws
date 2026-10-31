# AWS CloudFront & Global Accelerator - Handnote 📝

## 🌍 CLOUDFRONT (CDN)

### What is CloudFront?
- **Content Delivery Network (CDN)**
- **Global edge network** (750+ PoPs + 15 regional edge caches)
- **Caches content** at edge locations
- **Reduces latency** & **bandwidth costs**

### How It Works
1. User requests content
2. **DNS routes** to nearest edge location
3. **Edge location** serves cached content (if available)
4. If **cache miss**, fetches from origin
5. **Caches** content for future requests

### Origins
- **S3 Buckets**: Static website hosting, media files
- **EC2 Instances**: Dynamic content
- **ELB**: Load balanced applications
- **Custom Origins**: Any HTTP server
- **S3 Transfer Acceleration**: Accelerate uploads (not an origin type — a separate S3 feature that uses CloudFront edge locations)

### Distribution Types
- **Web Distribution**: HTTP/HTTPS content
- **RTMP Distribution**: Media streaming (deprecated)

---

## ⚡ CLOUDFRONT FEATURES

### Caching
- **TTL (Time To Live)**: How long content cached
- **Cache Behaviors**: Different rules for different paths
- **Cache Invalidation**: Force cache refresh ($$$)
- **Cache Policies**: Predefined or custom

### Security
- ✅ **HTTPS** support (SSL/TLS)
- ✅ **Signed URLs**: Temporary access to private content
- ✅ **Signed Cookies**: Access to multiple files
- ✅ **Origin Access Identity (OAI)**: Private S3 access
- ✅ **Origin Access Control (OAC)**: Newer, recommended
- ✅ **Field-Level Encryption**: Encrypt sensitive fields

### Performance
- ✅ **Compression**: Gzip and Brotli compression (when you turn on compression in the cache behavior)
- ✅ **HTTP/2**: Modern protocol support
- ✅ **Keep-Alive**: Persistent connections
- ✅ **Edge Locations**: 750+ PoPs worldwide

---

## 🎯 CLOUDFRONT BEHAVIORS

### Default Behavior
- **Path Pattern**: `*` (all paths)
- **Origin**: Primary origin
- **Cache Policy**: Caching behavior

### Cache Behaviors
- **Path Patterns**: `/images/*`, `/api/*`, etc.
- **Different origins** for different paths
- **Different cache policies** per behavior
- **Priority**: Lower number = higher priority

### Example
- `/api/*` → API Gateway (no cache)
- `/images/*` → S3 bucket (cache 1 year)
- `/*` → S3 bucket (default, cache 1 day)

---

## 🔒 CLOUDFRONT SECURITY

### Signed URLs
- **Temporary access** to private content
- **Expiration time** (URL expires)
- **IP restrictions** (optional)
- ✅ **Use case**: Premium content, time-limited access

### Signed Cookies
- **Access to multiple files** (not single URL)
- **Same benefits** as Signed URLs
- ✅ **Use case**: Entire website access

### Origin Access Identity (OAI) - Legacy
- **Private S3 bucket** access
- **CloudFront-only** access
- ⚠️ **Deprecated** - use OAC instead

### Origin Access Control (OAC) - Recommended
- **Private S3 bucket** access
- **CloudFront-only** access
- ✅ **Supports** all HTTP methods (GET, PUT, POST, etc.)
- ✅ **Use for** S3 buckets, Lambda function URLs, MediaStore, MediaPackage v2 and VPC origins (not custom HTTP origins or API Gateway)

---

## 📊 CLOUDFRONT PRICING

### Data Transfer
- **Out to Internet**: $0.085/GB (first 10 TB)
- **Out to Origin**: $0.02/GB (same region), $0.02/GB (different region)

### Requests
- **HTTP/HTTPS**: $0.0075 per 10,000 requests
- **Invalidation**: $0.005 per path (first 1,000 free/month)

### Key Points
- ✅ **Cheaper than S3** for data transfer
- ✅ **Free tier**: 1 TB data transfer, 10M requests/month (always free since Dec 2021, not only the first year)

---

## 🚀 AWS GLOBAL ACCELERATOR

### What is Global Accelerator?
- **Improves availability & performance** for global applications
- **Fixed IP addresses** (2 static IPs)
- **Automatic failover** across regions
- **Works with** any AWS region

### How It Works
1. **Anycast IPs** (2 static IPs)
2. **Routes to nearest** edge location
3. **Edge location** routes to application endpoint
4. **Automatic health checks** & failover

### Endpoints
- **Application Load Balancer (ALB)**
- **Network Load Balancer (NLB)**
- **EC2 Instances** (Elastic IP)
- **Elastic IP addresses**

### Features
- ✅ **Fixed IP addresses** (2 static anycast IPs)
- ✅ **Automatic failover** (health checks)
- ✅ **Traffic dials & endpoint weights** (no path-based routing — Global Accelerator works at layer 4)
- ✅ **Client affinity** (sticky sessions)
- ✅ **TCP termination** at edge

---

## 🔄 GLOBAL ACCELERATOR vs CLOUDFRONT

| Feature | CloudFront | Global Accelerator |
|---------|-----------|-------------------|
| **Use Case** | Static & dynamic content | TCP/UDP applications |
| **Protocols** | HTTP/HTTPS | TCP, UDP |
| **Caching** | ✅ Yes | ❌ No |
| **Fixed IPs** | ❌ No | ✅ Yes (2 static IPs) |
| **Content** | Web content, media | Any TCP/UDP traffic |
| **Latency** | Edge caching | Edge routing |

### When to Use
- **CloudFront**: Web content, media, static websites, APIs
- **Global Accelerator**: TCP/UDP apps, gaming, IoT, real-time apps

---

## 🛡️ GLOBAL ACCELERATOR FEATURES

### Health Checks
- **Automatic health checks** of endpoints
- **Failover** to healthy endpoints

### Client Affinity
- **Source IP affinity**: Same client → same endpoint
- ✅ **Use case**: Stateful applications

### Path-Based Routing
- ⚠️ **Not supported.** Global Accelerator routes TCP/UDP (layer 4) and cannot see URL paths. Use an ALB or CloudFront for path-based routing.

---

## 💰 GLOBAL ACCELERATOR PRICING

### Fixed Charges
- **$0.025/hour** per accelerator (no separate per-endpoint-group fee)

### Data Transfer
- **Data Transfer-Premium** per GB, on top of normal data transfer out. The rate depends on the source Region and the destination edge (e.g. US → US $0.015/GB, US → Asia Pacific $0.035/GB). It is charged only in the dominant direction.

---

## ⚠️ CRITICAL EXAM POINTS

1. **CloudFront**: CDN for HTTP/HTTPS content, caching at edge
2. **Global Accelerator**: TCP/UDP routing, fixed IPs, no caching
3. **CloudFront OAC**: Use for private S3 (not OAI - deprecated)
4. **Signed URLs**: Single file access, time-limited
5. **Signed Cookies**: Multiple files access
6. **Cache Invalidation**: Force refresh (costs money)
7. **Edge Locations**: 750+ PoPs worldwide
8. **Global Accelerator**: 2 static anycast IPs, automatic failover
9. **CloudFront cheaper** than S3 for data transfer
10. **Path-based routing**: Different origins for different paths

---

## 📋 QUICK REFERENCE

### CloudFront Use Cases
- Static website hosting (S3)
- Media streaming
- API acceleration
- Global content delivery

### Global Accelerator Use Cases
- TCP/UDP applications
- Gaming servers
- IoT applications
- Real-time applications
- Need fixed IP addresses

### Security Best Practice
```
Internet → CloudFront (Signed URLs/Cookies)
         ↓
    Private S3 (OAC)
```

---

*Last Updated: Based on AWS SAA-C03 Exam Guide*

