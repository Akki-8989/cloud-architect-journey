# Day 06 — VPC (Virtual Private Cloud)

## VPC Kya Hai?

VPC ek private, isolated network hai jo tum AWS ke andar apne liye banate ho. AWS ek bada shared infrastructure hai jisme lakhon customers ke resources hain. VPC tumhara apna private space hai us infrastructure ke andar jisme sirf tumhare resources hote hain aur koi doosra access nahi kar sakta.

Jab tum VPC banate ho toh tum khud decide karte ho ki is network mein kaunse IP addresses honge, kaun internet se connect ho sakta hai, kaun bilkul private rahega, aur kaun kisse baat kar sakta hai.

---

## CIDR Block

Jab VPC banate ho toh ek IP address range define karte ho — ise CIDR Block kehte hain.

**Example:** `172.31.0.0/16`

- `/16` ka matlab hai 65,536 IP addresses available hain
- Ye range sirf tumhare VPC ke andar use hoti hai
- Default VPC ka CIDR block hota hai: `172.31.0.0/16`

---

## Subnet Kya Hoti Hai?

VPC ek bada network hota hai. Us bade network ko chhote-chhote tukdon mein baant sakte ho — in tukdon ko Subnet kehte hain. Har subnet ek specific Availability Zone mein hoti hai.

### Public Subnet
Public Subnet woh subnet hai jiske resources internet se directly accessible hote hain. Jaise web server jo duniya ko response deta hai — wo Public Subnet mein hota hai.

### Private Subnet
Private Subnet woh subnet hai jiske resources internet se bilkul accessible nahi hote. Jaise database — sirf tumhara web server usse access kare, bahar se koi nahi. Ye subnet internet ke liye invisible hoti hai.

---

## Internet Gateway

Internet Gateway ek door hai jo tumhare VPC ko internet se connect karta hai. Agar Internet Gateway nahi hai toh VPC completely isolated hai — koi bhi resource internet se accessible nahi hoga. Public Subnet ke resources ke liye Internet Gateway zaroori hai.

---

## Route Table

Route Table ek traffic direction guide hai. Ye decide karta hai ki koi network request kahaan jaayegi.

- Internet pe jaane wali request → Internet Gateway ke through jaao
- Private Subnet mein jaane wali request → Seedha andar jaao, internet mat jaao

Har subnet ki apni Route Table hoti hai.

---

## NAT Gateway

Private Subnet ke servers internet access nahi kar sakte. Lekin kabhi kabhi Private Subnet ke server ko software update download karna hota hai ya external API call karni hoti hai.

NAT Gateway is problem ka solution hai. NAT Gateway Public Subnet mein rakha jaata hai. Private server pehle NAT Gateway ko request bhejta hai, NAT Gateway internet pe jaata hai, response laata hai aur private server ko de deta hai. Is tarah private server ka IP address internet pe kabhi nahi jaata — wo safe rehta hai.

---

## Security Group vs NACL

### Security Group
- Individual resource (EC2, RDS) ke liye firewall hota hai
- Sirf Allow rules likhte hain — Deny explicitly nahi likhte
- Stateful hai — agar outgoing traffic allow ki toh incoming response automatically allow hogi

### NACL (Network Access Control List)
- Poori Subnet ke liye firewall hota hai
- Allow aur Deny dono rules likhte hain
- Stateless hai — incoming aur outgoing dono alag alag allow karne padte hain

---

## Real Architecture Example

```
Internet
    ↓
Internet Gateway
    ↓
VPC: 172.31.0.0/16
    │
    ├── Public Subnet
    │   ├── Web Server (EC2)   ← Internet se accessible
    │   └── NAT Gateway        ← Private subnet ke liye
    │
    └── Private Subnet
        └── Database (RDS)     ← Sirf Web Server access kar sakta hai
```

Web server internet se request leta hai → Database se data fetch karta hai → Response deta hai. Database kabhi directly internet se accessible nahi hota.

