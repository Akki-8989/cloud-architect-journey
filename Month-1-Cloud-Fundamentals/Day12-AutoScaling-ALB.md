# Day 12 — Auto Scaling + Application Load Balancer (ALB)

## Problem Samjho — Kyun Chahiye Ye?

Normal din pe tumhara website 100 users handle kar raha hai — 1 EC2 server kafi hai. Phir **Diwali Sale** aayi — 10,000 users aa gaye. 1 server overloaded ho gaya, website crash ho gayi, customers chale gaye — revenue loss!

**Solution chahiye:**
```
Traffic badhe → automatically naaye servers start ho jaayein
Traffic kam ho → extra servers band ho jaayein (cost bachao)
Ek server crash ho → traffic automatically doosre pe chala jaaye
```

Ye kaam karta hai **Auto Scaling + Load Balancer (ALB)** ka combination.

---

## Load Balancer (ALB) — Traffic Distributor

**Developer Analogy:** Jaise Nginx reverse proxy multiple backend servers pe requests forward karta hai — ALB exactly wahi kaam karta hai, but AWS managed hai, kuch configure nahi karna.

```
User request aaya
       ↓
   ALB (Load Balancer)
  /    |    \
EC2-1  EC2-2  EC2-3
```

**ALB ke fayde:**
```
1. Traffic distribute karta hai — koi ek server overloaded nahi hoga
2. Health checks karta hai — unhealthy server pe traffic automatically band
3. Single entry point — users ko ek hi URL deni hai (ALB ka DNS name)
4. High Availability — ek server crash ho toh doosre chalte rahenge
```

**ALB ka DNS Name:** AWS automatically ek URL deta hai jaise:
```
my-alb-489589676.ap-south-1.elb.amazonaws.com
```
Users is URL se website access karte hain — directly EC2 ka IP nahi chahiye.

---

## Auto Scaling Group (ASG) — Automatic Server Management

**Developer Analogy:** Jaise pm2 Node.js processes manage karta hai — crash hone pe restart karta hai. ASG bhi wahi kaam karta hai EC2 ke level pe — automatically servers add/remove karta hai load ke hisaab se.

### 3 Important Numbers:

```
Minimum = Kam se kam itne servers HAMESHA chalte rahein
           → Site kabhi down nahi hogi
           → Example: 1

Desired  = Normal din mein itne servers chahiye
           → Right now ka target
           → Example: 2

Maximum  = Zyada se zyada itne tak badh sakta hai
           → Diwali Sale pe bhi itne se zyada nahi banenge
           → Cost control ke liye
           → Example: 4
```

### Scaling Kab Hogi:

```
Scale OUT (servers badho)  = CloudWatch Alarm: CPU > 50%  → naaya EC2 add karo
Scale IN  (servers ghato)  = CloudWatch: CPU normal hua  → extra EC2 terminate karo
```

**CloudWatch + ASG connection:** CloudWatch CPU monitor karta hai aur ASG ko batata hai — "ab server aur chahiye" ya "ab ek hatao."

---

## Launch Template — EC2 Ka Blueprint

**Developer Analogy:** Jaise Dockerfile ek baar define karo — baar baar same container banao. Launch Template bhi wahi hai — ek baar define karo, Auto Scaling baar baar same EC2 banata hai.

```
Launch Template mein hota hai:
- AMI ID        → Kaunsa OS (Amazon Linux 2023)
- Instance Type → t3.micro (kitna powerful)
- Key Pair      → SSH access ke liye
- Security Group→ Kaunse ports open
- User Data     → Startup script (Apache install karo automatically)
```

**User Data Script** (EC2 start hote hi automatically run hoti hai):
```bash
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Hello from $(hostname -f)</h1>" > /var/www/html/index.html
```

---

## Target Group — ALB Aur EC2 Ka Bridge

Target Group wo list hai jisme ALB ko pata hota hai ki traffic kaunse EC2 servers pe bhejna hai.

```
ALB → Target Group → [EC2-1, EC2-2, EC2-3]
```

**Health Check:** ALB Target Group ke through har EC2 pe GET / request bhejta hai. Agar 200 OK mila → Healthy, agar nahi mila → Unhealthy → traffic dena band.

**Important:** ASG ko Target Group se properly link karna zaroori hai — tabhi naye EC2 automatically target group mein register honge.

