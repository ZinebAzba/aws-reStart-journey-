

#  Managing Storage on AWS — Full Lab Summary  

---

## 1️⃣ EC2 + EBS: Understanding and Managing Block Storage

###  What I Did
- Launched an EC2 instance  
- Identified the attached EBS volume  
- Created snapshots of that volume  

###  Why This Matters
- EC2 = compute  
- EBS = persistent block storage  
- Snapshots = point‑in‑time backups stored internally in S3  
- Snapshots enable:
  - Disaster recovery  
  - Environment cloning  
  - Rollback to a known good state  

###  Key Learning
You **never** snapshot an instance (`i-xxxx`).  
You snapshot a **volume** (`vol-xxxx`).  
This distinction is foundational in AWS.

---

## 2️⃣ Automating Snapshots with Python + Boto3

###  What I Did
Using Boto3, you automated:
- Listing all volumes  
- Creating snapshots programmatically  
- Retrieving existing snapshots  
- Implementing retention logic (keep only the newest N snapshots)  

###  Why This Matters
Manual snapshots don’t scale.  
Automation provides:
- Consistency  
- Repeatability  
- Cost control (delete old snapshots)  
- A real‑world backup lifecycle policy  

###  Key Learning
You implemented a classic cloud pattern:  
**“Keep the latest N backups and delete the rest.”**  
This is exactly how production systems manage snapshot retention.

---

## 3️⃣ S3 Versioning + Sync: Managing Object Storage

###  What I Did
- Enabled versioning on an S3 bucket  
- Synced a local folder to S3  
- Deleted a file locally and mirrored the deletion using `--delete`  
- Recovered the deleted file using S3 versioning  
- Synced again to restore the file to the bucket  

###  Why This Matters
S3 is an **object store**, not a file system.  
But `aws s3 sync` lets you treat it like a versioned repository.

Versioning turns S3 into a **time machine**:
- Every overwrite = new version  
- Every delete = delete marker  
- Nothing is lost unless you delete versions explicitly  

###  Key Learning
S3 has **no “restore previous version” button**.  
To recover a file, you must:
1. List versions  
2. Download the version you want  
3. Re‑upload it  

This mirrors real-world recovery workflows.

---

## 4️ How Everything Fits Together

This lab is a unified story about **data durability and lifecycle management** in AWS.

| Component | Purpose |
|----------|---------|
| **EBS Snapshots** | Protect block storage (disks), system-level backups |
| **S3 Versioning** | Protect object storage (files), file-level recovery |
| **Automation (Boto3 + CLI)** | Scalability, consistency, cost control |
| **Syncing** | Align local and cloud state, including deletions |
| **Version Recovery** | Demonstrates AWS’s built-in safety net |

---

##  Final Takeaway

By completing this lab, you demonstrated the ability to:
- Understand EC2/EBS relationships  
- Create and manage snapshots  
- Automate snapshot retention with Python  
- Use S3 as a versioned storage system  
- Sync local and cloud data  
- Recover deleted files using versioning  
- Apply real-world backup and restore workflows  

This is practical, hands‑on cloud engineering — exactly what teams expect in production.

---

#  Screenshot Checklist 

---

## 1️⃣ EC2 + EBS Snapshot Work

### **Screenshot A — EC2 Instance Details**
Show:
- Running EC2 instance  
- Attached EBS volume (`vol-xxxx`)  
<img width="940" height="813" alt="image" src="https://github.com/user-attachments/assets/e36afb34-cef3-472a-92e9-2269dbad2828" />

**What:**   the correct volumeidentified.

---

### **Screenshot B — Snapshot Created**
From **EC2 → Snapshots**:
- Snapshot visible  
- Status: *completed*  
- Volume ID matches the instance  
<img width="940" height="154" alt="image" src="https://github.com/user-attachments/assets/03fd3ac2-3998-475a-9195-a0ec3137f96f" />

**What:**  snapshot creation confirmed

---

### **Screenshot C — Python Script Execution**
Terminal showing:
- Running `python snapshooter_v2.py`  
- Output (e.g., deleting old snapshots or no errors)  

<img width="833" height="95" alt="image" src="https://github.com/user-attachments/assets/3fd3be9a-c9c4-4cbc-a50c-a5105b237151" />

**What:**  Shows automation and retention logic.

---

## 2️⃣ S3 Versioning + Sync Work

### **Screenshot D — Versioning Enabled**
S3 → Bucket → Properties → Versioning  

<img width="940" height="302" alt="image" src="https://github.com/user-attachments/assets/c18fb49f-4068-4e18-b8ec-986509b39e40" />

**What:** Required for recovery.

---

### **Screenshot E — First Sync**
Terminal:
```
aws s3 sync files s3://YOUR-BUCKET/files/
```
Show upload of 3 files.
<img width="940" height="105" alt="image" src="https://github.com/user-attachments/assets/e2d45eb7-20c0-4095-9cd2-26f011400774" />

**Why:**  initial sync confirmed

---

### **Screenshot F — S3 Bucket Listing**
Terminal:
```
aws s3 ls s3://YOUR-BUCKET/files/
```
<img width="551" height="68" alt="image" src="https://github.com/user-attachments/assets/115fafef-a0ee-4307-afeb-02a0679751f3" />

**What:** Shows files in S3.

---

### **Screenshot G — Local Deletion**
Terminal:
```
rm files/file1.txt
ls files
```
Show only 2 files left.

<img width="709" height="83" alt="image" src="https://github.com/user-attachments/assets/81a3b6c8-4eb5-417a-ad31-99d19d2eeedf" />

**Why:** Shows deletion step.

---

### **Screenshot H — Sync with --delete**
Terminal:
```
aws s3 sync files s3://YOUR-BUCKET/files/ --delete
```
Show:
```
delete: s3://YOUR-BUCKET/files/file1.txt
```
<img width="940" height="51" alt="image" src="https://github.com/user-attachments/assets/422a0b3b-dd85-4869-9370-c382727bf27f" />

**What:** Confirms S3 mirrored deletion.

---

### **Screenshot I — List Object Versions**
Terminal:
```
aws s3api list-object-versions --bucket YOUR-BUCKET --prefix files/file1.txt
```
Show:
- DeleteMarker  
- Versions  
- VersionId  
<img width="940" height="554" alt="image" src="https://github.com/user-attachments/assets/5d3aeca9-adb9-40d1-8342-7a26d71fd461" />

**What:** Evidence that versioning captured the deleted file.

---

### **Screenshot J — Restoring the File**
Terminal:
```
aws s3api get-object --bucket YOUR-BUCKET --key files/file1.txt --version-id VERSION-ID files/file1.txt
ls files
```
Show all 3 files restored.
<img width="940" height="82" alt="image" src="https://github.com/user-attachments/assets/3b9d6fa3-cdb4-4796-a363-8cfe2da85fd4" />

**What:** Proves recovery.

---

### **Screenshot K — Final Sync**
Terminal:
```
aws s3 sync files s3://YOUR-BUCKET/files/
aws s3 ls s3://YOUR-BUCKET/files/
```
Show all 3 files back in S3.
<img width="940" height="205" alt="image" src="https://github.com/user-attachments/assets/964447d7-8415-4fd8-99c5-61aa89df4201" />

**What:** Confirms full recovery lifecycle.

---
