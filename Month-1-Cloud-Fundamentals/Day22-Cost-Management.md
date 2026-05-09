# Day 22 — AWS Cost Management

## Real World Problem Samjho

Ek startup ne AWS pe app deploy kiya. Month end pe bill aaya — ₹5 lakh!

```
Kyun itna aaya:
→ Test EC2 instances band nahi ki — 24/7 chalta raha
→ NAT Gateway unnecessarily chal raha tha
→ Data transfer charges notice nahi kiye
→ RDS instance badi size ki — zaroorat nahi thi
→ Koi budget alert set nahi tha — pata hi nahi chala!
```

**Cloud Architect ki zimmedari:** Sirf architecture nahi — **cost optimize karna bhi!**

---

## AWS Cost Management Ke 4 Tools

```
1. AWS Billing Dashboard  → Total bill dekho
2. Cost Explorer          → Kahan kharcha hua analyze karo
3. AWS Budgets            → Limit set karo — alert aaye
4. Cost Allocation Tags   → Kaunse project ka kitna kharcha
```

---

## Free Tier — Kya Free Milta Hai

AWS naye accounts ko **12 months free tier** deta hai:

```
12 Months Free (New Account):
EC2      → 750 hours/month t2.micro/t3.micro
S3       → 5 GB storage
RDS      → 750 hours/month db.t2.micro
CloudFront → 1 TB data transfer

Always Free (Hamesha — account kitna bhi purana ho):
Lambda   → 1 million requests/month FREE
DynamoDB → 25 GB storage FREE
SNS      → 1 million publishes FREE
CloudWatch → 10 custom metrics FREE
```

---

## AWS Billing Dashboard

```
Billing → Bills section:
→ Current month ka total bill
→ Service wise breakdown
→ Region wise breakdown
→ Credits applied

Billing → Payments:
→ Payment history
→ Upcoming payment

Billing → Free Tier:
→ Kitna use hua free tier ka
→ Kaunsi service limit ke kitni karib hai
```

---

## Cost Explorer — Kharcha Analyze Karo

Cost Explorer graphical tool hai jo dikhata hai:

```
→ Service wise kharcha (EC2 kitna, S3 kitna, RDS kitna)
→ Month wise trend (line graph)
→ Region wise kharcha
→ Future cost forecast (next month kitna hoga)
→ Daily breakdown
```

**Real use case:**
```
"Kyun last month bill zyada aaya?"
→ Cost Explorer mein jaao
→ EC2 → $50 zyada
→ Koi test instance band nahi ki thi!
→ Band karo → Next month normal bill ✅
```

**Note:** Cost Explorer pehli baar enable karne ke baad **24 hours** mein ready hota hai.

---

## AWS Budgets — Alert System

Budget = Alarm system — koi amount cut nahi karta, sirf alert bhejta hai!

```
Budget set karo: "Monthly $10 se zyada mat kharcho"

Alert 1: Actual spend $8.50 (85%) → Email ✅
Alert 2: Actual spend $10 (100%) → Email ✅
Alert 3: Forecast $12 hoga → Email ✅ (advance warning!)

Tum sote raho — AWS email bhejega!
```

### Budget Types

```
Zero Spend Budget:
→ $0.01 se zyada kharcha hua → Immediately alert
→ Free tier break hua → Pata chal jaata hai
→ Students/Learning ke liye perfect!

Monthly Cost Budget:
→ Monthly limit set karo ($10, $50, $100)
→ 85% pe alert, 100% pe alert, forecast pe alert
→ Production accounts ke liye

Daily Savings Plans Coverage:
→ Savings Plans kitna cover kar raha hai

Daily Reservation Utilization:
→ Reserved instances kitni use ho rahi hain
```

---

## Cost Allocation Tags

```
Tags = Labels jo resources pe lagate hain

Jaise:
EC2 instance pe tag:
  Project: Swiggy-App
  Environment: Production
  Team: Backend

RDS pe tag:
  Project: Swiggy-App
  Environment: Production
  Team: Database

Phir Cost Explorer mein filter karo:
"Project: Swiggy-App" → Sirf Swiggy ka kharcha dikhao
```

**Real use:** Company mein multiple projects hain — har project ka alag cost track karo!

---

## Cost Optimization Tips — Cloud Architect Must Know

```
1. Right Sizing
   EC2 t3.large use kar rahe ho but sirf 20% CPU?
   → t3.small pe shift karo → 70% cost saving!

2. Reserved Instances
   1 saal ka commitment → 40-60% discount
   3 saal ka commitment → 60-70% discount

3. Spot Instances
   AWS ki unused capacity → 90% discount
   Non-critical workloads ke liye (batch jobs)

4. Auto Scaling
   Off-peak hours mein instances kam karo
   Peak hours mein badhao → Cost optimize

5. S3 Storage Classes
   Frequently accessed → S3 Standard
   Monthly access → S3 Standard-IA (30% cheap)
   Archive → S3 Glacier (90% cheap)

6. Delete Unused Resources
   Unused EC2, EBS volumes, Elastic IPs
   → Sab money waste kar rahe hain!

7. NAT Gateway
   Expensive hai — ek hi AZ mein rakho dev mein
   Production mein 2 (HA ke liye)
```

---

## Hands-On — Aaj Kya Kiya

### Step 1 — Zero Spend Budget Banaya
```
Billing → Budgets → Create budget
→ Use a template
→ Zero spend budget

Budget name    : My-Zero-Spend-Budget
Email          : akashpatil6269@gmail.com
→ Create budget ✅
```
**Kyun:** Free tier se $0.01 bhi zyada hua toh immediately email aayega. Student account ke liye best protection!

