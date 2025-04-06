# 🎯 Target Tracking Scaling Policy - AWS Auto Scaling

Target Tracking is like a **thermostat** – it automatically adjusts the number of EC2 instances in your Auto Scaling Group (ASG) to maintain a **target value** for a specified metric (like CPU Utilization).

---

## 📌 What is a Target Tracking Scaling Policy?

A **Target Tracking Policy** automatically **scales in or out** your EC2 instances to maintain a specific **target metric value** (e.g., 60% CPU usage).

> 🧠 You set the "target" and AWS does the rest.

---

## 🛠 How It Works

- You select a metric (e.g., `Average CPU Utilization`)
- Set a **target value** (e.g., 50%)
- AWS Auto Scaling monitors the group and:
  - **Scales out** if metric > target
  - **Scales in** if metric < target

AWS uses **CloudWatch alarms** under the hood but manages them for you.

---

## 🔧 Example Use Case

```bash
Target: 50% CPU Utilization

If current avg CPU > 50%:
  ➜ Launch more EC2 instances

If current avg CPU < 50%:
  ➜ Terminate some EC2 instances
```

---

## 🧱 Metrics You Can Use

| Metric                       | Description                                |
|-----------------------------|--------------------------------------------|
| CPUUtilization              | Average CPU across instances               |
| ALBRequestCountPerTarget    | Avg requests per target from ALB           |
| NetworkIn/Out               | Average inbound/outbound traffic (bytes)   |
| Custom Metric (via CloudWatch) | You can use your own application metric |

---

## 🧪 Example: Create via AWS Console

1. Go to **Auto Scaling Groups**
2. Select your ASG
3. Go to **Automatic Scaling** > Add Policy
4. Choose **Target Tracking**
5. Select metric (e.g., CPU Utilization)
6. Set **Target value** (e.g., 60%)
7. Set **Estimated Instance Warm-up Time** (e.g., 300 seconds)
8. Save

---

## 💡 Benefits

- ✅ **No manual CloudWatch alarms**
- ✅ **Simple to configure**
- ✅ Ideal for applications with **steady workloads**
- ✅ AWS handles scaling math

---

## ⚠️ Tips & Best Practices

| Tip | Description |
|-----|-------------|
| 🎯 Choose realistic target | 100% CPU isn't sustainable. 50–70% is common. |
| 🧊 Set warm-up time correctly | Prevents premature scaling actions |
| ⚖️ Combine with Step or Scheduled Policies | For complete control |
| 🔐 Use IAM roles | Ensure the ASG has the right permissions |

---

## 🧰 Example JSON (AWS CLI/CloudFormation)

```json
{
  "TargetTrackingConfiguration": {
    "PredefinedMetricSpecification": {
      "PredefinedMetricType": "ASGAverageCPUUtilization"
    },
    "TargetValue": 50.0,
    "DisableScaleIn": false
  },
  "EstimatedInstanceWarmup": 300
}
```

---

## 🧠 Good To Know

- AWS **doesn’t scale in** below the **minimum capacity**
- It **respects cooldowns** and **warm-up periods**
- Works best for **applications with consistent demand**

