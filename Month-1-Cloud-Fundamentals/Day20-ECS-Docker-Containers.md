# Day 20 — ECS + Docker (Containers on AWS)

## Problem Samjho — Kyun Chahiye Containers?

Tumhara Node.js app tumhare laptop pe perfectly chal raha hai. Production server pe deploy kiya — **crash!**

```
Tumhara laptop:
Node.js 18, npm 9, Ubuntu 22 → App chal raha hai ✅

Production server:
Node.js 14, npm 6, Amazon Linux → App crash! ❌

"But mere machine pe toh chal raha tha!" 😤
```

**Ye problem har developer ne face ki hai!**

**Solution = Docker (Containers)**

```
Docker ke saath:
App + Node.js 18 + npm 9 + sab dependencies
= Ek package (Container Image) mein band kar do

Kahi bhi chalao → Same environment → Same result ✅
Laptop pe ✅  Server pe ✅  AWS pe ✅
```

---

## Docker Kya Hai

Docker ek tool hai jo application ko uski **saari dependencies ke saath package** karta hai — ek container mein.

```
Container = App + Runtime + Libraries + Config
           = Ek sealed box jisme sab kuch hai

Jaise:
Tiffin box = Khana + Dabba + Seal
Container  = App  + Dependencies + Isolated environment
```

**Developer Analogy:** Ek portable mini computer — sirf tumhara app aur uski exact environment. Kahi bhi le jao, wahi chalega!

---

## Docker Key Terms

### Image
Template/Blueprint — read only.

```
Jaise Class in OOP:
Class define karo → Multiple objects bana sakte ho
Image define karo → Multiple containers run kar sakte ho

Image mein hota hai:
→ Base OS (Ubuntu, Alpine)
→ Runtime (Node.js, Python)
→ App code
→ Dependencies
```

### Container
Running instance of Image.

```
Image (Class)  → Container (Object)

Container = Actually running process
Ek image se → 10 containers run kar sakte ho
Har container isolated → Ek crash hua → Doosre safe
```

### Dockerfile
Recipe/Instructions to build an Image.

```dockerfile
FROM nginx:alpine          # Base image lo
RUN echo 'Hello' > index.html  # Command run karo
EXPOSE 80                  # Port expose karo
```

### Registry
Images store karne ki jagah.

```
Docker Hub = Public registry (sab dekh sakte hain)
ECR        = AWS ka private registry (sirf tumhara account)
```

---

## ECS Kya Hai

ECS = **Elastic Container Service** — AWS pe containers manage karo!

```
Docker = Container banata hai (packaging)
ECS    = AWS pe containers CHALATA aur MANAGE karta hai

ECS ka kaam:
→ Containers start/stop karo
→ Automatically scale up/down karo
→ Health check — container crash → restart
→ Load balancer se connect karo
→ Logs CloudWatch mein bhejo
```

**Analogy:** Docker = Tiffin box banana. ECS = Delivery system — kaun sa box kaun ko, kitne boxes, kharab hua toh replace!

---

## ECS Key Terms — Detail Mein

### 1. Cluster
ECS ka container park — jahan saare containers chalte hain.

```
Production Cluster  → Live app ke containers
Staging Cluster     → Testing ke containers

Jaise office building:
Cluster    = Building
Containers = Employees jo andar kaam karte hain
```

### 2. Task Definition
Container ka blueprint — batata hai container kaise run hoga.

```
Task Definition mein define karte ho:
→ Kaunsi Docker Image use karni hai (ECR se)
→ Kitni CPU chahiye (0.25 vCPU, 0.5 vCPU...)
→ Kitni Memory chahiye (512 MB, 1 GB...)
→ Kaunsa port expose karna hai (80, 3000...)
→ Environment variables
→ Logs kahan jayenge (CloudWatch)

Jaise Job Description:
"Developer chahiye: Java, 5 saal exp, Node.js bhi"
= Task Definition
```

### 3. Task
Ek baar run hone wala container.

