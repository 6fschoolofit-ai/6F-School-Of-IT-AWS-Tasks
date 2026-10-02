# Amazon CloudWatch EC2 CPU Anomaly Detection Lab

This project demonstrates how to configure Amazon CloudWatch Anomaly Detection for EC2 CPU monitoring using machine learning-based alerting instead of static thresholds.

The lab includes:

* EC2 instance setup
* CloudWatch anomaly detection alarm
* SNS email notifications
* CPU stress testing
* Alarm triggering
* CloudWatch Investigate (GenAI RCA)

Detailed documentation with screenshots is available inside the `docs/` folder. 

---

# 📄 Documentation

Document Name:

* `11.CloudWatch_EC2_Anomaly_Detection_Lab.docx`

Repository Structure:

```text id="mjlwm4"
project-folder/
│
├── README.md
│
└── docs/
    └── 11.CloudWatch_EC2_Anomaly_Detection_Lab.docx
```

---

# 📌 Project Objective

The goal of this lab is to monitor EC2 CPU usage using:

* Amazon CloudWatch
* AI/ML-based anomaly detection
* SNS notifications
* CloudWatch Investigate (GenAI AI Operations)

Instead of using fixed CPU thresholds like:

```text id="l0q1w9"
80% CPU
```

CloudWatch learns normal server behavior automatically and alerts only when unusual activity occurs. 

---

# 🏗️ Real-World Use Case

Example Scenario:

A production Node.js API server experiences:

* High CPU during business hours
* Low CPU at night

Traditional static alarms generate false alerts.

With anomaly detection:

* Low CPU at night is considered normal
* Unexpected spikes trigger alerts immediately

This improves operational monitoring and reduces alert fatigue. 

---

# ☁️ AWS Services Used

* Amazon EC2
* Amazon CloudWatch
* Amazon SNS
* CloudWatch Investigate (GenAI AI Operations)
* EC2 Instance Connect

---

# 🛠️ Lab Environment

| Configuration        | Value                  |
| -------------------- | ---------------------- |
| Region               | ap-south-1 (Mumbai)    |
| EC2 Instance         | cloudwatch-test-server |
| Instance Type        | t2.micro               |
| Metric Used          | CPUUtilization         |
| Alarm Type           | Anomaly Detection      |
| Notification Service | SNS Email              |



---

# 🚀 Step-by-Step Workflow

# Step 1 — Launch EC2 Instance

Launch:

* Amazon Linux 2023
* t2.micro
* Free Tier eligible

Instance Name:

```text id="qcnrtu"
cloudwatch-test-server
```

---

# Step 2 — Select CloudWatch Metric

Navigate to:

```text id="6q4x6z"
CloudWatch → Alarms → Create Alarm → Select Metric
```

Choose:

```text id="dsmc94"
EC2 → Per-Instance Metrics → CPUUtilization
```

Metric configuration:

* Namespace: AWS/EC2
* Statistic: Average
* Period: 5 minutes



---

# Step 3 — Configure Anomaly Detection

Switch threshold type from:

```text id="y6y6x3"
Static
```

to:

```text id="qjlwm5"
Anomaly Detection
```

Configuration used:

| Setting             | Value                 |
| ------------------- | --------------------- |
| Threshold Type      | Anomaly Detection     |
| Condition           | Outside of the band   |
| Threshold           | 2 standard deviations |
| Datapoints to alarm | 1 out of 1            |
| Missing Data        | Treat as missing      |



---

# 🤖 Why Anomaly Detection?

Benefits:

* Learns normal workload patterns
* Reduces false alarms
* Detects unusual spikes automatically
* Adapts over time using ML

Example:

```text id="mxg5vl"
CPU spike at 2 AM → Alert triggered immediately
```

---

# Step 4 — Create Alarm

Alarm Name:

```text id="xxy5zq"
EC2-CPUAnomaly-cloudwatch-test-server
```

Initial State:

```text id="jlwmwi"
INSUFFICIENT_DATA
```

This is normal because CloudWatch needs baseline data to train the ML model. 

---

# Step 5 — Configure SNS Email Notification

SNS topic created for email alerts.

Example flow:

```text id="mjlwmk"
CloudWatch Alarm → SNS Topic → Email Notification
```

Important Note:

Gmail security link scanning may auto-unsubscribe SNS confirmation emails.

Fix used:

* Recreate SNS subscription
* Open confirmation link in Incognito mode



---

# Step 6 — CPU Stress Test

Connect to EC2 using EC2 Instance Connect.

Run stress command:

```bash id="1p5y1v"
yes > /dev/null & yes > /dev/null & yes > /dev/null & yes > /dev/null &
```

This creates multiple CPU-consuming background processes.

Result:

* CPU usage reached 100%
* CloudWatch graph updated within 5 minutes



---

# Step 7 — Force Alarm Trigger

Since anomaly detection requires training time, the alarm was temporarily switched to:

```text id="jlwmx4"
Static Threshold
```

Configuration:

| Setting   | Value        |
| --------- | ------------ |
| Condition | Greater than |
| Threshold | 5%           |
| CPU Usage | ~100%        |

This immediately triggered the alarm. 

---

# Step 8 — Alarm Triggered Successfully

Within 5 minutes:

* Alarm entered ALARM state
* SNS email notification received

Alarm details:

| Property     | Value                                 |
| ------------ | ------------------------------------- |
| Alarm Name   | EC2-CPUAnomaly-cloudwatch-test-server |
| State Change | INSUFFICIENT_DATA → ALARM             |
| CPU Detected | ~99.99%                               |
| Threshold    | Greater than 5%                       |

Pipeline Verified:

```text id="qzjlwm"
EC2 CPU Spike
    ↓
CloudWatch Detection
    ↓
SNS Notification
    ↓
Email Delivered
```



---

# 🤖 Step 9 — CloudWatch Investigate (GenAI RCA)

CloudWatch Investigate provides AI-powered operational troubleshooting.

Features:

* Root cause analysis
* Telemetry analysis
* Deployment correlation
* AWS Health event analysis
* CloudTrail analysis
* Remediation suggestions
* Incident reports

This helps DevOps and SRE teams quickly identify operational issues. 

---

# 📊 Key Learning Outcomes

After completing this lab, you will understand:

* CloudWatch anomaly detection
* AI-based monitoring
* EC2 metric monitoring
* SNS email notifications
* Alarm management
* CPU stress testing
* Root cause analysis using GenAI
* Real-time AWS monitoring pipelines

---

# 🔥 Useful Commands

## Stress CPU

```bash id="h4bjlwm"
yes > /dev/null & yes > /dev/null & yes > /dev/null & yes > /dev/null &
```

---

## Stop Stress Processes

```bash id="n7gnrc"
pkill yes
```

---

# 📈 Advantages of Anomaly Detection

| Static Threshold     | Anomaly Detection     |
| -------------------- | --------------------- |
| Fixed values         | Learns patterns       |
| More false alarms    | Smarter alerts        |
| Manual tuning        | Automatic adaptation  |
| Limited intelligence | ML-powered monitoring |

---

# ⚠️ Important Notes

* CloudWatch ML models improve over time
* More historical data = better accuracy
* Stop or terminate EC2 instances after testing
* SNS subscriptions must be confirmed



---

# 👨‍💻 Author

Rohith

---

# ⭐ Summary

This lab demonstrates a complete AI-powered monitoring workflow using AWS CloudWatch Anomaly Detection.

The project successfully implemented:

* EC2 monitoring
* ML-based anomaly detection
* SNS notifications
* Alarm triggering
* GenAI root cause analysis

This architecture reflects modern enterprise DevOps and SRE monitoring practices.
