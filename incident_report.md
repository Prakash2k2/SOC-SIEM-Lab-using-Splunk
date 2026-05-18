# Incident Report

## 📌 Incident Title

Brute Force Attack Detection using Splunk SIEM

---

#  Incident Information

| Field | Value |
|---|---|
| Incident ID | SOC-INC-001 |
| Incident Type | Brute Force Authentication Attack |
| Severity | High |
| Affected Service | Remote Desktop Protocol (RDP) |
| Log Source | Windows Security Logs |
| Detection Date | 18-May-2026 |
| Detection Method | Splunk SIEM Alert |
| Status | Investigated |
| Analyst | Prakash |

---

#  Executive Summary

A brute-force attack was simulated against a Windows 10 system using Hydra from an Ubuntu Linux machine. The attack targeted the Remote Desktop Protocol (RDP) service in an attempt to gain unauthorized access using multiple password attempts.

Windows Security logs generated EventCode 4625 entries for failed login attempts. These logs were collected by Splunk Universal Forwarder and analyzed in Splunk SIEM.

The SIEM successfully detected repeated failed authentication attempts originating from the same source IP address and generated a brute-force detection alert.

---

#  Indicators of Compromise (IOCs)

| Indicator | Value |
|---|---|
| Source IP Address | 192.168.29.150 |
| Target System | Windows 10 |
| Target User | Ghost |
| Event ID | 4625 |
| Attack Method | Brute Force |

---

# Event IDs Observed

| Event ID | Description |
|---|---|
| 4625 | Failed Login Attempt |

---

# Detection Query

```spl
source="WinEventLog:Security" EventCode=4625
| bucket span=5m _time
| stats count by Source_Network_Address,_time
| where count > 5
```

---

#  Investigation Findings

The investigation identified repeated failed authentication attempts against the Windows 10 RDP service originating from the Ubuntu attacker machine.

The attack generated multiple EventCode 4625 logs within a short time period, matching brute-force attack behavior.

Splunk SIEM successfully:
- Collected Windows Security logs
- Detected suspicious authentication activity
- Generated brute-force alerts
- Visualized attacker activity through dashboards

---

#  Impact Assessment

Potential risks associated with this attack include:

- Unauthorized system access
- Credential compromise
- Lateral movement
- Privilege escalation
- Data exposure

The attack was conducted in a controlled lab environment and no actual compromise occurred.

---

#  Mitigation Recommendations

The following security measures are recommended:

## Immediate Actions

- Block suspicious IP addresses
- Disable unused RDP services
- Monitor failed login activity

---

## Security Hardening

- Enable account lockout policy
- Enforce strong password policies
- Restrict RDP access
- Use firewall rules
- Enable multi-factor authentication

---

## Monitoring Improvements

- Configure additional SIEM alerts
- Monitor successful logins after repeated failures
- Implement endpoint monitoring
- Integrate Sysmon logs

---

#  MITRE ATT&CK Mapping

| Technique ID | Technique |
|---|---|
| T1110 | Brute Force |
| T1021 | Remote Services |

---

#  Dashboard Overview

The SOC Monitoring Dashboard contains:

| Dashboard Panel | Description |
|---|---|
| Failed Login Timeline | Shows authentication attack activity |
| Top Attacker IPs | Identifies attacking systems |
| Most Targeted Accounts | Displays targeted usernames |
| Brute Force Detection Table | Shows brute-force events |

---

#  SOC Workflow Demonstrated

This project demonstrates the following SOC operations:

- Log Collection
- Threat Monitoring
- Event Correlation
- Alert Engineering
- Incident Detection
- Incident Investigation
- Incident Documentation
- Security Analysis

---

#  Conclusion

The SOC SIEM Lab successfully demonstrated how Splunk SIEM can be used to detect and investigate brute-force authentication attacks against Windows systems.

The project provided hands-on experience in:

- SIEM configuration
- Windows log monitoring
- Threat detection
- Alert management
- SOC investigation workflow
- Incident response documentation

The simulated attack was successfully detected and analyzed using Splunk dashboards, correlation searches, and alerting mechanisms.

---

# 👨‍💻SOC Analyst

Prakash
