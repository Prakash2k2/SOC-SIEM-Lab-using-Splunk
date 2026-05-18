# Detection Logic

## Objective

The purpose of this detection logic is to identify brute-force login attempts against Windows systems.

---

# Log Source

Windows Security Logs

Source Type:

```text
WinEventLog:Security
```

---

# Important Event IDs

| Event ID | Description |
|---|---|
| 4625 | Failed Login Attempt |
| 4624 | Successful Login |

---

# Detection Query — Failed Logins

```spl
source="WinEventLog:Security" EventCode=4625
```

This query identifies all failed authentication attempts.

---

# Detection Query — Top Attacker IPs

```spl
source="WinEventLog:Security" EventCode=4625
| stats count by Source_Network_Address
| sort -count
```

Purpose:
- Identify most active attacker IP addresses

---

# Detection Query — Targeted Accounts

```spl
source="WinEventLog:Security" EventCode=4625
| stats count by Account_Name
| sort -count
```

Purpose:
- Detect heavily targeted user accounts

---

# Detection Query — Brute Force Detection

```spl
source="WinEventLog:Security" EventCode=4625
| bucket span=5m _time
| stats count by Source_Network_Address,_time
| where count > 5
```

Detection Logic:
- More than 5 failed logins
- From same IP
- Within 5 minutes

---

# Alert Configuration

| Setting | Value |
|---|---|
| Alert Name | Brute Force Detection |
| Severity | High |
| Trigger | Results > 5 |
| Schedule | Every 5 minutes |

---

# Dashboard Panels

The SOC dashboard contains:

- Failed Login Timeline
- Top Attacker IPs
- Most Targeted Accounts
- Brute Force Detection Table

---

# MITRE ATT&CK Mapping

| Technique ID | Technique |
|---|---|
| T1110 | Brute Force |
| T1021 | Remote Services |

---

# Outcome

The SIEM successfully detected brute-force attack activity and generated visual alerts for SOC monitoring.