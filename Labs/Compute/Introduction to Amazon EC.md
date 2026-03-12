# Introduction to Amazon EC2 <b>

## Overview

Amazon EC2 is a cloud service that provides flexible, scalable computing capacity. It lets developers quickly launch and manage virtual servers with full control over their resources. Because instances can be created in minutes and scaled up or down as needed, EC2 helps applications adapt to changing workloads efficiently. Its pay‑as‑you‑go model reduces costs by charging only for the capacity actually used. EC2 also includes features that support building resilient, fault‑tolerant applications.

## Topics covered
- Launch a web server with termination protection enabled</li>
- Monitor your EC2 instance</li>
- Modify the security group that your web server is using to allow HTTP access</li>
- Resize your Amazon EC2 instance to scale</li>
- Test termination protection</li>
- Terminate your EC2 instance</li>

## Lab Summary

### Launching the EC2 Instance
<img width="477" height="216" alt="image" src="https://github.com/user-attachments/assets/b93c29e3-1065-4c2f-9cc1-3fcf195ee73a" />


I created an EC2 web server by:
- Naming the instance  
- Selecting the default Amazon Linux AMI  
- Choosing a **t3.micro** instance type  
- Launching it **without a key pair**  
- Configuring networking using a lab VPC and a custom security group  
- Keeping the default **8 GiB** storage  
- Adding a **user‑data script** to install and start Apache and create a simple web page  

After launching, I waited for the instance to reach the **Running** state with all **3/3 checks passed**.

### Troubleshooting Web Server Access
Initially, I could not access my web server.  


<img width="200" height="202" alt="image" src="https://github.com/user-attachments/assets/a77fc55a-618b-4b4e-ba19-7b672791b8f9" />


After reviewing the inbound rules, I corrected the security group and successfully accessed the webpage.

<img width="214" height="26" alt="image" src="https://github.com/user-attachments/assets/7d6c0db7-13c0-454d-883c-7c8fc7d42a1b" />

### Scaling the Instance
I upgraded the EC2 instance by:
- Changing the instance type from **t3.micro → t3.small**  
- Increasing the EBS root volume from **8 GiB → 10 GiB**  
- Restarting the instance to apply the changes  

Everything worked as expected after the reboot.

---

## Screenshots

<img width="346" height="155" alt="image" src="https://github.com/user-attachments/assets/8b9b0202-b131-490c-9d43-43ae4e4d38fb" />

<img width="350" height="195" alt="image" src="https://github.com/user-attachments/assets/fbb2d153-f371-48c8-8539-2ad27f31d1cb" />






