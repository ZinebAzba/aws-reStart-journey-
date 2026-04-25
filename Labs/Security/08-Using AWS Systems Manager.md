
---

#  Lab Summary — Using AWS Systems Manager

##  Overview  

This lab introduced AWS Systems Manager. I followed guided steps to collect inventory, run remote commands, manage parameters, and access an EC2 instance without SSH. I’m still learning how these features fit together, but this lab helped me understand the basics.

---

##  Objectives  
- Generate inventory for managed instances  
- Install software remotely using Run Command  
- Manage application settings with Parameter Store  
- Access an EC2 instance using Session Manager  

---

##  Task 1 — Generate Inventory Lists  
I created an **Inventory Association** in Fleet Manager to collect OS, application, and metadata information from a managed EC2 instance.  
This enables configuration validation without logging into the instance.


<img width="951" height="473" alt="image" src="https://github.com/user-attachments/assets/d171cd9d-0f6f-4aa9-861e-07c779c809ee" />

---

##  Task 2 — Install a Custom Application Using Run Command  
I used a custom Systems Manager document to install the **Widget Manufacturing Dashboard**.  
The script installed Apache, PHP, AWS SDK, and the dashboard application, then started the web server.

After execution, I validated the installation by opening the instance’s public IP in a browser.


<img width="1344" height="562" alt="image" src="https://github.com/user-attachments/assets/54e078d2-7eea-4cf9-9a89-e9d0f380f8e5" />
<img width="1083" height="449" alt="image" src="https://github.com/user-attachments/assets/3afc8f60-5e07-4a85-9056-499a84b4dfc2" />
<img width="1199" height="389" alt="image" src="https://github.com/user-attachments/assets/b9944bbb-a7d5-4d60-9ee2-043299a24988" />

---

##  Task 3 — Manage Application Settings with Parameter Store  
I created a parameter `/dashboard/show-beta-features` with the value `True`.  
The application automatically detected this parameter and displayed an additional **beta chart**.

Deleting the parameter removed the chart again, demonstrating feature‑flag behavior.


<img width="1333" height="552" alt="image" src="https://github.com/user-attachments/assets/dc64d15e-b100-462c-9e7a-86735b92197d" />

---

##  Task 4 — Access the Instance Using Session Manager  
I opened a secure, browser‑based shell session to the EC2 instance using Session Manager — no SSH keys or open ports required.
I verified the application files and used the AWS CLI inside the instance to list EC2 details.

<img width="1347" height="599" alt="Screenshot 2026-04-25 143740" src="https://github.com/user-attachments/assets/f89dac0f-360e-48db-b6be-af1886d55b49" />

---

## Key Learnings  
- Systems Manager centralizes instance management and reduces operational overhead  
- Run Command enables remote software installation at scale  
- Parameter Store supports secure, dynamic configuration management  
- Session Manager provides secure, auditable access without SSH  

---