```
Task Definition se Task banta hai:
Class (Task Definition) → Object (Task)

Use case:
→ DB migration run karo → Khatam → Task stop
→ Report generate karo → Khatam → Task stop
→ One-time batch job
```

### 4. Service
Hamesha running rehne wala Task.

```
Service = Task + "Hamesha alive rakho!"

Tumne kaha: "3 containers hamesha chalte rahen"
Service ne kiya:
→ Container crash hua → Automatically naya start ✅
→ Traffic badha → 5 containers ✅
→ Traffic kam → 2 containers ✅

Use case:
→ Web app jo 24/7 chalni chahiye
→ API server jo hamesha available ho
```

### 5. ECR (Elastic Container Registry)
AWS ka private Docker Hub — images store karne ki jagah.

```
Flow:
Code likha → Docker Image banayi → ECR mein push kiya
                                          ↓
                                   ECS ne ECR se image li
                                          ↓
                                   Container run kiya ✅
```

---

## ECS Launch Types

```
EC2 Launch Type:
→ Tumhare apne EC2 servers pe containers chalao
→ Server manage karna padega
→ Zyada control chahiye toh

Fargate Launch Type:
→ Serverless containers — koi server manage nahi!
→ Sirf container define karo → AWS chalayega
→ Lambda jaisa — but containers ke liye!
→ Production mein prefer karte hain
```

---

## Poora Flow — Aaj Kya Kiya Aur Kyun

### Complete Flow Diagram

```
Step 1: Dockerfile likha
         ↓
Step 2: Docker Image build ki (docker build)
         ↓
Step 3: ECR mein login kiya (aws ecr get-login-password)
         ↓
Step 4: Image tag ki (docker tag)
         ↓
Step 5: ECR mein push ki (docker push)
         ↓
Step 6: ECS Cluster banaya
         ↓
Step 7: Task Definition banai (image + CPU + memory + port)
         ↓
Step 8: Task run kiya (Fargate pe)
         ↓
Step 9: Security Group mein Port 80 open kiya
         ↓
Step 10: Browser mein http://13.206.85.222 → Container live! ✅
```

---

## Hands-On — Aaj Kya Kiya (Step by Step)

### Step 1 — ECR Repository Banai
```
ECR → Create repository

Visibility      : Private
Repository name : akash-web-app
→ Create
```
**Kyun:** ECR = AWS ka private Docker registry. Yahan image store hogi jise ECS later pull karega. Private isliye taaki sirf hamara account access kar sake.

---

### Step 2 — ECR Login Kiya (CloudShell se)
```bash
aws ecr get-login-password --region ap-south-1 | \
docker login --username AWS --password-stdin \
313038579212.dkr.ecr.ap-south-1.amazonaws.com
```
**Result:** `Login Succeeded` ✅

**Kyun:** ECR private registry hai — push karne se pehle authenticate karna padta hai. AWS credentials se temporary password liya aur Docker login kiya.

---

### Step 3 — Dockerfile Banai
```bash
mkdir myapp && cd myapp
cat > Dockerfile << 'EOF'
FROM nginx:alpine
RUN echo '<h1>Hello from Akash ECS Container!</h1>
<p>Day 20 - Docker on AWS</p>' > /usr/share/nginx/html/index.html
EXPOSE 80
EOF
```
**Kyun:**
- `FROM nginx:alpine` = Nginx web server ka lightweight image base liya
- `RUN echo` = HTML file create ki jo browser mein dikhegi
- `EXPOSE 80` = Port 80 pe traffic aane do (HTTP)

---

### Step 4 — Docker Image Build Ki
```bash
docker build -t akash-web-app .
```
**Result:** Image build complete ✅

**Kyun:** Dockerfile ki recipe se actual Image bani. `.` matlab current directory mein Dockerfile hai. `-t` matlab tag/naam do image ko.

**Output mein kya hua:**
```
Step 1: nginx:alpine base image download hua
Step 2: HTML file create hua container mein
Step 3: Image layers save hue
Final:  akash-web-app image ready!
```

---

