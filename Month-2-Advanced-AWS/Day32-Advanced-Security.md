# Day 32 — AWS Advanced Security (WAF, Shield, GuardDuty, Inspector)

## 4 Security Services — Overview

```
WAF       → Request ka CONTENT check karo (SQL Injection, XSS)
Shield    → VOLUME check karo (DDoS attacks)
GuardDuty → BEHAVIOR check karo (Suspicious patterns)
Inspector → WEAKNESS dhundo (Vulnerabilities in EC2)
```

---

## Service 1 — WAF (Web Application Firewall)

### Problem
```
Hacker ne bheja:
URL: zomato.com/search?name=' OR 1=1; DROP TABLE users;--
→ SQL INJECTION ATTACK! Database delete ho sakta hai! ❌

Ya: <script>document.cookie="hacker"</script>
→ XSS ATTACK! Users ke browsers hack! ❌

Normal Security Group/Firewall ye rok nahi sakta!
(Ye valid HTTP requests lagte hain)
```

### Solution
```
WAF = Web Application Firewall

Normal Firewall → Port/IP check karta hai
WAF            → REQUEST KA CONTENT check karta hai!

WAF ne dekha → "SQL Injection hai!" → BLOCK! ✅
WAF ne dekha → "XSS hai!" → BLOCK! ✅
```

### Analogy
```
Normal Firewall = Airport ka main gate (ticket check)
WAF             = Airport ka baggage scanner (andar kya hai check)

Ticket valid + Andar bomb? → WAF pakad leta hai! ✅
```

### WAF Kahan Attach Hota Hai
```
→ Application Load Balancer (ALB)
→ CloudFront Distribution
→ API Gateway
```

### WAF Rules Types
```
Managed Rules (AWS ka ready-made):
→ Core Rule Set      = SQL Injection + XSS + Common attacks
→ Known Bad Inputs   = Log4j, shellshock attacks
→ IP Reputation List = Known hacker IPs block

Custom Rules (Khud banao):
→ Specific IP block karo
→ Specific country block karo
→ Rate limiting (ek IP se 1000 req/min se zyada nahi)
```

---

## Service 2 — AWS Shield (DDoS Protection)

### Problem
```
DDoS = Distributed Denial of Service

Hacker ke paas 1 lakh bots:
Normal traffic:   1,000 req/sec
DDoS attack:  1,00,00,000 req/sec

Server CRASH! Real users access nahi kar paate! ❌
```

### Solution
```
Shield STANDARD (FREE!):
→ Automatically sab AWS customers ko milta hai
→ Karna kuch nahi — automatic ON hai!
→ Basic DDoS attacks block karta hai
→ Layer 3 + Layer 4 protection

Shield ADVANCED ($3,000/month):
→ Large scale DDoS attacks handle karta hai
→ 24/7 AWS DDoS Response Team (DRT)
→ Real-time attack visibility
→ Financial protection (DDoS ki wajah se bill badha → AWS refund!)
```

### Analogy
```
Shield Standard = Stadium ka regular security guard (FREE) ✅
Shield Advanced = VIP match mein NSG commandos ($$$) ✅
```

### WAF + Shield Combo
```
WAF    → Smart attacks roko (SQL Injection, XSS)
Shield → Volume attacks roko (DDoS flood)

Dono saath = Complete protection! ✅
```

---

## Service 3 — GuardDuty (Intelligent Threat Detection)

### Problem
```
Andar andar ye ho raha tha — pata nahi chala:

1. IAM password crack → Raat 3 baje Brazil se login! 😱
2. EC2 Bitcoin mining site se baat kar raha hai!
3. S3 bucket se 50GB data bahar ja raha hai!

Koi alert nahi! Damage ho gaya! ❌
```

