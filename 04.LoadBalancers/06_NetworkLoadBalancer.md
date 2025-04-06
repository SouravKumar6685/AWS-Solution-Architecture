
# ⚡ AWS Network Load Balancer (NLB)

Network Load Balancer is designed to handle **millions of requests per second** while maintaining **ultra-low latency**. It operates at **Layer 4 (Transport Layer)**, making it ideal for **TCP/UDP-based applications**.

---

## 🧠 What is a Network Load Balancer?

- NLB routes **TCP, TLS, UDP, and TCP_UDP traffic**
- Works at **OSI Layer 4 (Transport Layer)**
- Best for **high-performance, low-latency** workloads
- Supports **static IPs** or **Elastic IPs**
- Can handle **volatile workloads** with millions of requests/sec

---

## 🛠️ Key Features of NLB

| Feature | Description |
|--------|-------------|
| 🔄 Layer 4 Routing | Routes traffic based on IP protocol data (TCP/UDP) |
| 🌍 Static IP Support | You can assign an Elastic IP to the NLB |
| 🔄 Zonal Failover | Routes traffic to healthy targets in different AZs |
| ⚙️ TLS Termination | Offload TLS at the NLB (with listener support) |
| 🚀 Extreme Performance | Handles millions of connections with ultra-low latency |
| 🔁 Connection Multiplexing | Reuses backend connections, reduces load |
| 🩺 Health Checks | Actively monitors registered targets |

---

## 🔁 How NLB Works

1. **Client request** arrives at the **NLB listener** (on TCP port)
2. NLB **chooses a healthy target** (based on health checks)
3. NLB **forwards traffic** to the selected target in the target group

---

## ⚙️ Listener Protocols Supported

- `TCP`
- `UDP`
- `TLS`
- `TCP_UDP`

---

## 🎯 Use Cases for NLB

- IoT applications requiring **UDP**
- **Gaming servers** or **VoIP** apps
- Real-time **financial transaction systems**
- High-performance **microservices** using TCP
- Apps needing **static IP** for whitelisting

---

## 🧪 Health Checks (at Target Group Level)

| Field | Description |
|-------|-------------|
| Protocol | TCP/HTTP/HTTPS |
| Port | Target port or fixed port |
| Interval | How often to check |
| Threshold | When to consider healthy/unhealthy |

---

## 🛡️ Security with NLB

- Attach **Security Groups to targets** (NLB itself doesn't use SGs)
- Ensure **NACLs** and **firewall rules** allow NLB traffic
- Use **TLS listener** for encrypted traffic

---

## 🧰 Pros & Cons

### ✅ Pros
- Super low latency, even under high traffic
- Static IP support
- Supports non-HTTP workloads (e.g., TCP/UDP)
- Scales automatically

### ⚠️ Cons
- No application-layer features (e.g., path-based routing, cookies)
- No built-in WebSocket support
- Does not natively support Web ACLs (WAF only supports ALB)

---

## 🖥️ Hands-On Example (Console)

1. Go to **EC2 → Load Balancers → Create Load Balancer**
2. Choose **Network Load Balancer**
3. Set:
   - Scheme: Internet-facing or Internal
   - IP Type: IPv4 or Dualstack
   - Listener Protocol: TCP/UDP/TLS
4. Assign **Elastic IPs** if needed
5. Create or select **Target Group**
6. Register EC2 instances
7. Configure **Health Checks**
8. Review and **Create**

---

## 📌 Real-World Tip

> Use NLB when you need **super low latency** and you're working with **non-HTTP protocols**. Combine with **Auto Scaling** for resilient systems.

---

## 🔄 NLB vs ALB vs CLB

| Feature | NLB | ALB | CLB |
|--------|-----|-----|-----|
| Layer | 4 | 7 | 4 & 7 |
| Protocols | TCP, UDP, TLS | HTTP, HTTPS | HTTP, HTTPS, TCP |
| Best For | High throughput | Web apps | Legacy apps |
| WAF Support | ❌ | ✅ | ❌ |

---
