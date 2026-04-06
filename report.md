# Executive Summary
This project focused on troubleshooting a network connectivity issue between a Linux and Windows 11 VM that were separated across pfSense-managed internal networks. Earlier testing already confirmed that both systems could communicate normally, and Wireshark packet captures showed expected ICMP request-and-reply traffic moving without problems. After that, a firewall rule change was made on the LAN side, and that is where things started to break.

Once the change was introduced, traffic started failing only when it was initiated from the Linux LAN side, while the rest of the network communication continued working as expected. That detail is critical because it showed this was not a full network outage or a major routing failure, but a directional configuration error. 

Additional testing, along with packet analysis, showed the issue was specifically tied to traffic entering through the LAN interface. That suggested a firewall policy problem inside pfSense rather than a broader connectivity issue. After restoring the LAN allow rule, normal communication returned, and the connectivity issue was resolved.
# Incident Overview
The lab environment was built using a pfSense firewall instance, a Linux VM, and a Windows 11 VM placed on separate internal subnets to simulate segmented network traffic. 

<img width="1078" height="591" alt="01-pfsense-segmented-network-configuration" src="https://github.com/user-attachments/assets/fa8c1034-cd0d-4b05-acc5-49cda9634558" />
<img width="1271" height="789" alt="02-Linux-to-pfsense-configuration-proof" src="https://github.com/user-attachments/assets/29b2b4d0-cc7c-427e-bfd8-61b9fb74ac78" />
<img width="1101" height="610" alt="03-windows-to-pfsense-configuration-proof" src="https://github.com/user-attachments/assets/9d2b86c5-c9f1-48f0-83b1-21c0238aaca4" />

Initial testing confirmed that the Linux and Windows systems could communicate successfully across pfSense under the original firewall configurations.

<img width="1269" height="791" alt="06-packet-level-linux-to-windows-connectivity-proof" src="https://github.com/user-attachments/assets/5c36f6e2-42bc-42e7-b7fe-0730fa29dcc7" />

A change was later made to the pfSense LAN firewall rule, and that is what ended up breaking Linux-initiated communication, while Windows-to-Linux traffic still worked normally. That made it clear the issue was directional, not a full network failure, random misconfiguration, or simple port-blocking problem. Then, the investigation centered on comparing successful traffic against failed packet behavior, reviewing the impact of the firewall policy, and confirming that the LAN-side rule change was the real cause.
# Detection and Initial Observation
The issue was first identified after a pfSense LAN firewall rule change caused Linux-to-Windows communication to stop working. Ping testing further backed this discovery, as the Linux VM showed that traffic was no longer reaching the Windows system, despite both hosts previously communicating successfully. 

<img width="730" height="463" alt="09-Linux-connection-error-post-fw-change" src="https://github.com/user-attachments/assets/4989ac57-77df-4389-8217-13f61e421828" />

Packet captures in Wireshark showed ICMP echo requests leaving the Linux VM without receiving replies, confirming a loss of return traffic. 

<img width="1273" height="791" alt="10-packet-level-proof-of-failed-linux-to-windows-attempt" src="https://github.com/user-attachments/assets/b0e05019-a2c7-4c72-9037-68a3d027446a" />

However, additional testing from the Windows VM to the Linux VM still succeeded, showing that the issue was not a full network outage, seeing as successful connections could still be made from a connected machine. This behavior suggested the issue is a directional firewall policy problem tied specifically to the Linux-side LAN interface.

<img width="2353" height="757" alt="11-proof-of-success-from-windows-to-linux-attempt" src="https://github.com/user-attachments/assets/02019564-ba66-4bdc-a144-5241e22577ee" />

Project Steps
- Verified Linux IP addressing, subnet, and default gateway to confirm correct network configuration on the LAN segment.
- Verified Windows IP addressing, subnet, and default gateway to confirm correct network configuration on the OPT1 segment.
- Tested initial connectivity from the Linux VM to the Windows VM using ping and Wireshark packet capture and confirmed successful ICMP communication.
- Investigated pfSense firewall rules and found the active LAN IPv4 allow rule responsible for allowing Linux-initiated outbound traffic.
- Disabled the LAN IPv4 allow rule to introduce a controlled connectivity failure.
- Re-tested Linux-to-Windows communication and confirmed that traffic now fails.
- Captured failed ICMP traffic in Wireshark and observed echo requests without returned replies.
- Performed Windows to Linux testing and confirmed that traffic in that direction still succeeded, hinting at a directional rule misconfiguration.
- Re-enabled the LAN IPv4 allow rule in pfSense and validated that Linux-initiated communication was restored.
  
<img width="1269" height="750" alt="12-fix-firewall-rule" src="https://github.com/user-attachments/assets/d5f21d98-becf-4526-8aeb-7de3e4b92a25" />
<img width="1297" height="817" alt="13-connectivity-restored-linux-to-windows" src="https://github.com/user-attachments/assets/7e68e0b1-c659-45d7-bc91-a1560cd3981e" />

# Evidence and Findings
Baseline testing showed that both the Linux and Windows systems were set up correctly from the start and could communicate normally across pfSense while the original firewall policy was still in place. Ping tests from Linux to Windows worked as expected, and Wireshark backed that up with normal ICMP echo requests and echo replies moving between both hosts. However, once the pfSense LAN IPv4 allow rule was disabled, Linux-initiated traffic started failing. Wireshark still showed the outbound ICMP requests leaving the Linux side, but no replies were coming back, which is a strong sign that the traffic was being sent but not received

Additional testing showed that Windows-to-Linux traffic still worked and produced normal request-and-reply behavior in Wireshark, proving the issue was directional rather than a full network outage. These findings showed that pfSense was enforcing policy based on the interface where traffic entered. Therefore, with the LAN-side allow rule turned off, traffic coming from the Linux segment was blocked while Windows-originated traffic entering through OPT1 still passed. In the end, the root cause was the disabled LAN IPv4 allow rule, which prevented outbound communication initiated from the Linux network.
# Lessons Learned and Recommendations
- A single firewall rule change can create a directional issue without causing a full network outage.
- Connectivity testing should be performed in all directions to confirm whether an issue is partial, one-way, or holistic.
- Firewall changes should be reviewed carefully before being applied, especially on the interface where traffic enters.
- Pre-change and post-change connectivity tests should be used to quickly identify unexpected traffic disruption before distributing to active environments
- Baseline packet captures and known-good rule documentation should be implemented to make future troubleshooting faster and more accurate.
