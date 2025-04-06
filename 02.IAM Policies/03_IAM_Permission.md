
# 🔐 AWS IAM Permissions & Policies

## 📘 What are IAM Permissions?

IAM Permissions control **who can do what** on which AWS resources.  
They are defined using **Policies**, written in **JSON**.

> ✅ Permissions = Identity + Allowed Actions + Resources + Conditions

---

## 📄 What is an IAM Policy?

An IAM Policy is a **JSON document** attached to:
- A **User**
- A **Group**
- A **Role**

It defines **allowed or denied** actions for AWS services.

---

## 🧱 IAM Policy Structure (JSON Breakdown)

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",                    // ✅ Allow or Deny
      "Action": "s3:PutObject",             // 🔧 What action? (e.g., upload file to S3)
      "Resource": "arn:aws:s3:::my-bucket/*" // 🎯 On which resource?
    }
  ]
}
```

### 🔍 Breakdown:
| Field     | Meaning                                                                 |
|-----------|-------------------------------------------------------------------------|
| `Version` | Policy language version (always use `2012-10-17`)                      |
| `Effect`  | Either `"Allow"` or `"Deny"`                                            |
| `Action`  | What operations are allowed (e.g., `s3:ListBucket`, `ec2:StartInstances`)|
| `Resource`| Specific resource(s) the action applies to (using ARN)                 |

---

## 🧪 Example Scenarios

### 1️⃣ **Allow read-only access to all S3 buckets**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:ListBucket"
      ],
      "Resource": "*"
    }
  ]
}
```

### 2️⃣ **Allow EC2 instance start/stop on specific instance**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:StartInstances",
        "ec2:StopInstances"
      ],
      "Resource": "arn:aws:ec2:us-east-1:123456789012:instance/i-0abcd1234efgh5678"
    }
  ]
}
```

---

## 🔐 Deny Example

Explicitly deny access to delete an S3 bucket (even if other policies allow it):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "s3:DeleteBucket",
      "Resource": "*"
    }
  ]
}
```

> ❗ IAM always honors **explicit denies** over allows.

---

## ⚠️ IAM Permissions Best Practices

### ✅ Apply the **Principle of Least Privilege**
- Give **only the permissions** needed to perform a task
- Start with **read-only access** and expand when necessary

### 🛡️ Avoid Using `"Resource": "*"` When Not Required
- It can grant access to **all resources**, which is dangerous

### 🔐 Use Conditions to Add Security
- Example: Allow only if MFA is used or access is from specific IP

```json
"Condition": {
  "Bool": {
    "aws:MultiFactorAuthPresent": "true"
  }
}
```

### 👥 Prefer Group or Role-based Permissions Over User-based
- Easier to manage access
- Scalable for large teams

### 📦 Use Managed Policies Where Applicable
- AWS provides predefined **Managed Policies** like:
  - `AmazonS3ReadOnlyAccess`
  - `AmazonEC2FullAccess`

---

## 📋 Summary

| Concept     | Description                                           |
|-------------|-------------------------------------------------------|
| Policy      | JSON defining permissions                             |
| Effect      | Allow or Deny                                         |
| Action      | AWS operations like `s3:GetObject`, `ec2:StartInstances` |
| Resource    | AWS resource in ARN format                            |
| Condition   | (Optional) Further restrict access (MFA, IP, tags)    |

---

## 💡 Pro Tip

> 🧠 Use the **IAM Policy Simulator** to test policies before attaching them!
