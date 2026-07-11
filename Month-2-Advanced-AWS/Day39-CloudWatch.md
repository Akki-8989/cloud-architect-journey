# Day 39 — CloudWatch (Monitoring + Alerting)

## Problem — Production mein kya ho raha hai pata nahi

```
App live hai — users use kar rahe hain. Lekin:

→ Server CPU 100% ho gaya — pata nahi! ❌
→ Lambda function fail ho rahi hai — pata nahi! ❌
→ 500 errors aa rahe hain — pata nahi! ❌
→ Bill badh raha hai — pata nahi! ❌

Subah uthke dekha → App down!
Users complaint kar rahe hain! 😱
```

---

## Solution — CloudWatch

```
CloudWatch = AWS ka Monitoring + Alerting Service

Poora AWS ka "CCTV + Alarm System" hai!

Har cheez track karta hai:
→ EC2 CPU kitna use ho raha hai
→ Lambda kitni baar fail hui
→ API Gateway pe kitne requests aaye
→ DynamoDB pe kitne reads hue

Aur agar kuch galat hua →
Turant alert bhejta hai! ✅
```

---

## Analogy — Hospital ICU

```
Patient     = AWS Services (EC2, Lambda, DynamoDB)

ICU Monitor = CloudWatch Metrics
→ Heartbeat     = CPU Utilization
→ Blood Pressure = Memory Usage
→ Temperature    = Error Rate

Alarm = CloudWatch Alarm
→ Heartbeat 0 → BEEP BEEP! Nurse alert! 🚨

Doctor Notes = CloudWatch Logs
→ "10:32 PM — Request aaya"
→ "10:45 PM — Error hua — Database connection failed"

Dashboard = Sab patients ki status ek screen pe ✅
```

---

## CloudWatch ke 4 Main Parts

### 1. Metrics
```
Numbers jo AWS services automatically collect karti hain

Examples:
EC2       → CPUUtilization, NetworkIn, NetworkOut
Lambda    → Errors, Duration, Invocations, Throttles
API GW    → 4XXError, 5XXError, Count
DynamoDB  → ConsumedReadCapacityUnits
SQS       → NumberOfMessagesSent
```

### 2. Alarms
```
"Agar metric X level cross kare → Action lo!"

Examples:
→ Lambda Errors > 5   → SNS → Email bhejo
→ EC2 CPU > 80%       → Auto Scaling trigger karo
→ SQS Queue > 1000    → Alert bhejo

Alarm States:
OK                = Sab theek hai ✅
IN ALARM          = Threshold cross ho gayi 🚨
INSUFFICIENT DATA = Data abhi aana shuru nahi hua
```

### 3. Logs
```
Application ke andar kya ho raha hai — line by line

Lambda mein print kiya:
print("Order received: ORD-001")
print("Payment processed: Rs 200")
print("ERROR: Database connection failed!")

→ CloudWatch Logs mein sab save hota hai ✅
→ Bug dhundna easy ho jaata hai ✅

Bina logs ke:
"Error aaya" — kahan? kyun? pata nahi! ❌
```

### 4. Dashboards
```
Sab metrics ek jagah graph mein!
Real-time monitoring ✅

Example Dashboard:
┌─────────────────────────────────────┐
│ Lambda Errors     [Graph] ✅        │
│ EC2 CPU Usage     [Graph] ✅        │
│ API Gateway Calls [Graph] ✅        │
│ DynamoDB Reads    [Graph] ✅        │
└─────────────────────────────────────┘
```

---

## CloudWatch Alarms — Kaise Kaam Karta Hai

```
Metric → Threshold set karo → Action define karo

Flow:
Lambda Errors > 5 (in 5 minutes)
        ↓
CloudWatch Alarm → IN ALARM state
        ↓
SNS Topic trigger
        ↓
Email → akashpatil6269@gmail.com 📧
        ↓
Engineer ne dekha → Fix kiya ✅
```

---

## Hands-On — Aaj Kya Kiya

