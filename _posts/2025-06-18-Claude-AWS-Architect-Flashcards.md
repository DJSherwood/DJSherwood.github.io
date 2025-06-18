---
author: "Daniel Sherwood"
layout: post
title: "Claude Generated AWS Certification Flaschards"
date: 2025-06-18 14:51:00 -0500
lead: "Claude as a study buddy"
---

# AWS Solutions Architect Associate Certification Flashcards

## Domain 1: Design Resilient Architectures (26% of exam)

### High Availability & Fault Tolerance

**Q1:** What is the difference between high availability and fault tolerance in AWS?
**A1:** High availability focuses on minimizing downtime through redundancy and quick recovery (99.9% uptime), while fault tolerance ensures continuous operation even when components fail (no downtime). HA uses multiple AZs, fault tolerance uses real-time replication across regions.

**Q2:** Which AWS services provide built-in multi-AZ deployment for high availability?
**A2:** RDS Multi-AZ, ElastiCache with cluster mode, EFS, S3 (automatically), ALB/NLB, NAT Gateway, and ECS with multiple AZ placement.

**Q3:** How many Availability Zones should you deploy across for high availability?
**A3:** Minimum of 2 AZs for high availability, but 3 AZs is recommended for better fault tolerance and to handle AZ failures more gracefully.

### Auto Scaling & Load Balancing

**Q4:** What are the three types of Application Load Balancer routing?
**A4:** Path-based routing (routes based on URL path), Host-based routing (routes based on hostname), and HTTP header-based routing (routes based on HTTP headers like user-agent).

**Q5:** When would you use Network Load Balancer instead of Application Load Balancer?
**A5:** Use NLB for ultra-high performance (millions of requests per second), static IP addresses, TCP/UDP traffic, extreme low latency requirements, or when you need to preserve source IP addresses.

**Q6:** What are the different Auto Scaling policies?
**A6:** Target Tracking (maintain metric at target value), Step Scaling (scale based on metric thresholds), Simple Scaling (scale by fixed amount), Scheduled Scaling (scale at specific times), and Predictive Scaling (ML-based forecasting).

### Backup & Disaster Recovery

**Q7:** What are the four disaster recovery strategies in order of cost and complexity?
**A7:** 1) Backup & Restore (cheapest, slowest RTO), 2) Pilot Light (core components always running), 3) Warm Standby (scaled-down version running), 4) Multi-Site Active/Active (most expensive, fastest RTO).

**Q8:** What is the difference between RTO and RPO?
**A8:** RTO (Recovery Time Objective) is how long it takes to restore service after failure. RPO (Recovery Point Objective) is the maximum acceptable data loss measured in time (how much data you can afford to lose).

## Domain 2: Design High-Performing Architectures (24% of exam)

### Compute Services

**Q9:** When should you use EC2 Spot Instances?
**A9:** For fault-tolerant, flexible workloads like batch processing, data analysis, image processing, distributed workloads, CI/CD, and workloads with flexible start/end times. Not suitable for critical or persistent workloads.

**Q10:** What are the different EC2 instance purchasing options?
**A10:** On-Demand (pay per use), Reserved Instances (1-3 year commitment), Spot Instances (up to 90% discount, can be terminated), Dedicated Hosts (physical server), Dedicated Instances (isolated hardware), Savings Plans (flexible pricing model).

**Q11:** When would you choose Lambda over EC2?
**A11:** For event-driven computing, short-running tasks (<15 minutes), serverless architectures, variable/unpredictable workloads, microservices, and when you want to avoid server management overhead.

### Storage Services

**Q12:** What are the S3 storage classes and their use cases?
**A12:** Standard (frequent access), Standard-IA (infrequent access), One Zone-IA (infrequent, single AZ), Glacier Instant Retrieval (archive, instant access), Glacier Flexible Retrieval (archive, minutes-hours), Glacier Deep Archive (long-term archive, 12+ hours), Intelligent-Tiering (automatic optimization).

