
---

#  AWS Lab — Migrate to Amazon RDS

##  Overview
This lab demonstrates how to migrate a MySQL database from an EC2 instance to an Amazon RDS MariaDB instance.  
It covers networking, security groups, database creation, data import, and connection monitoring.

---

##  Architecture Summary
- **EC2 Instance:** `CafeInstance` in a public subnet  
- **RDS Instance:** `CafeDBInstance` (MariaDB) in private subnets  
- **Security Groups:** Allow EC2 → RDS on port **3306**  
- **Database:** `cafe_db`  
- **Imported Data:** `product` table from `cafedb-backup.sql`

---

##  Key Steps Performed

### 1. Create and Configure RDS Instance
- Created a MariaDB RDS instance in private subnets  
- Attached a custom DB subnet group  
- Assigned a security group allowing inbound MySQL (3306) from the EC2 security group  

### 2. Connect to EC2 Instance
- Connected via SSH  
- Verified MySQL client availability  

### 3. Create the Database
```sql
create database cafe_db;
use cafe_db;
```

### 4. Import the SQL Backup
```bash
mysql --user=root --password='Re:Start!9' \
--host=<RDS endpoint> cafe_db < cafedb-backup.sql
```

### 5. Open Interactive SQL Session
```bash
mysql --user=root --password='Re:Start!9' \
--host=<RDS endpoint> cafe_db
```

### 6. Validate Data
```sql
select * from product;
```

### 7. Monitor RDS Connections
- Viewed the **DatabaseConnections** graph in the RDS console  
- Observed **1 active connection** during the interactive session  
- Observed **0 connections** after running `exit`  

---

##  Screenshots
This section documents the key steps of the migration workflow, including connectivity, monitoring, and configuration.

---

### 1️⃣ EC2 → RDS Interactive MySQL Session
**Description:**  
Shows the EC2 instance successfully connecting to the RDS MariaDB instance and querying the `product` table.

**Include:**  
- Connection command  
- MySQL prompt: `MariaDB [cafe_db]>`  
- Output of:  
  ```sql
  select * from product;
  ```
<img width="943" height="486" alt="Screenshot 2026-03-25 182125" src="https://github.com/user-attachments/assets/df9c5202-093e-43ed-bee0-a913abb43ed6" />

---

### 2️⃣ RDS Monitoring — DatabaseConnections (1 Active Connection)
**Description:**  
Displays the RDS monitoring graph showing one active connection created by the interactive MySQL session.


<img width="406" height="575" alt="image" src="https://github.com/user-attachments/assets/12f6c750-7ef3-4f84-bf2b-4606f09f8ce5" />


---

### 3️⃣ RDS Monitoring — DatabaseConnections (0 Connections After Exit)
**Description:**  
Shows the RDS graph after closing the MySQL session.


<img width="397" height="420" alt="image" src="https://github.com/user-attachments/assets/b002bc59-46a9-4e59-824d-09e84eb373f2" />

---

### 4️⃣ RDS Instance Details
**Description:**  
Captures the configuration of the RDS instance.

**to be Included:**  
- DB identifier  
- Engine (MariaDB)  
- Endpoint  
- Availability zone  
- Subnet group  
- Security group  

---

### 5️⃣ Security Group Rule (EC2 → RDS on Port 3306)
**Description:**  
Shows the inbound rule that allows the EC2 instance to connect to the RDS instance.

** to be Included:**  
- RDS security group inbound rule  
- Type: **MySQL/Aurora (3306)**  
- Source: **EC2 security group ID**  

---

### 6️⃣ RDS Subnet Group
**Description:**  
Documents the subnet group used by the RDS instance.

** to be Included:**  
- Subnet group name  
- Two private subnets  
- Availability zones  

---

##  Key Learnings
- Deploying an RDS instance in private subnets  
- Configuring security groups for controlled access  
- Connecting EC2 → RDS using MySQL  
- Importing data into RDS  
- Monitoring active database connections  
- Validating a successful migration with SQL queries  

---


