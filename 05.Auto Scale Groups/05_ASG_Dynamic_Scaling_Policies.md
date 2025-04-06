# ⚙️ Dynamic Scaling Policies in AWS Auto Scaling

---

## 🧠 What are Dynamic Scaling Policies?

Dynamic Scaling Policies automatically **increase or decrease EC2 instances** in an Auto Scaling Group (ASG) based on **real-time demand** using CloudWatch metrics.

---

## 🚀 Why Use Dynamic Scaling?

| 🔥 Feature | 💬 Description |
|-----------|----------------|
| Auto Reactive | Responds automatically to changes in metrics |
| Cost-Efficient | Adds instances when needed, removes when idle |
| Scalable | Handles workload spikes & dips effectively |
| Flexible | Supports multiple policy types |

---

## ⚖️ Types of Dynamic Scaling Policies

There are **three** main types of dynamic scaling in AWS:

---

### 1️⃣ Target Tracking Scaling Policy

#### 🔹 What is it?
- **Most commonly used**.
- Automatically keeps a specified metric (like CPU usage) at a **target value**.

#### 🔧 Example:
> Maintain average `CPUUtilization` at **60%**

#### ✅ Pros:
- Simple and smart
- No manual thresholds needed
- AWS handles logic internally

---

### 2️⃣ Step Scaling Policy

#### 🔹 What is it?
- You define **specific steps** for how much to scale based on how far the metric goes beyond the threshold.

#### 🔧 Example:
> If CPU > 70% → add 1 instance  
> If CPU > 85% → add 2 instances

#### ⚙️ Components:
- Alarm → Triggered by CloudWatch
- Scaling Adjustment → How many instances to add/remove

#### ✅ Pros:
- More control than Target Tracking

---

### 3️⃣ Simple Scaling Policy (Legacy)

#### 🔹 What is it?
- Basic policy that performs one action based on an alarm.
- Requires **cooldown period**.

#### 🔧 Example:
> If CPU > 75%, add 1 instance (wait 5 mins before another action)

#### 🚫 Not recommended for:
- Modern, production-level apps  
- Complex scaling scenarios

---

## 💡 How Dynamic Scaling Works Internally

1. **CloudWatch** monitors a metric (e.g., CPU usage)
2. A **CloudWatch Alarm** gets triggered
3. The alarm is linked to a **scaling policy**
4. **Auto Scaling Group** adds/removes instances based on the policy

---

## 🛠️ When to Use What?

| Scenario | Recommended Policy |
|----------|--------------------|
| ✅ Simplicity & auto-adjust | Target Tracking |
| ⚙️ Precise control | Step Scaling |
| ⚠️ Legacy or single-action | Simple Scaling (not ideal) |

---

## 📌 Best Practices

- ✅ Always apply **least required scale-in/out adjustments**
- ✅ Define a proper **warm-up time** to prevent flapping
- ✅ Monitor using **CloudWatch Dashboards**
- ✅ Use **multiple alarms** for redundancy
- ⚠️ Don't overlap thresholds between scale-in and scale-out

---

## 🧪 Example: Step Scaling Setup

```json
{
  "Metric": "CPUUtilization",
  "Alarms": [
    {
      "Name": "HighCPU",
      "Threshold": 75,
      "Action": "Add 1 instance"
    },
    {
      "Name": "VeryHighCPU",
      "Threshold": 90,
      "Action": "Add 2 instances"
    },
    {
      "Name": "LowCPU",
      "Threshold": 30,
      "Action": "Remove 1 instance"
    }
  ]
}
```