# Day 27 — Revision Notes (Day 13 to Day 25)

## Revision Method: Explain → Stuck → Samjhao → Clear!

---

## Day 17 — DynamoDB

### DynamoDB vs RDS
```
RDS = SQL Database (structured — Excel sheet jaisa)
→ Fixed columns, relations between tables
→ Complex queries possible
→ Banking, orders ke liye

DynamoDB = NoSQL Database (flexible — JSON jaisa)
→ Har record alag structure ho sakta hai
→ Milliseconds mein response
→ Gaming, social media, real-time apps ke liye
```

### Real Example — PUBG
```
{
  "PlayerID": "Akash123",
  "Kills": 150,
  "Level": 45,
  "Weapons": ["AKM", "M416"]
}
→ Crore players ka data → Milliseconds mein fetch! ✅
RDS se karte → Complex joins → Slow → Game lag! ❌
```

### Ek Line Trick
```
RDS      = Excel sheet (structured, relations)
DynamoDB = WhatsApp message (flexible, super fast)
```

---

## Day 18 — ElastiCache

### ElastiCache Kya Hai
```
ElastiCache = In-Memory Cache
→ Baar baar same DB query mat karo
→ Result cache mein rakho → Fast do!

Cache Miss: Data cache mein nahi → DB se laya → Cache mein rakha
Cache Hit:  Data cache mein mila → Direct do! (milliseconds) ✅
```

### Flow
```
Bina Cache:
User → Request → RDS (har baar) → Slow + DB overload! ❌

Cache ke saath:
1st: User → Cache Miss → RDS → Cache mein rakha ✅
2nd: User → Cache Hit → ElastiCache → Fast! ✅
```

### Redis vs Memcached
```
Memcached = Simple cache (sirf store/fetch)
Redis     = Smart cache (store/fetch + sort + expire + sessions)

Redis use cases:
→ PUBG Leaderboard (sorting) ✅
→ OTP store (5 min expiry) ✅
→ Shopping cart ✅
→ User sessions ✅

Production mein 90% → Redis use hota hai!
```

---

## Day 19 — CloudFormation

### CloudFormation Kya Hai
```
CloudFormation = Infrastructure as Code (IaC)
→ Click click manually resources banana nahi
→ YAML/JSON file likho → Sab automatically bane! ✅
```

### Problem vs Solution
```
Manually:
→ EC2, RDS, VPC, Security Groups — sab click click
→ 2-3 ghante + mistakes possible ❌

CloudFormation:
→ Ek file likho → 10 minutes mein sab ready! ✅
→ Dev, Staging, Production → Same file → Same setup ✅
```

### Important Terms
```
Template = YAML/JSON file (kya banana hai)
Stack    = Jo actually bana (EC2, RDS, VPC)

Stack delete kiya → Sab resources delete! ✅
Cleanup easy!
```

### Developer Analogy
```
Code GitHub pe → Koi bhi pull karo → Same app ✅
CloudFormation = Infrastructure ka GitHub! ✅
```

---

## Day 20 — ECS + Docker

### Docker Kya Hai
```
Problem:
→ "Mere machine pe toh kaam karta tha!" 😤
→ Different environment → Different result ❌

Solution = Docker:
→ App + Dependencies + Settings → Ek box mein band karo!
→ Kahi bhi le jao → Same result! ✅
```

### Docker Analogy
```
Bina Docker = Recipe batao (sab alag banayenge) ❌
Docker      = Tiffin box bhejo (exact same khana!) ✅
```

### ECS Kya Hai
```
ECS = AWS ka Container Management Service

Docker = Ek container banana
ECS    = 1000 containers manage karna

ECS karta hai:
→ Container crash → Naya launch ✅
→ Load zyada → Zyada containers ✅
→ Load kam → Containers band ✅
```

### Ek Line
```
Docker = App ko box mein band karo
ECS    = Hazaron boxes AWS pe manage karo
```

---

## Day 21 — VPC Advanced (NAT Gateway)

### NAT Gateway Kya Hai
```
Private Subnet mein server hai:
→ Internet se koi andar nahi aa sakta ✅ (secure!)
→ Lekin server ko update karna hai → Bahar bhi nahi ja sakta ❌

NAT Gateway = Private subnet ka one-way darwaza

Private EC2 → NAT Gateway → Internet (bahar ja sakta!) ✅
Internet    → NAT Gateway → Private EC2 (BLOCK!) ✅

Sirf OUTBOUND traffic allow! INBOUND block! ✅
```

### Analogy
```
NAT Gateway = One-way mirror
Andar se bahar dekh sakte ho ✅
Bahar se andar nahi dekh sakte ❌
```

### Internet Gateway vs NAT Gateway
```
Internet Gateway = Public subnet (2-way traffic)
NAT Gateway      = Private subnet (sirf outbound!)
```

---

## Day 22 — Cost Management

