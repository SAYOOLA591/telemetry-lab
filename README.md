# Telemetry-lab

# Introduction

To build a realistic SOC environment, I needed to generate meaningful telemetry across the lab network. The goal is to simulate real-world activity, both normal and malicious, so that the Splunk instance would receive diverse logs from pfSense, Zeek, Suricata, Windows, and Active Directory. This section will guide us through the foundational steps I took to generate traffic, execute attacks, and monitor everything using Splunk.

# Project Structure

```
telemetry-lab/
│
├── 01-network-setup/         → Lab network mapping, Kali setup, pfSense notes
├── 02-payload-generation/    → MSFvenom payloads, hosting scripts, screenshots
├── 03-attack-simulation/     → Exploitation, Nmap scanning, results
├── 04-atomic-red-team/       → ATT&CK simulations + PowerShell runner
├── 05-splunk-verification/   → Detection queries, MITRE mapping, log examples
└── 06-project-documentation/ → Telemetry foundation report + summaries
```
# What This Lab Demonstrates

- ✔ Realistic attacker behaviour
- ✔ Telemetry generation across multiple sensors
- ✔ Detection Engineering
- ✔ MITRE ATT&CK Simulation

# Tools & Technologies Used

```
| Category               | Tools                                       |
| ---------------------- | ------------------------------------------- |
| **Endpoint telemetry** | Sysmon, Windows Event Logs                  |
| **Network telemetry**  | Zeek, Suricata                              |
| **Firewall**           | pfSense + Squid                             |
| **Attack simulation**  | Kali Linux, MSFvenom, Nmap, Atomic Red Team |
| **SIEM**               | Splunk Enterprise + Universal Forwarder     |
| **Scripting**          | PowerShell, Bash                            |
| **Documentation**      | Markdown, Word report                       |
```

# Project Documentation

### Full report available under:

``06-project-documentation/final-summary.md``

### Documentation includes:

- Lab overview

- Attack timeline

- Telemetry walkthrough

- Detection justification

- Lessons learned & recommendations
