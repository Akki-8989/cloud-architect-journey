# Day 37 — API Gateway

## Problem — Lambda Directly Expose Nahi Kar Sakte

```
Lambda function ready hai lekin:
→ Lambda ka koi public URL nahi hota ❌
→ User browser/app seedha call nahi kar sakta ❌
→ Business logic expose ho jaati ❌
→ Security nahi hoti ❌
```

---

## Solution — API Gateway

```
API Gateway = AWS ka Fully Managed API Service

User/App → API Gateway → Lambda → Response wapas

API Gateway = Public Door
Jo internet aur Lambda ke beech mein khada hai
```

---

## Analogy — Restaurant Reception

```
Tum seedha kitchen mein nahi jaate ❌
Tum reception pe jaate ho → Order dete ho →
Reception kitchen ko batata hai →
Kitchen ne banaya → Reception ne tumhe diya ✅

API Gateway = Reception
Lambda      = Kitchen
User/App    = Customer
```

---

## API Gateway Kya Karta Hai

```
1. PUBLIC URL deta hai:
   https://abc123.execute-api.ap-south-1.amazonaws.com/dev

2. HTTP Methods handle karta hai:
   GET    → Data lao    (orders list)
   POST   → Data bhejo  (order place karo)
   PUT    → Update karo (order modify)
   DELETE → Delete karo (order cancel)

3. Sahi Lambda pe route karta hai:
   GET  /orders → ListOrders Lambda
   POST /orders → CreateOrder Lambda

4. Security handle karta hai:
   → Auth check (koi bhi call na kar sake)
   → Rate limiting (ek user 1000 calls/min se zyada nahi)

5. Response wapas bhejta hai:
   Lambda ka result → User ko JSON mein
```

---

## Bina API Gateway — Problems

```
1. Business Logic expose hoti
   Lambda ARN public → Code structure pata chalta ❌

2. Security nahi hoti
   Koi bhi, kahin se bhi call kar sakta ❌
   Rate limiting nahi → Server crash ❌

3. Scaling control nahi hota
   API Gateway throttle karta hai automatically ✅
   Direct expose = No control ❌
```

---

## Complete Serverless Architecture

```
Mobile App / Browser
        ↓
   API Gateway     ← Public URL + Security + Routing
        ↓
   AWS Lambda      ← Business Logic (code)
        ↓
   DynamoDB / S3   ← Data Store

Koi server nahi!
Koi EC2 nahi!
Sab managed ✅
```

---

## Real Zomato Example

```
App pe "Order Place" button dabaya
        ↓
POST https://api.zomato.com/orders
        ↓
API Gateway ne receive kiya
        ↓
CreateOrder Lambda trigger hua
        ↓
DynamoDB mein order save hua
        ↓
Response: { "orderId": "123", "status": "placed" }
        ↓
App pe "Order Placed!" dikha ✅
```

---

## API Gateway ke Types

```
REST API      → Standard HTTP APIs (most common) ✅
HTTP API      → Faster + Cheaper (simple use cases)
WebSocket API → Real-time (chat apps, live tracking)
```

---

## Stages — Kya Hota Hai

```
Stage = Deployment environment

dev  → Development (testing ke liye)
prod → Production (real users ke liye)

URL mein stage dikh ta hai:
https://abc123.execute-api.ap-south-1.amazonaws.com/dev
https://abc123.execute-api.ap-south-1.amazonaws.com/prod
```

---

## Hands-On — Aaj Kya Kiya

### Step 1 — Lambda Banaya
```
Name    : akash-api-demo
Runtime : Python 3.12

Code:
import json
def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': json.dumps({
            'message': 'Hello from API Gateway!',
            'name': 'Akash'
        })
    }
```

### Step 2 — API Gateway Banaya
```
Type : REST API
Name : akash-api-demo
```

### Step 3 — GET Method Create kiya
```
Method type      : GET
Integration type : Lambda function
Lambda function  : akash-api-demo
```

### Step 4 — Deploy kiya
```
Stage name : dev

Invoke URL:
https://h2nf074elc.execute-api.ap-south-1.amazonaws.com/dev
```

### Step 5 — Live Test kiya
```
Browser mein URL open kiya → Response aaya:

{
  "statusCode": 200,
  "body": "{\"message\": \"Hello from API Gateway!\", \"name\": \"Akash\"}"
}

API Gateway → Lambda trigger → Response ✅
Pure Serverless LIVE! ✅
```

### Cleanup
```
✅ API Gateway deleted
✅ Lambda deleted
→ Bill zero! ✅
```

---

## Interview Questions & Answers

**Q1. What is Amazon API Gateway and why is it needed?**

Amazon API Gateway is a fully managed service that allows developers to create, publish, and manage APIs at any scale. It acts as a front door for applications to access data, business logic, or functionality from backend services such as AWS Lambda. API Gateway is needed because Lambda functions do not have a public URL by default and cannot be accessed directly from the internet. API Gateway provides a public HTTPS endpoint, handles HTTP methods like GET and POST, manages authentication and authorization, and implements rate limiting to prevent abuse.

---

**Q2. What happens when a user calls an API Gateway endpoint?**

When a user sends an HTTP request to an API Gateway endpoint, API Gateway first receives the request and validates it against any configured authentication or authorization settings. It then routes the request to the appropriate backend integration, which is typically a Lambda function. The Lambda function executes and returns a response to API Gateway. API Gateway then formats and returns that response to the original caller. This entire flow is serverless — no EC2 instances or servers need to be managed.

---

**Q3. What is the difference between REST API and HTTP API in API Gateway?**

REST API is the original API Gateway offering with full features including request and response transformation, API keys, usage plans, and detailed logging. HTTP API is a newer, simplified version that is faster and approximately 70 percent cheaper than REST API. HTTP API supports core features like Lambda integration, JWT authorization, and CORS, but lacks some advanced features like request transformation. For most modern use cases, HTTP API is preferred due to lower cost and lower latency. REST API is chosen when advanced features like request/response mapping or API keys are required.

---

**Q4. What is a Stage in API Gateway?**

A stage in API Gateway represents a snapshot of the API deployment at a specific point in time. Stages allow you to maintain multiple versions of your API simultaneously, such as a dev stage for development and testing and a prod stage for production traffic. Each stage has its own URL endpoint, so developers can test new features in the dev stage without affecting production users. Stages can also have different configurations such as throttling limits, logging settings, and caching behavior.

---

## Key Points — Phone Pe Save Karo

```
API Gateway = AWS ka Public Door for Lambda

Bina API Gateway:
→ Lambda ka public URL nahi ❌
→ Business logic expose ❌
→ Security nahi ❌

API Gateway ke saath:
→ Public HTTPS URL milta hai ✅
→ GET/POST/PUT/DELETE handle ✅
→ Auth + Rate limiting ✅
→ Lambda se connect ✅

Types:
REST API    = Full featured (most common)
HTTP API    = Fast + Cheap (simple use)
WebSocket   = Real-time (chat/live)

Stage = Environment (dev/prod)
URL: .../dev  ya  .../prod

Complete Serverless Stack:
API Gateway + Lambda + DynamoDB
= No servers, No EC2, Pay per request ✅

FREE TIER:
1M API calls/month free ✅
```
