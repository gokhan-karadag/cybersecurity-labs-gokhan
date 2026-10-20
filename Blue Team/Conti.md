<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/b78541df-f0bc-4850-a3d7-d28ccdb7fed0" />

# Conti

![Platform](https://img.shields.io/badge/Platform-TryHackMe-212C42?logo=tryhackme)
![Tool](https://img.shields.io/badge/Tool-Splunk-000000?logo=splunk)
![Category](https://img.shields.io/badge/Category-Blue%20Team%20%2F%20DFIR-blue)

## Overview

In this room, we investigate the effects of **Conti ransomware** on an Exchange server, using **Splunk** to look for anomalies created during the malware's execution.

### Attack chain

1. The attacker exploited several vulnerabilities in Microsoft Exchange Server.
2. They gained a foothold via PowerShell.
3. They migrated into another process.
4. They launched a web shell.
5. They executed commands to retrieve the ransomware.

## Table of Contents

- [Answers at a Glance](#answers-at-a-glance)
- [Q1: Location of the ransomware](#q1-location-of-the-ransomware)
- [Q2: Sysmon event ID for file creation](#q2-sysmon-event-id-for-file-creation)
- [Q3: MD5 hash of the ransomware](#q3-md5-hash-of-the-ransomware)
- [Q4: File saved to multiple folders](#q4-file-saved-to-multiple-folders)
- [Q5: Command used to add a new user](#q5-command-used-to-add-a-new-user)
- [Q6: Process migration target](#q6-process-migration-target)
- [Q7: Process used to retrieve system hashes](#q7-process-used-to-retrieve-system-hashes)
- [Q8: Web shell deployed by the exploit](#q8-web-shell-deployed-by-the-exploit)
- [Q9: Command line that executed the web shell](#q9-command-line-that-executed-the-web-shell)
- [Q10: CVEs leveraged](#q10-cves-leveraged)
- [References](#references)

## Answers at a Glance

| # | Question | Answer |
|---|----------|--------|
| 1 | Location of the ransomware | `C:\Users\Administrator\Documents\cmd.exe` |
| 2 | Sysmon event ID for file creation | `11` |
| 3 | MD5 hash of the ransomware | `290c7dfb01e50cea9e19da81a781af2c` |
| 4 | File saved to multiple folders | `readme.txt` |
| 5 | Command used to add a new user | `net user /add securityninja hardToHack123$` |
| 6 | Migration target (full image path) | `C:\Windows\System32\wbem\unsecapp.exe` |
| 7 | Target process for system hashes | `C:\Windows\System32\lsass.exe` |
| 8 | Web shell deployed | `i3gfPctK1c2x.aspx` |
| 9 | Command line that executed the web shell | `attrib.exe -r \\win-aoqkg2as2q7.bellybear.local\C$\...\i3gfPctK1c2x.aspx` |
| 10 | Three CVEs (ascending) | `CVE-2018-13374, CVE-2018-13379, CVE-2020-0796` |

---

## Q1: Location of the ransomware

> **Question:** Can you identify the location of the ransomware?

I listed all images seen running in the Sysmon logs:

```spl
index=* sourcetype="WinEventLog:Microsoft-Windows-Sysmon/Operational"
| dedup CurrentDirectory
| table CurrentDirectory CommandLine Image Hashes ParentCommandLine ParentImage
```

`cmd.exe` normally lives in `C:\Windows\System32`. A copy running from a user's Documents folder is suspicious and suggests masquerading as a legitimate system binary.

<details>
<summary>Answer</summary>

`C:\Users\Administrator\Documents\cmd.exe`

</details>

> [!NOTE]
> The file sitting in the Administrator profile suggests that account was compromised or used, but it is not proof of privilege escalation on its own.

## Q2: Sysmon event ID for file creation

> **Question:** What is the Sysmon event ID for the related file creation event?

Filter by the `File created` TaskCategory and check the `EventCode` field:

```spl
index=main sourcetype="WinEventLog:Microsoft-Windows-Sysmon/Operational"
```

<details>
<summary>Answer</summary>

`11` (FileCreate)

</details>

## Q3: MD5 hash of the ransomware

> **Question:** Can you find the MD5 hash of the ransomware?

Search for the malware's name, then inspect the `Hashes` field in the results.

<details>
<summary>Answer</summary>

`290c7dfb01e50cea9e19da81a781af2c`

</details>

## Q4: File saved to multiple folders

> **Question:** What file was saved to multiple folder locations?

Check the `TargetFilename` field in Event Code 11 events:

```spl
index=main sourcetype="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=11
| table Image TargetFilename
```

<details>
<summary>Answer</summary>

`readme.txt` (the ransom note, dropped into many directories)

</details>

## Q5: Command used to add a new user

> **Question:** What was the command the attacker used to add a new user to the compromised system?

Searching for the keyword `add` surfaces events involving a user called `securityninja`. Windows Security Event ID **4720** ("A user account was created") records account creation ([reference](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/default.aspx)). Scrolling through the logs reveals the `net` command that created the user.

<details>
<summary>Answer</summary>

`net user /add securityninja hardToHack123$`

</details>

## Q6: Process migration target

> **Question:** The attacker migrated the process for better persistence. What is the migration target (full image path)?

Filter on Event Code 8 (CreateRemoteThread):

```spl
index=* sourcetype="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=8
| table SourceImage TargetImage
```

`powershell.exe` (the process used for the initial foothold) created a remote thread in `unsecapp.exe`, so the payload migrated into it.

<details>
<summary>Answer</summary>

`C:\Windows\System32\wbem\unsecapp.exe`

</details>

> [!NOTE]
> The original process was `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe`. It is not part of the answer but is useful context for your notes.

## Q7: Process used to retrieve system hashes

> **Question:** The attacker also injected into a system process to retrieve the hashes. What is the target process image used for getting the system hashes?

The same Event Code 8 (CreateRemoteThread) query applies, since the question describes injection into a system process:

```spl
index=* sourcetype="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=8
| table SourceImage TargetImage
```

The target is **LSASS** (Local Security Authority Subsystem Service), which holds credentials in memory and is a common target for credential dumping.

<details>
<summary>Answer</summary>

`C:\Windows\System32\lsass.exe`

</details>

> [!TIP]
> Event Code 10 (ProcessAccess) is a useful additional check for LSASS access:
> ```spl
> index=* sourcetype="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=10 TargetImage="*lsass.exe"
> | table SourceImage TargetImage GrantedAccess
> ```

## Q8: Web shell deployed by the exploit

> **Question:** What is the web shell the exploit deployed to the system?

Filter IIS logs for POST requests and inspect `cs_uri_stem` for URIs that don't belong to Exchange. Web shells on Exchange/IIS servers use the `.aspx` extension, so filtering on `aspx` narrows the results to a single suspicious file.

```spl
index=* sourcetype=iis cs_method=POST
| dedup cs_uri_stem
| table cs_uri_stem
```

```spl
index=* sourcetype="WinEventLog:Microsoft-Windows-Sysmon/Operational" *aspx
```

<details>
<summary>Answer</summary>

`i3gfPctK1c2x.aspx`

</details>

## Q9: Command line that executed the web shell

> **Question:** What is the command line that executed this web shell?

Searching for the web shell's filename shows the `attrib.exe` command.

<details>
<summary>Answer</summary>

```
attrib.exe -r \\win-aoqkg2as2q7.bellybear.local\C$\Program Files\Microsoft\Exchange Server\V15\FrontEnd\HttpProxy\owa\auth\i3gfPctK1c2x.aspx
```

</details>

> [!NOTE]
> `attrib.exe -r` removes the read-only attribute from the file; it does not execute it. It is the answer the room expects, but in analysis notes it is more accurate to describe it as the attacker modifying the web shell's file attributes while preparing it for use.

## Q10: CVEs leveraged

> **Question:** What three CVEs did this exploit leverage? Provide the answer in ascending order.

This is answered from the Cyber Security Works blog post, [Is Conti Ransomware on a Roll?](https://cybersecurityworks.com/blog/ransomware/is-conti-ransomware-on-a-roll.html), which lists the CVEs associated with Conti.

<details>
<summary>Answer</summary>

`CVE-2018-13374, CVE-2018-13379, CVE-2020-0796`

</details>

> [!WARNING]
> Verify these three CVEs against the blog's list before submitting.

## References

- [Cyber Security Works: Is Conti Ransomware on a Roll?](https://cybersecurityworks.com/blog/ransomware/is-conti-ransomware-on-a-roll.html)
- [Medium: TryHackMe Conti Write-up](https://medium.com/@kumarishefu.4507/try-hack-me-conti-write-up-c94713193e11)
- [Ultimate Windows Security: Event 4720](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/default.aspx)
























































