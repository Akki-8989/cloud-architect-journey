# Day 40 — Revision Day (Day 35 to 39)

## Topics Revised Today

```
Day 34 - CloudFront  → TEACH Level  ✅
Day 33 - Route 53    → TEACH Level  ✅
Day 37 - API Gateway → APPLY Level  ✅
Day 35 - Lambda      → APPLY Level  ✅
Day 36 - SQS + SNS   → APPLY Level  ✅
```

---

## CloudFront — TEACH Level

**What is it:**
CloudFront is AWS's CDN (Content Delivery Network) service.
It stores content at 400+ Edge Locations worldwide so users
get data from the nearest server instead of the origin.

**Cache Miss vs Cache Hit:**
```
Cache Miss = First time request
→ Edge location doesn't have the content
→ Fetches from origin (S3/EC2)
→ Stores it at the edge
→ Serves to user (slow, once only)

Cache Hit = Second time onwards
→ Edge location already has the content
→ Serves directly (fast!) ✅
```

**Why CloudFront + S3 together:**
```
S3 stores the files (images, HTML, videos)
CloudFront serves them from 400+ edge locations globally
→ Users worldwide get fast access
→ Origin (Mumbai S3) not hit every time ✅
```

---

## Route 53 — TEACH Level

**What is it:**
Route 53 is AWS's DNS service.
It translates domain names to IP addresses.
(Internet ka phone book)

**Hosted Zone:**
```
Container for all DNS records of a domain.
1 Domain = 1 Hosted Zone

Public  = Routes traffic on internet
Private = Routes traffic inside VPC only
```

**5 Routing Policies:**
```
1. Simple      = 1 domain → 1 server (basic)
2. Weighted    = Split traffic by % (70/30 A/B testing)
3. Latency     = Send user to nearest/fastest server
4. Failover    = Primary down → switch to backup (DR)
5. Geolocation = User's location → their region's server
```

---

## API Gateway — APPLY Level

**Problem solved:**
Lambda has no public URL.
API Gateway provides the public HTTPS door.

**Correct Serverless Flow:**
```
Customer → API Gateway → Lambda → DynamoDB
                                      ↓
                                    SNS → Customer notification ✅
```

**Key rule:**
```
API Gateway = Door (receives request)
Lambda      = Brain (processes request)
DynamoDB    = Memory (stores data)
```

---

## Lambda — APPLY Level

**Image Resize Scenario:**
```
Customer uploads photo
        ↓
S3 Bucket (stores the image)
        ↓ S3 triggers Lambda automatically
Lambda runs → creates 3 sizes:
→ Thumbnail (50x50)
→ Profile   (200x200)
→ Full      (800x800)
        ↓
All 3 saved back to S3
        ↓
SNS → "Profile photo updated!" ✅
```

**Key rule:**
```
Images/Files/Videos → S3
Text/Data/JSON      → DynamoDB
Code/Processing     → Lambda
```

---

## SQS + SNS — APPLY Level

**10,000 Orders Spike Scenario:**
```
10,000 orders arrived at once (New Year's Eve!)
Direct → Payment Service = CRASH ❌

Solution with SQS:
10,000 Orders
        ↓
SQS Queue (all stored safely)
        ↓
Payment Service picks them one by one
at its own pace ✅
        ↓
SNS → "Payment Successful!" to customer ✅

No crash! No lost orders! ✅
```

---

## Key Takeaways

```
1. CloudFront = Cache at edge, serve fast globally
2. Route 53   = DNS + 5 routing policies
3. API Gateway = Public door for Lambda
4. Lambda     = Event-driven, S3 trigger for files
5. SQS        = Buffer for traffic spikes (crash prevention)
6. SNS        = Broadcast notification after processing

Combined Serverless Stack:
API Gateway → Lambda → DynamoDB
S3 → Lambda → S3 (file processing)
SQS → Lambda → DynamoDB (queue processing)
```

---

## 1-4-7 Revision Status After Today

```
Day 34 CloudFront  → TEACH ✅ Complete
Day 33 Route 53    → TEACH ✅ Complete
Day 37 API Gateway → APPLY ✅ Complete
Day 36 SQS + SNS   → APPLY ✅ Complete
Day 35 Lambda      → APPLY ✅ Complete

Pending:
Day 35 Lambda      → TEACH (due: 2026-07-14)
Day 36 SQS+SNS     → TEACH (due: 2026-07-15)
Day 37 API Gateway → TEACH (due: 2026-07-16)
Day 38 DynamoDB    → APPLY (due: 2026-07-14)
Day 39 CloudWatch  → RECALL (due: 2026-07-12) ← missed, do soon
```
