# Day 46 — AWS CloudTrail + AWS Config

## Problem — Koi Audit Trail Nahi

```
Company mein kuch hua:
"Koi S3 bucket delete kar gaya!"
"Koi EC2 band kar gaya production mein!"
"Kisne kiya? Kab kiya? Kahan se kiya?"

Koi nahi jaanta — koi record nahi! ❌
Security team pareshan ❌
Compliance audit fail ❌
```

---

## Solution

```
CloudTrail = "Kya HUA?" — Past events ka record
AWS Config = "Kya HAI abhi?" — Current state check + Rules
```

---

## Analogy

**CloudTrail — CCTV Camera:**
```
Ghar mein CCTV lagao:
→ Kaun aaya? Kab aaya? Kya kiya?
→ Sab record rehta hai ✅

AWS CloudTrail = AWS ka CCTV!
→ Kaun ne AWS mein kya kiya?
→ Kab kiya? Kahan se kiya? (IP address bhi!)
→ Sab log hota hai → S3 mein save ✅
```

**AWS Config — Bank ka Compliance Officer:**
```
CloudTrail = Bank ka transaction history
             "2:30 PM pe Rahul ne 10,000 nikale"
             Past mein kya hua — complete record ✅

AWS Config = Bank ka compliance officer
             "Koi account overdraft mein nahi hona chahiye"
             Rule check karta rehta hai — breach hone pe alert ✅
```

---

## AWS CloudTrail

### Kya karta hai:
```
AWS mein koi bhi action karo (Console click / CLI command)
→ CloudTrail automatically log karta hai
→ WHO + WHAT + WHEN + WHERE sab record! ✅
```

### 3 Main Cheezein:
```
1. API Calls Record karta hai
   → AWS Console pe click = API call = CloudTrail log ✅
   → AWS CLI command = API call = CloudTrail log ✅

2. WHO + WHEN + WHERE
   → User kaun tha (IAM user/role)
   → Kab kiya (timestamp)
   → Kahan se kiya (IP address)

3. S3 mein store hota hai
   → 90 days free CloudTrail console mein
   → Zyada time chahiye → S3 bucket mein save karo
```

### Real Use Cases:
```
→ "Kisne ye resource delete kiya?" — CloudTrail mein dekho
→ Security breach hua — track karo kaun ne kya kiya
→ Compliance audit — proof do ki sab track tha
```

---

## AWS Config

### Kya karta hai:
```
Resources ka current state track karta hai
Rules set karo → Rule break hone pe alert! ✅

Example Rules:
→ "Koi S3 bucket public nahi hona chahiye"
→ "Har EC2 pe encryption hona chahiye"
→ Rule break hua → Non-compliant flag → Alert ✅
```

### 3 Main Cheezein:
```
1. Resource Inventory
   → Tumhare AWS mein kya kya hai — complete list ✅

2. Configuration History
   → Koi resource change hua → pehle kya tha, ab kya hai ✅

3. Rules (Compliance Check)
   → Managed Rules: AWS ke ready-made rules (753 rules!)
   → Custom Rules: Apne rules banao
   → Non-compliant → Red flag + Alert ✅
```

### Real Use Cases:
```
→ "Koi S3 bucket public toh nahi?" — Config rule check karo
→ "Sab EC2 pe encryption hai?" — verify karo
→ Company policy enforce karna hai — rules lagao
```

---

## CloudTrail vs AWS Config

| Feature | CloudTrail | AWS Config |
|---------|-----------|------------|
| Kya track karta | API calls/Events | Resource state |
| Sawaal | "Kisne kiya?" | "Kya hai abhi?" |
| Time focus | Past events | Current + Changes |
| Use case | Security audit | Compliance check |
| Default | ON (free 90 days) | OFF (manually enable) |

---

## Architecture

