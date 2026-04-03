# Windows Log Analysis – Splunk SIEM Investigation

![Splunk](https://img.shields.io/badge/SIEM-Splunk-orange)
![Windows](https://img.shields.io/badge/Target-Windows%20Server%202022-blue)
![Status](https://img.shields.io/badge/Status-Completed-green)

---

## Project Overview

Windows Event Logs are one of the most valuable sources of security data 
a SOC analyst works with daily. This project demonstrates deep analysis of 
Windows Security Event Logs using Splunk SIEM — investigating successful 
and failed logins, monitoring process creation, detecting a brute force attack, 
and building a real-time SOC monitoring dashboard.

---

## Lab Architecture

| Component | System | IP Address | Role |
|-----------|--------|------------|------|
| Attacker | Kali Linux 2025.2 | 192.168.52.133 | Brute force simulation |
| Target | Windows Server 2022 | 192.168.52.139 | Generate security logs |
| SIEM | Splunk Enterprise 10.2.1 | 127.0.0.1:8000 | Log analysis & dashboard |

---

## Screenshots

### Splunk Dashboard – Log Collection
![Splunk Dashboard](screenshots/1_splunk_dashboard.png)
*Splunk collecting Windows Security Event logs in real time from Windows Server 2022.*

---

### EventCode 4624 – Successful Login Analysis
![Event 4624](screenshots/2_event_4624.png)
*Splunk query for EventCode=4624 showing successful login events and account breakdown.*

---

### Attack Simulation – Hydra Brute Force
![Brute Force](screenshots/4_brute_force.png)
*Hydra brute force attack from Kali Linux targeting RDP on Windows Server — generating mass failed login events (MITRE ATT&CK T1110).*

---

### EventCode 4625 – Brute Force Detected
![Attack Detected](screenshots/5_attack_detected.png)
*Splunk detected 626 EventCode=4625 failed login events from the Hydra attack — confirming successful SIEM detection.*

---

### SOC Dashboard – Login Monitoring
![Dashboard Top](screenshots/6_dashboard_top.png)
*Splunk dashboard showing successful login trends and brute force attack spike clearly visible between 4:27–4:33 AM.*

---

### SOC Dashboard – Process Creation & Targeted Accounts
![Dashboard Bottom](screenshots/7_dashboard_bottom.png)
*Dashboard showing process creation events and top targeted accounts — administrator account hit 3,200+ times.*

---

### Investigation – Attacker Identified
![Attacker Identified](screenshots/8_attacker_identified.png)
*Splunk investigation confirmed attack originated from workstation "kali" with 3,326 failed login attempts.*

---

## Windows Event IDs Investigated

| Event ID | Name | SOC Relevance |
|----------|------|---------------|
| 4624 | Successful Logon | Monitor for unusual login activity |
| 4625 | Failed Logon | Key indicator of brute force attacks |
| 4688 | Process Creation | Detect suspicious process execution |
| 4672 | Special Privileges | Potential privilege escalation |

---

## Attacks Simulated

| Attack | Tool | MITRE ATT&CK |
|--------|------|--------------|
| RDP Brute Force | Hydra | T1110 – Brute Force |
| Credential Attack | Hydra | T1078 – Valid Accounts |

---

## Splunk Queries Used
```splunk
index=main source="WinEventLog:Security" EventCode=4624
index=main source="WinEventLog:Security" EventCode=4625
index=main source="WinEventLog:Security" EventCode=4688
index=main source="WinEventLog:Security" EventCode=4625 | stats count by Workstation_Name | sort -count
index=main source="WinEventLog:Security" EventCode=4625 | timechart count span=1m
```

---

## Tools Used

| Tool | Version | Purpose |
|------|---------|---------|
| Windows Server 2022 | Build 20348 | Target machine |
| Splunk Enterprise | 10.2.1 | SIEM platform |
| Kali Linux | 2025.2 | Attacker machine |
| Hydra | 9.5 | Brute force simulation |

---

## Author

**Muhammed Anshad**
Certified SOC Analyst (CSA) – EC-Council
[LinkedIn](https://www.linkedin.com/in/muhemmed-a501a0)
[GitHub](https://github.com/anshadshanu)
