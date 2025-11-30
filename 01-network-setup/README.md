# Network Setup

This section covers how I configured the foundational network environment required for the telemetry lab. The goal of this phase was to create a controlled, realistic network where normal user behavior and attacker activity can be observed, logged, and forwarded into Splunk for analysis.
#

# Kali Linux Attack Box – Static Network Configuration

To perform controlled attack simulations, I assigned a static IP to my Kali machine:

| Setting         | Value            |
| --------------- | ---------------- |
| **IP Address**  | 192.168.1.250    |
| **Subnet Mask** | /24              |
| **Gateway**     | pfSense firewall |
| **DNS**         | pfSense firewall |

### This ensures:

predictable callbacks for payloads

stable connectivity with Metasploit

consistent network identity for Zeek/Suricata logs
#

# Why This Network Setup Matters

This structure allowed me to generate realistic telemetry for:

Payload execution

C2 callbacks

Port scanning

Network probing

MITRE ATT&CK techniques

IDS signature hits

Lateral movement attempts

And have every log source correlated together in Splunk.
#

# Appendix: : Visual Evidence For Network Setup

### Kali Network Setting

![kali-setting](https://github.com/user-attachments/assets/4a2d1d4b-6fc8-40af-9a66-460678dc5161)

![kali-ipv4](https://github.com/user-attachments/assets/009cc589-f996-412e-a689-4ebc2113edf3)

![kali-ipv4-confirmation](https://github.com/user-attachments/assets/8dec8a84-8df9-416c-bb47-79a8b2ec5647)





























