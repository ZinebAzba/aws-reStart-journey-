
---

# **VPC Connectivity Troubleshooting & Flow Log Analysis — Lab Summary**

This lab focused on diagnosing and resolving multiple connectivity issues in a VPC environment, then validating the fixes using VPC Flow Logs.  
Below is a summary of all **four challenges**, including the root cause, fix, and verification steps.

---

# **Challenge 1 — Web Server Not Reachable (HTTP Failure)**

### **Root Cause**  
The public subnet’s route table did not contain a default route to the Internet Gateway, preventing inbound HTTP traffic from reaching the web server.

### **Fix**  
Added the missing route:  
```
0.0.0.0/0 → igw-0b09ce885b9d49386
```

### **Screenshot Placeholder**  
<img width="432" height="134" alt="Screenshot 2026-04-15 160744" src="https://github.com/user-attachments/assets/2f2fff82-ef6a-441d-8b38-efaeb59fa7db" />


---

# **Challenge 2 — SSH Access Failing (EC2 Instance Connect)**

### **Root Cause**  
The Network ACL associated with the public subnet contained an inbound **DENY** rule on port **22**, blocking SSH traffic.

### **Fix**  
Removed the deny rule and allowed inbound/outbound traffic for port 22.

### **Screenshot Placeholder**  
`[Insert screenshot: NACL inbound rules BEFORE fix]`  
`[Insert screenshot: NACL inbound rules AFTER fix]`
<img width="892" height="368" alt="Screenshot 2026-04-15 162524" src="https://github.com/user-attachments/assets/ea696125-1f83-4476-aabf-c724c511ee06" />


---

# **Challenge 3 — Flow Logs Download & Analysis**

### **Steps Completed**
- Downloaded VPC Flow Logs from S3  
- Extracted `.gz` files into `.log` files  
- Inspected log structure using `head`  
- Searched for rejected traffic using:
  ```
  grep -rn REJECT .
  ```
- Filtered for SSH traffic:
  ```
  grep -rn 22 . | grep REJECT
  ```
- Filtered by my public IP to isolate my own failed SSH attempts  
- Verified the ENI in the logs matches the Web Server’s ENI:
  ```
  aws ec2 describe-network-interfaces --filters "Name=association.public-ip,Values='<WebServerIP>'"
  ```
- Converted Unix timestamps to human-readable format:
  ```
  date -d @<timestamp>
  ```

### **Screenshot Placeholder**  

<img width="545" height="672" alt="Screenshot 2026-04-15 172246" src="https://github.com/user-attachments/assets/f5957050-818f-456c-8efc-327a254eff13" />
<img width="548" height="674" alt="Screenshot 2026-04-15 172541" src="https://github.com/user-attachments/assets/4ecd2f7a-4764-4409-9bfd-79a8901c2dac" />

<img width="544" height="104" alt="Screenshot 2026-04-15 172514" src="https://github.com/user-attachments/assets/5a36c12d-74f4-4d02-8c1a-59f2d68617ea" />


<img width="549" height="47" alt="Screenshot 2026-04-15 172604" src="https://github.com/user-attachments/assets/0c8d5716-29e3-4af7-867b-2ed4d7f08d8e" />
<img width="552" height="217" alt="Screenshot 2026-04-15 172625" src="https://github.com/user-attachments/assets/d3aab7ce-ca79-46a2-b6e9-e1276169aede" />

<img width="466" height="97" alt="Screenshot 2026-04-15 172803" src="https://github.com/user-attachments/assets/38dd37f8-82f2-442f-8abc-aba1e8820add" />


<img width="429" height="55" alt="Screenshot 2026-04-15 172901" src="https://github.com/user-attachments/assets/7e3feb1d-067d-409f-8741-0a13be1e772a" />
<img width="479" height="47" alt="Screenshot 2026-04-15 172938" src="https://github.com/user-attachments/assets/44e00534-f1cb-48ae-afb5-31b209104ab5" />

`[Insert screenshot: Flow logs folder structure]`  
`[Insert screenshot: Extracted .log files]`  
`[Insert screenshot: head <flowlog-file>]`  
`[Insert screenshot: grep -rn REJECT .]`  
`[Insert screenshot: grep -rn 22 . | grep REJECT]`  
`[Insert screenshot: grep filtered by my IP]`  
`[Insert screenshot: ENI verification output]`  
`[Insert screenshot: date -d @timestamp]`

---

# **Challenge 4 — Confirming the Fixes & Understanding Traffic Behavior**

### **Outcome**
- Flow logs confirmed that:
  - HTTP failures were caused by the missing IGW route  
  - SSH failures were caused by the NACL deny rule  
  - The ENI in the logs matched the Web Server’s ENI  
  - Timestamps aligned with the failed SSH attempts  
- After correcting the route table and NACL, both HTTP and SSH connectivity were restored.

---

# **Final Conclusion**

This lab demonstrated how routing, NACLs, and flow logs work together in a VPC.  
By analyzing rejected traffic, matching ENIs, and converting timestamps, I validated the root causes and confirmed that the applied fixes restored full connectivity.

---


