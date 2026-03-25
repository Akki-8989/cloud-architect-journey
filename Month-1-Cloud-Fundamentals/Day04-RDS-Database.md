# DAY 4 — RDS (Relational Database Service)
**Date: 22 March 2026 | Student: Akash | Mentor: Claude AI**

---

## WHAT IS DATABASE?

Database ek aisa system hai jahan information ko organized tarike se store kiya jaata hai taaki baad mein aasani se dhundha ja sake, padha ja sake, aur badla ja sake.

Database mein data **tables** mein store hota hai — bilkul rows aur columns ki tarah.

```
CUSTOMERS TABLE:
ID  | Naam          | Phone      | City
----|---------------|------------|--------
1   | Akash Patil   | 9876543210 | Mumbai
2   | Rahul Sharma  | 9123456789 | Delhi
```

---

## WHAT IS RDS?

RDS = Relational Database Service

AWS ka managed database service hai. AWS tumhare liye database server ready karta hai aur uski saari zimmedari leta hai.

```
Tumhara kaam:  Sirf data store karo aur use karo
AWS ka kaam:   Backup, updates, crash fix, scaling
```

---

## SUPPORTED DATABASES

| Database | Description |
|----------|-------------|
| MySQL | Sabse popular open source database |
| PostgreSQL | Advanced open source database |
| SQL Server | Microsoft ka database |
| Oracle | Enterprise database |
| MariaDB | MySQL ka improved version |
| Aurora | AWS ka khud ka database — sabse fast |

---

## 4 MAIN FEATURES

### 1. Automated Backup
AWS automatically roz database ka backup leta hai. Tum maximum 35 din peeche ja sakte ho aur us waqt ka poora data wapas le sakte ho. Kuch karna nahi hota — automatically hota hai.

### 2. Multi-AZ Deployment
Tumhara database ek saath do alag data centers mein chalta hai.

```
PRIMARY DATABASE  → ap-south-1a (main)
STANDBY DATABASE  → ap-south-1b (backup)
```

Agar primary crash ho jaaye toh AWS 60 seconds mein standby ko main bana deta hai. Users ko pata bhi nahi chalta. Yahi High Availability hai.

### 3. Read Replicas
Jab bahut zyada users database se data padh rahe hon toh load kam karne ke liye ek extra copy banayi jaati hai jo sirf padhne ke liye hoti hai.

```
PRIMARY DB    → Sirf writing ke liye (insert, update, delete)
READ REPLICA  → Sirf reading ke liye (select, search)
```

### 4. Encryption
Database mein jo bhi data jaata hai woh automatically secret code mein badal jaata hai. Bina key ke koi nahi padh sakta.

---

## MULTI-AZ vs READ REPLICA

| | Multi-AZ | Read Replica |
|--|----------|--------------|
| **Kisliye** | High Availability | Performance |
| **Kab use hoti hai** | Sirf failover pe | Roz read ke liye |
| **Data sync** | Real time | Thodi der se |
| **Cost** | 2x | Extra replica |
| **Kab lagao** | Production pe hamesha | High traffic pe |

---

## RDS vs EC2 PE DATABASE

```
RDS:
✓ AWS sab manage karta hai
✓ Automatic backup
✓ Easy Multi-AZ
✓ Easy Read Replicas
✗ OS level access nahi

EC2 pe Database:
✓ Full control
✗ Sab kuch tumhe karna hoga
✗ Backup manual
✗ HA setup complex
```

**Architect Rule: 99% cases mein RDS use karo.**

---

## AAJ KA HANDS-ON

```
Database Name:  akash-first-db
Engine:         MySQL Community
Size:           db.t4g.micro (Free Tier)
Username:       admin
Region:         ap-south-1a (Mumbai)
Status:         Available ✓
```

---

## INTERVIEW QUESTIONS & ANSWERS

**Q1: RDS kya hai?**
> AWS ka managed relational database service. AWS database server ki saari zimmedari leta hai — backup, updates, crash fix. Tum sirf data use karo.

**Q2: Multi-AZ kya hai aur kyun use karte hain?**
> Database ko ek saath do Availability Zones mein chalana. Agar ek AZ fail ho toh dusra automatically kaam karta hai. High Availability ke liye use karte hain.

**Q3: Multi-AZ aur Read Replica mein kya fark hai?**
> Multi-AZ = High Availability ke liye, failover pe kaam aata hai. Read Replica = Performance ke liye, roz read queries ke liye use hota hai.

**Q4: RDS mein automated backup kya hai?**
> AWS roz apne aap backup leta hai. Maximum 35 din peeche ja sakte ho. Koi manual kaam nahi.

**Q5: RDS vs EC2 pe database — kab kya choose karein?**
> 99% cases mein RDS choose karo kyunki AWS sab manage karta hai. EC2 pe sirf tab jab special OS level access chahiye ya specific software install karna ho.

**Q6: Aurora kya hai?**
> AWS ka khud ka database engine. MySQL aur PostgreSQL se 5x fast hai. Automatically scale hota hai. Enterprise production ke liye best choice.

**Q7: RDS mein encryption kaise hoti hai?**
> Database create karte waqt encryption enable karo. Uske baad jo bhi data store hoga woh automatically encrypt ho jaata hai. AWS KMS keys use hoti hain.

---

## KEY POINTS — YAAD RAKHO

```
1. RDS = AWS managed database — AWS sab sambhalta hai
2. Multi-AZ = High Availability (failover ke liye)
3. Read Replica = Performance (read load ke liye)
4. Automated Backup = 35 din tak wapas ja sakte ho
5. Production pe hamesha Multi-AZ lagao
6. 99% cases mein RDS choose karo EC2 pe DB se better
```

---
*Day 4 Complete | Next: Day 5 — IAM (Identity & Access Management)*