### CloudWatch Alarm Banaya
```
Metric    : Lambda → Across All Functions → Errors
Condition : Greater than 5
Period    : 5 minutes
```

### Action Set kiya
```
SNS Topic : akash-lambda-alerts (naya banaya)
Email     : akashpatil6269@gmail.com
```

### SNS Subscription Confirm kiya
```
Gmail pe AWS ka email aaya
"Confirm subscription" link click kiya
Status: Confirmed ✅
```

### Alarm Ready
```
Alarm Name: akash-lambda-error-alarm

Ab jab bhi Lambda 5 se zyada baar fail ho
→ 5 minutes mein
→ CloudWatch detect karega
→ SNS → Gmail pe alert aayega ✅
```

### Cleanup
```
✅ CloudWatch Alarm deleted
✅ SNS Topic deleted
✅ Old RDS alarm + topic deleted
→ Bill zero! ✅
```

---

## Interview Questions & Answers

**Q1. What is Amazon CloudWatch and what are its main components?**

Amazon CloudWatch is a monitoring and observability service for AWS resources and applications. It collects and tracks metrics, monitors log files, sets alarms, and automatically reacts to changes in AWS resources. The four main components are Metrics, which are numerical data points collected from AWS services like CPU utilization or error counts; Alarms, which watch a metric and trigger actions when a threshold is crossed; Logs, which capture detailed application and system log data for debugging; and Dashboards, which provide a unified view of multiple metrics and alarms in a single graphical interface.

---

**Q2. How does a CloudWatch Alarm work?**

A CloudWatch Alarm watches a single metric over a specified time period and performs one or more actions based on the value relative to a threshold. You define three things: the metric to watch such as Lambda Errors, the condition such as greater than 5, and the action to take such as sending an SNS notification. The alarm has three states — OK when the metric is within the threshold, IN ALARM when the threshold is breached, and INSUFFICIENT DATA when not enough data is available. When the alarm enters the IN ALARM state, it triggers the configured action, which can be sending an email via SNS, triggering Auto Scaling, or stopping an EC2 instance.

---

**Q3. What is the difference between CloudWatch Metrics and CloudWatch Logs?**

CloudWatch Metrics are numerical data points collected at regular intervals that represent the performance of AWS resources. They are used for monitoring trends and triggering alarms. Examples include EC2 CPU utilization percentage or Lambda function error count. CloudWatch Logs capture the actual text output from applications and AWS services, providing detailed information about what happened inside the application. While metrics answer "how much" or "how many," logs answer "what exactly happened" and "why did it fail." Logs are essential for debugging because they contain the actual error messages and application output line by line.

---

**Q4. How would you set up monitoring for a serverless application using CloudWatch?**

For a serverless application using API Gateway, Lambda, and DynamoDB, I would set up CloudWatch monitoring as follows. First, create alarms on Lambda Errors and Lambda Duration to detect failures and performance issues. Second, create an alarm on API Gateway 5XX errors to catch server-side failures reaching users. Third, enable Lambda CloudWatch Logs to capture all function output for debugging. Fourth, set up a CloudWatch Dashboard showing all key metrics in one view for quick status checks. Finally, connect all alarms to an SNS topic that sends email notifications to the team so they are alerted immediately when any threshold is breached.

---

## Key Points — Phone Pe Save Karo

```
CloudWatch = AWS ka CCTV + Alarm System

4 Parts:
1. Metrics    = Numbers track karo (CPU, Errors)
2. Alarms     = Threshold cross → Action lo
3. Logs       = Line by line kya hua (debugging)
4. Dashboards = Sab ek jagah graph mein

Alarm States:
OK               = Sab theek ✅
IN ALARM         = Problem! 🚨
INSUFFICIENT DATA = Data nahi hai

Alarm Flow:
Metric → Threshold → SNS → Email → Engineer

Common Metrics:
EC2     → CPUUtilization
Lambda  → Errors, Duration, Invocations
API GW  → 5XXError, Count
DynamoDB → ConsumedReadCapacityUnits

FREE TIER:
10 alarms free ✅
1M API requests free ✅
5 GB logs free ✅
```
