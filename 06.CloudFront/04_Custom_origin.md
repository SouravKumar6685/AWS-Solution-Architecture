
# 🌍 AWS CloudFront – Custom Origin

## 📘 What is a Custom Origin?

In AWS CloudFront, a **Custom Origin** is any **HTTP or HTTPS server** that serves content, other than an S3 bucket in standard mode. CloudFront fetches content from these origins and delivers it through edge locations globally.

---

## 🔧 Types of Custom Origins

CloudFront supports the following as **Custom Origins**:

### 1️⃣ Application Load Balancer (ALB)
- Use ALB domain (e.g., `my-alb-123.us-east-1.elb.amazonaws.com`) as origin.
- Ideal when your application is hosted on multiple EC2 instances behind a load balancer.
- ALB helps with:
  - Distributing load
  - Health checks
  - SSL termination

**Use Case**: Host dynamic websites or APIs on multiple EC2s behind an ALB and use CloudFront for caching + security.

---

### 2️⃣ EC2 Instance (Public IP or DNS)
- Use EC2's **public DNS name** or **Elastic IP** as origin.
- Ensure the EC2 instance is **reachable over HTTP/HTTPS**.
- You can cache API responses, images, or dynamic pages.

**Example Origin Domain**:  
`ec2-11-22-33-44.compute-1.amazonaws.com`

**Security Tip**:  
Use Security Groups and WAF to restrict access to CloudFront only (not directly accessible by users).

---

### 3️⃣ S3 Website Endpoint (Static Website Hosting)
- S3 Website URLs look like:  
  `http://your-bucket.s3-website-us-east-1.amazonaws.com`
- This is used **only when static website hosting** is enabled on the S3 bucket.
- S3 Website endpoints are treated as **HTTP custom origins** (not native S3 origin).

**Use Case**:  
Host a React/Angular app or static site in an S3 bucket with CloudFront on top.

---

### 4️⃣ Any Public HTTP Server (External Origin)
- You can configure any **external website/server** as origin.
- Example:  
  `https://example.com/static/`

CloudFront will pull content from this domain and cache it.

**Use Case**:  
- Use CloudFront as a CDN for third-party content.
- Integrate with legacy systems hosted outside AWS.

---

## ⚙️ Custom Origin Configuration in CloudFront

When adding a Custom Origin:

- 🔹 **Origin Protocol Policy**: HTTP only, HTTPS only, or Match Viewer
- 🔹 **Origin Headers**: Add custom headers (e.g., Auth tokens)
- 🔹 **Caching**: Configure behaviors for query strings, headers, cookies
- 🔹 **Origin Path**: Optional path (e.g., `/api/v1`) to prefix to all requests
- 🔹 **Origin Failover**: Add backup origin in case of failure

---

## ✅ Benefits of Using Custom Origins with CloudFront

| Feature                    | Benefit                                           |
|----------------------------|---------------------------------------------------|
| Global Edge Delivery       | Reduce latency & serve content faster             |
| Origin Offload             | Cache content at edge, reduce origin load         |
| HTTPS Support              | Serve over secure connection                      |
| WAF Integration            | Protect your origin with AWS WAF & Firewall       |
| Custom Cache Control       | Cache dynamic content (APIs, HTML, JSON, etc.)    |
| Real-Time Metrics          | CloudFront + CloudWatch = Full visibility         |

---

## 🔐 Security Tips

- 🔒 Restrict direct access to origin (using Security Groups, OAI/OAC, or WAF)
- ⚠️ Always use HTTPS for secure origin communication
- 📜 Use signed URLs/cookies for protected/private content

---

## 🧪 Use Case Matrix

| Origin Type       | Best For                         | Protocols Supported | Secured By         |
|-------------------|----------------------------------|----------------------|---------------------|
| ALB               | APIs, web apps                   | HTTP, HTTPS          | SGs, WAF, TLS       |
| EC2 Instance      | Standalone web/app servers       | HTTP, HTTPS          | SGs, TLS, IAM       |
| S3 Website        | Static sites (React, HTML, etc.) | HTTP only            | Bucket Policy       |
| External Server   | Legacy or 3rd-party content      | HTTP, HTTPS          | External Firewall   |

---

## 🚀 Summary

> Custom Origins give you flexibility to use CloudFront with **any HTTP/S backend** — whether it's hosted in AWS (ALB, EC2, S3 website) or externally.

CloudFront acts as a **shield and accelerator**, offering:
- Speed 🚀 via caching and edge locations
- Security 🔐 via HTTPS & WAF
- Cost savings 💰 by offloading traffic from origin
