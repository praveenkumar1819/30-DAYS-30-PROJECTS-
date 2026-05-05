# 🛡️ SIEM-Based SOC Lab using Splunk

## 📌 Overview

This project demonstrates a hands-on Security Operations Center (SOC) lab built using Splunk SIEM. The lab simulates real-world security monitoring by collecting Windows event logs, analyzing authentication activity, and detecting potential brute-force attacks.


## 🎯 Objectives

* Configure Splunk SIEM on Ubuntu Server
* Forward Windows logs using Universal Forwarder
* Analyze authentication logs (Event IDs 4624 & 4625)
* Simulate failed login attempts and brute-force behavior
* Develop detection queries using SPL (Search Processing Language)


## 🧱 Architecture

**Lab Setup:**

* 🖥️ Windows Machine → Log Source (Employee)
* 🐉 Kali Linux → Attack Simulation (Attacker)
* 🧠 Ubuntu Server → Splunk SIEM (Admin)

### 📊 Data Flow

```
Windows → Universal Forwarder → Splunk Indexer → Search Head → Analyst
```

---

## ⚙️ Technologies Used

* Splunk Enterprise
* Splunk Universal Forwarder
* Windows Event Logs
* Kali Linux
* Ubuntu Server
* Wireshark (optional network analysis)

---

## 🔄 Implementation Steps

### 1. Splunk Setup

* Installed Splunk on Ubuntu Server
* Accessed via: `http://<server-ip>:8000`
* Enabled receiving port: `9997`

📸 *Add Screenshot: Splunk Dashboard*

---

### 2. Forwarder Configuration (Windows)

* Installed Splunk Universal Forwarder
* Configured to send logs to Splunk server
* Enabled log inputs:

  * Security Logs
  * System Logs
  * 
<img width="715" height="201" alt="Screenshot 2026-05-05 155000" src="https://github.com/user-attachments/assets/3f962803-7a46-4cb8-abd4-6af8eb6268d7" />

### 3. Log Ingestion Verification

* Verified logs using:

```
index=*
```

<img width="1919" height="960" alt="Screenshot 2026-05-05 155045" src="https://github.com/user-attachments/assets/53939fd9-6a3a-4d6a-b67f-3f755bea8d88" />


---

## 🔐 Use Case 1: Successful Login Monitoring

### Event ID: 4624

* Represents successful login events
* Used to understand normal system behavior

<img width="1919" height="960" alt="Screenshot 2026-05-05 155045" src="https://github.com/user-attachments/assets/53939fd9-6a3a-4d6a-b67f-3f755bea8d88" />

---

## ❌ Use Case 2: Failed Login Detection

### Event ID: 4625

* Indicates failed login attempts
* Helps detect unauthorized access attempts

### SPL Query:

```spl
index=* EventCode=4625
```

<img width="1917" height="973" alt="Screenshot 2026-05-05 153042" src="https://github.com/user-attachments/assets/299ee7e0-7b8e-4650-844e-c501ba5ee614" />

---

## 🚨 Use Case 3: Brute Force Detection

### Scenario:

Multiple failed login attempts within short time

### Detection Query:

```spl
index=* EventCode=4625
| stats count by Account_Name, Source_Network_Address
| where count > 5
```

### Time-based Detection:

```spl
index=* EventCode=4625
| bin _time span=1m
| stats count by _time, Account_Name
| where count > 5
```

<img width="1913" height="958" alt="Screenshot 2026-05-05 160240" src="https://github.com/user-attachments/assets/64d868fb-3535-486c-a549-97ea77b573f3" />
<img width="1919" height="970" alt="Screenshot 2026-05-05 160400" src="https://github.com/user-attachments/assets/81bd5b6c-1635-4440-9ca2-50b91f31aa50" />


---

## 🧠 Analysis & Findings

* Event ID 4624 (Logon Type 5) indicates system/service activity
* Event ID 4625 highlights authentication failures
* Multiple 4625 events from same source indicate brute-force attempts
* Splunk effectively correlates logs to identify suspicious patterns

---

## 🎫 Sample Incident Report

```
Incident: Multiple Failed Login Attempts  
User: royal-admin  
Source IP: 192.168.x.x  
Attempts: 6 within 1 minute  
Conclusion: Possible brute-force activity detected  
```

---

## 📊 Key Learnings

* Understanding SIEM architecture and workflow
* Hands-on experience with log ingestion and parsing
* Writing SPL queries for detection
* Differentiating normal vs malicious activity
* Basic SOC analyst workflow and incident investigation

---

## 🚀 Future Improvements

* Create real-time alerts in Splunk
* Build dashboards for visualization
* Integrate additional log sources (Linux, Firewall)
* Simulate advanced attack scenarios


