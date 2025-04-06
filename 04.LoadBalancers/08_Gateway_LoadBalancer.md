# 🌀 AWS Gateway Load Balancer (GWLB)

## 📖 What is a Gateway Load Balancer?

A **Gateway Load Balancer (GWLB)** is a type of AWS Load Balancer that makes it easy to deploy, scale, and manage **third-party virtual appliances**, such as:

- Firewalls
- Intrusion Detection and Prevention Systems (IDS/IPS)
- Deep Packet Inspection
- Traffic Inspection Proxies

> ✅ Think of it as a transparent gateway that **forwards traffic** to a fleet of security appliances, and returns the response — without the sender/receiver knowing what’s happening in between.

---

## 🎯 Key Features

- **Combines Load Balancing + Gateway Functionality**
- **Uses GENEVE protocol (port 6081)** for encapsulation
- Supports **third-party appliances** (Palo Alto, Fortinet, etc.)
- High availability & scalability
- Works at **Layer 3 (network layer)**

---

## 🧩 Architecture

```
Inbound/Outbound Traffic
        |
+------------------------+
|  Gateway Load Balancer |
+------------------------+
         |
         v
  GWLB Target Group (appliances)
         |
         v
  Third-party appliances (e.g. firewall)
```

---

## ⚙️ Components

### 1. **Gateway Load Balancer**
- Handles GENEVE encapsulated traffic
- Acts as a single entry/exit point

### 2. **GWLB Target Group**
- Contains virtual appliances as **targets**
- Health checks used to ensure target availability

### 3. **GENEVE Protocol**
- Encapsulation protocol used for sending traffic to appliances
- Port: `6081`

### 4. **VPC Endpoint Service (for GWLB)**
- Used by consumers to **privately access** GWLB
- Supports multi-account / multi-VPC setups

---

## 📦 Use Cases

- Deploying **stateful firewalls** in AWS
- Centralized traffic **inspection/logging**
- Third-party **network security** solutions
- SaaS vendors offering **security as a service**

---

## 🛠️ How It Works (High-Level)

1. Traffic reaches the **Gateway Load Balancer**
2. It encapsulates the packets using **GENEVE**
3. Sends them to one of the **virtual appliances**
4. Appliance processes traffic (e.g. inspection, filtering)
5. Traffic returns back via the same GWLB

---

## 🔧 Hands-On Steps (Console Overview)

### Step 1: Create a GWLB Target Group
- Protocol: `GENEVE`
- Port: `6081`
- Add your **virtual appliance instances**

### Step 2: Create the Gateway Load Balancer
- Choose subnet in each AZ
- Attach the previously created **target group**

### Step 3: Create a VPC Endpoint Service
- Attach the GWLB
- Accept consumers (e.g. other VPCs or accounts)

### Step 4: Consumers Create **GWLB VPC Endpoint**
- These VPCs route traffic via the **endpoint**
- All packets get inspected via appliances

---

## ✅ Benefits

- Transparent: no change in routing or firewall rules
- Scalable: supports auto-scaling of appliance fleet
- Multi-tenant: can serve many VPCs/accounts
- Secure: traffic never leaves AWS network

---

## ⚠️ Tips & Considerations

- 🧠 **GENEVE must be supported** by your appliance
- 🔐 Protect the VPC endpoint with IAM and endpoint policies
- 🔄 Ensure proper routing (route tables pointing to GWLB endpoint)
- 🔍 Monitor health of appliance instances regularly

---

## 🧠 Summary Table

| Feature                     | Description                                     |
|----------------------------|-------------------------------------------------|
| Type                       | Layer 3 (Network Load Balancer variant)         |
| Protocol                   | GENEVE (Port 6081)                              |
| Primary Use                | Load balance & scale third-party appliances     |
| Key Components             | GWLB, Target Group, GENEVE, VPC Endpoint        |
| Common Use Cases           | Firewall, IDS/IPS, Packet Inspection            |

