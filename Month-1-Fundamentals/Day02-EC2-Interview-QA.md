# Day 02 — EC2 (Elastic Compute Cloud)
## Interview Questions & Answers

**Topic:** AWS Compute Service | **Date:** March 2026

---

## Core Interview Questions

### Q1. What is Amazon EC2?
**Answer:**
Amazon EC2 (Elastic Compute Cloud) is a web service that provides resizable compute capacity in the cloud. It allows you to launch virtual servers (instances) on-demand, scale capacity up or down, and pay only for what you use.

**Key Points:**
- Virtual servers in the cloud
- Complete control over computing resources
- Pay per hour/second
- Multiple instance types for different workloads

---

### Q2. What are the components of EC2?

**1. AMI (Amazon Machine Image)**
- Template containing OS, application server, and applications
- Blueprint for your instance
- Types: Amazon Linux, Ubuntu, Windows, Red Hat
- Can create custom AMIs from existing instances

**2. Instance Type**
- Defines CPU, memory, storage, and network capacity
- Families: General Purpose (t3), Compute Optimized (c5), Memory Optimized (r5)
- Example: t3.micro = 2 vCPU, 1GB RAM (Free Tier eligible)

**3. Security Group**
- Virtual firewall controlling inbound/outbound traffic
- Stateful — return traffic automatically allowed
- Rules based on protocol, port, and source/destination

**4. Key Pair**
- Public-private key pair for secure SSH access
- .pem file downloaded once — cannot be recovered if lost
- Essential for Linux instance access

---

### Q3. What are EC2 Pricing Models?

| Model | Description | Use Case | Savings |
|-------|-------------|---------|---------|
| **On-Demand** | Pay per hour/second, no commitment | Dev/Test, unpredictable workloads | Baseline |
| **Reserved** | 1-3 year commitment | Steady-state production | 60-70% |
| **Spot** | Use spare AWS capacity | Batch jobs, fault-tolerant workloads | Up to 90% |
| **Savings Plans** | Flexible usage commitment | Mixed workloads | Up to 66% |

**Interview Tip:** Always ask about workload characteristics before recommending pricing model.

---

### Q4. What is the difference between Stop and Terminate in EC2?

| | Stop | Terminate |
|--|------|-----------|
| **Action** | Shuts down the instance | Permanently deletes the instance |
| **Data** | EBS volume data preserved | EBS volume deleted (by default) |
| **IP Address** | Public IP released | Public IP released |
| **Billing** | No compute charge, EBS charged | No charges after termination |
| **Recovery** | Can be restarted | Cannot be recovered |

**Best Practice:** Always Stop for temporary shutdown, Terminate only when instance is no longer needed.

---

### Q5. What is the difference between Public IP and Private IP in EC2?

**Public IP:**
- Accessible from the internet
- Assigned automatically (changes on stop/start)
- Use Elastic IP for static public IP
- Example: 13.201.16.163

**Private IP:**
- Internal communication within VPC
- Remains constant throughout instance life
- Not accessible from internet directly
- Example: 172.31.45.192

---

### Q6. What is Security Group and how does it work?

**Answer:**
Security Group acts as a virtual firewall for EC2 instances, controlling inbound and outbound traffic at the instance level.

**Key Characteristics:**
- **Stateful** — If inbound allowed, outbound automatically allowed
- Default: All inbound DENIED, All outbound ALLOWED
- Rules are ALLOW only — cannot create DENY rules
- Multiple security groups can be attached to one instance

**Common Rules Example:**
```
Inbound:
Port 22  (SSH)   → Your IP only (secure)
Port 80  (HTTP)  → 0.0.0.0/0 (public web)
Port 443 (HTTPS) → 0.0.0.0/0 (public web)
Port 3306 (MySQL) → App server SG only

Outbound:
All traffic → 0.0.0.0/0 (allow all)
```

---

### Q7. What is an AMI and how do you create a custom AMI?

**Answer:**
AMI (Amazon Machine Image) is a template that contains the software configuration (OS, application server, applications) required to launch an instance.

**Types:**
- AWS Managed AMIs (Amazon Linux, Ubuntu, Windows)
- AWS Marketplace AMIs (third-party, may have license cost)
- Community AMIs (public, use with caution)
- Custom AMIs (created from your instances)

**Creating Custom AMI:**
1. Configure and harden your EC2 instance
2. Install required software
3. Actions → Image and templates → Create image
4. Use this AMI to launch identical instances

**Use Case:** Pre-baked AMI with .NET runtime, app config — launch identical instances in Auto Scaling

---

### Q8. What are EC2 Instance Types and families?

| Family | Optimized For | Example | Use Case |
|--------|--------------|---------|---------|
| **t3/t4g** | General Purpose (burstable) | t3.micro | Dev/Test, small apps |
| **m5/m6i** | General Purpose (balanced) | m5.large | Web servers, enterprise apps |
| **c5/c6i** | Compute Optimized | c5.xlarge | CPU-intensive, batch processing |
| **r5/r6i** | Memory Optimized | r5.large | Databases, caching |
| **p3/g4** | GPU Instances | p3.2xlarge | ML, graphics rendering |
| **i3/i4i** | Storage Optimized | i3.large | High I/O, databases |

---

### Q9. What is Elastic IP and when should you use it?

**Answer:**
Elastic IP is a static public IPv4 address allocated to your AWS account that you can associate with any EC2 instance.

**When to Use:**
- Need a fixed IP that doesn't change on stop/start
- Running services that need consistent DNS/IP
- Failover scenarios — remap IP to new instance quickly

**Cost:** Free when associated with running instance. Charged when allocated but NOT associated.

**Best Practice:** Use Route 53 DNS instead of Elastic IP where possible.

---

### Q10. Real Architect Scenario — What instance type would you recommend?

**Scenario:** E-commerce website expecting 10,000 concurrent users, .NET API, SQL Server backend

**Answer:**
```
Web/API Tier:  m5.large (2 vCPU, 8GB) × 2 instances
               Behind Application Load Balancer
               Auto Scaling Group (min 2, max 10)

Database:      RDS SQL Server (not on EC2)
               db.r5.large (Multi-AZ for HA)

Why not EC2 for DB:
→ RDS manages backups, patching, failover
→ Better HA with Multi-AZ
→ Less operational overhead
```

---

## Quick Reference

| Term | Definition |
|------|-----------|
| EC2 | Virtual server in AWS cloud |
| AMI | Template for launching instances |
| Instance Type | CPU + RAM + Network capacity |
| Security Group | Virtual firewall for instances |
| Key Pair | SSH authentication (public/private key) |
| Public IP | Internet-accessible IP (dynamic) |
| Private IP | Internal VPC IP (static) |
| Elastic IP | Static public IP address |
| EBS | Block storage volume for EC2 |
| On-Demand | Pay per hour, no commitment |
| Reserved | 1-3yr commitment, 60-70% savings |
| Spot | Spare capacity, 90% savings, interruptible |
| Stop | Shutdown, data preserved |
| Terminate | Delete permanently |

---

*Day 02 Complete | Next: Day 03 — S3 (Simple Storage Service)*
