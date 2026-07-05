# Day 33 — Route 53 (DNS + Domain Management)

## Route 53 Kya Hai

```
Route 53 = AWS ka DNS Service

DNS = Domain Name System = Internet ka Phone Book!

User ne type kiya: www.zomato.com
Browser ko chahiye: IP Address (13.235.45.67)

Route 53 ka kaam:
www.zomato.com → Route 53 → 13.235.45.67

Jaise:
"Akash ka number?" → Phone book → 9876543210
```

---

## Hosted Zone — Route 53 ka Core

```
Domain    = Ghar ka address (akash-demo.com)
Hosted Zone = Us domain ki directory/file

Hosted Zone mein hota hai:
→ www      → 1.2.3.4    (website)
→ mail     → 5.6.7.8    (email server)
→ api      → 9.10.11.12 (API server)

Ek domain = Ek hosted zone!

2 Types:
Public Hosted Zone  → Internet pe traffic route karo
Private Hosted Zone → Sirf VPC ke andar traffic route karo
```

### Analogy
```
Route 53    = City ka post office
Hosted Zone = Tumhare mohalle ki address book
Records     = Har ghar ka specific address

"Akash ka ghar kahan?"
→ Post office → Mohalla book → "3rd lane, 2nd ghar" ✅
```

---

## DNS Record Types

```
A Record     → Domain → IPv4 address
               www.site.com → 1.2.3.4

AAAA Record  → Domain → IPv6 address

CNAME Record → Domain → Doosra Domain
               blog.site.com → site.wordpress.com

MX Record    → Email server ka address
               site.com ka email → mail.site.com

NS Record    → Name Servers (Route 53 ke DNS servers)
               Automatically banta hai!

SOA Record   → Zone ka master record (info)
               Automatically banta hai!
```

---

## Routing Policies — Traffic Kahan Bhejo

### 1. Simple Routing
```
Sabse basic!
Ek domain → Ek server

www.myapp.com → 13.235.45.67

Kab use karein: Sirf ek server hai
Problem: Server down → Site down! ❌
```

### 2. Weighted Routing
```
Traffic % ke hisaab se baanto!

www.myapp.com → Server A (70% traffic)
www.myapp.com → Server B (30% traffic)

Kab use karein:
→ Naya feature test karna (Blue/Green deployment)
→ 10% users ko new version, 90% ko old

Real example — Zomato:
New UI → 10% test → Sab theek → 100% deploy ✅
```

### 3. Latency Routing
```
User ke PAAS wala server use karo!

India user  → Mumbai server  → 5ms ✅
US user     → New York server → 3ms ✅

Automatic sabse fast server choose karta hai!
```

### 4. Failover Routing
```
Primary down → Backup pe automatic switch!

Normal: Traffic → Mumbai (Primary) ✅
Down:   Traffic → Singapore (Secondary) ✅

Trade-off:
→ Failover = Slow ho sakta hai (Singapore far hai)
→ Lekin Slow > DOWN!
→ Available > Fast (emergency mein) ✅

Kab use karein: Disaster Recovery, High Availability
```

### 5. Geolocation Routing
```
User KAHAN SE hai us hisaab se bhejo!

India se user  → India server
US se user     → US server
Europe se user → Europe server

Kab use karein:
→ Regional content (default language set karo)
→ Legal compliance (GDPR — Europe data Europe mein)
→ Pricing difference by region

Important:
→ Route 53 sirf ROUTING karta hai (kahan bhejo)
→ Content kya dikhao = Application decide karta hai
→ India server pe English bhi possible hai! ✅
```

### Sab Policies — Ek Saath
```
Simple      = Ek server → Basic use
Weighted    = 70/30 split → Testing/Deployment
Latency     = Nearest server → Speed optimize
Failover    = Primary down → Backup → HA
Geolocation = User location → Regional content
```

---

## Health Checks

```
Route 53 automatically check karta hai:
"Server alive hai?"

Healthy   → Traffic bhejo ✅
Unhealthy → Traffic mat bhejo ❌ (Failover trigger!)

Failover Routing ke saath:
Primary unhealthy → Automatically Secondary pe switch ✅
```

