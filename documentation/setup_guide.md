# Setup Guide

## Overview

This guide explains how the SOC SIEM Lab environment was configured using Splunk, Windows 10, Ubuntu Linux, and Kali Linux.

---

# Lab Environment

| Machine | Purpose |
|---|---|
| Kali Linux | Splunk SIEM Server |
| Windows 10 | Victim Machine |
| Ubuntu Linux | Attacker Machine |

---

# Network Configuration

All virtual machines were configured using Bridged Adapter networking to allow communication between systems.

---

# Splunk Installation

Splunk Enterprise was installed on Kali Linux.

Access URL:

```text
http://<KALI-IP>:8000
```

---

# Enable Splunk Receiving Port

In Splunk:

Settings → Forwarding and Receiving → Receive Data

Port Enabled:

```text
9997
```

---

# Windows Log Forwarding

Splunk Universal Forwarder was installed on Windows 10.

## inputs.conf

```ini
[WinEventLog://Security]
disabled = 0
```

## outputs.conf

```ini
[tcpout]
defaultGroup = default-autolb-group

[tcpout:default-autolb-group]
server = <KALI-IP>:9997
```

---

# Audit Policy Configuration

Windows audit policies enabled:

- Audit Logon Events
- Audit Credential Validation
- Audit Account Lockout

---

# Verification

Logs were verified in Splunk using:

```spl
source="WinEventLog:Security"
```

---

# Result

Windows Security logs were successfully forwarded to Splunk SIEM.