# Day 08 — EBS (Elastic Block Storage)

## EBS Kya Hai

EBS ek virtual hard disk hai jo tumhare EC2 server se attach hoti hai. Jaise tumhare laptop mein hard disk hoti hai — bilkul waisa hi, bas ye AWS pe cloud mein hoti hai. Jab tum EC2 instance banate ho toh automatically ek EBS volume attach hoti hai — usi pe OS installed hota hai, tumhara application hota hai, aur data hota hai.

---

## EBS Ki Khaasiyat

**EC2 se alag exist karti hai:**
EBS volume aur EC2 instance alag alag hain. Agar tum EC2 instance terminate karo toh bhi EBS volume reh sakti hai — data safe rahega. Phir wo volume kisi doosre EC2 se attach kar sakte ho.

**Ek EC2 pe multiple EBS volumes:**
Ek EC2 server pe ek se zyada EBS volumes attach kar sakte ho. Jaise ek volume OS ke liye, ek volume database ke liye, ek volume logs ke liye.

**Same AZ mein hona zaroori:**
EBS volume aur EC2 instance same Availability Zone mein hone chahiye. Mumbai ap-south-1a ka EC2 sirf ap-south-1a ki EBS use kar sakta hai.

---

## EBS Volume Types

**gp3 — General Purpose SSD**
Sabse zyada use hone wala type. Normal applications, OS, databases ke liye. Fast hai aur reasonable cost hai. Ye default type hai.

**io2 — Provisioned IOPS SSD**
Jab bahut zyada fast storage chahiye — jaise large databases jo lakhs of transactions handle karein. Mehanga hai lekin bahut fast hai.

**st1 — Throughput Optimized HDD**
Jab bahut bada data process karna ho — jaise log files, big data. SSD se slow hai lekin bahut sasta hai.

**sc1 — Cold HDD**
Sabse sasta. Rarely access hone wale data ke liye. Backup storage ke liye use karte hain.

---

## EBS Snapshot Kya Hai

Snapshot EBS volume ka backup hai. Ek particular time pe volume ki exact copy S3 mein save ho jaati hai.

**Kyun zaroori hai:**
Agar tumhara EC2 crash ho gaya ya data corrupt ho gaya toh snapshot se exact usi state pe wapas aa sakte ho jab snapshot liya tha.

**Kaise kaam karta hai:**
Pehla snapshot poora volume copy karta hai. Uske baad ke snapshots sirf changes save karte hain — isliye fast aur sasta hota hai. Snapshot se naya EBS volume bana sakte ho — chahe kisi bhi AZ mein, chahe kisi bhi region mein.

---

## EBS vs Instance Store

| | EBS | Instance Store |
|---|---|---|
| Type | Network attached | Physically attached |
| Speed | Fast | Bahut fast |
| Data EC2 band hone pe | Safe rehta hai | Hamesha delete ho jaata hai |
| Use case | OS, Database, Apps | Temporary cache only |

Instance Store ka data EC2 stop ya terminate hone pe hamesha delete ho jaata hai — isliye important data kabhi Instance Store mein mat rakho.

---

## Architecture mein EBS Kahan Fit Hota Hai

```
EC2 Instance (Web Server)
    └── EBS Volume gp3 — OS + Application

EC2 Instance (Database Server)
    ├── EBS Volume gp3  — OS
    └── EBS Volume io2  — Database files (fast storage chahiye)

Backup System
    └── EBS Snapshots → S3 mein store hote hain
```

---

## Aaj Ka Hands-On — Kya Kiya

**Step 1:** EC2 Console → Volumes mein gaye

**Observations:**
- Volume ID: vol-09373d42bc81c2609
- Type: gp3
- Size: 8 GiB
- State: In-use
- Availability Zone: ap-south-1a
- Attached to: MyFirstServer (EC2 instance)
- Root volume: /dev/xvda

**Step 2:** Snapshot banaya
- Actions → Create Snapshot
- Description: Day 8 - My First Snapshot
- Snapshot ID: snap-0ddca8907c6629d1f
- State: Completed ✓

---

## Interview Questions & Answers

**Q1. What is Amazon EBS and how is it different from instance store?**

Amazon EBS, or Elastic Block Storage, is a persistent block storage service designed for use with Amazon EC2 instances. It functions like a virtual hard disk that can be attached to an EC2 instance. The key difference between EBS and instance store is persistence. EBS volumes are network-attached and retain data independently of the EC2 instance lifecycle, meaning if you stop or terminate an EC2 instance, the EBS volume can persist and be reattached to another instance. Instance store, on the other hand, is physically attached to the host machine and is ephemeral — all data is permanently lost when the instance is stopped or terminated. For this reason, EBS is used for critical data like operating systems, databases, and application files, while instance store is only used for temporary cache or scratch data.

---

**Q2. What is an EBS Snapshot and why is it important?**

An EBS Snapshot is a point-in-time backup of an EBS volume that is stored in Amazon S3. It captures the exact state of the volume at the moment the snapshot is taken. Snapshots are incremental, meaning the first snapshot copies the entire volume, but subsequent snapshots only store the changes made since the last snapshot, which makes them efficient in terms of both time and cost. Snapshots are important because they allow you to restore data in case of accidental deletion or corruption, create new EBS volumes from a snapshot in any Availability Zone or Region, and copy data across regions for disaster recovery purposes.

---

**Q3. What EBS volume type would you choose for a high-performance database?**

For a high-performance database that requires very high IOPS and low latency, I would choose the io2 volume type, also known as Provisioned IOPS SSD. This volume type allows you to provision a specific number of IOPS independently of the volume size, which ensures consistent and predictable performance for I/O intensive workloads like large relational databases. For a general-purpose application or a smaller database, gp3 would be sufficient and more cost-effective.

---

**Q4. Can you attach one EBS volume to multiple EC2 instances?**

By default, an EBS volume can only be attached to one EC2 instance at a time. However, AWS does offer a feature called EBS Multi-Attach, which allows a single io1 or io2 volume to be attached to multiple EC2 instances simultaneously, but only within the same Availability Zone. This is used for specialized workloads that require shared storage with concurrent write access. For most standard use cases, each EC2 instance has its own dedicated EBS volume.

---

**Q5. Why must an EBS volume and EC2 instance be in the same Availability Zone?**

An EBS volume must be in the same Availability Zone as the EC2 instance it is attached to because EBS volumes are physically located within a specific AZ and are connected to EC2 instances over a high-speed network within that AZ. There is no direct network path between an EBS volume in one AZ and an EC2 instance in another AZ. If you need to move a volume to a different AZ, you must first create a snapshot of the volume and then create a new volume from that snapshot in the desired Availability Zone.

---

## Key Points — Phone Pe Save Karo

```
EBS     = EC2 ka virtual hard disk
gp3     = Default SSD — normal use ke liye
io2     = Fast SSD — high performance database ke liye
st1     = HDD — big data, log files ke liye
sc1     = Cheapest — rarely access data ke liye
Snapshot = EBS ka backup — S3 mein store hota hai
Instance Store = Temporary — EC2 band hua toh data gone
Same AZ = EBS aur EC2 hamesha same AZ mein hone chahiye
```
