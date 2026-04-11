# AWS Free Tier Guide — Kya Free Hai, Kya Nahi

## Free Tier Kya Hota Hai

AWS naya account banane ke baad **12 months tak** bahut saari services free deta hai. Iska fayda uthao — bejhijhak practice karo.

---

## Kya Free Hai (12 Months)

| Service | Free Limit | Details |
|---------|-----------|---------|
| EC2 | 750 hours/month | Sirf t2.micro ya t3.micro |
| RDS | 750 hours/month | Sirf db.t2.micro ya db.t4g.micro |
| S3 | 5 GB storage | 20,000 GET requests free |
| CloudWatch | 10 alarms | 5 GB log data free |
| VPC | FREE | Hamesha free |
| IAM | FREE | Hamesha free |
| Lambda | 1 million requests/month | Hamesha free |

**750 hours = poora mahina** — matlab ek instance 24/7 chala sakte ho bina charge ke.

---

## Kya Dhyan Rakho

```
EC2/RDS    → Use karne ke baad STOP karo — DELETE mat karo
           → Stopped instance = charge nahi lagta
           → Terminated instance = data gone

NAT Gateway → FREE nahi — banaya toh charge lagega
           → Abhi mat banao

EBS        → Free Tier mein 30 GB free hai
           → 30 GB se zyada banaya toh charge lagega

Elastic IP → EC2 se attached ho toh FREE
           → EC2 se detached ho toh charge lagega
           → Use ke baad release karo
```

---

## Practice Ke Liye Safe — Ye Karo

```
✓ EC2 start/stop karo
✓ S3 mein files upload/delete karo
✓ Security Groups rules add/remove karo
✓ VPC explore karo
✓ CloudWatch metrics dekho
✓ IAM users/groups banao/delete karo
✓ EBS snapshots banao
```

---

## Abhi Mat Banao — Chargeable Hai

```
✗ NAT Gateway     → Chargeable — abhi zaroorat nahi
✗ Elastic IP      → EC2 se detach hone pe chargeable
✗ RDS Multi-AZ    → Double cost — practice mein OFF rakho
✗ Large EC2       → t3.micro se bada = chargeable
```

---

## Charge Check Karne Ka Tarika

**AWS Console → Billing → Bills** — yahan dekho kitna charge hua.

**AWS Console → Billing → Free Tier** — yahan dekho kitni Free Tier limit bacha hai.

Roz nahi, **hafte mein ek baar** check karo — sab theek rahega.

---

## Agar Accidentally Charge Ho Jaye

1. Turant AWS Console → Billing → Bills check karo
2. Kaun si service charge kar rahi hai wo dekho
3. Wo resource band karo ya delete karo
4. Pehli baar mostly AWS refund kar deta hai — Support se contact karo

---

## Key Rule — Hamesha Yaad Rakho

```
Practice ke baad → EC2 aur RDS STOP karo
Kaam khatam → Resources check karo
Naya kuch banao → Free Tier mein hai ya nahi pehle dekho
```
