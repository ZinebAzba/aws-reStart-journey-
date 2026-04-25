

#  **Activity 1 — IAM Policy Retrieval Using AWS CLI**  
### *Summary*

In this activity, I used the AWS Command Line Interface (CLI) to explore and retrieve IAM policy information. The goal was to identify a customer‑managed IAM policy and download its JSON document using the appropriate AWS CLI commands.

### **Objectives**
- List IAM policies using the AWS CLI  
- Filter customer‑managed policies  
- Identify a policy’s ARN and default version  
- Retrieve a specific policy version  
- Save CLI output to a local file  
- View and verify the JSON policy document  

### **Steps Completed**

#### **1. List customer‑managed IAM policies**
I used the `--scope Local` flag to display only policies created within the account:
```bash
aws iam list-policies --scope Local
```

#### **2. Identify the policy ARN and version**
From the output, I located the policy named `lab_policy` and noted:
- Its **ARN**
- Its **DefaultVersionId** (v1)

#### **3. Retrieve the policy JSON**
Using the ARN and version ID, I downloaded the policy document and saved it to a file:
```bash
aws iam get-policy-version \
  --policy-arn arn:aws:iam::<account-id>:policy/lab_policy \
  --version-id v1 > lab_policy.json
```

#### **4. View the policy file**
I confirmed the file was created and displayed its content:
```bash
cat lab_policy.json
```

### **Conclusion**
This activity demonstrated how to work with IAM policies using the AWS CLI. I learned how to list policies, extract metadata, retrieve specific policy versions, and save JSON output to a file. These skills are essential for managing IAM resources programmatically and understanding how policy versioning works in AWS.

---

