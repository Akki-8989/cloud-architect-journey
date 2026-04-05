# Day 09 — Security Groups Hands-On

## Security Group Kya Hai

Security Group ek virtual firewall hai jo decide karta hai ki EC2 server pe kaun si traffic andar aa sakti hai aur kaun si bahar ja sakti hai. Jab tumhara EC2 server internet pe hota hai toh koi bhi request bhej sakta hai — hacker bhi, tumhara user bhi. Security Group ye filter karta hai ki kaun andar aaye aur kaun nahi.

---

## Inbound aur Outbound Rules

**Inbound Rules** — Bahar se andar aane wali traffic ke liye. Jaise tumhare ghar ka darwaza — kaun andar aa sakta hai ye tum decide karte ho.

**Outbound Rules** — Andar se bahar jaane wali traffic ke liye. By default AWS sabko bahar jaane deta hai — isliye outbound rules mein kuch change nahi karna padta normally.

---

## Security Group Rule Mein 3 Cheezein Hoti Hain

**Protocol** — Kaunsa type ka traffic. Jaise HTTP, HTTPS, SSH, RDP.

**Port** — Kaunse port pe traffic allow hai.
```
Port 22   = SSH   (Linux server ka remote access)
Port 3389 = RDP   (Windows server ka remote access)
Port 80   = HTTP  (Normal website)
Port 443  = HTTPS (Secure website)
Port 3306 = MySQL database
Port 1433 = SQL Server database
```

**Source** — Kahaan se traffic aa sakti hai.
```
0.0.0.0/0        = Poori duniya se (anyone can access)
Tumhara IP/32    = Sirf tumhare computer se
Koi Security Group = Sirf us group ke resources se
```

---

## Security Group Stateful Hai

Stateful ka matlab — agar tumne inbound port 80 allow kiya toh response automatically bahar ja sakta hai. Tumhe alag se outbound rule nahi likhna padta. AWS khud track karta hai ki ye response kisi allowed request ka hai.

---

## Real Example — Web Server Security Group

```
Inbound Rules:
├── HTTP  Port 80  → 0.0.0.0/0       (sab website dekh sakein)
├── HTTPS Port 443 → 0.0.0.0/0       (secure website)
└── SSH   Port 22  → Tumhara IP/32   (sirf tum login kar sako)

Outbound Rules:
└── All traffic → 0.0.0.0/0          (default — sab bahar ja sakta hai)
```

---

## Architecture Mein Security Group Kahan Hota Hai

```
Internet
    ↓
Security Group (MyWebServer-SG)
    ├── Port 80 Allow  ✓
    ├── Port 22 Allow  ✓ (sirf tumhare IP se)
    └── Port 3306 Block ✗ (database port block)
    ↓
EC2 Instance (Web Server)
```

---

## Aaj Ka Hands-On — Kya Kiya

**Security Group banaya: MyWebServer-SG**

```
Security group name : MyWebServer-SG
Description         : Security group for web server
VPC                 : Default VPC (vpc-03549e7ab8477db52)

Inbound Rules:
Rule 1: HTTP  → Port 80 → Source: 0.0.0.0/0
Rule 2: SSH   → Port 22 → Source: 103.117.184.71/32 (My IP)

Outbound Rules:
Default: All traffic allowed
```

**Result:** Security Group successfully created — sg-09b01c612d402211c ✓

---

## Interview Questions & Answers

**Q1. What is a Security Group in AWS and how does it work?**

A Security Group in AWS acts as a virtual firewall that controls the inbound and outbound traffic for AWS resources like EC2 instances. It operates at the instance level, meaning each EC2 instance can have one or more security groups attached to it. Security groups work by evaluating rules that you define — each rule specifies a protocol, a port range, and a source or destination. If incoming traffic matches an allow rule, it is permitted; otherwise, it is denied by default. Security groups only support allow rules, not deny rules, which means any traffic that is not explicitly allowed is automatically blocked.

---

**Q2. What is the difference between Inbound and Outbound rules in a Security Group?**

Inbound rules control the traffic that is allowed to reach the resource from outside. For example, allowing HTTP traffic on port 80 so that users can access a website. Outbound rules control the traffic that the resource is allowed to send out. By default, AWS allows all outbound traffic, meaning the EC2 instance can make any outgoing connection without restriction. In most cases, you modify inbound rules to restrict who can access your resource, while leaving outbound rules at their default setting.

---

**Q3. What does it mean that Security Groups are stateful?**

Stateful means that Security Groups automatically track the state of a network connection. If an inbound rule allows traffic on port 80, then the response to that request is automatically allowed to flow back out, even if there is no explicit outbound rule permitting it. This is because the Security Group recognizes that the outbound traffic is a response to an allowed inbound request. This behavior simplifies rule management because you only need to define rules in one direction for request-response communication.

---

**Q4. What is the difference between a Security Group and a Network ACL?**

A Security Group operates at the individual resource level, such as an EC2 instance, while a Network ACL operates at the subnet level and applies to all resources within that subnet. Security Groups are stateful, meaning responses to allowed inbound traffic are automatically permitted outbound. Network ACLs are stateless, meaning you must explicitly allow both inbound and outbound traffic for each connection. Security Groups only support allow rules, whereas Network ACLs support both allow and deny rules. In practice, Security Groups are used for fine-grained control at the resource level, while Network ACLs provide an additional layer of defense at the subnet level.

---

**Q5. How would you secure an EC2 instance that runs a web application with a backend database?**

To secure such an EC2 instance, I would create two separate Security Groups. The first Security Group for the web server would allow inbound HTTP on port 80 and HTTPS on port 443 from anywhere, since users need to access the website. It would also allow SSH on port 22 only from my specific IP address for administrative access. The second Security Group for the database server would not allow any inbound traffic from the internet at all. It would only allow inbound traffic on the database port, such as port 1433 for SQL Server, from the web server's Security Group. This ensures that the database is never directly accessible from the internet and can only be reached through the web server.

---

## Key Points — Phone Pe Save Karo

```
Security Group = EC2 ka virtual firewall
Inbound        = Bahar se andar aane wali traffic
Outbound       = Andar se bahar jaane wali traffic
Stateful       = Response automatically allowed hota hai
0.0.0.0/0      = Poori duniya se access
/32            = Sirf ek specific IP se access
Port 80        = HTTP website
Port 443       = HTTPS secure website
Port 22        = SSH Linux server login
Port 3389      = RDP Windows server login
Default        = Sab block hai jab tak allow na karo
```
