
# 🧪 AWS IAM Hands-On Demo: Users, Groups & Permissions

## 🎯 Objective

Create 3 IAM groups and assign users as follows:

| Group           | Users                      |
|-----------------|----------------------------|
| 👩‍💻 Developer     | Alice, Bob, Charles         |
| 🕵️‍♂️ Audit Team    | Charles, David             |
| 🛠️ Operations Team | David, Edward              |

We'll organize them using **IAM Groups**, and later attach **policies** to groups to manage permissions effectively.

---

## 🛠️ Step-by-Step Setup

### 🔹 Step 1: Create IAM Users

Go to **IAM Console → Users → Add Users**:

1. Create user: `Alice`
2. Create user: `Bob`
3. Create user: `Charles`
4. Create user: `David`
5. Create user: `Edward`

> Choose **"Programmatic access"** and/or **"AWS Management Console access"** as needed.

---

### 🔹 Step 2: Create IAM Groups

Navigate to **IAM Console → User Groups → Create Group**:

1. Group: `Developer-Team`
2. Group: `Audit-Team`
3. Group: `Operations-Team`

---

### 🔹 Step 3: Add Users to Groups

#### 👩‍💻 Developer-Team:
- Add: Alice, Bob, Charles

#### 🕵️‍♂️ Audit-Team:
- Add: Charles, David

#### 🛠️ Operations-Team:
- Add: David, Edward

> 🧠 Note: A single user (e.g., Charles or David) can belong to **multiple groups**.

---

## 🔐 Step 4: Attach IAM Policies to Groups

You can attach predefined or custom policies based on group responsibilities.

### 👩‍💻 Developer-Team Policy
Attach policy: `AmazonEC2FullAccess` or custom dev policy  
> Allows developers to create, manage, and terminate EC2 instances.

### 🕵️‍♂️ Audit-Team Policy
Attach policy: `ReadOnlyAccess`  
> Ensures they can **view** all services and logs but cannot modify anything.

### 🛠️ Operations-Team Policy
Attach policy: `AmazonS3FullAccess`, `AmazonCloudWatchFullAccess`, etc.  
> They monitor, manage logs, and access S3 buckets for backups.

---

## 🧠 Best Practices

- ✅ Use **groups** for permission management – easier than assigning permissions to each user.
- ✅ Follow **Least Privilege** principle.
- ✅ Use **naming conventions** for clarity: e.g., `DevTeam-EC2-Access`.

---

## 📋 Summary Table

| User     | Belongs To           | Sample Access                       |
|----------|----------------------|-------------------------------------|
| Alice    | Developer-Team       | EC2 full access                     |
| Bob      | Developer-Team       | EC2 full access                     |
| Charles  | Developer, Audit     | EC2 full + ReadOnly across AWS      |
| David    | Audit, Operations    | ReadOnly + CloudWatch, S3 access    |
| Edward   | Operations-Team      | S3 and monitoring full access       |

---

## 🎯 Final Outcome

- Centralized permission control using groups
- Role-based access for different teams
- Easy user management at scale

