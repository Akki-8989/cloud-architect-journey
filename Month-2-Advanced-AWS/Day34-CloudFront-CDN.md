# Day 34 — CloudFront (CDN Service)

## CloudFront Kya Hai

```
CloudFront = AWS ka CDN (Content Delivery Network) Service

Problem:
Server sirf Mumbai mein hai
→ India user    → 10ms ✅
→ USA user      → 200ms ❌
→ Australia     → 250ms ❌

Solution:
Content duniya bhar mein PAAS PAAS copy kar do!
User ke nearest copy se serve karo!
```

---

## Edge Locations

```
CloudFront ke paas 400+ Edge Locations hain
(Mini servers / cache servers duniya bhar mein)

Mumbai    ← Edge Location ✅
New York  ← Edge Location ✅
London    ← Edge Location ✅
Sydney    ← Edge Location ✅
Tokyo     ← Edge Location ✅

Har major city mein ek Edge Location!
```

---

## Kaise Kaam Karta Hai — Cache Miss vs Hit

```
Pehli baar (Cache MISS):
USA user → CloudFront New York →
           "Content nahi hai mere paas" →
           Mumbai S3 se fetch kiya →
           New York Edge pe CACHE kiya →
           USA user ko diya ✅ (thoda slow - once only)

Doosri baar (Cache HIT):
USA user → CloudFront New York →
           "Content mere paas hai!" →
           Direct serve! ✅ (200ms → 5ms!)

Cache = Save karke rakhna (fast delivery ke liye)
```

---

## Analogy — Zomato Dark Store

```
Bina CloudFront:
Order kiya → Central kitchen (Mumbai) → 1 ghante mein ❌

CloudFront ke saath:
Order kiya → Paas ka dark store (2km) → 10 min ✅

CloudFront = AWS ka Dark Store Network!
Content pehle se paas mein ready rakho!
```

---

## CloudFront ke Fayde

```
1. Speed:
   Mumbai → USA: 200ms ❌
   New York Edge → USA: 5ms ✅ (40x faster!)

2. Cost Saving:
   95% requests → Edge (cached) → Origin pe load kam
   Sirf 5% → Mumbai (cache miss) → Bill kam! ✅

3. Security:
   CloudFront + WAF = Best combo!
   WAF Edge pe attach → Attack Edge pe block ✅
   DDoS protection built-in ✅

4. Static + Dynamic:
   Static  → Images, CSS, JS, Videos (cache karo)
   Dynamic → API responses (origin se fresh)
```

---

## CloudFront Architecture

```
Users (Worldwide)
      ↓
┌──────────────────────────────────────────┐
│        CloudFront Edge Locations         │
│  🖥️ Mumbai  🖥️ NY  🖥️ London  🖥️ Sydney  │
│      (Cache hit → Direct serve!)         │
└─────────────────┬────────────────────────┘
                  │ Cache Miss only!
                  ↓
           Origin Server
        (S3 Bucket / EC2 / ALB)
```

---

## CloudFront Origins — Kahan Se Content Aata Hai

```
S3 Bucket      → Static files (images, HTML, videos)
EC2 / ALB      → Dynamic web applications
API Gateway    → REST APIs
Custom Origin  → Any HTTP server
```

---

## CloudFront + S3 = Perfect Combo

```
S3 mein store karo → CloudFront se globally serve karo!

Example — Netflix:
Videos → S3 (storage)
       → CloudFront → Worldwide fast streaming ✅

Bina CloudFront: India se US movie = Buffering ❌
CloudFront ke saath: Nearest Edge → Smooth! ✅
```

---

## TTL — Time To Live

```
TTL = Content kitni der tak Edge pe cached rahe?

Default TTL = 24 hours (86400 seconds)
Custom TTL  = Tumhara choice

TTL kam = Fresh content (zyada origin requests)
TTL zyada = Old content but fast (kam origin requests)

Example:
News website  → TTL = 5 min (content jaldi change hota hai)
Product image → TTL = 7 days (rarely change hota hai)
```

---

## Cache Invalidation

```
File update ki S3 pe lekin Edge pe purani hai?

Cache Invalidation karo!
→ CloudFront ko bolo: "Ye file purni ho gayi, fresh lo!"

Example:
/index.html update kiya → Invalidation create karo
→ Sab Edge Locations pe purani cache delete ho gayi ✅
→ Next request pe fresh content aayega ✅

Cost: $0.005 per invalidation path (almost free)
```

