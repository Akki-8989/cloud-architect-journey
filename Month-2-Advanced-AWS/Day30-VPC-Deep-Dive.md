# Day 30 — VPC Deep Dive (Month 2 Start!)

## Month 1 VPC Recap

```
VPC = Tumhara private network AWS mein

Basic setup:
├── Public Subnet  → Web Server (Internet se accessible)
└── Private Subnet → Database (Internet se hidden)

Aaj 4 advanced concepts seekhe:
1. Network ACL vs Security Group
2. Bastion Host
3. VPC Peering
4. VPC Endpoints
```

---

## Concept 1 — Network ACL vs Security Group

### Security Group
```
Security Group = EC2 ka personal bodyguard

→ Instance level pe kaam karta hai (EC2 pe)
→ Sirf ALLOW rules likh sakte ho (Deny nahi!)
→ Stateful — ek baar allow kiya → response automatically allowed!

Example:
→ Port 80 allow kiya → Request aur response dono allowed ✅
```

### Network ACL
```
Network ACL = Subnet ka main gate security

→ Subnet level pe kaam karta hai
→ ALLOW + DENY dono rules likh sakte ho!
→ Stateless — har request alag check hoti hai!
           (Inbound aur Outbound dono rules likhne padte hain)

Example:
→ Port 80 inbound allow kiya
→ Port 80 outbound bhi alag se allow karna padega! ✅
```

### Analogy
```
Network ACL   = Society ka main gate (sabko check karo)
Security Group = Ghar ka darwaza (specific log allow karo)

Traffic flow:
Internet → Network ACL check → Security Group check → EC2 ✅
```

### Key Differences
```
Feature          | Security Group    | Network ACL
-----------------|-------------------|------------------
Level            | Instance (EC2)    | Subnet
Rules            | Sirf Allow        | Allow + Deny
State            | Stateful          | Stateless
Default          | All deny          | All allow
Order            | All rules check   | Number order check
```

---

## Concept 2 — Bastion Host

### Problem
```
Private Subnet mein App Server hai:
→ Internet se hidden hai ✅ (secure!)
→ Developer ko SSH se andar jaana hai
→ Direct internet se SSH nahi ho sakta! ❌
```

### Solution — Bastion Host
```
Bastion Host = Jump Server (Bridge!)

Public Subnet mein ek special EC2 rakho:
→ Developer pehle Bastion pe SSH kare
→ Bastion se Private Server pe SSH kare

Flow:
Developer (Laptop)
    ↓ SSH (Step 1)
Bastion Host (Public Subnet) ← Internet se accessible
    ↓ SSH (Step 2)
Private App Server (Private Subnet) ← Internet se hidden
```

### Architecture Diagram
```
Internet
    ↓
┌─────────────────────────────────┐
│            VPC                  │
│                                 │
│  ┌───────────────────────────┐  │
│  │      Public Subnet        │  │
│  │   🖥️ Bastion Host        │←─┼── Developer SSH kare
│  └─────────────┬─────────────┘  │
│                │ SSH             │
│                ↓                │
│  ┌───────────────────────────┐  │
│  │      Private Subnet       │  │
│  │   🖥️ App/DB Server       │  │
│  │   (Internet se hidden!)   │  │
│  └───────────────────────────┘  │
└─────────────────────────────────┘
```

### Bastion Host Security Rules
```
Bastion pe Security Group:
→ Port 22 (SSH) = Sirf tumhara IP allow karo
→ Baaki sab    = Block! ❌

Ye kyun?
→ Bastion = Sensitive entry point
→ Sirf authorized developer access kar sake ✅
```

### Analogy
```
Bank ka example:
→ Seedha vault mein nahi ja sakte ❌
→ Pehle reception (Bastion) pe aao ✅
→ Verify hua → Private area mein jao ✅

Bastion = Bank ki reception!
Private Server = Bank ka vault!
```

---

## Concept 3 — VPC Peering

### Problem
```
Company ke 2 alag VPCs hain:
→ VPC 1: App Servers  (10.0.0.0/16)
→ VPC 2: Analytics    (172.16.0.0/16)

App Server ko Analytics data chahiye
→ Dono alag VPCs → Direct communicate nahi kar sakte! ❌
```

### Solution — VPC Peering
```
VPC Peering = 2 VPCs ko privately connect karo!

VPC 1 ←──── Peering Connection ────→ VPC 2

→ Internet se nahi jaata traffic ✅
→ AWS private network se jaata hai ✅
→ Fast + Secure! ✅
→ Different regions mein bhi possible ✅
→ Different AWS accounts mein bhi possible ✅
```

