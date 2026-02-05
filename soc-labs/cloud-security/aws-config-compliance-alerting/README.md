# ☁️ AWS Continuous Compliance Monitoring

## Detecting Public S3 Exposure with AWS Config + SNS

### 📌 Project Summary

This project demonstrates a real-world cloud security detection pipeline that identifies and alerts on dangerous S3 misconfigurations in real time.

I implemented continuous compliance monitoring using AWS Config and automated alerting via Amazon SNS to detect when an S3 bucket becomes publicly accessible — one of the most common causes of data breaches in AWS environments.

The project simulates an attack/misconfiguration scenario, triggers detection, investigates the event, and performs remediation.

---

## 🚨 Threat Scenario

Publicly exposed S3 buckets have caused major global breaches (data leaks, credential exposure, internal document dumps).

This lab simulates that exact risk:

1. A bucket is accidentally made public
2. AWS Config detects policy drift
3. A compliance violation is triggered
4. SNS sends a real-time alert
5. The issue is investigated and fixed

---

## 🎯 Security Objective

Build an automated detection and response workflow that:

* Continuously monitors AWS resource configurations
* Detects policy violations
* Sends immediate alerts
* Provides audit visibility
* Enables fast remediation

---

## 🧰 Technologies Used

* AWS Config
* Amazon SNS
* Amazon S3
* IAM Roles
* CloudTrail

---

## 🔎 Detection Rule Implemented

**AWS Managed Rule:**

```
s3-bucket-public-read-prohibited
```

Detects:

* Buckets allowing public read access
* Policy misconfigurations
* Exposure risks

---

## 🏗️ Architecture Overview

```
S3 Misconfiguration
        ↓
AWS Config Evaluation
        ↓
Rule Violation Detected
        ↓
SNS Notification Triggered
        ↓
Email Alert Sent
        ↓
Investigation via Timeline
        ↓
Remediation Applied
```

---

## 📄 Full Evidence Documentation

📎 Detailed walkthrough with screenshots:

**Download the full lab report:** View Full Report (PDF)


---

## 🧠 Security Skills Demonstrated

* Cloud misconfiguration detection
* Continuous compliance monitoring
* Alert pipeline design
* Incident investigation
* Remediation workflow

---

## Project Summary (Quick Explanation)

“I built a real-time AWS compliance monitoring pipeline that detects when an S3 bucket becomes public, triggers an SNS alert, and provides investigation visibility through AWS Config timelines.”
