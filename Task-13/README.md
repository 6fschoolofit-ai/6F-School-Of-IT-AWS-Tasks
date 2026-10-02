# AWS SSM Session Manager – Private EC2 Access Setup

This project demonstrates how to securely connect to a private EC2 instance using AWS Systems Manager (SSM) Session Manager without:

* SSH keys
* Bastion hosts
* Public IP addresses
* Inbound security group rules

The setup uses VPC Interface Endpoints, IAM Roles, and SSM Agent for secure private connectivity. 

---

# 📄 Documentation

Detailed implementation steps with screenshots are available inside the `docs/` folder.

Document Name:

* `13.SSM_Session_Manager_Setup_Guide.docx`

Repository Structure:

```text id="jvjlwm"
project-folder/
│
├── README.md
│
└── docs/
    └── 13.SSM_Session_Manager_Setup_Guide.docx
```

---

# 📌 Project Objective

The goal of this project is to:

* Create a private VPC environment
* Launch a private EC2 instance
* Configure VPC Interface Endpoints for SSM
* Attach IAM permissions to EC2
* Connect securely using Session Manager

This architecture provides fully private and secure EC2 administration without internet exposure. 

---

# 🏗️ Architecture Overview

```text id="jlwmf9"
Your Local Machine
        |
AWS Systems Manager
        |
------------------------------------------------
|               SSM-vpc                        |
|                                              |
|  Private Subnet                              |
|                                              |
|  ssm-private-ec2                             |
|      |                                       |
|      | HTTPS 443                             |
|      v                                       |
|  VPC Interface Endpoints                     |
|  - ssm                                       |
|  - ssmmessages                               |
|  - ec2messages                               |
|                                              |
------------------------------------------------
```

---

# ☁️ AWS Services Used

* Amazon VPC
* Amazon EC2
* AWS Systems Manager (SSM)
* VPC Interface Endpoints
* IAM Roles
* Security Groups

---

# 🌐 Environment Details

| Configuration | Value               |
| ------------- | ------------------- |
| Region        | ap-south-1 (Mumbai) |
| Platform      | Amazon Linux 2023   |
| VPC Name      | SSM-vpc             |
| VPC CIDR      | 10.0.0.0/16         |
| EC2 Instance  | ssm-private-ec2     |



---

# 🔐 Key Security Features

✅ No Public IP

✅ No SSH Port (22) Open

✅ No Bastion Host Required

✅ No SSH Keys Required

✅ Fully Private Connectivity

✅ IAM-Based Authentication

---

# 🚀 Step-by-Step Implementation

# Step 1 — Create VPC

Created a custom VPC using:

```text id="jlwmm4"
VPC and more
```

Configuration:

| Setting         | Value       |
| --------------- | ----------- |
| VPC Name        | SSM-vpc     |
| CIDR            | 10.0.0.0/16 |
| Public Subnets  | 0           |
| Private Subnets | 1           |
| NAT Gateway     | None        |
| DNS Hostnames   | Enabled     |
| DNS Resolution  | Enabled     |

Important:

DNS Resolution and DNS Hostnames must both be enabled for VPC endpoints to function correctly. 

---

# Step 2 — Create Endpoint Security Group

Security group created:

```text id="jlwmk1"
ssm-endpoint-sg
```

Inbound Rule:

| Protocol | Port | Source      |
| -------- | ---- | ----------- |
| HTTPS    | 443  | 10.0.0.0/16 |

Purpose:

Allows EC2 instances inside the VPC to communicate with SSM endpoints. 

---

# Step 3 — Create EC2 Security Group

Security group created:

```text id="jlwmv2"
ssm-ec2-sg
```

Important Design:

* No inbound rules required
* Only outbound rules configured

Outbound Rules:

| Protocol | Port | Purpose           |
| -------- | ---- | ----------------- |
| HTTPS    | 443  | SSM communication |
| DNS UDP  | 53   | DNS resolution    |
| DNS TCP  | 53   | DNS resolution    |

Important:

DNS ports are critical for endpoint hostname resolution. 

---

# Step 4 — Create VPC Interface Endpoints

Created 3 Interface Endpoints:

| Endpoint             | Service                              |
| -------------------- | ------------------------------------ |
| ssm-endpoint         | com.amazonaws.ap-south-1.ssm         |
| ssmmessages-endpoint | com.amazonaws.ap-south-1.ssmmessages |
| ec2messages-endpoint | com.amazonaws.ap-south-1.ec2messages |