### Solution
```
GuardDuty = AWS ka AI-powered security CCTV!

Ye monitor karta hai:
→ CloudTrail logs  (kaun kya kar raha hai)
→ VPC Flow logs    (network traffic)
→ DNS logs         (kahan se requests)

AI + ML se patterns detect karta hai:

Normal:  Admin India se login ✅
ALERT:   Same admin Brazil se raat 3 baje? 🚨

Normal:  EC2 web traffic serve kare ✅
ALERT:   EC2 crypto mining site se baat kare? 🚨
```

### Analogy
```
Purana CCTV: Footage tha lekin koi dekhta nahi → Theft! ❌
GuardDuty:   AI CCTV jo khud suspicious pakde → Alert! ✅

24/7 automatic — tumhe kuch karna nahi! ✅
```

### GuardDuty vs WAF vs Shield
```
WAF        → Request content check (SQL Injection, XSS)
Shield     → Volume check (DDoS flood)
GuardDuty  → Behavior check (Suspicious patterns, insider threats)
```

---

## Service 4 — Inspector (Vulnerability Assessment)

### Problem
```
EC2 production mein chal raha hai. Lekin:
→ OS mein purani vulnerability hai (patch nahi kiya)
→ Installed software mein known bug hai
→ CVE-2024-1234 vulnerability exist karti hai

Hacker publicly jaanta hai ye vulnerability!
Tumhe pata hi nahi! ❌
```

### Solution
```
Inspector = Automatic Vulnerability Scanner!

EC2 scan karta hai:
→ OS patches check
→ Software vulnerabilities check
→ Network exposure check

Finding severity:
🔴 CRITICAL → Turant fix karo!
🟠 HIGH     → Jaldi fix karo!
🟡 MEDIUM   → Plan karo
🟢 LOW      → Monitor karo

Aur batata hai: "Ye kaise fix karo" ✅
```

### Analogy
```
Bina Inspector: Gaadi chal rahi → Brake fail! ❌
Inspector:      Mechanic aaya → "Brake pad ghisa, abhi badlo!" ✅

Inspector = AWS ka mechanic jo problems dhundh ke batata hai!
```

---

## Full Security Architecture

```
Internet
    ↓
🛡️ Shield    → DDoS attacks roko (Volume)
    ↓
🔥 WAF       → Smart attacks roko (Content)
    ↓
ALB → EC2 / App
    ↑
🔍 GuardDuty → Suspicious behavior pakdo (Behavior)
🔬 Inspector → Vulnerabilities dhundo (Weakness)
```

---

## Hands-On — Aaj Kya Kiya

### WAF Web ACL Banaya
```
WAF & Shield → Web ACLs → Create web ACL

Settings:
→ Resource type : Regional resources
→ Region        : Asia Pacific (Mumbai)
→ Name          : akash-demo-waf

Rule add kiya:
→ AWS Managed Rule Groups
→ Core Rule Set (AWS-AWSManagedRulesCommonRuleSet)
→ Capacity: 700 WCUs

Result: akash-demo-waf → Created ✅

Protection active:
→ SQL Injection blocked ✅
→ XSS attacks blocked ✅
→ Common web exploits blocked ✅

Cleanup: Deleted ✅ (Bill zero!)
```

---

## Interview Questions & Answers

**Q1. What is AWS WAF and what attacks does it protect against?**

AWS WAF is a Web Application Firewall that monitors and filters HTTP and HTTPS requests forwarded to your AWS resources. Unlike traditional firewalls that only check IP addresses and ports, WAF inspects the actual content of web requests. It protects against common web exploits such as SQL injection, where attackers attempt to manipulate database queries through input fields, and Cross-Site Scripting or XSS, where malicious scripts are injected into web pages. WAF can be attached to Application Load Balancers, CloudFront distributions, and API Gateway. You can use AWS managed rule groups for ready-made protection or create custom rules based on your specific requirements.

---

**Q2. What is the difference between AWS Shield Standard and Shield Advanced?**

