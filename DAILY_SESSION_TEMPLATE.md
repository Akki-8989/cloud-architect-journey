# DAILY SESSION TEMPLATE
### Mentor: Claude AI | Student: Akash | Goal: TOP-CLASS CLOUD ARCHITECT
### Target: 20-28 LPA (Month 8) → 35-45 LPA (Year 3+)

---

## HAR DIN KA STRUCTURE (~2 Hours)

---

### PART 1 — RECALL (10-15 min)
**1-4-7 Rule — Kal ka topic yaad hai ya nahi**

```
→ Bina notes dekhe 2-3 questions ka jawab do
→ Yaad nahi aaya → Yaad dilaaunga → Aage badhenge
→ 1-4-7 Tracker update karo (REVISION_TRACKER.md)
```

**1-4-7 Rule:**
```
+1 din  → RECALL  : Bina notes explain karo
+4 din  → APPLY   : Real-world problem solve karo
+7 din  → TEACH   : Apne words mein likho + Interview Q&A
```

---

### PART 2 — NAYA TOPIC THEORY (30-40 min)

```
Step 1: PROBLEM batao — ye topic kyun zaroori hai?
Step 2: SOLUTION samjhao — concept clearly
Step 3: ANALOGY do — Zomato/Swiggy/PUBG/Bank
Step 4: DIAGRAM banao — text se architecture
Step 5: COMPARISON — similar services se fark
Step 6: Quick Q&A — concept clear hua?
```

**Teaching Rules:**
```
→ Hinglish mein samjhaunga
→ Koi shortcut nahi — properly samjhaunga
→ Stuck ho → Turant bolo → Dobara samjhaunga
→ Concept clear hone ke baad hi aage badhenge
→ WORDING CORRECTION RULE: Agar answer ki wording galat ho →
  Exact sahi wording batao + Notes mein "Quick Wording" section mein add karo
  (Half-correct answer = wrong answer in interview!)
```

---

### PART 3 — HANDS-ON ⚠️ (30-40 min) — KABHI SKIP NAHI!

```
Step 1: AWS Console pe resource create karo
Step 2: Kaam karta hai verify karo
Step 3: Real scenario test karo
Step 4: CLEANUP karo! (bill nahi aana chahiye)
```

**Format:**
```
→ Step by step guide dunga
→ Tum karo → Screenshot do → Next step
→ Error aaye → Milke fix karenge
```

**Cleanup Checklist:**
```
❗ EC2       → Stop ya Terminate
❗ RDS       → Delete
❗ NAT GW    → Delete (costly!)
❗ Load Balancer → Delete
❗ Elastic IP → Release

Free hain (rehne do):
✅ S3 buckets (small data)
✅ IAM users/roles
✅ Security Groups
✅ VPCs
```

---

### PART 4 — NOTES + GITHUB (10-15 min)

```
Notes mein HAMESHA hoga:
✅ Real world problem (kyun zaroori hai)
✅ Solution + Concept
✅ Analogy (Zomato/Swiggy/PUBG)
✅ Architecture diagram (text se)
✅ Hands-On steps (kya kiya)
✅ Interview Q&A (minimum 4-5 — English mein)
✅ Key Points (phone pe save karne layak)

GitHub:
→ git add → git commit → git push
→ Commit: "Day XX - Topic complete"
```

---

### PART 5 — MOCK INTERVIEW (10 min) — ENGLISH MEIN!

```
→ 2-3 questions puchhunga — English mein jawab do!
→ Spoken style mein answer karo (interview jaisa!)
→ Wording galat hui → Main correct karunga
→ Better answer suggest karunga

Format:
Q: "What is [topic]?"
Tum: English mein answer likho
Main: Wording fix + Better version bataunga

Kyun zaroori hai:
→ Technical knowledge hona + English mein bolna = Job milti hai!
→ Sirf knowledge = Interview fail ho sakta hai ❌
→ Knowledge + Communication = SUCCESS ✅
```

---

### PART 6 — NOTEBOOK WRITING (5 min) — HAATH SE LIKHO!