---

## Aaj Ka Hands-On — Default VPC Observation

AWS Console → VPC → Your VPCs → Default VPC select kiya

**Observations:**
- **CIDR Block:** `172.31.0.0/16` → 65,536 IP addresses
- **Subnets:** 3 subnets — ap-south-1a, ap-south-1b, ap-south-1c
- **Internet Gateway:** Available — already attached hai
- **Route Table:** 1 Route Table — traffic Internet Gateway ke through jaati hai

**High Availability:** Mumbai region mein 3 Availability Zones hain isliye 3 subnets hain. Agar ek AZ band ho jaye toh doosra automatically kaam karta rahega.

---

## Interview Questions & Answers

**Q1. What is a VPC in AWS and why is it important?**

A VPC, or Virtual Private Cloud, is a logically isolated section of the AWS cloud where you can launch AWS resources in a virtual network that you define. It is important because it gives you complete control over your network environment, including the selection of your own IP address range, the creation of subnets, and the configuration of route tables and gateways. Without a VPC, all your resources would be in a shared network, which poses a significant security risk.

---

**Q2. What is the difference between a Public Subnet and a Private Subnet?**

A Public Subnet is a subnet whose route table has a route to an Internet Gateway, which means resources in that subnet can communicate directly with the internet. A Private Subnet does not have a route to an Internet Gateway, which means its resources are not directly accessible from the internet. Typically, web servers and load balancers are placed in Public Subnets, while databases and application servers are placed in Private Subnets for security.

---

**Q3. What is an Internet Gateway and what is its purpose?**

An Internet Gateway is a horizontally scaled, redundant, and highly available VPC component that allows communication between your VPC and the internet. It serves two purposes: it provides a target in your VPC route tables for internet-routable traffic, and it performs network address translation for instances that have been assigned public IPv4 addresses. Without an Internet Gateway, no resource in your VPC can access or be accessed from the internet.

---

**Q4. What is a NAT Gateway and when would you use it?**

A NAT Gateway is a managed AWS service that allows resources in a Private Subnet to initiate outbound connections to the internet while preventing the internet from initiating inbound connections to those resources. You would use a NAT Gateway when your private servers need to download software updates, access external APIs, or reach any internet resource, but you do not want those servers to be directly accessible from the internet. The NAT Gateway is always placed in a Public Subnet.

---

**Q5. What is the difference between a Security Group and a Network ACL?**

A Security Group acts as a virtual firewall for individual resources such as EC2 instances. It is stateful, meaning if you allow inbound traffic, the response is automatically allowed outbound. It only supports Allow rules.

A Network ACL acts as a firewall for an entire subnet. It is stateless, meaning you must explicitly allow both inbound and outbound traffic. It supports both Allow and Deny rules. Security Groups are the first line of defense at the resource level, while Network ACLs provide an additional layer of security at the subnet level.

---

**Q6. What is a CIDR block and what does /16 mean?**

A CIDR block, which stands for Classless Inter-Domain Routing, is a method for allocating IP addresses and defining the size of a network. When you create a VPC, you assign a CIDR block to define the range of private IP addresses available within that network. The /16 notation means that the first 16 bits are fixed as the network address, leaving the remaining 16 bits for host addresses. This gives you 2 to the power of 16, which equals 65,536 IP addresses within that VPC.

---

## Key Points — Phone Pe Save Karo

1. VPC = Tumhara private isolated network AWS ke andar
2. Public Subnet = Internet se accessible
3. Private Subnet = Internet se completely hidden
4. Internet Gateway = VPC ka internet door
5. NAT Gateway = Private servers ka internet access (one-way only)
6. Security Group = Resource level firewall (Stateful)
7. NACL = Subnet level firewall (Stateless)
8. Default VPC CIDR = 172.31.0.0/16 = 65,536 IPs
9. Mumbai region = 3 AZs = 3 Subnets (High Availability)
