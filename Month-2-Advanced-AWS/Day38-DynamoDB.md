# Day 38 — DynamoDB (NoSQL Database)

## Problem — SQL ki Limitations

```
SQL (MySQL/PostgreSQL):
→ Schema fixed hota hai — pehle design karo phir data daalo
→ Har row same columns honi chahiye
→ Scale karna mushkil — server upgrade karna padta hai
→ Millions of users → SQL slow ho jaata hai ❌
```

---

## Solution — DynamoDB

```
DynamoDB = AWS ka Fully Managed NoSQL Database

NoSQL = Not Only SQL
      = Flexible structure
      = Massive scale
      = Ultra fast (single digit milliseconds!)

Kuch manage nahi karna:
→ No server setup ❌
→ No patches ❌
→ No backups manually ❌
AWS sab karta hai ✅
```

---

## Analogy — Excel vs WhatsApp Contacts

```
SQL = Excel Sheet
      Columns fixed: Name | Age | City | Phone
      Har row same format mein honi chahiye

DynamoDB = WhatsApp Contacts
      Akash  → Name, Phone, Email
      Rahul  → Name, Phone only
      Priya  → Name, Phone, Email, Birthday, Company

Har contact alag info rakh sakta hai!
Schema flexible hai ✅
```

---

## SQL vs DynamoDB — Fark

```
Feature         | SQL (RDS)              | DynamoDB
----------------|------------------------|------------------
Type            | Relational             | NoSQL
Schema          | Fixed (strict)         | Flexible
Speed           | Query pe depend karta  | Always fast (ms)
Scale           | Manual                 | Automatic
JOINs           | Yes                    | No
Best for        | Complex queries        | Simple key-value
Server          | Manage karna padta     | Fully managed
```

---

## DynamoDB Structure

```
Table     = SQL ki Table jaisa
Item      = SQL ki Row jaisa
Attribute = SQL ka Column jaisa

Lekin fark:
SQL    → Har row same columns
Dynamo → Har item alag attributes rakh sakta hai ✅

Example — akash-orders Table:

Item 1:
  orderId  : "ORD-001"
  customer : "Akash"
  item     : "Burger"
  price    : 200
  status   : "Placed"

Item 2:
  orderId  : "ORD-002"
  customer : "Rahul"
  item     : "Pizza"
  price    : 350
  status   : "placed"
  discount : 50        ← Extra attribute!
                         SQL mein NULL daalna padta ❌
                         DynamoDB mein OK! ✅
```

---

## Primary Key — Sabse Important

```
Har item ka ek unique key hona chahiye

Partition Key (Simple):
→ Ek field jo unique ho
→ orderId: "ORD-001" → Sirf ek order ✅

Partition Key + Sort Key (Composite):
→ Do fields milke unique banate hain
→ customerId: "C001" + orderDate: "2026-07-10"
→ Same customer ke multiple orders store karo ✅
```

---

## Scan vs Query

```
Scan:
→ Poora table scan karta hai
→ Sab items return karta hai
→ Slow + Costly (bada table ho toh) ❌
→ Use karo: Jab filter chahiye ho

Query:
→ Partition key se direct dhundta hai
→ Sirf ek item return karta hai
→ Ultra fast! ✅
→ Use karo: Hamesha (jab orderId pata ho)

Real world:
"Order track karo ORD-001"
→ Query → Direct hit → Turant result ✅
```

---

## DynamoDB kyun use karein

```
Speed:
→ Single digit milliseconds ✅
→ SQL complex query pe slow ho sakti hai

Scale:
→ Crores of requests handle karta hai
→ Automatic scaling ✅

Serverless ke saath perfect:
API Gateway → Lambda → DynamoDB
Teeno managed, teeno serverless ✅

Use cases:
→ Order history (Zomato)
→ User sessions (Login tokens)
→ Shopping cart
→ Gaming leaderboard (PUBG scores)
→ Real-time data
```

---

## SQL kab, DynamoDB kab?

```
SQL (RDS) use karo:
→ Complex queries chahiye (JOIN, GROUP BY)
→ Relations hain (User → Orders → Products)
→ Financial transactions (strict consistency)
→ Reporting dashboards

DynamoDB use karo:
→ Simple key-value access (orderId se order do)
→ Massive scale (millions of users)
→ Serverless architecture
→ Fast reads (gaming, sessions, cart)
```

