# 🎯 Hands-On: Launching a Spot Instance from AWS Console

This guide walks you through launching a **Spot EC2 instance** using the AWS Management Console.

---

## 🛠️ Step-by-Step Instructions

### 1️⃣ Log in to AWS Console
- Visit [https://console.aws.amazon.com](https://console.aws.amazon.com)
- Go to **EC2 Dashboard**

---

### 2️⃣ Launch a Spot Instance
- Click **"Launch Instance"**
- Under **Name and tags**, give your instance a name (e.g., `spot-demo`)

---

### 3️⃣ Choose Amazon Machine Image (AMI)
- Select an AMI, such as:
  - ✅ Amazon Linux 2
  - ✅ Ubuntu Server

---

### 4️⃣ Choose Instance Type
- Select a lightweight instance (e.g., `t3.micro`, `t2.micro`, `t3a.small` etc.)
- 📌 Make sure your instance type is **Spot eligible**

---

### 5️⃣ Set Purchase Option as Spot
- Under **Purchasing Options**, check ✅ **"Request Spot Instances"**

---

### 6️⃣ Configure Spot Settings
- **Interruption behavior**: Choose what happens when AWS reclaims the instance (e.g., Stop, Terminate)
- (Optional) Set a **maximum price** you're willing to pay (AWS will not go beyond this)
- (Optional) Set **duration** (if you want a fixed running time)

---

### 7️⃣ Configure Key Pair
- Choose an existing **Key Pair** or create a new one to connect via SSH
> 🔐 You need this `.pem` file to securely access your EC2 instance.

---

### 8️⃣ Configure Network & Security
- Choose or create a **VPC + Subnet**
- Attach or create a **Security Group**:
  - Allow **SSH (port 22)** from your IP
  - (Optional) Allow **HTTP/HTTPS** if needed

---

### 9️⃣ Storage & Tags (Optional)
- Add tags like `Name: spot-demo`
- Choose storage size/type (default 8 GB gp2 is fine for demo)

---

### 🔟 Launch the Instance
- Review the configuration
- Click ✅ **Launch Instance**

---

## ✅ Confirm Spot Request
- Go to **EC2 > Instances** → View your instance
- Spot Instances may take a few minutes to launch depending on availability

---

## 🔧 Connect to the Spot Instance
- Once the state is **Running**, click **Connect**
- Use:
  ```bash
  ssh -i "your-key.pem" ec2-user@<Public-IP>
  ```

---

## 🧹 Clean Up
After your demo:
- Go to **EC2 > Instances**
- Select your Spot Instance → Click **Instance state > Terminate**

> 🧽 Cleaning up avoids unwanted charges.

---

## 📝 Notes

| Feature             | Notes                                                   |
|---------------------|---------------------------------------------------------|
| 💰 Cost             | Up to 90% cheaper than On-Demand                        |
| 🔁 Can be interrupted | AWS may terminate the instance with 2-minute warning |
| 📌 Best For         | Batch jobs, dev/test, stateless apps, ML workloads      |
| ⚠️ Persistence      | Data lost if not backed to EBS or S3                    |

---

> 💡 **Pro Tip**: Always use EBS-backed volumes and store logs/data externally when using Spot instances.

