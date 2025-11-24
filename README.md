# telemetry-lab

# Introduction

To build a realistic SOC environment, I needed to generate meaningful telemetry across the lab network. The goal is to simulate real-world activity, both normal and malicious, so that the Splunk instance would receive diverse logs from pfSense, Zeek, Suricata, Windows, and Active Directory. This section will guide us through the foundational steps I took to generate traffic, execute attacks, and monitor everything using Splunk.

telemetry-lab/
│
├── 01-network-setup/         → Lab network mapping, Kali setup, pfSense notes
├── 02-payload-generation/    → MSFvenom payloads, hosting scripts, screenshots
├── 03-attack-simulation/     → Exploitation, Nmap scanning, results
├── 04-atomic-red-team/       → ATT&CK simulations + PowerShell runner
├── 05-splunk-verification/   → Detection queries, MITRE mapping, log examples
└── 06-project-documentation/ → Telemetry foundation report + summaries
