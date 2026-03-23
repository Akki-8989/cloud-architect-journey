# DAY 3 — S3 (Simple Storage Service)
**Date: 21 March 2026 | Student: Akash | Mentor: Claude AI**

---

## WHAT IS S3?
- AWS ka unlimited, safe, cheap storage service
- Full form: Simple Storage Service
- 99.999999999% durability (11 nines) — data kabhi nahi jaata
- 3 Availability Zones mein automatically backup hota hai
- Pay only for what you use

---

## CORE CONCEPTS

### Bucket
- Ek container jahan files rakhi jaati hain
- Naam globally unique hona chahiye
- By default private hota hai
- Region specific hota hai

### Object
- S3 mein rakhi hui koi bhi file
- Max size: 5 TB per file
- Images, videos, PDFs, backups — kuch bhi

### Key
- File ka path/name S3 mein
- Example: s3://akash-documents/2026/march/invoice.pdf

---

## STORAGE CLASSES

| Class | Use Case | Speed | Cost |
|-------|----------|-------|------|
| Standard | Roz access hone wala data | Fast | High |
| Intelligent Tiering | Mixed access | Auto | Auto |
| Standard-IA | Monthly access | Medium | Medium |
| Glacier | Yearly access | 3-5 hrs | Low |
| Glacier Deep Archive | Compliance data | 12 hrs | Very Low |

---

## KEY FEATURES

### Versioning
- File update karo — purana version bhi save rahega
- Galti se delete kiya — recover kar sakte ho
- Architect must enable karo — data safety ke liye

### Static Website Hosting
- HTML, CSS, JS upload karo — website ready!
- EC2 se 90% sasta
- React/Angular frontend yahan host karo

### Lifecycle Policy
- Automatic rules set karo
- 30 days → Standard-IA
- 90 days → Glacier
- 1 year → Delete
- Cost automatically optimize hoti hai

### Access Control
- Bucket Policy — JSON rules
- ACL — Object level permissions
- Pre-signed URLs — Temporary access links

---

## INTERVIEW QUESTIONS & ANSWERS

**Q1: S3 kya hai?**
> AWS ka object storage service — unlimited storage, 99.999999999% durability, pay as you use

**Q2: S3 bucket name unique kyun hona chahiye?**
> S3 bucket names globally unique hote hain kyunki har bucket ka ek unique URL hota hai: bucket-name.s3.amazonaws.com

**Q3: S3 Standard vs Glacier?**
> Standard = Frequently accessed data, fast retrieval. Glacier = Archive data, 3-5 hour retrieval, 90% cheaper

**Q4: S3 versioning kyun enable karein?**
> Accidental deletion ya overwrite se bachne ke liye. Previous versions restore kar sakte ho

**Q5: S3 vs EBS vs EFS?**
> S3 = Object storage (files), EBS = Block storage (server disk), EFS = File storage (shared between servers)

**Q6: Static website S3 pe kyun host karein?**
> EC2 se 90% sasta, no server management, automatic scaling, highly available

**Q7: Lifecycle policy kya hai?**
> Automatic rules jo files ko automatically cheaper storage class mein move karti hain time ke saath

**Q8: Pre-signed URL kya hai?**
> Temporary URL jo private S3 object ko limited time ke liye access deta hai bina bucket public kiye

---

## KEY POINTS — YAAD RAKHO
```
1. S3 = Object Storage (files ke liye)
2. Bucket = Container, Object = File, Key = Path
3. 11 nines durability = 99.999999999%
4. Default = Private — hamesha!
5. Storage classes = Cost optimization
6. Versioning = Data safety
7. Lifecycle = Automatic cost saving
```

---
*Day 3 Complete | Next: Day 4 — RDS Database*
