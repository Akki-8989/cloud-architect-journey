# Day 11 — CloudWatch (Monitoring & Alarms)

## CloudWatch Kya Hai

CloudWatch AWS ka monitoring aur observability service hai. Ye tumhare saare AWS resources ko 24/7 monitor karta hai aur tumhe batata hai ki kya ho raha hai — real time mein. Bina monitoring ke tum andheron mein hote ho — kab CPU full hua, kab website slow hui, kab error aayi — kuch pata nahi chalta. CloudWatch ye andhera door karta hai.

---

## CloudWatch Ke 4 Main Parts

### 1. Metrics
Metrics woh numbers hain jo AWS resources ke baare mein automatically collect hote hain.

EC2 ke liye automatic metrics:
```
CPUUtilization     = CPU kitna use ho raha hai (%)
NetworkIn          = Kitna data server pe aa raha hai
NetworkOut         = Kitna data server se ja raha hai
StatusCheckFailed  = Server theek hai ya nahi
```

RDS ke liye automatic metrics:
```
CPUUtilization     = Database ka CPU usage
DatabaseConnections = Kitne connections hain
FreeStorageSpace   = Kitni storage bacha hai
ReadLatency        = Read query kitni der mein complete hoti hai
```

Ye metrics har 1 minute ya 5 minute pe automatically collect hote hain — tum kuch nahi karte.

---

### 2. Alarms
Alarm ek automatic alert hai jo trigger hota hai jab koi metric ek limit cross kare.

```
Agar CPU > 70% ho 1 minute tak
→ Email bhejo (SNS notification)
→ Ya automatically naya server start karo (Auto Scaling)
```

Alarm ke 3 states:
```
OK                 = Sab theek — limit ke andar
ALARM              = Limit cross ho gayi — action lo
INSUFFICIENT DATA  = Abhi data nahi aaya — normal at start
```

---

### 3. Logs
Logs tumhare applications aur AWS services ke detailed records hain. Jaise agar tumhara web server koi error de raha hai — wo error CloudWatch Logs mein store hoti hai. Tum baad mein jaake dekh sakte ho ki exactly kya error thi aur kab aayi.

---

### 4. Dashboards
Dashboard ek visual screen hai jisme tumhare saare important metrics ek jagah dikhte hain — graphs aur charts ke roop mein. Jaise ek car ka dashboard — speed, fuel, temperature sab ek jagah. CloudWatch Dashboard mein tumhare saare servers aur databases ka health ek screen pe dikhta hai.

---

## CloudWatch Kahan Use Hota Hai

```
EC2 Server    → CPU, Memory, Network monitor karo
RDS Database  → Connection count, query time monitor karo
S3 Bucket     → Request count, errors monitor karo
Load Balancer → Response time, error rate monitor karo
Billing       → Cost limit set karo — budget alert pao
```

---

## SNS — Simple Notification Service

Jab CloudWatch Alarm trigger hota hai toh notification bhejne ke liye **SNS** use hota hai. SNS ek messaging service hai jo email, SMS, ya other services pe message bhej sakta hai.

```
CloudWatch Alarm trigger hua
    ↓
SNS Topic ko message bheja
    ↓
SNS ne tumhare email pe notification bheja
```

SNS Topic ek channel hai — usme subscribe karo toh notifications milti hain.

---

## Hands-On — Aaj Kya Kiya

### Step 1 — CloudWatch Console Khola
**AWS Console → Search → CloudWatch**

### Step 2 — Alarm Create Kiya
**CloudWatch → Alarms → Create alarm**

### Step 3 — Metric Select Ki
```
Service  : RDS
Database : akash-first-db
Metric   : CPUUtilization
```

### Step 4 — Condition Set Ki
```
Threshold type : Static
Condition      : Greater than
Value          : 70
```
Matlab — agar RDS CPU 70% se zyada ho jaaye toh alarm trigger ho.

