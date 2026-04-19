

#  **Customer Request for Support**

Hello, Cloud Support!

When I create an Apache server through the command line, I cannot ping it. I also get an error when I enter the IP address in the browser. Can you please help figure out what is blocking my connection?
Thanks!
Ana
Contractor
---


##  Troubleshooting Summary – Apache Connectivity Issue

###  I investigated the issue
I reviewed the customer’s VPC configuration, including:
- Subnet associations  
- Route tables  
- Internet gateway attachment  
- EC2 instance configuration  
- Security groups and NACLs  
- Apache service status on the instance  

Apache was installed and running correctly, but the instance was unreachable from the internet.
<img width="672" height="411" alt="image" src="https://github.com/user-attachments/assets/dfd346fa-d8f6-4f9d-b842-177f9448dfbd" />

---

###  I found that
The security group attached to the EC2 instance only allowed:
- SSH (TCP 22)

It did **not** allow:
- HTTP (TCP 80)  
- ICMP (ping)

This prevented the Apache server from responding to web requests and caused the connection timeout.

---

### 🔧 I changed
I updated the inbound rules of the instance’s security group:

- **Added:**  
  - `HTTP (TCP 80)` from `0.0.0.0/0`  
  - `ICMP - Echo Request` from `0.0.0.0/0`

These rules allow public web access and basic connectivity testing.
<img width="1280" height="788" alt="image" src="https://github.com/user-attachments/assets/1483f969-f461-476f-9f65-ca4a438f76ef" />

---

###  The issue was resolved
After applying the updated rules:
- The Apache test page loaded successfully using the instance’s public IP  
- The EC2 instance became reachable from the internet  
- The customer’s connectivity issue was fully resolved  
<img width="1520" height="369" alt="image" src="https://github.com/user-attachments/assets/35b5dbe6-4b42-407f-976a-bd30075dc80a" />


###  Final Result
The Apache HTTP server is now accessible and functioning as expected.
```
#  **AWS Support Email (Professional Tone)**

**Subject:** Resolution for Apache Server Connectivity Issue

Hello Ana,

I investigated the issue you reported regarding the inability to reach your Apache server from the internet.

I found that the EC2 instance was correctly running the Apache HTTP server; however, the security group attached to the instance only allowed inbound SSH (port 22). There were no inbound rules permitting HTTP (port 80) or ICMP traffic. Because of this, the instance could not respond to web requests or ping tests.

I changed the security group configuration by adding inbound rules for HTTP (TCP port 80) and ICMP from `0.0.0.0/0`.

The issue was resolved after applying these changes. Your Apache test page now loads successfully using the instance’s public IP address.

Please let me know if you need any further assistance.

Best regards,  
Cloud Support Engineer  
Amazon Web Services

---

