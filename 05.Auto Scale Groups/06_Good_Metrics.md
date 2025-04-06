# 📊 Good Metrics to Scale On in AWS Auto Scaling

Auto Scaling can use various metrics from **CloudWatch** to determine when to add or remove EC2 instances. Choosing the right metric is crucial for performance, cost-efficiency, and reliability.

---

## 1️⃣ CPU Utilization

### 🔍 What it is:
- Percentage of CPU capacity in use.
- Commonly used for **compute-bound** workloads.

### ✅ Best For:
- Web servers
- API services
- Background processors

### 🛠️ Typical Setup:
- Scale out if `CPUUtilization > 70%`
- Scale in if `CPUUtilization < 30%`

### ⚠️ Watch Out:
- High CPU might not always mean high traffic (e.g., CPU leaks)
- Use in combination with network/request metrics for balance

---

## 2️⃣ Request Count Per Target (Only with ALB)

### 🔍 What it is:
- Number of requests each target (EC2) in a Target Group is handling.
- Useful for **load-based** scaling.

### ✅ Best For:
- APIs or web apps behind **Application Load Balancer (ALB)**

### 🛠️ Typical Setup:
- Scale out if `RequestCountPerTarget > 1000`
- Scale in if `RequestCountPerTarget < 300`

### ⚠️ Tips:
- Smooths out CPU spikes
- Can be combined with latency metrics for better results

---

## 3️⃣ Average Network In/Out

### 🔍 What it is:
- Measures total inbound/outbound network traffic in bytes.

### ✅ Best For:
- **I/O intensive apps** like media streaming, file transfers, or large data ingestion

### 🛠️ Typical Setup:
- Scale out if `NetworkIn > 10 MB/min` or `NetworkOut > 15 MB/min`

### ⚠️ Things to consider:
- Sudden traffic bursts could cause premature scaling
- Use averages or smoothing to avoid jittery behavior

---

## 4️⃣ Custom Metrics (Advanced)

### 🔍 What it is:
- User-defined metrics pushed to CloudWatch via application code or scripts.
- Offers **full control** over what you want to scale on.

### ✅ Best For:
- Business logic-based scaling, e.g.,
  - Number of active users
  - Queue length (SQS, Redis, Kafka)
  - Memory usage (via custom agent)

### 🛠️ How to Push:
```bash
aws cloudwatch put-metric-data \
  --metric-name ActiveUsers \
  --namespace "CustomApp" \
  --value 125
```