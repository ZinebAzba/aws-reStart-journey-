# Creating & Troubleshooting Amazon EC2 Instances  
---

##  Task 1 — Launching an EC2 Instance Using the AWS Management Console

This task involved launching an EC2 instance manually through the AWS Console.

### Step 1 — Choose Name and Tags
- **Name:** Bastion Host   


### Step 2 — Choose an AMI
- **AMI:** Amazon Linux 2  


### Step 3 — Choose an Instance Type
- **Instance type:** `t2.micro`

### Step 4 — Configure a Key Pair
- **Key pair name:** Proceeded without key pair (Not recommended).

### Step 5 — Configure the Network Settings
- Default VPC and subnet  
- Auto‑assign public IP enabled  
- Initial Security Group configured

**Inbound rule:**
| Type | Port | Source |
|------|------|---------|
| SSH | 22 | <	0.0.0.0/0> |

### Step 6 — Add Storage
- **8 GiB gp2** (default)

### Step 7 — Configure Advanced Details
- Default IAM role, user data, shutdown behavior, and monitoring

### Step 8 — Launch the EC2 Instance
- Instance launched successfully  
- Verified:
  - **Running**
  - **2/2 status checks passed**
  - Public IPv4 DNS assigned

---

##  Task 2 — Logging In to the Bastion Host

This task involved connecting to a bastion host to access private resources.

### Steps Completed
- Retrieved bastion host public IP  
- Connected via SSH or EC2 Instance Connect  
- Verified access to internal/private subnets
---

##  Task 3 — Launching an EC2 Instance Using the AWS CLI

This task required launching an EC2 instance programmatically using the AWS CLI.

### Step 1 — Retrieve the AMI to Use
#Set the Region
AZ=`curl -s http://169.254.169.254/latest/meta-data/placement/availability-zone`
export AWS_DEFAULT_REGION=${AZ::-1}
#Retrieve latest Linux AMI
AMI=$(aws ssm get-parameters --names /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2 --query 'Parameters[0].[Value]' --output text)
echo $AMI
### Step 2 — Retrieve the Subnet to Use
SUBNET=$(aws ec2 describe-subnets --filters 'Name=tag:Name,Values=Public Subnet' --query Subnets[].SubnetId --output text)
echo $SUBNET
### Step 3 — Retrieve the Security Group to Use
SG=$(aws ec2 describe-security-groups --filters Name=group-name,Values=WebSecurityGroup --query SecurityGroups[].GroupId --output text)
echo $SG
### Step 4 — Download a User Data Script
wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-100-RSJAWS-1-23732/171-lab-JAWS-create-ec2/s3/UserData.txt

cat UserData.txt
### Step 5 — Launch the Instance
INSTANCE=$(\
aws ec2 run-instances \
--image-id $AMI \
--subnet-id $SUBNET \
--security-group-ids $SG \
--user-data file:///home/ec2-user/UserData.txt \
--instance-type t3.micro \
--tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=Web Server}]' \
--query 'Instances[*].InstanceId' \
--output text \
)
echo $INSTANCE
### Step 6 — Wait for the Instance to Be Ready
aws ec2 describe-instances --instance-ids $INSTANCE
aws ec2 describe-instances --instance-ids $INSTANCE --query 'Reservations[].Instances[].State.Name' --output text

### Step 7 — Test the Web Server
aws ec2 describe-instances --instance-ids $INSTANCE --query Reservations[].Instances[].PublicDnsName --output text
- Retrieved Public IPv4 DNS  
- Opened in browser to verify user data execution

---

#  Challenge 1 — Fixing SSH Connectivity

### Problem Observed
EC2 Instance Connect failed to establish an SSH session.

### Diagnosis
Security Group did **not** allow inbound SSH on port 22.

### Fix Applied
Added inbound rule:

| Type | Port | Source |
|------|------|---------|
| SSH | 22 | 0.0.0.0/0 |

### Result
EC2 Instance Connect worked successfully.

### Instructor Answers
- **Problem:** Missing inbound SSH rule on port 22  
- **Fix:** Added inbound SSH rule allowing port 22 from 0.0.0.0/0  

---

#  Challenge 2 — Fixing the Web Server

### Problem Observed
Accessing the Public IPv4 DNS returned:
- `ERR_CONNECTION_TIMED_OUT`
- `ERR_CONNECTION_REFUSED`

### Diagnosis
Apache web server was:
- Not installed  
**or**
- Installed but **not running**

### Fix Applied
Installed and started Apache:


Ensured Security Group allowed HTTP:

| Type | Port | Source |
|------|------|---------|
| HTTP | 80 | 0.0.0.0/0 |

### Result
Browser displayed the **Apache Test Page**.

### Conclusion
- **Problem:** Apache web server was not running (or not installed)  
- **Fix:** Installed and started httpd, enabled it, and opened port 80  

---

#  Final Outcome

By the end of the lab, I successfully:

- Launched EC2 instances using both the Console and AWS CLI  
- Logged into a bastion host  
- Diagnosed and fixed SSH connectivity issues  
- Diagnosed and fixed a misconfigured web server  
- Verified full functionality of the instance  

This demonstrates practical skills in EC2 provisioning, networking, security groups, user data automation, and troubleshooting.

---