### Step 5 — Image Tag Ki aur ECR Mein Push Ki
```bash
# Tag karo — ECR ka address add karo
docker tag akash-web-app:latest \
313038579212.dkr.ecr.ap-south-1.amazonaws.com/akash-web-app:latest

# ECR mein push karo
docker push \
313038579212.dkr.ecr.ap-south-1.amazonaws.com/akash-web-app:latest
```
**Kyun tag kiya:** Docker ko batana pada ki ye image ECR mein jaayegi — isliye ECR ka full address add kiya image ke naam mein.

**Kyun push kiya:** Image abhi sirf CloudShell mein thi — ECR mein push kiya taaki ECS kahi se bhi download kar sake.

---

### Step 6 — ECS Cluster Banaya
```
ECS → Clusters → Create cluster

Cluster name  : akash-cluster-v2
Infrastructure: Fargate only
→ Create
```
**Kyun:** Cluster = Container park — ECS ko ek jagah chahiye containers chalane ke liye. Fargate select kiya taaki koi server manage na karna pade — fully serverless!

---

### Step 7 — Task Definition Banai
```
ECS → Task definitions → Create new

Task definition family : akash-web-task
Launch type            : AWS Fargate
CPU                    : 0.25 vCPU
Memory                 : 0.5 GB

Container:
  Name      : akash-web-container
  Image URI : 313038579212.dkr.ecr.ap-south-1.amazonaws.com/akash-web-app:latest
  Port      : 80
→ Create
```
**Kyun:** Task Definition = Blueprint — ECS ko bataya ki:
- Kaunsi image use karni hai (ECR wali)
- Kitni resources chahiye (0.25 vCPU, 0.5 GB — minimum — kyunki sirf demo hai)
- Kaunsa port expose karna hai (80 — HTTP)

---

### Step 8 — Task Run Kiya (Fargate pe)
```
ECS → Clusters → akash-cluster-v2 → Tasks → Run new task

Task definition : akash-web-task ✅
Launch type     : Fargate ✅
Networking:
  VPC           : Default VPC
  Subnets       : 3 subnets selected
  Public IP     : Turned ON ← Important!
→ Create
```
**Kyun Public IP ON:** Browser se access karna tha → Public IP chahiye tha. Bina Public IP ke container internet se accessible nahi hota.

**Result:**
```
Task Status: Provisioning → Running ✅
Public IP  : 13.206.85.222
```

---

### Step 9 — Security Group Mein Port 80 Open Kiya
```
EC2 → Security Groups → sg-0af707bde88d68af6 (default)
→ Inbound rules → Edit inbound rules
→ Add rule:
   Type   : HTTP
   Port   : 80
   Source : Anywhere-IPv4 (0.0.0.0/0)
→ Save rules
```
**Kyun:** Container running tha but browser se access nahi ho raha tha. Security Group firewall ki tarah kaam karta hai — by default sab traffic block hoti hai. Port 80 explicitly open karna pada taaki HTTP traffic container tak pahunch sake.

---

### Step 10 — Browser Mein Live Dekha
```
http://13.206.85.222

Result:
"Hello from Akash ECS Container!"
"Day 20 - Docker on AWS" ✅
```

**Tumhara Docker container AWS Fargate pe live chal raha tha — bina kisi server manage kiye!**

---

## Real World Architecture

```
Developer ne code likha
        ↓
Dockerfile se Image build ki
        ↓
ECR mein Image push ki
        ↓
ECS Task Definition banai
        ↓
ECS Service ne 3 Tasks run kiye (3 containers)
        ↓
ALB (Load Balancer) ne traffic distribute kiya
        ↓
Auto Scaling → Traffic badha → 6 containers
             → Traffic kam  → 2 containers
        ↓
Users → Fast response! ✅
```

---

## Docker vs VM (Virtual Machine)

```
VM (Virtual Machine):
Full OS + App = 20 GB+
Boot time = minutes
Heavy, slow

Container:
Sirf App + Dependencies = 50 MB
Start time = seconds
Lightweight, fast

Analogy:
VM = Poora ghar banao sirf ek kaam ke liye
Container = Sirf woh room banao jo chahiye
```

