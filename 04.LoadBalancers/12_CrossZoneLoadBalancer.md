
# 🌐 Cross-Zone Load Balancing in AWS

---

## 🧠 What is Cross-Zone Load Balancing?

**Cross-Zone Load Balancing** allows a Load Balancer to distribute **incoming traffic evenly** across all registered targets in **all Availability Zones (AZs)**.

Without it, traffic is distributed **only within the AZs** where the Load Balancer has nodes.

---

## 🎯 Why is it Useful?

✅ Ensures **better resource utilization**  
✅ Prevents **uneven load** on certain zones  
✅ Helps when **targets are not equally distributed** across AZs  
✅ Cost-effective and **improves high availability**

---

## 📊 Scenario Without Cross-Zone Load Balancing

Let's say:
- You have 3 targets:
  - 2 in AZ-1
  - 1 in AZ-2

If a request lands in AZ-2 and cross-zone is **disabled**, it only goes to the **1 target** in AZ-2, overloading it.

---

## 📊 Scenario With Cross-Zone Load Balancing

- All targets across **all AZs** are used
- Load is **evenly spread**
- Prevents **resource exhaustion**

---

## ⚙️ How to Enable Cross-Zone Load Balancing (Console)

### 🌀 For Application Load Balancer (ALB)

1. Go to **EC2 Console → Load Balancers**
2. Select your **ALB**
3. Choose the **"Description" tab**
4. Click **Edit attributes**
5. Toggle **"Cross-zone load balancing" → Enabled**
6. Save the configuration

---

### 🌀 For Network Load Balancer (NLB)

Same steps as ALB, but also note:

📌 Cross-zone load balancing in NLB **incurs charges**, while in ALB it’s **free**.

---

## 💰 Pricing Notes

| Load Balancer Type | Cross-Zone Charges | Default State   |
|--------------------|--------------------|-----------------|
| ALB                | Free               | Enabled         |
| CLB                | Free               | Optional        |
| NLB                | Charged per GB     | Disabled by default |

---

## ⚠️ Limitations / Considerations

- Enabling it **adds slight latency** in highly distributed architectures
- May require careful **cost analysis in NLB**
- If disabled and AZs are **unevenly provisioned**, traffic may become skewed

---

## 💡 Best Practices

- Enable cross-zone when your **target count is imbalanced across AZs**
- Combine it with **Auto Scaling Groups** for fault tolerance
- Monitor with **CloudWatch metrics** for load distribution insight

---

## 🧪 Real World Example

In an ALB with:
- 2 EC2 instances in us-east-1a
- 1 EC2 instance in us-east-1b

With **Cross-Zone Enabled**:  
→ Load is split 33%-33%-33% across all 3.

With **Cross-Zone Disabled**:  
→ 50% of traffic goes to 1b instance only!

---

## 📌 Summary

| Feature                       | With Cross-Zone | Without Cross-Zone |
|------------------------------|------------------|---------------------|
| Load Distribution            | Balanced          | AZ-dependent        |
| Fault Tolerance              | Higher             | Lower               |
| Cost (NLB only)              | Might increase     | Lower               |
| Recommended for ALB         | ✅ Yes             | ❌ Not preferred    |
