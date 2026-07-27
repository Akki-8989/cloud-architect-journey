# Day 49 - AWS Migration Strategies (7 R's)

## Problem
```
Company ke paas purana data center hai:
-> 50 servers, high maintenance cost, scaling mushkil
-> "Cloud pe jaana chahte hain — par KAISE?"
-> Sab ek saath migrate karo? Risk bahut zyada! x
Phased approach chahiye — har app ke liye sahi strategy!
```

## Solution - 7 R's of Migration
```
AWS ne 7 strategies define ki hain.
Har application ke liye alag strategy choose karo!
```

## Analogy - Ghar Shift Karna
```
Purana ghar (On-Premise) -> Naya ghar (AWS)

7 tarike:
1. Saaman uthao, waise hi rakh do    (Rehost)
2. Thoda adjust karke rakh do         (Replatform)
3. Naya furniture kharido             (Repurchase)
4. Kuch cheezein redesign karo        (Refactor)
5. Kuch cheezein waise rehne do       (Retain)
6. Kuch cheezein kuda mein phenko     (Retire)
7. VM waise hi naye ghar mein shift   (Relocate)
```

## 7 R's - Detail

### 1. REHOST (Lift and Shift)
```
Kya: App jaise hai waise AWS pe le jaao — koi code change nahi
Kab: Jaldi migrate karna ho, time/budget kam ho
Example: On-premise server -> AWS EC2 (same app, same code)
Speed: Fastest | Optimization: Minimum
```

### 2. REPLATFORM (Lift, Tinker and Shift)
```
Kya: Thoda optimize karo — code change nahi, platform better
Kab: Quick wins chahiye without full redesign
Example: MySQL server -> AWS RDS (managed DB)
         App wahi, DB managed ho gayi
Speed: Fast | Optimization: Medium
```

### 3. REPURCHASE (Drop and Shop)
```
Kya: Purana software chhodo, SaaS product lo
Kab: Better ready-made product available ho
Example: Apna CRM server -> Salesforce
         Apna email server -> Gmail/Office365
Speed: Medium | Cost: Subscription based
```

### 4. REFACTOR / RE-ARCHITECT
```
Kya: Poora app redesign karo cloud-native ke liye
     Microservices, Serverless, Containers
Kab: App future-proof banana ho, scale karna ho
Example: Monolith app -> Lambda + API Gateway + DynamoDB
Speed: Slowest | Optimization: Maximum
Cost: Sabse zyada effort — but long-term best!
```

### 5. RETIRE
```
Kya: App band kar do — kaam ki nahi
Kab: App use nahi hoti, duplicate hai
Example: "Ye old reporting app koi use nahi karta" -> Band karo
Benefit: Bill bachao, complexity kam karo
```

### 6. RETAIN (Revisit)
```
Kya: Abhi migrate mat karo — baad mein revisit
Kab: App recently upgrade hui ho, ya bahut complex ho
Example: "Ye legacy mainframe system abhi touch mat karo"
         -> On-premise hi rehne do temporarily
```

### 7. RELOCATE
```
Kya: VMware workloads ko AWS pe shift karo bina VM change kiye
Kab: On-premise pe VMware use karte ho
Example: VMware VMs -> AWS VMware Cloud on AWS
Speed: Fast | Effort: Low
```

## Quick Comparison Table
```
Strategy     Speed     Effort   Cloud Benefit
Rehost       Fast      Low      Low (just moved)
Replatform   Medium    Medium   Medium
Repurchase   Medium    Low      Medium (SaaS)
Refactor     Slow      High     Maximum
Retire       Instant   None     Cost saving
Retain       -         None     -
Relocate     Fast      Low      Low
```

## Real World Scenario
```
Company ke 10 apps - migration plan:

Core banking (complex, risky)     -> RETAIN
Old reporting (koi use nahi)      -> RETIRE
Email server                      -> REPURCHASE (Gmail)
Web servers (jaldi migrate karna) -> REHOST
MySQL database                    -> REPLATFORM (RDS)
Main product (future growth)      -> REFACTOR
```

## WHY Framework
```
Kab Rehost karu?
-> Jaldi migrate karna ho, budget kam, code change nahi karna

Kab Refactor karu?
-> App scale karni ho, performance improve karni ho
-> Time + money invest karne ready ho
-> Long-term best option

Rehost vs Replatform?
-> Rehost:     Sab kuch same — sirf AWS pe shift
-> Replatform: App same, platform better (MySQL -> RDS)
```

## Interview Questions & Answers

**Q1: What are the 7 R's of cloud migration?**
A: The 7 R's are: Rehost (lift and shift — move as-is to cloud), Replatform (move with minor optimizations like MySQL to RDS), Repurchase (replace with SaaS like moving to Salesforce), Refactor/Re-architect (redesign for cloud-native using microservices/serverless), Retire (decommission unused applications), Retain (keep on-premises temporarily), and Relocate (move VMware workloads to AWS VMware Cloud).

**Q2: What is the difference between Rehost and Replatform?**
A: Rehost (lift and shift) means moving the application to AWS exactly as it is — no code changes, no platform changes. It's the fastest migration strategy. Replatform means moving with minor optimizations — the application code stays the same but you switch to a better platform, like moving from a self-managed MySQL server to AWS RDS managed database service. You get cloud benefits without full redesign.

**Q3: When would you choose Refactor over Rehost?**
A: Choose Refactor when you need to improve scalability, performance, or add cloud-native features significantly. Refactor involves redesigning the application using microservices, serverless (Lambda), or containers, which takes more time and effort but delivers maximum cloud benefits. Choose Rehost when you have time constraints or budget limitations and just need to get off on-premises quickly.

**Q4: What is the first step before migrating to AWS?**
A: The first step is discovery and assessment — understanding what applications you have, their dependencies, and resource utilization. AWS Application Discovery Service helps by automatically collecting server inventory and performance data from on-premises servers. This data helps choose the right migration strategy (which R) for each application.

**Q5: Why would you choose to Retire an application during migration?**
A: During migration assessment, you often discover applications that are no longer actively used, have duplicate functionality, or provide minimal business value. Retiring these applications reduces the migration scope, eliminates unnecessary cloud costs, simplifies the architecture, and reduces maintenance overhead. Typically 10-20% of applications in an enterprise can be retired.

## Key Points - Phone Pe Save Karo
```
1. 7 R's: Rehost, Replatform, Repurchase, Refactor,
          Retire, Retain, Relocate
2. Rehost = Fastest, minimum change (lift & shift)
3. Refactor = Slowest, maximum cloud benefit
4. Retire = Delete unused apps = Cost saving
5. Retain = Abhi nahi, baad mein migrate karenge
6. Replatform = App same, platform better (MySQL -> RDS)
7. Repurchase = Apna software chhodo, SaaS lo
8. Interview mein: "Which R and WHY?" poochha jaata hai!
```
