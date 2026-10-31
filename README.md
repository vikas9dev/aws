# ☁️ AWS for System Design Rounds

> 🌿 **Branch:** `system_design` — AWS notes for senior backend system-design interviews.  
> 📘 **Exam notes:** the full AWS SAA-C03 notes stay on the `main` branch.  
> ✅ **Facts checked:** limits and quotas verified against AWS docs on 2026-10-31.

These notes keep only what helps in a design round. 🎯 Each service is explained the same way:

| | Every service answers |
|---|---|
| 🧩 | What problem does it solve? |
| ✅ ❌ | When to use it, and when not to |
| 📏 | Which limits and numbers change a design |
| ⚠️ | How does it fail? |
| ⚖️ | What are the trade-offs against the other options? |
| 🎤 | How do I say it in an interview? |

> 💡 **Interview questions sit inside the section they belong to**, under `### Interview questions`.  
> 🔢 **Files are numbered in reading order** — start at 01 and go down.  
> 🎯 **Relevance:** 🔥🔥 comes up in almost every design round · 🔥 common · ⚡ sometimes

## 📚 Files

| # | File | Covers | 🎯 Relevance | ❓ Questions |
|---|---|---|---|---|
| 01 | 🏛️ [Reference Architectures](system_design/01_reference_architectures.md) | 8 end-to-end designs as interview walk-throughs | 🔥🔥 the full picture every round asks for | 11 |
| 02 | 🗃️ [Databases](system_design/02_databases.md) | RDS, RDS Proxy, Aurora, DynamoDB, choosing a database | 🔥🔥 the data model drives the design | 11 |
| 03 | 📨 [Messaging and Streaming](system_design/03_messaging_and_streaming.md) | SQS, SNS, EventBridge, Kinesis, MSK, Step Functions | 🔥🔥 async work, decoupling, idempotency | 13 |
| 04 | ⚡ [Caching and Edge](system_design/04_caching_and_edge.md) | ElastiCache, CloudFront, Global Accelerator, Route 53 | 🔥🔥 read scaling and latency | 11 |
| 05 | ⚙️ [Compute and Scaling](system_design/05_compute_and_scaling.md) | EC2, Auto Scaling, ALB / NLB, ECS / EKS / Fargate, Lambda | 🔥 how the service tier scales | 14 |
| 06 | 🛡️ [Reliability and DR](system_design/06_reliability_and_dr.md) | Multi-AZ vs multi-Region, RPO / RTO, DR strategies, cells, retries | 🔥 "what happens when it fails?" | 11 |
| 07 | 🌐 [Networking](system_design/07_networking.md) | VPC, subnets, NAT, endpoints, PrivateLink, Transit Gateway | ⚡ when you draw the deployment | 11 |
| 08 | 🔐 [Security](system_design/08_security.md) | IAM, Cognito, KMS, Secrets Manager, WAF, Shield | 🔥 auth, secrets and encryption | 12 |
| 09 | 🪣 [Storage](system_design/09_storage.md) | S3, EBS, instance store, EFS | 🔥 uploads, files and media | 11 |
| 10 | 🔭 [Observability](system_design/10_observability.md) | CloudWatch, X-Ray / OpenTelemetry, CloudTrail, Config | ⚡ the closing "how do you monitor it?" | 9 |
| | | | **Total** | **114** |

## 🧭 Suggested order

1. 🏛️ **Start with the map:** [01 Reference Architectures](system_design/01_reference_architectures.md). It shows how the pieces fit together.
2. 🔥 **Then the core four** — they come up in almost every design round: 🗃️ [02 Databases](system_design/02_databases.md) → 📨 [03 Messaging](system_design/03_messaging_and_streaming.md) → ⚡ [04 Caching](system_design/04_caching_and_edge.md) → ⚙️ [05 Compute](system_design/05_compute_and_scaling.md).
3. 📖 **Then the depth topics:** 🛡️ [06 Reliability](system_design/06_reliability_and_dr.md) → 🌐 [07 Networking](system_design/07_networking.md) → 🔐 [08 Security](system_design/08_security.md) → 🪣 [09 Storage](system_design/09_storage.md) → 🔭 [10 Observability](system_design/10_observability.md).
