# 🧪 Hands-On: Enabling Sticky Sessions in AWS ALB (Session Affinity)

---

## 🛠️ Scenario

We want to ensure that a client’s session always connects to the **same target (EC2 instance)** behind an **Application Load Balancer (ALB)**. This is useful for apps like:
- Shopping carts 🛒
- Chat applications 💬
- Games 🎮

---

## 🧾 Pre-requisites

✅ An **Application Load Balancer** already set up  
✅ A **Target Group** with at least **2 EC2 instances**  
✅ EC2 instances have a **web app** running (like Apache/Nginx or Node.js)  

---

## 🔧 Steps to Enable Sticky Sessions (Console)

### 🌀 Step 1: Go to EC2 Target Group

1. Open the AWS Console
2. Navigate to **EC2 → Load Balancing → Target Groups**
3. Select your **Target Group** (used by ALB)

---

### 🧩 Step 2: Modify Stickiness Settings

1. In the selected Target Group, go to the **"Attributes"** tab  
2. Click **Edit**

---

### ⚙️ Step 3: Enable Stickiness

1. Toggle **Stickiness → ON**
2. Select:
   - **"Load balancer generated cookie"** (Default AWS cookie `AWSALB`)  
     OR  
   - **"Application-based cookie"** (You must configure this in your app)

---

### ⏱️ Step 4: Set Cookie Duration

- Enter duration (in seconds) for how long stickiness should persist (e.g., 300s = 5 mins)

---

### 💾 Step 5: Save Settings

- Click **"Save changes"**
- Your Target Group now supports **sticky sessions!**

---

## ✅ How to Test

1. Hit your ALB DNS in browser multiple times (e.g., `http://your-alb-dns`)
2. Make sure it **routes you to the same EC2 instance**
   - You can confirm this if each instance shows a unique hostname or ID on the page

---

## 🧪 Optional Testing Code (EC2 Instance)

On each instance, run a simple HTTP server to identify it:

```bash
sudo yum install -y httpd
sudo echo "Hello from Instance 1" > /var/www/html/index.html
sudo systemctl start httpd
sudo systemctl enable httpd
```

Change the message for other EC2s.

---

## 🧠 Bonus Tips

- 💡 Combine Sticky Sessions with **Health Checks** to prevent downtime
- 🔐 Use **HTTPS** to protect session cookies
- 🔁 For scalable apps, store sessions in **Redis or RDS**, not in-memory

---
