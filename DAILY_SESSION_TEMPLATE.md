# DAILY SESSION TEMPLATE
### Mentor: Claude AI | Student: Akash | Goal: TOP-CLASS CLOUD ARCHITECT
### Target: 20-28 LPA (Month 8) -> 35-45 LPA (Year 3+)

---

## HAR DIN KA STRUCTURE (~2 Hours)

---

### PART 1 - RECALL (10-15 min)
**1-4-7 Rule - Kal ka topic yaad hai ya nahi**

```
-> Bina notes dekhe 2-3 questions ka jawab do
-> Yaad nahi aaya -> Yaad dilaaunga -> Aage badhenge
-> 1-4-7 Tracker update karo (REVISION_TRACKER.md)
```

**1-4-7 Rule:**
```
+1 din  -> RECALL  : Bina notes explain karo
+4 din  -> APPLY   : Real-world problem solve karo
+7 din  -> TEACH   : Apne words mein likho + Interview Q&A
```

---

### PART 2 - NAYA TOPIC THEORY (30-40 min)

**TEACHING FLOW - HAR TOPIC KE LIYE:**
```
Step 1: PROBLEM - Ye service kyun bani? Kya problem thi pehle?
        (Bina problem ke solution samajh nahi aata)

Step 2: SOLUTION - Service kaise solve karti hai?
        (Concept clearly + andar kya hota hai)

Step 3: ANALOGY - Real life se connect karo
        (Zomato/PUBG/Bank/Hospital - jo samajh aaye)

Step 4: DEEP CONCEPTS - Andar kya hota hai?
        (Sirf "kya hai" nahi - "kaise kaam karta hai" bhi)

Step 5: ARCHITECTURE DIAGRAM - Text se visualize karo
        (Flow clearly dikhao)

Step 6: COMPARISON - Alternative kya tha?
        (RDS vs DynamoDB, SQS vs SNS, EC2 vs Lambda)

Step 7: 3 SAWAAL - Decision making practice
        (Niche dekho)

Step 8: Q&A - Concept clear hua? Doubts?
```

**SHORT MEIN MAT SAMJHAO - PROPER DEPTH CHAHIYE:**
```
Wrong (short):
"DynamoDB flexible database hai, fast hai" <- sirf surface

Correct (proper depth):
Problem: SQL mein schema fixed hota hai - har row same columns.
         10,000 orders mein agar ek order ka discount hai
         aur baaki ka nahi - SQL mein NULL daalna padta = messy

Solution: DynamoDB mein har item ke alag attributes ho sakte hain.
          ORD-001: orderId, customer, item, price
          ORD-002: orderId, customer, item, price, discount <- extra OK!

Deep: Partition Key se DynamoDB decide karta hai data
      kaunse partition (server) pe store hoga.
      Isliye Query fast hoti hai - seedha us partition pe jaata hai!

Comparison: RDS vs DynamoDB
            RDS      = Schema fixed, JOINs possible, slow at massive scale
            DynamoDB = Schema flexible, No JOINs, always fast

Depth se samajhna = Interview mein confidently explain karna
```

**HAR NAYE SERVICE KE BAAD - 3 SAWAAL ZAROOR POOCHHO:**
```
1. Isko KAB use karu?
   (Exact scenario batao - kaunsi situation mein perfect fit)

2. Isko KAB NA use karu?
   (Limitations kya hain - kab avoid karna chahiye + kyun)

3. Doosra option kya tha, aur maine YE KYUN chuna?
   (Dono compare karo - real scenario mein decision explain karo)

Ye 3 sawaal = Interview mein "Why did you choose X?" ka perfect answer!
```

**Teaching Rules:**
```
-> Hinglish mein samjhaunga
-> Koi shortcut nahi - properly samjhaunga
-> Stuck ho -> Turant bolo -> Dobara samjhaunga
-> Concept clear hone ke baad hi aage badhenge
-> WORDING CORRECTION RULE: Agar answer ki wording galat ho ->
  Exact sahi wording batao + Notes mein "Quick Wording" section mein add karo
  (Half-correct answer = wrong answer in interview!)
```

---

### PART 3 - HANDS-ON (30-40 min) - KABHI SKIP NAHI!

```
Step 1: AWS Console pe resource create karo
Step 2: Kaam karta hai verify karo
Step 3: Real scenario test karo
Step 4: CLEANUP karo! (bill nahi aana chahiye)
```

**Format:**
```
-> Step by step guide dunga
-> Tum karo -> Screenshot do -> Next step
-> Error aaye -> Milke fix karenge
```

**COMMAND EXPLAIN KARNE KA RULE - KABHI MAT BHOOLNA:**
```
Jab bhi koi CLI command do (AWS CLI, Docker, Linux, Git):
HAMESHA ye 3 cheezein batao:

1. YE COMMAND KYA KARTA HAI?
   (Ek line mein - simple words mein)

2. KYUN ZAROOR HAI YE STEP?
   (Bina is step ke kya hoga?)

3. OUTPUT KYA AAYEGA?
   (Success mein kya dikhega)

Wrong (sirf command dena):
   "docker push 313038579212.dkr.ecr.ap-south-1.amazonaws.com/akash-nginx-repo:latest"

Correct (explain karke dena):
   "docker push - ye command image ko ECR mein upload karta hai.
    Kyun: Tag karne ke baad image sirf tumhare paas hai, ECR mein
    nahi. Push karne se ECR mein actually save hoti hai.
    Output: 'Pushed' dikhega har layer ke liye"

Akash ko commands nahi aate - har command properly explain karo!
```

