
---

# Lab: Introduction to AWS Identity and Access Management (IAM)

##  Objectives
This lab introduced IAM concepts and demonstrated how to manage access to AWS resources through users, groups, roles, and policies.

---

##  What I Learned
- Created and applied a **custom password policy** to enforce stronger account‑wide security.
- Explored **pre‑created IAM users and groups** to understand how permissions are organized.
- Inspected **managed and inline policies** to see how actions, effects, and resources define access.
- Added users to groups to **inherit permissions** automatically instead of assigning them individually.
- Used the **IAM sign‑in URL** to log in as different users and tested their permissions across services.
- Observed how **policies control access**:
  - `user‑1` could view S3 buckets (read‑only).
  - `user‑2` could view EC2 instances but not modify them.
  - `user‑3` could start and stop EC2 instances as an administrator.

---

##  Key Takeaways
IAM enables secure, scalable access management by applying the **principle of least privilege**.  
Groups and policies simplify permission control, and testing with different users shows how AWS enforces access boundaries.

---

## Screenshots


#### Assigned users to defined groups ####
<img width="1519" height="441" alt="Screenshot 2026-04-22 114331" src="https://github.com/user-attachments/assets/5c246779-de02-46aa-9a7e-6a35652e84f5" />

#### When a user is not allowed to perform an action  ####
<img width="1517" height="357" alt="Screenshot 2026-04-22 115228" src="https://github.com/user-attachments/assets/30838767-3b78-450a-a94f-194167e8352f" />
<img width="1526" height="906" alt="Screenshot 2026-04-22 115851" src="https://github.com/user-attachments/assets/7df2df6b-ca94-4f36-91b7-d959109957a2" />
<img width="1515" height="876" alt="Screenshot 2026-04-22 120722" src="https://github.com/user-attachments/assets/d9e3c60e-96e9-4580-a9b3-31583c2a4bc3" />

#### When a user is allowed to perform an action  ####


<img width="1517" height="518" alt="Screenshot 2026-04-22 115255" src="https://github.com/user-attachments/assets/9353d6c3-c1d2-4581-baad-922acfae8a1c" />



<img width="1516" height="388" alt="Screenshot 2026-04-22 121105" src="https://github.com/user-attachments/assets/024414a3-4b4c-4096-acb7-e5ff47691f0e" /> 
---

