# AWS IAM - Handnote 📝

## 👥 IAM BASICS

### Key Concepts
- **Global service** (not region-specific)
- **Root account**: Created automatically, use only for initial setup
- **IAM Users**: People in your organization
- **IAM Groups**: Collections of users
- **IAM Roles**: For AWS services & cross-account access
- **IAM Policies**: JSON documents defining permissions

### Users & Groups
- ✅ **Groups contain users** (not other groups)
- ✅ **Users can belong to multiple groups**
- ✅ **Permissions inherited** from groups
- ✅ **Best practice**: Use groups, not individual user policies

### Policy Inheritance
- **Group policies** → Inherited by all group members
- **User policies** → Attached directly to user
- **Combined permissions**: User gets permissions from all groups + direct policies

---

## 📜 IAM POLICIES

### Policy Structure
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "StatementID",
    "Effect": "Allow" | "Deny",
    "Principal": "arn:aws:iam::ACCOUNT:user/USERNAME",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::bucket/*",
    "Condition": {
      "DateGreaterThan": {"aws:CurrentTime": "2023-01-01T00:00:00Z"}
    }
  }]
}
```

### Policy Elements
- **Version**: Policy language version (usually `2012-10-17`)
- **Effect**: `Allow` or `Deny`
- **Principal**: Who the policy applies to
- **Action**: API calls allowed/denied
- **Resource**: What resources the actions apply to
- **Condition**: Optional conditions for when policy applies

### Policy Types
- **Managed Policies**: AWS-managed or customer-managed (reusable)
- **Inline Policies**: Attached directly to user/group/role (not reusable)

### Policy Evaluation
1. **Explicit Deny** → Deny (overrides everything)
2. **Explicit Allow** → Allow
3. **No match** → Deny (implicit deny)

---

## 🔐 IAM ROLES

### Purpose
- **Temporary credentials** for AWS services
- **Cross-account access**
- **Federation** (SAML, OIDC)

### When to Use Roles
- ✅ **EC2 instances** (not IAM users)
- ✅ **Lambda functions**
- ✅ **Cross-account access**
- ✅ **Federation** (Active Directory, etc.)

### Role Types
- **Service Roles**: For AWS services (EC2, Lambda, etc.)
- **Service-Linked Roles**: Pre-defined for specific services
- **Cross-Account Roles**: For accessing resources in other accounts

### Trust Policy
- Defines **who can assume** the role
- **Principal**: Service, account, or user that can assume role

---

## 🛡️ IAM SECURITY

### Password Policies
- **Minimum length**: 6-128 characters (default policy: 8)
- **Character requirements**: Uppercase, lowercase, numbers, symbols
- **Password expiration**: 1-1095 days
- **Password reuse**: Prevent reuse of last N passwords

### Multi-Factor Authentication (MFA)
- ✅ **Virtual MFA**: Mobile app (Google Authenticator, etc.)
- ✅ **Hardware MFA**: Physical device
- ✅ **U2F Security Key**: FIDO-compliant (now called FIDO security key / passkey)
- ✅ **Best practice**: Enable MFA for root account & privileged users (root MFA is enforced by AWS for all account types since Jun 2025)

### Access Keys
- **Access Key ID**: Public identifier
- **Secret Access Key**: Private key (shown only once)
- ⚠️ **Never commit** access keys to code
- ✅ **Rotate regularly**

---

## 🔍 IAM SECURITY TOOLS

### Credentials Report
- **CSV report** of all account users & credentials
- Shows: Password age, access keys, MFA status, last login
- ✅ **Download** for audit purposes

### Access Advisor
- Shows **service permissions granted** to user/role
- Shows **last accessed** date for each service
- ✅ **Identify unused permissions**

### IAM Access Analyzer
- **Identifies resources** shared with external entities
- **Zone of Trust**: Your account or organization
- ✅ **Find unintended access**

---

## 📋 IAM BEST PRACTICES

### General
- ✅ **Never use root account** for daily operations
- ✅ **One IAM user** per physical person
- ✅ **One IAM role** per application
- ✅ **Use groups** to assign permissions
- ✅ **Least privilege principle**
- ✅ **Enable MFA** for privileged users
- ✅ **Use strong password policy**
- ✅ **Rotate access keys** regularly
- ✅ **Remove unused credentials**

### For EC2
- ✅ **Use IAM roles** (not IAM users)
- ✅ **Roles automatically rotate** credentials
- ✅ **No access keys** needed

### For Applications
- ✅ **Use IAM roles** for Lambda, ECS, etc.
- ✅ **Never hardcode** credentials
- ✅ **Use temporary credentials**

---

## 🌐 IAM FEDERATION

### Purpose
- **Single Sign-On (SSO)** for enterprise
- **Temporary credentials** for external users

### Options
- **AWS SSO (Identity Center)**: Managed SSO service
- **SAML 2.0**: Enterprise identity providers
- **OpenID Connect (OIDC)**: Web identity providers
- **Custom Identity Broker**: Build your own

### Benefits
- ✅ **No IAM users** needed
- ✅ **Centralized identity management**
- ✅ **Temporary credentials** (more secure)

---

## 🔗 CROSS-ACCOUNT ACCESS

### Methods
1. **IAM Roles**: Assume role in another account
2. **Resource-Based Policies**: S3 bucket policies, KMS key policies
3. **AWS Organizations**: Centralized management

### IAM Role for Cross-Account
1. **Trust Policy**: Allow other account to assume role
2. **Permission Policy**: Define what role can do
3. **Assume Role**: Other account assumes role for temporary access

---

## ⚠️ CRITICAL EXAM POINTS

1. **IAM is global** - not region-specific
2. **Root account** - use only for initial setup
3. **Groups contain users** - not other groups
4. **EC2 instances** - use IAM roles, NOT IAM users
5. **Policy evaluation**: Explicit Deny > Allow > Implicit Deny
6. **MFA** - Enable for root & privileged users
7. **Access keys** - Rotate regularly, never commit to code
8. **Least privilege** - Grant minimum permissions needed
9. **Roles** - Temporary credentials, auto-rotate
10. **Federation** - SSO without creating IAM users

---

## 📋 QUICK REFERENCE

### Creating IAM User
1. IAM Console → Users → Add user
2. Set password policy
3. Add to groups (recommended)
4. Or attach policies directly

### Creating IAM Role
1. IAM Console → Roles → Create role
2. Select trusted entity (service, account, etc.)
3. Attach permission policies
4. Review & create

### Enabling MFA
1. IAM Console → Users → Select user
2. Security credentials tab
3. Assign MFA device
4. Scan QR code or enter serial number

---

*Last Updated: Based on AWS SAA-C03 Exam Guide*

