
# 💸 EC2 Spot Instance Requests

## 🧾 What is a Spot Instance?

An **EC2 Spot Instance** is an unused EC2 instance offered at a **discount of up to 90%** compared to On-Demand pricing. It’s best for **fault-tolerant** and **flexible workloads**.

> 🪙 Spot = Cost-effective but can be interrupted when EC2 needs the capacity.

![](https://imgs.search.brave.com/H6gLT_cnVklm2qCQhcRZZP5NtIgGLP8qWjxprVmVHo8/rs:fit:860:0:0:0/g:ce/aHR0cHM6Ly93d3cu/bm9wcy5pby93cC1j/b250ZW50L3VwbG9h/ZHMvMjAyMy8wNi9B/V1MtZGlhZ3JhbS0x/LnBuZw)

---

## 🧠 What is a Spot Instance Request?

A **Spot Instance Request** is how you ask AWS for a spot instance. When capacity is available and your bid matches the price, AWS launches your instance.

Types of spot instance requests:
1. ✅ One-time Spot Request
2. 🔁 Persistent Spot Request

---

## 🔂 What is a **Persistent Spot Instance Request**?

A **Persistent Spot Request** stays active **even if your instance is interrupted** or stopped. AWS will try to **relaunch it automatically** when capacity becomes available again.

### 🟢 Use Case:
- Batch processing
- Fault-tolerant applications
- Long-running background jobs

### ✅ Benefits:
- Automation: Keeps trying until capacity is available
- Good for jobs that **can resume from a checkpoint**

![](https://imgs.search.brave.com/xl8P0mj4hg3TBUGM4NJ2zvqeLBGmcEtZx2V37E44AEM/rs:fit:860:0:0:0/g:ce/aHR0cHM6Ly9qYXll/bmRyYXBhdGlsLmNv/bS93cC1jb250ZW50/L3VwbG9hZHMvMjAy/MS8wOS9FQzJfU3Bv/dF9JbnN0YW5jZV9S/ZXF1ZXN0cy5wbmc)

---

## 🔁 What is a **One-Time Spot Instance Request**?

A **One-Time Spot Request** launches **only once**. If the instance is terminated or interrupted by AWS, it won’t relaunch automatically.

### 🟡 Use Case:
- Testing environments
- CI/CD jobs
- Short-lived jobs where auto-retry isn't necessary

### ⚠️ Limitation:
- No retries = You need to handle interruption manually

---

## ⚙️ How to Choose Between Them?

| Feature                       | Persistent Spot Request | One-Time Spot Request |
|------------------------------|--------------------------|------------------------|
| Retry on Interruption        | ✅ Yes                   | ❌ No                 |
| Best For                     | Long-running workloads   | Short jobs             |
| Needs Manual Intervention    | ❌ No                    | ✅ Yes                |
| Suitable For CI/CD Pipelines | ✅ Yes (with checkpoints) | ✅ Yes (if retryable) |

![](https://imgs.search.brave.com/VzcE3yD7Pzj-h7KItxmBIJGlqL2VEWEkZ1mybLN2L2k/rs:fit:860:0:0:0/g:ce/aHR0cHM6Ly9kb2Nz/LmF3cy5hbWF6b24u/Y29tL2ZyX2ZyL0FX/U0VDMi9sYXRlc3Qv/VXNlckd1aWRlL2lt/YWdlcy9zcG90X3Jl/cXVlc3Rfc3RhdGVz/LnBuZw)

---

## 🛠️ Pro Tips

- 🔁 **Use Persistent** if job can resume from previous state
- 📦 **Use EBS volume** so data persists across instance failures
- ⚠️ Handle **EC2 interruption notices** via CloudWatch Events
- 🧾 Use **Spot Fleet** or **EC2 Auto Scaling Groups** for managing multiple Spot Instances

---

## 🚫 Spot Instance Gotchas

- ⛔ No guarantee your instance won’t be interrupted
- 📉 Prices can fluctuate based on capacity and demand
- 💥 Data stored on instance store is lost if instance is interrupted

---

## ✅ Summary

| Topic                         | Description                                                                 |
|------------------------------|-----------------------------------------------------------------------------|
| Spot Instance                | Spare EC2 capacity at discount, interruptible                               |
| Spot Request                 | How you request a spot instance                                             |
| One-Time Spot Request        | Launches once, no retry if interrupted                                      |
| Persistent Spot Request      | Auto-relaunches until request is cancelled or fulfilled                     |
| Ideal For                    | Cost-saving use cases, stateless or fault-tolerant applications             |

---

> 💡 **Pro Tip**: Use Spot Instances for workers and On-Demand for critical components in hybrid architecture for best savings + reliability.