---

## Poora Architecture:

```
Internet
    ↓
ALB (my-alb) ← Single entry point, DNS name
    ↓
Target Group (my-alb-target-group) ← Healthy instances ki list
    ↓
Auto Scaling Group (my-asg)
  [EC2-1] [EC2-2]            ← Normal din (Desired: 2)
  
  ---- CPU > 50% (Diwali Sale) ----
  [EC2-1] [EC2-2] [EC2-3] [EC2-4]  ← Scale Out (Max: 4)
  
  ---- CPU normal ----
  [EC2-1] [EC2-2]            ← Scale In (Min: 1)
```

---

## Hands-On — Aaj Kya Kiya

### Step 1 — Launch Template Banaya
```
EC2 → Launch Templates → Create launch template

Name     : my-web-server-template
Version  : Version 1
AMI      : Amazon Linux 2023 (Free tier eligible)
Instance : t3.micro
Key Pair : my-first-key
SG       : MyWebServer-SG (port 80, 22 open)
```
**Kyun:** ASG ko EC2 banane ka blueprint chahiye tha.

### Step 2 — Auto Scaling Group Banaya
```
EC2 → Auto Scaling Groups → Create Auto Scaling group

Name             : my-asg
Launch Template  : my-web-server-template
VPC              : Default VPC
Subnets          : ap-south-1a, ap-south-1b, ap-south-1c (3 AZs)
```
**Kyun:** 3 alag Availability Zones select kiye — High Availability ke liye. Ek data center down ho toh doosron mein servers chalte rahenge.

### Step 3 — ALB Attach Kiya
```
Load Balancer Type   : Application Load Balancer
Name                 : my-alb
Scheme               : Internet-facing
Target Group         : my-alb-target-group (naya banaya)
```
**Kyun:** Internet-facing matlab public internet se traffic accept karega. Target Group ALB aur EC2 ke beech bridge hai.

### Step 4 — Health Check Enable Kiya
```
Turn on Elastic Load Balancing health checks : ✓ (checked)
Health check grace period : 300 seconds
```
**Kyun:** ALB automatically unhealthy instances detect kare aur unpe traffic band kare.

### Step 5 — Group Size Set Kiya
```
Desired  : 2
Minimum  : 1
Maximum  : 4

Scaling Policy : Target tracking
Metric         : Average CPU utilization
Target value   : 50%
```
**Kyun:** CPU 50% cross hone pe auto scale out, neeche aane pe scale in.

### Step 6 — Launch Template mein User Data Add Kiya
```
EC2 → Launch Templates → my-web-server-template
→ Actions → Modify template (Create new version)
→ Advanced details → User data:

#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Hello from $(hostname -f)</h1>" > /var/www/html/index.html
```
**Kyun:** Pehle bina web server ke instances bane — 503 error aa raha tha. User Data script se Apache automatically install ho jaata hai naye EC2 mein.

### Step 7 — ASG ko Latest Template Version pe Update Kiya
```
Auto Scaling Groups → my-asg → Details → Launch template → Edit
Version : Latest
→ Update
```
**Kyun:** ASG purana version use kar raha tha. Latest pe update kiya taaki Apache wala script use ho.

### Step 8 — Target Group se ASG Link Kiya
```
Auto Scaling Groups → my-asg → Integrations → Load balancing → Edit
→ Application, Network or Gateway Load Balancer target groups ✓
→ Select: my-alb-target-group
→ Update
```
**Kyun:** ASG aur Target Group ka connection missing tha — isliye instances register nahi ho rahe the. Ye link hone ke baad instances automatically register honge.

### Step 9 — Instance Refresh Kiya
```
Auto Scaling Groups → my-asg → Instance refresh → Start instance refresh
```
**Kyun:** Purane instances (bina Apache ke) replace karne ke liye. Rolling strategy se ek ek karke replace hue — zero downtime.

### Result:
```
Browser mein ALB DNS khola:
my-alb-489589676.ap-south-1.elb.amazonaws.com

First refresh  : Hello from ip-172-31-31-197.ap-south-1.compute.internal ✅
Second refresh : Hello from ip-172-31-8-48.ap-south-1.compute.internal   ✅

→ ALB traffic dono alag EC2 servers pe bhej raha hai — Load Balancing working!
```

---

