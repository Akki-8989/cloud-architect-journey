# Day 13 — Route 53 (DNS + Domain Management)

## DNS Kya Hai — Problem Samjho

Computer sirf IP addresses samajhta hai jaise `54.239.28.85`. But hum insaan `google.com` type karte hain — numbers yaad nahi rakhte.

**DNS = Internet ka Phone Book**

```
Tumhare phone mein contacts hain:
"Rahul" → 9876543210

DNS bhi wahi karta hai:
"google.com" → 142.250.195.46
```

Jab tum browser mein `google.com` type karte ho:
```
Browser → DNS Server se puchha → "google.com ka IP kya hai?"
DNS     → "142.250.195.46" → Browser us IP pe connect ho gaya
```

---

## Route 53 Kya Hai

Route 53 AWS ka **Smart DNS Service** hai. Normal DNS se better kyun hai:

```
Normal DNS   →  Sirf IP return karta hai — bas!

Route 53     →  Smart DNS:
  Health Check   → Server down? Automatically doosre pe bhejo
  Geo Routing    → India users → India server, US users → US server
  Failover       → Primary down → Secondary automatically on
  AWS Integration→ Direct ALB, EC2, S3 se connect
  100% Uptime    → Kabhi down nahi hoga
```

**Developer Analogy:** Normal DNS simple phone book hai. Route 53 smart call router hai — "Rahul busy hai? Automatically Rohan pe forward karo. Rohan bhi nahi? Message chhod do."

---

## Route 53 Ke 4 Main Concepts

### 1. Hosted Zone — Domain Ka Folder
Hosted Zone ek container hai jisme tumhare domain ke saare DNS records store hote hain.

```
myawslearning.com  →  Hosted Zone
                         ├── NS Record
                         ├── SOA Record
                         ├── A Record (www)
                         └── CNAME Record (blog)
```

**Public Hosted Zone** = Internet pe accessible (websites ke liye)
**Private Hosted Zone** = Sirf AWS VPC ke andar accessible (internal services ke liye)

---

### 2. Record Types — DNS Rules

```
A Record     → Domain/Subdomain ko IP Address se link karta hai
               www.myawslearning.com → 54.239.28.85
               Seedha server ka IP batata hai

CNAME Record → Domain ko doosre Domain se link karta hai
               blog.myawslearning.com → www.myawslearning.com
               Ek naam doosre naam pe point karta hai
               Ek jagah IP change karo → sab update

NS Record    → Name Server record — ye batata hai ki is domain ka
               DNS kaun handle kar raha hai (Route 53 ke servers)
               Automatically create hota hai Hosted Zone banane pe

SOA Record   → Start of Authority — domain ki basic information
               Automatically create hota hai Hosted Zone banane pe

Alias Record → Domain ko AWS resource se link karta hai
               myawslearning.com → my-alb-123.ap-south-1.elb.amazonaws.com
               ALB, CloudFront, S3 website ke liye use hota hai
```

---

### 3. Routing Policies — Traffic Kaise Route Karein

```
Simple      → Ek domain, ek server — basic use case
              example.com → 54.239.28.85

Weighted    → Traffic split karo percentage se
              Server A → 70% traffic (new version)
              Server B → 30% traffic (old version)
              A/B testing ke liye perfect

Geolocation → User kahan se aa raha hai us hisaab se route karo
              India users    → Mumbai server (fast experience)
              US users       → Virginia server

Failover    → Primary server down? Automatic Secondary pe
              Primary: main-server.com (healthy)
              Secondary: backup-server.com (standby)
              Primary down → Route 53 automatically Secondary pe bheja

Latency     → Sabse fast server pe bhejo
              Route 53 check karta hai — kaun sa server
              user ke liye fastest hai — wahan bhejo
```

---

### 4. Health Checks — Server Ki Sehat Monitor Karo

Route 53 automatically har 30 seconds pe server pe request bhejta hai.

```
Route 53 → GET / → server IP → Response aaya (200 OK) → Healthy ✅
Route 53 → GET / → server IP → No Response               → Unhealthy ✗
                                                              ↓
                                               Failover routing trigger
                                               Traffic doosre server pe
```

