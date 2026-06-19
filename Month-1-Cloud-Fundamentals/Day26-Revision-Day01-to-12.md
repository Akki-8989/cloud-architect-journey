# Day 26 — Revision Notes (Day 01 to Day 12)

## Revision Method: Option B (Direct Recall — No Notes)
> Struggle kiya → Brain mein deep set hua → Permanent memory!

---

## Day 01 — Cloud Computing Basics

### Cloud Computing Kya Hai
```
Internet pe ON-DEMAND computing resources —
servers, storage, networking — pay-as-you-go!

Apna server khareedna nahi → AWS se rent pe lo ✅
```

### 3 Types
```
IaaS = Infrastructure as a Service
       → EC2 (tum server manage karo)
       → Maximum control, maximum responsibility

PaaS = Platform as a Service
       → Elastic Beanstalk (AWS deploy manage kare)
       → Tum sirf code likho

SaaS = Software as a Service
       → Gmail, Zoom (sirf use karo)
       → Kuch manage nahi karna
```

---

## Day 02 — EC2 (Elastic Compute Cloud)

### EC2 Kya Hai
```
EC2 = AWS ka Virtual Machine (Cloud mein rent pe liya computer)
VM  = Virtual Machine = Virtual Server = EC2 Instance (sab ek hi!)

AWS manage karta hai → Physical hardware (Data Center, electricity, cooling)
Tum manage karte ho → OS, software, security, application
```

### EC2 = Khali Computer
```
EC2 alone    = Sirf ek blank computer
EC2 + Nginx  = Web Server ✅
EC2 + MySQL  = Database Server ✅
EC2 + Java   = Application Server ✅
```

### Instance Types
```
t3 = Burstable — Testing, small apps (Free tier: t3.micro)
m5 = General Purpose — Balanced CPU/RAM
c5 = Compute Optimized — CPU heavy (video processing)
r5 = Memory Optimized — RAM heavy (databases)
p3 = GPU — AI/ML workloads
```

---

## Day 03 — S3 (Simple Storage Service)

### S3 Kya Hai
```
S3 = Object Storage
→ Images, videos, files, backups store karo
→ Unlimited storage
→ 99.999999999% durability (11 nines!)
→ Internet se accessible
```

### Storage Classes
```
S3 Standard          = Roz ka use (costly)
S3 Standard-IA       = Mahine mein 1-2 baar (Infrequent Access)
S3 Glacier           = Saal mein 1-2 baar (archive - cheap)
S3 Glacier Deep      = Almost kabhi nahi (sabse cheap)
S3 Intelligent-Tier  = AWS khud decide kare
```

---

## Day 04 — RDS (Relational Database Service)

### RDS Kya Hai
```
RDS = AWS ka Managed Relational Database
→ MySQL, PostgreSQL, Oracle, SQL Server support karta hai
→ AWS manage karta hai: backups, patches, scaling
→ Tum sirf data ke saath kaam karo!
```

### EC2 DB vs RDS
```
EC2 pe DB:
→ Tum DBA bhi ho + Developer bhi
→ Backup tum karo, patch tum karo ❌

RDS:
→ Sirf developer bano
→ AWS DBA ka kaam kare ✅
→ Automatic backups, Multi-AZ, Read Replicas
```

---

## Day 05 — IAM (Identity and Access Management)

### IAM Kya Hai
```
IAM = AWS ka Security System
"Kaun kya kar sakta hai" — ye IAM decide karta hai
```

### Users, Groups, Roles
```
USER  = Ek insaan
        → Login karta hai username/password se
        → Example: akash-dev-user

GROUP = Logon ka folder/dabba
        → "Developer Group" = 5 developers
        → Group ko permission do → Sab ko mil gayi ✅
        → Alag alag dene ki zaroorat nahi!

ROLE  = Service ka temporary badge
        → Kisi insaan ko nahi — SERVICE ko dete hain!
        → Example: Lambda ko S3 access chahiye → Role do
        → Password nahi hota — temporary access!
```

### Real Example
```
Company = Swiggy

Users  → Akash, Rahul, Priya
Group  → "Backend-Dev-Group" → S3 + RDS access
Role   → Lambda function → S3 read access

Akash ne company chodi → Group se hata → Access gaya! ✅
```

---

## Day 06 — VPC (Virtual Private Cloud)

### VPC Kya Hai
```
VPC = AWS mein tumhara apna private network!

Internet = Public city (sab aa ja sakte hain)
VPC      = City mein tumhara private plot
           → Sirf tum control karte ho ✅
```

### Public vs Private Subnet
```
VPC ke andar:
├── Public Subnet  → Internet se accessible
│   └── EC2 (Web Server) — Users yahan aate hain
│
└── Private Subnet → Internet se hidden!
    └── RDS (Database) — Sirf web server access kare
        → Bahar se koi directly nahi pahunch sakta ✅
```

### Traffic Flow
```
User (Browser)
    ↓
Internet
    ↓
Internet Gateway (VPC ka darwaza)
    ↓
Security Group (firewall — port 80/443 allow?)
    ↓
EC2 (Web Server)
    ↓
Response wapas user ko! ✅
```

