
# ⚙️ Auto Scaling Group with Load Balancer (ELB)

---

## 🚀 Overview

When you combine **Auto Scaling Group (ASG)** with a **Load Balancer**, it ensures:

- **High availability** across multiple instances and zones ✅
- **Automatic scaling** based on demand 📈📉
- **Traffic distribution** to healthy instances only 💡

This setup is perfect for modern cloud apps that require fault tolerance, scalability, and efficiency.

---

## 🔄 How They Work Together

- **ASG** manages the number of EC2 instances.
- **Load Balancer (ALB/NLB)** distributes incoming traffic.
- **ASG registers instances** with the Load Balancer.
- Health checks ensure **only healthy EC2s** receive traffic.

---

## 🔧 Setup Steps in AWS Console

### ✅ Prerequisites:
- One or more **subnets in different AZs**
- A **Launch Template**
- An existing or new **Application Load Balancer (ALB)** or **Network Load Balancer (NLB)**

---

### 🔨 Step-by-step Setup

### 1️⃣ Create a Launch Template
- Go to **EC2 > Launch Templates > Create launch template**
- Configure:
  - AMI
  - Instance type
  - Security group (must allow inbound from ALB on port 80/443)
- Save the template

---

### 2️⃣ Create Load Balancer (ALB)
- Go to **EC2 > Load Balancers > Create ALB**
- Choose:
  - **Application Load Balancer**
  - Internet-facing
- Select VPC, subnets (2+ AZs recommended)
- Add **Listener (HTTP 80 / HTTPS 443)**
- Create **Target Group**:
  - Choose **instance** type
  - **Protocol**: HTTP
  - **Health Check**: HTTP path (e.g., `/health`)
- Create ALB

---

### 3️⃣ Create Auto Scaling Group
- Go to **EC2 > Auto Scaling Groups > Create ASG**
- Use **Launch Template**
- Select **VPC + Subnets**
- Attach to **existing Target Group**
- Define:
  - Min, Max, Desired capacity
  - Scaling policies (e.g., target tracking)
- Choose **Health Check** type: EC2 + ELB
- Review & Create

---

### ✅ Result:
- ALB forwards traffic to **healthy EC2s in ASG**
- ASG automatically adds/removes EC2s based on traffic
- If instance fails → ASG replaces it
- If traffic increases → ASG scales up
- If traffic drops → ASG scales down

---

## 💡 Benefits of ASG + Load Balancer

| Feature | Benefit |
|--------|---------|
| ✅ Auto Healing | Automatically replaces unhealthy EC2 |
| ✅ Load Distribution | Traffic split among healthy instances |
| ✅ High Availability | Supports Multi-AZ deployment |
| ✅ Elasticity | Grows and shrinks with traffic |
| ✅ Better Cost Control | No over-provisioning required |

---

## 📌 Example Use Case

🛒 **E-Commerce Website**  
- Traffic spikes during sales
- ALB handles load routing
- ASG scales up backend EC2s
- When sale ends, ASG scales down

---

## ⚠️ Pro Tips

- Use **ALB Target Group health checks** over EC2 checks
- Enable **Cross-Zone Load Balancing** on ALB
- Don’t forget to allow traffic from ALB in EC2 **security group**
- Monitor via **CloudWatch + Alarms**
- Set a **cooldown period** to avoid rapid scale-in/out

---

## 🧠 Diagram (Textual)

```
User Request → ALB → Target Group → EC2 Instances (Managed by ASG)
                                         ↑
                        Health Checked & Scaled Automatically
```

---

## 🧪 Sample Target Tracking Policy

- **Metric**: Average CPU Utilization
- **Target Value**: 50%
- **Cooldown**: 300 seconds

---

## 🔚 Summary

Combining **Auto Scaling Group** with a **Load Balancer** =  
📈 **Scalable**, 🔄 **Self-healing**, 🌐 **Highly available**, and 💸 **Cost-optimized** architecture.

This is a **core AWS design pattern** used in nearly every production-grade deployment.

