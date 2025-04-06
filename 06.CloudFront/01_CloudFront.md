
# 🌍 AWS CloudFront - Content Delivery Network (CDN)

**Amazon CloudFront** is a **fast, secure, and programmable CDN** (Content Delivery Network) offered by AWS.

It securely delivers content (web pages, videos, APIs, etc.) with **low latency** and **high transfer speed** using a global network of **edge locations**.

---

## 🚀 Why Use CloudFront?

| Feature                     | Benefit                                                                 |
|----------------------------|-------------------------------------------------------------------------|
| ⚡ Low Latency              | Delivers content from the nearest **edge location** to users             |
| 🔒 Secure Content Delivery  | Supports **HTTPS**, **Origin Access Control (OAC)**, **Signed URLs**     |
| 🌍 Global Reach             | 400+ edge locations globally for faster access                           |
| 🧠 Intelligent Routing      | Automatically routes requests to the best edge location                  |
| 💸 Cost Efficient           | Reduces load on origin, caching content closer to users                  |
| 🛡️ Integrated with AWS WAF | Protects against DDoS, SQL Injection, XSS attacks                        |

---

## 🏗️ Key Components

### 📦 Origin
- The source of the content
- Can be:
  - **S3 bucket**
  - **EC2 instance**
  - **ALB / NLB**
  - **Custom origin (external web server)**

### 📍 Edge Location
- CDN data centers located around the world
- Cache content to reduce latency

### 🔄 Distribution
- A configuration that tells CloudFront where to get your content and how to deliver it
  - **Web Distribution** (for websites, APIs)
  - **RTMP Distribution** (deprecated - used for media streaming)

### 📁 Cache Behavior
- Rules that define how CloudFront handles requests (e.g., cache TTL, redirects)

---

## 🔐 Security Features

- ✅ **HTTPS Support**
- 🔐 **Signed URLs and Cookies** (for access control)
- 🛡️ **Origin Access Control (OAC)** to restrict direct S3 access
- 🔗 **Custom SSL Certificates** via ACM
- 🧱 **AWS WAF Integration** for DDoS protection

---

## 🛠️ How CloudFront Works

1. User sends a request (e.g., to `cdn.example.com`)
2. CloudFront routes request to the nearest **edge location**
3. If content is cached:
   - ✅ Served immediately
4. If not cached:
   - ⬇️ Fetched from **origin** (S3, EC2, etc.)
   - 📦 Cached at edge for next request

---

## 🎯 Use Cases

- 🌐 Hosting static websites (via S3 + CloudFront)
- 🎥 Delivering large files (images, video, software)
- 🚀 Accelerating dynamic content (APIs, real-time apps)
- 🛡️ Serving secure content with signed URLs
- 🧭 Reducing latency for global users

---

## ⚙️ Example Setup (S3 + CloudFront)

1. Upload your website to an S3 bucket
2. Create a CloudFront **Web Distribution**
   - Set the S3 bucket as **origin**
   - Enable **OAC** for security
   - Set default root object to `index.html`
3. CloudFront generates a **domain name** (e.g., `d1234abcd.cloudfront.net`)
4. Optionally configure a custom domain + SSL
5. Done! 🚀

---

## 📊 Cache Invalidation

Sometimes you want to **refresh cache** (e.g., after a website update).

- You can **invalidate** specific paths:
  ```
  /index.html
  /images/*
  ```

- ⚠️ Invalidation requests may incur a cost!

---

## 💡 Tips & Best Practices

| ✅ Do                          | ❌ Don't                                |
|------------------------------|----------------------------------------|
| Use **OAC** for private S3   | Don't make S3 bucket public directly   |
| Use **versioning** in URLs   | Don’t rely on cache invalidation often |
| Enable **Compression**       | Don’t serve large assets without gzip  |
| Integrate with **WAF & Shield** | Ignore security for public content    |

---

## 🧪 Advanced Features

- Lambda@Edge → Run code at edge locations (auth, redirects, headers)
- Field-Level Encryption
- Geo-Targeting (different content for different countries)
- Real-Time Logs & Metrics via CloudWatch

---

## 📚 Useful Metrics (CloudWatch)

| Metric              | Description                                 |
|---------------------|---------------------------------------------|
| `Requests`          | Total number of requests                    |
| `BytesDownloaded`   | Outgoing data volume                        |
| `BytesUploaded`     | Incoming data volume                        |
| `4xxErrorRate`      | Client-side error rate                      |
| `5xxErrorRate`      | Server-side error rate                      |
| `CacheHitRate`      | % of requests served from edge              |

---

## 🧠 Summary

> CloudFront = 🔥 Speed + 🔐 Security + 💵 Cost Savings

- Perfect for **global delivery**
- Works with S3, EC2, ALB, and more
- Integrates deeply with AWS ecosystem
- Accelerates content and saves bandwidth

