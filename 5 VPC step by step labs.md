Perfect 👍 — here are **5 beginner-friendly AWS labs** that use **only the AWS Management Console (GUI)** — no CLI or coding required — ideal for learning **VPC (CPV: Cloud Private Virtual Network) concepts** step by step.

---

## 🧭 Overview

These labs are designed for **absolute beginners** to understand AWS networking hands-on.
Each lab uses only **GUI navigation**, so you can follow them easily inside your AWS Free Tier account.

---

# 🧪 **Lab 1 — Create a Custom VPC from Scratch**

### 🎯 Objective

Build your own **isolated Virtual Private Cloud (VPC)** with one public and one private subnet.

### 🧩 Steps

1. Go to **VPC → Your VPCs → Create VPC**
2. Select **“VPC Only”**
3. Enter details:

   * **Name tag:** `MyFirstVPC`
   * **IPv4 CIDR:** `10.0.0.0/16`
4. Click **Create VPC** ✅
5. Go to **Subnets → Create Subnet**

   * Choose your VPC → Add two subnets:

     * `10.0.1.0/24` → *Public Subnet*
     * `10.0.2.0/24` → *Private Subnet*
6. Go to **Internet Gateways → Create IGW** → Attach it to your VPC.
7. Go to **Route Tables → Create Route Table** → Name it `PublicRT`.
8. Edit routes → Add:

   ```
   Destination: 0.0.0.0/0
   Target: Internet Gateway
   ```
9. Associate this Route Table to your **Public Subnet**.
10. Done ✅ — you now have a basic **Public + Private VPC** setup.

---

# 🧪 **Lab 2 — Launch an EC2 Instance in Your VPC**

### 🎯 Objective

Launch a small EC2 instance in your **public subnet**.

### 🧩 Steps

1. Go to **EC2 → Instances → Launch Instance**.
2. Choose **Amazon Linux 2 (Free Tier)**.
3. Instance type: **t2.micro**.
4. Under **Network Settings:**

   * Select your custom **VPC**.
   * Subnet: **Public Subnet**.
   * Enable **Auto-assign Public IP: Yes**.
   * Security Group: Allow **SSH (port 22)** and **HTTP (port 80)**.
5. Launch the instance using a new key pair.
6. Once running, go to **Instance → Public IPv4 DNS → Open in browser.**

   * If you installed a web server, it should show a test page.

---

# 🧪 **Lab 3 — Create a Private EC2 Instance and NAT Gateway**

### 🎯 Objective

Enable Internet access for a private subnet using a **NAT Gateway**.

### 🧩 Steps

1. In your **private subnet**, launch another EC2 instance (same as Lab 2).

   * Disable **Auto-assign Public IP**.
2. Create a **NAT Gateway**:

   * Go to **VPC → NAT Gateways → Create NAT Gateway**.
   * Subnet: choose **Public Subnet**.
   * Allocate a new **Elastic IP**.
3. Go to **Route Tables** → select **Private Route Table**.

   * Edit routes → add:

     ```
     Destination: 0.0.0.0/0
     Target: NAT Gateway
     ```
4. Test: connect to the private instance via the **public EC2 (SSH jump)** or **Session Manager** → try `ping google.com`.
5. ✅ Private subnet now has **outbound-only** Internet access.

---

# 🧪 **Lab 4 — Connect Two VPCs Using VPC Peering**

### 🎯 Objective

Connect two VPCs so they can communicate privately.

### 🧩 Steps

1. Create **VPC-A (10.0.0.0/16)** and **VPC-B (172.31.0.0/16)**.
2. Go to **VPC → Peering Connections → Create Peering Connection**.

   * Requester: **VPC-A**
   * Accepter: **VPC-B**
3. In **VPC-B**, accept the request.
4. Go to **Route Tables** of each VPC and add routes:

   * In VPC-A:

     ```
     Destination: 172.31.0.0/16
     Target: Peering Connection
     ```
   * In VPC-B:

     ```
     Destination: 10.0.0.0/16
     Target: Peering Connection
     ```
5. Launch an EC2 in each VPC and test `ping` using private IPs.
6. ✅ VPCs are now peered and communicating privately.

---

# 🧪 **Lab 5 — Enable Flow Logs and Monitor Traffic**

### 🎯 Objective

Capture and view traffic logs for your VPC.

### 🧩 Steps

1. Go to **VPC → Your VPCs**.
2. Select your VPC → **Actions → Create Flow Log**.
3. Choose:

   * **Filter:** All
   * **Destination:** Create new CloudWatch Log Group → `VPC-FlowLogs`
4. Create the IAM role automatically when prompted.
5. Launch EC2 traffic (from previous labs).
6. Go to **CloudWatch → Logs → VPC-FlowLogs**.
7. You’ll see logs like:

   ```
   10.0.1.15 -> 8.8.8.8 ACCEPT TCP 443
   ```
8. ✅ You’re now monitoring network traffic.

---

## 🎓 **Summary Table**

| Lab | Focus             | Outcome                          |
| --- | ----------------- | -------------------------------- |
| 1   | Create Custom VPC | Public & Private subnets created |
| 2   | Launch EC2 in VPC | Instance accessible via Internet |
| 3   | Add NAT Gateway   | Private subnet outbound access   |
| 4   | VPC Peering       | Cross-VPC communication          |
| 5   | VPC Flow Logs     | Traffic monitoring enabled       |

---

Would you like me to create this as a **Markdown lab workbook (AWS VPC CPV Beginner Labs.md)** file that you can **download directly** and use as your personal training guide?
