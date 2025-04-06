
# 🚀 Hands-On: CloudFront with S3 Bucket

This guide walks you through **delivering a static website or content** from an S3 bucket using **AWS CloudFront** (Content Delivery Network).

---

## 🎯 Objective

- Host a static site or assets (HTML, JS, CSS, images) in an S3 bucket.
- Connect S3 to CloudFront for global delivery, performance boost, and security.
- Optional: Make S3 content **private**, only accessible via CloudFront.

---

## 🔧 Step-by-Step Setup

### ✅ Step 1: Create an S3 Bucket

1. Go to **S3 Console** → Click **"Create Bucket"**.
2. **Bucket Name**: `my-static-site-bucket` (globally unique)
3. Choose Region (same as most resources)
4. **Uncheck** “Block all public access” (only if using website mode)
5. Enable **Static Website Hosting**:
   - Upload `index.html`, `error.html`
   - Note the **Bucket Website Endpoint**

👉 If you're not using website hosting mode (for private content), skip this and use object URLs.

---

### ✅ Step 2: Upload Website Files

- Upload your static site files (HTML, CSS, JS, images).
- Make files **public** (optional) if using as website hosting.
- OR configure permissions to keep them private if using with CloudFront securely.

---

### ✅ Step 3: Create CloudFront Distribution

1. Go to **CloudFront Console**
2. Click **“Create Distribution”**
3. Under **Origin**:
   - Origin domain: Select your **S3 bucket**
   - If it's private: Use **Origin Access Control (OAC)** or **OAI** to restrict access.
4. **Origin Path** (optional): if you want to serve files from a sub-folder.
5. **Viewer Protocol Policy**: Redirect HTTP to HTTPS
6. **Cache Policy**: Choose default or “CachingOptimized”
7. **Default Root Object**: `index.html`

🛡️ **Access Control Option**:
- Use **OAC** (recommended): Grants CloudFront access to your S3 content without making it public.
- CloudFront will add a signed request to S3.

---

### ✅ Step 4: Restrict S3 Bucket Access to CloudFront (Security Best Practice)

1. In **S3 bucket permissions**, remove public access if using OAC.
2. Add the **CloudFront Origin Access Control policy** to allow only CloudFront to fetch files.
3. This ensures **users cannot bypass CloudFront** and access S3 directly.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "cloudfront.amazonaws.com"
      },
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-static-site-bucket/*",
      "Condition": {
        "StringEquals": {
          "AWS:SourceArn": "arn:aws:cloudfront::123456789012:distribution/EDFDVBD632BHDS5"
        }
      }
    }
  ]
}
```

> 📌 Replace `my-static-site-bucket` and distribution ARN accordingly.

---

### ✅ Step 5: Access Website via CloudFront URL

- Once distribution is deployed (~10 mins), access your site via:
  ```
  https://<your-distribution-id>.cloudfront.net
  ```
- You can also configure **Custom Domain + SSL** via Route53 and ACM later.

---

## ⚠️ Common Mistakes to Avoid

- ❌ Don't leave S3 public if using CloudFront with OAC/OAI.
- ❌ Don’t forget to set **Default Root Object** in CloudFront.
- ❌ Don’t use S3 Website endpoint with OAC. Use S3 REST endpoint.

---

## 🧠 Summary

| Feature                  | Benefit                         |
|--------------------------|----------------------------------|
| CloudFront + S3          | Fast, secure static content      |
| OAC/OAI Access           | Lock down S3 to CloudFront only |
| Global Edge Locations    | Low latency worldwide           |
| HTTPS Everywhere         | Secure by default                |
| Caching + TTLs           | Performance & cost saving       |

---

## 🚀 Bonus: Use Case

📦 **Use CloudFront + S3 for**:
- Static websites
- Image/video hosting
- Frontend apps (React/Angular)
- Downloadable software/files
