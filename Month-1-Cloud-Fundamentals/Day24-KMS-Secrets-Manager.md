# Day 24 — AWS Security (KMS + Secrets Manager)

## Real World Problem Samjho

```
Ek developer ne apna code GitHub pe push kiya:

config.js:
  DB_PASSWORD = "mypassword123"
  AWS_ACCESS_KEY = "AKIAIOSFODNN7EXAMPLE"
  API_KEY = "sk-1234567890abcdef"

Result:
→ 2 ghante mein hacker ne AWS account access kiya
→ 50 EC2 instances launch kiye — crypto mining!
→ $10,000 ka bill aa gaya ek raat mein! 😱
```

**Ye real incident hai — hazaron developers ke saath hua hai!**

**2 Problems — 2 Solutions:**
```
Problem 1: Data encrypt karna hai (S3, RDS, EBS)
Solution  : AWS KMS (Key Management Service)

Problem 2: Passwords, API keys securely store karna hai
Solution  : AWS Secrets Manager
```

---

## AWS KMS — Key Management Service

**KMS = Tumhara digital locker ka master key system**

```
Normal way:
→ Data directly store karo → Hacker ne access kiya → Data mila! ❌

KMS way:
→ Data → Encrypt with KMS key → Store karo
→ Hacker ne access kiya → Encrypted data mila → Kuch nahi! ✅
→ Key sirf authorized log use kar sakte hain
```

### KMS Kaise Kaam Karta Hai

```
Step 1: KMS mein ek key banao (CMK)
Step 2: Data encrypt karo us key se
Step 3: Encrypted data store karo (S3, RDS, EBS)
Step 4: Access karna ho → KMS se decrypt karo
        (Sirf authorized users/services kar sakte hain)
```

### KMS Key Types

```
AWS Managed Keys:
→ AWS khud manage karta hai
→ Free hai
→ aws/s3, aws/rds — AWS ke default keys
→ Control kam tumhara

Customer Managed Keys (CMK):
→ Tum khud banate ho
→ $1/month per key
→ Tum control karo — rotate, disable, delete
→ Production ke liye recommended ✅
```

### Symmetric vs Asymmetric

```
Symmetric:
→ Ek hi key — encrypt bhi, decrypt bhi
→ S3, RDS, EBS ke liye ✅
→ Fast aur simple

Asymmetric:
→ 2 keys — Public key (encrypt) + Private key (decrypt)
→ Digital signatures ke liye
→ Cross-account encryption ke liye
```

### Key Rotation

```
→ Automatically har saal key rotate karo
→ Old key se encrypted data → Still work karega ✅
→ New data → New key se encrypt hoga
→ Security best practice!
```

---

## AWS Secrets Manager

**Secrets Manager = Secure digital safe for passwords**

```
Galat tarika:
→ Code mein password likho → GitHub pe push karo
→ Hacker dekh le → Game over! ❌

Sahi tarika:
→ Password Secrets Manager mein store karo
→ Code mein sirf reference rakho: "prod/myapp/db-password"
→ Runtime pe app automatically fetch kare ✅
→ GitHub pe sirf reference — password nahi! ✅
```

### Secrets Manager Ke Fayde

```
1. Secure storage:
   → Encrypted with KMS ✅
   → IAM se access control ✅

2. Automatic rotation:
   → RDS password automatically rotate karo!
   → Har 30 din mein naya password
   → App automatically naya password fetch kare
   → Zero downtime! ✅

3. Audit trail:
   → CloudTrail se dekho — kaun ne secret access kiya?
   → "Raat 2 baje kisi ne DB password access kiya!" → Alert!

4. Versioning:
   → Purana password bhi rakha jaata hai
   → Rollback possible ✅
```

### Naming Convention

```
prod/myapp/db-password
 ↑     ↑       ↑
Env   App    Secret type

Examples:
prod/swiggy/rds-password
dev/myapp/api-key
staging/payment/stripe-key
```

### Secrets Manager vs Parameter Store

```
Parameter Store:
→ Free (standard tier)
→ Simple config values
→ Auto rotation nahi
→ Non-sensitive config ke liye

Secrets Manager:
→ $0.40/secret/month
→ Passwords, API keys, certificates
→ Automatic rotation ✅
→ Sensitive data ke liye ✅
```

---

## Real World Secure Flow

```
Insecure (Old way):
Code → DB_PASS="abc123" → GitHub → Hacker! ❌

Secure (New way):
Code → "Get secret: prod/myapp/db-password"
     → Secrets Manager → KMS se decrypt → Password return
     → App DB se connect kare ✅
     → GitHub pe sirf secret name — password nahi! ✅
```

