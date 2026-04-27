
---

#  **Optimize Utilization – Lab Summary (by Sofia:-))**

##  **Lab Objective**
In this lab, I played the role of *Sofia*, a junior cloud technician responsible for optimizing the cost and performance of the Café web application. My goal was to:

- Remove unnecessary components from the CaféInstance  
- Resize the EC2 instance to a more cost‑efficient type  
- Use the AWS Pricing Calculator (new interface) to estimate monthly savings  
- Validate that the application still works after optimization  

---

##  **1. Connecting to the CaféInstance**
I started by SSH‑ing into the CaféInstance using its public IP address.  
Once connected, I confirmed I was on the correct machine:

```
[ec2-user@web-server ~]$
```


##  **2. Removing the Local Database**
The Café application had already migrated to Amazon RDS, so the local MariaDB/MySQL database was no longer needed.

I stopped and removed the database service:

```bash
sudo systemctl stop mariadb
sudo systemctl disable mariadb
sudo yum remove mariadb-server -y
sudo rm -rf /var/lib/mysql
```

This helped reduce resource usage on the instance.

 **Screenshot Placeholder:** *Database removal commands*
<img width="657" height="459" alt="image" src="https://github.com/user-attachments/assets/6ef22cf0-cdad-4196-a252-4148c327e510" />
<br><br>
<img width="1346" height="140" alt="image" src="https://github.com/user-attachments/assets/f6185929-6c5f-4b52-a02f-6683987878ad" />

---

##  **3. Resizing the EC2 Instance**
To optimize costs, I resized the CaféInstance from **t3.small → t3.micro**.

Steps I followed:

1. Stopped the instance  
2. Changed the instance type to **t3.micro**  
3. Restarted the instance  
4. Verified the website still loads  

<img width="1909" height="919" alt="Screenshot 2026-04-27 170441" src="https://github.com/user-attachments/assets/c784d849-6a8f-4739-a4ca-1c57027013a5" />

<img width="1354" height="599" alt="Screenshot 2026-04-27 170658" src="https://github.com/user-attachments/assets/dfff84ae-a821-4d59-8cad-b9eb46e06d15" />


<img width="1908" height="491" alt="Screenshot 2026-04-27 170811" src="https://github.com/user-attachments/assets/34feaf81-8fed-4dfa-9e8b-ae6463738e9c" />
<br><br>
When I opened the public IP, I saw:

```
Hello From Your Web Server!
```

This confirmed the instance was healthy after resizing.
<img width="457" height="137" alt="Screenshot 2026-04-27 171129" src="https://github.com/user-attachments/assets/a837fe8d-676c-4a79-95b7-e4a02d83dd6d" />


---

##  **4. Estimating Cost Savings (New AWS Pricing Calculator UI)**

The lab instructions were written for the old calculator, but I used the **new interface**, which uses **730 hours/month** instead of 750.

I created **two EC2 estimates**:

### **Before Optimization – t3.small**
- Instance type: **t3.small**
- Monthly cost: **15.18 USD**
- Utilization: 730 hours/month  
- Pricing model: On‑Demand


---

### **After Optimization – t3.micro**
- Instance type: **t3.micro**
- Monthly cost: **7.59 USD**
- Utilization: 730 hours/month  
- Pricing model: On‑Demand

<img width="729" height="615" alt="Screenshot 2026-04-27 174322" src="https://github.com/user-attachments/assets/e572872e-ccce-47b7-936b-1a5885575b3a" />


---

###  **Cost Comparison**
| Configuration | Instance Type | Monthly Cost | Annual Cost |
|--------------|----------------|--------------|--------------|
| Before Optimization | t3.small | 15.18 USD | 182.16 USD |
| After Optimization | t3.micro | 7.59 USD | 91.08 USD |

###  **Savings**
- **Monthly savings:** 7.59 USD  
- **Annual savings:** 91.08 USD  

---

##  **What I Learned**
- How to SSH into EC2 instances and identify the correct host  
- How to remove unused services to reduce resource consumption  
- How to resize an EC2 instance safely  
- How to use the **new AWS Pricing Calculator UI**  
- How instance type changes impact monthly AWS costs  
- How to validate application functionality after optimization  

---

##  **Lab Completed**
This lab helped me understand how small configuration changes can significantly reduce cloud costs while keeping the application running smoothly.

---