---

## Hands-On — Aaj Kya Kiya

### Step 1 — Hosted Zone Banaya
```
Route 53 → Hosted zones → Create hosted zone

Settings:
→ Domain name: akash-demo.com
→ Type: Public hosted zone

Result:
→ akash-demo.com hosted zone created ✅

Automatically 2 records bane:
→ NS Record  : 4 AWS Name Servers (Route 53 ke DNS)
→ SOA Record : Zone ki master information
```

### Step 2 — A Record Banaya
```
Create record:
→ Record name : www
→ Record type : A
→ Value       : 1.2.3.4 (demo IP)
→ Routing     : Simple
→ TTL         : 300

Result: www.akash-demo.com → 1.2.3.4 ✅

Matlab: Agar real domain hota aur real IP hota
        → User browser → www.akash-demo.com type kare
        → Route 53 → 1.2.3.4 pe bhej deta!
```

### Cleanup
```
✅ A Record deleted
✅ Hosted Zone deleted
→ Bill zero! ✅
```

---

## Interview Questions & Answers

**Q1. What is Amazon Route 53 and what are its main features?**

Amazon Route 53 is a highly available and scalable cloud Domain Name System service provided by AWS. It translates human-readable domain names like www.example.com into machine-readable IP addresses like 192.0.2.1. Route 53 offers three main features: domain registration where you can purchase and manage domain names, DNS routing where it routes internet traffic to your application resources using various routing policies, and health checking where it monitors the health of your resources and routes traffic away from unhealthy endpoints.

---

**Q2. What is a Hosted Zone in Route 53?**

A hosted zone is a container in Route 53 that holds information about how to route traffic for a specific domain and its subdomains. Think of it like an address book for your domain — it contains DNS records that map domain names to IP addresses or other resources. There are two types: a public hosted zone that routes traffic on the internet, and a private hosted zone that routes traffic within an Amazon VPC. Each domain has its own hosted zone, and Route 53 automatically creates NS and SOA records when a hosted zone is created.

---

**Q3. Explain the different routing policies in Route 53.**

Route 53 offers five main routing policies. Simple routing directs traffic to a single resource and is used when you have a single server. Weighted routing distributes traffic across multiple resources based on assigned weights, which is useful for testing new versions by sending a small percentage of traffic to the new version. Latency routing sends users to the AWS region that provides the lowest latency, improving performance. Failover routing uses health checks to automatically route traffic to a secondary resource when the primary resource becomes unavailable, providing disaster recovery. Geolocation routing directs users to different resources based on their geographic location, which is useful for compliance requirements like GDPR or serving region-specific content.

---

**Q4. What is the difference between Failover and Latency routing?**

Latency routing optimizes for speed by directing users to the nearest or fastest server during normal operations. Failover routing optimizes for availability by switching to a backup server when the primary server becomes unhealthy. These two serve different purposes — latency routing is used for performance optimization while failover routing is used for disaster recovery. In a production architecture, both can be combined — latency routing handles normal traffic while failover ensures the application remains available during outages. An important trade-off with failover is that the backup server may be geographically farther away, resulting in higher latency, but availability is prioritized over speed in disaster scenarios.

---

## Key Points — Phone Pe Save Karo

```
Route 53 = AWS DNS Service (Internet ka Phone Book)

Hosted Zone:
→ Domain ki saari records ki container
→ Public = Internet routing
→ Private = VPC internal routing
→ Automatically NS + SOA record banta hai

Record Types:
→ A     = Domain → IPv4
→ CNAME = Domain → Domain
→ MX    = Email server
→ NS    = Name servers

Routing Policies:
→ Simple     = 1 server (basic)
→ Weighted   = % split (A/B testing)
→ Latency    = Nearest server (speed)
→ Failover   = Primary down → Backup (HA)
→ Geolocation = User location → Region

TTL = Time To Live
    = DNS cache kitni der tak valid hai (seconds)
    = 300 = 5 minutes
```
