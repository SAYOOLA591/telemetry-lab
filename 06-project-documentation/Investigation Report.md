# Investigation Report

## Findings
During this lab exercise, several controlled attack scenarios were executed to generate telemetry across the environment. The findings below summarise the malicious behaviours observed. 

### User Accounts Involved: 

Both the sayop user account and the SYSTEM account were involved at different stages of the simulation.

### Payload Deployment: 

A disguised payload, invoices.docx.exe, was created on the attacker machine (192.168.1.250) and downloaded onto the victim host (192.168.1.100) using Microsoft Edge before being executed.

### C2 Behaviour: 

The malicious payload initiated outbound communication to the attacker system, simulating Command-and-Control (C2) behaviour. This allowed demonstration of post-exploitation behaviours, including process execution and file interaction.

### Network Reconnaissance: 

Nmap scans were executed from the attacker to enumerate hosts and open ports on the subnet (192.168.1.0/24).

### Atomic Red Team Simulations: 

To enrich telemetry, several MITRE ATT&CK simulations were run:

T1136.001 – Create Local Account: A new local user NewLocalUser was created to simulate persistence and account manipulation.

T1547.001 – Registry RunKeys / RunOnce (Persistence)

persistence techniques were executed using Atomic Red Team, modifying several registry locations:

•	HKLM\Software\Microsoft\Windows\CurrentVersion\RunOnce\NextRun

•	HKCU\Software\Microsoft\Windows\CurrentVersion\Run\atomicTest

•	HKCU\Software\Microsoft\Windows\CurrentVersion\Run\Updater

•	Policies\Explorer\Run\atomicTest

These changes resulted in the execution of calc.exe, notepad.exe, and a downloaded PowerShell script, all automatically launched via RunKey persistence mechanisms.
This produced clean telemetry through Sysmon Event ID 13 (Registry Value Set) and Event ID 1 (Process Create).

# Investigation Summary
On 2025-11-20 at 14:10:09 UTC, the user "sayop" executed the file "invoices.docx.exe," which initiated immediate command-and-control (C2) callbacks to the attacker's machine. Atomic Red Team tests were conducted to create a new local account and establish persistence through Registry RunKeys. These tests generate multiple telnet sources for detection engineering and produce observable events across process creation, network activity, registry modifications, and account changes, effectively simulating a complete attack chain.

#

### Incident Context (WHO, WHAT, WHEN, WHERE, WHY, HOW)

### WHO: Host 192.168.1.100 (DESKTOP-MTRUFTL), user sayop

### WHAT:

•	Downloaded and executed invoices.docx.exe

•	Created a new local user using Atomics

•	Added RunKey persistence entries launching calc.exe, notepad.exe, and a discovery script

### WHEN: Events occurred between 2025-11-20 and 2025-11-23

### WHERE: Windows host DESKTOP-MTRUFTL (192.168.1.100)

### WHY: To intentionally generate telemetry for detection testing within the homelab.

### HOW:

•	Malicious payloads download and execution

•	PowerShell-based persistence tests via Atomic Red Team

•	Registry modification–based persistence (RunKeys)

•	C2 callbacks

•	Reconnaissance scans
#

### Recommendations / Lessons Learned
1, Building Baseline Detections: I want to use the Telemetry from this simulation to craft detections for suspicious process creation, unusual outbound network connections, local account creation events, registry RunKey persistence, and lastly reconnaissance activity for (Nmap / port scanning). These detections provide the basic environmental monitoring coverage.

2, Telemetry Custom Detection Content: For each technique, I simulated to produce Splunk correlation searches, developed Suricata signatures where applicable, added Zeek scripting enrichment, and lastly documented why each detection rule matters (attack impact + how it strengthens SOC visibility). This ensures that each telemetry source contributes to a stronger and more meaningful detection capability.

#

### Timeline Analysis

•	2025-11-20 14:10:09 UTC – invoices.docx.exe downloaded via Microsoft Edge into the Downloads directory. | Execution

•	2025-11-20 14:10:10 UTC – Payload initiated outbound connection to attacker machine. | Command and Control

•	2025-11-20 18:02:11 UTC – PowerShell executed whoami.exe. | Discovery

•	2025-11-20 18:02:13 UTC – PowerShell spawned cmd.exe to run net user /add. | Persistence

•	2025-11-20 18:02:19 UTC – PowerShell downloaded a GitHub script and executed CreateNewLocalAdmin_ART.ps1 to create NewLocalUser. | Persistence

•	2025-11-20 18:02:23 UTC – net.exe executed to finish creation of NewLocalUser. | Persistence

•	2025-11-23 18:47:24 UTC – PowerShell executed a RedCanary script that set a RunOnce\NextRun value for persistence. | Persistence (T1547.001)

•	2025-11-23 18:47:34 UTC – calc.exe added to Policies\Explorer\Run registry key. | Persistence (T1547.001)

•	2025-11-23 20:42:18 UTC – notepad.exe created as a persistence value Updater under Run. | Persistence (T1547.001)

