# AgentTesla Phishing & FTP Data Exfiltration Investigation

## Overview

This project is a SOC incident investigation of an AgentTesla phishing attack that resulted in FTP-based data exfiltration.

The investigation focused on two evidence sources:

* Phishing email evidence (`.eml`)
* Network packet capture evidence (`.pcap`)

The goal was to analyze the phishing email, identify the malicious attachment, investigate the infected host, confirm data exfiltration, and document indicators of compromise.

---

## Scenario

A staff member at **Globex Manufacturing Ltd.** opened an email attachment from a supposed supplier. Shortly after, the workstation showed suspicious behavior, including slow performance, outbound connections, and unknown FTP traffic.

The investigation confirmed phishing delivery, malware execution indicators, and successful FTP-based exfiltration.

---

## Tools Used

* Wireshark
* emldump.py
* unrar
* file
* strings
* md5sum
* sha256sum
* Linux CLI

---

## Key Findings

### Phishing Email Analysis

| Field          | Finding                                |
| -------------- | -------------------------------------- |
| Sender         | `Sertan ÇOKER <sertan@acronas.com.tr>` |
| Return-Path    | `sertan@acronas.com.tr`                |
| Originating IP | `94.141.120.32`                        |
| Subject        | `PURCHASE QUOTATION`                   |
| SPF            | `softfail`                             |
| DKIM           | `none`                                 |
| DMARC          | `none`                                 |

The email used a procurement-themed lure, asking the recipient to review attached technical specifications and provide a quotation.

---

### Attachment Analysis

The email attachment was extracted and identified as a RAR archive containing a Windows executable.

| Item           | Finding                                                            |
| -------------- | ------------------------------------------------------------------ |
| Archive        | `extracted.rar`                                                    |
| Payload        | `TECHNICAL SPECIFICATIONS.exe`                                     |
| File Type      | PE32 executable, Mono/.NET assembly                                |
| Payload MD5    | `65feefe926eb3f734b6968b35c23acb3`                                 |
| Payload SHA256 | `d1b068b826e3a9527cddd09866886caba895f390af930a9b35c027eb1c2db34c` |

Suspicious strings found in the payload included:

```text
txtPassword
txtUsername
btnLogin
frmLogin
pbHidePassword
pbShowPassword
```

This suggests credential/login-related functionality.

---

## Network Investigation

### Infected Host

```text
10.12.4.101
```

The infected host performed suspicious DNS lookups, connected to an external FTP server, authenticated with cleartext credentials, and uploaded files using FTP `STOR` commands.

### DNS Findings

| Domain                 | Resolved IP                                       |
| ---------------------- | ------------------------------------------------- |
| `api.ipify.org`        | `172.67.74.152`, `104.26.12.205`, `104.26.13.205` |
| `ftp.ercolina-usa.com` | `192.254.225.136`                                 |

The query to `api.ipify.org` suggests the malware checked the victim’s public IP address.

---

## FTP Exfiltration

| Field         | Finding                |
| ------------- | ---------------------- |
| FTP Server    | `192.254.225.136`      |
| FTP Domain    | `ftp.ercolina-usa.com` |
| FTP Username  | `ben@ercolina-usa.com` |
| FTP Password  | `nXe0M~WkW&nJ`         |
| Server Banner | Pure-FTPd              |

FTP login was successful, and the server returned successful transfer responses.

### Uploaded Files

| Uploaded File                                                                         | Likely Data Type |
| ------------------------------------------------------------------------------------- | ---------------- |
| `PW_gary.strickman-DESKTOP-VJCRXEB_2024_12_04_21_20_57.html`                          | Password data    |
| `CO_Chrome_Default.txt_gary.strickman-DESKTOP-VJCRXEB_2024_12_04_21_21_03.txt`        | Chrome cookies   |
| `CO_Edge Chromium_Default.txt_gary.strickman-DESKTOP-VJCRXEB_2024_12_04_21_21_04.txt` | Edge cookies     |
| `KL_gary.strickman-DESKTOP-VJCRXEB_2024_12_04_21_41_04.html`                          | Keylogger logs   |

The `STOR` commands confirmed successful file upload activity from the infected host to the external FTP server.

---

## Attack Timeline

| Time                      | Event                               |
| ------------------------- | ----------------------------------- |
| `2024-12-04 12:51:17 UTC` | Phishing email sent                 |
| `2024-12-04 21:20:56 UTC` | Host queried `api.ipify.org`        |
| `2024-12-04 21:21:02 UTC` | Host queried `ftp.ercolina-usa.com` |
| `2024-12-04 21:21:02 UTC` | FTP login initiated                 |
| `2024-12-04 21:21:03 UTC` | Password file uploaded              |
| `2024-12-04 21:21:04 UTC` | Browser cookie files uploaded       |
| `2024-12-04 21:41:08 UTC` | Keylogger file uploaded             |

---

## Indicators of Compromise

| IOC Type        | Value                                                              |
| --------------- | ------------------------------------------------------------------ |
| Sender Email    | `sertan@acronas.com.tr`                                            |
| Originating IP  | `94.141.120.32`                                                    |
| Payload         | `TECHNICAL SPECIFICATIONS.exe`                                     |
| Payload SHA256  | `d1b068b826e3a9527cddd09866886caba895f390af930a9b35c027eb1c2db34c` |
| Infected Host   | `10.12.4.101`                                                      |
| FTP Server      | `192.254.225.136`                                                  |
| FTP Domain      | `ftp.ercolina-usa.com`                                             |
| Public IP Check | `api.ipify.org`                                                    |

---

## Risk Rating

**High**

This incident is rated High because the investigation confirmed:

* Phishing delivery
* Malicious executable attachment
* Infected host activity
* Cleartext FTP authentication
* Successful exfiltration of password data, browser cookies, and keylogger logs

---

## Reports

* [Phishing Analysis Report](reports/Phishing-Analysis-Report.pdf)
* [Network Investigation Report](reports/Network-Investigation-Report.pdf)
* [IOC Summary Table](reports/IOC-Summary-Table.xlsx)

---

## Evidence

Screenshots are available in the [`screenshots/`](screenshots/) folder.

Key evidence includes:

* Email authentication results
* Attachment extraction and hashes
* DNS queries and responses
* FTP username and password
* FTP `STOR` uploads
* Successful FTP transfer responses
* Follow TCP Stream evidence

---

## Disclaimer

This project was completed in a controlled lab environment for educational and professional development purposes.

Raw malware samples, executable payloads, raw packet captures, credentials, and raw email evidence are not included in this repository.
