# Day 29 — Linux Hands-On (EC2 pe Real Practice)

## Aaj Kya Kiya

```
✅ EC2 launch kiya (Amazon Linux 2023)
✅ SSH se EC2 ke andar gaye
✅ Linux commands use kiye
✅ Nginx install kiya
✅ Website live ki — browser mein dekhi!
✅ EC2 stop kiya (bill nahi aayega)
```

---

## Step 1 — EC2 Launch Kiya

```
AWS Console → EC2 → Launch Instance

Settings:
→ Name          : linux-practice-server
→ AMI           : Amazon Linux 2023 (Free tier)
→ Instance Type : t3.micro (Free tier)
→ Key Pair      : linux-practice-key.pem (naya banaya)
→ Security Group:
   ✅ SSH   (Port 22)  — Terminal access
   ✅ HTTP  (Port 80)  — Website
   ✅ HTTPS (Port 443) — Secure website

Result: Instance ID i-045f19d76ce6e551d ✅
        Public IP: 13.233.148.176
```

---

## Step 2 — SSH Se Andar Gaye

```
Git Bash mein command:
ssh -i "D:\Cloud Architect\Important keys\linux-practice-key.pem" ec2-user@13.233.148.176

Result:
   ,     #_
   ~\_  ####_        Amazon Linux 2023
  ~~  \_#####\
  ~~     \###|

[ec2-user@ip-172-31-45-50 ~]$  ← EC2 ke andar! ✅
```

**SSH ka matlab:**
```
-i = Identity file (.pem key)
ec2-user = Amazon Linux ka default username
13.233.148.176 = EC2 ka Public IP
```

---

## Step 3 — Linux Commands Use Kiye

```bash
# Main kahan hoon?
$ pwd
/home/ec2-user

# Files dekho
$ ls -la
total 12
drwx------. ec2-user  .ssh
-rw-r--r--. ec2-user  .bashrc
-rw-r--r--. ec2-user  .bash_profile

# Naya folder banao
$ mkdir myapp
$ ls
myapp  ← Ban gaya! ✅
```

**Permissions samjhe output se:**
```
drwx------ = .ssh folder (sirf owner dekh sake — 600!)
-rw-r--r-- = config files (owner rw, baaki r — 644!)
```

---

## Step 4 — Nginx Install Kiya

```bash
$ sudo yum install nginx -y

Output:
Installing: nginx x86_64 1:1.30.2
...
Complete! ✅

7 packages install hue!
```

**yum vs apt:**
```
Amazon Linux → yum use karo
Ubuntu       → apt use karo
Dono kaam ek hi karte hain — software install!
```

---

## Step 5 — Nginx Start Kiya

```bash
$ sudo systemctl start nginx
$ sudo systemctl status nginx

Output:
● nginx.service
   Active: active (running) ✅
   Main PID: 25875 (nginx)
```

---

## Step 6 — Website Live!

```
Browser mein: http://13.233.148.176

Result: "Welcome to nginx!" ✅

Matlab:
→ EC2 (Cloud server) chal raha hai ✅
→ Nginx (Web server) chal raha hai ✅
→ Internet pe accessible hai ✅
→ Real website jaisa! ✅
```

---

## Step 7 — Cleanup

```bash
# EC2 se bahar aao
$ exit
logout
Connection to 13.233.148.176 closed.

# AWS Console mein:
EC2 → linux-practice-server → Stop instance ✅
```

**Stop vs Terminate:**
```
Stop      = EC2 band karo — data safe, phir start kar sakte ho
Terminate = EC2 delete karo — data gone! (Free tier mein careful!)
```

---

## Aaj Seekhe Sab Commands

```bash
pwd                          # Current location
ls -la                       # Detailed file list
mkdir myapp                  # Folder banao
sudo yum install nginx -y    # Software install
sudo systemctl start nginx   # Service start
sudo systemctl status nginx  # Status check
exit                         # EC2 se bahar aao
```

---

## Real Flow — Aaj Kya Hua

```
Ghar (Windows laptop)
    ↓ ssh -i key.pem ec2-user@IP
Internet
    ↓
Security Group (Port 22 open)
    ↓
EC2 (Amazon Linux 2023) — andar aa gaye!
    ↓ sudo yum install nginx
Nginx install hua
    ↓ sudo systemctl start nginx
Web server start hua
    ↓
Browser: http://13.233.148.176
    ↓
"Welcome to nginx!" ✅
```

---

## Key Points — Phone Pe Save Karo

```
SSH command:
ssh -i "key.pem" ec2-user@IP

Amazon Linux username = ec2-user
Ubuntu username       = ubuntu

Software install:
Amazon Linux → sudo yum install package -y
Ubuntu       → sudo apt install package -y

Service manage:
sudo systemctl start/stop/restart/status nginx

Stop vs Terminate:
Stop      = Safe, data rehta hai
Terminate = Delete, data gone!

.pem file:
→ Safe rakho — kisi ko mat dena!
→ chmod 600 lagao
→ .gitignore mein add karo!
```
