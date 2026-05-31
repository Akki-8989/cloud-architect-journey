# Day 23 — AWS Well-Architected Framework

## Real World Problem Samjho

Ek company ne AWS pe app deploy kiya — sab chal raha tha. 6 mahine baad:
```
Problems aaye:
→ Site slow ho gayi (Performance)
→ Security breach hua (Security)
→ Bill bahut zyada aaya (Cost)
→ Outage aaya — recovery nahi hua (Reliability)
→ Koi monitoring nahi thi (Operational Excellence)
```

**Solution = AWS Well-Architected Framework**
→ Architecture review karo pehle se
→ Problems aane se pehle fix karo!

---

## Well-Architected Framework Kya Hai

AWS ka official guide hai — **best practices** for cloud architecture.

```
AWS ne hazaron customers ke architectures review kiye
→ Common mistakes dekhe
→ Best practices collect kiye
→ Framework banaya — FREE!

Tum apna architecture iske against review kar sakte ho
→ Gaps milenge → Fix karo → Better architecture!
```

---

## 6 Pillars — CORSPS

```
1. Operational Excellence  → Sahi se operate karo
2. Security                → Secure rakho
3. Reliability             → Hamesha available rakho
4. Performance Efficiency  → Fast aur efficient rakho
5. Cost Optimization       → Smart spend karo
6. Sustainability          → Environment ka dhyan rakho (New!)
```

**Yaad karne ka trick: "CORSPS"**

---

## Pillar 1 — Operational Excellence

```
Goal: Workload sahi se run karo aur continuously improve karo

Key practices:
→ IaC use karo (CloudFormation) — manual clicks nahi
→ Small, frequent, reversible changes karo
→ Runbooks banao — step-by-step procedures
→ Playbooks banao — problem investigation guide
→ Post-incident analysis karo — blameless!
→ Monitoring aur observability implement karo
→ Continuously improve karo

Real example:
→ Deployment runbook: "Step 1: Traffic shift, Step 2: Deploy, Step 3: Health check"
→ Incident playbook: "Site slow → CloudWatch check → RDS check → EC2 check"
```

---

## Pillar 2 — Security

```
Goal: Data aur systems ko protect karo

Key practices:
→ Least Privilege access — sirf zaroorat ka access do
→ MFA everywhere — root user pe zaroor
→ Encryption at rest (KMS) + in transit (HTTPS/TLS)
→ VPC — Public + Private subnets
→ CloudTrail — audit logs
→ GuardDuty — threat detection
→ Regular penetration testing
→ Incident response plan banao

Key concept — Defense in Depth:
→ Multiple layers of security
→ Ek layer toot gayi → Doosra layer protect kare

Least Privilege rule:
→ Developer ko sirf us service ka access do jis pe kaam kare
→ Admin access sabko mat do!
```

---

## Pillar 3 — Reliability

```
Goal: Workload hamesha available ho, failure se recover kare

Key practices:
→ Multi-AZ deployment — minimum 2 AZs hamesha
→ Auto Scaling — demand ke saath scale karo
→ Backups — RDS automated backups, S3 versioning
→ Chaos Engineering — deliberately failures create karo
→ Game Days — team ko practice karvo
→ DR strategy define karo

RTO vs RPO — Interview mein zaroor poochha jaata hai:
→ RPO = Recovery Point Objective
         "Kitna purana data lose kar sakte hain?"
         RPO = 1 hour → Har ghante backup lo
→ RTO = Recovery Time Objective
         "Kitne time mein wapas online aana chahiye?"
         RTO = 4 hours → 4 ghante mein restore hona chahiye

DR Strategies (cost low to high):
1. Backup & Restore   → RTO: Hours    | Cheapest
2. Pilot Light        → RTO: Minutes  | Medium
3. Warm Standby       → RTO: Minutes  | Medium-High
4. Multi-Site Active  → RTO: Seconds  | Most Expensive
```

---

## Pillar 4 — Performance Efficiency

```
Goal: Resources efficiently use karo, performance maintain karo

Key practices:
→ Right service choose karo — DynamoDB vs RDS vs ElastiCache
→ Right sizing — chhoti instance jo kaam kare
→ Caching — ElastiCache se DB load kam karo
→ CloudFront — users ke paas content deliver karo
→ Auto Scaling — demand ke saath resources badhao/ghataao
→ Load testing — production se pehle test karo
→ Graviton instances — 40% better performance per watt

Purpose-built services:
→ Key-value data → DynamoDB
→ Complex queries → RDS
→ Real-time cache → ElastiCache
→ Search → OpenSearch
→ Media → S3 + CloudFront
```

---

## Pillar 5 — Cost Optimization

```
Goal: Maximum business value — minimum cost mein

Key practices:
→ Right sizing — EC2 downsize karo
→ Reserved Instances — 1-3 year commitment → 40-70% discount
→ Spot Instances — batch jobs → 90% discount
→ Auto Scaling — off-peak mein instances kam karo
→ S3 lifecycle policies — old data Glacier mein move karo
→ Delete unused resources — EBS, Elastic IPs, NAT Gateway
→ Cost Allocation Tags — project wise track karo
→ AWS Budgets — alerts set karo

Cost tools:
→ Billing Dashboard  = Total bill
→ Cost Explorer      = Graphical analysis
→ AWS Budgets        = Alert system
→ AWS Cost Optimizer = Right-sizing recommendations

Real saving examples:
→ t3.large → t3.small = 70% saving
→ On-Demand → Reserved = 40-70% saving
→ EC2 → Lambda = 80% saving (event-driven workloads)
```

