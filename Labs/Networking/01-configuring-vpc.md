
---

# Lab Summary – Configuring an Amazon VPC

This lab focused on building a custom Virtual Private Cloud (VPC) and configuring the networking components required to support secure and controlled connectivity. The main objectives included:

- Creating a VPC with a defined CIDR block  
- Building public and private subnets  
- Configuring an Internet Gateway and NAT Gateway  
- Setting up route tables and subnet associations  
- Launching EC2 instances in public and private subnets  
- Testing connectivity between components  

By completing these tasks, I gained hands-on experience with core AWS networking concepts such as subnet isolation, routing, NAT behavior, and secure access patterns commonly used in production architectures.

# Challenges Encountered & Resolutions

### 1. NAT Gateway Not Appearing in the Private Route Table

**Issue**  
The NAT Gateway did not appear as an available target when editing the private route table.

**Root Cause**  
I was editing a route table belonging to the default VPC (`172.31.x.x`) instead of the route table associated with the custom Lab VPC.

**Resolution**  
I selected the correct private route table for the Lab VPC and associated it with the private subnet. Once the association was corrected, the NAT Gateway appeared immediately.

---

### 2. Ping Test Failing from the Private EC2 Instance

**Issue**  
The private EC2 instance could not ping external domains.

**Root Cause**  
NAT Gateways do not support ICMP traffic. Ping uses ICMP, which is not forwarded through a NAT Gateway.

**Resolution**  
I validated outbound connectivity using TCP-based commands such as `curl` and `yum update`. These succeeded, confirming that the NAT Gateway and routing configuration were functioning correctly.

---
