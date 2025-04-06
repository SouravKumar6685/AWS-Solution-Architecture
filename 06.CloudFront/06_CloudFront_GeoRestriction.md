
# 🌍 AWS CloudFront GeoRestriction (Geo Blocking)

## 📌 What is GeoRestriction?

**GeoRestriction** (aka **Geolocation Restriction**) in AWS CloudFront allows you to **allow or block content delivery** to users based on their **geographic location** 🌐.

It helps in complying with **licensing agreements, legal regulations, or business rules**.

---

## 🚀 Why Use GeoRestriction?

| Scenario | Use Case |
|----------|----------|
| 🎥 Content Licensing | Allow streaming in certain countries only |
| ⚖️ Legal Restrictions | Block access from sanctioned countries |
| 💸 Cost Management | Prevent usage from unwanted regions |
| 🧪 Beta Testing | Serve features to specific countries |

---

## 🛠️ How GeoRestriction Works

When a user sends a request to CloudFront:
- CloudFront uses the **viewer’s IP address** to determine the **country of origin**.
- If the country is in the **blocked list**, the request is denied with a **403 Forbidden**.
- If allowed, CloudFront serves the content normally.

---

## ⚙️ Types of GeoRestriction Policies

1. **Whitelist** – Only allow access from specific countries.
2. **Blacklist** – Block access from specific countries.

---

## ✍️ How to Set Up GeoRestriction (AWS Console)

1. Go to **CloudFront Console**
2. Select your **Distribution**
3. Choose the **"Behaviors"** tab
4. Edit the desired behavior
5. Under **"Restrict Viewer Access (Geo Restriction)"**, click **Yes**
6. Choose:
   - **Whitelist** or **Blacklist**
   - Select countries (searchable list)
7. Save and **Deploy**

⏳ It may take a few minutes to update across Edge Locations.

---

## 💡 Example Use Case

You are hosting a **video streaming site** with licensing allowed only in **India, US, and UK**.

You can:
- Set a **Whitelist policy**
- Allow content only to: `IN`, `US`, `GB`

Anyone outside will get a **403 Forbidden** error.

---

## ❗ Important Notes

- 🛑 Country detection is **based on IP**, not browser language or time zone.
- 🌍 You cannot restrict by **state or city** – only **country-level** restrictions are supported.
- 🧾 Consider using **Lambda@Edge** for more granular control (e.g., custom redirects).
- 📦 Works for **Web distributions** only (CloudFront + HTTP/HTTPS).
- 🔐 Combine with signed URLs or cookies for private content access.

---

## 🔄 Whitelist vs Blacklist Example

```yaml
Whitelist:
  - IN
  - US
  - GB
=> Only users from India, US, and UK can access content

Blacklist:
  - CN
  - RU
=> Users from China and Russia cannot access content
```

---

## 🧠 Summary

| Feature          | Details                                 |
|------------------|------------------------------------------|
| ✅ Use for        | Country-based access control             |
| 🔐 Level          | Distribution behavior-level              |
| 📍 Region Scope   | Country-based (no city/state granularity)|
| 💻 Detects via    | IP Address                               |
| ⚙️ Setup via      | CloudFront Behavior settings             |

---

## 📚 Further Learning

- [CloudFront GeoRestriction Docs](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/georestrictions.html)
- Use with Lambda@Edge for custom rules and redirect logic