## Interview Questions & Answers

**Q1. What is the difference between a Load Balancer and Auto Scaling?**

A Load Balancer distributes incoming traffic evenly across multiple existing EC2 instances to prevent any single instance from being overwhelmed. Auto Scaling automatically adds or removes EC2 instances based on demand. They work together: Auto Scaling ensures you have the right number of instances running at all times, and the Load Balancer ensures incoming traffic is distributed evenly across all of them. Neither is complete without the other in a production architecture.

---

**Q2. What are the three capacity settings in an Auto Scaling Group and what do they mean?**

The three capacity settings are Minimum, Desired, and Maximum. Minimum is the floor — the number of instances that must always be running regardless of load, ensuring the application never goes completely offline. Desired is the target — the number of instances Auto Scaling tries to maintain under normal conditions. Maximum is the ceiling — the upper limit on how many instances can run, which prevents runaway scaling and unexpected costs. For example, with Minimum 1, Desired 2, and Maximum 4, you always have at least 1 instance, normally run 2, but can scale up to 4 during traffic spikes like a sale event.

---

**Q3. What is a Launch Template and why is it required for Auto Scaling?**

A Launch Template is a configuration blueprint that defines the specifications for EC2 instances created by Auto Scaling. It includes the AMI ID, instance type, key pair, security groups, and any User Data startup scripts. Auto Scaling needs a Launch Template because when it automatically creates a new instance to handle increased load, it must know exactly what kind of instance to create — which operating system to use, how much compute power to allocate, which security rules to apply, and what software to install at startup. Without a Launch Template, Auto Scaling cannot create consistent, properly configured instances.

---

**Q4. What is a Target Group and how does it connect the Load Balancer to EC2 instances?**

A Target Group is a logical grouping of EC2 instances that a Load Balancer routes traffic to. The ALB does not communicate directly with EC2 instances — it forwards requests to a Target Group, which maintains a list of registered healthy instances and distributes traffic among them. The Target Group continuously performs health checks on each registered instance by sending HTTP requests. If an instance fails a health check, the Target Group marks it as unhealthy and the ALB stops sending traffic to it. When an Auto Scaling Group is properly linked to a Target Group, new instances are automatically registered when they launch and deregistered when they terminate.

---

**Q5. What happens when an EC2 instance fails a health check in this architecture?**

When an EC2 instance fails a health check, a sequence of automated actions occurs. First, the Target Group marks the instance as unhealthy and the ALB immediately stops routing traffic to it, so users are not impacted. Second, the Auto Scaling Group detects the unhealthy instance through its own health check monitoring and terminates it. Third, Auto Scaling launches a replacement EC2 instance using the Launch Template to maintain the Desired capacity. Fourth, once the new instance starts, runs its User Data script, and passes health checks, the Target Group registers it and the ALB begins sending traffic to it. This entire process is automatic and requires no manual intervention, which is why this architecture provides high availability.

---

**Q6. What is Multi-AZ deployment and why is it important?**

Multi-AZ deployment means distributing EC2 instances across multiple Availability Zones within a region. An Availability Zone is essentially a separate physical data center with its own power, cooling, and networking infrastructure. By spreading instances across multiple AZs and configuring the Load Balancer and Auto Scaling Group to use all of them, the application remains available even if an entire data center experiences an outage. If the data center hosting one set of instances goes down, the ALB automatically routes all traffic to instances in the remaining AZs with no manual intervention required. This is a fundamental principle of high availability architecture on AWS.

---

## Key Points — Phone Pe Save Karo

```
ALB             = Traffic distributor — multiple EC2 mein baanta hai
ASG             = Auto Scaling Group — servers automatically add/remove
Launch Template = EC2 ka blueprint — kaisa server banana hai
Target Group    = ALB aur EC2 ka bridge — healthy instances ki list
User Data       = Startup script — EC2 start hote hi run hoti hai
Min/Des/Max     = 1/2/4 → hamesha 1, normally 2, max 4 tak
Scale OUT       = Servers badhaao (CPU high)
Scale IN        = Servers ghataao (CPU low)
Health Check    = Sick server detect karo → auto replace
Multi-AZ        = Different data centers → high availability
Instance Refresh= Purane EC2 ko naye se replace karo (rolling)
503 Error       = ALB reach ho gaya but koi healthy target nahi
```
