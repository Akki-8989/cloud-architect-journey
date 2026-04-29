# Day 17 — DynamoDB (NoSQL Database)

## Problem Samjho — Kyun Chahiye NoSQL?

Zomato pe 10 lakh orders aa rahe hain ek saath. Traditional SQL database (RDS) mein:

```
WITHOUT NoSQL (SQL Database):
Orders table mein fixed columns hain
Mobile order → same columns
Web order    → same columns
Partner order → kuch alag columns chahiye → ERROR!

PROBLEM:
Schema rigid hai → Naya field add karna mushkil ❌
10 lakh orders → SQL slow ho jaata hai ❌
Data structure hamesha same nahi hota ❌
```

**Solution = DynamoDB (NoSQL Database)**

```
WITH DynamoDB:
Mobile order → orderId, amount, item ✅
Web order    → orderId, amount, item, coupon ✅ (extra field!)
Partner order → orderId, amount, restaurantId ✅ (alag fields!)

Har item ka alag structure ho sakta hai — koi problem nahi!
10 lakh orders → DynamoDB handles instantly ✅
```

---

## PUBG Analogy

```
SQL Database = Fixed form bharni padti hai
Every player ka same data: name, kills, deaths
Koi naya field nahi add kar sakte easily

DynamoDB = Flexible bag
Har player jo chahiye woh rakh sakta hai
Kisi ke paas gun hai, kisi ke paas medkit, kisi ke paas vehicle
Koi fixed structure nahi — jo chahiye woh!
```

---

## DynamoDB Kya Hai

DynamoDB AWS ka **Fully Managed NoSQL Database** hai.

```
Tumhara kaam  = Sirf table banao aur data daalo
AWS ka kaam   = Server manage, backup, scaling — sab kuch

Pay karo      = Sirf jo read/write karo uska
Scale         = Automatically — 1 request se 1 crore request tak
Speed         = Single-digit milliseconds — hamesha!
```

**Developer Analogy:** SQL = strict class ka teacher — sab rules follow karo. DynamoDB = flexible mentor — apne hisaab se kaam karo!

---

## DynamoDB vs RDS — Kab Kaunsa Use Karein

```
RDS (SQL) Use Karo Jab:
→ Data structured hai — fixed columns
→ Complex queries chahiye (JOIN, GROUP BY)
→ Financial data — transactions important hain
→ Relationships hain tables ke beech

DynamoDB (NoSQL) Use Karo Jab:
→ Data flexible hai — different structures
→ Simple queries — key se data dhundho
→ High speed chahiye — millisecond response
→ Massive scale — crores of records
→ Serverless architecture (Lambda ke saath)
```

---

## DynamoDB Ke Key Concepts

### 1. Table
DynamoDB mein data **Tables** mein store hota hai — lekin SQL tables se bilkul alag!

```
SQL Table:
| id | name  | email          | age |
|----|-------|----------------|-----|
| 1  | Rahul | rahul@mail.com | 25  |
| 2  | Priya | priya@mail.com | 22  |
(Har row same columns)

DynamoDB Table:
Item 1: { orderId: "ORD-001", customerName: "Rahul", amount: 599, status: "placed" }
Item 2: { orderId: "ORD-002", customerName: "Priya", amount: 1299, coupon: "SAVE10" }
Item 3: { orderId: "ORD-003", restaurantId: "REST-45", partnerName: "Swiggy" }
(Har item alag structure — koi problem nahi!)
```

---

### 2. Item
Item = DynamoDB ka ek record (SQL mein "row" bolte hain).

```
{
  "orderId": "ORD-001",         ← Partition Key (required)
  "customerName": "Rahul",      ← Attribute
  "amount": 599,                ← Attribute
  "status": "placed"            ← Attribute
}
```

---

### 3. Partition Key — Unique ID
Partition Key = Item ka **unique identifier** — mandatory hota hai.

```
Har item ka Partition Key alag hona chahiye
DynamoDB isi se item dhundta hai — super fast!

Example:
orders table    → orderId (ORD-001, ORD-002...)
users table     → userId  (USR-001, USR-002...)
products table  → productId (PRD-001, PRD-002...)
```

