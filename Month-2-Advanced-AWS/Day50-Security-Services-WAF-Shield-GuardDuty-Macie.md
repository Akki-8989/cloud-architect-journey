# Day 50 - AWS Security Services (WAF + Shield + GuardDuty + Macie)

## Problem
```
Company pe 4 alag attacks:
1. SQL injection -> Database wipe! x
2. DDoS -> 1M fake requests -> Server down! x
3. EC2 suspicious country pe data bhej raha hai x
4. S3 mein credit card numbers unprotected! x

Ek solution sab handle nahi kar sakta ->
Har attack ke liye alag service!
```

## 4 Services Overview
```
WAF       -> Web attacks rokta hai (SQL injection, XSS)
Shield    -> DDoS attacks se bachata hai
GuardDuty -> Suspicious activity detect karta hai
Macie     -> Sensitive data dhundhta hai (S3 mein)
```

## Analogy - Building Security
```
WAF       = Security guard at entrance
            "Ye request valid nahi — andar nahi jaane do"

Shield    = Bullet-proof glass
            "Hazaron log attack karein — koi fark nahi"

GuardDuty = CCTV + AI camera
            "Ye banda suspicious jagah ja raha hai — alert!"

Macie     = Inspector jo files check kare
            "Is folder mein credit card numbers hain — flag!"
```

## AWS WAF - Web Application Firewall
```
Kya: Web requests filter karta hai — bad traffic rokta hai
Kahan: ALB, CloudFront, API Gateway ke saamne lagta hai

Kya rokta hai:
  SQL Injection:  "'; DROP TABLE--"  x
  XSS:            "<script>steal()</script>"  x
  Bad IPs         x
  Specific countries  x

Flow:
  Request -> WAF (Rules check) -> Match? Block / Allow

Managed Rules: AWS ke ready-made rules (free + paid)
  SQL database rule = SQLi attacks block karta hai
```

## AWS Shield - DDoS Protection
```
DDoS = Hazaron fake requests -> Server crash

Shield Standard (FREE):
  -> Automatically sab AWS customers ko milta hai
  -> Common DDoS attacks se protect
  -> Layer 3/4 network level protection

Shield Advanced (PAID ~$3000/month):
  -> Large sophisticated attacks
  -> Real-time visibility
  -> 24/7 DDoS Response Team (DRT)
  -> Cost protection (bill spike nahi aayega)
```

## AWS GuardDuty - Threat Detection
```
Kya: AI/ML se suspicious activity detect karta hai
Sources: VPC Flow Logs + CloudTrail + DNS logs

Kya detect karta hai:
  -> EC2 se cryptocurrency mining
  -> Unusual country se login
  -> Known malicious IP se communication
  -> Data exfiltration (data bahar ja raha hai)

Ek baar enable karo -> Automatically monitor karta hai
Cost: Pay per use (logs analyze karne ka charge)
```

## AWS Macie - Data Security
```
Kya: S3 mein sensitive data dhundhta hai (ML use karta hai)

Kya dhundhta hai:
  -> Credit card numbers
  -> Social Security Numbers (SSN)
  -> Passwords, PII data

Use case:
  Developer ne accidentally credit card data S3 pe upload kiya
  -> Macie dhundhega -> Alert dega
```

## Comparison - Kab Kaunsa?
```
Attack/Need                     Service
Web attacks (SQL, XSS)       -> WAF
DDoS attack                  -> Shield
Suspicious account activity  -> GuardDuty
Sensitive data in S3         -> Macie

Best Practice Combination:
CloudFront + WAF + Shield = Complete web protection
GuardDuty + Macie         = Complete data protection
```

## Hands-On - Aaj Kya Kiya
```
WAF Console khola
Web ACL banaya: akash-demo-waf (Regional, Mumbai)
SQL injection rule add kiya:
  AWS-AWSManagedRulesSQLiRuleSet (Capacity: 200 WCUs)
  Default action: Allow (sirf SQLi block)
Verified: WAF successfully created!
Cleanup: Deleted (paid service) ✅
```

## Interview Questions & Answers

**Q1: What is AWS WAF and what does it protect against?**
A: AWS WAF (Web Application Firewall) is a service that filters incoming web traffic to protect applications from common web exploits. It can block SQL injection attacks, Cross-Site Scripting (XSS), bad IP addresses, and traffic from specific countries. It sits in front of CloudFront, Application Load Balancer, or API Gateway and evaluates each request against defined rules before allowing it to reach the application.

**Q2: What is the difference between AWS Shield Standard and Shield Advanced?**
A: Shield Standard is free and automatically protects all AWS customers from common DDoS attacks at the network and transport layer (Layer 3/4). Shield Advanced is a paid service (~$3000/month) that provides protection against larger, more sophisticated DDoS attacks, real-time attack visibility, 24/7 access to the DDoS Response Team (DRT), and cost protection so your AWS bill doesn't spike during an attack.

**Q3: What is AWS GuardDuty?**
A: AWS GuardDuty is a threat detection service that continuously monitors your AWS account for malicious activity and unauthorized behavior using machine learning. It analyzes VPC Flow Logs, CloudTrail logs, and DNS logs to detect threats like cryptocurrency mining on EC2 instances, unusual API calls, communication with known malicious IPs, and potential data exfiltration. You enable it once and it monitors automatically.

**Q4: What is the difference between WAF and Security Groups?**
A: Security Groups work at the network layer — they control traffic based on IP addresses and port numbers (allow port 80, block everything else). WAF works at the application layer — it inspects the actual content of HTTP requests to detect malicious patterns like SQL injection or XSS. WAF is more intelligent as it understands web application protocols and can block sophisticated attacks that Security Groups cannot.

**Q5: What is AWS Macie used for?**
A: AWS Macie is a data security service that uses machine learning to automatically discover, classify, and protect sensitive data stored in S3 buckets. It can detect credit card numbers, social security numbers, passwords, and other personally identifiable information (PII). When sensitive data is found, it generates findings and alerts so you can take action to protect it.

## Key Points - Phone Pe Save Karo
```
1. WAF = Web attacks block (SQL injection, XSS)
2. Shield Standard = FREE, all customers, basic DDoS
3. Shield Advanced = PAID, sophisticated DDoS + DRT support
4. GuardDuty = AI threat detection (enable once, auto monitor)
5. Macie = Sensitive data finder in S3 (ML based)
6. WAF + Shield = Web protection combo
7. GuardDuty + Macie = Data protection combo
8. WAF vs SG: WAF = App layer | SG = Network layer
```