### Security Group
```
Port 80  (HTTP)  = Allow ✅ (website)
Port 443 (HTTPS) = Allow ✅ (secure website)
Port 22  (SSH)   = Sirf tumhara IP ✅
Baki sab         = Block ❌
```

---

## Day 08 — EBS (Elastic Block Storage)

### EBS Kya Hai
```
EC2 = Computer (CPU + RAM)
EBS = Hard Drive (Storage) — alag se attach karo!

EC2 band kiya → RAM clear (temporary)
EBS band kiya → Data safe! (permanent) ✅
```

### Important Feature
```
EC2 delete kiya → EBS still exists!
→ Data safe ✅
→ Doosre EC2 se attach kar lo ✅

Jaise:
Laptop kharaab hua → Hard drive nikali
Naye laptop mein lagayi → Data wapas! ✅
```

---

## Day 11 — CloudWatch

### CloudWatch Kya Hai
```
CloudWatch = Doctor ka monitor
→ Patient (EC2/RDS) ki pulse check karta hai
→ Problem aai → Alarm bajata hai!

AWS Budgets = Accountant (sirf paisa track!)
CloudWatch  = Health monitor (CPU, RAM, Logs)
```

### 3 Main Features
```
1. METRICS  → Numbers track (CPU%, RAM%, Network)
2. ALARMS   → Threshold cross kiya → Alert bhejo!
3. LOGS     → Sab kuch record karo (errors, events)
```

---

## Day 12 — SNS + SQS

### SNS — Simple Notification Service
```
SNS = Broadcast (1 → Sabko ek saath!)

Example:
→ Server down alert → Email + SMS + Slack ek saath ✅

Jaise WhatsApp Group message ✅
```

### SQS — Simple Queue Service
```
SQS = Line mein lagao — ek ek karke process karo!

Example:
→ 1000 orders ek saath aaye
→ SQS queue mein line → Ek ek process ✅
→ Koi order miss nahi! ✅

Jaise Bank ki line ✅
```

### Difference
```
SNS = WhatsApp group (1 → Many, ek saath)
SQS = Bank ki line  (1 → 1, order mein)
```

---

## Day 13 — Lambda

### Lambda Kya Hai
```
Lambda = Serverless Computing
→ Koi server manage nahi karna!
→ Sirf tab chalta hai jab kaam ho

EC2     = Full time employee (salary 24/7) ❌
Lambda  = Freelancer (sirf kaam ka paisa) ✅
```

### Kaise Kaam Karta Hai
```
EVENT → LAMBDA TRIGGER → CODE CHALA → BAND

Example:
→ User ne photo upload ki S3 pe (EVENT)
→ Lambda chala → Photo resize ki → Band!
→ Sirf us 2 second ka bill! ✅
```

### EC2 vs Lambda
```
EC2     → 24/7 running, fixed cost, tum manage karo
Lambda  → Event pe chale, pay per use, AWS manage kare
         Max 15 minutes run kar sakta hai!
```

---

## Day 15 — Route53

### Route53 Kya Hai
```
Route53 = AWS ka DNS Service

DNS = Internet ki phone book!
→ "zomato.com" → "192.168.1.100" (IP address)
→ Computer sirf numbers samjhta hai!
```

### Route53 Kya Karta Hai
```
1. Domain Registration  → "zomato.com" kharido
2. DNS Routing          → Domain → EC2 IP pe bhejo
3. Health Checks        → Server down? → Traffic shift karo!
4. Latency Routing      → Mumbai user → Mumbai server (fast!)
```

---

## Day 14 — CloudFront

### CloudFront Kya Hai
```
CloudFront = CDN (Content Delivery Network)
→ Content user ke paas le jao → Speed! ✅

Problem:
→ Zomato server Mumbai mein
→ London user → 300ms delay ❌

Solution:
→ CloudFront = 400+ Edge Locations worldwide
→ London mein bhi copy rakho
→ London user → London se mile → 10ms ✅
```

### Route53 vs CloudFront
```
Route53     = Phone book (domain → IP translate karo)
CloudFront  = Local delivery hub (content paas mein rakho)

Dono saath:
User → Route53 → CloudFront (nearest) → Content! ✅
```

---

## Quick Revision Table — Sab Ek Jagah

| Service | Kya Hai | Ek Line |
|---------|---------|---------|
| EC2 | Virtual Machine | Cloud mein rent ka computer |
| S3 | Object Storage | Files/images store karo |
| RDS | Managed Database | AWS managed SQL database |
| IAM | Access Control | Kaun kya kar sakta hai |
| VPC | Private Network | AWS mein apna network |
| EBS | Block Storage | EC2 ka hard drive |
| CloudWatch | Monitoring | Server ki health check |
| SNS | Notification | 1 → Many broadcast |
| SQS | Queue | Line mein process karo |
| Lambda | Serverless | Event pe chalo, pay per use |
| Route53 | DNS | Domain → IP translate |
| CloudFront | CDN | Content user ke paas |
