# Day 01 — Cloud Computing Basics
## Interview Questions & Answers

**Topic:** Cloud Fundamentals | **Date:** March 2026

---

## Core Interview Questions

### Q1. What is Cloud Computing?
**Answer:**
Cloud computing is the on-demand delivery of IT resources (servers, storage, databases, networking, software) over the internet with pay-as-you-go pricing. Instead of buying and maintaining physical servers, you access technology services from a cloud provider like AWS.

**Key Points to mention in interview:**
- On-demand self-service
- Broad network access
- Resource pooling
- Rapid elasticity
- Measured service (pay per use)

---

### Q2. What are the differences between IaaS, PaaS, and SaaS?

| | IaaS | PaaS | SaaS |
|--|------|------|------|
| **Full Form** | Infrastructure as a Service | Platform as a Service | Software as a Service |
| **You Manage** | Applications, Data, Runtime, OS | Applications, Data | Nothing |
| **Provider Manages** | Virtualization, Servers, Storage | Everything except App & Data | Everything |
| **AWS Example** | EC2 | Elastic Beanstalk | — |
| **Real Example** | Blank Virtual Machine | Heroku | Gmail, Zoom |
| **Best For** | Full control needed | Quick deployment | Ready-to-use software |

**Interview Tip:** Always give real-world examples when explaining these concepts.

---

### Q3. What are the types of Cloud Deployment Models?

**1. Public Cloud**
- Infrastructure owned by cloud provider (AWS/Azure/GCP)
- Shared among multiple customers
- Best for: Startups, web applications, cost-sensitive workloads
- Example: Netflix runs on AWS public cloud

**2. Private Cloud**
- Dedicated infrastructure for single organization
- Higher security and control
- Best for: Banks, government, healthcare
- Example: A bank's internal cloud infrastructure

**3. Hybrid Cloud**
- Combination of public and private cloud
- Sensitive data on private, normal workloads on public
- Best for: Large enterprises with compliance requirements
- Example: Company keeps customer PII on private cloud, runs web app on AWS

**Interview Tip:** For architect roles, always mention hybrid cloud as the enterprise-standard approach.

---

### Q4. What is AWS and why is it the market leader?

**Answer:**
AWS (Amazon Web Services) is the world's most comprehensive and broadly adopted cloud platform, offering 200+ services from data centers globally.

**Why Market Leader:**
- Launched in 2006 — first mover advantage
- 32% global market share (2024)
- Most mature and feature-rich platform
- Largest partner ecosystem
- Most certifications and trained professionals

---

### Q5. What is an AWS Region and Availability Zone?

**Region:**
- A physical geographic location (e.g., Mumbai, Singapore, US-East)
- Each region is completely independent
- Choose region based on: latency, compliance, cost
- **Mumbai Region Code:** ap-south-1

**Availability Zone (AZ):**
- One or more discrete data centers within a Region
- Each AZ has independent power, cooling, networking
- Mumbai has 3 AZs: ap-south-1a, ap-south-1b, ap-south-1c

**Architecture Rule:**
> Always deploy across minimum 2 AZs for High Availability

---

### Q6. What is the difference between High Availability and Fault Tolerance?

| | High Availability | Fault Tolerance |
|--|------------------|-----------------|
| **Definition** | System remains operational with minimal downtime | System continues operating even when components fail |
| **Downtime** | Minimal (seconds to minutes) | Zero downtime |
| **Cost** | Moderate | High |
| **Example** | Multi-AZ RDS | Active-Active multi-region |
| **Use Case** | Most web applications | Banking, Healthcare, Critical systems |

---

### Q7. What are the 6 advantages of Cloud Computing?

1. **Trade capital expense for variable expense** — No upfront server cost
2. **Benefit from economies of scale** — AWS serves millions, you get lower prices
3. **Stop guessing capacity** — Scale on demand
4. **Increase speed and agility** — Deploy in minutes, not months
5. **Stop spending on data centers** — Focus on business, not infrastructure
6. **Go global in minutes** — Deploy worldwide instantly

---

### Q8. What is the Shared Responsibility Model?

**AWS Responsibility (Security OF the Cloud):**
- Physical security of data centers
- Hardware, networking infrastructure
- Hypervisor, managed services

**Customer Responsibility (Security IN the Cloud):**
- Operating system patches
- Application security
- Data encryption
- Network configuration (Security Groups, NACLs)
- IAM user management

**Interview Tip:** This is asked in almost every cloud architect interview. Know it thoroughly.

---

### Q9. What factors should you consider when choosing an AWS Region?

1. **Latency** — Choose region closest to your users
2. **Compliance/Data Residency** — Data must stay in specific country
3. **Service Availability** — Not all services available in all regions
4. **Cost** — Pricing varies by region
5. **Disaster Recovery** — Use secondary region for DR

---

### Q10. What is the difference between Scalability and Elasticity?

**Scalability:**
- Ability to handle increased load by adding resources
- Can scale UP (bigger server) or OUT (more servers)

**Elasticity:**
- Automatic scaling based on current demand
- Scales both UP and DOWN automatically
- Saves cost by releasing resources when not needed

**Example:** Auto Scaling Group — adds EC2 instances during peak, removes during off-peak

---

## Quick Reference — Key Terms

| Term | Definition |
|------|-----------|
| Cloud Computing | On-demand IT resources over internet |
| IaaS | Virtual hardware — manage everything above OS |
| PaaS | Platform ready — just deploy your code |
| SaaS | Complete software — just use it |
| Public Cloud | Shared cloud infrastructure (AWS) |
| Private Cloud | Dedicated cloud for one organization |
| Hybrid Cloud | Mix of public and private |
| Region | Geographic location of AWS data centers |
| Availability Zone | Isolated data center(s) within a Region |
| High Availability | Minimize downtime |
| Fault Tolerance | Zero downtime even during failures |
| Scalability | Handle increased load |
| Elasticity | Auto scale up AND down |
| Shared Responsibility | AWS secures cloud, you secure what's in it |

---

*Day 01 Complete | Next: Day 02 — EC2 (Elastic Compute Cloud)*
