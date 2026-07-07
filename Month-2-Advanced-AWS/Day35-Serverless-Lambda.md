# Day 35 — Serverless & AWS Lambda

## Problem — EC2 ka Issue

```
Normal way (EC2):
→ Server 24/7 chalta hai
→ Traffic ho ya na ho — bill aa raha hai!
→ Raat 3 baje 0 users → Server chal raha hai → Paisa waste! ❌
→ Traffic badha → Manually scale karo ❌
→ OS patches → Tumhe karna hai ❌
```

---

## Solution — Serverless

```
Serverless = Server manage mat karo!
             Code likho → AWS run karega →
             Kaam khatam → Server band!

Bill sirf tab aata hai jab code RUN hota hai!
Raat 3 baje 0 requests → $0.00 ✅
```

---

## AWS Lambda Kya Hai

```
Lambda = AWS ka Serverless Compute Service

Tum sirf CODE likhte ho (function):
→ Server provision nahi karna ❌
→ OS manage nahi karna ❌
→ Scaling manually nahi karna ❌

AWS sab handle karta hai! ✅
```

---

## EC2 vs Lambda — App Structure

```
EC2 way (Monolithic):
Ek bada server → Sab ek saath:
├── Login
├── Payment
├── Image resize
├── Email send
└── Reports
24/7 ON → 24/7 bill ❌

Lambda way (Microservices):
Lambda 1 → sirf LOGIN ka code
Lambda 2 → sirf PAYMENT ka code
Lambda 3 → sirf IMAGE RESIZE ka code
Lambda 4 → sirf EMAIL ka code
Lambda 5 → sirf REPORTS ka code

Har function sirf tab chale jab zaroorat ho! ✅
```

---

## Kaise Kaam Karta Hai

```
Event aaya → Lambda trigger hua → Code run hua → Done!

Example:
S3 mein image upload hui (EVENT)
    ↓
Lambda trigger hua
    ↓
Image resize kiya (CODE)
    ↓
Naya image S3 mein save kiya
    ↓
Lambda band ho gaya! ✅
```

---

## Analogy — Motion Sensor Light

```
EC2 = Purana fan
      Switch OFF karo → Fan chalta rahta hai!
      (24/7 bill) ❌

Lambda = Motion sensor light
         Koi aaya → Light ON ✅
         Koi nahi → Light OFF → Bill zero! ✅

Lambda = Motion sensor compute!
```

---

## Lambda ke Fayde

```
1. Cost:
   EC2    = 24/7 bill (use karo ya na karo)
   Lambda = Sirf execution time ka bill!
   1M requests/month = $0.20 (almost FREE!) ✅

2. Auto Scaling:
   1 request  → 1 Lambda instance
   1M requests → 1M Lambda instances (automatic!)
   No configuration needed! ✅

3. No Server Management:
   OS patches → AWS karta hai ✅
   Scaling    → AWS karta hai ✅
   Tum sirf code likho! ✅
```

---

## Lambda Triggers

```
API Gateway  → HTTP request aaya → Lambda run karo
S3           → File upload hui → Lambda run karo
DynamoDB     → Data change hua → Lambda run karo
CloudWatch   → Schedule (har 5 min) → Lambda run karo
SNS/SQS      → Message aaya → Lambda run karo
EventBridge  → AWS event aaya → Lambda run karo
```

---

## Lambda Important Limits

```
Timeout     : Max 15 minutes (default 3 sec)
Memory      : 128 MB → 10 GB
Package size: 50 MB (zip) / 250 MB (unzipped)
Languages   : Python, Node.js, Java, Go, Ruby, C#

Agar 15 min se zyada kaam hai → EC2 use karo!
```

---

## EC2 vs Lambda — Kab Kya Use Karein

```
EC2:
→ 24/7 running app (web server)
→ Long running processes (hours)
→ Custom OS/software needed
→ Constant predictable traffic

Lambda:
→ Event-driven tasks (image resize, file processing)
→ Short tasks (max 15 minutes!)
→ Unpredictable/spiky traffic
→ Microservices architecture
→ Scheduled tasks (cron jobs)
```

---

## Architect ka Role vs Developer ka Role