### Step 5 — Notification Set Ki
```
SNS Topic  : MyRDS-CPU-Alert (naya banaya)
Email      : Tumhara email
```

### Step 6 — Alarm Name Diya
```
Alarm name : MyRDS-CPU-Alarm
```

### Step 7 — Email Confirm Kiya
AWS ne email bheja → "Confirm subscription" click kiya → Email confirmed ✓

**Result:**
```
Alarm Name : MyRDS-CPU-Alarm
State      : Insufficient data (normal — data collect ho raha hai)
Condition  : CPUUtilization > 70 for 1 datapoint within 1 minute ✓
Action     : Email notification via MyRDS-CPU-Alert SNS topic ✓
```

---

## Interview Questions & Answers

**Q1. What is Amazon CloudWatch and why is it important?**

Amazon CloudWatch is a monitoring and observability service provided by AWS. It collects and tracks metrics, monitors log files, sets alarms, and automatically reacts to changes in your AWS resources. It is important because without monitoring, you have no visibility into how your systems are performing. You would not know if your CPU is maxed out, if your application is throwing errors, or if your database is running out of storage space. CloudWatch provides real-time insights so you can detect and respond to problems before they impact your users.

---

**Q2. What is a CloudWatch Alarm and what are its three states?**

A CloudWatch Alarm watches a single metric over a specified time period and performs one or more actions based on the value of the metric relative to a threshold. The three states of a CloudWatch Alarm are: OK, which means the metric is within the defined threshold and everything is working normally; ALARM, which means the metric has breached the threshold and the configured action has been triggered; and INSUFFICIENT DATA, which means the alarm has just started, the metric is not available, or not enough data has been collected yet to determine the state.

---

**Q3. What is the difference between CloudWatch Metrics and CloudWatch Logs?**

CloudWatch Metrics are numerical data points collected over time that represent the performance of your AWS resources, such as CPU utilization percentage or number of database connections. They are used to track the health and performance of resources at a high level. CloudWatch Logs, on the other hand, store detailed text-based records of events that occur within your applications and AWS services. For example, every HTTP request to your web server, every error message, and every database query can be stored as a log entry. Metrics tell you what is happening in numbers, while logs tell you exactly what happened in detail.

---

**Q4. What is SNS and how does it work with CloudWatch?**

SNS stands for Simple Notification Service. It is a messaging service that can send notifications to email addresses, phone numbers via SMS, or other AWS services. It works with CloudWatch through a concept called SNS Topics. When you create a CloudWatch Alarm, you configure it to send a notification to an SNS Topic when the alarm triggers. Anyone who has subscribed to that topic — such as a developer's email address — will receive the notification. This way, the right people are immediately alerted when something goes wrong with their infrastructure.

---

**Q5. How would you use CloudWatch to monitor a production web application?**

For a production web application, I would set up CloudWatch monitoring at multiple levels. For the EC2 instances running the web servers, I would monitor CPU utilization, memory usage, and network traffic, and create alarms if CPU exceeds 80 percent for more than five minutes. For the RDS database, I would monitor CPU utilization, the number of database connections, and free storage space, with alarms for high CPU or low storage. For the load balancer, I would monitor request count, response time, and error rates, and create alarms if the error rate exceeds one percent. All alarms would be connected to an SNS topic that notifies the on-call engineer via email and SMS. I would also create a CloudWatch Dashboard that displays all these metrics in one place so the team can see the overall health of the application at a glance.

---

## Key Points — Phone Pe Save Karo

```
CloudWatch  = AWS ka monitoring system
Metrics     = Numbers jo automatically collect hote hain
Alarms      = Limit cross hone pe automatic alert
Logs        = Detailed records of everything that happened
Dashboard   = Sab metrics ek screen pe
SNS         = Notification service — email/SMS bhejta hai
OK          = Sab theek
ALARM       = Limit cross ho gayi
INSUFFICIENT DATA = Abhi data nahi aaya
```