### Analogy
```
2 alag buildings ek campus mein:

Normal way:
Building A → Main road → Building B ❌ (Slow + Unsafe)

VPC Peering:
Building A ←── Underground tunnel ──→ Building B ✅
(Direct + Fast + Secure!)
```

### IMPORTANT — Transitive Rule
```
Dost ke dost = Tumhara dost NAHI!

VPC A ←→ VPC B ✅ (Peering hai)
VPC B ←→ VPC C ✅ (Peering hai)
VPC A ←→ VPC C ❌ (NAHI HOGA!)

A se C jaana hai?
→ B ke through nahi ja sakte!
→ Alag se A-C Peering banani padegi! ✅

Matlab:
3 VPCs fully connect karne ke liye
3 alag Peering connections chahiye:
A↔B + B↔C + A↔C ✅
```

---

## Concept 4 — VPC Endpoints

### Problem
```
Private Subnet mein EC2 hai
S3 se data fetch karna hai

Normal way:
EC2 → NAT Gateway → Internet Gateway → Internet → S3

Problems:
→ Private data public internet pe ja raha hai ❌
→ NAT Gateway ka extra bill aa raha hai ❌
→ Slow hai ❌
→ Compliance issue (Bank/Hospital data internet pe!) ❌
```

### Solution — VPC Endpoint
```
VPC Endpoint = AWS services tak private tunnel!

EC2 → VPC Endpoint → S3

Fayde:
→ Internet pe gaya hi nahi! ✅
→ AWS ka internal private network use hua ✅
→ NAT Gateway ka bill nahi ✅
→ Fast + Secure! ✅
→ Compliance happy! ✅
```

### Analogy
```
Normal way:
Office (3rd floor) → Main gate → Public road → S3 building
→ Sensitive documents public road pe ❌

VPC Endpoint:
Office (3rd floor) → Internal corridor → S3 building
→ Bahar nikle hi nahi ✅
→ Documents safe ✅
→ Time bhi bacha ✅
```

### 2 Types of VPC Endpoints

**Gateway Endpoint:**
```
Kab use karein:
→ S3 access karna hai
→ DynamoDB access karna hai

Kharcha: FREE! ✅
Kaise: Route table mein entry add hoti hai
```

**Interface Endpoint:**
```
Kab use karein:
→ Baaki AWS services (CloudWatch, SSM, EC2 API)

Kharcha: Per hour + Per GB charge
Kaise: ENI (private IP) banata hai service ko
```

---

## Interview Questions & Answers

**Q1. What is the difference between Security Group and Network ACL?**

Security Group operates at the instance level — it is attached directly to an EC2 instance and controls what traffic can reach that specific instance. It only supports allow rules, meaning you cannot explicitly deny traffic. Security Groups are stateful, which means if you allow inbound traffic on port 80, the response traffic is automatically allowed without needing a separate outbound rule.

Network ACL operates at the subnet level — it controls traffic entering and leaving the entire subnet. Unlike Security Groups, Network ACLs support both allow and deny rules, which is useful for blocking specific IP addresses. Network ACLs are stateless, meaning you must explicitly create both inbound and outbound rules for traffic to flow correctly.

---

**Q2. What is a Bastion Host and why is it used?**

A Bastion Host is a special EC2 instance placed in a public subnet that acts as a secure bridge to reach instances in private subnets. Since private subnet instances are not directly accessible from the internet, developers cannot SSH into them directly. The Bastion Host solves this by being the only entry point — developers SSH into the Bastion Host first, and from there SSH into the private instances. The Bastion Host is hardened with strict security group rules allowing SSH only from specific IP addresses.

---

**Q3. What is VPC Peering and what is the transitive peering limitation?**

VPC Peering is a networking connection between two VPCs that allows them to communicate using private IP addresses as if they were in the same network. Traffic between peered VPCs does not traverse the internet — it uses AWS private network, making it fast and secure. The transitive peering limitation means that if VPC A is peered with VPC B, and VPC B is peered with VPC C, VPC A cannot communicate with VPC C through VPC B. Each VPC pair requires its own direct peering connection.

---

**Q4. What is a VPC Endpoint and when should you use it?**

