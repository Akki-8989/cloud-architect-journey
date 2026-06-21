# Day 28 — Linux Basics

## Linux Kyun Zaroori Hai Cloud Ke Liye

```
AWS pe 90% servers Linux pe chalte hain!
→ EC2 launch karo → Linux milta hai
→ SSH se andar jaao → Linux terminal
→ Docker containers → Linux based
→ Kubernetes → Linux nodes

Linux nahi pata → EC2 ke andar kuch nahi kar sakte ❌
Linux pata hai → EC2 pe kaam easily! ✅
```

---

## Part 1 — Basic Commands

### Commands aur kaam

```
pwd   = "Main kahan hoon?" (current location print karo)
ls    = "Yahan kya kya hai?" (files aur folders list)
cd    = "Andar jao" (change directory)
mkdir = "Naya folder banao"
rm    = "File delete karo"
cat   = "File ka content dekho"
grep  = "File mein kuch dhundho"
sudo  = "Admin ke taur pe karo (superuser)"
```

### Real EC2 Scenario

```
EC2 mein SSH se ghuse — ye commands use karoge:

$ pwd
/home/ubuntu              ← Main yahan hoon

$ ls
app  logs  config         ← Ye files hain

$ cd app                  ← App folder mein jao
$ ls
index.js  package.json

$ cat index.js            ← File padho
console.log("Hello!")

$ mkdir backup            ← Naya folder banao
$ sudo apt update         ← Software update karo (admin)
```

### Useful Combinations

```
ls -l        = Detailed list (permissions bhi dikhao)
ls -la       = Hidden files bhi dikhao
cd ..        = Ek folder upar jao
rm -rf folder = Folder aur sab kuch delete (careful!)
grep "error" logs.txt = logs.txt mein "error" dhundho
```

---

## Part 2 — File Permissions (chmod)

### 3 Owners Hote Hain

```
Owner   = File banane wala (tum)
Group   = Tumhari team
Others  = Baaki sab (duniya!)
```

### 3 Permissions Hoti Hain

```
r = read    (4) → File dekhna/padhna
w = write   (2) → File badalna
x = execute (1) → File chalana (script/program)
```

### ls -l se kaise dikhta hai

```
-rwxr-xr-- akash developers app.js

 rwx = Owner  (akash)       → read+write+execute
 r-x = Group  (developers)  → read+execute (write nahi!)
 r-- = Others (duniya)      → sirf read
```

### chmod — Numbers ka matlab

```
4 = read    (r)
2 = write   (w)
1 = execute (x)
0 = kuch nahi (-)

Jodne se permissions:
4+2+1 = 7 = rwx (full access)
4+0+1 = 5 = r-x (read + execute)
4+0+0 = 4 = r-- (sirf read)
4+2+0 = 6 = rw- (read + write)
```

### chmod Examples

```
chmod 755 app.js
→ Owner  : rwx (7) — full access
→ Group  : r-x (5) — read + execute
→ Others : r-x (5) — read + execute

chmod 644 index.html
→ Owner  : rw- (6) — read + write (tum edit kar sako)
→ Group  : r-- (4) — sirf read
→ Others : r-- (4) — sirf read (users sirf dekhen)

chmod 600 my-key.pem
→ Owner  : rw- (6) — sirf tum dekh sako
→ Group  : --- (0) — koi nahi
→ Others : --- (0) — koi nahi (private key!)
```

### Yaad Karne Ka Trick

```
chmod 777 = Sabko sab kuch ← DANGEROUS! Kabhi mat karo! ❌
chmod 755 = Normal scripts/programs ✅
chmod 644 = Web files (HTML, CSS) ✅
chmod 600 = Private keys (.pem files) ✅
```

---

## Part 3 — SSH (Secure Shell)

### SSH Kya Hai

```
SSH = Remotely server ke andar jaana (securely!)

Ghar pe baithe EC2 ke andar ghusna:
→ Direct nahi ja sakte (cloud mein hai!)
→ SSH se terminal milta hai ✅

Jaise:
TeamViewer = GUI se doosre computer pe
SSH        = Terminal se doosre server pe ✅
```

### SSH Command

```
ssh -i "my-key.pem" ubuntu@<EC2-IP>

Breakdown:
ssh        = SSH command
-i         = Identity file (tumhara .pem key)
ubuntu     = Server ka username (Ubuntu EC2 pe)
<EC2-IP>   = Server ka public IP address

Real example:
ssh -i "my-first-key.pem" ubuntu@13.233.45.67
```

