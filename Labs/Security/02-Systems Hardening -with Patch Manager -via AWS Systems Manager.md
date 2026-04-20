
---

#  Systems Hardening Lab — Patch Manager 

##  Lab Objective
In this lab, I learned how to harden both Linux and Windows systems by applying security patches using **AWS Systems Manager Patch Manager**. I practiced creating patch baselines, tagging instances, running on‑demand patch operations, and verifying compliance across all managed nodes.

---

##  What I Did

### **1. Patched Linux Instances (Using Default Baseline)**
- Verified that all Linux instances were already tagged with:  
  `Patch Group = LinuxProd`
- Ran **Patch now** with:
  - Operation: *Scan and install*
  - Target: *LinuxProd* tag
  - Reboot: *Reboot if needed*
- Observed the execution in **State Manager** and **Run Command**
- Confirmed all three Linux instances successfully installed updates
<img width="1794" height="1308" alt="Screenshot_20-4-2026_155543_us-west-2 console aws amazon com" src="https://github.com/user-attachments/assets/43729450-fa5e-40ba-a901-5ec206d51b17" />
<img width="1407" height="842" alt="Screenshot_20-4-2026_162329_us-west-2 console aws amazon com" src="https://github.com/user-attachments/assets/ceda3376-1776-4774-a0c5-a12822d3ad8e" />


---

### **2. Created a Custom Windows Patch Baseline**
- Created a new baseline: **WindowsServerSecurityUpdates**
- Operating system: *Windows*
- Added two approval rules:
  - **Critical** severity, *SecurityUpdates*, auto‑approve after 3 days  
  - **Important** severity, *SecurityUpdates*, auto‑approve after 3 days
- Associated the baseline with the patch group:  
  `WindowsProd`

---

### **3. Tagged and Patched Windows Instances**
- Added the tag to all Windows instances:  
  `Patch Group = WindowsProd
  
  `<img width="1265" height="688" alt="Screenshot 2026-04-20 1709111" src="https://github.com/user-attachments/assets/0f8a1774-1a43-4bde-8418-0ef1c35167e3" />


- Ran **Patch now** targeting the WindowsProd group
 <img width="1407" height="1469" alt="Screenshot_20-4-2026_164437_us-west-2 console aws amazon com" src="https://github.com/user-attachments/assets/7ef6e7a1-3000-45b2-81d1-7f0b49dd289a" />
 
- Observed patching progress through:
  - Patch Manager execution summary
  - State Manager execution details
  - Run Command output (showing PatchBaselineOperations)
- Confirmed all three Windows instances completed successfully

  

 
  
<img width="1498" height="868" alt="Screenshot 2026-04-20 165145" src="https://github.com/user-attachments/assets/6fc1bf31-9020-4d3c-a221-254b44fff48d" />

<img width="1524" height="462" alt="Screenshot 2026-04-20 165255" src="https://github.com/user-attachments/assets/f8c7c26d-da05-43e4-b254-a384a209e2d0" />



---

### **4. Verified Compliance**
- Opened **Patch Manager → Compliance reporting**
- Confirmed **all six instances** (3 Linux + 3 Windows) were **Compliant**

  <img width="1508" height="854" alt="Screenshot 2026-04-20 165809" src="https://github.com/user-attachments/assets/0a317ba3-01e6-4aa0-b0ac-8ed52cb1307e" />

- Reviewed:
  - Critical noncompliant count  
  - Security noncompliant count  
  - Other noncompliant count  
  - Last operation date  
  - Baseline ID used
- Opened one Windows instance → **Patch tab**  
  - Reviewed installed patches and timestamps

---

##  What I Learned
- How Patch Manager automates OS patching across multiple instances  
- How to create and customize patch baselines  
- How patch groups control which instances receive which baselines  
- How Systems Manager uses **Run Command** behind the scenes  
- How to verify patch compliance at both fleet and instance level  

---

