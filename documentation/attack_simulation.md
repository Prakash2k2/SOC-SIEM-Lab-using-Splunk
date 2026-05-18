# Attack Simulation

## Objective

The objective of this simulation was to generate brute-force authentication logs for SIEM monitoring and threat detection.

---

# Attack Scenario

A brute-force attack was simulated from Ubuntu Linux against a Windows 10 machine using the Remote Desktop Protocol (RDP).

---

# Tools Used

- Hydra
- Ubuntu Linux
- Windows 10
- RDP Service

---

# Test User Created

The following Windows test account was created:

```cmd
net user Ghost Password123 /add
```

---

# Password List

A custom password list was created containing common weak passwords.

Example:

```text
123456
password
admin
Password123
welcome
```

---

# Hydra Command Used

```bash
hydra -t 1 -W 3 -l Ghost -P password.txt rdp://<WINDOWS-IP>
```

---

# Expected Outcome

The attack generated multiple failed login attempts which created Windows Security EventCode 4625 logs.

---

# Detection Verification

The generated logs were successfully detected in Splunk using:

```spl
source="WinEventLog:Security" EventCode=4625
```

---

# Security Impact

This attack demonstrates how attackers attempt unauthorized access using password guessing techniques.

---

# Mitigation Recommendations

- Enable account lockout policy
- Enforce strong passwords
- Restrict RDP access
- Monitor failed login attempts
- Use multi-factor authentication