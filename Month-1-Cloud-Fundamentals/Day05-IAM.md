# DAY 5 — IAM (Identity and Access Management)
**Date: 26 March 2026 | Student: Akash | Mentor: Claude AI**

---

## WHAT IS IAM?

IAM stands for Identity and Access Management. It is a service provided by AWS that allows you to control who can access your AWS account and what actions they can perform. Using IAM, you can create separate identities for each person or service that needs access to AWS, and you can define exactly what each identity is allowed to do.

---

## THE PROBLEM IAM SOLVES

When you create an AWS account, you get one Root account that has complete access to everything. If you share this account with your team members, anyone can accidentally or intentionally delete databases, modify billing, or shut down servers.

IAM solves this by allowing you to create separate users with limited, controlled access — so each person only gets access to what they need for their job.

---

## 4 MAIN COMPONENTS OF IAM

### 1. IAM Users
A separate identity created for each person who needs AWS access. Each user has their own username and password. You can give each user specific permissions based on their role.

### 2. IAM Groups
A collection of IAM users. Instead of giving permissions to each user individually, you give permissions to a group once, and all users in that group automatically get those permissions.

### 3. IAM Policies
A document that defines what is allowed and what is not allowed. AWS provides ready-made policies like "AmazonEC2FullAccess" or you can create custom policies.

### 4. IAM Roles
Temporary permissions given to AWS services or users. Unlike users, roles are not for specific people — they are for services like EC2 that need to access other AWS services.

---

## ROOT ACCOUNT vs IAM USER

| | Root Account | IAM User |
|--|-------------|----------|
| Access | Full — everything | Limited — only what you define |
| Use | Only for billing and account settings | Daily work |
| Best Practice | Enable MFA, never use daily | Use for all regular tasks |

---

## PRINCIPLE OF LEAST PRIVILEGE

Give only the minimum permissions required to perform a specific job — nothing more, nothing less. This ensures that if an account is compromised, the damage is limited.

---

## HANDS-ON — AAJ KYA KIYA

**AWS Console → IAM → User Groups → Create group**

```
Step 1: Group banaya
        Group name  : Developers
        Policy      : AmazonEC2FullAccess attach ki

Step 2: IAM User banaya
        AWS Console → IAM → Users → Create user
        Username    : akash-dev-user
        Access      : AWS Console access enable kiya
        Password    : Custom password set kiya

Step 3: User ko Group mein add kiya
        akash-dev-user → Developers group mein add kiya

Result: akash-dev-user sirf EC2 access kar sakta hai
        S3, RDS, billing — kuch nahi ✓
```

**Principle of Least Privilege applied:**
```
Developers group = Sirf EC2FullAccess
akash-dev-user   = Group ke through sirf EC2 access
Root Account     = Daily use nahi kiya ✓
```

---

## INTERVIEW QUESTIONS & ANSWERS

---

**Q1. What is IAM in AWS and why is it important?**

> IAM stands for Identity and Access Management. It is a service that allows you to manage access to AWS resources securely. It is important because it enables you to control who is authenticated and authorized to use AWS resources. Instead of sharing the root account, you can create individual IAM users with specific permissions, which improves security and accountability.

---

**Q2. What is the difference between IAM Users, Groups, Roles, and Policies?**

> IAM Users are individual identities created for people who need AWS access. Groups are collections of users that share the same permissions — you assign permissions to the group, and all users in that group inherit those permissions. Policies are documents that define what actions are allowed or denied on which AWS resources. Roles are similar to users but are meant for AWS services or temporary access — for example, giving an EC2 instance permission to access S3.

---

**Q3. What is the Principle of Least Privilege?**

> The Principle of Least Privilege means giving users, services, or applications only the minimum permissions they need to perform their specific job — nothing more, nothing less. For example, if a developer only needs to work with EC2, they should only have EC2 access and not access to RDS, S3, or billing. This reduces the security risk in case an account is compromised.

---

**Q4. What is the difference between Root User and IAM User?**

> The Root User is the account created when you first sign up for AWS. It has unrestricted access to all AWS services and billing. The IAM User is a separate identity created by you with limited, controlled permissions. Best practice is to enable MFA on the Root account, avoid using it for daily tasks, and instead create an IAM Admin user for regular work.

---

**Q5. What is MFA and why should you enable it on the Root account?**

> MFA stands for Multi-Factor Authentication. It adds a second layer of security beyond just a password. When MFA is enabled, logging in requires both the password and a one-time code generated on your phone. Even if a hacker gets your password, they cannot log in without your phone. It is strongly recommended to enable MFA on the Root account because it has full access to everything in AWS.

---

**Q6. What is an IAM Role and when would you use it?**

> An IAM Role is a set of permissions that can be assumed by an AWS service or a user temporarily. Unlike IAM Users, roles are not tied to a specific person. For example, if an EC2 instance needs to read files from S3, instead of storing credentials on the server, you attach an IAM Role to the EC2 instance that grants S3 read access. This is more secure because no passwords or access keys are stored on the server.

---

**Q7. What is the difference between IAM Policies — AWS Managed vs Customer Managed?**

> AWS Managed Policies are pre-built policies created and maintained by AWS for common use cases, such as AmazonS3FullAccess or AmazonEC2ReadOnlyAccess. Customer Managed Policies are custom policies that you create yourself when the AWS managed policies do not meet your specific requirements. AWS Managed Policies are easier to use but less flexible, while Customer Managed Policies give you full control.

---

**Q8. Why should you not use the Root account for daily tasks?**

> The Root account has unrestricted access to everything in AWS including billing, account closure, and all services. If the Root account credentials are compromised, an attacker can do irreversible damage including deleting all resources and changing payment methods. Best practice is to create an IAM Admin user for daily tasks and keep the Root account credentials secure with MFA enabled.

---

## KEY POINTS TO REMEMBER

```
1. IAM = Control who accesses what in AWS
2. Root = Full access — use only for billing/setup
3. IAM User = Limited access — use for daily work
4. Group = Assign permissions once, apply to many users
5. Policy = Document defining allow/deny rules
6. Role = Temporary permissions for services
7. MFA = Always enable on Root account
8. Principle of Least Privilege = Minimum required access only
```

---
*Day 5 Complete | Next: Day 6 — VPC (Virtual Private Cloud)*