---

## Hands-On — Aaj Kya Kiya

### Step 1 — S3 Bucket banaya
```
Bucket name: akash-cloudfront-demo
Region: Asia Pacific (Mumbai)
Public access: Blocked ✅ (CloudFront hi access karega)
```

### Step 2 — index.html upload kiya
```html
<html>
<body>
<h1>Hello from CloudFront!</h1>
<p>This content is delivered via AWS CloudFront CDN</p>
</body>
</html>
```

### Step 3 — CloudFront Distribution banaya
```
Plan: Free ($0/month) ✅
Name: akash-cloudfront-demo
Origin: akash-cloudfront-demo.s3.ap-south-1.amazonaws.com
Security: WAF enabled (free mein included!)
Price class: All edge locations

Distribution domain: duyfyku1hw9hm.cloudfront.net
Status: Deployed ✅
```

### Step 4 — LIVE Test kiya
```
URL: https://duyfyku1hw9hm.cloudfront.net/index.html

Result:
"Hello from CloudFront!"
"This content is delivered via AWS CloudFront CDN"

CDN LIVE! S3 Mumbai mein hai lekin
Edge Location se globally serve ho raha hai! ✅
```

### Cleanup (Pending)
```
❗ CloudFront Distribution → Disable → Delete
❗ S3 bucket → Empty → Delete
```

---

## Interview Questions & Answers

**Q1. What is Amazon CloudFront and how does it work?**

Amazon CloudFront is a Content Delivery Network service that speeds up the distribution of static and dynamic web content to users worldwide. CloudFront has over 400 edge locations globally. When a user requests content, CloudFront routes the request to the nearest edge location. If the content is cached at that edge location, it is served directly to the user with minimal latency. If the content is not cached, CloudFront retrieves it from the origin server such as an S3 bucket or EC2 instance, caches it at the edge location, and then serves it to the user. Subsequent requests for the same content are served directly from the cache until the TTL expires.

---

**Q2. What is the difference between Cache Hit and Cache Miss in CloudFront?**

A cache hit occurs when a user requests content and CloudFront finds it already cached at the nearest edge location. The content is served directly from the edge without contacting the origin server, resulting in very low latency. A cache miss occurs when the requested content is not available at the edge location, typically on the first request. CloudFront must retrieve the content from the origin server, which takes longer, and then caches it at the edge for future requests. A high cache hit ratio means most requests are served from edge locations, reducing origin load and improving performance.

---

**Q3. How does CloudFront improve security?**

CloudFront improves security in multiple ways. It integrates with AWS WAF to filter malicious web requests like SQL injection and XSS at the edge before they reach the origin server. AWS Shield is automatically enabled for DDoS protection. CloudFront can enforce HTTPS by redirecting HTTP requests to HTTPS and supports custom SSL certificates through AWS Certificate Manager. By serving content through CloudFront, the origin server's IP address is hidden from end users, reducing direct attack surface. Additionally, Origin Access Control ensures that S3 buckets can only be accessed through CloudFront and not directly.

---

**Q4. What is TTL in CloudFront and why does it matter?**

TTL stands for Time To Live and it determines how long CloudFront caches content at edge locations before checking the origin for a fresh copy. A longer TTL means content stays cached longer, improving performance and reducing origin costs, but users may receive stale content if the origin file is updated. A shorter TTL ensures users always get fresh content but increases requests to the origin server. The appropriate TTL depends on how frequently the content changes — static assets like images can have a long TTL of several days, while frequently updated content like news articles should have a shorter TTL.

---

## Key Points — Phone Pe Save Karo

```
CloudFront = AWS CDN (Content Delivery Network)
400+ Edge Locations worldwide

Cache Miss = Pehli baar → Origin se fetch → Edge pe store
Cache Hit  = Baad mein → Edge se direct serve ✅

Origins:
→ S3 Bucket (static files)
→ EC2 / ALB (dynamic apps)
→ API Gateway (REST APIs)

TTL = Cache kitni der tak valid (default 24 hours)
Invalidation = Purani cache manually hatao

Best Combos:
CloudFront + S3   = Static website globally fast ✅
CloudFront + WAF  = Security at edge ✅
CloudFront + ACM  = Free SSL certificate ✅

Price class:
All edge locations = Best performance (worldwide)
Only specific regions = Cost optimize karo
```
