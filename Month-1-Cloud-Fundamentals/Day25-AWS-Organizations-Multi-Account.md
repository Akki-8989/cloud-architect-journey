# Day 25 — AWS Organizations + Multi-Account Strategy

## Real World Problem Samjho

```
Ek badi company — Swiggy — AWS use karti hai:

Problem bina Organizations ke:
→ 500 developers — sab ek hi AWS account mein!
→ Developer ne galti se Production DB delete kar diya ❌
→ Finance team ko pata nahi kaunsa team kitna kharch kar rahi hai ❌
→ Security team har account alag alag manage karti hai ❌
→ Ek account hack hua → Poori company ka data risk mein! ❌

Solution = AWS Organizations + Multi-Account Strategy
```

---

## AWS Organizations Kya Hai

```
AWS Organizations = Multiple AWS accounts ko
                    ek jagah se manage karo!

Jaise company ke departments:
→ Finance Department  → Finance AWS Account
→ Engineering         → Engineering AWS Account
→ Security Team       → Security AWS Account
→ Sab ek Organization ke under ✅
```

---

## Key Concepts

### 1. Management Account (Root Account)
```
→ Organization ka master account
→ Sabhi child accounts yahan se manage hote hain
→ Billing bhi yahan aati hai — consolidated!
→ Is account mein kaam mat karo — sirf manage karo!
```

### 2. Member Accounts
```
→ Har team/project ka alag account
→ Dev account alag
→ Production account alag
→ Security account alag
→ Ek account compromise hua → Doosre safe! ✅
```

### 3. Organizational Units (OUs)
```
OU = Accounts ka group — folder jaisa!

Example:
Root
├── Production OU
│   ├── Prod-App Account
│   └── Prod-DB Account
├── Development OU
│   ├── Dev Account
│   └── Staging Account
└── Security OU
    └── Security-Audit Account
```

### 4. Service Control Policies (SCPs)
```
SCP = Organization wide rules — override nahi ho sakti!

Example SCPs:
→ "Koi bhi account us-east-1 ke alawa region use nahi kar sakta"
→ "Dev account mein GPU instances allowed nahi"
→ "Production mein koi resource delete nahi kar sakta"

Even if developer ke paas Admin access hai →
SCP ne block kar diya → Kuch nahi kar sakta! ✅

SCP > IAM — SCP hamesha wins karti hai!
```

---

## Multi-Account Strategy — Kyun Zaroori Hai

```
Single Account (Bad):
→ Sab ek jagah → Ek galti → Sab khatam ❌
→ Cost tracking mushkil ❌
→ Security blast radius = Poori company ❌

Multi-Account (Good):
→ Isolation → Ek account ka issue → Doosre safe ✅
→ Cost per team track karo ✅
→ Different security policies per account ✅
→ Compliance easy hoti hai ✅
```

### Real World Example — Swiggy
```
Swiggy ka AWS setup:
├── Management Account (Billing only)
├── Security OU
│   └── Security-Audit Account (CloudTrail logs)
├── Production OU
│   ├── Prod-App Account (Web servers)
│   ├── Prod-DB Account (Databases)
│   └── Prod-ML Account (ML models)
└── Non-Production OU
    ├── Dev Account
    └── Staging Account

SCPs:
→ Production OU → Delete operations block!
→ Dev OU → Expensive instances block!
→ All accounts → Only Mumbai region allowed!
```

---

## AWS Control Tower

```
Control Tower = Organizations ka automatic setup!

Manual Organizations:
→ Har account manually configure karo → Time waste! ❌

Control Tower:
→ Best practices automatically apply karo ✅
→ New account banao → Automatically secure ho ✅
→ "Landing Zone" = Pre-configured secure environment
→ Enterprise companies use karte hain ✅
```

---

## Consolidated Billing

```
Bina Organizations:
→ Dev account  → $200/month → Alag bill
→ Prod account → $800/month → Alag bill
→ 3 alag bills → Confusing! ❌

AWS Organizations ke saath:
→ Ek bill → Management account pe → $1100 total ✅
→ Volume discounts! → Zyada use = Cheaper per unit ✅
```

