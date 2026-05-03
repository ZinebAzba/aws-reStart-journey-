

---

#  **Troubleshooting AWS CloudFormation Deployments**  
### *CloudFormation Troubleshooting, Drift Detection & Stack Deletion*

This lab focused on diagnosing CloudFormation stack failures, analyzing userdata logs, preventing rollbacks, detecting drift, and resolving deletion failures. I worked from a pre‑provisioned EC2 “CLI Host” instance and used the AWS CLI to perform all troubleshooting steps.

---

##  **Objectives**
- Connect to a CLI Host EC2 instance and configure AWS CLI  
- Deploy a CloudFormation stack and analyze failure causes  
- Inspect userdata logs on an EC2 instance created by CloudFormation  
- Fix template errors and redeploy successfully  
- Perform drift detection after manual resource changes  
- Resolve a DELETE_FAILED stack by retaining resources  

---

##  **Task 1 — Troubleshooting CloudFormation Stack Failures**

### **1.1 — SSH into the CLI Host & Configure AWS CLI**
- Connected via SSH using either `.ppk` (Windows) .  
- Retrieved the instance’s public IP from the lab credentials panel.
- Retrieved the instance’s region using instance metadata.  
- Ran `aws configure` and entered Access Key, Secret Key, region, and JSON output format. 

📸 *Screenshot Placeholder — SSH connection to CLI Host*
<img width="625" height="118" alt="Screenshot 2026-04-28 171151" src="https://github.com/user-attachments/assets/e3563204-6d68-4fd1-8870-15aac955001c" />



---

### **1.2 — Attempt to Create the CloudFormation Stack**
- Reviewed the provided template (`template1.yaml`) using `less`.  
- Launched the stack:

```
aws cloudformation create-stack \
--stack-name myStack \
--template-body file://template1.yaml \
--capabilities CAPABILITY_NAMED_IAM \
--parameters ParameterKey=KeyName,ParameterValue=vockey
```

- Monitored resource creation with `watch` and `describe-stack-resources`.  
- Observed resources being created, then deleted — indicating rollback.  
- Identified the failure: **WaitCondition timed out**.

*STACK CREATED_DELETED_ROLLBACK*


<img width="605" height="294" alt="Screenshot 2026-04-28 172024" src="https://github.com/user-attachments/assets/2b5b20fe-6fce-4868-a9e4-d5749099d3f0" />
--

<img width="564" height="290" alt="Screenshot 2026-04-28 172145" src="https://github.com/user-attachments/assets/3806911a-1961-4bbe-ae1a-521c9acd3355" />
--

<img width="1088" height="556" alt="Screenshot 2026-04-28 172256" src="https://github.com/user-attachments/assets/cbbd27e3-6fca-4981-8e87-28fc82c05a18" />

*CREATE_FAILED events*
<img width="944" height="531" alt="Screenshot 2026-04-28 172511" src="https://github.com/user-attachments/assets/b0ebf29a-c5ca-4fc1-97f4-a16b7ca54b0b" />

---

### **1.3 — Root Cause Analysis**
- The EC2 instance was deleted during rollback, so userdata logs were lost.  
- Recreated the stack **without rollback**:

```
--on-failure DO_NOTHING
```
<img width="942" height="166" alt="Screenshot 2026-04-28 173653" src="https://github.com/user-attachments/assets/f8e4a840-d7e3-4905-816e-8556c1fa3878" />

- The recreation failed again , but the above preserved the EC2 instance for inspection.
  
*ISSUE_Timeout of the WaitConditions*
<img width="942" height="964" alt="Screenshot 2026-04-28 174053" src="https://github.com/user-attachments/assets/72eca386-5305-4313-8cfb-83932b2cfec7" />


### **1.4 — Inspecting the Web Server EC2 Instance**
- Retrieved the instance’s public IP using `describe-instances`.  
- SSH’d into the Web Server instance.  
- Checked userdata logs:

```
tail -50 /var/log/cloud-init-output.log
```

- Found the error:  
  **“No package http available”**  
- Inspected the userdata script (`part-001`) and confirmed the incorrect package name.

---

### **1.5 — Fixing the Template**
- Edited `template1.yaml` and replaced:

```
yum install -y http
```

with:

```
yum install -y httpd
```

- Deleted the failed stack and redeployed.  
- Verified all resources reached **CREATE_COMPLETE**.  
- Tested the web server in a browser — saw the message:  
  **“Hello from your web server!”**

*Successful CREATE_COMPLETE*

<img width="939" height="311" alt="Screenshot 2026-04-28 183331" src="https://github.com/user-attachments/assets/2d9a7abd-8f5f-4ae5-9063-3ad664aaaf17" />
<br><br>
<img width="980" height="1000" alt="Screenshot 2026-04-28 183446" src="https://github.com/user-attachments/assets/6d3348ed-e187-4a33-b7ca-b59b7da8259d" />
<br><br>
<img width="393" height="133" alt="Screenshot 2026-04-28 183612" src="https://github.com/user-attachments/assets/a0a38a33-648c-4073-8c7c-55b617098265" />


---

##  **Task 2 — Drift Detection**

### **2.1 — Manual Modification**
- Modified the WebServerSG inbound SSH rule to “My IP” instead of `0.0.0.0/0`.

### **2.2 — Add an Object to the S3 Bucket**
- Retrieved bucket name from stack outputs.  
- Uploaded a file (`myfile`) to the bucket using `aws s3 cp`.

### **2.3 — Detect Drift**
- Started drift detection:

```
aws cloudformation detect-stack-drift --stack-name myStack
```

- Drift result: **DRIFTED**  
- Only the **security group** showed drift (MODIFIED).  
- S3 bucket remained **IN_SYNC** because adding objects does not count as drift.

---

##  **Task 3 — Deleting the Stack**

### **Initial Delete Attempt**
- Attempted to delete the stack.  
- All resources deleted **except the S3 bucket**.  
- Stack entered **DELETE_FAILED** because the bucket contained objects.

<img width="925" height="295" alt="Screenshot 2026-04-28 190911" src="https://github.com/user-attachments/assets/c9b724d5-d071-4654-a684-b93b8245ad76" />
<br><br>
<img width="936" height="1028" alt="Screenshot 2026-04-28 191013" src="https://github.com/user-attachments/assets/fe32d80a-6d3a-4440-9cef-973a386fa177" />

### **Challenge — Delete the Stack Without Deleting the Bucket**
- Used the `--retain-resources` option to keep the bucket while deleting the stack:

```
aws cloudformation delete-stack \
--stack-name myStack \
--retain-resources MyBucket
```
- Stack successfully reached **DELETE_COMPLETE** while preserving the bucket and its contents.

<img width="930" height="176" alt="Screenshot 2026-04-28 193419" src="https://github.com/user-attachments/assets/f8216b43-3296-4f63-9241-1a92999002be" />

---

##  **What I Learned**
- How CloudFormation handles resource dependencies and rollbacks  
- How to analyze stack events and userdata logs to find root causes  
- How WaitConditions interact with userdata scripts  
- How to detect and interpret drift  
- How to retain resources during stack deletion  
- How to troubleshoot DELETE_FAILED scenarios involving S3 buckets  

---

##  **Final Result**
I successfully diagnosed the failing CloudFormation stack, fixed the template, redeployed the infrastructure, detected drift after manual changes, and resolved a DELETE_FAILED state by retaining resources. This lab reinforced real‑world troubleshooting skills for Infrastructure as Code workflows.

---

