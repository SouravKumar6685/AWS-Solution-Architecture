
# 🌍 AWS Global Accelerator – Notes

## 🚀 What is AWS Global Accelerator?

AWS Global Accelerator is a **networking service** that improves the **availability** and **performance** of your applications with **global users**.

It provides **static IP addresses** that act as a **single entry point** to your application, then routes traffic through the **AWS global network** to optimal endpoints.

---

## ✅ Key Benefits

- 🌐 **Global Traffic Management**
- ⚡ **Improved performance** using the AWS backbone
- 🛑 **DDoS protection** via AWS Shield
- 🔁 **Automatic failover** between healthy endpoints
- 📌 **Static IPs** (no need for DNS changes)
- 🌍 **Low latency** routing to the nearest region

---

## 🔧 How It Works

1. Users connect using a **static IP** provided by Global Accelerator.
2. Traffic enters the **AWS global network** at the nearest AWS edge location.
3. It is routed to the **best-performing** AWS endpoint (e.g., EC2, ALB, NLB) in a **region**.
4. Uses **Anycast routing** to direct user traffic to the **nearest healthy endpoint**.

---

## 🧱 Components of Global Accelerator

| Component         | Description |
|------------------|-------------|
| **Accelerator**   | Main resource. Provides two static Anycast IPs |
| **Listener**      | Monitors incoming traffic on specific ports |
| **Endpoint Group**| Group of endpoints in a single AWS Region |
| **Endpoint**      | ALB, NLB, EC2 IP, or Elastic IP in that region |

---

## 📌 Static IPs

- You get **two static IPs** per accelerator.
- You can **bring your own IPs** (BYOIP).
- These IPs stay fixed across deployments.

---

## 🧠 Use Cases

- 🌍 Global web applications
- 📉 Low-latency gaming or video streaming
- 💼 Multi-region API gateways
- 🔁 Disaster recovery (failover to other regions)
- 🌐 Hybrid workloads (on-prem + AWS)

---

## 🛡️ Health Checks & Failover

- Built-in **health checks** at the endpoint level
- If an endpoint or region becomes unhealthy, traffic is **rerouted** automatically
- Ensures **high availability and resilience**

---

## 💡 Global Accelerator vs CloudFront

| Feature              | Global Accelerator           | CloudFront                         |
|----------------------|------------------------------|-------------------------------------|
| Content Type         | Dynamic + Static             | Primarily Static (CDN)              |
| IP Address           | Static IPs                   | Domain name (CloudFront URL)        |
| Routing Optimization | ✅ Yes                        | ✅ Yes                              |
| Use Case             | APIs, gaming, low-latency    | Websites, videos, static content    |
| Protocols Supported  | TCP & UDP                    | HTTP & HTTPS                        |

---

## 📘 Best Practices

- 🧭 Use **multiple regions** to fully leverage performance gains
- 🔁 Pair with **auto scaling + health checks**
- 🛡️ Combine with **WAF, Shield, and IAM policies** for security
- 💼 Use for **production workloads** where performance & availability matter

---

## 🧪 Bonus: Hands-on Ideas

- Create a Global Accelerator with two ALBs in **us-east-1** and **ap-south-1**
- Simulate region failover using **EC2 health checks**
- Compare performance with and without Global Accelerator using `traceroute`

