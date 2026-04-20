# Day 15 — AWS Lambda (Serverless Computing)

## Serverless Ka Matlab

"Serverless" ka matlab ye nahi ki server nahi hoga. **Server toh hoga** — but **tumhe manage nahi karna padega!**

```
EC2 (Normal Server):
Tum ek flat khareedte ho
Bijli, paani, maintenance — sab tumhara kaam
24/7 pay karo — chahe koi aaye ya na aaye

Lambda (Serverless):
Tum ek hotel mein rehte ho
Koi kaam nahi — hotel wala sab karta hai
Sirf jab rehte ho tab pay karo — baaki time free!
```

---

## AWS Lambda Kya Hai

Lambda AWS ka **Serverless Compute** service hai. Tum sirf code likhte ho — baaki sab AWS handle karta hai.

```
Tumhara kaam  = Sirf code likhna
AWS ka kaam   = Server manage, scale, maintain sab kuch

Pay karo      = Sirf jab code RUN hoga
0 requests    = Rs. 0 cost!
```

**Developer Analogy:** Lambda = ek function jo event pe automatically call hoti hai — jaise JavaScript ka event listener!

```javascript
// Ye tumhara Lambda hai essentially:
button.onClick(() => {
    // kuch karo
})
```

---

## EC2 vs Lambda — Kab Kaunsa Use Karein

```
EC2 Use Karo Jab:
→ 24/7 server chahiye (long running process)
→ Full control chahiye (OS, software customize)
→ Heavy compute — ML models, video processing
→ Legacy application jo server pe hi chalti ho

Lambda Use Karo Jab:
→ Short tasks hain (max 15 minutes)
→ Event pe trigger ho
→ Unpredictable traffic — kabhi 0, kabhi 1000 requests
→ Cost bachana ho — sirf run hone pe pay karo
```

---

## Lambda Ke Key Concepts

### 1. Function
Tumhara actual code jo Lambda run karta hai.

```python
def lambda_handler(event, context):
    # tumhara code yahan
    return response
```

- `event` = Jo data aaya trigger ke saath (input)
- `context` = Lambda environment ki info
- `return` = Response jo wapas bheja

---

### 2. Trigger — Lambda Kab Run Hoga
Lambda khud se nahi chalta — koi **event trigger** karta hai.

```
S3 Trigger      → Image upload hui → Lambda run → Thumbnail banao
API Gateway     → HTTP request aaya → Lambda run → Response do
CloudWatch      → Schedule (har raat 12 baje) → Lambda run → Report banao
SNS             → Notification aayi → Lambda run → Email bhejo
DynamoDB        → Data change hua → Lambda run → Process karo
```

---

### 3. Runtime — Kaunsi Language
Lambda multiple languages support karta hai:

```
Python    → python3.12
Node.js   → nodejs20.x
Java      → java17
Go        → go1.x
.NET      → dotnet8
Ruby      → ruby3.2
```

---

### 4. Cold Start vs Warm Start

```
Cold Start (Pehli Baar):
Lambda request aayi → AWS container start kiya → Code run hua
Extra time lagta hai (50-500ms)
Kyun: AWS ne naaya container initialize kiya

Warm Start (Agli Baar):
Lambda request aayi → Container already ready → Code run hua
Super fast (1-10ms)
Kyun: Container pehle se chal raha tha
```

**Production mein:** High traffic apps mein cold start problem nahi hoti kyunki containers hamesha warm rehte hain.

---

### 5. Lambda Limits

```
Max execution time : 15 minutes
Default timeout    : 3 seconds
Memory             : 128 MB to 10 GB
Package size       : 50 MB (zip), 250 MB (unzipped)
Free tier          : 1 million requests/month FREE
                     400,000 GB-seconds FREE
```

---

## Lambda Pricing — Kya Pay Karna Padega

```
Free Tier (hamesha free):
→ 1,000,000 requests/month FREE
→ 400,000 GB-seconds compute FREE

Uske baad:
→ $0.20 per million requests
→ $0.0000166667 per GB-second
```

**Simple matlab:** Tumhare small projects ke liye Lambda practically **FREE** hai!

---

## Real World Use Cases

```
Image Processing  → User ne photo upload ki → Lambda → Resize/compress karo
Email Service     → Order place hua → Lambda → Confirmation email bhejo
Scheduled Tasks   → Har din subah → Lambda → Database cleanup karo
API Backend       → Mobile app se request → Lambda → Data return karo
Notifications     → Payment failed → Lambda → Alert bhejo
```

---

## Hands-On — Aaj Kya Kiya

### Step 1 — Lambda Function Banaya
```
Lambda → Create function

Function name : my-first-lambda
Runtime       : Python 3.12
Architecture  : x86_64
→ Create function
```
**Kyun:** Python 3.12 select kiya kyunki simple aur readable hai. AWS automatically IAM role bhi banata hai function ke liye.

### Step 2 — Code Likha
```python
import json

def lambda_handler(event, context):
    name = event.get('name', 'World')
    message = f"Hello, {name}! Ye Akash ka pehla Lambda function hai!"
    
    return {
        'statusCode': 200,
        'body': json.dumps(message)
    }
```
**Kyun:** `event.get('name', 'World')` — event mein naam aaya toh use karo, nahi aaya toh 'World' default. `statusCode: 200` — HTTP success response.

