
---

#  **Lab Summary — Build and Access an RDS Server**

This lab focused on designing, deploying, and interacting with a managed relational database using **Amazon RDS (MySQL)** and connecting to it securely from an **EC2 instance** within the same VPC. The goal was to understand how AWS services integrate, how network permissions affect connectivity, and how to perform basic SQL operations on a cloud‑hosted database.

---

##  **1. RDS Deployment**

The lab began by creating a **MySQL 5.7 RDS instance** inside a private subnet.  
Key configuration steps included:

- Selecting the correct engine version (MySQL 5.7)  
- Placing the database in the same VPC as the EC2 instance  
- Assigning a dedicated security group that allows inbound MySQL traffic (port 3306)  
- Ensuring the database is **not publicly accessible**  
- Copying the RDS endpoint for later use  

This step introduced the concept of **managed databases**, where AWS handles backups, patching, monitoring, and availability.

---

##  **2. EC2 Setup and Connectivity**

An **Amazon Linux 2 EC2 instance** was launched in the same VPC and subnet.  
The EC2 instance was configured with a security group that:

- Allows outbound traffic to the RDS security group  
- Enables private, secure communication inside the VPC  

A MySQL‑compatible client (MariaDB) was installed on the EC2 instance to allow command‑line access to the RDS database.

Once the client was installed, the EC2 instance successfully connected to the RDS endpoint using:

```
mysql -h <endpoint> -u admin -p
```

This demonstrated how **security groups, VPC networking, and DNS resolution** all work together to enable private service‑to‑service communication.

---

##  **3. SQL Operations**

Inside the RDS MySQL environment, several SQL tasks were completed:

### **a. Creating the `RESTART` table**  
Columns included student ID, name, city, and graduation date.
USE labdb;
CREATE TABLE RESTART (
    student_id INT,
    student_name VARCHAR(50),
    restart_city VARCHAR(50),
    graduation_date DATETIME
);

### **b. Inserting 10 sample rows**  
This reinforced understanding of SQL `INSERT` statements and data types.

INSERT INTO RESTART VALUES
(1, 'Amina', 'Zurich', '2024-01-10 10:00:00'),
(2, 'Youssef', 'Geneva', '2024-02-15 11:30:00'),
(3, 'Sara', 'Basel', '2024-03-20 09:45:00'),
(4, 'Omar', 'Bern', '2024-04-05 14:00:00'),
(5, 'Lina', 'Lausanne', '2024-05-12 16:20:00'),
(6, 'Adam', 'Lugano', '2024-06-01 13:10:00'),
(7, 'Maya', 'Winterthur', '2024-07-18 08:50:00'),
(8, 'Karim', 'Lucerne', '2024-08-22 12:40:00'),
(9, 'Nadia', 'St. Gallen', '2024-09-30 15:15:00'),
(10, 'Hassan', 'Biel', '2024-10-25 17:05:00');

### **c. Selecting all rows**  
A simple `SELECT *` query validated that the data was stored correctly.

SELECT * FROM RESTART;

### **d. Creating the `CLOUD_PRACTITIONER` table**  
This table stored student IDs and certification dates.

CREATE TABLE CLOUD_PRACTITIONER (
    student_id INT,
    certification_date DATETIME
);

### **e. Inserting 5 sample rows**  
Demonstrated inserting data into a second table.

INSERT INTO CLOUD_PRACTITIONER VALUES
(1, '2024-11-01 10:00:00'),
(3, '2024-11-05 09:30:00'),
(5, '2024-11-10 14:15:00'),
(7, '2024-11-12 11:45:00'),
(10, '2024-11-20 16:00:00');


### **f. Performing an INNER JOIN**  
A join between the two tables displayed:

- student ID  
- student name  
- certification date  
This step reinforced relational database concepts and how tables relate through keys.

SELECT 
    r.student_id,
    r.student_name,
    c.certification_date
FROM RESTART r
INNER JOIN CLOUD_PRACTITIONER c
    ON r.student_id = c.student_id;


