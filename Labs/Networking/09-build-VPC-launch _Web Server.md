
# **Lab Summary – Building a Highly Available VPC with Public & Private Subnets**

##  Lab Objective
The goal of this lab is to design and deploy a **custom Virtual Private Cloud (VPC)** that follows AWS best practices for high availability and secure network segmentation.  
For this lab the following should be created: 

- A VPC with **four subnets** (two public, two private)  
- An **Internet Gateway** for public access  
- A **NAT Gateway** for secure outbound traffic from private subnets  
- Public and private **route tables**  
- A **security group** allowing HTTP access  
- A **web server EC2 instance** reachable from the internet  

This architecture mirrors real‑world production environments where public‑facing services and internal workloads must be isolated and protected.

---

#  What I Built
A complete VPC architecture containing:

- **Lab VPC** (10.0.0.0/16)
- **Public Subnet 1** (10.0.0.0/24)
- **Private Subnet 1** (10.0.1.0/24)
- **Public Subnet 2** (10.0.2.0/24)
- **Private Subnet 2** (10.0.3.0/24)
- **Internet Gateway** attached to the VPC
- **NAT Gateway** in Public Subnet 1
- **Public Route Table** (routes to IGW)
- **Private Route Table** (routes to NAT Gateway)
- **Web Security Group** allowing HTTP (port 80)
- **EC2 instance** running Apache web server

Even though the lab environment uses a single Availability Zone, the subnet layout follows the pattern used for multi‑AZ high availability.

---

#  Process Overview

## **1. VPC Creation**
I created a new VPC with a /16 CIDR block and enabled DNS support.  
The VPC serves as the isolated network environment for all resources.
<img width="742" height="571" alt="Screenshot 2026-04-19 223244" src="https://github.com/user-attachments/assets/012061f4-2ea1-4405-8338-14b8d5f482c7" />



## **2. Subnet Design**
I created four subnets:

- Two public subnets (10.0.0.0/24 and 10.0.2.0/24)  
- Two private subnets (10.0.1.0/24 and 10.0.3.0/24)
- 
<img width="1527" height="815" alt="image" src="https://github.com/user-attachments/assets/ab429abc-5e09-4df8-be96-f787e7c28be0" />

This layout supports load‑balanced, multi‑tier architectures.

## **3. Internet Gateway & NAT Gateway**
- The **Internet Gateway** enables inbound/outbound internet access for public subnets.  
- The **NAT Gateway** allows private subnets to reach the internet **outbound only**, keeping them secure.

## **4. Route Tables**
I configured:

### **Public Route Table**
- Associated with Public Subnet 1 and Public Subnet 2  
- Contains a default route to the Internet Gateway

  
<img width="1274" height="635" alt="image" src="https://github.com/user-attachments/assets/7d03f8d8-82dd-42d5-957f-0c6c7fcf51f5" />





<img width="1279" height="582" alt="image" src="https://github.com/user-attachments/assets/67176f09-c71e-4dea-a4ad-f8db0f0f3cd3" />


### **Private Route Table**
- Associated with Private Subnet 1 and Private Subnet 2  
- Contains a default route to the NAT Gateway  

<img width="1276" height="635" alt="image" src="https://github.com/user-attachments/assets/02bc119b-6670-4cb6-ad32-970470cc411f" />


This ensures correct traffic flow for both subnet types.

## **5. Security Group**
You created **Web Security Group** with:

- **Inbound:** HTTP (80) from Anywhere IPv4  
- **Outbound:** All traffic (default)

<img width="1284" height="620" alt="image" src="https://github.com/user-attachments/assets/b4c21293-8709-4931-815e-ce2e81efd533" />
<img width="1277" height="625" alt="image" src="https://github.com/user-attachments/assets/dc779551-57e6-4497-b5e6-183072c6673f" />


This allows public access to the web server while maintaining outbound connectivity.

## **6. EC2 Web Server Deployment**
I launched an EC2 instance in **Public Subnet 1**, attached the Web Security Group, and used user data to install Apache:

```
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "Welcome to the Customer Web Server" > /var/www/html/index.html

```
<img width="1526" height="767" alt="image" src="https://github.com/user-attachments/assets/e7e4c9c2-2cf7-424a-bfac-7b3fb189142b" />


Once the instance was running, I accessed the public IPv4 address in a browser and confirmed the web server was reachable.

<img width="526" height="171" alt="image" src="https://github.com/user-attachments/assets/6793b463-7498-46cf-a20e-fa463a94b660" />

---

#  Final Output
By the end of the lab, i successfully deployed:

- A fully functional VPC with public and private network tiers  
- A secure routing configuration using IGW and NAT Gateway  
- A working EC2 web server accessible over HTTP  
- A network architecture aligned with AWS best practices  



---


