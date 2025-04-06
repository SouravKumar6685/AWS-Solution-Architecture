
# ❤️‍🩹 Health Checks in AWS

## 🔍 What is a Health Check?

A **Health Check** in AWS is a process that automatically monitors the **availability and performance** of your AWS resources (like EC2 instances, targets in Load Balancers, Route53 endpoints, etc.).

Health Checks help AWS services to **detect failure** and take corrective actions like:
- Stopping traffic to unhealthy targets
- Redirecting traffic using DNS failover
- Launching replacement EC2 instances (with Auto Scaling)

---

## 🚦 Where Are Health Checks Used in AWS?

| AWS Service         | Purpose of Health Check                             |
|---------------------|-----------------------------------------------------|
| **Elastic Load Balancer (ELB)** | Routes traffic only to healthy instances       |
| **Auto Scaling Groups**         | Replaces failed/unhealthy EC2 instances        |
| **Route 53 (DNS Failover)**     | Diverts traffic away from unhealthy endpoints |
| **Target Groups (ALB/NLB)**     | Monitors targets like EC2, ECS, Lambda        |

---

## 🧬 How Does a Health Check Work?

1. AWS sends a request (ping, HTTP, HTTPS, or TCP) to a specified **endpoint or port**.
2. It waits for a **response** (with a status code).
3. If it gets **success responses** for a certain number of times → marks **Healthy**
4. If it gets **failures** consecutively → marks **Unhealthy**

---

## ⚙️ Key Parameters in Health Checks

| Parameter              | Description                                                             |
|------------------------|-------------------------------------------------------------------------|
| **Protocol**           | HTTP, HTTPS, TCP, or Ping                                               |
| **Port**               | The port to check (e.g., 80, 443)                                       |
| **Path**               | URL path to check (e.g., `/health`) – for HTTP/HTTPS                    |
| **Interval**           | Time between health check requests (e.g., every 30 seconds)             |
| **Timeout**            | Time AWS waits for a response before marking failed (e.g., 5 seconds)  |
| **Healthy Threshold**  | No. of successful checks before marking the target as healthy           |
| **Unhealthy Threshold**| No. of failed checks before marking the target as unhealthy             |

---

## 📦 Health Check Example (ELB)

```yaml
Protocol: HTTP
Port: 80
Path: /health
Interval: 30 seconds
Timeout: 5 seconds
HealthyThreshold: 3
UnhealthyThreshold: 2
```

This means:
- ELB will check `/health` on port 80 every 30s.
- If 2 checks fail → mark instance unhealthy.
- If 3 consecutive checks succeed → mark it healthy again.

---

## 🧠 Real-World Use Case

Let’s say you have 3 EC2 instances behind an Application Load Balancer.

- You configure health check on `/health`.
- Instance-2 stops responding → ELB stops sending it traffic.
- Auto Scaling Group replaces the instance automatically.

✅ Users experience no downtime  
✅ ELB + ASG + Health Check = **High Availability**

---

## 🛠️ Best Practices

- 💡 Keep health check endpoints lightweight and fast
- ⚠️ Never add DB logic or heavy computation in `/health` endpoint
- 🔄 Combine with **Auto Scaling** for healing unhealthy instances
- 📊 Monitor with **CloudWatch Metrics** for failures and latency

---

## 🧪 Sample Health Endpoint in Express.js (Node.js)

```js
app.get('/health', (req, res) => {
  res.status(200).send('OK');
});
```

---

## 🔁 Summary

- Health Checks ensure your app stays **resilient and available**.
- AWS uses them in ELB, ASG, and Route53 for failover and auto-healing.
- Proper configuration = **zero downtime + happy users 🚀**
