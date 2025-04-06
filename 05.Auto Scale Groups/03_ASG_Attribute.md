
# ⚙️ Auto Scaling Group (ASG) Attributes

---

## 📘 What is an Auto Scaling Group?

An **Auto Scaling Group (ASG)** ensures you always have the right number of EC2 instances running.  
It automatically adds/removes instances based on defined policies, schedules, and health checks.

---

## 🔍 Key ASG Attributes You Must Know

Here are the main attributes and their purpose:

---

### 1️⃣ **Launch Template or Launch Configuration**
- Specifies how EC2 instances should be launched
- Includes: AMI, instance type, key pair, security groups, user data, etc.
- Prefer **Launch Templates** (modern & more features)

---

### 2️⃣ **Minimum Size**
- Minimum number of EC2 instances in the ASG
- Even during scale-in, it won’t go below this number

🧠 *Use case:* Maintain at least 2 backend instances always running.

---

### 3️⃣ **Maximum Size**
- Maximum number of EC2 instances the ASG can scale up to
- Prevents excessive scaling

🚨 *Set carefully to control costs!*

---

### 4️⃣ **Desired Capacity**
- Target number of instances that ASG tries to maintain
- Can lie between min & max size
- Changes when scaling events occur

---

### 5️⃣ **VPC and Subnets**
- ASG must be associated with at least one subnet (typically across AZs)
- Enables multi-AZ high availability

🌐 *Always prefer 2+ AZs for resilience*

---

### 6️⃣ **Health Checks**
- Ensures only healthy instances remain in service
- Two types:
  - **EC2 Health Check** (default)
  - **ELB Health Check** (recommended with Load Balancer)
- Can automatically terminate and replace unhealthy EC2s

---

### 7️⃣ **Health Check Grace Period**
- Wait time (in seconds) after launching an instance before checking its health
- Allows time for startup scripts to finish

⌛ *Default: 300 seconds (5 mins)*

---

### 8️⃣ **Scaling Policies**
- Define how and when to scale:
  - **Target Tracking** (e.g. CPU target)
  - **Step Scaling** (based on thresholds)
  - **Scheduled Scaling** (scale at specific time)

⚙️ *Helps handle predictable or unpredictable load*

---

### 9️⃣ **Cooldown Period**
- Time period after scaling activity to prevent rapid consecutive scale-in/out actions
- Avoids thrashing

---

### 🔟 **Instance Termination Policy**
- Controls **which instance** to terminate during scale-in
- Options:
  - OldestInstance
  - NewestInstance
  - ClosestToNextInstanceHour
  - Default: OldestLaunchConfiguration

---

### 🔁 **Replacement Policy**
- If an instance becomes unhealthy, ASG **automatically replaces** it
- Ensures capacity and health compliance

---

### 🔗 **Load Balancer Integration**
- ASG can be linked to:
  - **ALB** or **NLB** via **Target Groups**
- ASG registers and deregisters EC2 instances from Load Balancer automatically

---

### 🗓️ **Lifecycle Hooks**
- Pause instance launch/terminate process to perform custom actions (e.g. install software, send SNS)
- Types:
  - `autoscaling:EC2_INSTANCE_LAUNCHING`
  - `autoscaling:EC2_INSTANCE_TERMINATING`

🧪 *Used for custom bootstrapping or graceful shutdowns*

---

### 🏷️ **Tags**
- Metadata for cost tracking, automation, and grouping
- Example: `Environment=Production`, `Team=DevOps`

---

## 🧠 Summary Table

| Attribute                  | Purpose                                     |
|---------------------------|---------------------------------------------|
| Launch Template            | Defines EC2 launch configuration           |
| Min / Max / Desired Size   | Controls instance count limits             |
| Health Check & Grace Time | Ensures healthy EC2 operation               |
| Scaling Policies           | Defines how ASG scales                     |
| Cooldown                   | Prevents too frequent scaling              |
| Lifecycle Hooks            | Custom control during launch/terminate     |
| Termination Policy         | Chooses which instance to remove           |
| Subnets & AZs              | Enables multi-AZ deployments               |
| Tags                       | Organize and track resources               |

---

## ⚠️ Pro Tips

- ✅ Use **Target Tracking** for easy scaling setup
- ✅ Use **Lifecycle Hooks** for custom logic
- ⚠️ Avoid setting a **high max size** unless really needed
- ⚠️ Don’t forget to define a **Health Check Grace Period**
- ✅ Monitor using **CloudWatch Alarms**

