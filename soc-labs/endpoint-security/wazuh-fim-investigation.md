# Wazuh EDR & File Integrity Monitoring (FIM)

---

## Objective
Deploy Endpoint Detection and Response (EDR) capabilities using Wazuh to
monitor sensitive files on a Windows 11 endpoint and detect unauthorized
modifications in real time. The lab also validates compliance alignment
with **GDPR** and **PCI DSS**.

---

## Lab Environment

- Endpoint: Windows 11  
- EDR Platform: Wazuh Agent  
- Monitoring Scope: `C:\SensitiveData\`

---

## Step 1 — FIM Configuration

The Wazuh agent was configured to monitor a sensitive directory using the
`<syscheck>` module in `ossec.conf`.

The following settings were enabled:

- Real-time monitoring
- File change reporting
- Directory: `\SensitiveData`

This ensures every file modification, deletion, or creation is captured
by the EDR agent.

---

## Step 2 — Service Activation

After configuration, the Wazuh agent service was restarted using
PowerShell to apply the new FIM rules.

This confirmed that the endpoint was actively enforcing the new
monitoring policy.

---

## Step 3 — Incident Simulation

To test detection, a simulated insider-threat scenario was created:

- `ImportantData.txt` was modified  
- The same file was then deleted  

This represents:
- Unauthorized data manipulation  
- Potential data destruction or cover-up

---

## Step 4 — Alert Detection & Forensics

The Wazuh dashboard successfully generated security alerts for the event.

Key findings:

- **Rule 553** was triggered (File Deleted)
- The alert included:
  - Full file path  
  - Timestamp  
  - Action performed  

This provided forensic-grade evidence of what changed, when it happened,
and which file was affected.

---

## Compliance Mapping

Wazuh automatically mapped the alert to regulatory standards:

### GDPR
The event was classified under:
- Personal data integrity
- Unauthorized data modification

### PCI DSS
The alert triggered:
- **Requirement 11.5** — File Integrity Monitoring

This proves the system was enforcing compliance-grade monitoring rather
than basic logging.

---

## Alert Severity

The simulated attack generated:

- Level 7 (High-severity) alerts  
- Level 5 alerts  

This allowed the SOC to visualize risk escalation during the incident.

---

## Final Assessment

This investigation confirmed that Wazuh provides:

- Real-time endpoint visibility  
- File-level forensic tracking  
- Compliance-aligned security monitoring  

The endpoint transitioned from **blind** to **fully monitored**, enabling
security teams to detect, investigate, and report unauthorized activity.

---

## SOC Value

This lab demonstrates:

- Endpoint detection and response  
- File integrity monitoring  
- Compliance-based alerting  
- Digital forensics capability  

These are core responsibilities of a modern Security Operations Center.
