# Atomic Red Team (ATT&CK Technique Simulation)
This section covers the use of Atomic Red Team to execute small, controlled tests mapped directly to the MITRE ATT&CK framework. After manually generating telemetry using MSFvenom and network scans, Atomic Red Team helps produce structured, repeatable, and technique-specific events across the lab.

The goal is to simulate adversarial behavior in a safe way and observe how each component in the environment responds.

### Purpose of Atomic Red Team in This Lab

Atomic Red Team allows me to:

- Trigger specific MITRE techniques
- Produce high-quality telemetry for detection engineering
- Validate that Zeek, Suricata, Sysmon, pfSense, and Splunk all record the activity
- Build ATT&CK mappings for my detections
- Compare real attacker behavior (from earlier phases) vs structured attack simulations

This gives me the full picture of what happens, how logs appear, and how Splunk should detect it.

## Installing Atomic Red Team (on Windows)

I installed Atomic Red Team using PowerShell:

```
Set-ExecutionPolicy Bypass -Scope Process -Force
iwr https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1 -UseBasicParsing | iex
Install-AtomicRedTeam -GetAtomics
```

This downloaded the full Atomic test library to:

```
C:\AtomicRedTeam
```

### Running Atomic Tests

I executed specific Atomic tests to generate telemetry for:

✔ Persistence

✔ Execution

✔ Discovery


Example test:

```
Invoke-AtomicTest T1059.001
```

This produces Sysmon events under:

- Process Creation (Event ID 1)
- Script block execution
- Network traffic
- File modifications
- Registry changes

And generates logs readable by:

- Zeek (connection + script detection)

- Suricata (network signatures)

- Windows event logs

- Splunk searches

### T1547.001 – Registry RunKeys / RunOnce

A dedicated simulation was executed for T1547.001, using Atomic Red Team to create persistence items under:

- ``HKCU\Software\Microsoft\Windows\CurrentVersion\Run``

- ``HKLM\Software\Microsoft\Windows\CurrentVersion\RunOnce``

- ``Policies\Explorer\Run``

This resulted in observable actions such as:

- ``calc.exe`` launched from a Run key

- ``notepad.exe`` launched as a persistence payload

- PowerShell execution of hidden scripts

- Registry modifications (Sysmon Event ID 13)

- Process creation from Run keys (Event ID 1)

These artifacts were fully captured in Splunk and validated through queries.

This provided excellent telemetry for building persistence detections.

### Where the Telemetry Appears

Atomic Red Team tests generated logs in:

▪ Splunk

- ``index=sysmon``

- ``index=windows``

- ``index=homelab-detect (Zeek + Suricata)``
 
▪ pfSense

- Port scanning + firewall behaviors

This phase helped verify how well each tool captures ATT&CK techniques.

### MITRE ATT&CK Mapping

Telemetry results from Atomic tests were mapped to MITRE using:

``splunk-verification/queries/mitre-mapping.md``

This forms the basis for the detection engineering portion of the project.

### Outcome of the Atomic Red Team Phase

Running these tests provided:

- ✔ Reliable, structured telemetry

- ✔ ATT&CK-based simulations

- ✔ End-to-end visibility of adversary techniques

- ✔ Detection opportunities for Splunk

- ✔ Stronger understanding of how each sensor reacts

Atomic Red Team balanced the earlier “free-form” attacks by providing high-signal, repeatable scenarios tailored for SOC analysis and practice.
#

# Appendix: : Visual Evidence For AtomicRedTeam

![atomic-redteam](https://github.com/user-attachments/assets/0aee74cd-5f93-480d-bc6d-da8f68fcdd54)

![atomic-redteam1](https://github.com/user-attachments/assets/338f0e6b-76af-470d-bc5b-28811c2c9bc0)

![atomic-redteam2](https://github.com/user-attachments/assets/456a33a5-6022-4d27-a5ad-0b42bf50002e)



































