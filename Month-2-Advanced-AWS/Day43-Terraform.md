# Day 43 — Terraform (Infrastructure as Code)

## Problem — Manual Console Clicking

```
Abhi tak kya kiya:
AWS Console → Click → EC2 bana
AWS Console → Click → S3 bana
AWS Console → Click → ECS bana

Problems:
❌ 50 servers banana = 50 baar manually click karo
❌ Doosre region mein same setup = phir se sab click
❌ Team member ne kuch delete kiya = kya tha yaad nahi
❌ Dev aur Prod same hain ya nahi = pata nahi
❌ Audit = "kab kya banaya?" = koi record nahi

Real companies mein 100s of resources = Manually impossible!
```

---

## Solution — Terraform (Infrastructure as Code)

```
Code likhke infrastructure banao — click nahi karo.

Pehle:
  AWS Console → Manual click → EC2 bana

Terraform ke saath:
  Code likho → terraform apply → EC2 ban gaya ✅

"as Code" ka matlab:
  → Blueprint code mein hota hai
  → Git mein save = history rehti hai
  → Dobara run karo = same infrastructure banta hai
  → Team ke saath share karo = sab same setup banate hain
```

---

## Analogy — Recipe Card

```
Manual (Console clicking):
  Har roz khud kitchen mein jaao
  → Ingredients lo → Cook karo → Plate karo
  Kal phir same kaam karo ❌

Terraform (IaC):
  Recipe card likho ek baar:
  "2 EC2, 1 S3, 1 RDS chahiye"
  → Chef (Terraform) padhega
  → Sab automatically ban jaayega ✅

  Recipe same = Har baar same output ✅
  Recipe change karo = Infrastructure update ✅
```

---

## 4 Main Commands

### 1. terraform init
```
Kya karta hai: AWS plugin (provider) download karta hai.
Kyun zaroori: Bina plugin ke Terraform AWS se baat nahi kar sakta.
Kab chalao: Pehli baar, ya provider change hone pe.
Output: "Terraform has been successfully initialized!" ✅

(Jaise npm install — dependencies lata hai)
```

### 2. terraform plan
```
Kya karta hai: Preview dikhata hai — kya banega, kya badlega, kya delete hoga.
Kyun zaroori: Apply karne se pehle confirm karo ki sahi hai.
Actually kuch nahi karta — sirf preview!
Output: "Plan: 1 to add, 0 to change, 0 to destroy" ✅

(Jaise order confirm karne se pehle cart check karna)
```

### 3. terraform apply
```
Kya karta hai: Actually resources AWS mein banata hai.
Kyun zaroori: Plan sirf preview tha — apply se actual kaam hota hai.
Process: Plan dikhata hai → "yes" type karo → Resources ban jaate hain
Output: "Apply complete! Resources: 1 added" ✅
```

### 4. terraform destroy
```
Kya karta hai: Terraform ne jo banaya sab delete kar deta hai.
Kyun zaroori: Cleanup — bill nahi aana chahiye.
Process: "yes" type karo → Sab delete
Output: "Destroy complete! Resources: 1 destroyed" ✅
```

---

## Architecture — Kaise Kaam Karta Hai

```
main.tf (tumhara code)
┌──────────────────────────┐
│ provider "aws" {         │
│   region = "ap-south-1"  │
│ }                        │
│                          │
│ resource "aws_s3_bucket" │
│   bucket = "akash-demo"  │
└──────────┬───────────────┘
           │
           ↓ terraform apply
      Terraform
      (AWS ka plugin)
           │
           ↓ AWS API calls automatically
    ┌─────────────┐
    │    AWS      │
    │ S3 bana ✅  │
    └─────────────┘
```

---

## Hands-On — Aaj Kya Kiya

### Step 1 — Terraform Install (CloudShell mein)
```bash
# Install kiya:
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo
sudo yum install -y terraform

# Verify kiya:
terraform --version
# Output: Terraform v1.15.8 ✅
```