AWS Shield Standard is automatically enabled for all AWS customers at no additional cost. It provides protection against common and most frequently occurring network and transport layer DDoS attacks. Shield Advanced is a paid service costing $3,000 per month that provides enhanced protection against larger and more sophisticated DDoS attacks. It includes access to the AWS DDoS Response Team available 24/7, real-time attack visibility through CloudWatch dashboards, and financial protection where AWS provides bill credits if your costs increase due to a DDoS attack.

---

**Q3. What is Amazon GuardDuty and how does it work?**

Amazon GuardDuty is an intelligent threat detection service that continuously monitors your AWS account for malicious activity and unauthorized behavior. It analyzes data from three sources: AWS CloudTrail logs to track API calls and account activity, VPC Flow Logs to monitor network traffic, and DNS logs to detect suspicious domain queries. GuardDuty uses machine learning and threat intelligence to identify anomalies such as unusual login locations, EC2 instances communicating with known malicious IP addresses, or large amounts of data being exfiltrated from S3 buckets. It requires no additional software to deploy and works automatically once enabled.

---

**Q4. What is Amazon Inspector and how is it different from GuardDuty?**

Amazon Inspector is an automated vulnerability assessment service that scans your EC2 instances and container images for software vulnerabilities and unintended network exposure. It checks for known CVEs in the operating system and installed packages and provides findings ranked by severity as Critical, High, Medium, or Low, along with remediation recommendations. The key difference from GuardDuty is that Inspector looks for weaknesses inside your resources before an attack happens, while GuardDuty detects suspicious behavior and threats that are actively occurring. Inspector is proactive vulnerability scanning, while GuardDuty is reactive threat detection.

---

**Q5. How would you design a complete security architecture for a web application on AWS?**

A complete security architecture would layer multiple services. At the network perimeter, AWS Shield Standard provides automatic DDoS protection for all traffic. AWS WAF is attached to the Application Load Balancer to inspect incoming HTTP requests and block SQL injection, XSS, and other common web exploits using managed rule groups. EC2 instances are placed in private subnets with Security Groups allowing only necessary ports. Amazon GuardDuty is enabled account-wide to continuously monitor CloudTrail, VPC Flow Logs, and DNS logs for suspicious behavior patterns. Amazon Inspector regularly scans EC2 instances for vulnerabilities and missing patches. IAM roles follow the principle of least privilege, and AWS KMS encrypts sensitive data at rest. This defense-in-depth approach ensures multiple layers of protection.

---

## Quick Wording — Ye Exact Words Use Karo (Interview mein!)

```
WAF:
→ "WAF inspects the content of HTTP requests"
→ "Protects against SQL injection and XSS attacks"
→ "Attached to ALB, CloudFront, or API Gateway"

Shield:
→ "Standard = FREE, automatic, all customers"
→ "Advanced = $3000/month, 24/7 DRT, financial protection"
→ "Protects against DDoS attacks"

GuardDuty:
→ "Analyzes CloudTrail, VPC Flow Logs, and DNS logs"
→ "Uses ML to detect suspicious behavior"
→ "No software to install — fully managed"

Inspector:
→ "Scans EC2 for known CVEs and vulnerabilities"
→ "Proactive — finds weakness before attack"
→ "GuardDuty = active threats, Inspector = vulnerabilities"
```

---

## Key Points — Phone Pe Save Karo

```
WAF     = Content check → SQL Injection + XSS block
Shield  = Volume check → DDoS block (Standard FREE!)
GuardDuty = Behavior check → Suspicious patterns detect
Inspector = Weakness check → CVE vulnerabilities scan

Security Layers:
Internet → Shield → WAF → ALB → EC2
GuardDuty = Account-wide behavior monitoring
Inspector = EC2-level vulnerability scanning

Remember:
WAF attach hota hai = ALB / CloudFront / API Gateway
GuardDuty ON karo = CloudTrail + VPC Flow + DNS automatically monitor
Inspector = EC2 ke andar ki problems dhundta hai
Shield Standard = FREE, already ON hai tumhare account mein!
```