---

## Pillar 6 — Sustainability

```
Goal: Environmental impact minimize karo

Key practices:
→ Right sizing — less hardware = less electricity
→ Graviton instances — 60% less energy than x86
→ Managed services — shared infrastructure = efficient
→ Auto Scaling — idle resources nahi chalao
→ S3 lifecycle — unnecessary data delete karo
→ Green regions choose karo (Oregon, Stockholm)
→ CloudFront — data travel kam karo

AWS sustainability goals:
→ 2025: 100% renewable energy
→ 2040: Net zero carbon

Simple rule:
→ Less resources = Less electricity = Less carbon
→ Right sizing + Auto Scaling = Sustainable architecture!
```

---

## Well-Architected Tool — Hands-On

```
AWS Console → Well-Architected Tool → Define workload

Step 1: Workload create kiya
  Name: akash-learning-app
  Environment: Pre-production
  Region: ap-south-1

Step 2: AWS Well-Architected Framework lens select kiya

Step 3: Saare 57 questions answer kiye:
  Operational Excellence : 11 questions ✅
  Security               : 11 questions ✅
  Reliability            : 13 questions ✅
  Performance Efficiency :  5 questions ✅
  Cost Optimization      : 11 questions ✅
  Sustainability         :  6 questions ✅
  TOTAL                  : 57/57 ✅

Step 4: Report generate kiya → PDF downloaded ✅

Result: High risks aur Medium risks identify hue
        → Ye real architecture review hoti hai!
```

---

## Interview Questions & Answers

**Q1. What is the AWS Well-Architected Framework?**

The AWS Well-Architected Framework is a set of best practices and guidelines that AWS has developed based on reviewing thousands of customer architectures. It provides a consistent approach for evaluating architectures against AWS best practices and identifying areas for improvement. The framework is organized around six pillars: Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization, and Sustainability. AWS provides the Well-Architected Tool, a free service in the AWS console, that allows you to review your workloads against these pillars by answering a series of questions and receiving recommendations. It helps organizations build secure, high-performing, resilient, and efficient infrastructure for their applications.

---

**Q2. What are the 6 pillars of the Well-Architected Framework?**

The six pillars are Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization, and Sustainability. Operational Excellence focuses on running workloads effectively and continuously improving processes. Security covers protecting data, systems, and assets. Reliability ensures workloads perform their intended function correctly and consistently. Performance Efficiency focuses on using computing resources efficiently to meet requirements. Cost Optimization focuses on avoiding unnecessary costs and getting maximum business value. Sustainability, the newest pillar, focuses on minimizing the environmental impact of running cloud workloads by reducing energy consumption and carbon footprint.

---

**Q3. What is RTO and RPO in disaster recovery?**

RTO stands for Recovery Time Objective and represents the maximum acceptable time for a workload to be offline after a disaster. For example, an RTO of 4 hours means the system must be restored within 4 hours of a failure. RPO stands for Recovery Point Objective and represents the maximum acceptable amount of data loss measured in time. For example, an RPO of 1 hour means you can lose at most 1 hour of data, so backups must be taken every hour. These two metrics are defined by business requirements and drive the choice of disaster recovery strategy. Critical systems like banking require near-zero RTO and RPO requiring active-active multi-site deployments, while less critical systems can tolerate higher values and use cheaper backup-and-restore strategies.

---

**Q4. What is the Principle of Least Privilege?**

The Principle of Least Privilege means granting users, services, and systems only the minimum permissions they need to perform their specific tasks and nothing more. For example, a developer working on the frontend should only have access to S3 and CloudFront, not to RDS or IAM. A Lambda function that reads from DynamoDB should only have DynamoDB read permissions, not write or admin permissions. This principle limits the blast radius of security incidents — if an account is compromised, the attacker can only access what that account had permission to access. In AWS, this is implemented through IAM policies where you explicitly grant specific actions on specific resources rather than giving broad admin access.

---

**Q5. What are the four disaster recovery strategies in AWS?**

The four DR strategies in order of increasing cost and decreasing recovery time are: Backup and Restore, where data is backed up to S3 and restored when needed — this has the highest RTO of hours but is the cheapest option. Pilot Light, where a minimal version of the system runs in a second region and is scaled up during a disaster — RTO of tens of minutes. Warm Standby, where a reduced-capacity fully functional system runs in a second region and is scaled to full capacity during a disaster — RTO of minutes. Multi-Site Active-Active, where the full system runs simultaneously in multiple regions and traffic is instantly shifted — RTO of seconds but the most expensive option. Banks and stock exchanges use active-active while startups typically use backup and restore.

---

## Key Points — Phone Pe Save Karo

```
Well-Architected = AWS best practices framework — FREE!
6 Pillars        = CORSPS (Cost, Ops, Reliability, Security, Perf, Sus)
Operational      = IaC + Runbooks + Monitoring + Continuous improvement
Security         = Least Privilege + Encryption + MFA + Defense in Depth
Reliability      = Multi-AZ + Backups + Chaos Engineering + Auto Scaling
Performance      = Right service + Caching + CloudFront + Load testing
Cost             = Right sizing + Reserved + Spot + Delete unused
Sustainability   = Less resources + Graviton + Green regions
RTO              = Recovery Time (kitne time mein wapas online)
RPO              = Recovery Point (kitna data lose kar sakte hain)
Least Privilege  = Sirf zaroorat ka access — ek important rule!
Well-Arch Tool   = Free AWS Console tool — workload review karo
```