---

## Interview Questions & Answers

**Q1. What is Docker and what problem does it solve?**

Docker is a containerization platform that packages an application along with all its dependencies, runtime, libraries, and configuration into a single portable unit called a container. It solves the classic "it works on my machine" problem. Without Docker, an application might work on a developer's laptop with a specific version of Node.js or Python but fail in production because the server has a different version or missing libraries. Docker ensures that the exact same environment runs everywhere — on a developer's laptop, a staging server, or in AWS. Containers are lightweight because they share the host operating system kernel rather than each running a full OS like virtual machines do.

---

**Q2. What is the difference between a Docker Image and a Container?**

A Docker Image is a read-only template that contains the application code, runtime, libraries, and all dependencies needed to run the application. It is built from a Dockerfile and stored in a registry like ECR or Docker Hub. An Image is like a class in object-oriented programming — it defines the blueprint but does not run by itself. A Container is a running instance of an Image, like an object created from a class. You can create multiple containers from the same image, each running independently. If one container crashes, it does not affect others. Images are immutable — you create a new image when you make changes rather than modifying an existing one.

---

**Q3. What is Amazon ECS and what are its two launch types?**

Amazon ECS is a fully managed container orchestration service that makes it easy to run, stop, and manage Docker containers on AWS. It handles container scheduling, health monitoring, scaling, and integration with other AWS services like load balancers and CloudWatch. ECS has two launch types. Fargate is a serverless option where AWS manages all the underlying infrastructure — you simply define your container's CPU and memory requirements and ECS runs it without you having to provision or manage any servers. EC2 launch type lets you run containers on your own EC2 instances, giving you more control over the infrastructure but requiring you to manage the servers yourself. Fargate is preferred for most modern workloads because it eliminates operational overhead.

---

**Q4. What is the flow from writing code to running a container on ECS?**

The complete flow is: first, write a Dockerfile that defines how to build your container image — the base image, commands to run, and ports to expose. Second, build the Docker image locally using docker build. Third, authenticate with ECR using the AWS CLI to get a login token. Fourth, tag the image with the ECR repository URI and push it to ECR using docker push. Fifth, create an ECS cluster which is the environment where containers will run. Sixth, create a Task Definition that specifies which ECR image to use, how much CPU and memory to allocate, and which ports to expose. Finally, run a Task or create a Service in the cluster — ECS pulls the image from ECR and starts the container on Fargate infrastructure.

---

**Q5. What is ECR and why is it used instead of Docker Hub?**

Amazon Elastic Container Registry is AWS's fully managed private container image registry. It is used instead of Docker Hub in AWS environments for several reasons. First, security — ECR repositories are private by default and access is controlled through IAM policies, ensuring only authorized AWS accounts and services can pull images. Second, performance — since ECR is in the same AWS region as your ECS cluster, image pulls are faster and do not incur data transfer costs. Third, integration — ECS, EKS, and Lambda natively integrate with ECR without additional authentication configuration. Fourth, compliance — images stored in ECR benefit from AWS's security and compliance certifications. Docker Hub is a public registry suitable for open-source images, but production workloads with proprietary code should use a private registry like ECR.

---

## Key Points — Phone Pe Save Karo

```
Docker         = App ko dependencies ke saath package karo
Image          = Blueprint (read-only) — Class jaisa
Container      = Running instance — Object jaisa
Dockerfile     = Image banane ki recipe
ECR            = AWS ka private Docker registry
ECS            = AWS pe containers manage karo
Cluster        = Container park — jahan containers chalte hain
Task Definition= Container ka blueprint (image + CPU + memory + port)
Task           = Ek baar run hone wala container
Service        = Hamesha running rehne wala Task (auto-restart)
Fargate        = Serverless containers — koi server manage nahi
docker build   = Image banao Dockerfile se
docker push    = Image ECR mein bhejo
docker tag     = Image ko ECR address do
Port 80        = HTTP traffic — Security Group mein open karna padta hai
Public IP      = Task run karte waqt ON karo — tabhi browser se access
```
