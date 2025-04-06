
# 🔐 AWS IAM Policies, Users, Roles & Permissions

## 📌 What is IAM?

**IAM (Identity and Access Management)** is AWS's service for **managing access to AWS resources** securely.  
It helps you **control who is authenticated and authorized** to use services.

---

## 👤 IAM Users vs IAM Roles

### ✅ IAM User
- Represents a **person or service** that interacts with AWS
- Has **long-term credentials**: username, password, access keys
- Used for **individual login**

> 🧪 Example: A developer logging into the AWS Console or using the AWS CLI

---

### 🎭 IAM Role
- An **identity with specific permissions** meant to be **assumed temporarily**
- Has **no credentials of its own**
- Ideal for services like **EC2, Lambda, ECS**, etc.

> 🧪 Example: An EC2 instance uploading files to an S3 bucket by assuming a role with S3 permissions

![](https://imgs.search.brave.com/cFkbMxX4EKu1STqrOpPokkGNyoY3ZXcR8nLHhdad9Sc/rs:fit:860:0:0:0/g:ce/aHR0cHM6Ly9yZXMu/Y2xvdWRpbmFyeS5j/b20vaHk0a3lpdDJh/L2ZfYXV0byxmbF9s/b3NzeSxxXzcwL2xl/YXJuL21vZHVsZXMv/YXdzLWlkZW50aXR5/LWFuZC1hY2Nlc3Mt/bWFuYWdlbWVudC91/bmRlcnN0YW5kLWlh/bS1yb2xlcy9pbWFn/ZXMvMmMxNmU0NjUz/YTlhZWFlOTcxYzk2/NmM1NDAxYWY1Zjlf/ZC01MS1iLTEtYi05/LWYtODEzLWMtNDMy/NS1hLTI4LWUtNTI2/OTEtY2EtOS1lZmEt/NC5wbmc)
---

## 🔏 What is an IAM Policy?

An **IAM policy** is a **JSON document** that defines **what actions are allowed or denied** for which resources.

### ✍️ Sample IAM Policy (Allow EC2 Read Only):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ec2:Describe*",
      "Resource": "*"
    }
  ]
}
```

### 🔍 Policy Breakdown:
- `Effect`: Allow or Deny
- `Action`: The operation (e.g., `s3:PutObject`, `ec2:StartInstances`)
- `Resource`: The AWS resource ARN

---

## 🛑 Why You Should **Never** Embed Credentials in EC2 Instances

### ❌ Bad Practice: Hardcoding AWS credentials

```bash
export AWS_ACCESS_KEY_ID="AKIA********"
export AWS_SECRET_ACCESS_KEY="abc123********"
```

### 🚨 Risks:
- 🔓 **Security Breach** if someone accesses the instance
- 🕵️‍♂️ **Leakage via logs or uploads** to GitHub
- 🔁 **Difficult to rotate** credentials
- ❗ **Non-compliant** with AWS best practices

---

## ✅ Best Practice: Use IAM Roles with EC2

- Attach IAM Role to EC2 when launching
- EC2 instance receives **temporary, auto-rotated credentials**
- Permissions are **granular and scoped**

> 🧠 Example:  
> Attach a role with `s3:PutObject` permission so EC2 can upload logs to S3 securely.

---

## 🔐 What is IAM Permission?

IAM permissions define **who can do what** on which AWS resources.

### Components of Permissions:
- ✅ **Principal** – The user/role the policy is attached to
- ✅ **Action** – Operation being allowed (e.g., `s3:GetObject`)
- ✅ **Resource** – The ARN (e.g., `arn:aws:s3:::my-bucket/*`)
- ✅ **Condition** – Optional filters (e.g., IP address, MFA used)

> 🧠 Example Permission:
> "User can read objects from `my-bucket` only when using MFA."



---

## 📋 Summary

| Concept       | Description |
|---------------|-------------|
| **IAM User**  | AWS identity with login & long-term credentials |
| **IAM Role**  | Temporary, credential-free identity for services |
| **Policy**    | JSON doc defining allowed/denied actions |
| **Permission**| Specific rights to perform operations on AWS |

---

## 🚀 Bonus Tip:
Always follow **least privilege principle**:  
> Give users/roles **only the permissions they actually need**, nothing more.
