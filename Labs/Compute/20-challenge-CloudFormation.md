
---

#  **CloudFormation Challenge — Lab Summary**

In this lab, I practiced building AWS infrastructure using **AWS CloudFormation** instead of creating resources manually. The goal was to write a CloudFormation template that automatically deploys a small network environment and an EC2 instance.

### **What I built**
Using my YAML template, CloudFormation successfully created:

- A **VPC** (10.0.0.0/16)  
- An **Internet Gateway** attached to the VPC  
- A **private subnet** (10.0.1.0/24)  
- A **route table** associated with the private subnet  
- A **security group** allowing SSH (port 22)  
- A **t3.micro EC2 instance** inside the private subnet  

### **What I learned**
- How CloudFormation automates AWS resource creation  
- How to structure a YAML template with parameters and resources  
- How dependencies are handled automatically by CloudFormation  
- How to upload and deploy a template using the AWS Console  
- How to verify stack creation through the **Events** and **Resources** tabs
  
<img width="1525" height="916" alt="Screenshot 2026-04-28 212409" src="https://github.com/user-attachments/assets/f0042828-979f-42b5-9eb0-e641c5750fab" />

<img width="1525" height="768" alt="Screenshot 2026-04-28 212358" src="https://github.com/user-attachments/assets/460c140e-f7d4-43d0-a5e4-a1b812d01a4e" />


### **Result**
My stack (**ChallengeStack**) deployed successfully with **CREATE_COMPLETE**, and all required resources were created without errors.


<img width="1522" height="382" alt="Screenshot 2026-04-28 212423" src="https://github.com/user-attachments/assets/45ff8307-9d37-4dee-b507-b4e41cf3ef97" />

---
