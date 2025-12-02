# 🚀 **AWS EC2 (Elastic Compute Cloud)**

**EC2** is basically **a virtual server in the cloud**.
Think of it as renting a computer from AWS where you choose:

* how much CPU you want
* how much RAM
* how much storage
* which operating system
* the network rules
* and how long you want it running

You pay **only for what you use**.

---

# 🔥 **Why EC2 Is Important**

It’s one of the core foundations of AWS.
If AWS was a city, EC2 would be the apartments where your apps “live.”

---

# 🧩 **Key Concepts You Should Know**

### **1. AMI (Amazon Machine Image)**

A template used to create your EC2 instance.
Examples:

* Amazon Linux
* Ubuntu
* Windows Server
* Custom AMIs

---

### **2. Instance Types**

AWS gives you different “sizes” depending on your workload.

| Instance Type | Use Case                        |
| ------------- | ------------------------------- |
| **t2 / t3**   | Low-cost, general-purpose       |
| **m5 / m6**   | Balanced compute + memory       |
| **c5 / c6**   | Compute-heavy apps              |
| **r5 / r6**   | Memory-heavy apps               |
| **g4 / g5**   | GPU workloads (ML/AI, graphics) |

---

### **3. Security Groups**

Firewall rules for your EC2 instance.
Example: allow port **22** for SSH, **80/443** for web traffic.

---

### **4. EBS (Elastic Block Store)**

Storage attached to your instance — works like a hard disk.

---

### **5. Elastic IP**

A static IP you can attach to your EC2 instance.

---

### **6. Load Balancers**

When running multiple EC2 servers, you can put an **ELB** in front to distribute traffic.

---

### **7. Auto Scaling**

Automatically adds/removes EC2 instances based on load.

---

# ⚙️ **Common EC2 Use Cases**

* Hosting a website or web app
* Running backend APIs
* Hosting a database (though RDS is preferred)
* Machine learning workloads
* Video rendering
* Any application that needs full control over OS

---


