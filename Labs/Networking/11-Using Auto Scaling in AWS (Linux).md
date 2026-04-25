
---

#  **Lab Summary — Using Auto Scaling in AWS (Linux)**  


## **1. Overview**
In this lab, I learned how to build a scalable and highly available web application using **Amazon EC2 Auto Scaling**, **Launch Templates**, **Load Balancers**, and **Target Groups**.  
The goal was to automatically adjust the number of EC2 instances based on CPU load and verify both scale‑out and scale‑in behavior.

---

## **2. What I Built**
- A **custom AMI** containing a PHP web app  
- A **Launch Template** using that AMI  
- An **Auto Scaling Group (ASG)** across two private subnets  
- An **Application Load Balancer (ALB)** with a target group  
- A **CPU‑based scaling policy** (target tracking at 50%)  
- A stress‑test script to trigger scaling events

---

## **3. Key Steps & What I Learned**

### **3.1 Creating the AMI**
I launched a base EC2 instance, installed the web application, and created an AMI called **WebServerAMI**.  
This AMI ensured every Auto Scaling instance had the same configuration.

📸 *Screenshot Placeholder — AMI creation page*

<img width="1324" height="492" alt="image" src="https://github.com/user-attachments/assets/e09698bb-95a4-4086-a2cf-b8278a1c222a" />

---

### **3.2 Creating the Launch Template**
I created a launch template named **web-app-launch-template**, specifying:
- AMI: *WebServerAMI*  
- Instance type: *t3.micro*  
- Security group: *HTTPAccess*  
- No key pair (not needed for this lab)

📸 *Screenshot Placeholder — Launch Template configuration*
<img width="951" height="594" alt="Screenshot 2026-04-25 214705" src="https://github.com/user-attachments/assets/f50bb8ba-c0ac-4464-9351-186cb88b08dd" />
<img width="944" height="569" alt="Screenshot 2026-04-25 214732" src="https://github.com/user-attachments/assets/45fcee1d-5f43-4517-b5c6-f1428a8c73fa" />

---

### **3.3 Creating the Auto Scaling Group**
I created an ASG named **Web App Auto Scaling Group**, configured to:
- Run in **Private Subnet 1 & 2**  
- Attach to the **webserver-app** target group  
- Use **ELB health checks**  
- Maintain:
  - **Desired:** 2  
  - **Min:** 2  
  - **Max:** 4  

📸 *Screenshot Placeholder — ASG instance management showing 2 instances*

<img width="1647" height="759" alt="Screenshot 2026-04-25 215850" src="https://github.com/user-attachments/assets/7c56d0d3-21b5-4f97-beba-95a89347d321" />

---

### **3.4 Verifying Health Checks**
I confirmed:
- Both EC2 instances passed **2/2 (or 3/3)** status checks  
- Both appeared as **healthy** in the target group  

📸 *Screenshot Placeholder — Target Group with 2 healthy instances*
<img width="1893" height="873" alt="Screenshot 2026-04-25 220206" src="https://github.com/user-attachments/assets/9a9952da-80d1-46a2-bd4e-0a9415c8ca9a" />

---

### **3.5 Triggering Scale‑Out**
Using the ALB DNS name, I opened the test web app and clicked **Start Stress**.  
This increased CPU usage above 50%, causing the ASG to launch additional instances.

I observed:
- ASG scaling from **2 → 3 → 4 instances**  
- New instances registering in the target group and becoming healthy

📸 *Screenshot Placeholder — ASG showing 3 or 4 instances during scale‑out*
<img width="1910" height="880" alt="Screenshot 2026-04-25 220049" src="https://github.com/user-attachments/assets/85e0e5c7-721e-4d13-b75e-c49166c3af02" />

---

### **3.6 Triggering Scale‑In**
After clicking **Stop Stress**, CPU dropped.  
The ASG automatically terminated extra instances and returned to the desired capacity of **2**.

📸 *Screenshot Placeholder — ASG scaling back to 2 instances*
<img width="1038" height="364" alt="Screenshot 2026-04-25 221247" src="https://github.com/user-attachments/assets/5a3e1b04-aa3e-4cfe-9161-b447b0d23022" />

---


## **4. Scaling Activity Timeline**
During the lab, I observed the full lifecycle of Auto Scaling events:

- CPU exceeded 50% → scale‑out triggered  
- ASG increased capacity from **2 → 3 → 4**  
- After stopping stress, CPU dropped  
- Scale‑in triggered automatically  
- ASG terminated instances safely using **connection draining**  
- Capacity returned to **2 instances**

📸 *Screenshot Placeholder — ASG Activity tab showing scale‑out and scale‑in events*
<img width="1496" height="908" alt="Screenshot 2026-04-25 224032" src="https://github.com/user-attachments/assets/3c6499c3-53b2-48d7-82e1-27d264c26855" />
<img width="1497" height="908" alt="Screenshot 2026-04-25 224703" src="https://github.com/user-attachments/assets/906d14dc-605e-435d-a9bf-ebd9719b9959" />

---

## **5. Troubleshooting & What I Learned**
During scale‑in, the instance count did not immediately decrease.  
I learned that:

- Auto Scaling waits for **ELB connection draining** before terminating  
- Target tracking policies include a **stabilization window** (several minutes)  
- Scale‑in can take **5–7 minutes** even after CPU drops  
- The correct target group must be checked under the **Load balancing** tab of the ASG  
- The **Activity** tab is the best place to confirm whether scale‑in has started  
- Seeing an instance in **Terminating** state confirms scale‑in is in progress

📸 *Screenshot Placeholder — Activity tab showing “Connection draining in progress”*
<img width="1496" height="908" alt="Screenshot 2026-04-25 224032" src="https://github.com/user-attachments/assets/8b8bbf18-3cd2-45f9-98ed-f379dde26138" />

<img width="1649" height="376" alt="image" src="https://github.com/user-attachments/assets/ca4c76cc-4d83-4136-b488-2af879ed4bfc" />
---

## **6. Final Architecture State**
At the end of the lab:

- The Auto Scaling Group stabilized at **2 healthy instances**  
- Instances were distributed across **two Availability Zones**  
- The ALB routed traffic correctly  
- The target group showed **2 healthy registered targets**  
- Scaling policies worked exactly as expected

📸 *Screenshot Placeholder — ASG with 2 InService instances*  
📸 *Screenshot Placeholder — Target Group with 2 healthy targets*

<img width="1668" height="410" alt="image" src="https://github.com/user-attachments/assets/d31db0b6-f9bc-4686-9a4e-e471fddba6d4" />
<img width="1919" height="871" alt="image" src="https://github.com/user-attachments/assets/100ba814-d387-48ae-8e86-95b72615844c" />

---

## **7. Key Takeaways**
- Auto Scaling ensures **high availability** and **cost efficiency**  
- Launch Templates standardize instance configuration  
- ALB + Target Groups distribute traffic and monitor health  
- Target tracking policies automatically adjust capacity  
- Stress testing is essential to validate scaling behavior  
- Real AWS scaling includes delays: draining, cooldown, stabilization  

---

