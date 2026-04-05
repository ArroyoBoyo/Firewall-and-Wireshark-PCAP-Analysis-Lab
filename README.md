# Firewall-and-Wireshark-PCAP-Analysis-Lab
## 📢 Project Overview
- This project is a network troubleshooting and traffic analysis lab built using pfSense, a Linux VM, and a Windows VM
- The environment was configured with segmented internal networks to simulate traffic flowing through a central firewall.
- A connectivity issue was purposely introduced and investigated using pfSense, Wireshark, and Python-based PCAP review.
- The goal is to identify why traffic failed, determine the root cause, and validate a fix.
## 🛠️ Tools Used
- ✅ pfSense
- ✅ Linux VM
- ✅ Windows 11 VM
- ✅ Wireshark
- ✅ Python
## 🚧 Problem Scenario
- A segmented virtual lab environment was built using pfSense, a Linux VM, and a Windows 11 VM.
- Communication between the Linux VM and Windows 11 VM was verified as working under original firewall configurations. 
- Then, a firewall rule change was introduced on pfSense, causing communication between the machines to fail.
- Now we must investigate and fix this issue. 
## 📋 Objective
- Capture and review traffic related to the failure
- Check pfSense interfaces and firewall rules
- Use Wireshark and Python to analyze packet behavior
- Identify the root cause
- Apply a fix and confirm connectivity is restored
## 🤝 Resolution Summary
temp
# 📷 Screenshots
