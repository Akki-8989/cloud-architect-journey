# Day 21 — VPC Advanced (Subnets, Internet Gateway, NAT Gateway)

## Pehle Yaad Karo — VPC Kya Tha?

VPC = Virtual Private Cloud = AWS mein tumhara apna private network.

```
Jaise ghar ka private WiFi network:
Bahar ke log directly connect nahi kar sakte
Andar ke devices aapas mein baat kar sakte hain
```

Ye basic tha. Aaj production-level advanced concepts cover kiye!

---

## Real World Problem — Kyun Chahiye Advanced VPC?

Swiggy ka AWS architecture:

```
Internet → Users order karte hain

AWS mein:
Web Servers     → Internet se accessible hone chahiye ✅
Database        → Internet se BILKUL accessible nahi hona chahiye ❌
Payment Service → Internet se BILKUL accessible nahi hona chahiye ❌
```

**Problem bina proper VPC setup ke:**
```
Agar sab ek hi network mein hain:
Hacker ne Web Server hack kiya → Database tak bhi pahunch gaya! ❌
```

**Solution = Public Subnet + Private Subnet**

---

## Public Subnet vs Private Subnet

```
Public Subnet:
→ Internet se directly accessible
→ Web Servers, Load Balancers yahan hote hain
→ Internet Gateway attached hota hai
→ Publicly routable IP addresses

Private Subnet:
→ Internet se directly accessible NAHI
→ Databases, Backend Services yahan hote hain
→ Extra security layer
→ Internet tak jaane ke liye NAT Gateway use karta hai
```

**Analogy:**
```
Public Subnet  = Ghar ka drawing room
                 Bahar ke log aa sakte hain (guests)
                 
Private Subnet = Ghar ka bedroom/locker room
                 Sirf ghar wale ja sakte hain
                 Bahar wala directly nahi aa sakta
```

---

## Internet Gateway

```
Internet Gateway = VPC ka main gate

Public Subnet ke resources:
→ Internet se request aati hai → Internet Gateway → Resource tak
→ Resource se response jaata hai → Internet Gateway → Internet

Dono direction traffic → Inbound + Outbound ✅

Ek VPC → Ek Internet Gateway
```

**Analogy:** Ghar ka main darwaza — andar bahar dono taraf jaana ho sakta hai.

---

## NAT Gateway

```
NAT = Network Address Translation

Private Subnet ke resources ko kabhi kabhi internet chahiye:
→ Security patches download karne ke liye
→ npm/pip install karne ke liye  
→ External API call karne ke liye

BUT directly internet se access nahi karna chahte!

NAT Gateway kaam karta hai:
Private Resource → NAT Gateway (Public Subnet mein) → Internet ✅
Internet → NAT Gateway → Private Resource ❌ (blocked!)

One way traffic → Outbound only!
```

**Analogy:**
```
NAT Gateway = Security guard
Andar se bahar jaana: Allowed ✅
Bahar se andar aana: Blocked ❌
```

**Important:** NAT Gateway khud **Public Subnet** mein hota hai — Private Subnet mein nahi!

---

## Route Tables

Route Table = Traffic ka GPS — batata hai ki kahan jaana hai.

```
Public Subnet Route Table:
Destination    Target
10.0.0.0/16   local          (VPC ke andar ka traffic)
0.0.0.0/0     igw-xxxxx      (baaki sab → Internet Gateway)

Private Subnet Route Table:
Destination    Target
10.0.0.0/16   local          (VPC ke andar ka traffic)
0.0.0.0/0     nat-xxxxx      (baaki sab → NAT Gateway)
```

**Simple matlab:**
```
Public subnet → Internet Gateway se connected → Internet access ✅
Private subnet → NAT Gateway se connected → Outbound only ✅
```

---

## Availability Zones — High Availability

```
AZ = Availability Zone = Alag physical data center

Mumbai region mein:
ap-south-1a = Mumbai data center 1
ap-south-1b = Mumbai data center 2

Kyun dono AZ mein same setup:
ap-south-1a ka data center down hua →
ap-south-1b automatically traffic handle karega!

High Availability = Kabhi down nahi hoga ✅
```

**Production rule:** Hamesha **minimum 2 AZ** mein resources deploy karo!

---

## VPC Peering

```
Company A ka VPC ←→ Company B ka VPC
Direct private connection — internet se nahi!

Use cases:
→ App VPC ←→ Database VPC
→ AWS Account 1 ←→ AWS Account 2
→ Different regions ke VPCs

Fayde:
→ Internet se nahi jaata → Secure ✅
→ Fast — direct connection ✅
→ AWS backbone network use karta hai ✅
```

**Real example:**
```
Zomato App VPC ←→ Zomato Payment VPC
Direct connection — koi third party beech mein nahi!
```

---

## Complete Production Architecture

```
                    INTERNET
                        ↓
               Internet Gateway
                        ↓
    ┌──────────────────────────────────────┐
    │              VPC (10.0.0.0/16)        │
    │                                       │
    │  Public Subnet        Public Subnet   │
    │  (ap-south-1a)        (ap-south-1b)   │
    │  [ALB]                [ALB]           │
    │  [NAT Gateway]        [NAT Gateway]   │
    │        ↓                    ↓         │
    │  Private Subnet       Private Subnet  │
    │  (ap-south-1a)        (ap-south-1b)   │
    │  [EC2 Web Server]     [EC2 Web Server]│
    │        ↓                    ↓         │
    │  Private Subnet       Private Subnet  │
    │  (ap-south-1a)        (ap-south-1b)   │
    │  [RDS Primary]        [RDS Standby]   │
    └──────────────────────────────────────┘
```

