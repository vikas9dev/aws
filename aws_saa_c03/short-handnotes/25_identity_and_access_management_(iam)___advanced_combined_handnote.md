# IAM Advanced - Handnote 📝

## 🏢 AWS ORGANIZATIONS

### What is Organizations?
- **Global Service**: Manage multiple AWS accounts
- **Structure**:
  - **Management Account**: Main account (root)
  - **Member Accounts**: Other accounts in organization
  - **One Organization**: Member accounts can only be in one organization

### Benefits
- ✅ **Consolidated Billing**: Single payment method
- ✅ **Pricing Benefits**: Aggregated usage = discounts
- ✅ **Shared Discounts**: Reserved Instances, Savings Plans across accounts
- ✅ **Account Automation**: API for account creation

### Organizational Units (OUs)
- **Root OU**: Outermost OU (contains all accounts)
- **Sub-OUs**: Nested OUs for organization
- **Organization Methods**:
  1. Business units (Sales, Finance)
  2. Environment (Prod, Test, Dev)
  3. Project-based

### Service Control Policies (SCPs)
- **Purpose**: Restrict what users/roles can do in accounts
- **Scope**: Applied to OUs or accounts
- ⚠️ **Management Account**: SCPs do NOT apply (safety measure)
- **Inheritance**: Each OU in hierarchy must allow action
- **Types**:
  - **Deny List**: Allow all, deny specific services
  - **Allow List**: Allow only specific services

### Tag Policies
- **Purpose**: Enforce consistent tagging across accounts
- **Benefits**: Cost allocation, resource tracking, compliance
- **Note**: Only affects resources with tags

---

## 🔐 IAM ADVANCED POLICIES

### Policy Types
- **Identity-Based**: Attached to users, groups, roles
- **Resource-Based**: Attached to resources (S3 bucket, KMS key)

### IAM Roles vs Resource-Based Policies

#### IAM Roles
- **Principal**: Assumes role
- **Permissions**: Defined in role's permission policy
- **Evaluation**: Only the role's permissions apply. The principal gives up its own permissions while it uses the role

#### Resource-Based Policies
- **Principal**: Specified in policy
- **Permissions**: Defined in resource policy
- **Evaluation**: Resource policy grants access directly

### Key Difference
- **Roles**: Principal must have permission to assume role
- **Resource Policies**: Can grant access to external principals

---

## 🛡️ IAM PERMISSION BOUNDARIES

### What are Permission Boundaries?
- **Maximum Permissions**: Set upper limit on permissions
- **Use Case**: Delegate administration without full access
- **How It Works**:
  - User gets permissions from: Identity policy + Permission boundary
  - **Effective permissions**: Intersection of both
- **Example**: User policy allows S3, boundary allows EC2 → User gets nothing

---

## 🔑 AWS IAM IDENTITY CENTER

### What is Identity Center?
- **SSO Service**: Single Sign-On for AWS and applications
- **Features**:
  - ✅ Centralized identity management
  - ✅ SSO to AWS accounts
  - ✅ SSO to business applications (SAML 2.0)
  - ✅ Multi-factor authentication (MFA)
- **Replaces**: AWS SSO (legacy name)

---

## 🏛️ AWS DIRECTORY SERVICES

### Microsoft Active Directory
- **Managed AD**: AWS Managed Microsoft AD
- **Use Case**: Windows workloads, Microsoft applications
- **Features**: Full AD compatibility, multi-AZ

### Simple AD
- **Basic AD**: Lightweight directory
- **Use Case**: Small applications, Linux workloads
- **Limitations**: No trust relationships

### AD Connector
- **Proxy**: Connects to on-premises AD
- **Use Case**: Hybrid environments
- **Note**: No directory in AWS, just proxy

---

## 🎛️ AWS CONTROL TOWER

### What is Control Tower?
- **Multi-Account Management**: Govern multiple AWS accounts
- **Features**:
  - ✅ Automated account setup
  - ✅ Guardrails, now called controls (preventive / detective / proactive)
  - ✅ Landing zone setup
  - ✅ Centralized logging
- **Built On**: Organizations, SCPs, Config

---

## ⚠️ CRITICAL EXAM POINTS

1. **Organizations**: Manage multiple accounts, consolidated billing
2. **SCPs**: Restrict account permissions, don't apply to management account
3. **OUs**: Organize accounts hierarchically
4. **Tag Policies**: Enforce consistent tagging
5. **Permission Boundaries**: Maximum permissions limit
6. **Identity Center**: SSO for AWS and applications
7. **Directory Services**: Managed AD, Simple AD, AD Connector
8. **Control Tower**: Multi-account governance
9. **Resource Policies**: Can grant access to external principals
10. **Roles**: Principal must assume role to get permissions

---

## 📋 QUICK REFERENCE

### Organizations Setup
1. Create organization (management account)
2. Create/invite member accounts
3. Organize into OUs
4. Apply SCPs to OUs/accounts
5. Enable tag policies

### SCP Examples
- **Deny S3**: `Deny s3:*`
- **Allow Only EC2**: `Allow ec2:*` (implicit deny others)

### Permission Boundary Example
- User policy: `Allow s3:*`
- Boundary: `Allow ec2:*`
- Result: No permissions (no intersection)

---

*Last Updated: Based on AWS SAA-C03 Exam Guide*

