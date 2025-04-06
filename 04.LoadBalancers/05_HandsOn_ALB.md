# 🔐 Securing EC2 with ALB: Isolate Instances & Route Traffic Only Through Load Balancer

This hands-on setup ensures that **all HTTP traffic must go through the Application Load Balancer (ALB)** instead of allowing direct access to EC2 instances.

---

## 🎯 Goal:
- **Users → ALB → Target Group → EC2**
- Restrict users from directly accessing EC2 via public IP/port.
- All traffic must **flow through ALB**, enhancing security and control.

---

## 🛠️ Hands-On Steps

### 1️⃣ Launch EC2 Instances (Target Group Members)
- Launch EC2 instance(s) in a **private subnet** or with a **security group** that has:
  - ❌ **NO inbound HTTP (port 80)** from anywhere (`0.0.0.0/0`)
  - ✅ Allow HTTP (port 80) **only from ALB’s security group**

### 2️⃣ Create a Security Group for ALB
- Inbound Rule:
  - ✅ Allow HTTP (port 80) from anywhere (`0.0.0.0/0`)
- Outbound Rule:
  - ✅ Allow traffic to EC2 instances (default is open)

### 3️⃣ Create Target Group
- Register EC2 instances into this target group
- **Protocol:** HTTP
- **Port:** 80

> ⚠️ Even though EC2 listens on port 80, the SG doesn't allow public traffic. Only ALB will be allowed to communicate.

### 4️⃣ Attach Security Group to EC2
- Inbound Rule:
  - ✅ Allow HTTP (port 80) **from ALB security group ONLY**
  - ❌ Remove any existing rule like HTTP from `0.0.0.0/0`

### 5️⃣ Create Application Load Balancer (ALB)
- **Internet-facing**
- Listens on port 80 (HTTP)
- Attach the ALB’s security group (created above)
- Forward requests to the target group (created earlier)

---

## ✅ Result

### 🔁 Flow:
```text
Client → ALB (port 80) → Target Group (port 80) → EC2
```

- Client cannot access EC2 directly on its public IP.
- EC2 instances are **isolated** behind ALB.

---

## 🔐 Benefits of This Architecture

| Benefit | Explanation |
|--------|-------------|
| ✅ Instance Isolation | EC2 is hidden from public access; improves security |
| 🔁 Centralized Traffic Control | All traffic flows through ALB – easier to manage |
| 🛡️ WAF/SSL/Firewall | You can attach **Web Application Firewall**, SSL termination, and monitoring on ALB |
| 🎯 Load Balancing | ALB can distribute traffic across multiple instances |
| 📈 Scaling | Works perfectly with Auto Scaling Groups |
| 🔎 Logging & Monitoring | Easier to trace requests, health checks, and analyze failures |

---

## 🧪 Test It

### ✅ Success Case:
- Access `http://<ALB-DNS>` → You should reach the app.

### ❌ Failure Case:
- Try `http://<EC2-Public-IP>` → Should **not** connect if SG is properly set.

---

## 💡 Pro Tips

- 📍 Use **private subnets** for EC2 + **NAT Gateway** for internet access if needed.
- 🔒 Pair with **IAM roles**, **CloudWatch**, and **WAF** for stronger architecture.
- 📦 Tag resources properly for maintainability.

Absolutely! Here's a complete hands-on walkthrough in **Markdown format** including **how to set this up in AWS Console**, step-by-step, with explanations and **real benefits**. 💻🌐

---

# 🔐 Isolating EC2 Access with ALB (Console Walkthrough)

In this setup, we will ensure **users can only access your application via ALB**, not directly through EC2’s public IP. This is a best practice for production environments to **improve security, control, and monitoring**.

---

## 🎯 Objective

> Force all HTTP traffic to go through an ALB and block direct access to EC2.

---

## 🛠 Step-by-Step Setup (AWS Console)

### 1️⃣ Launch EC2 Instance
1. Go to **EC2 Dashboard → Instances → Launch Instance**
2. Choose an **Amazon Linux 2** or Ubuntu AMI.
3. Choose a **public subnet** for simplicity (you can use private subnet + NAT in real setups).
4. **Enable HTTP server**:
   - Use a User Data script like:
     ```bash
     #!/bin/bash
     sudo yum install -y httpd
     sudo systemctl enable httpd
     sudo systemctl start httpd
     echo "<h1>Welcome from $(hostname -f)</h1>" > /var/www/html/index.html
     ```
5. Create a **new Security Group**:
   - ❌ **DO NOT add HTTP (port 80) from 0.0.0.0/0**
   - ✅ Only add **SSH (port 22)** from your IP to debug if needed.
6. Launch the instance.

---

### 2️⃣ Create Security Group for ALB
1. Go to **EC2 → Security Groups → Create security group**
2. Name: `ALB-SG`
3. Inbound Rules:
   - ✅ Allow HTTP (port 80) from **Anywhere (0.0.0.0/0)**
4. Outbound: leave default (All traffic)

---

### 3️⃣ Modify EC2 Instance Security Group
1. Go to EC2 → Instances → Select your instance → **Networking → Security → Edit inbound rules**
2. Remove HTTP from `0.0.0.0/0` if it exists
3. Add a new rule:
   - Type: HTTP
   - Source: **Custom** → Select the **ALB-SG**
   - This means: Only ALB can talk to EC2 on port 80 ✅

---

### 4️⃣ Create a Target Group
1. Go to **EC2 → Target Groups → Create target group**
2. Choose:
   - **Target Type**: Instances
   - **Protocol**: HTTP
   - **Port**: 80
3. Register your EC2 instance
4. Click **Create target group**

---

### 5️⃣ Create Application Load Balancer
1. Go to **EC2 → Load Balancers → Create Load Balancer → Application Load Balancer**
2. Name: `My-ALB`
3. Scheme: **Internet-facing**
4. Listeners: HTTP → port 80
5. Availability Zones: Choose same AZ as your EC2 instance
6. **Security Group**: Attach the `ALB-SG` you created earlier
7. Target Group:
   - Choose the one you just created
   - Health check path: `/`
8. Click **Create Load Balancer**

---

## ✅ Verify the Setup

### ✅ Access via ALB:
- Visit: `http://<ALB-DNS-name>`
- Should display the default HTML from your EC2 instance.

### ❌ Access EC2 Directly:
- Visit: `http://<EC2-Public-IP>` → Should not work
- Because the EC2 SG only allows traffic from ALB’s SG

---

## 🧪 Bonus - Testing

| Test Type | URL | Expected Outcome |
|-----------|-----|------------------|
| ALB Access | `http://<ALB-DNS>` | ✅ Shows content |
| EC2 Direct Access | `http://<EC2-Public-IP>` | ❌ Blocked |

