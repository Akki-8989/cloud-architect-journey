# Day 10 — Custom VPC Banana

## Aaj Kya Kiya

Day 6 mein sirf Default VPC dekha tha jo AWS ne khud banaya hua tha. Aaj humne apna khud ka VPC banaya — Public Subnet, Private Subnet, Internet Gateway sab khud configure kiya. Ye ek Cloud Architect ka sabse important kaam hai — har company ka AWS infrastructure isi tarah ke custom VPC pe chalta hai.

---

## Jo Architecture Banaya

```
MyCustomVPC (10.0.0.0/16)
│
├── PublicSubnet  (10.0.1.0/24) → ap-south-1a → Internet se connected
├── PrivateSubnet (10.0.2.0/24) → ap-south-1a → Internet se isolated
├── Internet Gateway (MyIGW)    → VPC ka internet door
└── Route Table (MyPublicRouteTable)
        ├── 0.0.0.0/0   → MyIGW  (internet traffic)
        └── 10.0.0.0/16 → local  (VPC internal traffic)
```

---

## Step by Step Hands-On — Kya Kiya aur Kyun Kiya

### Step 1 — VPC Banaya

**AWS Console → VPC → Your VPCs → Create VPC**

```
Resources to create : VPC only
Name tag            : MyCustomVPC
IPv4 CIDR           : 10.0.0.0/16
Tenancy             : Default
```

**Result:** VPC ID `vpc-00b1769aab1177de1` — State: Available ✓

**Kyun:** VPC banate waqt CIDR block `10.0.0.0/16` diya — iska matlab 65,536 IP addresses available hain is private network mein. Ye poora ek private space hai sirf hamare resources ke liye.

---

### Step 2 — Public Subnet Banaya

**VPC → Subnets → Create subnet**

```
VPC ID            : MyCustomVPC (vpc-00b1769aab1177de1)
Subnet name       : PublicSubnet
Availability Zone : ap-south-1a
IPv4 CIDR         : 10.0.1.0/24
```

**Result:** Subnet ID `subnet-040414a037efcd4fa` — State: Available — 251 IPs available ✓

**Kyun:** Public Subnet mein web servers aur load balancers aayenge jo internet se accessible honge. CIDR `10.0.1.0/24` diya matlab 256 IP addresses is subnet ke liye.

---

### Step 3 — Private Subnet Banaya

**VPC → Subnets → Create subnet**

```
VPC ID            : MyCustomVPC (vpc-00b1769aab1177de1)
Subnet name       : PrivateSubnet
Availability Zone : ap-south-1a
IPv4 CIDR         : 10.0.2.0/24
```

**Result:** Subnet ID `subnet-05e0c00f74c8361b9` — State: Available — 251 IPs available ✓

**Kyun:** Private Subnet mein databases aur application servers aayenge. Ye subnet internet se bilkul isolated rahegi — bahar se koi directly access nahi kar sakta.

---

### Step 4 — Internet Gateway Banaya aur VPC se Attach Kiya

**VPC → Internet gateways → Create internet gateway**

```
Name tag : MyIGW
```

**Result:** IGW ID `igw-05c27392c552c13e2` — State: Detached ✓

Phir **"Attach to a VPC"** button click kiya → MyCustomVPC select kiya

**Result:** State: Attached to `vpc-00b1769aab1177de1` ✓

**Kyun:** Internet Gateway VPC ka internet door hai. Banane ke baad attach karna zaroori hai — bina attach kiye VPC internet se connect nahi hota.

---

### Step 5 — Route Table Banaya aur Internet Route Add Kiya

**VPC → Route tables → Create route table**

```
Name : MyPublicRouteTable
VPC  : MyCustomVPC
```

**Result:** Route Table ID `rtb-006c08ba5fc0d24dc` ✓

Phir **Edit routes → Add route:**

```
Destination : 0.0.0.0/0
Target      : Internet Gateway → MyIGW (igw-05c27392c552c13e2)
```

**Result:** Routes (2):
```
0.0.0.0/0   → igw-05c27392c552c13e2  (Active) ✓
10.0.0.0/16 → local                  (Active) ✓
```

**Kyun:** Route Table traffic ka direction guide hai. `0.0.0.0/0 → MyIGW` ka matlab hai ki koi bhi internet traffic Internet Gateway ke through jaayegi. Bina is route ke subnet public nahi hoti.

---

### Step 6 — PublicSubnet ko Route Table se Associate Kiya

**Route Table → Subnet associations → Edit subnet associations**

PublicSubnet checkbox tick kiya → Save associations

**Result:** Explicit subnet associations: `subnet-040414a037efcd4fa / PublicSubnet` ✓

**Kyun:** Sirf PublicSubnet ko is Route Table se associate kiya. PrivateSubnet associate nahi kiya — isliye PrivateSubnet internet se isolated rahegi. Yahi fark hai Public aur Private Subnet mein.

