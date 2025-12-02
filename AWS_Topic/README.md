# **🌩️ Core Components of AWS (High-Level Overview)**

AWS provides more than 200 services, but the following are the **core** components every AWS user should know:

---

## ✅ **1. Compute Services**

These let you run applications, containers, and serverless workloads.

### **Main Compute Components**

* **EC2 (Elastic Compute Cloud)** – Virtual servers in the cloud.
* **Lambda** – Serverless compute, runs code without provisioning servers.
* **ECS (Elastic Container Service)** – Orchestration for Docker containers.
* **EKS (Elastic Kubernetes Service)** – Managed Kubernetes clusters.
* **Elastic Beanstalk** – Simplified deployment for web apps.

---

## ✅ **2. Storage Services**

AWS storage is scalable, durable, and cost-optimized.

### **Main Storage Components**

* **S3 (Simple Storage Service)** – Object storage for files, backups, logs.
* **EBS (Elastic Block Store)** – Block storage for EC2 (like virtual hard disks).
* **EFS (Elastic File System)** – Shared NFS file storage for Linux systems.
* **FSx** – Managed Windows & high-performance file systems (Lustre/NetApp).

---

## ✅ **3. Database Services**

AWS provides relational, NoSQL, and in-memory databases.

### **Main Database Components**

* **RDS** – Managed relational database (MySQL, PostgreSQL, MariaDB, Oracle, SQL Server).
* **Aurora** – High-performance AWS-native relational database.
* **DynamoDB** – Serverless NoSQL key-value database.
* **ElastiCache** – Managed Redis/Memcached.
* **Redshift** – Data warehousing.

---

## ✅ **4. Networking & Content Delivery**

These services connect your workloads inside AWS or to the internet.

### **Key Network Components**

* **VPC (Virtual Private Cloud)** – Your isolated network in AWS.
* **Route 53** – DNS and domain management.
* **CloudFront** – CDN for caching global content.
* **API Gateway** – Build/manage secure APIs.
* **ELB (Elastic Load Balancer)** – Distribute traffic across servers.

  * ALB – Application Load Balancer
  * NLB – Network Load Balancer
  * CLB – Classic Load Balancer

---

## ✅ **5. Security, Identity & Compliance**

Security is built into AWS architecture.

### **Main Security Components**

* **IAM (Identity & Access Management)** – User, roles, permissions.
* **KMS (Key Management Service)** – Encryption key management.
* **Secrets Manager** – Stores secrets securely.
* **Cognito** – Authentication for users and apps.
* **WAF & Shield** – Firewall & DDoS protection.

---

## ✅ **6. Management & Monitoring**

These help you monitor and manage AWS resources.

### **Key Monitoring Components**

* **CloudWatch** – Monitoring logs, metrics, alarms.
* **CloudTrail** – Auditing API activities.
* **Config** – Compliance and resource tracking.
* **Systems Manager (SSM)** – Manage OS and applications.

---

## ✅ **7. Developer Tools**

Services to support DevOps and automation.

### **Main DevOps Components**

* **CodeCommit** – Git repositories.
* **CodeBuild** – Build automation.
* **CodeDeploy** – Deployment automation.
* **CodePipeline** – CI/CD pipelines.
* **CloudFormation** – Infrastructure as Code (IaC).
* **Terraform (3rd party, popular with AWS)** – Many use Terraform for IaC.

---

## ✅ **8. Analytics**

Services to analyze big data and streaming data.

### **Core Analytics Components**

* **Athena** – Query data in S3 using SQL.
* **EMR** – Hadoop/Spark clusters.
* **Kinesis** – Real-time data streaming.
* **Glue** – ETL (Extract, Transform, Load).

---

## ✅ **9. AI/ML Services**

Machine learning and AI-powered tools.

### **Key ML Components**

* **SageMaker** – Build/train/deploy ML models.
* **Rekognition** – Image & video analysis.
* **Comprehend** – NLP (text analytics).
* **Textract** – Extract data from documents.

---

## ✅ **10. Application Integration**

Connect and coordinate distributed systems.

### **Key Components**

* **SNS** – Publish/subscribe messaging.
* **SQS** – Queues for decoupling services.
* **EventBridge** – Event-based integrations.
* **Step Functions** – Orchestration of workflows.

---

# 🎉 Summary (Super Quick)

| Category        | Core AWS Services            |
| --------------- | ---------------------------- |
| **Compute**     | EC2, Lambda, ECS, EKS        |
| **Storage**     | S3, EBS, EFS                 |
| **Database**    | RDS, Aurora, DynamoDB        |
| **Networking**  | VPC, Route 53, ELB           |
| **Security**    | IAM, KMS, WAF                |
| **Monitoring**  | CloudWatch, CloudTrail       |
| **DevOps**      | CodePipeline, CloudFormation |
| **Analytics**   | Athena, EMR, Kinesis         |
| **AI/ML**       | SageMaker                    |
| **Integration** | SQS, SNS, EventBridge        |

---

