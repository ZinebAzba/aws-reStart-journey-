
---

# **Working with Amazon EBS – Lab Summary**

## **1. EC2 Instance Preparation**
- Launched the EC2 instance provided in the lab environment  
- Connected via SSH using the Vocareum Workbench terminal  
- Verified existing storage using:
```bash
lsblk
df -h
```

---

## **2. Attaching the New EBS Volume**
- Identified the new EBS volume (`/dev/nvme1n1`) attached to the instance  
- Confirmed visibility using:
```bash
lsblk
```

---

## **3. Formatting the Volume**
- Created an **ext4 filesystem** on the new device:
```bash
sudo mkfs -t ext4 /dev/nvme1n1
```

---

## **4. Mounting the Volume**
- Created the mount directory:
```bash
sudo mkdir -p /mnt/data-store
```
- Mounted the new EBS volume:
```bash
sudo mount /dev/nvme1n1 /mnt/data-store
```

---

## **5. Verification**
- Confirmed the mount using:
```bash
df -h
```
- Expected and achieved output included the new line:
```
/dev/nvme1n1    974M   24K  907M   1% /mnt/data-store
```
- Additional validation:
```bash
mount | grep data-store
```

---

## **6. Screenshot Evidence**

<img width="621" height="212" alt="Screenshot 2026-03-22 164847" src="https://github.com/user-attachments/assets/b72ee7eb-e16f-4492-b283-a148d9ff7d69" />

---

## **✔ Outcome**
This lab demonstrates successful completion of the core EBS workflow:

- Detecting and attaching a new EBS volume  
- Formatting the device with a Linux filesystem  
- Mounting the volume to a persistent directory  
- Verifying correct configuration using system tools  

This ensures the EC2 instance can use the new storage for application data, logs, or persistent workloads.

---
