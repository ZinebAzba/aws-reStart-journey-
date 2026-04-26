
---

#  **AWS Café Security Incident Response — Full Lab Summary**

## **Overview**
In this lab, I investigated and resolved a security incident affecting the Café’s web server hosted on AWS. The goal was to identify how the attacker gained access, remove their presence, secure the environment, and restore the website to its original state. This exercise demonstrated practical incident‑response skills using EC2, IAM, CloudTrail, and Linux system administration.

---

## **1. Investigating the Security Breach**
The lab began with unusual behavior on the Café website. Using **AWS CloudTrail**, I traced suspicious activity back to an unauthorized IAM user named **chaos**, who had executed AWS CLI commands to modify the EC2 security group and open SSH access to the entire internet.

CloudTrail logs confirmed:
- The attacker used the AWS CLI  
- They modified the security group to allow `0.0.0.0/0` on port 22  
- They connected to the EC2 instance and created a new OS user  

This established the timeline and method of compromise.

---

## **2. Removing the Attacker’s OS Access**
After connecting to the EC2 instance using SSH, I identified the malicious OS user created by the hacker. I removed the account and terminated any active sessions associated with it.

Actions performed:
- Listed system users  
- Removed the unauthorized user  
- Confirmed no active malicious processes remained  

This ensured the attacker could no longer log in at the OS level.

---

## **3. Securing SSH Configuration**
The attacker had modified the SSH daemon configuration to enable password authentication, allowing them to log in without an SSH key.

I corrected the SSH settings by:
- Commenting out `PasswordAuthentication yes`  
- Enabling `PasswordAuthentication no`  
- Saving the configuration and restarting the SSH service  

Then I removed the insecure inbound rule from the EC2 security group, restoring SSH access to **my IP only**.

This closed the entry point the attacker used.

---

## **4. Restoring the Website**
Inside the web server directory, I found that the attacker had replaced one of the café’s images. Fortunately, they left a backup.

I restored the original website image by:
- Navigating to `/var/www/html/cafe/images/`  
- Renaming the backup file back to its original name  
- Refreshing the website to confirm the fix  

The café’s website returned to normal.

---

## **5. Removing the Malicious IAM User**
To prevent further AWS‑level access, I removed the attacker’s IAM user from the account.

Steps:
- Opened IAM in the AWS Console  
- Selected the **chaos** user  
- Deleted the user and confirmed removal  

This eliminated the attacker’s ability to interact with AWS resources.

---

## **Conclusion**
By the end of the lab, I successfully:
- Identified the attacker’s actions using CloudTrail  
- Removed unauthorized OS and IAM users  
- Secured SSH access and corrected configuration vulnerabilities  
- Restored the website to its original state  
- Closed the security gaps that allowed the breach  

This lab reinforced the importance of:
- Monitoring AWS activity with CloudTrail  
- Restricting SSH access  
- Using key‑pair authentication  
- Regularly reviewing IAM users and permissions  

The Café team can now continue maintaining their website with a stronger understanding of AWS security best practices.

---

