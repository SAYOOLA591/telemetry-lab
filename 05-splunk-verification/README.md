# Splunk Verification
This folder contains the steps I used to confirm that all telemetry sources in the lab are properly forwarding logs into Splunk. The goal is to verify visibility across the entire environment before building detections.

## Purpose of This Step

The main objective is to validate:

- Zeek logs are arriving and correctly parsed
- Suricata alerts appear in real time
- Windows + Sysmon events are indexed
- pfSense firewall logs are received
- Timestamps and source types look correct
- Attack activity from earlier phases is visible

This ensures the lab is producing high-quality data for SOC analysis.

## Quick Verification Queries

### Zeek
``index=homelab-detect sourcetype=bro:json``

### Suricata

``index=homelab-detect sourcetype=suricata``

### Sysmon

``index=endpoint EventCode=1``

### Windows

``index=endpoint host=WIN10*``

### pfSense

``index=pfsense*``

These basic checks confirm whether each tool is sending logs correctly.

### What I Verified

- Payload download events show in Zeek/Suricata
- Meterpreter callback appears as outbound traffic
- Process creation & network connections logged by Sysmon
- Nmap scans flagged by Suricata + visible in Zeek
- pfSense firewall flows match the attack behavior

Once everything was visible in Splunk, the lab was ready for building use cases and detections.
#

# Appendix: : Visual Evidence For Detection Engineering

![invoices-creation](https://github.com/user-attachments/assets/b0ac5bba-b025-4dd8-9301-61f2a59bbf73)

![metasploit-C2](https://github.com/user-attachments/assets/879a1a4f-313d-41cc-80fc-f89aa2f046be)

![nmap-image](https://github.com/user-attachments/assets/94728fff-76f4-41c9-a7c8-a7f4326e1eaf)

![newlocaluser-image](https://github.com/user-attachments/assets/4af2590f-ccab-49aa-aa1f-4239c9e4e46d)

![registryrun-image](https://github.com/user-attachments/assets/390f4174-e522-48b8-9072-faf6581a787c)

![registrykey-image](https://github.com/user-attachments/assets/2fa86fc3-4801-4443-82a6-351e94a3a00d)


































