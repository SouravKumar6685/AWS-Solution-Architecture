# 🛠️ Hands-On: Create Gateway Load Balancer (GWLB) using AWS Console

## 🎯 Objective

We’ll set up a **Gateway Load Balancer (GWLB)** to route traffic through **virtual appliances** (e.g., firewalls). This makes traffic inspection or network security easy and scalable.

---

## ✅ Prerequisites

- A VPC with at least **two public subnets** (in different AZs)
- EC2 instances acting as **appliances** (firewall/proxy)
- Security groups that allow **GENEVE (Port 6081)**
- IAM permissions for Load Balancer, VPC, and EC2

---

## 🧱 Step-by-Step Guide

---

### 🔹 Step 1: Create a GWLB Target Group

1. Go to **EC2 Dashboard → Target Groups**
2. Click **"Create target group"**
3. Select **Target type**: `Instances`
4. Choose **Protocol**: `GENEVE`
5. Port: `6081`
6. Name it: `gwlb-appliance-tg`
7. Select the appropriate **VPC**
8. Click **Next**
9. Register your **EC2 appliances**
10. Click **Create target group**

---

### 🔹 Step 2: Launch EC2 Instances (Optional)

If not already done:
1. Launch EC2 instances (Linux/Windows)
2. These will act as **security appliances**
3. Ensure **inbound rule** allows GENEVE traffic:
   - Protocol: `Custom UDP`
   - Port Range: `6081`
   - Source: VPC CIDR / ALB Subnet CIDR

---

### 🔹 Step 3: Create Gateway Load Balancer

1. Go to **EC2 Dashboard → Load Balancers**
2. Click **"Create Load Balancer"**
3. Select **"Gateway Load Balancer"**
4. Enter a name: `My-GWLB`
5. Scheme: **Internal**
6. IP address type: **IPv4**
7. Add listeners:
   - Protocol: `GENEVE`
   - Port: `6081`
8. Select at least **2 subnets** (from different AZs)
9. Choose previously created **target group**
10. Click **Create Load Balancer**

---

### 🔹 Step 4: Create VPC Endpoint Service (Consumer Access)

1. Go to **VPC Dashboard → Endpoint Services**
2. Click **"Create Endpoint Service"**
3. Select the GWLB you just created
4. Enable:
   - **Require acceptance**
   - **Supported IP address types** (IPv4)
5. Share with specific AWS accounts or org
6. Click **Create service**

---

### 🔹 Step 5: Consumer Creates a Gateway Load Balancer Endpoint

1. Go to the **consumer VPC**
2. Navigate to **VPC → Endpoints**
3. Click **"Create Endpoint"**
4. Choose **type**: `Gateway Load Balancer`
5. Enter the **service name** (shared from provider)
6. Select the **subnets**
7. Attach it to a **route table** (to route traffic through this endpoint)
8. Click **Create endpoint**

---

## 🧠 Understanding Routing

🔁 Update **Route Tables** to send traffic to GWLB endpoint for specific destinations (e.g. all internet-bound traffic or inter-VPC traffic)

```bash
Destination: 0.0.0.0/0
Target: VPCE (Gateway Load Balancer Endpoint)
```

---

## 🎁 Bonus Tips

- Use **CloudWatch** to monitor appliance health
- Add **Auto Scaling** to your target group for scalability
- Keep **GENEVE port (6081)** open between GWLB and appliances
- Use **ENIs (Elastic Network Interfaces)** to inspect traffic inside appliances

---

## ✅ Benefits of GWLB Setup

- Centralized **traffic inspection**
- Multi-account **security enforcement**
- Appliance scaling without manual routing
- No user needs to know about firewalls — they’re transparent!

