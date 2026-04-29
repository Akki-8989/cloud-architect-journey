# Day 18 — ElastiCache (Caching Service)

## Problem Samjho — Kyun Chahiye Caching?

Swiggy pe har second 50,000 users "restaurants near me" search kar rahe hain.

```
WITHOUT Cache:
Har user ka request → Database hit karo
                    → Query run karo
                    → Result wapas bhejo

50,000 requests/second → 50,000 DB queries/second
Database overload → Slow response → Users frustrated ❌
Cost bhi zyada — DB har baar kaam karta hai ❌
```

**Socho — 50,000 mein se kitne users ka result same hoga?**

Almost sabka! Mumbai mein restaurants wahi hain — baar baar DB kyun hit karein?

```
WITH Cache:
Pehla user  → DB se data laya → Cache mein rakha
Baaki 49,999 → Cache se diya instantly ⚡

DB sirf ek baar hit hua!
Speed = Milliseconds
Cost  = Kam
```

**Ye hai ElastiCache ka kaam!**

---

## ElastiCache Kya Hai

ElastiCache AWS ka **Fully Managed In-Memory Caching Service** hai.

```
Normal DB    = Data disk pe store hota hai → slow
ElastiCache  = Data RAM mein store hota hai → super fast!

RAM  = Memory → Access time nanoseconds
Disk = Storage → Access time milliseconds

ElastiCache = Database ke saamne ek fast layer
```

**Developer Analogy:** Tumhare Node.js app mein variable mein data store karte ho toh baar baar function call nahi karna padta. ElastiCache wahi kaam karta hai — but **distributed scale pe** poori application ke liye!

---

## Cache Hit vs Cache Miss

```
CACHE MISS (Pehli baar):
User request → Cache check kiya → Nahi mila
             → Database se laya
             → Cache mein store kiya
             → User ko diya
(Thoda slow — DB tak gaya)

CACHE HIT (Agli baar):
User request → Cache check kiya → Mila! ⚡
             → Direct Cache se diya
             → DB nahi gaya
(Super fast — RAM se mila)
```

**CloudFront vs ElastiCache:**
```
CloudFront   = Files cache karta hai (HTML, images, JS)
ElastiCache  = Database results cache karta hai (query results, sessions)
```

---

## ElastiCache Ke 2 Engines

### Redis
```
✅ Rich data structures (strings, lists, sets, sorted sets, hashes)
✅ Data persistence (disk pe bhi save kar sakta hai)
✅ Pub/Sub messaging support
✅ Master-Replica replication
✅ Leaderboards, session store, pub/sub
→ Production mein mostly Redis use hota hai
```

### Memcached
```
✅ Simple key-value store
✅ Multi-threaded — multiple cores use karta hai
✅ Very fast for simple caching
❌ No persistence
❌ No pub/sub
→ Simple caching ke liye, jab sirf strings chahiye
```

**Simple rule:** Production mein **Redis** use karo — zyada features hain.

---

## Redis Key Commands

```bash
# Data store karo (SET)
SET user:101 "Rahul Sharma"
SET order:ORD-001 "599"

# Data nikalo (GET)
GET user:101          → "Rahul Sharma"

# Expiry set karo (EX = seconds)
SET session:ABC123 "logged_in" EX 3600
# 1 hour baad auto delete ho jaayega

# Key exist karta hai?
EXISTS user:101       → 1 (yes) / 0 (no)

# Key delete karo
DEL user:101

# Sab keys dekho
KEYS *

# Counter badhao (INCR)
INCR page:views       → 1, 2, 3...
```

---

## Real World Use Cases

```
Sessions      → User login session store karo (EX 3600 = 1 hour)
               Har request pe Redis se check — DB hit nahi!

Leaderboards  → PUBG top 100 players score
               Redis Sorted Sets — automatic ranking!

Rate Limiting → API ke 100 requests/minute limit
               INCR counter → 100 se zyada? Block!

Hot Data      → Swiggy restaurants list, popular products
               Cache karo → DB load kam

Pub/Sub       → Real-time notifications
               Redis channels pe subscribe karo
```

---

## Caching Pattern — Application Mein Kaise Use Karte Hain

```
Application Code (Node.js/Python):

1. User request aaya
2. Redis mein check karo
   → Mila (Cache Hit) → Return karo ⚡
   → Nahi mila (Cache Miss):
      → DB se fetch karo
      → Redis mein store karo (EX 300 = 5 min)
      → User ko return karo
```

**Real example — Swiggy:**
```
GET restaurants:mumbai:pizza

Cache Hit  → Instant result ✅
Cache Miss → DB query → Cache mein store → Return
             (Next user ko Cache Hit milega!)
```

---

## ElastiCache Security

```
VPC ke andar hota hai → Internet se directly accessible nahi
Same VPC ke EC2/Lambda hi connect kar sakte hain
Encryption in transit → TLS (port 6379 encrypted)
Encryption at rest   → KMS keys se
```

**Kyun important hai:** CloudShell se directly connect nahi hua kyunki ElastiCache VPC ke andar tha — ye security feature hai, bug nahi!

---

## Hands-On — Aaj Kya Kiya