---

## Complete Serverless Stack

```
Mobile App / Browser
        ↓
   API Gateway     ← Public URL + Security
        ↓
   AWS Lambda      ← Business Logic
        ↓
   DynamoDB        ← Data Store

Koi server nahi! Koi EC2 nahi!
Pay per request ✅
```

---

## Hands-On — Aaj Kya Kiya

### Table Banaya
```
Table name    : akash-orders
Partition key : orderId (String)
```

### Items Daale
```
Item 1:
orderId  = ORD-001
customer = Akash
item     = Burger
price    = 200
status   = Placed

Item 2:
orderId  = ORD-002
customer = Rahul
item     = Pizza
price    = 350
status   = placed
discount = 50    ← Extra attribute (SQL mein nahi hota!) ✅
```

### Query kiya
```
Query → orderId = ORD-001
→ Items returned: 1 (Akash ka Burger order)
→ Items scanned: 1 (Direct hit — ultra fast!) ✅
```

### Cleanup
```
✅ Table deleted
→ Bill zero! ✅
```

---

## Interview Questions & Answers

**Q1. What is Amazon DynamoDB and how is it different from RDS?**

Amazon DynamoDB is a fully managed NoSQL database service provided by AWS that delivers single-digit millisecond performance at any scale. Unlike RDS which is a relational database requiring a fixed schema, DynamoDB is schema-flexible — each item in a table can have different attributes. RDS is suited for complex queries with JOINs and relationships between tables, while DynamoDB is designed for simple key-value access patterns with massive scalability. DynamoDB automatically scales to handle any amount of traffic without any manual configuration, whereas RDS requires manual scaling or read replicas for high traffic.

---

**Q2. What is a Partition Key in DynamoDB and why is it important?**

A partition key is the primary identifier for each item in a DynamoDB table and must be unique across all items. DynamoDB uses the partition key value to determine which partition to store the item in internally. When querying, DynamoDB uses the partition key to directly locate the item without scanning the entire table, resulting in ultra-fast single-digit millisecond reads. Choosing the right partition key is critical — a good partition key distributes data evenly across partitions and is used in most access patterns. For example, using orderId as a partition key allows direct lookup of any specific order instantly.

---

**Q3. What is the difference between Scan and Query in DynamoDB?**

A Query operation uses the partition key to directly find items and is extremely fast because DynamoDB knows exactly which partition to look in. It returns only the items matching the specified key. A Scan operation reads every item in the table and then filters results, making it much slower and more expensive as the table grows. In production, Query should always be preferred over Scan. Scan is only appropriate for small tables or one-time administrative operations. Good table design ensures that most data access patterns can be served by Query operations using the partition key.

---

**Q4. When would you choose DynamoDB over RDS for an application?**

DynamoDB is the better choice when the application needs to handle millions of users or requests with consistent low latency, when the data access pattern is simple key-value lookups without complex JOIN queries, when the application follows a serverless architecture using Lambda and API Gateway, or when the schema needs to be flexible with different items having different attributes. RDS is preferred when the application requires complex reporting queries, transactions across multiple tables, or strict relational integrity. For example, Zomato order history is a perfect DynamoDB use case — each order is looked up by orderId, the system handles millions of requests, and different orders can have different attributes like discounts or special instructions.

---

## Key Points — Phone Pe Save Karo

```
DynamoDB = AWS Fully Managed NoSQL Database

SQL se fark:
→ Schema flexible (har item alag attributes) ✅
→ No JOINs
→ Single digit milliseconds fast ✅
→ Auto scaling ✅

Structure:
Table → Items → Attributes
(Table)  (Row)   (Column)

Primary Key:
→ Partition Key       = Simple (orderId)
→ Partition + Sort Key = Composite (customerId + date)

Scan vs Query:
→ Scan  = Poora table (slow, avoid karo) ❌
→ Query = Partition key se direct (fast) ✅

Best use cases:
→ Order history, Sessions, Cart
→ Gaming leaderboard, Real-time data

Complete Serverless Stack:
API Gateway + Lambda + DynamoDB = No servers! ✅

FREE TIER:
25 GB storage free ✅
25 read/write capacity units free ✅
```