### Step 2 — Project folder banaya
```bash
mkdir terraform-demo && cd terraform-demo
# mkdir = folder banao
# cd = us folder mein jaao
```

### Step 3 — main.tf file likhi
```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "ap-south-1"
}

resource "aws_s3_bucket" "demo" {
  bucket = "akash-terraform-demo-bucket-2026"
}
```

### Step 4 — Commands chalaye
```bash
terraform init     # AWS plugin download ✅
terraform plan     # Preview: 1 to add ✅
terraform apply    # S3 bucket ban gaya ✅
terraform destroy  # Cleanup: 1 destroyed ✅
```

### Step 5 — Verify kiya
- S3 Console mein `akash-terraform-demo-bucket-2026` dikha ✅
- terraform destroy ke baad bucket gone ✅

---

## Terraform vs Manual (Console)

| Feature | Manual (Console) | Terraform |
|---------|-----------------|-----------|
| Speed | Slow (click by click) | Fast (ek command) |
| Repeatability | Har baar manually | Same code = Same result |
| Error risk | High (human error) | Low (code same hai) |
| History | Koi record nahi | Git mein track hota hai |
| Team work | Mushkil | Easy (code share karo) |
| Multi-region | Bahut time | Variable change karo ✅ |

---

## WHY Framework — Terraform

**Kab use karu?**
- Production infrastructure manage karna ho
- Same setup dev/staging/prod mein chahiye
- Team ke saath kaam kar rahe ho
- 10+ resources manage karne hain

**Kab NA use karu?**
- Ek baar ka quick test → Console se fast hai
- Sirf ek resource → Overkill
- Learning phase → Console se samajhna easy hai

**Alternative kya tha, Terraform kyun chuna?**
- AWS CloudFormation bhi IaC tool hai (AWS ka apna)
- Terraform better kyunki:
  → Multi-cloud (AWS + Azure + GCP)
  → Simple syntax (HCL)
  → Huge community + modules available ✅

---

## Interview Questions & Answers

**Q1: What is Terraform and what problem does it solve?**
A: Terraform is an open-source Infrastructure as Code (IaC) tool by HashiCorp. It solves the problem of manual infrastructure management — instead of clicking through the AWS Console to create resources, you write code that describes what you want, and Terraform creates it automatically. This makes infrastructure repeatable, trackable in Git, and consistent across environments.

**Q2: What is the difference between terraform plan and terraform apply?**
A: `terraform plan` is a dry run — it shows what changes will be made without actually making them. It's like a preview or cart review before checkout. `terraform apply` actually executes those changes and creates/modifies/deletes resources in AWS. Always run plan before apply to verify changes.

**Q3: What is a provider in Terraform?**
A: A provider is a plugin that allows Terraform to interact with a specific cloud or service. The AWS provider lets Terraform make API calls to AWS to create resources like EC2, S3, RDS etc. You specify the provider in your .tf file and `terraform init` downloads it.

**Q4: What is terraform destroy used for?**
A: `terraform destroy` deletes all resources that were created by Terraform in the current project. It's used for cleanup — especially important in learning/testing environments to avoid unnecessary AWS costs.

**Q5: What is Infrastructure as Code (IaC)?**
A: Infrastructure as Code means managing and provisioning infrastructure through code files instead of manual processes. The infrastructure configuration is written in files (like main.tf), stored in version control (Git), and can be executed to create identical environments repeatedly. Benefits include consistency, repeatability, and full audit history.

---

## Key Points — Phone Pe Save Karo

```
1. Terraform = Code likhke AWS resources banao (no clicking!)
2. main.tf = Tumhara infrastructure ka blueprint
3. terraform init   = Plugin download karo (ek baar)
4. terraform plan   = Preview dekho (kuch nahi banta)
5. terraform apply  = Actually banao (yes type karo)
6. terraform destroy = Sab delete karo (cleanup)
7. IaC = Repeatable + Trackable + Team-friendly ✅
8. Terraform vs CloudFormation: Terraform = Multi-cloud ✅
```
