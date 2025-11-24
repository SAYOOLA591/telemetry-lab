# Malicious Payload Execution — invoices.docx.exe

### Process Creation Detection

```
index=homelab-detect sourcetype=xmlwineventlog EventID=1 Image="*invoices.docx.exe"
| table _time, ParentImage, ParentCommandLine, Image, CommandLine, Computer
| sort _time
```

# C2 Callback Detection
### Outbound Connection to Attacker (192.168.1.250)

```
index=homelab-detect sourcetype=xmlwineventlog EventID=3 Initiated=true (DestinationIp="192.168.1.250" OR SourceIp="192.168.1.250")
| stats count by _time, Image, SourceIp, SourcePort, DestinationIp, DestinationPort, Initiated
```

# Nmap Scan Detection (from attacker → subnet)
### High Port Fan-Out = Scanning Behavior

```
index=homelab-detect id.orig_h=192.168.1.250 id.resp_h="192.168.1.*"
| stats dc(id.resp_p) as unique_ports count by id.resp_h
| sort - unique_ports
```

# Atomic Red Team – Process Creation
### New Local User Atomic Test (T1136.001)

```
index=homelab-detect sourcetype=xmlwineventlog EventID=1 CommandLine="*NewLocalUser*"
| table _time, ParentImage, ParentCommandLine, Image, CommandLine, Computer
| sort _time`
```

# Atomic Red Team – Account Manipulation Events

### Useful Windows Event IDs for local account creation/change:

| Event ID | Meaning                           |
| -------- | --------------------------------- |
| 4720     | A user account was created        |
| 4722     | A user account was enabled        |
| 4724     | Password reset attempt            |
| 4738     | A user account was changed        |
| 4726     | A user account was deleted        |
| 4798     | Local group membership enumerated |

### Query (Account Creation / Abuse)

```
index=homelab-detect sourcetype=xmlwineventlog EventID IN (4720,4722,4724,4726,4738,4798)
| table _time, EventID, TargetUserName, SubjectUserName, Computer
| sort _time
```

# Atomic Red Team — Persistence Technique (T1547.001 RunKeys)

### Detection for Registry RunKey Persistence

Atomic Red Team created persistence through:

- ``HKCU\Software\Microsoft\Windows\CurrentVersion\Run``

- ``HKLM\Software\Microsoft\Windows\CurrentVersion\RunOnce``

- ``Policies\Explorer\Run``

These often launch calc.exe, notepad.exe, or hidden PowerShell.

### Registry Value Set Detection (Sysmon EventID 13)

```
index=homelab-detect EventCode=13 TargetObject="*\\Run*" OR TargetObject="*\\RunOnce*"
| table _time, registry_value_data, registry_value_name registry_path 
| sort +_time
```

### Process Execution Triggered by Persistence (Sysmon EventID 1)

Detects processes launched via RunKeys:

```
index=homelab-detect EventID=1 ParentImage="*explorer.exe" (Image="*calc.exe" OR Image="*notepad.exe")
| table _time, Computer, ParentImage, Image, CommandLine
| sort _time
```


