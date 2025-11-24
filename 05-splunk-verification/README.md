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

![invoices-creation](https://github.com/user-attachments/assets/2c19d94d-8cfa-489f-8d7b-0e1481b37fa6)

![metasploit-C2](https://github.com/user-attachments/assets/4e95e03e-819b-46e4-8ba7-a8ff24c8b040)

![nmap-image](https://github.com/user-attachments/assets/7468f2e5-9cca-4311-b341-70e47391e059)

![newlocaluser-image](https://github.com/user-attachments/assets/f264bb5f-a2e2-4d03-8b07-6222f55d7599)

![registryrun-image](https://github.com/user-attachments/assets/3fac6047-0584-4673-ac1f-e2bc88f3d992)

![registrykey-image](https://github.com/user-attachments/assets/25eff9ca-cf0c-4471-ab4b-fe11dbea0193)


