**Flow — User ne order diya:**
```
User → Internet → Internet Gateway
     → Public Subnet → ALB (Load Balancer)
     → Private Subnet → EC2 (Backend)
     → Private Subnet → RDS (Database)
     → Response wapas same path se
```

---

## Hands-On — Aaj Kya Kiya

### Step 1 — Custom VPC Banaya
```
VPC → Create VPC → VPC and more

Name             : my-production-vpc
IPv4 CIDR        : 10.0.0.0/16
Availability Zones: 2
Public subnets   : 2
Private subnets  : 2
NAT Gateways     : None (paid)
→ Create VPC
```

**Automatically ye sab bana:**
```
✅ VPC (10.0.0.0/16)
✅ 4 Subnets (2 public + 2 private, 2 AZs mein)
✅ Internet Gateway (automatically attached)
✅ Route Tables (public + private ke liye alag alag)
✅ S3 Endpoint
```

**Kyun 10.0.0.0/16:** /16 matlab 65,536 IP addresses available — production ke liye kaafi!

### Step 2 — Resource Map Dekha
```
VPC → my-production-vpc → Resource map tab

Dikh raha tha:
ap-south-1a: public1 subnet + private1 subnet
ap-south-1b: public2 subnet + private2 subnet
Internet Gateway: my-production-vpc-igw ✅
Route Tables: public + private ✅
```

### Step 3 — Internet Gateway Verify Kiya
```
VPC → Internet gateways
my-production-vpc-igw → Attached to my-production-vpc ✅
```

### Step 4 — Cleanup
```
VPC → Your VPCs → my-production-vpc → Delete VPC ✅
```

---

## CIDR Notation Samjho

```
10.0.0.0/16  = 65,536 IP addresses (VPC level — large)
10.0.1.0/24  = 256 IP addresses (Subnet level — normal)
10.0.2.0/24  = 256 IP addresses (Another subnet)

/16 = Bada block (VPC ke liye)
/24 = Chota block (Subnet ke liye)

Rule: VPC ka CIDR bada hona chahiye
      Subnets VPC ke andar hote hain (smaller blocks)
```

---

## Interview Questions & Answers

**Q1. What is the difference between a Public Subnet and a Private Subnet?**

A Public Subnet is a subnet that has a route to the internet through an Internet Gateway in its route table. Resources in a public subnet, such as load balancers and NAT gateways, can receive inbound traffic from the internet and send outbound traffic to the internet directly. A Private Subnet does not have a direct route to the internet. Resources in a private subnet, such as application servers and databases, cannot be directly accessed from the internet, which provides an additional layer of security. If resources in a private subnet need to make outbound connections to the internet, such as downloading security patches, they do so through a NAT Gateway that sits in a public subnet.

---

**Q2. What is an Internet Gateway and what does it do?**

An Internet Gateway is a horizontally scaled, redundant, and highly available VPC component that allows communication between your VPC and the internet. It serves two purposes: it provides a target in your VPC route tables for internet-routable traffic, and it performs network address translation for instances that have been assigned public IPv4 addresses. Each VPC can have only one Internet Gateway attached. Without an Internet Gateway, resources in your VPC cannot communicate with the internet regardless of their subnet or security group configuration. Attaching an Internet Gateway to a VPC and adding a route to it in the subnet's route table makes that subnet a public subnet.

---

**Q3. What is a NAT Gateway and why is it needed?**

A NAT Gateway, or Network Address Translation Gateway, allows resources in a private subnet to initiate outbound connections to the internet while preventing the internet from initiating inbound connections to those resources. It is needed because private subnet resources sometimes require internet access — for example, to download software updates, install packages, or call external APIs — but should not be directly reachable from the internet for security reasons. The NAT Gateway sits in a public subnet and translates the private IP addresses of outbound requests to its own public IP address. The return traffic is then forwarded back to the originating private resource. NAT Gateways are managed by AWS and highly available within an Availability Zone.

---

**Q4. Why should you deploy resources across multiple Availability Zones?**

Deploying across multiple Availability Zones ensures high availability and fault tolerance. An Availability Zone is essentially a separate physical data center with independent power, cooling, and networking. If one Availability Zone experiences an outage due to a hardware failure, power issue, or natural disaster, resources in other Availability Zones continue operating normally. By placing your resources — such as EC2 instances and RDS databases — in at least two Availability Zones with a load balancer distributing traffic between them, your application remains available even if an entire data center goes down. This is a fundamental best practice in AWS architecture and is required for most production workloads.

---

**Q5. What is VPC Peering and when would you use it?**

VPC Peering is a networking connection between two VPCs that allows resources in both VPCs to communicate with each other using private IP addresses as if they were in the same network. The traffic between peered VPCs travels over the AWS backbone network and never traverses the public internet, making it secure and fast. You would use VPC Peering when you have separate VPCs for different environments or services that need to communicate. For example, if your application servers are in one VPC and your database cluster is in another VPC for isolation, you would peer them so the application can reach the database privately. It is also used to connect VPCs across different AWS accounts when multiple teams or companies need to share resources securely.

---

## Key Points — Phone Pe Save Karo

```
VPC            = Tumhara private AWS network
Public Subnet  = Internet se accessible (ALB, NAT yahan)
Private Subnet = Internet se NOT accessible (DB, Backend yahan)
Internet GW    = VPC ka main gate — dono direction traffic
NAT Gateway    = Private → Internet (outbound only)
Route Table    = Traffic ka GPS — kahan jaaye
AZ             = Alag physical data center
/16            = 65,536 IPs (VPC level)
/24            = 256 IPs (Subnet level)
VPC Peering    = 2 VPCs direct private connection
High Avail     = Min 2 AZs mein deploy karo hamesha
Production VPC = Public subnets (ALB) + Private subnets (DB/EC2)
```
