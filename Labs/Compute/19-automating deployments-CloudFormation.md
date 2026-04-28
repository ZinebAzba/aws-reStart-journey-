
---

#  **CloudFormation Infrastructure Deployment— Summary**  

This lab helped me understand how to build, update, and delete AWS infrastructure using **CloudFormation**. I worked with YAML templates, parameters, resources, and stack operations. Below is my full summary of Tasks 1–4, written from a student learning perspective.

---

#  Task 1 — Launch the CloudFormation Stack

##  Goal  
Deploy the initial infrastructure using a provided CloudFormation template.

##  What I Did  
- Opened the CloudFormation console.  
- Uploaded the starter template.  
- Launched the stack named **Lab**.  
- Waited for `CREATE_COMPLETE`.


<img width="1510" height="878" alt="Screenshot 2026-04-28 144722" src="https://github.com/user-attachments/assets/b6911183-5383-410e-ace8-95a962b11ff1" />


##  What I Learned  
- CloudFormation automates resource creation.  
- A stack groups resources so they can be managed together.  
- YAML indentation matters a lot.

---

#  Task 2 — Explore the Created Resources

##  Goal  
Understand what CloudFormation deployed.

##  What I Did  
I checked the **Resources** tab of the stack and explored:

- **VPC**
- **Public Subnet**
- **Internet Gateway**
- **Route Table + Route**
- **Security Group**

<img width="1521" height="910" alt="Screenshot 2026-04-28 145120" src="https://github.com/user-attachments/assets/20256cee-397a-4705-bc7b-4b82bf21c4d6" />

##  What I Learned  
- CloudFormation templates describe infrastructure as code.  
- Each resource has a logical ID and a physical ID.  
- Dependencies are handled automatically.

---

#  Task 3 — Update the Stack (Add EC2 + S3)

##  Goal  
Modify the template to add:
- An **EC2 instance**  
- An **S3 bucket**

##  What I Did  
- Added a new parameter for the Amazon Linux 2 AMI using SSM.  
- Added the `AppServer` EC2 instance.  
- Added the `MyBucket` S3 bucket.  
- Fixed YAML indentation issues.  
- Re‑uploaded the updated template.  
- CloudFormation detected changes and created the new resources.

- 
📸 *S3 Bucket created*

<img width="1518" height="870" alt="Screenshot 2026-04-28 150130" src="https://github.com/user-attachments/assets/4ee163e6-1912-4464-b59c-d719028651d4" />
<img width="1528" height="881" alt="Screenshot 2026-04-28 151121" src="https://github.com/user-attachments/assets/f8aa7ff6-afe3-499d-869e-e18594dfdccb" />

<img width="1528" height="881" alt="Screenshot 2026-04-28 151121" src="https://github.com/user-attachments/assets/0c051028-b2e9-4d1b-a837-8fbf476c1961" />



*EC2 — Change set preview*  

<img width="1305" height="280" alt="Screenshot 2026-04-28 153957" src="https://github.com/user-attachments/assets/d56428b0-2a87-4809-af94-a665227a280b" />

*EC2 created* 
<img width="1523" height="874" alt="Screenshot 2026-04-28 154036" src="https://github.com/user-attachments/assets/b5d637ea-b757-486c-9226-e679c81f8b77" />

<img width="1527" height="878" alt="Screenshot 2026-04-28 154051" src="https://github.com/user-attachments/assets/2f4d718b-756f-41e1-9e4f-a0e129121b1f" />

*EC2 instance running*  

<img width="1529" height="285" alt="Screenshot 2026-04-28 154244" src="https://github.com/user-attachments/assets/a788fea8-6071-448f-8572-d86f8dc1351a" />


##  What I Learned  
- Updating a stack is safer than manually editing resources.  
- YAML indentation errors break CloudFormation.  
- SSM parameters make AMI selection dynamic.  
- CloudFormation only updates what changed.

---

#  Task 4 — Delete the Stack

##  Goal  
Clean up all resources created by the stack.

##  What I Did  
- Selected the **Lab** stack.  
- Clicked **Delete** → **Delete stack**.  
- Watched it move to `DELETE_IN_PROGRESS`.  
- After a few minutes, the stack disappeared.

*Delete stack confirmation*  
<img width="1336" height="565" alt="Screenshot 2026-04-28 155926" src="https://github.com/user-attachments/assets/187293bc-b9c5-4c60-acf7-d991eeb15ff8" />


##  What I Learned  
- Deleting a stack removes all managed resources.  
- CloudFormation is a clean way to avoid leftover AWS costs.  
- Buckets must be empty before deletion (good to remember).

---

#  Final Reflection

This lab helped me understand the full lifecycle of infrastructure as code:

- **Deploy** a stack  
- **Inspect** the created resources  
- **Update** the stack with new resources  
- **Delete** everything cleanly  

I also learned how important YAML structure is, how CloudFormation handles dependencies, and how powerful it is to manage infrastructure declaratively.

This lab improved my confidence with:
- VPC networking  
- EC2 provisioning  
- S3 buckets  
- CloudFormation templates  
- Stack updates and change sets  