### Kyun .pem File Chahiye

```
Password login = Hack ho sakta hai (brute force) ❌
.pem key login = Cryptographic — mathematically secure ✅

.pem = Tumhari unique digital key
→ Kisi ko mat dena!
→ .gitignore mein add karo! ✅
→ chmod 600 my-key.pem (sirf tum dekh sako!)
```

### SSH Flow

```
Tumhara laptop
    ↓ ssh -i key.pem ubuntu@IP
Internet
    ↓
EC2 Security Group (Port 22 open?)
    ↓ Yes → Allow
EC2 Terminal milta hai! ✅

$ ubuntu@ip-172-31-x-x:~$  ← EC2 ke andar ho!
```

---

## Part 4 — Services (systemctl)

### Service Kya Hoti Hai

```
Linux pe applications "services" ke taur pe chalte hain
Background mein silently kaam karte hain

Examples:
→ Nginx (web server service)
→ MySQL (database service)
→ SSH daemon (SSH service)
```

### systemctl Commands

```
sudo systemctl start nginx    → Service start karo
sudo systemctl stop nginx     → Service stop karo
sudo systemctl restart nginx  → Restart karo (changes apply)
sudo systemctl status nginx   → Status check karo
sudo systemctl enable nginx   → Boot pe automatically start ho
```

### Real Scenario — Nginx Install karna

```
Step 1: Install karo
$ sudo apt update
$ sudo apt install nginx

Step 2: Start karo
$ sudo systemctl start nginx

Step 3: Status check karo
$ sudo systemctl status nginx
● nginx.service - A high performance web server
   Active: active (running) ✅

Step 4: Enable karo (reboot ke baad bhi chale)
$ sudo systemctl enable nginx

Browser mein EC2 ka IP daalo → Nginx page dikhe! ✅
```

---

## Software Install Karna (apt)

```
apt = Ubuntu ka package manager (app install karo)

sudo apt update           → Available packages update karo
sudo apt install nginx    → Nginx install karo
sudo apt install mysql    → MySQL install karo
sudo apt remove nginx     → Nginx remove karo

Jaise:
apt = Ubuntu ka App Store! ✅
```

---

## Interview Questions & Answers

**Q1. What is SSH and why is it used in cloud computing?**

SSH (Secure Shell) is a cryptographic network protocol used to securely connect to remote servers over an unsecured network. In cloud computing, it is the primary way to access EC2 instances and other Linux servers. Instead of being physically present at the server, you can open a terminal on your laptop and use SSH to get a command-line interface on the remote server. SSH uses public-key cryptography — you have a private key file (.pem) on your laptop and the corresponding public key is stored on the server. This is much more secure than password-based authentication because it is mathematically very difficult to crack.

---

**Q2. What does chmod 755 mean?**

chmod 755 sets file permissions where the owner has read, write, and execute permissions (7 = 4+2+1), while the group and others have read and execute permissions only (5 = 4+0+1). This is the standard permission for executable scripts and programs — the owner can modify and run the file, while everyone else can only read and run it but cannot modify it.

---

**Q3. What is the difference between sudo and a regular command?**

sudo stands for "superuser do" and allows a regular user to run commands with administrator (root) privileges. In Linux, certain operations like installing software, modifying system files, or managing services require root access. Instead of logging in as root (which is dangerous), you prefix commands with sudo to temporarily elevate privileges for just that command. For example, apt install nginx requires sudo because installing software affects the whole system, not just your user account.

---

## Key Points — Phone Pe Save Karo

**Basic Commands:**
```
pwd    = Current location
ls     = Files list
cd     = Folder change
mkdir  = Folder banao
cat    = File padho
grep   = File mein search
sudo   = Admin mode
```

**Permissions:**
```
r=4, w=2, x=1
chmod 755 = rwxr-xr-x (scripts)
chmod 644 = rw-r--r-- (web files)
chmod 600 = rw------- (private keys!)
777 = KABHI MAT KARO! ❌
```

**SSH:**
```
ssh -i key.pem ubuntu@IP = EC2 mein ghuso
.pem file = Private rakho, chmod 600 lagao!
Port 22 = SSH ka port
```

**Services:**
```
systemctl start/stop/restart/status/enable
apt update + apt install = Software install
```
