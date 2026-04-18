
---

# **Lab Summary – Cloud Support Engineer Intervention**

In this lab, I played the role of a Cloud Support Engineer helping a customer troubleshoot why their EC2 instance could not reach the internet.  
To reproduce the issue and validate the customer’s architecture, I first replicated their environment exactly as described:

- **VPC:** 192.168.0.0/18  
- **Public Subnet:** 192.168.1.0/26  
- **Internet Gateway:** Created and attached  
- **EC2 Instance:** Launched in the public subnet  
- **Security Group:** Applied around the EC2 instance  
- **Network ACL:** Custom NACL created and associated with the public subnet  

This represents a minimal public subnet architecture.

---

## **1. VPC Creation**

I created the VPC using the same CIDR block as the customer and verified that the subnet, route table, and IGW were configured correctly.

**Screenshot**  
<img width="1354" height="523" alt="Screenshot 2026-04-18 114456" src="https://github.com/user-attachments/assets/52de78cc-b1a7-4369-a4d7-b1a24159631e" />
<img width="1902" height="909" alt="Screenshot 2026-04-18 125705" src="https://github.com/user-attachments/assets/227d495e-07da-4084-abea-73f600789cfc" />
---


## 2.  Network ACL & Security Group Creation
To match the customer’s intended configuration, I created a custom Network ACL for the public subnet.
I added inbound and outbound rule 100 to allow all traffic and associated the NACL with the public subnet.
This ensured that no stateless filtering would block outbound or return traffic.

In addition, I created the Security Group that would be attached to the EC2 instance.
The SG allows inbound SSH (22), HTTP (80), and HTTPS (443) from anywhere (IPv4), and allows all outbound traffic.
This ensures the instance can be accessed and can reach the internet.


**Screenshot**  
<img width="1913" height="910" alt="Screenshot 2026-04-18 130232" src="https://github.com/user-attachments/assets/e3e78b89-6592-4b43-8545-33a20423bdcb" />


<img width="1907" height="915" alt="Screenshot 2026-04-18 130253" src="https://github.com/user-attachments/assets/59af894a-9ff3-40c1-a8d9-1e865b35ee30" />

<img width="1917" height="920" alt="Screenshot 2026-04-18 130304" src="https://github.com/user-attachments/assets/a565b401-44d0-4cce-b21b-56fd8915e940" />


---

## **3. EC2 Instance Launch**

Next, I launched an EC2 instance inside the public subnet, enabled auto‑assign public IP, and attached the security group allowing SSH, HTTP, and HTTPS.

**Screenshot**  
<img width="1902" height="909" alt="Screenshot 2026-04-18 125705" src="https://github.com/user-attachments/assets/227d495e-07da-4084-abea-73f600789cfc" />

---

## **4. Connectivity Test (Ping)**

After connecting to the instance via SSH (PuTTY), I tested outbound connectivity using the `ping` command.  
The instance successfully reached external domains, confirming that the VPC infrastructure was functioning correctly.

**Screenshot**  
<img width="800" height="980" alt="Screenshot 2026-04-18 125725" src="https://github.com/user-attachments/assets/44c5b283-d6ce-4382-834e-40b60a426f63" />

---

## **5. Customer Communication**

Since everything worked properly in my replicated environment, I contacted the customer to explain the root cause of their issue and what they needed to implement to restore connectivity.  
I clarified that the missing components were:

- A default route (`0.0.0.0/0`) pointing to the Internet Gateway  
- Proper subnet association with the public route table  
- Ensuring the instance had a public IP  
- Ensuring the NACL allowed return traffic  

Once these were corrected, the customer’s EC2 instance would be able to ping external hosts successfully.

---
 
- A **customer-facing email** 

Subject: Resolution – EC2 Instance Unable to Reach the Internet

Hello,

I reviewed your VPC configuration and identified the root cause of the connectivity issue. Your EC2 instance was unable to reach the internet because the subnet did not have a route to an Internet Gateway (IGW). Without this route, outbound traffic cannot leave the VPC.

To resolve the issue, I created and attached an Internet Gateway to your VPC, updated the route table to include a default route (0.0.0.0/0) pointing to the IGW, and ensured that the subnet was associated with this route table. I also verified that your security group and network ACL allow outbound traffic.

After applying these changes, I launched a test EC2 instance in the public subnet and confirmed successful connectivity using the ping command. The instance can now reach external hosts, confirming that the VPC is configured correctly.

Please let me know if you would like assistance setting up additional subnets, NAT gateways, or private workloads.

Kind regards,
Zineb  
AWS Cloud Support (Lab Simulation)
