# DAY 2 — EC2 (Elastic Compute Cloud)
**Date: 22 March 2026 | Student: Akash | Mentor: Claude AI**

---

## EC2 KYA HAI?

```
EC2 = Elastic Compute Cloud = AWS ka Virtual Server

Tumhara .NET API chalana hai?
→ EC2 pe deploy karo
→ 5 minutes mein server ready
→ Pay per hour
→ Band karo toh charge band
```

---

## EC2 KE 4 MAIN PARTS

### 1. AMI — Amazon Machine Image
```
Kya hai:  Server ka blueprint/template

Jaise .NET mein "New Project → Template"
Waise EC2 mein "Launch → AMI choose karo"

Types:
├── Amazon Linux  → Free, AWS ke liye best ✓
├── Ubuntu        → Popular Linux
├── Windows       → Costly (.NET ke liye)
└── Red Hat       → Enterprise
```

### 2. Instance Type — Server ki Size
```
t3.micro   → 2 CPU, 1GB RAM  → FREE TIER ✓
t3.small   → 2 CPU, 2GB RAM  → Paid
t3.medium  → 2 CPU, 4GB RAM  → Paid
t3.large   → 2 CPU, 8GB RAM  → Paid

Jitna bada instance → Utna zyada cost
```

### 3. Security Group — Firewall
```
Kya hai:  Server ka firewall — traffic control karta hai

Inbound Rules  = Kaun andar aa sakta hai
Outbound Rules = Kaun bahar ja sakta hai

Example:
Port 22  (SSH)   → Sirf tumhara IP
Port 80  (HTTP)  → Sabko allow
Port 443 (HTTPS) → Sabko allow
Port 1433 (SQL)  → Block!
```

### 4. Key Pair — Login Key
```
Kya hai:  Server mein login karne ki private key

.pem file download hoti hai
KABHI DELETE MAT KARNA!
Ye tumhara server ka darwaza hai

Bina key ke server mein ghus nahi sakte
```

---

## EC2 PRICING MODELS

```
On-Demand  → Jab chahiye chalao → Pay per hour
             TUMHARE LIYE YAHI ✓

Reserved   → 1-3 saal commit → 60-70% discount
             Production ke liye

Spot       → 90% discount → AWS kabhi bhi band kar sakta hai
             Batch jobs ke liye

Savings    → Flexible reserved pricing
Plan
```

---

## TUMHARA PEHLA SERVER — DETAILS

```
Instance ID:   i-0efc95a4ab1e35958
Name:          MyFirstServer
Type:          t3.micro (2 CPU, 1GB RAM)
Public IP:     13.201.16.163
Private IP:    172.31.45.192
Region:        ap-south-1 (Mumbai) ✓
State:         Stopped (band kiya)
```

---

## IMPORTANT TERMS

| Term | Meaning |
|------|---------|
| EC2 | AWS ka virtual server |
| AMI | Server ka template/blueprint |
| Instance Type | Server ki size (CPU + RAM) |
| Security Group | Server ka firewall |
| Key Pair | Server ka login key (.pem) |
| Public IP | Internet se access karne ka address |
| Private IP | Internal network address |
| On-Demand | Pay per hour pricing |
| EBS | Server ki hard disk (storage) |
| VPC | Server ka private network |

---

## INTERVIEW QUESTIONS — DAY 2

**Q: EC2 kya hai?**
> AWS ka virtual server service. On-demand compute capacity cloud mein.

**Q: AMI kya hota hai?**
> Amazon Machine Image — server ka template. OS, applications ka pre-configured package.

**Q: Security Group kya karta hai?**
> Virtual firewall jo EC2 instance ka inbound/outbound traffic control karta hai.

**Q: On-Demand vs Reserved vs Spot mein kya fark hai?**
> On-Demand=Pay per use, Reserved=1-3yr commit 60% discount, Spot=90% discount but interruptible

**Q: EC2 stop vs terminate mein kya fark hai?**
> Stop=Server band, data safe, restart kar sakte ho
> Terminate=Server delete, data gone — CAREFUL!

---

## HANDS-ON SUMMARY

```
Aaj kiya:
✓ EC2 Dashboard dekha
✓ "Launch Instance" pe gaye
✓ Amazon Linux 2023 AMI select ki
✓ t3.micro (Free Tier) select ki
✓ Key Pair banaya
✓ Security Group set kiya
✓ Server launch kiya — SUCCESS!
✓ Server properly stop kiya
```

---

## KAL KA TOPIC — DAY 3
```
S3 — Simple Storage Service
(AWS ka unlimited storage — files, images, videos sab)
```

---
*Day 2 Complete ✓ | Score: 2.5/3*
*Next: Day 3 — S3 Storage*
