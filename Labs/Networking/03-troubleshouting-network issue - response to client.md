
---

#  **Response to Jess (Customer)**

## **Issue Summary**
Instance A is unable to reach the internet, while Instance A works correctly.

## **Findings**
After reviewing the VPC, subnet, routing, and instance configuration, the root cause was identified:

- **Instance A does not have a Public IPv4 address assigned.**
- Even though the subnet is public and the route table correctly routes `0.0.0.0/0` to the Internet Gateway, an EC2 instance **must** have a Public IPv4 address to communicate with the internet.
- Instance B has a Public IPv4 address → internet works.  
- Instance A has *no* Public IPv4 address → no internet access.
<img width="1350" height="513" alt="Screenshot 2026-04-16 173712" src="https://github.com/user-attachments/assets/53d7df00-7c21-4af7-bc49-46290bc091c4" />

This behavior is expected in AWS:  
A public subnet alone does not provide internet access unless the instance has a public IP.

## **Recommendation**
To enable internet access, Instance B would need one of the following:

- A Public IPv4 address assigned at launch, or  
- An Elastic IP associated with the instance (if allowed), or  
- Placement behind a NAT Gateway (if intended to remain private)

In this lab environment, public IP assignment after launch is restricted, so Instance A is intentionally private.

## **Additional Note: VPC CIDR**
Using `12.0.0.0/16` as a VPC CIDR is **not recommended** because it is not part of the RFC 1918 private IP ranges.  
Best practice is to use:

- `10.0.0.0/8`  
- `172.16.0.0/12`  
- `192.168.0.0/16`

## **Conclusion**
The internet connectivity issue is caused by the absence of a Public IPv4 address on Instance A.  
No routing or subnet misconfiguration was found.

---

