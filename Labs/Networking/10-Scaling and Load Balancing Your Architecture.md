
---

#  **Lab Summary — Scale and Load Balance Your Architecture**

##  **Lab Objective**
The goal of this lab was to design and deploy a **highly available, scalable, and load‑balanced architecture** on AWS by combining:

- A **custom AMI**
- An **Application Load Balancer (ALB)**
- A **Launch Template**
- An **Auto Scaling Group (ASG)**
- CloudWatch alarms for **dynamic scaling**

The final architecture automatically distributes traffic and scales EC2 instances based on CPU load.

---

#  **What I Learned**
Throughout this lab, I learned how different AWS services work together to create a resilient and scalable application environment:

### **1. Creating a Custom AMI**
- I launched an EC2 instance, installed a web server, and created a reusable AMI.
- This AMI ensured that every Auto Scaling instance launched with the same configuration.

### **2. Configuring an Application Load Balancer**
- I created an ALB with a public DNS name.
- I learned that the ALB is the **only public entry point**, while EC2 instances remain private.
- The ALB performs **health checks** and routes traffic only to healthy instances.

### **3. Building a Launch Template**
- I created a launch template defining the AMI, instance type, and security group.
- This template ensures consistency across all Auto Scaling instances.

### **4. Deploying an Auto Scaling Group**
- I configured the ASG to launch instances in **private subnets** across two Availability Zones.
- I attached the ASG to the ALB’s target group.
- I set scaling policies based on **average CPU utilization**.

### **5. Verifying Load Balancing**
- I confirmed that the ALB successfully routed traffic to private EC2 instances.
- I accessed the Load Test application using the ALB DNS name.

### **6. Testing Auto Scaling**
- By generating high CPU load, I triggered CloudWatch alarms.
- The ASG automatically launched a **third instance**, demonstrating successful scale‑out behavior.

---

#  **Screenshot Placeholders (with suggestions)**

### **1. Custom AMI Created**
<img width="1353" height="611" alt="image" src="https://github.com/user-attachments/assets/767e2c7e-1f84-435f-8f52-5a3dbd2ae734" />


### **2. Load Balancer Configuration**
<img width="1352" height="601" alt="image" src="https://github.com/user-attachments/assets/ba2e2f44-0157-4c4e-a518-9364b3443377" />


### **3. Launch Template Details**
<img width="1911" height="881" alt="image" src="https://github.com/user-attachments/assets/360f58bd-c225-423d-8eff-457501f0706f" />


### **4. Auto Scaling Group Created**
<img width="1915" height="905" alt="image" src="https://github.com/user-attachments/assets/68f666c1-35ed-4e9e-bf40-9048683961b7" />


### **5. Target Group Health Checks**
<img width="1359" height="600" alt="image" src="https://github.com/user-attachments/assets/6b8054e9-987a-47b3-805f-0d4a8d2601aa" />


### **6. Load Test Application Running**
<img width="1151" height="301" alt="image" src="https://github.com/user-attachments/assets/b96d0a13-6d8d-4e45-8d72-25b9b9771b68" />


### **7. CloudWatch Alarms Triggering**
<img width="1356" height="601" alt="image" src="https://github.com/user-attachments/assets/30fc0975-851a-4a61-93d6-886b0fb8831e" />


### **8. Auto Scaling Scale‑Out**
<img width="1348" height="596" alt="image" src="https://github.com/user-attachments/assets/e810842a-9e28-452b-b5d7-2311d475cae4" />


---

#  **Final Result**
By the end of the lab, I successfully deployed a **fully automated, load‑balanced, and scalable architecture**.  
The system:

- Routes traffic through a public ALB  
- Runs EC2 instances in private subnets  
- Automatically scales based on CPU load  
- Ensures high availability across multiple AZs  

This lab helped me understand how AWS services integrate to build real‑world, production‑grade architectures.

---