```
Developer:
→ Lambda function ka CODE likhta hai
→ Business logic implement karta hai

Architect:
→ CODE nahi likhta
→ YE decide karta hai:
   ✅ Lambda kab use karein (EC2 vs Lambda)
   ✅ Trigger kya hoga
   ✅ Timeout kitna chahiye
   ✅ Memory kitni chahiye
   ✅ Cost estimate
   ✅ Security (IAM role)
   ✅ Other services se connection

"Yahan Lambda use karo EC2 nahi" = Architect ka kaam! ✅
```

---

## Hands-On — Aaj Kya Kiya

### Lambda Function Banaya
```
Lambda → Create function

Settings:
→ Author from scratch
→ Name   : akash-hello-lambda
→ Runtime: Python 3.12

Auto-generated code:
import json

def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': json.dumps('Hello from Lambda!')
    }
```

### Test kiya
```
Test → Event name: akash-test → Test button

Result:
{
  "statusCode": 200,
  "body": "Hello from Lambda!" ✅
}

Summary:
Duration       : 1.94 ms ← Ultra fast!
Billed duration: 82 ms
Memory used    : 36 MB

Lambda LIVE! Code chala → Result aaya → Band! ✅
```

### Cleanup
```
✅ Lambda function deleted
→ Bill zero! ✅
```

---

## Interview Questions & Answers

**Q1. What is AWS Lambda and how is it different from EC2?**

AWS Lambda is a serverless compute service that lets you run code without provisioning or managing servers. You simply upload your code as a function, and Lambda runs it in response to events such as an HTTP request, file upload to S3, or a scheduled time. The key difference from EC2 is that with EC2 you pay for the server whether it is processing requests or sitting idle, while with Lambda you only pay for the actual execution time — measured in milliseconds. Lambda also automatically scales to handle any number of concurrent requests without any configuration.

---

**Q2. What is an event trigger in Lambda?**

An event trigger is what causes a Lambda function to execute. Lambda is event-driven, meaning it only runs when something specific happens. Common triggers include API Gateway for HTTP requests, S3 for file uploads or deletions, DynamoDB for database changes, CloudWatch Events for scheduled tasks like cron jobs, and SNS or SQS for message processing. When the trigger event occurs, Lambda automatically invokes the function, runs the code, and then shuts down. This event-driven model is what makes Lambda cost-effective since the function only runs when needed.

---

**Q3. What are the limitations of AWS Lambda?**

AWS Lambda has several important limitations that architects must consider. The maximum execution timeout is 15 minutes, so any process that takes longer must use EC2 or other services. Memory can be configured between 128 MB and 10 GB. The deployment package size is limited to 50 MB for zip files. Lambda has a cold start problem where the first invocation after a period of inactivity may be slower because AWS needs to initialize the execution environment. Lambda is not suitable for applications that require persistent connections, custom operating systems, or processes that run continuously.

---

**Q4. When would you choose Lambda over EC2?**

Lambda is the better choice for event-driven workloads where code runs in response to specific triggers, short-duration tasks that complete within 15 minutes, applications with unpredictable or spiky traffic where paying for idle EC2 capacity would be wasteful, microservices architectures where different functions handle different concerns, and scheduled tasks like generating nightly reports or cleanup jobs. EC2 is preferable for applications that run continuously, require specific operating system configurations, need persistent connections, or have long-running processes exceeding 15 minutes.

---

## Key Points — Phone Pe Save Karo

```
Lambda = Serverless Compute
       = Event aaya → Code chala → Band!
       = Sirf execution time ka bill!

Triggers:
API Gateway, S3, DynamoDB, CloudWatch,
SNS, SQS, EventBridge

Limits:
→ Timeout: Max 15 minutes
→ Memory: 128MB - 10GB
→ Languages: Python, Node.js, Java, Go, Ruby, C#

Cost:
→ 1M requests = $0.20 (almost FREE!)
→ 400,000 GB-seconds FREE per month

EC2 vs Lambda:
→ 24/7 app = EC2
→ Event-driven, short tasks = Lambda

Architect ka kaam:
→ Code nahi likhna
→ KAHAN Lambda use karein = Ye decide karna! ✅
```
