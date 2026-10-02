# AWS VPC Endpoints Task

This repository demonstrates how to configure and test AWS VPC Endpoints for secure private connectivity without using public internet access.

The project covers:

* S3 Gateway Endpoint
* EC2 Instance Connect Endpoint (EICE)

Detailed documentation is available inside the `docs/` folder.

---

# 📄 Documentation

Document Name:

* `9.VPC Endpoint Task.docx`

Repository Structure:

```text
project-folder/
│
├── README.md
│
└── docs/
    └── 9.VPC Endpoint Task.docx
```

---

# 📌 Project Objectives

## 1. S3 Gateway Endpoint

Enable a private EC2 instance to access Amazon S3 without:

* Internet Gateway (IGW)
* NAT Gateway
* Public IP

Using:

* AWS Gateway Endpoint for S3

---

## 2. EC2 Instance Connect Endpoint (EICE)

Connect securely from your browser/laptop to a private EC2 instance without:

* Bastion Host
* Public IP

Using:

* EC2 Instance Connect Endpoint

---

# 🏗️ Architecture

## S3 Gateway Endpoint Architecture

```text
Private EC2
     |
Route Table
     |
S3 Gateway Endpoint
     |
Amazon S3
```

---

## EC2 Instance Connect Endpoint Architecture

```text
Your Browser
      |
HTTPS 443
      |
EICE Endpoint
      |
SSH 22
      |
Private EC2
```

---

# 🚀 Task 1 — S3 Gateway Endpoint

## Step 1 — Create VPC

Example CIDR:

```text
10.0.0.0/16
```

---

## Step 2 — Create Private Subnet

Example:

```text
10.0.1.0/24
```

Important:

* Disable auto-assign public IP
* Do NOT attach Internet Gateway route
* Do NOT use NAT Gateway

---

## Step 3 — Launch Private EC2

Launch an EC2 instance inside the private subnet.

Security Group:

* Allow SSH if needed

---

## Step 4 — Create S3 Bucket

Create an S3 bucket.

Example:

```text
rohith-private-s3-demo
```

Upload a sample file for testing.

---

## Step 5 — Create Gateway Endpoint

Go to:

```text
VPC Console → Endpoints → Create Endpoint
```

Select:

* AWS Services
* Search for `s3`
* Choose:

```text
com.amazonaws.ap-south-1.s3
```

Type:

```text
Gateway
```

---

## Step 6 — Select Route Table

Choose:

* Your VPC
* Private Route Table

AWS automatically adds the route.

---

## Step 7 — Verify Route Table

You should see a route similar to:

```text
pl-xxxxx → vpce-xxxxx
```

This is the S3 prefix list route.

---

## Step 8 — Attach IAM Role

Create and attach IAM Role:

```text
AmazonS3ReadOnlyAccess
```

Attach the role to the EC2 instance.

---

## Step 9 — Test Connectivity

Connect to the EC2 instance and run:

```bash
aws s3 ls
```

OR

```bash
aws s3 ls s3://rohith-private-s3-demo
```

Expected Result:

* S3 access works privately
* No internet connectivity required

---

## Step 10 — Verification

Run:

```bash
ping google.com
```

Expected:

```text
Should fail
```

But:

```bash
aws s3 ls
```

Should work successfully.

This proves that S3 access is happening privately through the VPC Endpoint.

---

# 🏢 Real-Time Enterprise Use Case

```text
Private App Servers
        |
S3 Gateway Endpoint
        |
S3 Backups / Logs / Images
```

Benefits:

* Improved security
* No public internet exposure
* Reduced NAT Gateway cost

---

# 🚀 Task 2 — EC2 Instance Connect Endpoint (EICE)

## Step 1 — Create Private Subnet

Example:

```text
10.0.2.0/24
```

* No public IP assignment

---

## Step 2 — Launch Private EC2

Launch EC2 with:

* No public IP
* Amazon Linux preferred

---

## Step 3 — Configure Security Group

Inbound Rule:

| Type | Port | Source              |
| ---- | ---- | ------------------- |
| SSH  | 22   | EICE Security Group |

---

## Step 4 — Create EC2 Instance Connect Endpoint

Go to:

```text
VPC Console → Endpoints → Create Endpoint
```

Select:

```text
EC2 Instance Connect Endpoint
```

Choose:

* VPC
* Private Subnet
* Security Group

---

## Step 5 — Configure Endpoint Security Group

Inbound Rule:

| Type  | Port | Source |
| ----- | ---- | ------ |
| HTTPS | 443  | My IP  |

---

## Step 6 — Connect to Private EC2

Go to:

```text
EC2 Console → Select Instance → Connect
```

Choose:

```text
EC2 Instance Connect
```

Select the endpoint and connect.

Expected Output:

```bash
[ec2-user@ip-10-0-x-x ~]$
```

This works without:

* Public IP
* Bastion Host

---

# 🏢 Real-Time Enterprise Use Case

```text
Production Private EC2
         |
EICE for DevOps/Admin Access
```

Benefits:

* Secure administration
* Modern private infrastructure
* Reduced attack surface

---

# 🔍 Difference Between S3 Gateway Endpoint and EICE

| Feature            | S3 Gateway Endpoint        | EICE                     |
| ------------------ | -------------------------- | ------------------------ |
| Purpose            | Private AWS service access | Private EC2 admin access |
| Internet Needed    | No                         | No                       |
| NAT Gateway Needed | No                         | No                       |
| Common Use         | S3/DynamoDB access         | SSH to private EC2       |

---

# 🛠️ AWS Services Used

* Amazon VPC
* Amazon EC2
* Amazon S3
* IAM
* VPC Endpoints
* EC2 Instance Connect Endpoint

---

# ✅ Key Learning Outcomes

After completing this task, you will understand:

* Private AWS networking
* Secure S3 access without internet
* Accessing private EC2 securely
* VPC Endpoint architecture
* Enterprise-grade AWS security practices

---

# 👨‍💻 Author

Rohith

---

# ⭐ Notes

This project demonstrates secure and cost-optimized AWS networking architecture commonly used in enterprise environments.
