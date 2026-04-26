
---

# AWS CloudWatch Monitoring & Compliance Lab — Summary

This lab helped me understand how different AWS monitoring and compliance services work together to give visibility into the infrastructure. I went through CloudWatch Logs, Metrics, Alarms, EventBridge, SNS, and AWS Config. 
Below is my personal summary of what I did, what I learned, and how each task helped me build confidence with AWS monitoring tools.

---

## Task 1 — Installing & Configuring the CloudWatch Agent

In this task, I installed the CloudWatch Agent on my EC2 instance so I could collect system‑level metrics like memory, disk usage, and internal OS performance. Before this lab, I didn’t realize that EC2 doesn’t provide memory metrics by default — the agent is required for that.

I stored the agent configuration in Parameter Store, started the agent, and then verified that CloudWatch was receiving the metrics.

### What I learned
- EC2 native metrics are limited; the CloudWatch Agent unlocks deeper OS visibility.

- Parameter Store can be used to centrally store agent configuration.
- Once the agent runs, CloudWatch automatically starts receiving new metric namespaces.


[CloudWatch Agent Install](#)

  <img width="1915" height="906" alt="Screenshot 2026-04-26 145519" src="https://github.com/user-attachments/assets/4f19a33e-2c93-4f8d-a67c-ddefdbd72e1d" />
  
[Parameter Store Config](#)

 <img width="1912" height="584" alt="Screenshot 2026-04-26 145836" src="https://github.com/user-attachments/assets/23967cfc-1a6b-4ed4-bf3b-0c8f524694df" />
 
[Run Command Executed ](#)

<img width="1914" height="916" alt="Screenshot 2026-04-26 150130" src="https://github.com/user-attachments/assets/f5b68f23-698d-4a5f-9ad9-61d8bd13cb0b" />


## Task 2 — Creating Metric Filters & Alarms

Here I worked with CloudWatch Logs and learned how to turn log patterns into metrics. I created a metric filter that detects HTTP 404 errors from Apache logs. Then I created a CloudWatch Alarm that sends me an SNS email whenever the number of 404 errors crosses a threshold.

Triggering the alarm manually by visiting a non‑existent URL helped me understand how logs → metrics → alarms → notifications all connect.

### What I learned
- CloudWatch Logs can be transformed into metrics using filters.
- Alarms can notify me in real time when something unusual happens.
- SNS topics act as the communication layer for alerts.


[Log Group](#)
<img width="1911" height="915" alt="Screenshot 2026-04-26 151936" src="https://github.com/user-attachments/assets/3dc1f6d1-56df-4157-97b5-54da22ade8f2" />

[Metric Filter](#)
<img width="1905" height="874" alt="Screenshot 2026-04-26 153752" src="https://github.com/user-attachments/assets/bb7ea9ac-5fb8-4074-ae7d-4060c0e9ed46" />

[Alarm Config](#)
<img width="1905" height="917" alt="Screenshot 2026-04-26 155127" src="https://github.com/user-attachments/assets/fd9da98f-44ed-48ca-a8ea-d658505eabc0" />

[SNS Subscription](#)

<img width="1914" height="418" alt="Screenshot 2026-04-26 155715" src="https://github.com/user-attachments/assets/810f3c6f-03d1-4ead-a014-aa9aa6746bd8" />
<img width="643" height="281" alt="Screenshot 2026-04-26 155213" src="https://github.com/user-attachments/assets/4a5064fc-1a47-495b-8de7-5df099206404" />

[Alarm Triggered](#)

<img width="1914" height="916" alt="Screenshot 2026-04-26 160312" src="https://github.com/user-attachments/assets/8f4011a4-7c49-4be9-93db-4bdfee8a6e10" />


## Task 3 — Monitoring Instance Metrics with CloudWatch

This task was mostly exploration. I compared EC2 metrics with the CloudWatch Agent metrics I enabled earlier. At first, my graph was empty because I didn’t realize I had to select the actual metric (not just the category). Once I selected a metric like `disk_used_percent`, the graph populated.

### What I learned
- EC2 metrics = hypervisor-level  
- CloudWatch Agent metrics = OS-level  
- CloudWatch graphs only show data after selecting specific metrics


[EC2 Monitoring](#)
<img width="1910" height="899" alt="Screenshot 2026-04-26 161331" src="https://github.com/user-attachments/assets/adadce32-0886-4ea3-b9cb-a2414e538834" />

[CWAgent Metrics](#)

<img width="1915" height="539" alt="Screenshot 2026-04-26 173230" src="https://github.com/user-attachments/assets/fed6db6d-ce98-4bf6-b96b-031fbfa409f9" />


[Example:Disk Memory Metrics](#)

<img width="1916" height="585" alt="Screenshot 2026-04-26 173429" src="https://github.com/user-attachments/assets/a3b7ac3a-95d9-4892-9348-1dabee8642ee" />


## Task 4 — Creating Real-Time Notifications (EventBridge + SNS)

In this task, I created a rule that notifies me when an EC2 instance is stopped or terminated. The lab instructions referenced the old CloudWatch Events UI, but I learned that this is now done in **EventBridge**.

I created a rule that listens for EC2 state changes and sends them to my SNS topic. When I stopped my instance, I received an email with the event details in JSON format.

### What I learned
- EventBridge is the modern replacement for CloudWatch Events.
- Event patterns can detect infrastructure changes in real time.
- SNS integrates easily with EventBridge for notifications.


[EventBridge Rule](#)

<img width="1908" height="696" alt="Screenshot 2026-04-26 173800" src="https://github.com/user-attachments/assets/61b28baf-0e2d-4a7d-86c1-0228b99542e9" />
-
<img width="1911" height="702" alt="Screenshot 2026-04-26 173809" src="https://github.com/user-attachments/assets/02f7fa8f-e763-4761-b532-dc4cae7a2839" />


[SNS Topic](#)
<img width="1909" height="709" alt="image" src="https://github.com/user-attachments/assets/efa653a6-d57e-4209-a6c1-84474aeb709f" />


[Email Notification](#)

<img width="1003" height="629" alt="Screenshot 2026-04-26 164751" src="https://github.com/user-attachments/assets/c919e70d-b10e-4b3d-bf39-244c3b57e9fd" />


## Task 5 — Monitoring Infrastructure Compliance with AWS Config

This was my first time using AWS Config. I enabled it and added two managed rules:

1. **required-tags** — checks whether resources have a `project` tag  
2. **ec2-volume-inuse-check** — checks whether EBS volumes are attached to EC2 instances  

It took a few minutes for AWS Config to scan my resources. Once it finished, I could see which resources were compliant and which weren’t.

### What I learned
- AWS Config continuously evaluates resource compliance.
- Tagging rules help enforce governance and cost tracking.
- Unattached EBS volumes can be detected automatically.
- Compliance results take time to populate — patience is required.

[Required Tags Rule and Volume In Use Rule](#)


<img width="1910" height="748" alt="Screenshot 2026-04-26 171005" src="https://github.com/user-attachments/assets/3f936395-5b31-4aa7-bef5-07d6a1423abb" />

[Compliance Results](#)

<img width="1896" height="905" alt="Screenshot 2026-04-26 174544" src="https://github.com/user-attachments/assets/4b6e6608-f93c-43ff-85b4-a1fd4dd7eb43" />

---

## Final Reflection

This lab helped me understand how AWS monitoring tools connect together:

- CloudWatch Logs → detect issues  
- Metric Filters → convert logs into metrics  
- CloudWatch Alarms → notify on thresholds  
- EventBridge → detect infrastructure events  
- SNS → deliver notifications  
- AWS Config → enforce compliance  

By the end of the lab, I felt more confident navigating CloudWatch, EventBridge, and AWS Config. I also learned how important tagging and monitoring are for real-world cloud environments.



---




