
---

#  Lab Summary — Working with Amazon S3 (File Sharing + Notifications)

## Overview
In this lab, I created and configured an Amazon S3 bucket to securely share product images with an external media user (`mediacouser`). I also set up automatic email notifications using Amazon SNS so that the administrator is alerted whenever the bucket contents change.

The workflow included:

- Creating an S3 bucket and uploading initial images  
- Reviewing IAM permissions for the external user  
- Testing allowed and denied actions  
- Configuring S3 event notifications to an SNS topic  
- Verifying that notifications are sent for object creation and deletion  

This lab demonstrates how to securely share S3 resources while maintaining visibility and control over changes.

---

##  Key Learnings

### 1. External user permissions
The `mediacouser` IAM user can:

- View objects  
- Upload new images  
- Delete images  

…but **cannot** change bucket‑level permissions.  
This ensures that external collaborators can work with files without compromising security.

### 2. Why the ACL change failed (`AccessDenied`)
When running:

```
aws s3api put-object-acl --bucket <cafe-xxxnnn> --key images/Donuts.jpg --acl public-read
```

The command failed because:

- **I am not the bucket owner**, so I cannot change who is allowed to access the object.  
- It’s like being allowed to enter a house but not allowed to change the locks.

This failure is expected and confirms that permissions are correctly restricted.

### 3. Why the `get-object` command did NOT generate a notification
The bucket was configured to send notifications **only for:**

- `s3:ObjectCreated:*`
- `s3:ObjectRemoved:*`

A `get-object` is just a **read**, and reads do not trigger S3 events.  
So no email notification is expected — and this confirms the configuration is correct.

### 4. This has nothing to do with private IPs
The AccessDenied error is purely about **permissions**, not networking.  
No private/public IP concepts are involved in this lab.

---

##  What I Did in the Lab

### ✔️ Task 1 — Connected to the CLI Host and configured AWS CLI  
Configured credentials and region to run AWS CLI commands.

### ✔️ Task 2 — Created the S3 bucket and uploaded images  
Used `aws s3 mb`, `aws s3 sync`, and `aws s3 ls` to initialize the bucket.

### ✔️ Task 3 — Reviewed IAM permissions  
Verified that the `mediaco` group grants:

- Read (GetObject)  
- Write (PutObject)  
- Delete (DeleteObject)  

…but **not** permission to modify bucket ACLs.

### ✔️ Task 4 — Configured SNS notifications  
Created an SNS topic, allowed S3 to publish to it, subscribed via email, and added an S3 event notification configuration.

### ✔️ Task 5 — Tested notifications  
- Upload → **Email received**  
- Delete → **Email received**  
- Get → **No email** (correct behavior)  
- Change ACL → **AccessDenied** (correct behavior)

---

##  Screenshots 



1. **S3 bucket showing the images folder**
 <img width="943" height="508" alt="image" src="https://github.com/user-attachments/assets/8a64a8a3-8c05-4cda-bf8b-c20f4f2e286a" />
 
2. **Terminal screenshot of the failed ACL command**  
   - Shows the AccessDenied error
   - <img width="936" height="139" alt="image" src="https://github.com/user-attachments/assets/6c8269dc-f8bb-4e79-8f9e-9a481499b583" />
   
3. **SNS topic configuration page**
. <img width="1908" height="357" alt="image" src="https://github.com/user-attachments/assets/4311e6b8-edfd-46fb-ad92-57d53f53b255" />

4. **Email inbox showing S3 notifications**
<img width="1022" height="182" alt="image" src="https://github.com/user-attachments/assets/0342096a-370d-4eaa-a7e7-82e62a89d96c" />
<img width="1285" height="251" alt="image" src="https://github.com/user-attachments/assets/988f8cf9-409f-422e-b571-cf25a702c06e" />
<img width="1295" height="251" alt="image" src="https://github.com/user-attachments/assets/588d9eba-6034-434b-8045-df535146822c" />

5. **IAM policy view:**  for `mediaco` group  
<img width="1915" height="375" alt="image" src="https://github.com/user-attachments/assets/8a57c3f4-207d-46bc-b06a-8077bb4c4e59" />

These screenshots demonstrate the full workflow and the security controls in action.

---

