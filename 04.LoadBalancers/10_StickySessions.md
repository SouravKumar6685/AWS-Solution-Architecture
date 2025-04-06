
# 📌 Sticky Sessions (Session Affinity) in AWS

---

## 🧠 What are Sticky Sessions?

**Sticky Sessions**, also known as **Session Affinity**, ensure that **requests from a specific client** are always routed to the **same backend instance** for the duration of the session.

This is useful for apps that **store user session data in memory**, and where switching between instances could break the session.

---

## 🚀 How Sticky Sessions Work in AWS?

AWS uses a cookie to track sessions:
- With **Application Load Balancer (ALB)**: It uses **ALB-generated** or **application-based cookies**.
- With **Classic Load Balancer (CLB)**: It uses **either duration-based or application-generated cookies**.

---

## 🍪 Cookie Types

### 1. **AWSALB Cookie** (ALB)
- Managed by ALB.
- Automatically created and inserted into the client response.
- Ensures requests stick to the same target within the target group.

### 2. **App Cookie**
- Created and managed by the **application**.
- ALB uses this cookie to route requests.

---

## 🛠️ How to Enable Sticky Sessions (ALB)

1. Go to **EC2 Console → Target Groups**
2. Select your **target group**
3. Click **"Attributes" tab**
4. Edit the **Stickiness** setting:
   - Enable **stickiness**
   - Choose:
     - **"LB cookie"** (Managed by AWS)
     - or **"App cookie"** (Managed by your app)
   - Set **duration** (in seconds)
5. Save changes

---

## ✅ When to Use Sticky Sessions

- Shopping cart applications 🛒
- In-memory session storage apps (e.g., chat apps)
- Real-time games or streaming sessions
- Apps with non-distributed cache/session data

---

## ❌ When Not to Use Sticky Sessions

- Stateless apps (REST APIs)
- Scalable microservices (better with shared session stores like Redis or DynamoDB)
- Apps already storing session in DB or external store

---

## ⚠️ Limitations

- Fails if the target instance goes down — session will break
- Doesn't scale well with multiple AZs unless session replication is done
- Not ideal for long-lived sessions if you're using Spot Instances

---

## 🔐 Security Considerations

- Cookies must be protected with **Secure** and **HttpOnly** flags
- Session hijacking is possible if cookies are not protected over HTTPS

---

## 💡 Best Practices

- Prefer **external session storage** (like Redis, RDS) for **horizontal scaling**
- Use **sticky sessions only when necessary** and for **short durations**
- Monitor load across targets to avoid **imbalances due to stickiness**

---

## 📚 Summary

| Feature             | ALB                              | CLB                              |
|---------------------|----------------------------------|----------------------------------|
| AWS-Managed Cookie  | Yes (AWSALB)                     | Yes (Elastic Load Balancing cookie) |
| App Cookie Support  | Yes                              | Yes                              |
| Control Duration    | Yes                              | Yes                              |
| Granular Control    | Better with ALB                  | Limited                          |