**Analogy:** Aadhar card number — har insaan ka unique, isi se instantly dhundh sakte hain.

---

### 4. Attribute
Attribute = Item ka koi bhi field/property.

```
Partition Key ke alawa jo bhi data daalo = Attribute
customerName, amount, status, coupon, address — sab attributes hain
Attributes flexible hain — har item mein alag ho sakte hain
```

---

### 5. Scan vs Query

```
Scan  = Poora table padhta hai — slow (costly)
        Jaise library mein har shelf check karo

Query = Partition Key se directly item dhundta hai — fast!
        Jaise library catalog mein search karo → exact location mila
```

**Rule:** Production mein hamesha Query use karo, Scan avoid karo.

---

### 6. DynamoDB Ke Fayde

```
Serverless    = Koi server manage nahi karna
Fully Managed = AWS handle karta hai — backup, patching, scaling
Auto Scaling  = Traffic badha → Automatically scale up
High Speed    = Single-digit milliseconds
Flexible      = Schema-less — data structure change kar sakte ho
Global Tables = Ek table, multiple regions mein replicate (worldwide fast)
Free Tier     = 25 GB storage + 25 RCU/WCU FREE hamesha!
```

---

## Real World Use Cases

```
E-commerce    → Orders, Cart, Product catalog
Gaming        → Player profiles, Leaderboards, Game state (PUBG!)
Social Media  → User posts, Likes, Comments
IoT           → Sensor data, Device status
Session Store → User login sessions
```

---

## Hands-On — Aaj Kya Kiya

### Step 1 — Table Banai
```
DynamoDB → Tables → Create table

Table name    : orders-table
Partition key : orderId (String)
→ Create table
```
**Kyun:** `orderId` partition key hai — har order ka unique ID. String type isliye kyunki "ORD-001" format use kiya.

### Step 2 — Items Add Kiye (Create)
```
orders-table → Explore items → Create item

Item 1:
  orderId      : ORD-001
  customerName : Rahul Sharma
  amount       : 599
  status       : placed

Item 2:
  orderId      : ORD-002
  customerName : Priya Singh
  amount       : 1299
  status       : delivered

Item 3:
  orderId      : ORD-003
  customerName : Amit Kumar
  amount       : 299
  status       : cancelled
```
**Kyun:** Ye 3 items real Zomato orders ki tarah hain — har order ka alag data. NoSQL ki power — `amount` Number type hai, baaki String.

### Step 3 — Query Kiya (Read)
```
Explore items → Query select karo

Partition key value : ORD-002
→ Run

Result: Priya Singh ka data instantly aaya ✅
```
**Kyun:** Query Partition Key se direct item dhundti hai — poora table scan nahi karta. Isliye milliseconds mein result aata hai.

### Step 4 — Item Update Kiya
```
Scan → Run → ORD-001 pe click karo
→ status field → "placed" se "delivered" change kiya
→ Save changes ✅
```
**Kyun:** Order deliver ho gayi toh status update karna padta hai. DynamoDB mein specific attribute update kar sakte ho — poora item replace nahi karna.

### Step 5 — Item Delete Kiya
```
Scan → Run → ORD-003 checkbox tick karo
→ Actions → Delete items → Confirm ✅
```
**Kyun:** Cancelled order ko cleanup kiya. DynamoDB mein Partition Key se directly item delete hota hai.

### Step 6 — Cleanup
```
Tables → orders-table → Delete
→ "confirm" type karo → Delete ✅
```

---

## CRUD Summary

```
Create = Create item → Data add karo
Read   = Query/Scan  → Data padhao
Update = Item pe click → Field change karo → Save
Delete = Checkbox → Actions → Delete items
```

---

## Real World Architecture

```
User ne Order diya
        ↓
   API Gateway
        ↓
   Lambda Function
        ↓
   DynamoDB (orders-table)
        ↓
   Order stored! ✅
   (Milliseconds mein!)
```

