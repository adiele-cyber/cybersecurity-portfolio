# AWS Threat Detection & Automated Incident Response System (Honeytoken Defense)

## Overview

This project simulates a real-world cloud credential compromise scenario and demonstrates how to design a detection engineering pipeline with automated containment using native AWS services.

The system detects unauthorized access to a high-value honeytoken secret and automatically revokes the attacker’s permissions within seconds.

This lab mirrors real SOC detection + response workflows used in enterprise cloud environments.

---

## Threat Model

Scenario:
An IAM user gains unauthorized access and attempts to retrieve sensitive production credentials stored in AWS Secrets Manager.

Goal:
Detect the action in near real-time and immediately contain the threat.

---

## Architecture

Detection Flow 1:
CloudTrail → CloudWatch Logs → Metric Filter → CloudWatch Alarm → SNS Notification

Detection Flow 2 (Preferred Production Path):
CloudTrail → EventBridge Rule → SNS + Lambda Kill-Switch

Response:
Lambda function automatically attaches an explicit deny-all IAM policy to the offending user, blocking all further AWS activity.

---

## Key Security Capabilities Demonstrated

- CloudTrail log analysis & management event monitoring
- Detection engineering using metric filters & event patterns
- Event-driven security automation
- IAM privilege revocation using explicit deny policy
- Honeytoken-based intrusion detection
- Real-time SOC alerting via SNS
- Automated containment (“kill-switch” model)

---

## Attack Simulation Results

Test user: victim-lab-user  
Action: GetSecretValue on Production_Database_Credentials  

System Behavior:
- Detection triggered
- SNS alert received
- Lambda executed
- EmergencyDenyAll policy attached
- Subsequent AWS calls returned AccessDenied

This demonstrates effective containment of credential misuse and insider threat scenarios.

---

## Why EventBridge Was Superior

- Lower latency (event-driven vs 60s metric window)
- Full CloudTrail JSON context (IP, ARN, secret ID)
- Better extensibility for production automation

---

## Compliance Alignment

This project aligns with:

- NIST SP 800-53 SI-4 (Continuous Monitoring)
- SI-7 (Automated Response)
- CM-6 (Least Privilege Enforcement)

---

## Lessons Learned

- Read-only API calls require ENABLED_WITH_ALL_CLOUDTRAIL_MANAGEMENT_EVENTS
- Explicit Deny overrides all permissions
- Automated response drastically reduces dwell time
- Event-driven architectures are superior for containment

---

## Documentation

Full technical implementation:
[Project Report](./Adiele_Chibuike_Ambless_CloudSecurity_Exam.pdf.pdf)

---

## Author

Chibuike Ambless Adiele  
Systems & Security Analyst  
