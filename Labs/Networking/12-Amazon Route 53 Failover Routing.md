
---

# **AWS Route 53 Failover Lab — Summary**

##  Lab Objective  
Implement DNS failover using **Amazon Route 53**, ensuring that a web application automatically switches from a **primary EC2 instance** to a **secondary EC2 instance** when the primary becomes unavailable.

This lab demonstrates:

- Creating and configuring **Route 53 health checks**  
- Creating **primary and secondary failover A records**  
- Testing DNS failover by stopping the primary instance  
- Observing health check transitions and DNS routing behavior  

---

##  1. Health Check Configuration  
A Route 53 health check was created to monitor the primary web server’s `/cafe/index.php` endpoint.

**Configuration details:**

- Protocol: **HTTP**  
- Path: `/cafe/index.php`  
- Failure threshold: **2**  
- Health check name: **Primary-Website-Health**  

 *Health check creation form*
  <img width="955" height="603" alt="Screenshot 2026-04-26 114623" src="https://github.com/user-attachments/assets/a669e7cc-69f9-4735-86a1-dd0e35c52ffa" />
 
 *Health check monitoring graph — healthy*  
<img width="1361" height="603" alt="Screenshot 2026-04-26 114922" src="https://github.com/user-attachments/assets/e88bcea3-4857-468a-93e5-b92f301540f5" />

<img width="1358" height="606" alt="Screenshot 2026-04-26 115244" src="https://github.com/user-attachments/assets/b3f696ed-69da-4cdc-9517-d54c914e301b" />

---

##  2. Creating Route 53 Failover Records  

### **Primary A Record**
- Name: `www`  
- Type: `A`  
- Value: **CafeInstance1IPAddress**  
- Routing policy: **Failover**  
- Failover type: **Primary**  
- Health check: **Primary-Website-Health**  
- Record ID: `FailoverPrimary`  

### **Secondary A Record**
- Name: `www`  
- Type: `A`  
- Value: **CafeInstance2IPAddress**  
- Routing policy: **Failover**  
- Failover type: **Secondary**  
- Health check: *(none)*  
- Record ID: `FailoverSecondary`  


*Hosted zone showing NS, SOA, and both A records*  
<img width="1914" height="909" alt="Screenshot 2026-04-26 121154" src="https://github.com/user-attachments/assets/3d836e5b-9eb2-48bc-a8a3-924bef5124ac" />

---

##  3. Verifying DNS Resolution (Primary Active)  
Accessed the website:

```
http://www.<your-domain>.vocareum.training/cafe/
```

The page correctly displayed the **primary instance’s Availability Zone**, confirming that DNS routing was pointing to the primary server.


*Café page showing primary AZ*  
<img width="1353" height="717" alt="Screenshot 2026-04-26 121507" src="https://github.com/user-attachments/assets/70014bc0-aeaa-460d-a1ec-fe461931f605" />

---

##  4. Testing Failover  
To simulate failure, **CafeInstance1** was stopped in EC2.

<img width="1358" height="599" alt="Screenshot 2026-04-26 121736" src="https://github.com/user-attachments/assets/5eefd620-4904-4526-8c94-ab91ae175c12" />



### **Observed behavior:**
- Route 53 health check transitioned from **Healthy → Unhealthy**  
- DNS automatically routed traffic to the **secondary instance**  
- Refreshing the café website showed a **different Availability Zone** (secondary AZ)  


  *Health check graph showing failure*  

<img width="1316" height="912" alt="Screenshot 2026-04-26 122019" src="https://github.com/user-attachments/assets/fedd556e-6ef4-4164-8228-fe80a43342a4" />
<img width="1910" height="914" alt="Screenshot 2026-04-26 122443" src="https://github.com/user-attachments/assets/88c1cc3f-f959-42c2-96bb-05cf628c5e64" />




 *Café page showing secondary AZ*  
<img width="1353" height="717" alt="Screenshot 2026-04-26 122121" src="https://github.com/user-attachments/assets/9ee51f18-0b7a-493a-9f01-3dc9896728b0" />

---

##  Challenges with the New AWS Interface  
This lab was completed using the **updated AWS Route 53 interface**, which differs from the original lab instructions.

### **Key differences encountered:**

- **SNS alarm creation is no longer available** inside the health check wizard  
  - Therefore, **no email notification** is sent when the health check becomes unhealthy  
- UI layout for health checks and record creation has changed  
- Some labels and navigation paths differ from the older screenshots in the lab manual  
- Despite these differences, the lab was successfully completed using the updated interface  


---

##  What I Learned  
- How Route 53 health checks monitor endpoint availability  
- How DNS failover routing works using **Primary** and **Secondary** A records  
- How to validate failover behavior using EC2 instance state changes  
- How to interpret Route 53 health check metrics  
- How to adapt lab instructions to the **new AWS console interface**  

---