**Real use case:** Production mein Primary server down hua → Health check failed → Route 53 automatically Secondary server pe traffic bhej deta hai → Users ko kuch pata nahi chalta!

---

## TTL — Time To Live

TTL batata hai ki DNS response kitne time tak **cache** rahega.

```
TTL = 300 seconds (5 minutes)
→ Browser ne DNS se IP puchha → 54.239.28.85 mila
→ Ye IP 5 minute tak browser cache mein rahega
→ 5 minute tak dobara DNS se nahi puchhega

TTL kam  = Zyada DNS queries, fresh results, IP change jaldi reflect hoga
TTL zyada = Kam DNS queries (fast), IP change reflect hone mein time lagega
```

---

## Hands-On — Aaj Kya Kiya

### Step 1 — Route 53 Console Khola
```
AWS Console → Search → Route 53 → Open
```
**Kyun:** Route 53 Global service hai — region select nahi karna padta.

### Step 2 — Hosted Zone Banaya
```
Route 53 → Hosted zones → Create hosted zone

Domain name : myawslearning.com
Type        : Public hosted zone
→ Create hosted zone
```
**Kyun:** Domain ke saare DNS records store karne ke liye container chahiye tha.

**Result:** Automatically 2 records bane:
```
NS  Record → 4 AWS Name Servers assign hue
             ns-1592.awsdns-07.co.uk
             ns-552.awsdns-05.net
             ns-1174.awsdns-18.org
             ns-421.awsdns-52.com
             (Real domain pe ye 4 servers domain registrar mein enter karte hain)

SOA Record → Domain authority information automatically create hua
```

### Step 3 — A Record Banaya
```
Route 53 → myawslearning.com → Create record

Record name   : www
Record type   : A
Value         : 54.239.28.85
TTL           : 300
Routing policy: Simple
→ Create records
```
**Kyun:** `www.myawslearning.com` ko ek server ke IP address se link kiya.
**Flow:** User `www.myawslearning.com` type kare → DNS A record dekhe → `54.239.28.85` pe connect kare.

### Step 4 — CNAME Record Banaya
```
Record name   : blog
Record type   : CNAME
Value         : www.myawslearning.com
TTL           : 300
Routing policy: Simple
→ Create records
```
**Kyun:** `blog.myawslearning.com` ko alag IP yaad nahi rakhna — seedha `www` pe point kar diya.
**Flow:** User `blog.myawslearning.com` type kare → CNAME → `www.myawslearning.com` → A Record → `54.239.28.85`.

### Step 5 — Health Check Banaya
```
Route 53 → Health checks → Create health check

Name              : my-web-health-check
Monitor           : Endpoint
Specify by        : IP address
Protocol          : HTTP
IP address        : 54.239.28.85
Port              : 80
Path              : /
→ Create health check
```
**Kyun:** Route 53 har 30 seconds pe server check kare. Real IP nahi thi isliye Unhealthy ho gayi — ye expected tha.
**Production mein:** Real server down hua → Unhealthy → Failover routing → Traffic backup server pe.

### Step 6 — Cleanup (Delete Order Important Hai!)
```
1. Health check delete karo pehle
2. A Record aur CNAME Record delete karo (andar se)
3. Hosted Zone delete karo
```
**Kyun ye order:** Hosted Zone directly delete nahi hoti agar andar custom records hain — pehle records hatao phir zone.

---

## Real World Architecture — Route 53 + ALB

```
User types: www.myawslearning.com
      ↓
Route 53 (DNS)
      ↓
Alias Record → my-alb-123.ap-south-1.elb.amazonaws.com
      ↓
ALB (Load Balancer)
      ↓
Auto Scaling Group
  [EC2-1]  [EC2-2]  [EC2-3]
```

Route 53 + ALB + ASG = Complete Production Architecture!

---

## Interview Questions & Answers

**Q1. What is DNS and why is it needed?**

