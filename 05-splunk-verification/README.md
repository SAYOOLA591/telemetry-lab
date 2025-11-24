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




