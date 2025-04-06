
# 📌 Elastic IP in AWS

An **Elastic IP (EIP)** is a **static public IPv4 address** provided by AWS that you can associate with your EC2 instance or network interface.

It allows you to have a **consistent public IP** address even if the underlying instance changes.

---

## 🧠 Why Use Elastic IP?

- To maintain a **static public IP** for your server
- To **mask instance or network failures** by remapping the IP
- Useful when:
  - Hosting a public web server
  - You need to whitelist IPs in third-party services
  - Running bastion hosts or jump boxes

---

## 🏗️ How It Works

- AWS allocates an **Elastic IP** from its public IP pool
- You **associate** the Elastic IP with:
  - An EC2 instance (via primary network interface)
  - A **network interface (ENI)** directly
- You can **reassociate** the EIP with another instance if the original one fails

---

## 🧪 Example Use Case

- EC2 instance `i-12345` is hosting a web app
- You assign an Elastic IP `54.201.100.10`
- If the EC2 crashes, launch a new EC2 and reassign the same EIP
- Users continue accessing the app with the **same IP**

---

## ⚙️ How to Allocate and Associate

### 📍 Steps via AWS Console:

1. Go to **EC2 Dashboard**
2. Click on **Elastic IPs** in the sidebar
3. Click **Allocate Elastic IP address**
4. Choose network or instance scope and allocate
5. Select the IP → Click **Actions > Associate Elastic IP**
6. Choose instance or ENI to associate

---

## 💸 Cost Consideration

| Condition                            | Charge |
|--------------------------------------|--------|
| Associated with a running instance   | ✅ Free |
| Unassociated OR associated with stopped instance | ⚠️ Charged |

> ⚠️ **Avoid idle Elastic IPs** to reduce costs.

---

## 🔄 Reassociation

- You can move an Elastic IP from one instance to another anytime
- No DNS change needed — ideal for high availability

---

## 📊 Differences: Public IP vs Elastic IP

| Feature         | Public IP (Default) | Elastic IP             |
|------------------|---------------------|--------------------------|
| Static           | ❌ No (changes on stop/start) | ✅ Yes |
| Persistence      | ❌ Tied to instance lifecycle | ✅ Independent |
| Cost             | Free                | Charged if unassociated |
| Reusable         | ❌                  | ✅ Can reassign anytime |

---

## 🧠 Tips & Best Practices

- 🎯 Use **Elastic IPs** only when a static IP is really required
- 🔄 **Automate reassociation** in case of instance failure (via Lambda/Scripts)
- 🧹 **Release unused EIPs** to avoid charges
- 🔐 Restrict access using **Security Groups** and **NACLs**

---

## 🔚 Summary

- Elastic IP = Static Public IPv4 from AWS
- Can associate & reassociate anytime
- Used for fault tolerance, static addressing, whitelisting
- Free only when actively used — otherwise, it incurs cost

> 💡 **Pro Tip**: Consider using **Route 53 + DNS Failover** instead of managing multiple EIPs when building for high availability.
