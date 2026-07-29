# Day 51 - AWS Cost Explorer + Billing Management

## Problem
```
Month end par AWS bill aaya - $500!
"Kaunsi service itna charge kar rahi hai?"
"Kab se charge ho raha hai?"
"Aage aur badhega kya?"
Bina tools ke: Blindly pay karo x
```

## Solution - AWS Cost Management Tools
```
Billing Dashboard  -> Overall spend snapshot
Cost Explorer      -> Service-wise deep dive + graphs
AWS Budgets        -> Spending limits + alerts
Free Tier Monitor  -> Free usage track karo
```

## Analogy - Bank App
```
Billing Dashboard  = Bank balance (total kitna gaya)
Cost Explorer      = Transaction history (kahan gaya)
AWS Budgets        = Spending limit set karo (UPI limit jaisi)
Free Tier Monitor  = "Free quota khatam hone wala hai" alert
```

## 1. Billing Dashboard
```
Kya: AWS spend ka overview - ek nazar mein sab
Kahan: AWS Console -> top right -> Account -> Billing

Dikhata hai:
  Current month spend
  Last month total
  Forecasted spend
  Service-wise top charges
  Budget alerts
```

## 2. AWS Cost Explorer
```
Kya: Bill ka X-ray - date/service/region wise breakdown
     Graphs + forecasting + savings recommendations

Features:
  Service-wise breakdown  -> "EC2 ne $X khaya"
  Date filter             -> "July mein kitna hua?"
  Granularity             -> Daily / Monthly view
  Forecasting             -> Aage kitna aayega?
  Savings Plans           -> "Ye karo, $X bachega"

Use kab karo?
  Bill zyada aaya -> Kaunsi service charge kar rahi?
  Month ke beech mein spend track karna ho
```

## 3. AWS Budgets
```
Kya: Spending limit set karo + alert aaye jab exceed ho

Types:
  Cost Budget    -> "Total $10 se zyada na ho"
  Usage Budget   -> "EC2 750 hours se zyada na chale"
  Savings Plan   -> Commitment track karo

Alert: Email aata hai jab threshold cross ho
```

## 4. Free Tier Monitoring
```
Billing -> Free Tier -> Har service ka usage %
"EC2: 120/750 hours used (16%)"

Best practice: Enable Free Tier alerts!
Billing -> Billing Preferences -> Free Tier alerts ON
```

## Hands-On - Aaj Kya Kiya
```
Billing Dashboard:
  Current month: $2.65
  Last month:    $18.82
  Forecast:      $2.82

Cost Explorer (July 2026 breakdown):
  EC2-Other:  $1.58  <- Stopped instances ka EBS charge!
  RDS:        $0.72
  Tax:        $0.41
  Config:     $0.12
  Rest:       $0.00  (Free Tier)

Budgets:
  My Zero-Spend Budget  -> $1 limit, $2.65 used = 265% EXCEEDED!
  My-Monthly-Budget     -> $10 limit, $2.65 used = 26.5% OK

CLEANUP kiya:
  EC2 Snapshot deleted
  MyFirstServer + linux-practice terminated
  EBS Volumes auto-deleted
  Config recording OFF confirmed
  -> Next month bill ~$0 expected!
```

## WHY Framework
```
Cost Explorer vs Billing Dashboard?
  Dashboard  = Quick overview (30 seconds check)
  Explorer   = Deep investigation (kahan gaya exactly)

Budget vs Alert?
  Budget     = Proactive - "pehle se limit set karo"
  Alert      = Reactive  - "ho gaya tab pata chala"
  Best:      Dono saath use karo!

Kab cleanup karna chahiye?
  Har session ke baad resources delete karo
  Stopped instances bhi EBS charge karte hain!
  NAT Gateway = ~$1/day (sabse expensive!)
```

## Interview Questions & Answers

**Q1: What is AWS Cost Explorer?**
A: AWS Cost Explorer is a tool that visualizes and analyzes your AWS spending. It provides service-wise, date-wise, and region-wise cost breakdown with graphs. It also offers cost forecasting for the next 12 months and savings recommendations like Reserved Instances or Savings Plans.

**Q2: What is the difference between AWS Budgets and Cost Explorer?**
A: Cost Explorer is for analyzing past and current spending — it shows where money was spent with detailed breakdowns. AWS Budgets is for proactive cost control — you set spending limits and receive alerts when approaching or exceeding thresholds. Cost Explorer tells you what happened; Budgets prevents overspending.

**Q3: How would you reduce an unexpectedly high AWS bill?**
A: First, use Cost Explorer to identify which service is causing the spike. Check for stopped EC2 instances (still charge for EBS), unattached EBS volumes, NAT Gateways, RDS snapshots, and data transfer costs. Enable AWS Budgets alerts so you know before the bill gets too high. Use Trusted Advisor for automated recommendations.

**Q4: What resources cost money even when stopped?**
A: Stopped EC2 instances still charge for attached EBS volumes. Elastic IPs not associated with running instances incur charges. RDS stopped instances charge for storage. NAT Gateways charge hourly regardless of traffic. Always terminate (not just stop) practice resources after use.

**Q5: What is Free Tier monitoring?**
A: AWS Free Tier monitoring tracks your usage against free tier limits for each service. You can enable alerts in Billing Preferences to receive email notifications when you're approaching free tier limits, preventing unexpected charges.

## Key Points - Phone Pe Save Karo
```
1. Billing Dashboard = Quick overview (total spend)
2. Cost Explorer = Deep dive (service/date/region wise)
3. AWS Budgets = Spending limits + email alerts
4. Stopped EC2 = Still charges EBS! Always terminate!
5. NAT Gateway = ~$1/day - delete karo when not needed
6. Free Tier alerts = Billing Preferences mein ON karo
7. Cost Explorer forecasting = Next month estimate deta hai
8. Cleanup checklist: EC2 -> EBS -> Snapshots -> NAT GW -> Config
```