DNS stands for Domain Name System. It is essentially the internet's phone book. Computers communicate using IP addresses like 54.239.28.85, but humans cannot memorize numbers for every website they visit. DNS solves this by mapping human-readable domain names like google.com to their corresponding IP addresses. When a user types a domain name in their browser, the browser queries a DNS server to resolve the domain to an IP address, then connects to that IP address to load the website.

---

**Q2. What is Amazon Route 53 and how is it different from regular DNS?**

Amazon Route 53 is AWS's highly available and scalable cloud DNS service. Unlike a regular DNS service that simply maps domain names to IP addresses, Route 53 is a smart DNS with additional capabilities. It performs health checks on endpoints and can automatically route traffic away from unhealthy resources. It supports multiple routing policies including geolocation routing to direct users to the nearest server, weighted routing for A/B testing, and failover routing for disaster recovery. It also integrates natively with other AWS services through Alias records, allowing you to point domains directly to load balancers, CloudFront distributions, and S3 buckets without needing their IP addresses.

---

**Q3. What is a Hosted Zone in Route 53?**

A Hosted Zone is a container in Route 53 that holds all the DNS records for a specific domain. When you create a Hosted Zone for a domain like myawslearning.com, Route 53 automatically creates two records: an NS record containing the four AWS name servers that will handle DNS queries for your domain, and an SOA record with administrative information about the zone. You then add your own records like A records and CNAME records inside the Hosted Zone. There are two types: a Public Hosted Zone for domains accessible on the internet, and a Private Hosted Zone for domains accessible only within an AWS VPC.

---

**Q4. What is the difference between an A Record, CNAME Record, and Alias Record?**

An A Record maps a domain or subdomain directly to an IPv4 address. For example, www.myawslearning.com pointing to 54.239.28.85. A CNAME Record maps a domain to another domain name rather than an IP address. For example, blog.myawslearning.com pointing to www.myawslearning.com. This is useful when you want multiple subdomains to point to the same place without duplicating IP addresses. An Alias Record is AWS-specific and maps a domain to an AWS resource like an Application Load Balancer or CloudFront distribution. Unlike a CNAME, an Alias Record can be used for the root domain (like myawslearning.com directly, not just subdomains), and it does not incur additional DNS query charges.

---

**Q5. What is TTL in DNS and how does it affect your application?**

TTL stands for Time To Live. It is a value in seconds that tells DNS resolvers how long to cache a DNS response before querying again. For example, a TTL of 300 means the resolved IP address will be cached for 5 minutes. A low TTL means DNS changes propagate quickly across the internet, which is important during deployments or failovers when you need to change your server's IP address rapidly. However, a low TTL results in more frequent DNS queries. A high TTL reduces DNS query load and improves performance, but means IP address changes take longer to reflect for all users. Best practice is to lower the TTL before making a DNS change, wait for it to propagate, make the change, then raise the TTL again.

---

**Q6. What is Route 53 Health Check and how does it enable high availability?**

Route 53 Health Check is a monitoring feature that automatically sends requests to your endpoints at regular intervals, typically every 30 seconds, to verify they are responding correctly. If an endpoint fails to respond or returns an error, Route 53 marks it as unhealthy. When combined with Failover routing policy, Route 53 automatically stops sending traffic to the unhealthy primary endpoint and redirects all traffic to a designated secondary backup endpoint. This happens automatically without any manual intervention, ensuring high availability. The moment the primary endpoint recovers and starts passing health checks again, Route 53 automatically resumes sending traffic to it.

---

## Key Points — Phone Pe Save Karo

```
DNS          = Internet ka phone book — domain → IP
Route 53     = AWS ka smart DNS service
Hosted Zone  = Domain ke saare records ka container
NS Record    = Kaun se DNS servers domain handle karenge
SOA Record   = Domain ki basic info — auto created
A Record     = Domain → IP Address
CNAME Record = Domain → Doosra Domain
Alias Record = Domain → AWS Resource (ALB, S3, CloudFront)
TTL          = Kitne seconds tak DNS response cache rahega
Health Check = Server theek hai ya nahi — har 30 sec pe check
Failover     = Primary down → Automatic Secondary pe traffic
Geo Routing  = India users → India server, US → US server
Weighted     = 70% traffic new server, 30% old server
```