<img width="581" height="266" alt="Screenshot 2026-04-13 174340" src="https://github.com/user-attachments/assets/c39e3463-d7dd-4f6f-bbd5-c017131a4056" />
<img width="480" height="327" alt="Screenshot 2026-04-13 174423" src="https://github.com/user-attachments/assets/53096c43-bc35-4292-90ac-e8d2c0110ac6" />
<img width="446" height="318" alt="Screenshot 2026-04-13 174435" src="https://github.com/user-attachments/assets/21211d5a-9a2c-4173-b295-7d8dfebb15d4" />

---

#  **Troubleshootinge**

## **1. What Went Wrong**

Several issues combined to make the challenge more difficult than necessary. Here is the full list, including the missing step:

### **a. The EC2 Instance Did Not Have the Correct Security Group Attached Initially**  
The EC2 instance was launched with the wrong security group.  
Because of this, even after creating the correct RDS security group, the EC2 instance was not allowed to communicate with RDS.

This required manually attaching the correct security group to EC2 later — something that should have been done before creating RDS.

### **b. The RDS Security Group Was Not Created Before the RDS Instance**  
The RDS instance was created **before** creating the correct security group.  
This caused RDS to be associated with the **default security group**, which does *not* allow EC2 → RDS traffic on port 3306.

This forced me to delete and recreate the RDS instance.

### **c. Wrong MySQL Version (MySQL 8 Instead of 5.7)**  
The first RDS instance used **MySQL 8**, which uses the `caching_sha2_password` plugin.  
The EC2 instance could not install a MySQL 8 client because AWS Academy blocks MySQL 8 packages due to GPG key restrictions.

This caused authentication plugin errors.

### **d. MariaDB Client Was Blocked by the MySQL 8 Repo**  
When trying to install the MariaDB client (which is compatible with MySQL 5.7), the MySQL 8 repository marked it as “obsolete,” forcing yum to install MySQL 8 packages instead — which failed.

### **e. Multiple Attempts to Install MySQL 8**  
Because the MySQL 8 repo was enabled, every installation attempt kept pulling MySQL 8 packages, which always failed due to GPG key mismatch.

---

## **2. The Correct Happy Path (Smooth, No Errors)**

Here is the clean, correct sequence to complete the challenge without any obstacles:

---

###  **Step 1 — Create the EC2 Security Group FIRST**  
Before launching EC2 or RDS:

1. Create a security group (e.g., `web-security-group`)
2. Add inbound rule:

```
Type: MySQL/Aurora
Port: 3306
Source: web-security-group (self-reference)
```

This allows EC2 → RDS communication inside the VPC.

---

###  **Step 2 — Launch the EC2 Instance Using This Security Group**  
- Amazon Linux 2  
- Same VPC and subnet where RDS will be created  
- Attach the **web-security-group** at launch time  

This ensures EC2 is ready to communicate with RDS from the start.

---

###  **Step 3 — Create the RDS Instance (MySQL 5.7)**  
- Engine: **MySQL 5.7**  
- Public access: **No**  
- VPC: same as EC2  
- Security group: **web-security-group**  
- Copy the endpoint carefully

MySQL 5.7 uses `mysql_native_password`, which works with the MariaDB client.

---

###  **Step 4 — Install the Correct MySQL Client on EC2**  
AWS Academy blocks MySQL 8, so install MariaDB:

```bash
sudo yum install -y mariadb
```

This client is fully compatible with MySQL 5.7.

---

###  **Step 5 — Connect to RDS Using the Correct Endpoint**

```bash
mysql -h <correct-endpoint> -u admin -p
```

When successful, you will see:

```
Welcome to the MariaDB monitor...
MySQL [(none)]>
```

---

###  **Step 6 — Run All SQL Commands for the Challenge**  
Create tables, insert rows, select data, and perform the join.

---

## **3. Final Outcome**

After:

- attaching the correct security group to EC2  
- creating the RDS security group before RDS  
- recreating the RDS with MySQL 5.7  
- installing the MariaDB client  
- using the correct endpoint  

…the EC2 instance successfully connected to the RDS database, and all SQL tasks were completed without further issues.

---