### AWS Budgets vs Cost Explorer
```
AWS Budgets   = Alarm system
→ "Rs. 5000 se zyada hua → Email bhejo!"
→ Proactive — pehle warn karta hai ✅

Cost Explorer = Analysis tool
→ "Pichle 6 mahine mein paisa kahan gaya?"
→ Graphs + Service wise breakdown
→ Reactive — baad mein analyse karo ✅
```

### Analogy
```
AWS Budgets   = Guard (limit cross → alert!)
Cost Explorer = Doctor (baad mein analyse karo!)
```

### Dono Saath Use Karo
```
Month start → Budget set karo (Budgets)
Month end   → Analyse karo (Cost Explorer) → Next month improve!
```

---

## Day 23 — Well-Architected Framework

### 6 Pillars — CORSPS
```
C → Cost Optimization      = Smart spend karo
O → Operational Excellence = Automate karo, sahi operate karo
R → Reliability            = Hamesha available raho
S → Security               = Data protect karo
P → Performance Efficiency = Fast aur efficient raho
S → Sustainability         = Environment ka dhyan rakho
```

### Cloud Architect Ki Checklist
```
"Maine paisa bachaya?"          → Cost ✅
"Sab automate hai?"             → Operational ✅
"Crash hua toh kya hoga?"       → Reliability ✅
"Secure hai?"                   → Security ✅
"Fast hai?"                     → Performance ✅
"Environment friendly hai?"     → Sustainability ✅
```

### RTO vs RPO
```
RTO = Recovery Time Objective
      "Kitne time mein wapas online?"
      RTO 4 hours → 4 ghante mein restore hona chahiye

RPO = Recovery Point Objective
      "Kitna data lose kar sakte hain?"
      RPO 1 hour → Har ghante backup lo
```

---

## Day 24 — KMS + Secrets Manager

### KMS — Key Management Service
```
KMS = Data ka taala (encrypt karo)
→ S3, RDS, EBS sab encrypt karo
→ Hacker ne data liya → Locked → Kuch kaam nahi! ✅

Key Types:
AWS Managed    = Free, AWS manage kare
Customer (CMK) = $1/month, tum manage karo (Production!) ✅
```

### Secrets Manager
```
Secrets Manager = Password ki tijori

Galat: Code mein password likho → GitHub → Hack! ❌
Sahi:  Secrets Manager mein rakho → Code mein sirf reference ✅

Best feature = Auto Rotation!
→ RDS password har 30 din mein change
→ App automatically naya le leti hai
→ Zero downtime! ✅
```

### KMS vs Secrets Manager
```
KMS             = Data encrypt karo (taala lagao)
Secrets Manager = Credentials store karo (tijori mein rakho)

Dono saath:
Secrets Manager mein password → KMS se encrypted! ✅
```

---

## Day 25 — AWS Organizations

### Management Account
```
Organization ka BOSS account
→ Sab accounts yahan se manage
→ Consolidated billing — ek bill ✅
→ Is account mein actual kaam mat karo!
```

### OU — Organizational Unit
```
OU = Accounts ka folder

Production OU → Prod accounts
Dev OU        → Dev accounts
Security OU   → Security accounts

OU pe policy lagao → Sab accounts pe apply! ✅
```

### SCP — Service Control Policy
```
SCP = Unbreakable rules!
→ Admin ke paas bhi SCP override nahi hoti
→ SCP > IAM — SCP hamesha wins!

Examples:
→ Dev OU mein GPU instances BLOCK!
→ Sirf Mumbai region allowed!
→ Production mein delete BLOCK!
```

### Blast Radius
```
Bina Organizations:
→ Ek account hack → Poori company risk! ❌

Organizations ke saath:
→ Dev account hack → Sirf Dev affected
→ Production safe! ✅

Multi-Account = Blast Radius kam! ✅
```

---

## Quick Revision Table — Day 13-25

| Service | Kya Hai | Ek Line |
|---------|---------|---------|
| DynamoDB | NoSQL Database | Flexible + Super fast (PUBG!) |
| ElastiCache | In-Memory Cache | DB hit kam karo → Fast! |
| Redis | Advanced Cache | Sort + Expire + Sessions |
| Memcached | Simple Cache | Sirf store/fetch |
| CloudFormation | IaC | Code se infrastructure banao |
| Docker | Containerization | App ko box mein band karo |
| ECS | Container Manager | 1000 containers manage karo |
| NAT Gateway | Outbound only | Private subnet → Internet (one-way) |
| AWS Budgets | Alert System | Limit cross → Alert! |
| Cost Explorer | Analysis | Paisa kahan gaya? Graph! |
| Well-Architected | Best Practices | CORSPS — 6 pillars |
| KMS | Encryption | Data ka taala |
| Secrets Manager | Credential Store | Password tijori + Auto rotate |
| Organizations | Multi-Account | Blast radius kam karo |
| SCP | Org-level Rules | IAM se bhi powerful! |
