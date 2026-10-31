# AWS VPC & Networking - Handnote 📝

## 🌐 CIDR (Classless Inter-Domain Routing)

### CIDR Notation
| CIDR | Subnet Mask | IPs | Description |
|------|-------------|-----|-------------|
| `/32` | 255.255.255.255 | 1 | Single IP |
| `/24` | 255.255.255.0 | 256 | Last octet can change |
| `/16` | 255.255.0.0 | 65,536 | Last two octets can change |
| `/8` | 255.0.0.0 | 16M+ | Last three octets can change |
| `/0` | 0.0.0.0 | All | All IPv4 addresses |

### Private IP Ranges
- `10.0.0.0/8` (10.0.0.0 - 10.255.255.255) - Large networks
- `172.16.0.0/12` (172.16.0.0 - 172.31.255.255) - Default VPCs
- `192.168.0.0/16` (192.168.0.0 - 192.168.255.255) - Home networks

---

## 🏗️ VPC COMPONENTS

### Default VPC
- ✅ Auto-created with every AWS account
- ✅ Internet connectivity enabled
- ✅ Public IPv4 address assigned
- ✅ Public & private DNS names
- ✅ One subnet per AZ

### Custom VPC
- **CIDR**: IP address range (e.g., `10.0.0.0/16`)
- **Subnets**: Subdivisions tied to specific AZ
- **Route Tables**: Control traffic routing
- **Internet Gateway (IGW)**: Provides internet access
- **NAT Gateway**: Internet access for private subnets

---

## 🔀 PUBLIC vs PRIVATE SUBNETS

### Making a Subnet Public
1. Attach **Internet Gateway (IGW)** to VPC
2. Add route in route table: `0.0.0.0/0 → igw-xxxxx`

### Private Subnet
- No route to IGW
- Use NAT Gateway for outbound internet access

---

## 🚪 INTERNET GATEWAY (IGW)

### Features
- **One IGW per VPC** (can attach/detach)
- Provides **IPv4 & IPv6** internet access
- **Horizontally scaled, redundant, highly available**
- **No bandwidth constraints**
- Must be attached to VPC to work

### Requirements for Internet Access
1. ✅ Route table entry to IGW
2. ✅ Public IP or Elastic IP on instance
3. ✅ Security Group allows traffic
4. ✅ NACL allows traffic (stateless - check both inbound & outbound)

---

## 🛡️ BASTION HOSTS

### Purpose
- Public EC2 instance to SSH into private EC2 instances
- Acts as secure jump box

### Security Best Practice
- **Security Group**: Allow port 22 (SSH) from company's **public CIDR** only
- ⚠️ NOT from private CIDR (developers connect from internet)

---

## 🔄 NAT INSTANCES vs NAT GATEWAYS

### NAT Instances (Legacy - Deprecated)
- ⚠️ EC2 instance in public subnet
- ⚠️ Disable source/destination check
- ⚠️ Not highly available (single instance)
- ⚠️ Manual scaling & management

### NAT Gateway (Recommended)
- ✅ AWS-managed service
- ✅ Highly available (deploy in each AZ)
- ✅ Auto-scaling
- ✅ **$0.045/hour + $0.045/GB** processed
- ✅ Handles IPv4 traffic only
- ✅ **5 Gbps bandwidth** (scales automatically)

### NAT Gateway Setup
1. Create NAT Gateway in **public subnet** (zonal mode, the default; a regional NAT Gateway, added Nov 2025, needs no public subnet)
2. Allocate Elastic IP
3. Update **private subnet route table**: `0.0.0.0/0 → nat-gateway-id`

### Egress-Only Internet Gateway (EIGW)
- Very similar to NAT Gateway but only for IPV6 (NAT Gateway for IPV4)

---

## 🔒 SECURITY GROUPS vs NACLs

