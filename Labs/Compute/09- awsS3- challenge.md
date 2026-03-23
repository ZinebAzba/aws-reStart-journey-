
---

#  **S3 Challenge Summary — (AWS CLI)**  
This challenge demonstrates the full workflow of creating and managing an Amazon S3 bucket using the AWS CLI. The objective was to create a bucket, upload an object, test access behavior, modify permissions, and verify public accessibility.

---

## **1. Create an S3 Bucket**  
A new S3 bucket was created using the AWS CLI.  
The first attempt failed due to a globally‑unique name conflict, and the second attempt succeeded with a unique bucket name.

**Outcome:** Bucket successfully created.

---

## **2. Upload an Object to the Bucket**  
A test file was created locally and uploaded to the bucket using `aws s3 cp`.

**Outcome:** Object successfully uploaded.

---

## **3. Attempt to Access the Object (Expected Failure)**  
Accessing the object via browser resulted in an **AccessDenied** XML error.  
This is the expected behavior before enabling public access.

**Outcome:** Access correctly denied.

---

## **4. Modify Permissions to Allow Public Access**  
Two permission layers had to be adjusted:

### **a. Block Public Access (Bucket Settings)**  
This setting was disabled to allow public ACLs.

### **b. Object Ownership**  
The bucket defaulted to **Bucket owner enforced**, which disables ACLs entirely.  
It was changed to **ACLs enabled (Object writer)** to allow `public-read`.

After these changes, the ACL command succeeded.

**Outcome:** Object ACL updated to `public-read`.

---

## **5. Access the Object Again (Expected Success)**  
Re‑opening the object URL in the browser displayed the file contents:

```
Hello S3 challenge
```

**Outcome:** Public access confirmed.

---

## **6. List Bucket Contents Using AWS CLI**  
The bucket contents were listed successfully, showing the uploaded object.

**Outcome:** Bucket listing verified.

---

#  **Final Result**  
All required steps were completed:

✔ Bucket created  
✔ Object uploaded  
✔ Access denied (expected)  
✔ Public access enabled  
✔ Object accessible in browser  
✔ Bucket contents listed via CLI  

Your workflow is correct, complete, and ready for submission.

---

#  ** Code & Screenshot Placeholder Section**  


# --- AWS CLI Commands Used ---

# 1. Create bucket
[ec2-user@ip-10-200-0-193 ~]$ aws configure
AWS Access Key ID [None]: xxxxxxxxxxxxxxxx
AWS Secret Access Key [None]: xxxxxxxxxxxxxxxx
Default region name [None]: us-west-2
Default output format [None]: json
[ec2-user@ip-10-200-0-193 ~]$ ^C
[ec2-user@ip-10-200-0-193 ~]$ aws s3api create-bucket \
>   --bucket my-s3-challenge \
>   --region us-west-2 \
>   --create-bucket-configuration LocationConstraint=us-west-2

An error occurred (BucketAlreadyExists) when calling the CreateBucket operation: The requested bucket name is not available. The bucket namespace is shared by all users of the system. Please select a different name and try again.
[ec2-user@ip-10-200-0-193 ~]$ aws s3api create-bucket \
>   --bucket my-s3-challenge-230326 \
>   --region us-west-2 \
>   --create-bucket-configuration LocationConstraint=us-west-2
{
    "Location": "http://my-s3-challenge-230326.s3.amazonaws.com/"
}

# 2. Upload object
[ec2-user@ip-10-200-0-193 ~]$ aws s3 cp test1.txt s3://my-s3-challenge-230326/

The user-provided path test1.txt does not exist.
[ec2-user@ip-10-200-0-193 ~]$ echo "Hello S3 challenge" > test1.txt
[ec2-user@ip-10-200-0-193 ~]$ aws s3 cp test1.txt s3://my-s3-challenge-230326/
upload: ./test1.txt to s3://my-s3-challenge-230326/test1.txt      
[ec2-user@ip-10-200-0-193 ~]$ ls -l
total 4
drwxr-xr-x 4 ec2-user ec2-user 78 Mar 23 16:05 sysops-activity-files
-rw-rw-r-- 1 ec2-user ec2-user 19 Mar 23 16:23 test1.txt

# 3. Browser AccessDenied screenshot
[ec2-user@ip-10-200-0-193 ~]$ aws s3api put-object-acl \
>   --bucket my-s3-challenge-230326 \
>   --key test1.txt \
>   --acl public-read

An error occurred (AccessDenied) when calling the PutObjectAcl operation: User: arn:aws:iam::817587450548:user/awsstudent is not authorized to perform: s3:PutObjectAcl on resource: "arn:aws:s3:::my-s3-challenge-230326/test1.txt" because public ACLs are prevented by the BlockPublicAcls setting in S3 Block Public Access.

# 4. Disable Block Public Access (console screenshot)
<img width="666" height="657" alt="Screenshot 2026-03-23 173849" src="https://github.com/user-attachments/assets/22ebdfd8-d216-4dac-85e9-469292b3ecd2" />


# 5. Enable ACLs (Object Ownership screenshot)
<img width="640" height="479" alt="image" src="https://github.com/user-attachments/assets/b70521de-f7c6-4dd1-ab84-60a4bd7584ff" />


# 6. Apply public-read ACL
[ec2-user@ip-10-200-0-193 ~]$ aws s3api put-object-acl \
>   --bucket my-s3-challenge-230326 \
>   --key test1.txt \
>   --acl public-read

# 7. Browser successful access screenshot
<img width="206" height="74" alt="Screenshot 2026-03-23 174303" src="https://github.com/user-attachments/assets/498192a2-9a10-43ae-a8a4-24107e982150" />


# 8. List bucket contents
<img width="643" height="40" alt="image" src="https://github.com/user-attachments/assets/5d5d610e-806e-4c63-bb3c-ae80837fdc5a" />



```

---
