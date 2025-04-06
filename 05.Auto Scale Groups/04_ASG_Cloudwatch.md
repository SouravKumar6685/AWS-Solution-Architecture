# 📈 Auto Scaling with CloudWatch Alarms & Scaling Policies

---

## 🚀 What is Auto Scaling with CloudWatch?

AWS **Auto Scaling + CloudWatch Alarms** let you:
- **Automatically scale** EC2 instances up/down
- Based on metrics like **CPU utilization**, **network I/O**, **custom app metrics**
- Helps maintain app performance while optimizing costs

---

## 🧠 Why Use CloudWatch Alarms for Scaling?

| Benefit | Description |
|--------|-------------|
| ⚙️ Auto Scaling | No manual intervention to scale EC2 |
| 💵 Cost Efficiency | Scale only when needed |
| 📊 Real-time Monitoring | Metrics monitored every minute |
| 🔄 Dynamic | Reacts to both increase & decrease in traffic |

---

## 🔔 What is a CloudWatch Alarm?

- A **trigger** based on a **CloudWatch metric**
- Can notify, take action, or **initiate a scaling policy**

> 🔧 Example:  
> If average CPU utilization > 70% for 5 minutes → Scale out (add instance)

---

## 🔍 Key Metrics Used for Alarms

- `CPUUtilization`
- `NetworkIn` / `NetworkOut`
- `DiskReadOps`, `DiskWriteOps`
- Custom app metrics (via CloudWatch Agent or Embedded Metrics)

---

## ⚙️ Scaling Policies in Auto Scaling Group (ASG)

There are **3 types** of Scaling Policies:

### 1️⃣ Target Tracking Scaling
- Easiest & most recommended
- You define a target (e.g., 60% CPU)
- AWS handles everything automatically

🧪 *Example:*  
Maintain average CPU at 60% → add/remove instances accordingly

---

### 2️⃣ Step Scaling
- More **granular** and **threshold-based**
- You define steps to scale based on how much metric deviates

🧪 *Example:*  
- If CPU > 70% → add 1 instance  
- If CPU > 85% → add 2 instances

---

### 3️⃣ Simple Scaling (Legacy)
- Basic version with cooldowns
- One alarm triggers one scaling action

🛑 *Not recommended for complex or modern apps*

---

## 🛠️ Hands-On Setup via Console

### Step 1: Create Auto Scaling Group (ASG)
1. Go to **EC2 → Auto Scaling Groups**
2. Click **Create Auto Scaling Group**
3. Choose **Launch Template**
4. Select subnets (multi-AZ recommended)
5. Define **min/max/desired capacity**
6. Attach **ALB or NLB** if needed

---

### Step 2: Add Scaling Policies
#### ➕ Target Tracking Policy
1. Go to ASG → **Automatic Scaling**
2. Click **Add policy**
3. Select **Target tracking**
4. Choose metric: `CPUUtilization`
5. Set target value: e.g., `60%`
6. Set instance warm-up time: e.g., `300 sec`

#### ➕ Step Scaling Policy
1. Click **Add policy**
2. Select **Step scaling**
3. Define Alarm (or create new)
4. Set scaling adjustments for threshold ranges

---

### Step 3: Create CloudWatch Alarms (Optional Advanced)
1. Go to **CloudWatch → Alarms**
2. Click **Create Alarm**
3. Choose metric (e.g., `EC2 > CPUUtilization`)
4. Set threshold (e.g., > 75%)
5. Choose action: **Trigger ASG policy**

---

## ✅ Best Practices

| ✅ Do This | ❌ Avoid This |
|-----------|--------------|
| Use **Target Tracking** for simplicity | Using only Simple Scaling |
| Always define a **cooldown period** | Setting conflicting alarms |
| Monitor using **CloudWatch Dashboards** | Ignoring instance warm-up time |
| Use **custom metrics** for app-specific scaling | Blindly scaling on CPU alone |

---

## 🧠 Bonus Tip: Instance Warm-Up Time

- Ensures ASG **waits** before considering new instance ready
- Prevents premature scale-in
- Typical values: **300 seconds (5 mins)**

---

## 📊 Example Target Tracking Setup

| Metric          | CPUUtilization |
|------------------|----------------|
| Target           | 60%           |
| Min Instances    | 2             |
| Max Instances    | 5             |
| Instance Warm-up | 300 seconds   |

---

## 🔗 Integration with Other Services

- 🔄 Works with **Application Load Balancer**
- 📬 Can trigger **SNS Notifications**
- 🎯 Supports **Lifecycle Hooks** for custom scripts

---