---

## Hands-On — Aaj Kya Kiya

### Step 1 — Organization Banai
```
AWS Organizations → Create an organization → Click karo
→ "You successfully created an AWS organization" ✅
→ Organization ID: o-d84tokl08k
```

### Step 2 — SCP Enable Kiya
```
Policies → Service control policies
→ Enable service control policies → Click karo
→ FullAWSAccess policy automatically aa gayi ✅
```

### Step 3 — Custom SCP Banai
```
Create policy:
→ Name       : Deny-Expensive-Instances
→ Description: Block GPU and expensive EC2 instances in dev accounts
→ JSON:
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "DenyExpensiveInstances",
      "Effect": "Deny",
      "Action": "ec2:RunInstances",
      "Resource": "arn:aws:ec2:*:*:instance/*",
      "Condition": {
        "StringLike": {
          "ec2:InstanceType": [
            "p2.*", "p3.*", "p4.*", "g3.*", "g4.*"
          ]
        }
      }
    }
  ]
}
→ Create policy ✅
```
**Result:** Dev accounts mein GPU instances block ho gaye!

---

## Interview Questions & Answers

**Q1. What is AWS Organizations and why is it used?**

AWS Organizations is a service that allows you to centrally manage and govern multiple AWS accounts. It is used because large companies need separate AWS accounts for different teams, environments, and purposes — for example, separate accounts for development, staging, production, and security. Managing all these accounts individually is complex and error-prone. AWS Organizations provides a single management account from which you can create and manage all member accounts, apply security policies across all accounts simultaneously using Service Control Policies, and consolidate all billing into a single invoice. It also provides better security isolation — if one account is compromised, other accounts remain unaffected.

---

**Q2. What are Service Control Policies (SCPs) and how are they different from IAM policies?**

Service Control Policies are organization-level permission guardrails that define the maximum permissions available to accounts within an organization. The key difference from IAM policies is that SCPs apply at the account level and cannot be overridden — even if a user has Administrator access in their account, they cannot perform actions that are denied by an SCP. For example, if an SCP says no one can use regions outside Mumbai, even the account's root user cannot launch resources in other regions. IAM policies grant permissions within an account, while SCPs restrict what permissions are even possible within an account. SCPs always win over IAM policies.

---

**Q3. What are Organizational Units (OUs)?**

Organizational Units are containers within AWS Organizations that group accounts together for easier management. They work like folders — you can create a hierarchy where OUs contain other OUs or individual accounts. You can apply Service Control Policies to an OU and those policies automatically apply to all accounts within that OU. For example, you can create a Production OU and apply a policy that prevents resource deletion, and that policy will automatically apply to all production accounts. Common OUs include Production, Development, Security, and Sandbox.

---

**Q4. What is the difference between Management Account and Member Accounts?**

The Management Account is the master account used to create and manage the AWS Organization. It has authority to create member accounts, apply Service Control Policies, and receives consolidated billing for all accounts. It should have minimal workloads. Member Accounts are individual AWS accounts belonging to the organization — each team or environment has its own member account. They receive policies from the management account but operate independently for day-to-day workloads.

---

## Key Points — Phone Pe Save Karo

**AWS Organizations Basics:**
```
AWS Organizations  = Multiple accounts ek jagah manage karo
Management Account = Master account — billing + management
Member Accounts    = Har team/env ka alag account
OU                 = Accounts ka group — folder jaisa
```

**Security Controls:**
```
SCP                = Organization wide rules — override nahi hoti!
SCP vs IAM         = SCP > IAM — SCP wins hamesha!
Production OU      = Delete operations block karo
Dev OU             = Expensive instances block karo
```

**Automation:**
```
Control Tower      = Organizations ka automatic setup
Landing Zone       = Pre-configured secure environment
```

**Billing:**
```
Consolidated Bill  = Sab accounts ka ek bill + volume discount
```

**Architecture Benefit:**
```
Blast Radius       = Multi-account = Ek account issue → Doosre safe
```
