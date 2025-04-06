
# ⚖️ AWS Load Balancer Notes

## 🔍 What is a Load Balancer?

A **Load Balancer** automatically distributes incoming application traffic across multiple targets (like EC2 instances, containers, or IPs) to ensure:

![](https://imgs.search.brave.com/Rl5BbRW5duQVkQhM3CDgPZ3ZDNyUa2RmCnZYBQi4luY/rs:fit:860:0:0:0/g:ce/aHR0cHM6Ly91bWJy/ZWxsYWNvc3QuY29t/L3dwLWNvbnRlbnQv/dXBsb2Fkcy9sb2Fk/X2JhbGFuY2VyLnBu/Zw)

- ✅ High Availability
- ⚡ Improved Performance
- 🛡️ Fault Tolerance
- 🔁 Seamless Scaling

---

## 🚀 Elastic Load Balancer (ELB)

**Elastic Load Balancing (ELB)** is a fully managed service provided by AWS to handle load distribution for applications.

### 🧱 Types of Load Balancers in AWS:

| Load Balancer Type          | Use Case                                   |
|-----------------------------|--------------------------------------------|
| **Application (ALB)**       | HTTP/HTTPS based web applications          |
| **Network (NLB)**           | TCP/UDP traffic at ultra-low latency       |
| **Gateway Load Balancer**   | 3rd party virtual appliances (firewalls)   |
| **Classic Load Balancer**   | Legacy apps (Layer 4 + Layer 7 support)    |

---

## 🧠 Why Use ELB?

- Automatically scales to handle variable traffic
- Can span across **multiple Availability Zones**
- Monitors instance health and routes traffic only to healthy instances
- Integrated with **Auto Scaling**, **CloudWatch**, and **WAF**
- Supports **SSL termination**, **sticky sessions**, **path-based routing**, and more

---

## ❤️‍🩹 Health Checks in ELB

### 🧾 What is a Health Check?

A **Health Check** is a mechanism to **monitor the health of registered targets (EC2, Lambda, etc.)**. ELB periodically sends requests (pings, HTTP/HTTPS requests) to verify whether the instance is healthy.

### 🧬 How It Works:
1. ELB sends periodic health check requests to each registered instance.
2. If an instance **fails** the health check (consecutively), it is marked **unhealthy**.
3. ELB **stops sending traffic** to unhealthy targets.
4. Once the instance passes health checks again, it becomes healthy and is added back to the pool.

---

## ⚙️ Health Check Parameters

| Parameter           | Description                                         |
|---------------------|-----------------------------------------------------|
| **Protocol**        | HTTP, HTTPS, TCP                                    |
| **Port**            | Target port for health check                        |
| **Ping Path**       | For HTTP/HTTPS – the URL to ping (e.g. `/health`)   |
| **Interval**        | Time (seconds) between each health check            |
| **Timeout**         | Time ELB waits for a response before failing        |
| **Healthy Threshold** | Number of successes before marking healthy         |
| **Unhealthy Threshold** | Number of failures before marking unhealthy     |

---

## 🧪 Example Health Check Setup

```yaml
Protocol: HTTP
Port: 80
Ping Path: /health
Interval: 30 seconds
Timeout: 5 seconds
Healthy threshold: 3
Unhealthy threshold: 2
```

> 📌 This means ELB will send a request every 30s. If it gets 2 failed responses in a row, the instance is marked unhealthy. Once it gets 3 successful responses in a row, it's marked healthy again.

---

## 🔐 Example: Health Check Endpoint in Node.js

```js
// ExpressJS server health route
app.get('/health', (req, res) => {
  res.status(200).send('OK');
});
```

---

## 🎯 Real-World Example Scenario

Imagine you have:
- 3 EC2 instances behind an Application Load Balancer
- A `/health` endpoint implemented on all instances
- Health checks configured with `/health` as the Ping Path

If **EC2-2** crashes, ELB stops routing traffic to it, while **EC2-1 and EC2-3** continue to serve users without disruption.

---

## 🛠️ Best Practices

- ✅ Always implement a **dedicated health check endpoint** (`/health`)
- ⚠️ Avoid heavy logic in health check routes — keep it lightweight and fast
- 🔁 Use **Auto Scaling Group** with ELB for seamless failover and recovery
- 🔍 Monitor instance health using **CloudWatch metrics**

---

## 🧠 Summary

- ELB improves availability and performance by routing traffic intelligently.
- Health checks protect the app by routing traffic **only to healthy instances**.
- Configuring **fine-tuned health checks** ensures faster recovery and better user experience.

> 💡 Always combine **ELB + Auto Scaling + CloudWatch** for production-grade architecture.
