

---

# **Lab: Creating a VPC Based on Customer Requirements**

##  Scenario
I received a support ticket from a customer, Paulo Santos, requesting help building a VPC with specific requirements:

> *“I need around 15,000 private IP addresses, the VPC must use a 192.x.x.x IPv4 range, and the public subnet must have at least 50 IP addresses. Can you confirm which 192.x.x.x ranges are private and help me build this VPC?”*

To respond accurately, I replicated the customer’s environment and validated each requirement.

---

# **1. Determining the Correct Private IPv4 Range**

The customer asked for a **192.x.x.x** range but wasn’t sure which ranges are private.  
RFC1918 defines the private IPv4 ranges:

- **10.0.0.0/8**  
- **172.16.0.0/12**  
- **192.168.0.0/16**

Since the customer specifically wants *192.x.x.x*, the **only valid private block** is:

✔ **192.168.0.0/16**

---

# **2. Choosing a CIDR Block for ~15,000 Private IPs**

The VPC must support **at least 15,000 private IP addresses**.

CIDR size reference:

| CIDR | Approx. IPs |
|------|-------------|
| /16  | 65,536      |
| /17  | 32,768      |
| /18  | 16,384      |
| /19  | 8,192       |

The smallest block that satisfies the requirement is:

✔ **/18 → 16,384 IPs**

Therefore, the VPC CIDR should be:

**VPC CIDR = 192.168.0.0/18**

---

# **3. Designing the Public Subnet**

The customer needs **at least 50 usable IPs** in the public subnet.

Subnet size reference:

| CIDR | Usable IPs |
|------|------------|
| /26  | 62         |
| /27  | 30         |

The smallest valid block is:

✔ **/26 → 62 usable IPs**

So the public subnet CIDR is:

**Public Subnet = 192.168.1.0/26**

---

# **4. Final Architecture (Replicated for the Customer)**

| Component | Configuration |
|----------|---------------|
| **VPC** | 192.168.0.0/18 |
| **Public Subnet** | 192.168.1.0/26 |
| **Internet Gateway** | Created and attached |
| **Route Table** | Includes 0.0.0.0/0 → IGW |
| **Subnet Association** | Public subnet associated with public route table |
| **IPv6** | Disabled |
| **AZ Preference** | None |
| **VPC Name** | First VPC |
| **Subnet Name** | Public subnet |

This architecture fully aligns with the customer’s requirements.

---

# 5. Customer‑Facing Explanation 

```
Hello Paulo,

Your VPC has been successfully created according to your requirements. Here is a summary of the configuration:

VPC Configuration
• IPv4 CIDR: 192.168.0.0/18
  This provides 16,384 private IP addresses, meeting your requirement for approximately 15,000 internal devices.

Public Subnet
• Subnet CIDR: 192.168.1.0/26
  This provides 62 usable IP addresses, satisfying your requirement for at least 50 public IPs.

Internet Access
• An Internet Gateway is attached to the VPC.
• The public subnet is associated with a route table that includes:
  - A local route for internal communication.
  - A default route (0.0.0.0/0) pointing to the Internet Gateway for outbound internet access.

Why These Ranges?
The 192.168.x.x range is part of the RFC1918 private IP space.
A /18 block is the smallest range that provides more than 15,000 IPs.
A /26 block is the smallest range that provides more than 50 usable IPs.

If you need help deploying instances or expanding this architecture later, I’d be happy to assist.

Best regards,
Cloud Support Engineer
```

---

# **✔Summary**
This lab demonstrated how to translate customer requirements into a correct VPC design by:

- Validating private IP ranges using RFC1918  
- Calculating CIDR sizes based on IP requirements  
- Designing a public subnet with sufficient usable IPs  
- Building a VPC that matches the customer’s architecture  
- Preparing a professional customer‑facing explanation  



