
________________________________________
# Daily Sales Analysis Automation — AWS Lambda, MySQL, SNS & EventBridge

This project implements a fully automated **Daily Sales Analysis Report** pipeline using AWS serverless and managed services. The workflow retrieves sales data from a MySQL database hosted on EC2, processes it with AWS Lambda, and sends a formatted email report through Amazon SNS. The entire process is scheduled using Amazon EventBridge.

---

##  Architecture Overview

The system consists of four main components:

1. **AWS Lambda**  
   - Python 3.12 function  
   - Connects to MySQL using PyMySQL  
   - Executes SQL queries and formats the report  
   - Publishes the report to SNS

2. **MySQL Database on EC2**  
   - Stores Café sales data  
   - Accessible only from Lambda via VPC + Security Groups  
   - Port 3306 restricted to Lambda’s security group

3. **Amazon SNS**  
   - Sends the Daily Sales Analysis Report via email  
   - Subscribers receive automated notifications

4. **Amazon EventBridge (CloudWatch Events)**  
   - Triggers the Lambda function on a schedule  
   - Production cron expression (UTC):  
     ```
     cron(0 20 ? * MON-SAT *)
     ```


---

# **Implementation Checklist**

## **1. Lambda Setup**
- Created Lambda function with correct handler:  
  `salesAnalysisReportDataExtractor.lambda_handler`
- Added **PyMySQL layer** for database connectivity
- Configured **environment variables** for DB credentials

---

## **2. VPC & Networking**
- Attached Lambda to **Cafe VPC** and correct private subnet
- Identified Lambda security group:  
  `sg-0a950aca28e205c35`
- Updated DB EC2 security group to allow inbound:  
  **MySQL/Aurora (3306)** FROM `sg-0a950aca28e205c35`

---

## **3. Database Validation**
- Connected to **CafeInstance** via SSH
- Logged into MySQL and verified database name using:
  ```
  SHOW DATABASES;
  ```
- Updated Lambda test event with correct DB name

---

## **4. SNS Integration**
- Created **SNS topic** for report delivery
- Subscribed email endpoint
- Updated Lambda to publish formatted report to SNS

---

## **5. EventBridge Scheduling**
- Created scheduled rule to run **Monday–Saturday at 20:00 UTC**
- Final production cron expression:  
  `cron(0 20 ? * MON-SAT *)`

---

## **6. End‑to‑End Validation**
- Triggered Lambda manually to confirm DB connectivity
- Waited for scheduled EventBridge trigger
- Successfully received **Daily Sales Analysis Report** email

---

## **✔ Outcome**
This project demonstrates a complete, production‑ready serverless automation workflow:

- Secure **VPC‑based Lambda → MySQL** connectivity  
- Automated **data extraction and reporting**  
- Email notifications via **SNS**  
- Reliable scheduling with **EventBridge**  
- Clean, maintainable, cloud‑native architecture  

---

---

#  **Architecture Diagram**  


               ┌──────────────────────────────┐
               │        EventBridge            │
               │  cron(0 20 ? * MON-SAT *)     │
               └──────────────┬───────────────┘
                              │ triggers
                              ▼
                 ┌────────────────────────┐
                 │        Lambda          │
                 │ salesAnalysisReport... │
                 └─────────────┬──────────┘
                               │ queries
                               ▼
                 ┌────────────────────────┐
                 │     MySQL on EC2       │
                 │   CafeInstance (3306)  │
                 └─────────────┬──────────┘
                               │ report
                               ▼
                 ┌────────────────────────┐
                 │          SNS           │
                 │ Daily Sales Report     │
                 └─────────────┬──────────┘
                               │ email
                               ▼
                     ┌───────────────────┐
                     │     Subscriber     │
                     │     (Your Email)   │
                     └────────────────────┘

---