```
Har session ke baad physical notebook mein likho.

Language Rule:
→ Technical words ENGLISH mein likho  (SQS, Lambda, Queue, Trigger)
→ Explanation HINDI mein likho        (kya karta hai, kyun use karte hain)
→ Pure Hindi ya Pure English NAHI     (Hinglish = best for understanding)

Format (notebook mein):
┌──────────────────────────────────────────────────────┐
│ Day 36 — SQS + SNS             Date: 08 July 2026   │
│                                                      │
│ SQS kya hai:                                         │
│   Message Queue service hai.                         │
│   Producer message daalta hai → Queue mein safe      │
│   rehta hai → Consumer ready hua → Uthaya → Process. │
│                                                      │
│ SNS kya hai:                                         │
│   Pub/Sub Notification service hai.                  │
│   1 message publish karo → Sab subscribers ko        │
│   turant milta hai.                                  │
│                                                      │
│ Fark:                                                │
│   SQS = 1 Consumer (Post Box)                        │
│   SNS = Multiple Consumers (WhatsApp Group)          │
│                                                      │
│ Combined use:                                        │
│   Order aaya → SNS broadcast kiya → SQS Queue 1     │
│   (Kitchen) + SQS Queue 2 (Payment) — dono ko       │
│   ek saath pata chala ✅                             │
│                                                      │
│ Key Points:                                          │
│   1. SQS = Message safe rakhta hai (crash safe)     │
│   2. SNS = Ek message → Multiple services ko        │
│   3. Dono saath = Fan-out pattern (best practice)   │
└──────────────────────────────────────────────────────┘

Kyun zaroori hai:
→ Haath se likhna = Brain mein deep set hota hai ✅
→ Hinglish format = Reading easy + Concept clear ✅
→ Revision notebook = Quick recap before interview ✅
```

---

### PART 7 — NEXT DAY PREP (5 min)

```
→ 1-4-7 Tracker update kiya? ✅
→ Aaj ka topic Tracker mein add kiya? ✅
→ Koi doubt bacha? Seedha poocho! ✅
→ Kal ka topic: [bataunga] ✅
```

---

## NOTES FILE FORMAT — HAR DIN

```markdown
# Day XX — Topic Name

## Topic Kya Hai
[Full explanation in Hinglish]

## Kaise Kaam Karta Hai
[Detailed explanation]

## Real Life Example
[Practical example]

## Architecture mein Kahan Use Hota Hai
[Diagram ya explanation]

## Hands-On — Aaj Kya Kiya
[Step by step jo kiya]

## Interview Questions & Answers
[Minimum 5 Q&A in English]

## Key Points — Phone Pe Save Karo
[5-7 one-liners]
```

---

## MONTH 1 — REVISED DAY BY DAY PLAN

| Day | Topic | Status |
|-----|-------|--------|
| Day 01 | Cloud kya hai, IaaS/PaaS/SaaS, AWS Structure | ✅ Done |
| Day 02 | EC2 Basics — AMI, Instance Types, Security Groups | ✅ Done |
| Day 03 | S3 Storage — Buckets, Objects, Storage Classes | ✅ Done |
| Day 04 | RDS — Managed Database, Multi-AZ, Read Replicas | ✅ Done |
| Day 05 | IAM — Users, Groups, Roles, Policies, MFA | ✅ Done |
| Day 06 | VPC Basics — Subnets, Internet Gateway, Route Table | ✅ Done |
| Day 07 | REVISION — Day 1 to 6 | ✅ Done |
| Day 08 | EBS — EC2 ka Storage, Volumes, Snapshots | 🔄 Current |
| Day 09 | Security Groups — Hands-on Rules, Inbound/Outbound | ⬜ Pending |
| Day 10 | Apna VPC Banana — Public + Private Subnet | ⬜ Pending |
| Day 11 | CloudWatch — Monitoring, Alarms, Logs | ⬜ Pending |
| Day 12 | Billing + Cost Management — Free Tier, Budget Alert | ⬜ Pending |
| Day 13 | EC2 Deep Dive — Elastic IP, User Data, Web Server | ⬜ Pending |
| Day 14 | REVISION — Day 1 to 13 + Mini Project | ⬜ Pending |

---

## GOLDEN RULES — KABHI MAT BHOOLNA

```
1. Revision pehle — naya topic baad mein
2. Har concept hands-on karna zaroori hai
3. Notes banana — GitHub pe push karna
4. Interview questions English mein
5. Koi bhi shortcut nahi — poora samajhna hai
6. Roz 1 ghanta — no excuse, no skip
7. Jo nahi samjha — turant poocho, aage mat badho
```

---

## SALARY PROGRESSION

```
Abhi:           Developer salary
Month 4:        15-22 LPA (AWS SAA Certification)
Month 7:        25-35 LPA (AWS SAP Certification)
Year 2:         35-50 LPA (Experience + Projects)
```

---

*Template Version: April 2026*
*Mentor: Claude AI*
*"Zero se Best tak — sahi neenv ke saath"*