A VPC Endpoint allows resources in a VPC to privately connect to AWS services like S3 or DynamoDB without requiring an internet gateway, NAT gateway, or public IP addresses. Traffic between the VPC and AWS services travels through the AWS private network instead of the public internet. You should use VPC Endpoints when your private subnet resources need to access AWS services, when you want to avoid NAT Gateway costs, when you need to keep data off the public internet for security or compliance reasons such as in banking or healthcare applications.

---

## Hands-On — Aaj Kya Kiya

### Step 1 — Bastion Host Setup
```
EC2 Launch kiya — Bastion Host:
→ Name          : akash-bastion-host
→ AMI           : Amazon Linux 2023
→ Subnet        : ap-south-1a (Public Subnet)
→ Public IP     : Enable ✅
→ Security Group: SSH Port 22 = My IP only! ✅
→ Public IP     : 52.66.42.86

EC2 Launch kiya — Private Server:
→ Name          : akash-private-server
→ Subnet        : ap-south-1b (Private Subnet)
→ Public IP     : DISABLE ✅ (Internet se hidden!)
→ Security Group: SSH Port 22 = Sirf Bastion SG se!
→ Private IP    : 172.31.9.106
```

### Step 2 — Bastion se Private Server SSH kiya
```
Step 1: Laptop → Bastion SSH
ssh -i "linux-practice-key.pem" ec2-user@52.66.42.86
→ [ec2-user@ip-172-31-33-231 ~]$ ✅

Step 2: Key Bastion pe copy ki
scp -i "key.pem" "key.pem" ec2-user@52.66.42.86:~/.ssh/
chmod 600 ~/.ssh/linux-practice-key.pem

Step 3: Bastion → Private Server SSH
ssh -i ~/.ssh/linux-practice-key.pem ec2-user@172.31.9.106
→ [ec2-user@ip-172-31-9-106 ~]$ ✅

LIVE PROOF:
Laptop → Bastion (Public) → Private Server = SUCCESS! ✅
Direct Laptop → Private Server = IMPOSSIBLE ❌
```

### Step 3 — VPC Gateway Endpoint banaya
```
VPC → Endpoints → Create endpoint

Settings:
→ Name    : akash-s3-endpoint
→ Type    : AWS Services
→ Service : com.amazonaws.ap-south-1.s3 (Gateway) ← FREE!
→ VPC     : Default VPC
→ Policy  : Full access

Result: vpce-039c02a9eb5b8c19f → Available ✅

Ab EC2 → S3 = Internet bypass → AWS Private Network! ✅
NAT Gateway bill = Zero! ✅
```

### Cleanup
```
✅ akash-bastion-host → Terminated
✅ akash-private-server → Terminated
✅ VPC Endpoint → Deleted
```

---

## Quick Wording — Ye Exact Words Use Karo (Interview mein!)

```
Security Group:
→ "Instance level pe kaam karta hai"
→ "Sirf ALLOW rules — Deny nahi likh sakte"
→ "Stateful — response automatic allowed hota hai"

Network ACL:
→ "Subnet level pe kaam karta hai"
→ "ALLOW + DENY dono rules likh sakte ho"
→ "Stateless — inbound aur outbound alag likhne padte hain"

Bastion Host:
→ "Bastion Host ek Jump Server hota hai jo Public Subnet mein hota hai —
   ek bridge ki tarah kaam karta hai Private Subnet ke servers ko SSH se access karne ke liye"

VPC Peering:
→ "2 VPCs ko privately connect karo — internet se nahi!"
→ "Transitive nahi hota — A↔B + B↔C ≠ A↔C"

VPC Endpoint:
→ "AWS services tak private tunnel — internet bypass!"
→ "Gateway = S3/DynamoDB (FREE)"
→ "Interface = Baaki services (cost lagta hai)"
```

---

## Key Points — Phone Pe Save Karo

**Network ACL vs Security Group:**
```
Security Group = Instance level, Only Allow, Stateful
Network ACL    = Subnet level, Allow+Deny, Stateless
```

**Bastion Host:**
```
Bridge = Developer → Bastion (Public) → Private Server
Strict rules = Sirf authorized IP SSH kar sake!
```

**VPC Peering:**
```
2 VPCs ko privately connect karo
Transitive NAHI hota! A↔B + B↔C ≠ A↔C
Different regions + accounts possible ✅
```

**VPC Endpoints:**
```
Gateway   = S3 + DynamoDB (FREE!) ✅
Interface = Other services (cost lagta hai)
Internet bypass → Private AWS network use karo!
```