| Feature | Security Groups | NACLs |
|---------|----------------|-------|
| **Level** | Instance level | Subnet level |
| **State** | **Stateful** (return traffic auto-allowed) | **Stateless** (check both directions) |
| **Rules** | Allow rules only | Allow & Deny rules |
| **Evaluation** | All rules evaluated | Rules evaluated in order (lowest # first) |
| **Default** | Deny all inbound, Allow all outbound | Allow all (default) |
| **Ephemeral Ports** | Not needed (stateful) | **Must allow** (1024-65535) |

### ⚠️ Key Exam Points
- **Security Groups are stateful**: If outbound allowed, inbound return traffic auto-allowed
- **NACLs are stateless**: Must explicitly allow both inbound & outbound
- **NACL ephemeral ports**: Must allow return traffic on ephemeral ports (1024-65535)

---

## 🤝 VPC PEERING

### Features
- Connect **two VPCs** together
- **Non-overlapping CIDR blocks** required
- **Non-transitive**: A↔B and B↔C does NOT mean A↔C
- Works: Same account, different accounts, different regions

### Setup
1. Create peering connection
2. **Update route tables** in both VPCs
3. Update Security Groups (can reference peered VPC SGs)

### ⚠️ Key Point
- **NOT transitive** - need separate peering for each pair

---

## 🎯 VPC ENDPOINTS

### Gateway Endpoints (FREE)
- **S3 & DynamoDB only**
- Add route to route table
- No internet gateway, NAT, or VPN needed
- **No additional cost**

### Interface Endpoints ($$$)
- **All other AWS services**
- Uses **PrivateLink** technology
- **$0.01/hour per endpoint + $0.01/GB** data processed
- ENI in your subnet
- Requires Security Group

### Use Cases
- Private access to AWS services
- Avoid NAT Gateway costs
- Keep traffic within AWS network

---

## 📊 VPC FLOW LOGS

### Features
- Capture metadata about network traffic
- Can be created at: **VPC, Subnet, or ENI level**
- Captures: Accepted & rejected traffic
- **No real-time** - logs are delivered with delay

### Destinations
- **S3**: Analyze with Athena
- **CloudWatch Logs**: Analyze with CloudWatch Logs Insights

### Use Cases
- Troubleshoot connectivity issues
- Security analysis
- Network monitoring

---

## 🌉 CONNECTING TO DATA CENTER

### Site-to-Site VPN
- **Virtual Private Gateway (VGW)** on AWS side
- **Customer Gateway (CGW)** on on-prem side
- **Public internet** connection (encrypted)
- **Max 1.25 Gbps per tunnel** (standard tunnel; since late 2025 a VPN on Transit Gateway / Cloud WAN can use 5 Gbps large bandwidth tunnels — not on a VGW)
- **VPN CloudHub**: Hub-and-spoke model with multiple VPNs to same VGW

### Direct Connect
- **Private connection** (bypasses internet) from your remote network into your VPC
- **Dedicated**: 1 Gbps, 10 Gbps, 100 Gbps, 400 Gbps (AWS provided; 400 Gbps is newer)
- **Hosted**: 50 Mbps - 25 Gbps (partner provided; older material says 10 Gbps)
- **More secure & stable** than VPN
- **Direct Connect Gateway**: Connect to multiple VPCs in different regions
- ⚠️ Warning: Setting up either a dedicated or hosted connection often takes longer than one month.

### Direct Connect Virtual Interfaces (VIFs)
- **Private VIF**: Access private AWS resources (EC2 in VPC)
- **Public VIF**: Access public AWS resources (S3, DynamoDB)

---

## 🚏 TRANSIT GATEWAY

### Purpose
- **Transitive peering** between VPCs, VPNs, Direct Connect
- Central hub for network topology
- Simplifies complex network architectures

### Use Cases
- Connect multiple VPCs
- Scale VPN beyond 1.25 Gbps (aggregate multiple tunnels)
- Hub-and-spoke architecture

---

## 🔗 PRIVATELINK (VPC Endpoint Services)

### Features
- Private connectivity to services in **other AWS accounts**
- **No VPC peering, internet, NAT, or route tables** needed
- Uses **Network Load Balancer (NLB)** + **ENIs**
- Expose services to many customer VPCs

### Use Cases
- SaaS providers exposing services
- Cross-account private connectivity

---

## 🔍 VPC TRAFFIC MIRRORING

### Purpose
- Copy network traffic from ENIs for analysis
- Send to destinations for security/network analysis
- VPC Traffic Mirroring is a security feature that allows you to capture and inspect network traffic within your VPC in a non-intrusive way. The goal is to route traffic to security appliances that you manage for analysis.

---

## 🌍 IPv6 IN VPC

### Features
- Enable IPv6 for VPC
- **Egress-Only Internet Gateway**: Like NAT Gateway but for IPv6
- Provides outbound-only IPv6 internet access

---

## 🔥 AWS NETWORK FIREWALL

### Purpose
- Layer 3-7 protection for entire VPC
- Inspect traffic: VPC-to-VPC, outbound, inbound, Direct Connect, VPN

### Features
- Filter by IP, port, protocol, domain, regex
- Thousands of rules at VPC level
- Active flow inspection (intrusion prevention)
- Send matches to S3, CloudWatch Logs, Kinesis Firehose
- Managed by AWS Firewall Manager

---

## 💰 NETWORKING COSTS (Per GB)

### Intra-Region (Same Region)
- **Same AZ, private IPs**: **FREE** ✅
- **Different AZs, private IPs**: **$0.01/GB** (charged in each direction, so $0.02/GB in total)
- **Different AZs, public IPs**: **$0.02/GB**

### Inter-Region
- **Cross-region**: **$0.02/GB**

### Key Cost Optimization Tips
- ✅ Use **private IPs** instead of public IPs
- ✅ Place instances in **same AZ** for cost savings (trade-off: HA)
- ✅ Use **VPC Endpoints** instead of NAT Gateway for S3/DynamoDB
- ✅ Keep traffic **within AWS** to minimize egress costs

### NAT Gateway vs VPC Endpoint (S3)
| Option | Cost |
|--------|------|
| **NAT Gateway** | $0.045/hour + $0.045/GB + data transfer |
| **VPC Endpoint** | FREE — no per-GB charge for EC2 ↔ S3 in the same region |

💡 **Tip**: VPC Endpoint much cheaper for S3/DynamoDB access

---

## ⚠️ CRITICAL EXAM POINTS

1. **Security Groups are stateful** - return traffic auto-allowed
2. **NACLs are stateless** - must allow both inbound & outbound
3. **NACL ephemeral ports** - must allow 1024-65535 for return traffic
4. **VPC Peering is NOT transitive** - need separate peering for each pair
5. **Internet access requires**: Route to IGW + Public IP + SG/NACL allow
6. **NAT Gateway**: $0.045/hour + $0.045/GB, highly available
7. **VPC Endpoints**: Gateway (S3/DynamoDB, FREE) vs Interface (others, $)
8. **Direct Connect**: Dedicated (1Gbps+) vs Hosted (50Mbps-25Gbps; older material says 10Gbps)
9. **Site-to-Site VPN**: Max 1.25 Gbps per tunnel (use Transit Gateway to scale)
10. **Route tables must be updated** for VPC peering, VPN, Direct Connect

---

## 📋 QUICK REFERENCE

### Making Subnet Public
1. Attach IGW to VPC
2. Route: `0.0.0.0/0 → igw-xxxxx`

### Private Subnet Internet Access
1. NAT Gateway in public subnet
2. Route: `0.0.0.0/0 → nat-gateway-id`

### VPC Peering Setup
1. Create peering connection
2. **Update route tables** in both VPCs
3. Update Security Groups

### VPC Endpoint for S3
1. Create Gateway Endpoint
2. Add route to route table
3. **FREE** - no NAT Gateway needed

---

*Last Updated: Based on AWS SAA-C03 Exam Guide*