### Step 2 — Monthly Cost Budget Banaya
```
Billing → Budgets → Create budget
→ Use a template
→ Monthly cost budget

Budget name    : My-Monthly-Budget
Amount         : $10
Email          : akashpatil6269@gmail.com
→ Create budget ✅
```
**Kyun:** Monthly $10 limit set ki. Teen alerts:
- 85% ($8.50) pe warning
- 100% ($10) pe alert
- Forecast 100% se zyada hoga toh advance alert

**Important:** Budget sirf alert bhejta hai — koi charge cut nahi hota, koi service band nahi hoti!

### Step 3 — Cost Explorer Dekha
```
Billing → Cost Explorer
→ "Data prepare ho rahi hai — 24 hours mein ready"
```
**Kyun:** Cost Explorer pehli baar enable karne pe AWS ko 24 hours lagte hain historical data process karne mein. Kal se graphs dikhenge!

---

## AWS Pricing Models

```
On-Demand:
→ Pay per hour/second
→ No commitment
→ Most expensive
→ Testing/unpredictable workloads

Reserved Instances:
→ 1 or 3 year commitment
→ 40-70% discount
→ Predictable production workloads

Spot Instances:
→ AWS unused capacity
→ Up to 90% discount
→ Can be interrupted anytime
→ Batch jobs, fault-tolerant workloads

Savings Plans:
→ Commit to $ amount per hour
→ 40-66% discount
→ More flexible than Reserved Instances
```

---

## Interview Questions & Answers

**Q1. What tools does AWS provide for cost management?**

AWS provides several tools for cost management. The Billing Dashboard shows your current and historical bills broken down by service and region. Cost Explorer is a graphical tool that lets you visualize and analyze your AWS spending patterns, see trends over time, and forecast future costs. AWS Budgets allows you to set custom cost thresholds and receive email or SNS alerts when your spending reaches or is forecasted to reach those thresholds. Cost Allocation Tags let you tag resources with project or team names and then filter cost reports by those tags to understand which teams or projects are incurring which costs. Together these tools give you full visibility and control over your AWS spending.

---

**Q2. What is AWS Budgets and how does it work?**

AWS Budgets is a cost management tool that lets you set custom spending thresholds and receive alerts when your actual or forecasted costs approach or exceed those thresholds. You define a budget with a specific dollar amount and time period, and add email recipients to be notified. AWS Budgets does not automatically stop services or charge your account differently — it only sends notifications. For example, you can set a monthly budget of $50 and receive alerts when your spending hits 85 percent and 100 percent of that limit, and when AWS forecasts that you will exceed it. This helps teams catch unexpected spending early before it becomes a large bill.

---

**Q3. What is the AWS Free Tier and what are its two types?**

The AWS Free Tier provides limited access to AWS services at no charge. There are two types. The twelve months free tier applies to new AWS accounts for the first twelve months after account creation and includes services like 750 hours per month of EC2 t2.micro instances, 5 GB of S3 storage, and 750 hours of RDS db.t2.micro. The always free tier applies permanently regardless of how long the account has existed and includes services like one million Lambda requests per month, 25 GB of DynamoDB storage, and one million SNS publishes per month. It is important to monitor free tier usage through the Billing console to avoid unexpected charges when limits are exceeded.

---

**Q4. What are Reserved Instances and when should you use them?**

Reserved Instances are a billing model where you commit to using a specific EC2 instance type in a specific region for a one or three year term in exchange for a significant discount compared to On-Demand pricing. One year commitments typically offer 40 to 45 percent discounts and three year commitments offer 60 to 70 percent discounts. You should use Reserved Instances for predictable, steady-state production workloads where you know you will need the capacity continuously for at least a year. For example, a production web server that runs 24/7 is a good candidate. They are not suitable for variable or unpredictable workloads, development environments, or anything that might be turned off, since you pay for the reservation whether or not the instance is running.

---

**Q5. What are some key cost optimization strategies for a Cloud Architect?**

A Cloud Architect should apply several cost optimization strategies. Right-sizing means choosing the smallest instance type that meets performance requirements — many teams over-provision and waste money on unused capacity. Auto Scaling ensures you only run as many instances as needed at any given time, scaling down during off-peak hours. Using appropriate S3 storage classes moves infrequently accessed data to cheaper tiers like S3 Standard-IA or Glacier. Purchasing Reserved Instances for predictable workloads provides significant discounts. Using Spot Instances for fault-tolerant batch workloads can reduce costs by up to 90 percent. Deleting unused resources like stopped EC2 instances, unattached EBS volumes, and unused Elastic IPs eliminates waste. Finally, setting up AWS Budgets alerts ensures the team is notified before unexpected bills accumulate.

---

## Key Points — Phone Pe Save Karo

```
Billing Dashboard = Total bill + service breakdown
Cost Explorer     = Graphical analysis + forecast (24hr setup)
AWS Budgets       = Alert set karo — koi charge nahi karta!
Zero Spend Budget = $0.01 se zyada → Immediately alert
Monthly Budget    = 85%, 100%, forecast pe alert
Free Tier         = 12 months + Always free (2 types)
On-Demand         = Pay per hour — flexible, expensive
Reserved          = 1-3 year commitment — 40-70% discount
Spot              = 90% discount — interrupt ho sakta hai
Right Sizing      = Chhoti instance → Same kaam → Less cost
Cost Tags         = Project wise kharcha track karo
NAT Gateway       = Expensive — zaroorat ho tabhi use karo
```