### Step 1 — Redis OSS Cache Banaya
```
ElastiCache → Redis OSS caches → Create cache

Deployment : Serverless
Name       : my-redis-cache
→ Create
```
**Kyun:** Serverless select kiya — koi capacity planning nahi, auto scale hota hai. Sirf use karo aur pay karo.

### Step 2 — Cache Available Hua
```
Status: Creating → Available ✅

Endpoint: my-redis-cache-zqky4x.serverless.aps1.cache.amazonaws.com:6379
Port 6379 = Redis ka default port
```
**Kyun endpoint important hai:** Application code mein ye endpoint use hota hai Redis se connect karne ke liye.

### Step 3 — CloudShell Se Connect Kiya
```
redis-cli --tls -h my-redis-cache-zqky4x.serverless.aps1.cache.amazonaws.com -p 6379
```
**Result:** Connection timeout — kyunki ElastiCache VPC ke andar hai, CloudShell bahar se directly access nahi kar sakta.

**Ye security ka proof hai!** Real project mein EC2 (same VPC) se connect karte hain.

### Step 4 — Cleanup
```
ElastiCache → my-redis-cache → Actions → Delete
→ No backup → Confirm → Delete ✅
```

---

## Real World Architecture

```
User Request
     ↓
Application (EC2 / Lambda)
     ↓
Check Redis Cache (ElastiCache)
     ↓
Cache Hit?  → YES → Return data ⚡ (fast!)
            → NO  → Query RDS/DynamoDB
                   → Store in Redis (EX 300)
                   → Return data
```

**With ElastiCache:**
```
Response time  : 1ms (cache hit) vs 100ms (DB query)
DB load        : 90% kam ho jaata hai
Cost           : DB instances chhote rakh sakte ho
Scale          : Crores of requests handle hote hain
```

---

## Interview Questions & Answers

**Q1. What is Amazon ElastiCache and why is it used?**

Amazon ElastiCache is a fully managed in-memory caching service that supports Redis and Memcached engines. It is used to improve application performance by storing frequently accessed data in memory rather than fetching it from a database on every request. Since RAM access is orders of magnitude faster than disk-based database queries, ElastiCache can reduce response times from hundreds of milliseconds to single-digit milliseconds. Common use cases include caching database query results, storing user session data, maintaining leaderboards, and implementing rate limiting. By reducing the number of database calls, ElastiCache also lowers the load on backend databases and reduces infrastructure costs.

---

**Q2. What is the difference between Cache Hit and Cache Miss?**

A Cache Hit occurs when the application requests data and finds it already stored in ElastiCache. The data is returned directly from memory without querying the database, resulting in very fast response times. A Cache Miss occurs when the requested data is not found in the cache — either because it was never cached, or because it expired. On a cache miss, the application queries the database, retrieves the data, stores it in ElastiCache for future requests, and then returns it to the user. The first request for any data is always a cache miss, but subsequent requests become cache hits until the data expires.

---

**Q3. What is the difference between Redis and Memcached in ElastiCache?**

Redis is a feature-rich in-memory data store that supports complex data structures including strings, lists, sets, sorted sets, and hashes. It supports data persistence to disk, replication with automatic failover, pub/sub messaging, and Lua scripting. Redis is suitable for use cases like leaderboards using sorted sets, session management, pub/sub notifications, and anywhere you need data to survive a restart. Memcached is simpler — it only supports string key-value pairs, has no persistence, and no replication. However, it is multi-threaded and can be slightly faster for simple caching workloads. In practice, Redis is the more popular choice for production systems due to its richer feature set.

---

**Q4. Why couldn't CloudShell connect to ElastiCache directly?**

ElastiCache runs inside a VPC and is not accessible from the public internet by design. CloudShell runs outside the VPC, so it cannot reach the ElastiCache endpoint directly. This is a security feature — it ensures that only resources within the same VPC, such as EC2 instances or Lambda functions, can connect to the cache. In production, your application servers run inside the same VPC as ElastiCache and connect without any issues. To connect from outside the VPC for debugging, you would need to either set up a bastion host inside the VPC or configure VPC peering.

---

**Q5. How does ElastiCache reduce database load in a high-traffic application?**

ElastiCache sits between the application and the database as a caching layer. When a user requests data, the application first checks ElastiCache. If the data is found in cache, it is returned immediately without touching the database. Only on a cache miss does the application query the database, and the result is then stored in ElastiCache with a TTL for future requests. In a scenario where thousands of users request the same popular data — like a restaurant menu or product listing — the database is queried only once, and all subsequent requests are served from cache. This can reduce database load by 80 to 90 percent, allowing you to use smaller, cheaper database instances and handle far more traffic.

---

## Key Points — Phone Pe Save Karo

```
ElastiCache  = AWS ka In-Memory Caching Service
Redis        = Feature-rich — sessions, leaderboards, pub/sub
Memcached    = Simple, fast — basic key-value caching
Cache Hit    = Data mila RAM mein → fast ⚡
Cache Miss   = Data nahi mila → DB se laya → Cache mein rakha
SET          = Data store karo
GET          = Data nikalo
EX           = Expiry time (seconds mein)
Port 6379    = Redis ka default port
VPC          = ElastiCache sirf VPC ke andar accessible
TTL          = Time To Live — kitne seconds mein cache expire hoga
Use Case     = Sessions, Leaderboards, Hot Data, Rate Limiting
```
