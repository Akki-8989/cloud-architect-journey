# Day 45 — AWS Systems Manager (SSM) + Secrets Manager

## Problem 1 — Server Management

```
50 EC2 servers hain → Security patch aaya → Sab pe install karna hai

Manual way:
❌ Har server pe SSH karo (50 baar!)
❌ Command chalaao
❌ Check karo — hua ya nahi
❌ 4-5 ghante ka kaam
❌ Koi server miss hua → Security risk!
```

## Problem 2 — Passwords in Code

```
Developer ne code mein likha:
DB_PASSWORD = "mypassword123"  ← GitHub pe push ho gaya!
                                  Koi bhi dekh sakta hai! ❌

Ya environment variable:
→ Har server pe manually set karo
→ Rotate karna hua → Phir sab pe manually change karo ❌
```

---

## Solution 1 — AWS Systems Manager (SSM)

```
50 servers pe ek saath command chalaao — bina SSH ke!
Browser se → Command diya → Sab servers pe run ho gaya ✅
```

## Solution 2 — Secrets Manager

```
Passwords, API keys, DB credentials —
sab securely store karo AWS mein.
Application automatically fetch karti hai —
code mein kuch hardcode nahi! ✅
```

---

## Analogies

**SSM — Army General:**
```
Bina SSM: General → Har sipahi ke paas jaao → Alag order do (50 baar) ❌
SSM:      General → Ek order (SSM) → Sab 50 sipahi ek saath execute ✅

SSM = Walkie-talkie system — ek jagah se sab control!
```

**Secrets Manager — Bank Locker:**
```
Bina SM: Password diary mein likha → Koi bhi dekh sakta hai ❌
SM:      Password bank locker mein (encrypted)
         → Sirf authorized log access (IAM)
         → Application locker se seedha leta hai ✅
         → Code mein kuch nahi likha ✅
```

---

## SSM — 3 Main Features

```
1. Session Manager
   → EC2 pe SSH ki zaroorat nahi!
   → Browser se directly terminal access
   → Port 22 open nahi karna padta (secure!) ✅

2. Run Command
   → Ek saath multiple servers pe command chalaao
   → Example: "Sab servers pe nginx update karo"
   → Results dashboard pe dikh jaate hain ✅

3. Parameter Store
   → Simple config values store karo (FREE)
   → Example: /myapp/db-host = "db.example.com"
   → Non-sensitive config ke liye best ✅
```

---

## Secrets Manager — Features

```
1. Secure Storage
   → AES-256 encryption (always)
   → IAM se access control

2. Automatic Rotation
   → Password 30/60/90 din mein automatically change
   → Application ko pata bhi nahi chalta ✅

3. Versioning
   → Purane secrets bhi rakhe jaate hain
   → Rollback possible ✅
```

---

## Parameter Store vs Secrets Manager

| Feature | Parameter Store | Secrets Manager |
|---------|----------------|-----------------|
| Cost | Free | ~$0.40/secret/month |
| Use case | Config values | Sensitive credentials |
| Auto-rotation | ❌ No | ✅ Yes |
| Encryption | Optional | Always |
| Best for | App config, URLs | DB passwords, API keys |

---

## Architecture

**SSM Run Command:**
```
SSM Console
"Run nginx -v on all servers"
        ↓ SSM Agent (on each EC2)
EC2-1 ✅  EC2-2 ✅  EC2-3 ✅ ... EC2-50 ✅
Ek command → 50 servers pe ek saath! ✅
```

**Secrets Manager Flow:**
```
Lambda / EC2 App
        ↓ fetch secret (API call)
Secrets Manager (encrypted)
        ↓ returns password
App connects to RDS/DB ✅
Code mein koi password nahi! ✅
```

---

## Hands-On — Aaj Kya Kiya

### Step 1 — Secret banaya
- Type: Other type of secret
- Key: `db_password`
- Value: `MySecurePass@2026`
- Encryption: `aws/secretsmanager` ✅
- Name: `akash/demo/db-password`
- Description: Demo DB password for learning

### Step 2 — CloudShell se fetch kiya
```bash
aws secretsmanager get-secret-value \
  --secret-id akash/demo/db-password \
  --region ap-south-1
```

**Output:**
```json
{
  "Name": "akash/demo/db-password",
  "SecretString": "{\"db_password\":\"MySecurePass@2026\"}"
}
```
Password code mein nahi — AWS se fetch kiya! ✅

### Step 3 — Cleanup
- Secret scheduled for deletion: 7/21/2026 ✅

---

## WHY Framework

**SSM kab use karu?**
- Multiple EC2 servers manage karne ho bina SSH ke
- Ek saath sab servers pe patch/update karna ho
- Browser se server terminal access chahiye (Session Manager)

**Secrets Manager kab use karu?**
- DB passwords, API keys store karne ho
- Automatic password rotation chahiye
- Code mein credentials hardcode nahi karne

**Parameter Store vs Secrets Manager — kyun choose karu?**
- Simple config (DB hostname, feature flags) → Parameter Store (free) ✅
- Sensitive credentials (passwords, API keys) → Secrets Manager (auto-rotation) ✅

---

## Interview Questions & Answers

**Q1: What is AWS Systems Manager (SSM)?**
A: AWS Systems Manager is a management service that helps you manage EC2 instances and other AWS resources at scale. Its key features include Session Manager (browser-based terminal without SSH), Run Command (execute commands on multiple servers simultaneously), and Parameter Store (store configuration values). It eliminates the need to open port 22 or manage SSH keys.

**Q2: What is AWS Secrets Manager?**
A: AWS Secrets Manager is a service to securely store and manage sensitive information like database passwords, API keys, and OAuth tokens. It encrypts secrets using AES-256, controls access via IAM, supports automatic rotation, and allows applications to fetch credentials at runtime via API — eliminating hardcoded credentials in code.

**Q3: What is the difference between SSM Parameter Store and Secrets Manager?**
A: Parameter Store is free and suitable for non-sensitive configuration values like database hostnames, feature flags, or app settings. Secrets Manager is paid (~$0.40/secret/month) but provides automatic rotation, always-on encryption, and versioning — making it suitable for sensitive credentials like passwords and API keys.

**Q4: Why should you never hardcode passwords in code?**
A: Hardcoded passwords in code get pushed to version control (GitHub) where anyone with repo access can see them. If the repo is public, the password is exposed to everyone. Instead, use Secrets Manager or Parameter Store — the application fetches the secret at runtime, keeping credentials out of the codebase entirely.

**Q5: What is automatic rotation in Secrets Manager?**
A: Automatic rotation is a feature where Secrets Manager automatically changes a secret's value on a defined schedule (e.g., every 30, 60, or 90 days). It uses a Lambda function to update both the secret in Secrets Manager and the actual resource (like an RDS password). The application always fetches the latest version, so rotation happens transparently without any code changes.

---

## Key Points — Phone Pe Save Karo

```
1. SSM = Manage multiple servers without SSH ✅
2. SSM Session Manager = Browser terminal (no port 22 needed)
3. SSM Run Command = One command → All servers ✅
4. Secrets Manager = Secure password store (encrypted, IAM controlled)
5. Never hardcode passwords in code → Use Secrets Manager ✅
6. Parameter Store = Free, config values
7. Secrets Manager = Paid, sensitive credentials + auto-rotation
8. App fetches secret at runtime → Code mein kuch nahi ✅
```
