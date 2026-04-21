

---

# **Lab Summary — Monitor an EC2 Instance with CloudWatch & SNS**

In this lab I practiced how to monitor an EC2 instance using Amazon CloudWatch and configured automated notifications through Amazon SNS. 
I created an SNS topic, built a CloudWatch alarm based on CPUUtilization, stress‑tested the EC2 instance to trigger the alarm, and built a dashboard to visualize the metric.

---

## **1. Configure Amazon SNS**
Created an SNS topic (`MyCwAlarm`) and subscribed an email endpoint. Confirmed the subscription to enable alarm notifications.

**SNS topic & confirmed subscription**
<img width="1332" height="570" alt="Screenshot 2026-04-21 212314" src="https://github.com/user-attachments/assets/62c8a8b2-c1e7-43cf-91b2-587cd3b08d38" />

<img width="1352" height="434" alt="Screenshot 2026-04-21 212436" src="https://github.com/user-attachments/assets/1aa5b804-e0c9-4350-9ad4-6f47045b74e3" />


---

## **2. Create a CloudWatch Alarm**
Configured a CloudWatch alarm (`LabCPUUtilizationAlarm`) to monitor the Stress Test EC2 instance.  
The alarm triggers when **CPUUtilization > 60%** for a 1‑minute period and sends a notification to the SNS topic.

**Alarm configuration**
<img width="1353" height="604" alt="Screenshot 2026-04-21 215126" src="https://github.com/user-attachments/assets/8a3cecfe-612c-4fd4-9bc3-e784569d0d69" />

---

## **3. Test the Alarm**
Connected to the Stress Test EC2 instance and ran a CPU stress command:

```
sudo stress --cpu 10 -v --timeout 400s
```

This pushed CPU usage to 100%, causing the alarm to enter the **In alarm** state.  
Verified the spike in CloudWatch and received the SNS email notification.

**top command showing CPU load**  
<img width="764" height="859" alt="Screenshot 2026-04-21 220010" src="https://github.com/user-attachments/assets/9f7b5449-05a2-474e-9d64-1055148e48f5" />

**CloudWatch alarm in “In alarm” state** 
<img width="1407" height="842" alt="Screenshot_21-4-2026_215323_us-west-2 console aws amazon com" src="https://github.com/user-attachments/assets/8fe00710-1df8-4131-8349-a1d5d7eb992d" />

**SNS email notification**
<img width="737" height="497" alt="image" src="https://github.com/user-attachments/assets/7c571fda-d1e2-4bb3-b835-db5f3dd27088" />


---

## **4. Create a CloudWatch Dashboard**
Built a dashboard (`LabEC2Dashboard`) with a line graph widget showing the **CPUUtilization** metric for the Stress Test instance, providing quick visibility into performance.



##### CloudWatch dashboard widget
<img width="1357" height="540" alt="Screenshot 2026-04-21 215838" src="https://github.com/user-attachments/assets/efa51b7c-d399-4364-8031-1ea1e7eba7e7" />


## **Result**
By the end of the lab, I successfully implemented a monitoring and alerting workflow using EC2, CloudWatch metrics, CloudWatch alarms, SNS notifications, and a custom dashboard—providing a complete view of instance performance and automated alerting.

---

