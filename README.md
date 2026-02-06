<p align="center">
  <img src="./screenshots/banner.png"
       alt="Linux SSH Attack Monitoring — Splunk SIEM"
       width="1000"
       height="400"/>
</p>



# 🛡️ Linux SSH Attack Monitoring — Splunk SIEM

Real-world SOC lab simulating SSH brute-force attacks and detecting authentication threats using Splunk dashboards and SPL analytics.

---

![Splunk](https://img.shields.io/badge/SIEM-Splunk-green)
![Linux](https://img.shields.io/badge/OS-Linux-blue)
![SOC](https://img.shields.io/badge/Role-SOC%20Analyst-red)
![Status](https://img.shields.io/badge/Project-Completed-success)

---

## 📌 Project Overview

This project demonstrates a Security Operations Center (SOC) detection lab where SSH authentication attacks are simulated from an attacker machine and monitored on a Linux victim system using Splunk Enterprise.

Authentication logs are ingested, analyzed, and visualized to detect brute-force attempts and unauthorized access.

---

## 🔍 Key Features

• Failed Login Detection
• Successful Login Monitoring
• Attacker IP Identification
• Targeted Username Analysis
• Authentication Timeline Visualization
• Interactive SOC Dashboard

---

## 🧪 Lab Stack

* Kali Linux (Attacker)
* Ubuntu Linux (Victim)
* Splunk Enterprise SIEM
* Linux Auth Logs (`/var/log/auth.log`)
* VirtualBox Lab Environment


# Linux SSH Attack Monitoring — Splunk SIEM Lab

## 📌 Project Overview

This project demonstrates a real-world **SOC (Security Operations Center) detection lab** built using **Splunk SIEM** to monitor and analyze SSH authentication activity on a Linux victim machine.

The lab simulates attacker behavior from Kali Linux and detects it on an Ubuntu system using log ingestion, SPL queries, and security dashboards.

---

## 🧪 Lab Architecture

```
Kali Linux (Attacker)
        │
        │  SSH Brute Force / Login Attempts
        ▼
Ubuntu Linux (Victim)
        │
        │  /var/log/auth.log
        ▼
Splunk Enterprise (SIEM)
        │
        ▼
Dashboards + Detection Analytics
```

---
![image](https://github.com/NATTOMR/Linux-SSH-Attack-Monitoring-Splunk-SIEM-Lab/blob/main/Lab%20Architecture.jpg)

<p align="center">
  <img src="./screenshots/Architecture.jpg"
       alt="Linux SSH Attack Monitoring — Splunk SIEM"
       width="500"
       height="200"/>
</p>

## 🎯 Objectives

* Simulate SSH attacks from attacker machine
* Monitor authentication logs on victim machine
* Detect failed & successful logins
* Identify attacker IP address
* Analyze targeted usernames
* Build SOC-style security dashboards

---

## 🖥️ Lab Environment

| Component        | Technology              |
| ---------------- | ----------------------- |
| Attacker Machine | Kali Linux              |
| Victim Machine   | Ubuntu 24.04            |
| SIEM Platform    | Splunk Enterprise 9.2.4 |
| Log Source       | /var/log/auth.log       |
| Virtualization   | VirtualBox              |

---

## ⚙️ Step 1 — Splunk Installation

Splunk Enterprise was installed on the Ubuntu victim machine using the `.deb` package.

Access URL:

```
http://<ubuntu-ip>:8000
```

---

## 📂 Step 2 — Log Ingestion

Authentication logs were ingested into Splunk:

**Path Monitored**

```
/var/log/auth.log
```

**Method**

Settings → Add Data → Monitor → Files & Directories

**Sourcetype**

```
linux_secure / linux_source
```

---

## 🧨 Step 3 — Attack Simulation (Kali → Ubuntu)

### Failed Login Attempts

```bash
ssh wronguser@<ubuntu-ip>
ssh fakeuser@<ubuntu-ip>
```

### Successful Login

```bash
ssh natto@<ubuntu-ip>
```

These generated authentication logs inside:

```
/var/log/auth.log
```

---

## 🔍 Step 4 — SPL Detection Queries

### Failed Logins

```spl
source="/var/log/auth.log" "Failed password"
```

### Successful Logins

```spl
source="/var/log/auth.log" "Accepted password"
```

### Combined Authentication Activity

```spl
source="/var/log/auth.log"
("Failed password" OR "Accepted password")
```

---

## 📊 Step 5 — Dashboard Analytics

The following SOC panels were created:

---

### 🔢 Single Value Metrics

**Total Successful Logins**

```spl
source="/var/log/auth.log"
"Accepted password"
| stats count as "Successful Logins"
```

**Total Failed Logins**

```spl
source="/var/log/auth.log"
"Failed password"
| stats count as "Failed Logins"
```

---

### 🌍 Attacker IP Detection

```spl
source="/var/log/auth.log"
"Failed password"
| rex "from (?<src_ip>\d+\.\d+\.\d+\.\d+)"
| stats count by src_ip
| sort -count
```

---

### 👤 Targeted Usernames

```spl
source="/var/log/auth.log"
"Failed password"
| rex "for (invalid user )?(?<user>\w+)"
| stats count by user
| sort -count
```

---

### 📈 Login Timeline

```spl
source="/var/log/auth.log"
("Failed password" OR "Accepted password")
| eval status=if(searchmatch("Accepted password"),"Success","Failed")
| timechart count by status
```

---

### 🧑‍💻 Session Monitoring

```spl
source="/var/log/auth.log"
("session opened" OR "session closed")
```

---

## 📸 Dashboard Screenshots

### SSH SOC Dashboard Overview

![Dashboard Screenshot 1](https://github.com/NATTOMR/Linux-SSH-Attack-Monitoring-Splunk-SIEM-Lab/blob/main/dashboard-1.png)

---

### Authentication Metrics & Attacker Analysis

![Dashboard Screenshot 2](https://github.com/NATTOMR/Linux-SSH-Attack-Monitoring-Splunk-SIEM-Lab/blob/main/dashboard-2.png)

---

## 🧠 Key Security Findings

* Multiple failed login attempts detected
* Successful login observed after attempts
* Attacker IP identified: `192.168.56.104`
* Multiple usernames targeted
* Authentication activity visualized via timeline

---

## 🛡️ SOC Use Cases Demonstrated

* SSH Brute Force Detection
* Credential Stuffing Monitoring
* Account Compromise Identification
* Attacker Attribution
* Login Trend Analysis

---

## 🚀 Skills Gained

* Splunk SIEM Administration
* Log Ingestion & Parsing
* SPL Query Writing
* Dashboard Development
* Threat Detection Engineering
* Linux Log Analysis

---

## 📁 Project Structure

```
linux-ssh-splunk-soc-lab/
│
├── README.md
├── screenshots/
│   ├── dashboard_overview.png
│   └── dashboard_metrics.png
└── queries/
    └── detection_spl.txt
```

---

## 🔮 Future Enhancements

* Geo-location attacker mapping
* Brute force alert automation
* Privilege escalation detection
* File integrity monitoring (auditd)
* Multi-host log forwarding

---

## 👤 Author

**Natto Muni Chakma**  
B.Tech (Computer Science & Engineering)  
Cybersecurity & SIEM Enthusiast  

- 💻 Interested in Security Operations Center (SOC)
- 🔍 Log Analysis & Incident Detection
- 🛡️ SIEM, Splunk, Network & System Security

📫 GitHub: https://github.com/NATTOMR  

---

## 📚 References

1. **Splunk Official Documentation**  
   https://docs.splunk.com

2. **Splunk Search Processing Language (SPL) Reference**  
   https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/WhatsInThisManual

3. **Splunk Dashboard Studio Guide**  
   https://docs.splunk.com/Documentation/Splunk/latest/DashStudio/AboutDashboardStudio

4. **Linux Authentication Logs (auth.log)**  
   https://man7.org/linux/man-pages/man5/syslog.conf.5.html

5. **SOC & SIEM Concepts**  
   https://www.splunk.com/en_us/solutions/siem.html

6. **MITRE ATT&CK Framework (Attack Techniques Reference)**  
   https://attack.mitre.org

---

