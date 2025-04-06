# 🛠️ Hands-On: Routing Requests via Network Load Balancer (NLB)

## 🎯 Scenario

Two types of requests:
- **User-related queries** → go to **Users Target Group**
- **Search-related queries** → go to **Search Target Group**

We will configure an **NLB (Layer 4)** to handle this setup. Since NLB doesn't support URL/path-based routing (unlike ALB), we must use **different ports or protocols** to distinguish the traffic.

---

## 🧩 Architecture Overview

```
Client (Request)
     |
     v
+---------------------+
|  Network Load Balancer |
|    TCP Listener(s)     |
+---------------------+
    |               |
    v               v
[Users TG]       [Search TG]
  |                   |
[EC2s: User]     [EC2s: Search]
```

---

## 🔧 Prerequisites

✅ 2 EC2 instances:
- One running a **User service** (e.g., port 8080)
- One running a **Search service** (e.g., port 9090)

✅ Security Group allows inbound TCP on ports 8080 and 9090  
✅ NLB should be **Internet-facing**

---

## 🛠️ Step-by-Step: Setup via AWS Console

### 1. 🚀 Create Target Groups

- Go to **EC2 → Target Groups → Create target group**
- Select type: `Instances`
- Protocol: `TCP`
- Ports:
  - For Users: `8080`
  - For Search: `9090`
- Health check protocol: TCP
- Name:
  - `tg-users`
  - `tg-search`
- Register targets (EC2s running the respective services)

---

### 2. ⚙️ Create the Network Load Balancer

- Go to **EC2 → Load Balancers → Create Load Balancer**
- Choose **Network Load Balancer**
- Name it: `nlb-user-search`
- Scheme: Internet-facing
- IP Type: IPv4
- Listeners:
  - Listener 1: TCP 80 → forward to `tg-users`
  - Listener 2: TCP 81 → forward to `tg-search`
- Assign Elastic IPs if needed
- Choose or create a security group that allows inbound TCP on 80 & 81

---

### 3. 🔁 Attach Listeners

> NLB doesn’t allow URL-based routing, so you use **port-based routing**.

- Listener 1: Port `80` (→ `tg-users`)
- Listener 2: Port `81` (→ `tg-search`)

---

### 4. 🔍 Test the Setup

From a browser or `curl`, access the services:

```bash
# For User requests (port 80)
curl http://<NLB-DNS>:80

# For Search requests (port 81)
curl http://<NLB-DNS>:81
```

✅ Each request should route to the correct backend instance.

---

## 💡 Benefits of This Setup

- You can isolate services behind the same NLB
- Flexibility for microservices using **different ports**
- Better **traffic management** using listener rules
- Secure: EC2s can be placed in private subnets and exposed only via NLB

---

## ⚠️ Pro Tips

- **Don't use NLB if you need path-based routing** — switch to **ALB**.
- Use **Elastic IPs** for static access
- Health check on correct ports (`8080` and `9090`) is critical
- Enable **Access Logs** for monitoring requests

---

## 🧠 Summary

| Request Type | Port | Target Group  | Service     |
|--------------|------|---------------|-------------|
| User Query   | 80   | tg-users      | EC2: Users  |
| Search Query | 81   | tg-search     | EC2: Search |