**Q13:** What are the different EBS volume types?
**A13:** gp3/gp2 (general purpose SSD), io2/io1 (provisioned IOPS SSD for high performance), st1 (throughput optimized HDD for big data), sc1 (cold HDD for infrequent access), and Magnetic (previous generation).

**Q14:** When would you use EFS vs EBS vs S3?
**A14:** EFS for shared file storage across multiple EC2 instances (NFS), EBS for block storage attached to single EC2 instance, S3 for object storage accessible via REST API from anywhere.

### Database Services

**Q15:** When should you choose Aurora over RDS?
**A15:** Aurora offers better performance (5x MySQL, 3x PostgreSQL), auto-scaling storage, faster backups, global databases, serverless options, and better high availability. Choose RDS for specific engines Aurora doesn't support or simpler use cases.

**Q16:** What are the differences between DynamoDB and RDS?
**A16:** DynamoDB is NoSQL, serverless, single-digit millisecond latency, auto-scaling, and good for simple queries. RDS is relational SQL, requires instance management, complex queries with joins, ACID compliance, and structured data.

**Q17:** When would you use ElastiCache Redis vs Memcached?
**A17:** Redis for complex data structures, persistence, pub/sub, advanced features, and Multi-AZ. Memcached for simple caching, multithreading, and when you need to scale horizontally easily.

### Networking

**Q18:** What is the difference between Security Groups and NACLs?
**A18:** Security Groups are stateful (return traffic automatically allowed), operate at instance level, only allow rules, and evaluate all rules. NACLs are stateless (return traffic must be explicitly allowed), operate at subnet level, allow deny rules, and process rules in order.

**Q19:** When would you use a NAT Gateway vs NAT Instance?
**A19:** NAT Gateway is managed, highly available, higher bandwidth, and preferred for production. NAT Instance is self-managed, can be used as bastion host, supports port forwarding, and is cheaper for low traffic scenarios.

**Q20:** What are the different VPC endpoint types?
**A20:** Gateway endpoints (S3 and DynamoDB only, route table based), Interface endpoints (most AWS services, ENI with private IP), and Gateway Load Balancer endpoints (for traffic inspection appliances).

## Domain 3: Design Secure Architectures (30% of exam)

### Identity & Access Management

**Q21:** What is the difference between IAM roles and IAM users?
**A21:** IAM users have permanent credentials and represent people or services. IAM roles are temporary credentials that can be assumed by users, services, or applications. Roles are more secure for applications and cross-account access.

**Q22:** What are the components of an IAM policy?
**A22:** Version, Statement (array), Effect (Allow/Deny), Action (what can be done), Resource (what it applies to), Principal (who), Condition (when/where), and Sid (statement ID).

**Q23:** How does IAM policy evaluation work?
**A23:** 1) Deny by default, 2) Explicit deny always wins, 3) Explicit allow overrides default deny, 4) AWS evaluates all applicable policies (identity-based, resource-based, SCPs, permission boundaries).

### Data Protection

**Q24:** What are the different types of encryption at rest in AWS?
**A24:** Client-side encryption (encrypt before sending to AWS), Server-side encryption with customer keys (SSE-C), Server-side encryption with AWS managed keys (SSE-S3), and Server-side encryption with KMS keys (SSE-KMS).

**Q25:** What is the difference between AWS managed keys and customer managed keys in KMS?
**A25:** AWS managed keys are created/managed by AWS services, free to use, automatic rotation. Customer managed keys are created/managed by you, $1/month, you control rotation, cross-account access, and key policies.

**Q26:** How does envelope encryption work?
**A26:** Large data is encrypted with a data key, the data key is encrypted with a master key (KMS), both encrypted data and encrypted data key are stored. To decrypt: decrypt the data key with KMS, then decrypt the data with the data key.

### Network Security

**Q27:** What is AWS WAF and when should you use it?
**A27:** Web Application Firewall that protects web applications from common exploits (SQL injection, XSS, DDoS). Use with CloudFront, ALB, API Gateway, or AppSync. Create rules based on IP addresses, HTTP headers, body, or URI strings.

