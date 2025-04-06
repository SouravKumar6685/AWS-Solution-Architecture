
# 🚀 EC2 Spot Fleet

## 🧾 What is a Spot Fleet?

A **Spot Fleet** is a collection of Spot Instances (and optionally On-Demand Instances) that are launched together to meet a desired capacity and price.

> 🔁 It automatically finds and provisions the most cost-effective instances from different instance types and Availability Zones (AZs).

![](https://imgs.search.brave.com/Du9GTXx3bwY5maM54JKQBRCWTbgHKryTylXzhQhAmxw/rs:fit:860:0:0:0/g:ce/aHR0cHM6Ly9ibG9n/LmJvbHRvcHMuY29t/L2ltZy9wb3N0cy8y/MDE4LzA3L3Nwb3Qt/ZmxlZXQtcmVxdWVz/dC1leGFtcGxlLnBu/Zw)

---

## 🧠 Why Use Spot Fleet?

- 🔄 Automatically manages capacity across **multiple instance types & AZs**
- 💰 Maximizes savings while meeting workload needs
- ⚖️ Balances cost, capacity, and performance using strategies like **lowest price**, **diversified**, or **capacity optimized**

---

## ⚙️ Key Components of a Spot Fleet

| Component              | Description                                                                 |
|------------------------|-----------------------------------------------------------------------------|
| 📦 Launch Specifications | Defines instance types, AMI, subnet, etc.                                  |
| 🎯 Target Capacity       | Number of instances or vCPUs desired                                       |
| 🧠 Allocation Strategy   | How to select instances (e.g., lowest-price, diversified)                  |
| 💡 Instance Types        | Can include multiple types for flexibility                                 |
| 🌍 Availability Zones    | Supports multiple AZs for high availability                                |
| 🛠️ Mixed Instances       | Mix Spot + On-Demand (great for fallback)                                  |

---

## 🎯 Allocation Strategies

1. **Lowest Price**  
   - Launch instances from pools with the lowest current Spot price  
   - 🪙 Maximum cost savings  
   - ⚠️ Can lead to interruptions

2. **Diversified**  
   - Distributes across multiple Spot pools  
   - 🔁 Higher availability and fault tolerance  
   - 📉 May cost slightly more

3. **Capacity Optimized**  
   - Chooses pools with the most available Spot capacity  
   - 🚫 Reduces interruptions  
   - 💰 Balanced cost & stability

---

## ✅ Best Use Cases

- 🧪 Big Data & analytics workloads  
- 🎬 Video rendering pipelines  
- 🚀 High-performance computing (HPC)  
- 🧠 Machine learning model training  
- 🔄 Stateless, scalable applications

---

## 🛠️ Sample JSON – Spot Fleet Request (Simplified)

```json
{
  "SpotFleetRequestConfig": {
    "TargetCapacity": 4,
    "IamFleetRole": "arn:aws:iam::123456789012:role/aws-ec2-spot-fleet-role",
    "AllocationStrategy": "lowestPrice",
    "LaunchSpecifications": [
      {
        "InstanceType": "t3.large",
        "SubnetId": "subnet-abc12345",
        "ImageId": "ami-0abcdef1234567890"
      },
      {
        "InstanceType": "m5.large",
        "SubnetId": "subnet-def67890",
        "ImageId": "ami-0abcdef1234567890"
      }
    ]
  }
}
```

---

## ⚠️ Tips & Warnings

- ⚖️ Use **diversified or capacity-optimized** strategy to reduce chance of interruptions
- 🧹 Clean up Spot Fleets to avoid unnecessary charges
- ☁️ Combine Spot Fleets with **Auto Scaling Groups** for advanced control
- 🔐 Attach proper IAM roles (like `aws-ec2-spot-fleet-tagging-role`) for managing tags and permissions
- 💾 Use **Elastic Block Store (EBS)** to persist data across interruptions

---

## 📝 Summary

| Feature                | Description                                          |
|------------------------|------------------------------------------------------|
| What is it?           | Auto-managed fleet of Spot (and optionally On-Demand) instances |
| Cost Benefit          | High (up to 90% savings over On-Demand)              |
| Allocation Strategies | lowestPrice, diversified, capacityOptimized          |
| Scaling               | Yes, auto-scales to meet capacity                    |
| Use Cases             | Batch jobs, ML, rendering, fault-tolerant apps       |

---

> 💡 **Pro Tip**: Use **AWS Auto Scaling Group with Mixed Instances** if you want even finer control + automated scaling with Spot + On-Demand mix.