```
CloudTrail:
Developer/Admin → AWS Console/CLI
                        ↓ (API call)
                  CloudTrail logs
                        ↓
                  S3 bucket (stored)
                        ↓
                  "Who did what, when, from where" ✅

AWS Config:
AWS Resources (S3, EC2, RDS...)
        ↓ (state change detected)
   AWS Config Recorder
        ↓ (evaluate against rules)
   Rules check karo
        ↓ (rule break hua?)
   Non-compliant → Alert ✅
```

---

## Hands-On — Aaj Kya Kiya

### CloudTrail:
- Event History khola (default ON tha) ✅
- Recent events dekhe:
  - `DeleteSecret` → July 21, 19:49 → akash/demo/db-password ← Kal ka kaam!
  - `CreateSecret` → July 21, 19:45 → akash/demo/db-password
  - `DeletePipeline` → July 18 → akash-demo-pipeline ← Day 44!
  - `CreatePipeline` → July 18 → akash-demo-pipeline
- `DeleteSecret` event click kiya → Details dekhe:
  - User: root
  - IP: 105.117.184.80 (mera IP!)
  - Exact timestamp ✅

### AWS Config:
- 1-click setup kiya ✅
- Rule add kiya: `s3-bucket-public-read-prohibited`
  - Kya karta hai: S3 buckets public read access check karta hai
  - Type: AWS Managed, Detective ✅
- Recording band kar diya (cleanup) ✅

---

## WHY Framework

**CloudTrail kab use karu?**
- Security incident hua → "Kisne kiya?" investigate karna ho
- Compliance audit → Proof chahiye ki sab track tha
- Unusual activity detect karna ho

**AWS Config kab use karu?**
- "Koi resource wrong configuration mein toh nahi?" check karna ho
- Company security policy enforce karni ho
- Drift detect karna ho (koi resource change kiya kisi ne)

---

## Interview Questions & Answers

**Q1: What is AWS CloudTrail?**
A: AWS CloudTrail is a service that records all API calls made in your AWS account — whether from the Console, CLI, or SDK. It captures WHO made the call, WHAT action was taken, WHEN it happened, and WHERE it came from (IP address). It's AWS's audit log — the equivalent of a CCTV camera for your AWS account. Logs are stored for 90 days free in the console, or longer in S3.

**Q2: What is AWS Config?**
A: AWS Config is a service that continuously monitors and records the configuration of your AWS resources. It lets you set compliance rules (like "no S3 bucket should be public") and automatically evaluates your resources against those rules. When a rule is violated, it flags the resource as non-compliant and can trigger alerts.

**Q3: What is the difference between CloudTrail and AWS Config?**
A: CloudTrail answers "WHO did WHAT and WHEN?" — it's a log of all actions/events. AWS Config answers "What IS the current state?" — it tracks resource configurations and checks compliance rules. CloudTrail is for security auditing (who deleted what), while AWS Config is for compliance monitoring (are resources configured correctly right now).

**Q4: Is CloudTrail enabled by default?**
A: Yes, CloudTrail Event History is enabled by default for all AWS accounts and keeps 90 days of management events for free. For longer retention or data events (like S3 object-level logging), you need to create a Trail and store logs in S3.

**Q5: What are AWS Config Rules?**
A: AWS Config Rules are compliance checks that evaluate your resources against desired configurations. AWS provides 753+ managed rules (pre-built, like s3-bucket-public-read-prohibited), and you can also create custom rules using Lambda. When a resource violates a rule, Config marks it as NON_COMPLIANT and can trigger SNS notifications or auto-remediation.

---

## Key Points — Phone Pe Save Karo

```
1. CloudTrail = AWS ka CCTV — WHO + WHAT + WHEN + WHERE ✅
2. CloudTrail default ON hai — 90 days free ✅
3. AWS Config = Compliance checker — rules set karo ✅
4. Config = "Kya hai abhi?" | CloudTrail = "Kya hua tha?"
5. Config Rules: 753+ managed rules available
6. Non-compliant resource → Alert aata hai ✅
7. CloudTrail → Security audit
8. AWS Config → Compliance enforcement
```