**Cleanup Checklist:**
```
EC2          -> Stop ya Terminate
RDS          -> Delete
NAT Gateway  -> Delete (costly!)
Load Balancer -> Delete
Elastic IP   -> Release

Free hain (rehne do):
S3 buckets (small data)
IAM users/roles
Security Groups
VPCs
```

---

### PART 4 - NOTES + GITHUB (10-15 min)

```
Notes mein HAMESHA hoga:
- Real world problem (kyun zaroori hai)
- Solution + Concept
- Analogy (Zomato/Swiggy/PUBG)
- Architecture diagram (text se)
- Hands-On steps (kya kiya)
- Interview Q&A (minimum 4-5 - English mein)
- Key Points (phone pe save karne layak)

GitHub:
-> git add -> git commit -> git push
-> Commit: "Day XX - Topic complete"
```

---

### PART 5 - MOCK INTERVIEW (10 min) - ENGLISH MEIN!

```
-> 2-3 questions puchhunga - English mein jawab do!
-> Spoken style mein answer karo (interview jaisa!)
-> Wording galat hui -> Main correct karunga
-> Better answer suggest karunga

Format:
Q: "What is [topic]?"
Tum: English mein answer likho
Main: Wording fix + Better version bataunga

Kyun zaroori hai:
-> Technical knowledge hona + English mein bolna = Job milti hai!
-> Sirf knowledge = Interview fail ho sakta hai
-> Knowledge + Communication = SUCCESS
```

---

### PART 6 - NOTEBOOK WRITING (5 min) - HAATH SE LIKHO!

```
Har session ke baad physical notebook mein likho.

Notes ka RULE: Short + Clear - Ek baar padho, sab yaad ho jaye!
- Itna chota likho ki 2 min mein padh lo
- Itna clear likho ki bina sochhe samajh aaye
- Analogy zaroor likho - concept pakka ho jaata hai

FORMAT (ye use karo har din):

DAY XX - Topic Name                    Date: DD Mon YYYY

------------------------
Service/Concept 1 = Ek line mein kya hai
------------------------
Kya: Kya karta hai (1 line)
Analogy: Real life se (Bank/CCTV/Zomato)
Use: Kab use karu (1 line)

------------------------
Service/Concept 2 = Ek line mein kya hai
------------------------
Kya: Kya karta hai (1 line)
Analogy: Real life se
Use: Kab use karu (1 line)

------------------------
DIFFERENCE (agar 2 services hain)
------------------------
Service 1 -> "Kab use karu?" (1 line)
Service 2 -> "Kab use karu?" (1 line)

EXAMPLE (Day 46 - CloudTrail + AWS Config):

DAY 46 - CloudTrail + AWS Config       Date: 22 July 2026

------------------------
CloudTrail = AWS ka CCTV
------------------------
Kya: Har AWS action record karta hai
Kya milta hai: WHO + WHAT + WHEN + WHERE (IP bhi!)
Default: ON hai - 90 din free
Use: "Kisne kiya?" investigate karna ho tab

------------------------
AWS Config = Compliance Checker
------------------------
Kya: Resources ka current state check karta hai
Rules: Set karo -> break hone pe alert
Example rule: S3 bucket public nahi honi chahiye
Use: "Sab resources sahi hain?" verify karna ho tab

------------------------
DIFFERENCE
------------------------
CloudTrail -> "Kya HUA tha?" (past)
AWS Config -> "Kya HAI abhi?" (present)

Security incident -> CloudTrail dekho
Compliance check  -> AWS Config dekho

Kyun zaroori hai:
-> Haath se likhna = Brain mein deep set hota hai
-> Short format = Revision mein 2 min mein sab yaad
-> Ek baar padho = Sab clear ho jaye - yahi goal hai!
```

---

### PART 7 - NEXT DAY PREP (5 min)

```
-> 1-4-7 Tracker update kiya?
-> Aaj ka topic Tracker mein add kiya?
-> Koi doubt bacha? Seedha poocho!
-> Kal ka topic: [bataunga]
```

---

## NOTES FILE FORMAT - HAR DIN

```markdown
# Day XX - Topic Name

## Problem
[Kyun zaroori hai ye service]

## Solution + Concept
[Kya karta hai - clearly]

## Analogy
[Real life example]

## Architecture
[Flow diagram text mein]

## Hands-On - Aaj Kya Kiya
[Step by step jo kiya]

## Interview Questions & Answers
[Minimum 5 Q&A in English]

## Key Points - Phone Pe Save Karo
[5-7 one-liners]
```

---

## GOLDEN RULES - KABHI MAT BHOOLNA

```
1. Revision pehle - naya topic baad mein
2. Har concept hands-on karna zaroori hai
3. Notes banana - GitHub pe push karna
4. Interview questions English mein
5. Koi bhi shortcut nahi - poora samajhna hai
6. Roz 1 ghanta - no excuse, no skip
7. Jo nahi samjha - turant poocho, aage mat badho
```

---

## SALARY PROGRESSION

```
Abhi:    Developer salary
Month 4: 15-22 LPA (AWS SAA Certification)
Month 7: 25-35 LPA (AWS SAP Certification)
Year 2:  35-50 LPA (Experience + Projects)
```

---

*Template Version: July 2026*
*Mentor: Claude AI*
*"Zero se Best tak - sahi neenv ke saath"*
