# ☁️ AWS CloudFront Origins

In CloudFront, the **origin** is the source where your actual content lives. CloudFront pulls (or pushes) content from there to serve it through its global **edge locations**.

You can have **multiple origins** and use different cache behaviors to serve them differently.

---

## 🎯 Types of Origins

### 🪣 1. Amazon S3 Bucket (Origin Type: AWS S3)

**Use case:** Static website hosting, image files, JS/CSS, software downloads, etc.

#### 🔧 Setup
- Create an S3 bucket and upload your content.
- You can choose to:
  - ❌ Make it **public** (not recommended)
  - ✅ Use **Origin Access Control (OAC)** (recommended)
- Enable **static website hosting** (optional, if you're doing site hosting)

#### ✅ Benefits
- Simple + scalable
- Integrates natively with CloudFront
- Secure with OAC (only CloudFront can access bucket)

#### ⚠️ Gotchas
- Don't use public access unless you enjoy the sound of **security compliance alarms**
- If you forget to set a default root object (like `index.html`), prepare for a world of 403s

---

### 🌐 2. Custom Origin (Origin Type: HTTP/S)

**Use case:** EC2 instances, Load Balancers, On-prem servers, 3rd-party web servers.

#### 🔧 Setup
- You can point CloudFront to any **publicly accessible HTTP(S)** endpoint:
  - EC2 with web server
  - ALB/NLB
  - API Gateway
  - Non-AWS server (why though?)

#### ✅ Benefits
- Flexibility to use anything with HTTP
- Dynamic content delivery with caching
- Supports custom headers for routing/versioning

#### ⚠️ Warnings
- You are now responsible for **securing** your backend
- No native OAC—so make sure it's not exposed to the world without CloudFront in front
- You need to manage **health checks**, **TLS certs**, and **domain names** yourself

---

## 🧠 Quick Comparison

| Feature                  | S3 Bucket                          | Custom Origin                       |
|--------------------------|------------------------------------|--------------------------------------|
| Native with AWS         | ✅ Yes                             | 🚫 No                                 |
| SSL Support             | ✅ Easy via CloudFront             | ⚠️ Must configure HTTPS backend       |
| Caching Support         | ✅ Yes                             | ✅ Yes                                |
| Good For                | Static websites, files             | APIs, dynamic sites, external apps   |
| Security (with OAC)     | ✅ Built-in                        | ❌ You have to implement manually     |

---

## 🔐 Security Notes

- **Always use HTTPS** for custom origins unless you like living dangerously
- For S3: **Enable OAC** and block all public access
- Use **WAF** to protect your origin from nasty bots and DDoS trolls

---

## 🛠️ Example Architecture

**S3 Static Website + API Gateway for Backend**