### Step 3 — Deploy Kiya
```
Code editor → Deploy button click kiya
```
**Kyun:** Code likhne ke baad Deploy karna zaroori hai — tabhi changes save hote hain aur function update hota hai.

### Step 4 — Test Event Banaya aur Run Kiya
```
Test tab → Create new test event

Event name : my-test-event
Event JSON :
{
  "name": "Akash"
}

→ Save → Invoke
```
**Kyun:** Test event se hum manually Lambda ko trigger karte hain aur dekh sakte hain ki code sahi kaam kar raha hai ya nahi.

### Step 5 — Result
```
Status   : Succeeded ✅
Response :
{
  "statusCode": 200,
  "body": "Hello, Akash! Ye Akash ka pehla Lambda function hai!"
}

Duration      : 1.89 ms  (code run time)
Billed        : 87 ms    (init + run time)
Memory used   : 36 MB
Init Duration : 84.29 ms (cold start — pehli baar)
```

**Cold Start explain hua:** 84ms init time — AWS ne pehli baar container start kiya. Agli baar sirf 1.89ms lagega.

---

## Lambda Execution Log Samjho

```
START RequestId: xxx  Version: $LATEST
→ Lambda start hui

END RequestId: xxx
→ Lambda khatam hui

REPORT RequestId: xxx
Duration: 1.89 ms        → Actual code run time
Billed Duration: 87 ms   → AWS charge karega itne ka
Memory Size: 128 MB      → Allocated memory
Max Memory Used: 36 MB   → Actually use hua
Init Duration: 84.29 ms  → Cold start time
```

---

## Interview Questions & Answers

**Q1. What is AWS Lambda and what does "serverless" mean?**

AWS Lambda is a serverless compute service that lets you run code without provisioning or managing servers. Serverless does not mean there are no servers — it means you do not have to manage them. AWS handles all the infrastructure including server provisioning, scaling, patching, and maintenance. You simply write your code, upload it to Lambda, and it runs in response to events. You only pay for the compute time your code actually uses, measured in milliseconds, with no charges when your code is not running.

---

**Q2. What is a Lambda trigger and give three examples?**

A Lambda trigger is an event source that automatically invokes a Lambda function when a specific event occurs. Examples include: an S3 trigger where uploading a file to an S3 bucket automatically triggers a Lambda function to process or resize the image; an API Gateway trigger where an HTTP request to a REST API endpoint invokes Lambda to process the request and return a response; and a CloudWatch Events trigger where a scheduled rule (like a cron job) triggers Lambda at specific times, such as running a database cleanup job every night at midnight. Triggers make Lambda event-driven, meaning the function only runs when needed.

---

**Q3. What is a Cold Start in Lambda and why does it matter?**

A Cold Start occurs when Lambda receives a request but does not have a warm container ready to execute the code. AWS needs to initialize a new execution environment, load the runtime, and set up the function before running the code. This adds latency, typically between 50 to 500 milliseconds depending on the runtime and package size. After the first invocation, AWS keeps the container warm for a period of time, so subsequent requests experience a Warm Start with much lower latency. Cold starts matter in latency-sensitive applications like real-time APIs. Solutions include keeping functions warm with scheduled pings, using Provisioned Concurrency, or choosing faster runtimes like Node.js or Python over Java.

---

**Q4. When would you choose Lambda over EC2?**

Lambda is the better choice when you have event-driven workloads that run for short durations, unpredictable or intermittent traffic patterns, or when you want to minimize operational overhead. For example, processing images after upload, sending notification emails, or running scheduled cleanup jobs. EC2 is better when you need long-running processes that exceed Lambda's 15-minute timeout, require specific operating system configuration, need persistent connections like a continuously running web server, or involve heavy compute workloads like machine learning inference. The key question is: does your workload respond to events and run briefly, or does it need to run continuously?

---

**Q5. How does Lambda pricing work and why is it cost-effective?**

Lambda pricing is based on two dimensions: the number of requests and the duration of execution. AWS provides a permanent free tier of one million requests per month and 400,000 GB-seconds of compute time per month at no charge. Beyond that, you pay approximately $0.20 per million requests and a small amount per GB-second of execution time. This model is extremely cost-effective because you pay only when your code actually runs — there are no charges for idle time. Compared to EC2 where you pay for the server whether it is processing requests or sitting idle, Lambda can result in significant cost savings for workloads with variable or low traffic.

---

## Key Points — Phone Pe Save Karo

```
Lambda       = Serverless compute — code run karo, server manage mat karo
Serverless   = Server hoga but tumhe manage nahi karna
Trigger      = Event jo Lambda ko invoke karta hai
Event        = Input data jo trigger ke saath aata hai
Cold Start   = Pehli baar container initialize — thoda slow
Warm Start   = Container ready — super fast
Runtime      = Language (Python, Node.js, Java, .NET)
Deploy       = Code save karo Lambda mein
Free Tier    = 1 million requests/month FREE
Max Timeout  = 15 minutes
lambda_handler = Function ka entry point — hamesha yahi naam
```
