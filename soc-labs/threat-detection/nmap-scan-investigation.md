# Network Reconnaissance Investigation
**Snort • Suricata • Wireshark**

---

## Objective
Detect, analyze, and validate network reconnaissance activity using
Snort IDS, Suricata NSM, and Wireshark packet analysis.

---

## Lab Scenario
Unusual network traffic was observed targeting internal systems.
Security monitoring tools were used to determine whether a scan
or attack was taking place.

---

## Step 1 — Packet Capture & Traffic Analysis (Wireshark)

Network traffic revealed repeated TCP SYN packets with the following pattern:

- Fixed destination port: **443**
- Multiple destination IP addresses:
  - 192.168.1.114
  - 192.168.1.6
  - Others
- Only **SYN packets** were present (no ACK responses)
- Randomized source ports
- Fast and continuous timing

### Analysis
This traffic pattern matches a **horizontal SYN scan**, where a single
service is probed across many hosts. This is a common reconnaissance
technique used by tools such as:

nmap -sS -p 443 192.168.1.0/24

Each host received only one probe, confirming that this was a
service discovery scan rather than a full port scan.

### Packet Evidence

![Wireshark SYN Scan](threat-detection/wireshark-syn-scan.png)


---

## Step 2 — IDS Detection (Snort & Suricata)

Both Snort and Suricata generated alerts for abnormal TCP connection
attempts based on:

- Repeated SYN packets without handshake completion
- Rapid scanning behavior
- Consistent targeting of port 443 across multiple hosts

Snort preprocessors such as:
- `sfPortscan`
- `stream5`

identified this activity as network reconnaissance.

Suricata similarly detected port scanning and abnormal HTTP/SSL
probing attempts.

### Snort Detection

![Snort Alert](threat-detection/snort-alert.png)

### Suricata Detection

![Suricata Alert](threat-detection/suricata-alert.png)


---

## Step 3 — Credential and Data Exfiltration Review

Additional analysis was performed to determine whether the scan
led to further compromise:

| Check | Result |
|------|--------|
Suspicious domains (>50 characters) | Not found  
Large outbound data transfers | None detected  
Suspicious DNS queries | None observed  
Credentials observed | Username: **test**, Password: **test**  

The absence of large transfers, abnormal DNS activity, or exploitation
indicates this was a reconnaissance-only event.

### IDS Log Output

![IDS Logs](threat-detection/ids-log-output.png)


---

## Step 4 — Detection Architecture

Snort analyzed the traffic using:

- Packet Decoder  
- Preprocessors (stream5, frag3, sfPortscan, http_inspect)  
- Detection Engine  
- Alert Output Modules  

Snort was running in **IDS mode**, meaning the traffic was logged
but not blocked.

In a production IPS configuration, this scan could have been
blocked using a rule such as:

drop tcp any any -> any 443


---

## Final Assessment

The environment was targeted by a **horizontal Nmap SYN scan
against HTTPS services (port 443)**.

✔ Detected by Snort  
✔ Detected by Suricata  
✔ Confirmed by Wireshark  
❌ No exploitation observed  
❌ No data exfiltration detected  

---

## SOC Value

This investigation demonstrates:
- Early-stage attack detection
- IDS alert validation
- Packet-level traffic analysis
- Threat classification and reporting

These are core activities performed in a Security Operations Center.

