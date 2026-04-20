
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
  `Patch Group = WindowsProd`
- Ran **Patch now** targeting the WindowsProd group
- Observed patching progress through:
  - Patch Manager execution summary
  - State Manager execution details
  - Run Command output (showing PatchBaselineOperations)
- Confirmed all three Windows instances completed successfully

---

### **4. Verified Compliance**
- Opened **Patch Manager → Compliance reporting**
- Confirmed **all six instances** (3 Linux + 3 Windows) were **Compliant**
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