**DynamoDB + Lambda = Perfect Serverless Combo!**

---

## Interview Questions & Answers

**Q1. What is Amazon DynamoDB and how is it different from relational databases?**

Amazon DynamoDB is a fully managed NoSQL key-value and document database service provided by AWS. It is fundamentally different from relational databases in several ways. Relational databases like RDS require a fixed schema where every row must have the same columns, use SQL for complex queries including JOINs across tables, and scale vertically by adding more powerful hardware. DynamoDB is schema-less, meaning each item in a table can have different attributes. It scales horizontally by distributing data across multiple servers automatically and provides single-digit millisecond performance at any scale. DynamoDB is best suited for applications with flexible data structures, high traffic, and simple access patterns, while relational databases are better for complex queries and transactional systems with strong relationships between data.

---

**Q2. What is a Partition Key in DynamoDB and why is it important?**

A Partition Key is the primary key attribute that uniquely identifies each item in a DynamoDB table. It is mandatory — every item must have a partition key and no two items can have the same partition key value. DynamoDB uses the partition key to determine which physical partition to store the item in, enabling it to retrieve items with single-digit millisecond performance. When you query DynamoDB with a partition key, it goes directly to the exact partition where the item is stored without scanning the entire table. Choosing a good partition key with high cardinality — meaning many distinct values — is critical for distributing data evenly and avoiding hot partitions that can throttle performance.

---

**Q3. What is the difference between Scan and Query in DynamoDB?**

A Query operation retrieves items based on the partition key value and optionally a sort key condition. It is highly efficient because DynamoDB goes directly to the partition containing items with that key value, without reading other partitions. A Scan operation reads every item in the entire table and then filters the results. While Scan is flexible since you can filter by any attribute, it is expensive and slow for large tables because it consumes read capacity for every item scanned regardless of whether it matches the filter. In production applications, Query should always be preferred over Scan. Scan should only be used for administrative tasks, small tables, or when there is no alternative, and even then with pagination to limit the impact.

---

**Q4. When would you choose DynamoDB over RDS in an architecture?**

DynamoDB is the better choice when you need single-digit millisecond performance at any scale, have flexible or variable data structures where different records may have different attributes, expect massive write throughput such as millions of requests per second, or are building a serverless architecture with AWS Lambda. For example, storing game player state, user session data, IoT sensor readings, or e-commerce shopping carts are ideal DynamoDB use cases. RDS is better when you need complex SQL queries with JOINs across multiple tables, strong ACID transactions across multiple tables, reporting and analytics queries, or when your data has strict relational integrity requirements such as financial systems with foreign key constraints.

---

**Q5. What is DynamoDB's pricing model and what is included in the free tier?**

DynamoDB pricing is based on two dimensions: read and write capacity, and storage. For capacity, you choose between On-Demand mode where you pay per request and Provisioned mode where you pre-allocate capacity units and pay per hour. The AWS free tier for DynamoDB includes 25 GB of storage, 25 provisioned write capacity units, and 25 provisioned read capacity units permanently — not just for the first 12 months. This free tier is sufficient to handle up to 200 million requests per month, making DynamoDB essentially free for small applications and development workloads. You only incur charges when your application scales beyond the free tier limits.

---

## Key Points — Phone Pe Save Karo

```
DynamoDB     = AWS ka Fully Managed NoSQL Database
NoSQL        = Schema-less — flexible data structure
Item         = Ek record (SQL mein "row")
Attribute    = Item ka field/property
Partition Key= Item ka unique identifier — mandatory
Table        = Items ka collection (SQL table se alag)
Query        = Partition Key se fast search — use karo!
Scan         = Poora table padhta hai — avoid karo!
On-Demand    = Pay per request — unpredictable traffic ke liye
CRUD         = Create, Read, Update, Delete — sab ho gaya aaj!
Free Tier    = 25 GB + 25 RCU/WCU hamesha free
Use Case     = E-commerce, Gaming, IoT, Sessions, Social Media
```
