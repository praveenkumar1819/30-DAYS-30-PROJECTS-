# Wazuh SSH Brute Force Detection & Telegram Alerting Lab

## Project Overview

This project demonstrates a complete SOC (Security Operations Center) mini-lab using Wazuh for SSH brute-force detection, alert escalation, and Telegram-based incident notification.

The lab simulates a real-world brute-force attack from a Kali Linux attacker machine targeting an Ubuntu server running Wazuh Manager.

After detecting multiple failed SSH login attempts, Wazuh:

* Generates security alerts
* Correlates brute-force activity
* Escalates the detection using a custom rule
* Sends real-time Telegram notifications

---

# Objectives

* Setup Wazuh SIEM environment
* Monitor Linux authentication logs
* Simulate SSH brute-force attacks
* Create custom Wazuh correlation rules
* Generate alerts on Wazuh dashboard
* Integrate Telegram alert notifications
* Understand SOC detection workflows

---

# Lab Architecture

```text
Kali Linux VM
     ↓
SSH Brute Force Attack (Hydra)
     ↓
Ubuntu Server VM (Wazuh Manager)
     ↓
auth.log generates failed login events
     ↓
Wazuh built-in SSH rules detect brute force
     ↓
Custom Rule ID 100700 escalates alert
     ↓
Telegram integration script executes
     ↓
Real-time mobile notification received
```

---

# Environment Setup

| Component        | Role                |
| ---------------- | ------------------- |
| Ubuntu Server VM | Wazuh Manager       |
| Kali Linux VM    | Attacker Machine    |
| Windows 11 Host  | Virtualization Host |
| Wazuh Dashboard  | SIEM Visualization  |
| Telegram Bot     | Alert Notification  |

---

# Technologies Used

* Wazuh
* Ubuntu Server
* Kali Linux
* Hydra
* Telegram Bot API
* Python3
* Linux Authentication Logs
* SSH

---

# Step 1 — Wazuh Setup

Wazuh Manager was installed on Ubuntu Server VM.

The Wazuh dashboard was used for:

* Security event monitoring
* Rule verification
* Alert analysis
* Brute-force detection visualization

---

# Step 2 — Linux Authentication Log Monitoring

The following log file was monitored:

```bash
/var/log/auth.log
```

Added inside:

```bash
/var/ossec/etc/ossec.conf
```

Configuration:

```xml
<localfile>
  <location>/var/log/auth.log</location>
  <log_format>syslog</log_format>
</localfile>
```

---

# Step 3 — SSH Brute Force Simulation

Hydra was used from Kali Linux to simulate SSH brute-force attacks.

Command used:

```bash
hydra -l fakeuser -P pass.txt ssh://TARGET-IP
```

This generated:

* Failed SSH authentication attempts
* Wazuh authentication alerts
* Built-in brute-force detection events

---

# Step 4 — Wazuh Rule Analysis

Observed built-in Wazuh SSH rules:

| Rule ID | Description               |
| ------- | ------------------------- |
| 5710    | SSH invalid user attempt  |
| 5712    | SSH brute force detection |
| 5503    | PAM login failed          |

The dashboard confirmed successful brute-force correlation.

---

# Step 5 — Custom Escalation Rule

Custom rule created in:

```bash
/var/ossec/etc/rules/local_rules.xml
```

Rule:

```xml
<group name="local,telegram_alerts">

  <rule id="100700"
        level="15">

    <if_sid>5712</if_sid>

    <description>
      Telegram alert: SSH brute force detected
    </description>

    <group>
      authentication_failed,
      ssh,
      brute_force
    </group>

    <mitre>
      <id>T1110</id>
    </mitre>

  </rule>

</group>
```

Purpose:

* Escalate Wazuh brute-force detection
* Trigger high-severity custom alert
* Execute Telegram integration

---

# Step 6 — Telegram Integration

A custom Python integration script was created:

```bash
/var/ossec/integrations/custom-telegram
```

The script:

* Receives Wazuh alert JSON
* Extracts alert information
* Sends real-time Telegram notifications

Wazuh integration configuration:

```xml
<integration>
  <name>custom-telegram</name>
  <level>10</level>
  <alert_format>json</alert_format>
</integration>
```

---

# Step 7 — Alert Workflow

```text
Hydra Attack
    ↓
Ubuntu auth.log
    ↓
Wazuh Built-in SSH Detection
    ↓
Custom Rule 100700
    ↓
Telegram Integration
    ↓
Phone Notification
```

---

# Detection Results

The following detections were successfully generated:

* SSH invalid user detection
* PAM login failures
* SSH brute-force attack detection
* Custom high-severity escalation alert
* Real-time Telegram notification

---

# MITRE ATT&CK Mapping

| Technique ID | Technique         |
| ------------ | ----------------- |
| T1110        | Brute Force       |
| T1110.001    | Password Guessing |

---

# Skills Learned

## SIEM & SOC Skills

* Wazuh administration
* SIEM alert analysis
* Threat detection
* Security event monitoring
* Correlation rule engineering
* Alert escalation
* Detection tuning

## Linux Skills

* Authentication log analysis
* SSH monitoring
* Linux service management
* Log investigation

## Security Skills

* Brute-force attack simulation
* SSH attack detection
* Threat monitoring
* Incident investigation

## Automation Skills

* Telegram API integration
* Python automation
* Real-time alerting

---

# Challenges Faced

* Incorrect rule SID mapping
* Wazuh custom integration naming issues
* XML configuration debugging
* Telegram bot integration troubleshooting
* Correlation rule tuning

---

# Future Improvements

* Active response IP blocking
* Email alert integration
* GeoIP attacker tracking
* Threat intelligence integration
* Dashboard visualization improvements
* Suricata integration
* SOAR automation workflows

---

# Conclusion

This project successfully demonstrates a complete beginner SOC detection engineering workflow using Wazuh.

The lab includes:

* Threat simulation
* Security monitoring
* Correlation rule creation
* Custom alert escalation
* Real-time mobile notifications

This project provides hands-on experience in:

* SIEM operations
* Linux security monitoring
* Threat detection engineering
* Incident alerting workflows
* Blue Team security operatios
