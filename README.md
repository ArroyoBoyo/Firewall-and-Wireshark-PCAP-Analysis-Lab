# Firewall-and-Wireshark-PCAP-Analysis-Lab
## 📢 Project Overview
- This project is a network troubleshooting and traffic analysis lab built using pfSense, a Linux VM, and a Windows VM
- The environment was configured with segmented internal networks to simulate traffic flowing through a central firewall.
- A connectivity issue was purposely introduced and investigated using pfSense and Wireshark packet analysis.
- The goal is to identify why traffic failed, use packet-level analysis to validate connection status, determine the root cause, and validate a fix.
## 🛠️ Tools Used
- ✅ pfSense Firewall
- ✅ Linux VM
- ✅ Windows 11 VM
- ✅ Wireshark
## 🚧 Problem Scenario
- A segmented virtual lab environment was built using pfSense, a Linux VM, and a Windows 11 VM.
- Communication between the Linux VM and Windows 11 VM was verified as working under original firewall configurations. 
- Then, a pfSense LAN firewall rule change was introduced, causing communication between the machines to fail.
- Now we must investigate and fix this issue. 
## 📋 Objective
- Capture and review traffic related to the failure
- Check pfSense interfaces and firewall rules
- Use Wireshark to analyze packet behavior
- Identify the root cause
- Apply a fix and confirm connectivity is restored
## 🤝 Resolution Summary
- A pfSense LAN firewall rule change caused communication from the Linux environment to fail
- Ping tests and Wireshark showed Linux traffic was sending requests, but no replies were returned.
- Windows-to-Linux traffic still worked, confirming the issue was directional rather than a full outage.
- This painted more of a LAN-side firewall policy issue, rather than a routing or host misconfiguration
- Restoring the LAN rule brought Linux-initiated communication back online.
# 📷 Screenshots
<img width="1078" height="591" alt="01-pfsense-segmented-network-configuration" src="https://github.com/user-attachments/assets/52c132e2-b59c-49c8-adcf-1bb091e3dfb7" />
<img width="1271" height="789" alt="02-Linux-to-pfsense-configuration-proof" src="https://github.com/user-attachments/assets/e62d89cf-cff8-435f-8df2-eb773e2b4bbd" />
<img width="1101" height="610" alt="03-windows-to-pfsense-configuration-proof" src="https://github.com/user-attachments/assets/ce1c2d46-faa1-4582-9693-11b0fe665a06" />
<img width="386" height="441" alt="04-windows-to-pfsense-configuration-proof-v2" src="https://github.com/user-attachments/assets/3b63ca72-b715-4476-ac15-3e0b4df67e20" />
<img width="1273" height="800" alt="05-linux-to-windows-connectivity-proof" src="https://github.com/user-attachments/assets/1bcabc26-cb10-4e79-a6ae-d8cb3ba070aa" />
<img width="1269" height="791" alt="06-packet-level-linux-to-windows-connectivity-proof" src="https://github.com/user-attachments/assets/987e0299-4449-4c42-bdac-960bcb7922a8" />
<img width="1277" height="754" alt="07-pre-firewall-rule-change" src="https://github.com/user-attachments/assets/3a0cdc87-850b-493f-97ec-c12ccafd2106" />
<img width="1271" height="754" alt="08-post-firewall-rule-change" src="https://github.com/user-attachments/assets/9c567305-345c-4da4-8942-5813d20f2229" />
<img width="730" height="463" alt="09-Linux-connection-error-post-fw-change" src="https://github.com/user-attachments/assets/0cd1cd63-85aa-4520-aae1-a18d6de05d84" />
<img width="1273" height="791" alt="10-packet-level-proof-of-failed-linux-to-windows-attempt" src="https://github.com/user-attachments/assets/25e1c6e2-07da-4ca9-b983-2f4307b57e68" />
<img width="2353" height="757" alt="11-proof-of-success-from-windows-to-linux-attempt" src="https://github.com/user-attachments/assets/732961fd-43ac-4595-8d59-63be395bce22" />
<img width="1269" height="750" alt="12-fix-firewall-rule" src="https://github.com/user-attachments/assets/b0c9865c-724a-4ab1-959f-ed718281a4ae" />
<img width="1297" height="817" alt="13-connectivity-restored-linux-to-windows" src="https://github.com/user-attachments/assets/880f9b9e-c965-40c9-9e04-b88fb19cd19a" />
