# 🔐 Security Groups at Load Balancers (ELB)

## 📌 What is a Security Group?

A **Security Group** in AWS acts like a **virtual firewall** that controls **inbound** and **outbound** traffic **to AWS resources**, including:

- EC2 Instances
- Load Balancers (ALB/NLB)
- RDS Databases
- Lambda functions (via VPC)

They work at the **instance level**, not at the subnet level (unlike NACLs).

---

## 🚦 Security Groups on Load Balancers

### ✅ Load Balancers (especially ALB) **can and should** have Security Groups attached.

#### 🧱 Key Points:
- **Application Load Balancer (ALB)** supports Security Groups.
- **Network Load Balancer (NLB)** does **NOT** support Security Groups (because it works at Layer 4).
- **Classic Load Balancer** also supports Security Groups.

---

## 🔄 How Traffic Flows with Security Groups

### 📶 Incoming Request Flow:

```text
[Internet]
     ↓
[Security Group of ALB]
     ↓
[ALB Listener (e.g., HTTP/HTTPS)]
     ↓
[Target Group (EC2 instances)]
     ↓
[Security Group of EC2 Instance]
```

> Both the **ALB’s Security Group** and the **EC2’s Security Group** must allow traffic for communication to succeed.

---

## 🛡️ Example Use Case

Let’s say you are deploying a web application:

### Step 1: ALB Security Group
- Inbound:
  - Port 80 (HTTP) from 0.0.0.0/0 (for public access)
  - Port 443 (HTTPS) from 0.0.0.0/0
- Outbound:
  - Port 80/443 to the instances in Target Group

### Step 2: EC2 Security Group
- Inbound:
  - Port 80 only from **ALB Security Group**
- Outbound:
  - All traffic (default)

📌 This setup ensures:
- Only ALB can access the EC2 instances
- The instances are **not exposed to the internet directly**

---

## 🧪 Sample Setup

### ALB Security Group Rule

| Type   | Protocol | Port | Source         |
|--------|----------|------|----------------|
| HTTP   | TCP      | 80   | 0.0.0.0/0      |
| HTTPS  | TCP      | 443  | 0.0.0.0/0      |

### EC2 Security Group Rule

| Type   | Protocol | Port | Source                   |
|--------|----------|------|--------------------------|
| HTTP   | TCP      | 80   | SG-ID of the ALB         |

---

## 🚫 Common Mistakes

- ❌ Allowing `0.0.0.0/0` on EC2 directly — defeats the purpose of using a Load Balancer.
- ❌ Not allowing traffic from ALB’s Security Group in EC2 Security Group — causes **504 Gateway Timeout**.

---

## 📌 Summary

| Component         | Needs Security Group? | Notes                              |
|------------------|------------------------|------------------------------------|
| ALB              | ✅ Yes                 | Controls traffic into the ALB      |
| EC2 in Target Group | ✅ Yes              | Must allow traffic from ALB SG     |
| NLB              | ❌ No                  | Operates at Layer 4 (TCP/UDP)      |

> 🔐 Always restrict access using **Security Groups** for better control and security.

