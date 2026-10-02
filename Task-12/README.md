   # AWS Route 53 Private Hosted Zone

This project demonstrates the implementation of an AWS Route 53 Private Hosted Zone integrated with a custom VPC for internal DNS resolution between EC2 instances.

The setup enables EC2 instances inside the VPC to communicate using domain names instead of hardcoded private IP addresses — a real-world architecture pattern used for internal service discovery in enterprise environments. 

---

# 📄 Documentation

Detailed implementation steps with screenshots are available inside the `docs/` folder.

Document Name:

* `12.Route53_Private_Hosted_Zone.docx`

Repository Structure:

```text id="tw1jlwm"
project-folder/
│
├── README.md
│
└── docs/
    └── 12.Route53_Private_Hosted_Zone.docx
```

---

# 📌 Project Objective

The goal of this project is to:

* Create a custom VPC
* Configure public and private subnets
* Launch EC2 instances
* Create Route 53 Public and Private Hosted Zones
* Configure internal DNS records
* Verify private DNS resolution inside the VPC

This setup allows internal services to communicate securely using DNS names rather than private IP addresses. 

---

# 🏗️ Architecture Overview

```text id="tkzjlwm"
Internet
    |
Route 53 Public Hosted Zone
    |
App Server (Public EC2)
    |
--------------------------------
|                              |
VPC: my-vpc (10.0.0.0/16)
|                              |
Public Subnet            Private Subnet
10.0.1.0/24              10.0.2.0/24
|                              |
App Server                DB Server
10.0.1.85                 10.0.2.71
    |
Route 53 Private Hosted Zone
```

---

# ☁️ AWS Services Used

* Amazon VPC
* Amazon EC2
* Amazon Route 53
* Internet Gateway
* Route Tables
* GoDaddy DNS

---

# 🌐 Environment Details

| Configuration | Value                   |
| ------------- | ----------------------- |
| Region        | us-east-1 (N. Virginia) |
| Domain        | rohithportfolio.xyz     |
| VPC Name      | my-vpc                  |
| VPC CIDR      | 10.0.0.0/16             |



---

# 🚀 Step-by-Step Implementation

# Step 1 — Create Custom VPC

Created VPC:

```text id="q6cz4l"
my-vpc
```

CIDR:

```text id="wjlwm2"
10.0.0.0/16
```

Purpose:

* Isolated network environment
* Supports public and private workloads



---

# Step 2 — Create Public and Private Subnets

Two subnets created:

| Subnet     | CIDR        | Purpose         |
| ---------- | ----------- | --------------- |
| pub-subnet | 10.0.1.0/24 | App Server      |
| pvt-subnet | 10.0.2.0/24 | Database Server |



---

# Step 3 — Configure Route Tables

Created:

| Route Table | Purpose         |
| ----------- | --------------- |
| pub-RT      | Internet access |
| pvt-RT      | Private routing |

Public Route Table:

```text id="l4gjlwm"
0.0.0.0/0 → Internet Gateway
```

Private Route Table:

```text id="tnjlwm1"
Local routes only
```



---

# Step 4 — Create Internet Gateway

Internet Gateway created:

```text id="0pjlwm"
my-igw
```

Attached to:

```text id="jjlwm6"
my-vpc
```

Purpose:

* Allows internet access for public subnet resources



---

# Step 5 — Launch EC2 Instances

Two EC2 instances launched:

| Instance          | Subnet         | IP Type             |
| ----------------- | -------------- | ------------------- |
| rohith-app-server | Public subnet  | Public + Private IP |
| rohith-db-server  | Private subnet | Private IP only     |

Configuration:

| Instance   | Private IP |
| ---------- | ---------- |
| App Server | 10.0.1.85  |
| DB Server  | 10.0.2.71  |



---

# Step 6 — Create Route 53 Hosted Zones

Two hosted zones created using the same domain:

```text id="jlwmc9"
rohithportfolio.xyz
```

| Hosted Zone         | Purpose             |
| ------------------- | ------------------- |
| Public Hosted Zone  | Internet-facing DNS |
| Private Hosted Zone | Internal VPC DNS    |

Important:

The private hosted zone was associated with the VPC during creation itself. 

---

# Step 7 — Add Private DNS Records

Created internal A records:

| Record Name                  | IP Address |
| ---------------------------- | ---------- |
| app.rohithportfolio.xyz      | 10.0.1.85  |
| database.rohithportfolio.xyz | 10.0.2.71  |

TTL:

```text id="4g3w2o"
60 seconds
```

These records are accessible only from inside the VPC. 

---

# Step 8 — Add Public DNS Records

Public hosted zone configured with:

| Record              | Value        |
| ------------------- | ------------ |
| rohithportfolio.xyz | 13.222.7.129 |

This points internet traffic to the public EC2 application server. 

---

# Step 9 — Update GoDaddy Nameservers

Updated domain nameservers in GoDaddy:

```text id="jlwm9x"
ns-3.awsdns-00.com
ns-1050.awsdns-03.org
ns-780.awsdns-33.net
ns-1786.awsdns-31.co.uk
```

Purpose:

* Delegate DNS authority to AWS Route 53



---

# Step 10 — Verify VPC DNS Settings

Verified:

| Setting                  | Status     |
| ------------------------ | ---------- |
| DNS Resolution           | Enabled    |
| DNS Hostnames            | Enabled    |
| Private Zone Association | Successful |

These settings are required for Route 53 private DNS functionality. 

---

# Step 11 — Verify Private DNS Resolution

From inside the App Server:

```bash id="jlwmv8"
nslookup database.rohithportfolio.xyz
```

Result:

```text id="jlwmu2"
10.0.2.71
```

---

Another verification:

```bash id="rjlwm7"
nslookup app.rohithportfolio.xyz
```

Result:

```text id="fjlwm1"
10.0.1.85
```

---

Public internet verification:

```bash id="7jlwm0"
nslookup app.rohithportfolio.xyz 8.8.8.8
```

Result:

```text id="jlwmn5"
SERVFAIL
```

This confirms that the private DNS records are not accessible publicly. 

---

# 🔐 Security Benefits

Benefits of Private Hosted Zones:

* Internal DNS resolution only
* No public exposure
* Secure service communication
* Easier infrastructure management
* No hardcoded private IPs

---

# 🏢 Real-World Enterprise Use Case

Instead of applications connecting directly to:

```text id="jlwmf4"
10.0.2.71
```

they connect using:

```text id="jlwm8g"
database.rohithportfolio.xyz
```

Advantages:

* Easier IP management
* Better scalability
* Simplified migrations
* Cleaner microservice architecture



---

# 📚 Key Learning Outcomes

After completing this project, you will understand:

* Route 53 Public Hosted Zones
* Route 53 Private Hosted Zones
* VPC DNS architecture
* Internal DNS resolution
* Public vs private DNS
* EC2 internal service discovery
* Enterprise DNS design patterns

---

# ⚠️ Important Notes

* Private Hosted Zone records are only resolvable inside associated VPCs
* DNS Resolution and DNS Hostnames must be enabled
* VPC association is required during Private Hosted Zone creation
* Public and Private Hosted Zones can share the same domain name

---

# 👨‍💻 Author

Rohith

---

# ⭐ Summary

This project successfully implemented an enterprise-style internal DNS architecture using:

* Route 53 Private Hosted Zones
* Custom VPC networking
* EC2 internal service discovery

The setup enables secure, scalable, and manageable communication between internal AWS resources using domain names instead of hardcoded IP addresses.
