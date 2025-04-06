
# ⚖️ Application Load Balancer (ALB) - AWS Notes

## 🧠 What is an Application Load Balancer?

An **Application Load Balancer (ALB)** is a part of **Elastic Load Balancing (ELB)** in AWS that operates at the **Application Layer (Layer 7)** of the OSI model.

It routes **HTTP/HTTPS traffic** based on:
- Content of the request (host, path, headers, query strings)
- User-defined routing rules

> Think of ALB as a **smart traffic controller** that routes web requests to the right service or application based on rules.

---

## 🔍 Key Features of ALB

| Feature                    | Description                                                                 |
|----------------------------|-----------------------------------------------------------------------------|
| **Layer 7 Routing**        | Routes requests based on URL path, hostname, headers, query strings         |
| **Host-based Routing**     | Directs traffic to different target groups using domain names (e.g. /api)    |
| **Path-based Routing**     | Routes traffic based on URL paths (e.g., /images, /videos)                  |
| **Support for WebSockets** | Fully supports WebSockets for real-time apps                                |
| **SSL Termination**        | Handles HTTPS and manages SSL certificates                                  |
| **Sticky Sessions**        | Binds a user session to the same target using cookies                       |
| **Target Groups**          | Routes traffic to EC2, ECS, Lambda, or IPs                                  |
| **Health Checks**          | Checks health of targets, reroutes traffic from unhealthy instances         |
| **Security Groups**        | Can restrict inbound/outbound traffic to/from ALB                           |
| **WAF Integration**        | Works with AWS Web Application Firewall                                     |

---

## 🧱 ALB Architecture

![](https://imgs.search.brave.com/nzBRKZmDXWZ6Z0Jsx0tpa9H72N7mR41NgaCs0HFPP9o/rs:fit:860:0:0:0/g:ce/aHR0cHM6Ly93d3cu/ZGVuc2lmeS5jb20v/d3AtY29udGVudC91/cGxvYWRzL2FydGlj/bGUtazhzLWNhcGFj/aXR5LWluZ3Jlc3Mt/bG9hZC1iYWxhbmNl/ci5zdmc)

---

## ⚙️ Target Group Example

You can create different target groups like:

- `web-app-group` → `/app/*`
- `admin-group`   → `/admin/*`
- `api-group`     → `/api/*`

Each target group can have:
- EC2 instances
- ECS services
- Lambda functions
- IP addresses (in on-prem or VPC)

---

## 📥 Listener Rules

Example Listener Rule on port 80:

```text
If Path is /api/* → forward to target group: api-group
If Host is admin.example.com → forward to target group: admin-group
```

---

## 🌐 Use Cases

- Microservices Architecture
- Path/Host-based routing
- Blue-Green or Canary Deployments
- SSL Termination and WebSocket support
- Routing to ECS/Lambda targets

---

## ✅ When to Use ALB?

| Use Case                            | Use ALB? |
|-------------------------------------|----------|
| HTTP/HTTPS traffic                  | ✅ Yes   |
| Host or path-based routing          | ✅ Yes   |
| Routing to Lambda functions         | ✅ Yes   |
| WebSockets, TLS termination         | ✅ Yes   |
| Layer 4 TCP/UDP load balancing      | ❌ No (use NLB) |

---

## 🔐 ALB Security Group Setup

- ALB: Allow inbound HTTP (80) or HTTPS (443) from the internet
- EC2: Allow inbound only from the ALB's Security Group

---

## 📊 Monitoring and Logs

- **Access Logs** (S3)
- **CloudWatch Metrics**: Request count, Target response time, Healthy host count
- **AWS WAF** for protection

---

## 🛠️ Hands-On Tip (AWS Console)

1. Go to **EC2 > Load Balancers**
2. Create a new **Application Load Balancer**
3. Choose VPC and at least 2 subnets (multi-AZ)
4. Add Listeners (HTTP/HTTPS)
5. Create **Target Groups** and attach instances
6. Add **Routing Rules** (Path or Host-based)
7. Review and Launch 🚀

---

## 🚨 Best Practices

- ☑️ Always enable health checks
- ☑️ Use HTTPS with SSL certificates (ACM)
- ☑️ Attach WAF for added protection
- ☑️ Use path-based routing to separate services
- ☑️ Keep logs enabled for debugging

---

# 🧭 X-Forwarded-* Headers in AWS and Web Architecture

These headers are added by **reverse proxies**, **load balancers** (like ALB), or **CDNs** to provide metadata about the original client request before it reaches your backend servers.

---

## 🔹 1. `X-Forwarded-For`

### 📌 Definition:
`X-Forwarded-For` contains the **original client's IP address** before the request passed through proxies or load balancers.

### 📦 Example:
```http
X-Forwarded-For: 203.0.113.1, 70.41.3.18, 150.172.238.178
```

- The **left-most IP** is the **originating client’s IP**.
- The rest are intermediate proxies.

### 🔍 Use Cases:
- Logging real client IPs behind a load balancer
- Geo-location of users
- Rate-limiting and security filtering
- Analytics

---

## 🔹 2. `X-Forwarded-Port`

### 📌 Definition:
`X-Forwarded-Port` shows the **original port number** used by the client in the request.

### 📦 Example:
```http
X-Forwarded-Port: 443
```

This means the user accessed the service on **port 443** (HTTPS).

### 🔍 Use Cases:
- Detecting protocol/port in backend services
- Redirection logic
- Constructing proper URLs in applications

---

## 🔹 3. `X-Forwarded-Proto`

### 📌 Definition:
`X-Forwarded-Proto` tells the backend what **protocol** (HTTP or HTTPS) the client used originally.

### 📦 Example:
```http
X-Forwarded-Proto: https
```

Even if the internal traffic is HTTP, the client requested via **HTTPS**, and this header tells the backend that.

### 🔍 Use Cases:
- Detecting secure requests behind a load balancer
- Force HTTPS redirects (in apps like Django, Rails, Express)
- Serving secure cookies or constructing absolute URLs

---

## 📁 Real-World Use Case

In AWS ALB or NGINX:

```http
X-Forwarded-For: 198.51.100.23
X-Forwarded-Port: 443
X-Forwarded-Proto: https
```

→ Your backend app knows:
- The **real IP** of the user
- They came via **HTTPS**
- They used **port 443**

---

## 🧠 How to Access These in Web Frameworks

- **Express.js (Node.js)**:
```js
req.headers['x-forwarded-for'];
req.headers['x-forwarded-proto'];
```

- **Django (Python)**:
```python
request.META.get('HTTP_X_FORWARDED_FOR')
```

- **Spring Boot (Java)**:
```java
request.getHeader("X-Forwarded-For");
```

---

## ⚠️ Security Tips

- ⚠️ `X-Forwarded-For` can be spoofed if you trust it blindly.
- ✅ In AWS, **trust ALB/NLB** to inject these headers.
- Use middleware like `helmet` (Node.js) or `django.middleware.security` to properly handle these headers.

---

## ✅ Summary

| Header              | Meaning                          | Example           |
|---------------------|----------------------------------|--------------------|
| `X-Forwarded-For`   | Original client IP               | `203.0.113.1`     |
| `X-Forwarded-Port`  | Port used by client              | `443`             |
| `X-Forwarded-Proto` | Protocol used by client (http/s) | `https`           |

These headers ensure your app behaves **correctly behind proxies**, and logs **accurate request information**.




## 🧠 Summary

- **ALB** = Advanced Layer 7 load balancer for smart routing
- Ideal for **web applications, APIs, and microservices**
- Supports **routing, health checks, SSL, WebSockets, and WAF**