---

## Hands-On — Aaj Kya Kiya

### Step 1 — Secret Banaya
```
Secrets Manager → Store a new secret
→ Secret type   : Other type of secret
→ Key           : db_password
→ Value         : MySecurePass@123
→ Encryption    : aws/secretsmanager
→ Secret name   : prod/myapp/db-password
→ Description   : Production database password for my app
→ Store ✅
```
**Result:** Secret securely store hua — KMS se encrypted!

### Step 2 — Secret Retrieve Kiya
```
prod/myapp/db-password → Retrieve secret value
→ db_password: MySecurePass@123 dikh gaya ✅
```
**Kyun:** Sirf authorized user (akash-dev-user) dekh sakta hai!

### Step 3 — KMS Key Banai
```
KMS → Customer-managed keys → Create key
→ Key type  : Symmetric
→ Key usage : Encrypt and decrypt
→ Alias     : akash-myapp-key
→ Admin     : akash-dev-user
→ Users     : akash-dev-user
→ Finish ✅
```
**Result:** akash-myapp-key — Enabled, Symmetric, Encrypt and decrypt!

### Step 4 — Cleanup
```
KMS key → Schedule deletion (7 days) ✅
Secret  → Schedule deletion (7 days) ✅
```

---

## Interview Questions & Answers

**Q1. What is AWS KMS and why is it important?**

AWS KMS (Key Management Service) is a managed service that makes it easy to create and control cryptographic keys used to encrypt your data. It is important because storing unencrypted sensitive data is a major security risk — if an attacker gains access to your storage, they can read all your data. With KMS, your data is encrypted at rest using keys that only authorized users and services can access. KMS integrates natively with services like S3, RDS, EBS, and DynamoDB, so you can enable encryption with a single setting. KMS also provides automatic key rotation, audit logging through CloudTrail, and centralized key management across all your AWS services.

---

**Q2. What is AWS Secrets Manager and what problem does it solve?**

AWS Secrets Manager is a service that helps you protect access to your applications by securely storing and managing sensitive information like database passwords, API keys, and certificates. It solves the problem of hardcoded credentials — developers often store passwords directly in code which can be accidentally committed to version control and exposed publicly. With Secrets Manager, your application retrieves credentials at runtime using an API call, so no sensitive information ever appears in your code. Secrets Manager also supports automatic rotation of secrets — for example, it can automatically change your RDS database password every 30 days without any application downtime.

---

**Q3. What is the difference between Secrets Manager and Parameter Store?**

Both services store configuration data securely, but they serve different purposes. AWS Systems Manager Parameter Store is free for standard parameters and is suitable for storing non-sensitive configuration values like feature flags or environment names. It does not support automatic secret rotation. AWS Secrets Manager is designed specifically for sensitive credentials like passwords and API keys. It costs $0.40 per secret per month but provides automatic rotation capabilities, deeper RDS integration, and is the recommended solution for any truly sensitive data. The general rule is to use Parameter Store for configuration and Secrets Manager for secrets.

---

**Q4. What is the difference between Symmetric and Asymmetric KMS keys?**

A Symmetric KMS key uses a single key for both encryption and decryption operations. It is faster and simpler to use, and is the most common type used for encrypting data at rest in AWS services like S3, RDS, and EBS. An Asymmetric KMS key uses a key pair — a public key for encryption and a private key for decryption. Asymmetric keys are used for digital signatures, certificate signing, and scenarios where you need to share the public key externally so others can encrypt data that only you can decrypt. For most AWS encryption use cases like protecting S3 buckets or RDS databases, Symmetric keys are the right choice.

---

## Key Points — Phone Pe Save Karo

```
KMS              = Encryption keys manage karo
CMK              = Customer Managed Key — tum control karo ($1/month)
Symmetric        = Ek key — encrypt + decrypt (S3, RDS, EBS)
Asymmetric       = Public + Private key pair (signatures)
Key rotation     = Automatic — har saal naya key ✅
Secrets Manager  = Passwords, API keys securely store karo
Auto rotation    = RDS password automatically change karo
Never hardcode   = Password code mein mat likho — KABHI NAHI!
Parameter Store  = Free — non-sensitive config ke liye
Secrets Manager  = $0.40/month — sensitive data ke liye
prod/app/secret  = Naming convention — environment/app/type
CloudTrail       = Kaun ne secret access kiya — audit trail
Encryption at rest  = KMS (stored data)
Encryption in transit = HTTPS/TLS (moving data)
```