**Q28:** What is AWS Shield and its tiers?
**A28:** Shield Standard (free DDoS protection for all AWS customers), Shield Advanced (paid, enhanced DDoS protection, 24/7 DRT support, cost protection, works with WAF, ELB, CloudFront, Route 53).

**Q29:** When would you use AWS PrivateLink?
**A29:** To securely access services over private network without internet gateway, NAT, VPN, or Direct Connect. Keeps traffic within AWS network, provides private connectivity to SaaS applications and AWS services.

## Domain 4: Design Cost-Optimized Architectures (20% of exam)

### Cost Optimization Strategies

**Q30:** What are the main cost optimization strategies in AWS?
**A30:** Right-sizing instances, using appropriate purchasing options (Reserved/Spot), leveraging auto-scaling, choosing cost-effective storage classes, using CloudFront for content delivery, and implementing lifecycle policies.

**Q31:** What AWS tools help with cost optimization?
**A31:** AWS Cost Explorer (analyze spending), AWS Budgets (set spending alerts), Trusted Advisor (optimization recommendations), AWS Compute Optimizer (right-sizing), Cost and Usage Reports (detailed billing data).

**Q32:** When should you use Reserved Instances vs Savings Plans?
**A32:** Reserved Instances for predictable workloads with specific instance types/regions. Savings Plans for flexibility across instance families, regions, and compute services (EC2, Fargate, Lambda) with similar commitment discounts.

### Storage Cost Optimization

**Q33:** How can you optimize S3 costs?
**A33:** Use appropriate storage classes, implement lifecycle policies to transition objects, enable S3 Intelligent-Tiering, delete incomplete multipart uploads, use S3 Analytics for access patterns, and consider S3 Transfer Acceleration costs.

**Q34:** What is S3 Intelligent-Tiering?
**A34:** Automatically moves objects between access tiers based on changing access patterns. Monitors access patterns and moves objects to the most cost-effective tier without performance impact or operational overhead.

### Data Transfer Optimization

**Q35:** How can you minimize data transfer costs?
**A35:** Use CloudFront for content delivery, place resources in same AZ when possible, use VPC endpoints for AWS services, implement data compression, and choose appropriate regions for data storage.

**Q36:** What is the most cost-effective way to transfer large amounts of data to AWS?
**A36:** For TB-scale: AWS DataSync or Direct Connect. For PB-scale: AWS Snow family (Snowball, Snowmobile). For one-time migrations: consider bandwidth costs vs Snow family shipping costs.

## Additional Key Concepts

### Well-Architected Framework

**Q37:** What are the five pillars of the AWS Well-Architected Framework?
**A37:** 1) Operational Excellence (run and monitor systems), 2) Security (protect information and systems), 3) Reliability (recover from failures), 4) Performance Efficiency (use resources efficiently), 5) Cost Optimization (avoid unnecessary costs).

### Migration Strategies

**Q38:** What are the 6 R's of cloud migration?
**A38:** 1) Rehost (lift and shift), 2) Replatform (lift, tinker, and shift), 3) Refactor/Re-architect (modify architecture), 4) Repurchase (move to SaaS), 5) Retain (keep on-premises), 6) Retire (eliminate).

### Global Infrastructure

**Q39:** What is the difference between AWS Regions, Availability Zones, and Edge Locations?
**A39:** Regions are geographic areas with multiple AZs. Availability Zones are isolated data centers within a region. Edge Locations are CDN endpoints for CloudFront, more numerous than regions, used for content caching.

**Q40:** How do you choose an AWS Region?
**A40:** Consider: 1) Compliance and data governance requirements, 2) Proximity to users (latency), 3) Available services and features, 4) Pricing variations between regions.

---

## Study Tips:
- Focus on scenario-based questions - AWS exams test practical application
- Understand when to use each service and why
- Practice with AWS whitepapers and architecture examples
- Take practice exams to identify knowledge gaps
- Hands-on experience is invaluable - use AWS Free Tier