---

## Public vs Private Subnet — Fark Kaise Hota Hai

Dono subnets technically same hain — fark sirf Route Table mein hota hai.

**Public Subnet** — Us Route Table se associated hai jisme `0.0.0.0/0 → Internet Gateway` route hai. Isliye internet traffic aa-ja sakti hai.

**Private Subnet** — Kisi aise Route Table se associated hai jisme Internet Gateway ka route nahi hai. Isliye internet traffic bilkul nahi aayi-jaayegi.

---

## Banaye Gaye Resources Ka Summary

| Resource | Name | ID | Details |
|----------|------|----|---------|
| VPC | MyCustomVPC | vpc-00b1769aab1177de1 | CIDR: 10.0.0.0/16 |
| Public Subnet | PublicSubnet | subnet-040414a037efcd4fa | CIDR: 10.0.1.0/24, AZ: ap-south-1a |
| Private Subnet | PrivateSubnet | subnet-05e0c00f74c8361b9 | CIDR: 10.0.2.0/24, AZ: ap-south-1a |
| Internet Gateway | MyIGW | igw-05c27392c552c13e2 | State: Attached |
| Route Table | MyPublicRouteTable | rtb-006c08ba5fc0d24dc | 0.0.0.0/0 → MyIGW |

---

## Real Production Architecture

```
Internet
    ↓
Internet Gateway (MyIGW)
    ↓
Public Subnet (10.0.1.0/24)
    ├── Web Server (EC2)      ← Users yahan se access karte hain
    └── Load Balancer         ← Traffic distribute karta hai
    ↓
Private Subnet (10.0.2.0/24)
    └── Database (RDS)        ← Sirf Web Server access kar sakta hai
```

---

## Interview Questions & Answers

**Q1. What is the difference between a Default VPC and a Custom VPC?**

A Default VPC is automatically created by AWS in every region when you create an AWS account. It comes pre-configured with subnets, an internet gateway, and route tables, making it easy to get started quickly. However, it is not suitable for production workloads because it lacks the security isolation and custom configuration that enterprise applications require. A Custom VPC is one that you create and configure yourself. You define your own IP address range, create public and private subnets based on your requirements, and configure routing and security according to your needs. All production architectures use custom VPCs because they give you full control over your network environment.

---

**Q2. How do you make a subnet public in AWS?**

A subnet becomes public by associating it with a route table that has a route directing internet-bound traffic to an Internet Gateway. Specifically, you need to add a route with destination 0.0.0.0/0 and target pointing to an Internet Gateway, and then associate this route table with the subnet. Without this route, even if the subnet is in a VPC that has an Internet Gateway, the resources in that subnet cannot communicate with the internet.

---

**Q3. Walk me through how you would design a VPC for a web application with a database.**

I would start by creating a custom VPC with a CIDR block like 10.0.0.0/16 to have sufficient IP addresses. Then I would create two types of subnets. The first would be public subnets where I would place the web servers and load balancers that need to be accessible from the internet. The second would be private subnets where I would place the database servers that should never be directly accessible from the internet. I would then create an Internet Gateway and attach it to the VPC, create a route table for the public subnets with a route directing all internet traffic through the Internet Gateway, and associate only the public subnets with this route table. The private subnets would remain isolated from the internet. Finally, I would configure Security Groups to allow only the necessary traffic between the web servers and the database.

---

**Q4. What happens if you do not associate a subnet with any route table?**

If a subnet is not explicitly associated with a custom route table, it automatically gets associated with the main route table of the VPC. The main route table is created by default when you create a VPC and only has a local route that allows traffic within the VPC. This means the subnet would only be able to communicate within the VPC and would have no internet access unless the main route table itself has a route to an Internet Gateway.

---

**Q5. Why should databases always be placed in private subnets?**

Databases should always be placed in private subnets because they contain sensitive business data that should never be directly accessible from the internet. If a database were placed in a public subnet, it would be exposed to the internet, making it vulnerable to unauthorized access, brute force attacks, and data breaches. By placing the database in a private subnet, you ensure that it can only be accessed by resources within the VPC, such as the web servers or application servers that legitimately need to query it. This is a fundamental security principle in cloud architecture — minimize the attack surface by exposing only what is absolutely necessary to the internet.

---

## Key Points — Phone Pe Save Karo

```
Custom VPC     = Khud banao, khud configure karo
Public Subnet  = Route Table mein Internet Gateway route hai
Private Subnet = Route Table mein Internet Gateway route nahi
Internet Gateway = VPC ka internet door — attach karna zaroori
Route Table    = Traffic ka direction guide
0.0.0.0/0      = Poori internet ki traffic
10.0.0.0/16 → local = VPC ke andar ki traffic
Web Server     = Public Subnet mein
Database       = Private Subnet mein
```