Configuration:

| Setting        | Value           |
| -------------- | --------------- |
| Endpoint Type  | Interface       |
| Private DNS    | Enabled         |
| Subnet         | Private Subnet  |
| Security Group | ssm-endpoint-sg |

Purpose:

Allows private EC2 instances to communicate with SSM services without internet access. 

---

# Step 5 — Create IAM Role

Created IAM Role:

```text id="jlwmx8"
EC2-SSM-Role
```

Attached AWS Managed Policy:

```text id="9jlwm1"
AmazonSSMManagedInstanceCore
```

Purpose:

Provides minimum permissions required for:

* Session Manager
* Run Command
* Patch Manager



---

# Step 6 — Launch Private EC2 Instance

Launched private EC2 instance:

```text id="qjlwm0"
ssm-private-ec2
```

Configuration:

| Setting        | Value             |
| -------------- | ----------------- |
| AMI            | Amazon Linux 2023 |
| Instance Type  | t3.micro          |
| Public IP      | Disabled          |
| Security Group | ssm-ec2-sg        |
| IAM Profile    | EC2-SSM-Role      |



---

# 📜 User Data Script

Used the following script to install and start SSM Agent automatically:

```bash id="jlwmt4"
#!/bin/bash
sudo yum install -y amazon-ssm-agent
sudo systemctl enable amazon-ssm-agent
sudo systemctl start amazon-ssm-agent
```

Purpose:

Ensures SSM Agent registers with Systems Manager during instance boot. 

---

# Step 7 — Connect Using Session Manager

After waiting 3–5 minutes:

Navigate to:

```text id="jlwmc5"
AWS Systems Manager → Session Manager
```

Then:

1. Click Start Session
2. Select instance
3. Start interactive shell

No SSH required. 

---

# 💻 AWS CLI Connection

You can also connect using AWS CLI:

```bash id="zjlwm7"
aws ssm start-session \
 --target i-02479f6ba85990af6 \
 --region ap-south-1
```



---

# 📊 Security Group Reference

## Endpoint Security Group

| Direction | Protocol | Port |
| --------- | -------- | ---- |
| Inbound   | TCP      | 443  |
| Outbound  | All      | All  |

---

## EC2 Security Group

| Direction | Protocol | Port |
| --------- | -------- | ---- |
| Inbound   | None     | —    |
| Outbound  | TCP      | 443  |
| Outbound  | UDP      | 53   |
| Outbound  | TCP      | 53   |



---

# 🔥 Key Advantages

| Traditional SSH Access | SSM Session Manager |
| ---------------------- | ------------------- |
| Requires Public IP     | No Public IP        |
| SSH Key Required       | No SSH Key          |
| Port 22 Open           | No Inbound Ports    |
| Bastion Host Needed    | No Bastion          |
| Harder Auditing        | Centralized Logging |
| Less Secure            | More Secure         |

---

# 🏢 Real-World Use Cases

This architecture is commonly used for:

* Production EC2 administration
* Enterprise private environments
* Financial sector workloads
* Highly secure AWS environments
* Zero-trust infrastructure

---

# ⚠️ Important Lessons Learned

| Issue               | Root Cause           | Fix                  |
| ------------------- | -------------------- | -------------------- |
| Instance not in SSM | Missing S3 Endpoint  | Add S3 Endpoint      |
| SSM not working     | DNS ports blocked    | Allow Port 53        |
| Agent not starting  | Boot issue           | Add User Data script |
| Endpoints failing   | Private DNS disabled | Enable Private DNS   |

Most critical issue:

```text id="7jlwm3"
S3 Gateway Endpoint must be associated with the route table
```

for Amazon Linux 2023 SSM initialization. 

---

# 📚 Key Learning Outcomes

After completing this project, you will understand:

* AWS Systems Manager
* Session Manager architecture
* VPC Interface Endpoints
* Private EC2 connectivity
* IAM role-based EC2 access
* Secure AWS administration
* Zero-trust infrastructure design

---

# 👨‍💻 Author

Rohith

---

# ⭐ Summary

This project successfully implemented a secure private EC2 access architecture using:

* AWS Systems Manager
* VPC Interface Endpoints
* IAM Roles
* Private Networking

The solution eliminates the need for:

* Public IPs
* SSH keys
* Bastion hosts
* Open inbound ports

making it a modern enterprise-grade AWS administration approach.
