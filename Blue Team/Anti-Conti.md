<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/88fa01cc-d1df-4175-a647-2471e3f5769b" />

# Conti Ransomware Investigation
## Table of Contents

- [Overview](https://github.com/gokhan-karadag/cybersecurity-labs-gokhan/blob/main/Blue%20Team/Anti-Conti.md#overview)
  - [Learning objectives](https://github.com/gokhan-karadag/cybersecurity-labs-gokhan/blob/main/Blue%20Team/Anti-Conti.md#learning-objectives)
  - [Attack chain](https://github.com/gokhan-karadag/cybersecurity-labs-gokhan/blob/main/Blue%20Team/Anti-Conti.md#attack-chain)
  - [Environment details](https://github.com/gokhan-karadag/cybersecurity-labs-gokhan/blob/main/Blue%20Team/Anti-Conti.md#environment-details)
- [Background: Ransomware and Conti](https://github.com/gokhan-karadag/cybersecurity-labs-gokhan/blob/main/Blue%20Team/Anti-Conti.md#background-ransomware-and-conti)
- [Scenario and Ransom Note](https://github.com/gokhan-karadag/cybersecurity-labs-gokhan/blob/main/Blue%20Team/Anti-Conti.md#scenario-and-ransom-note)
- [Initial Analysis: Task Category](https://github.com/gokhan-karadag/cybersecurity-labs-gokhan/blob/main/Blue%20Team/Anti-Conti.md#initial-analysis-task-category)
- [Q1: Location of the ransomware](https://github.com/gokhan-karadag/cybersecurity-labs-gokhan/blob/main/Blue%20Team/Anti-Conti.md#q1-location-of-the-ransomware)
- [Q2: Sysmon event ID for file creation](https://github.com/gokhan-karadag/cybersecurity-labs-gokhan/blob/main/Blue%20Team/Anti-Conti.md#q2-sysmon-event-id-for-file-creation)
- [Q3: MD5 hash of the ransomware](https://github.com/gokhan-karadag/cybersecurity-labs-gokhan/blob/main/Blue%20Team/Anti-Conti.md#q3-md5-hash-of-the-ransomware)
- [Q4: File saved to multiple folders](https://github.com/gokhan-karadag/cybersecurity-labs-gokhan/blob/main/Blue%20Team/Anti-Conti.md#q4-file-saved-to-multiple-folders)
- [Q5: Command used to add a new user](https://github.com/gokhan-karadag/cybersecurity-labs-gokhan/blob/main/Blue%20Team/Anti-Conti.md#q5-command-used-to-add-a-new-user)
- [Q6: Process migration target](https://github.com/gokhan-karadag/cybersecurity-labs-gokhan/blob/main/Blue%20Team/Anti-Conti.md#q6-process-migration-target)
- [Q7: Process used to retrieve system hashes](https://github.com/gokhan-karadag/cybersecurity-labs-gokhan/blob/main/Blue%20Team/Anti-Conti.md#q7-process-used-to-retrieve-system-hashes)
- [Q8: Web shell deployed by the exploit](https://github.com/gokhan-karadag/cybersecurity-labs-gokhan/blob/main/Blue%20Team/Anti-Conti.md#q8-web-shell-deployed-by-the-exploit)
- [Q9: Command line that executed the web shell](https://github.com/gokhan-karadag/cybersecurity-labs-gokhan/blob/main/Blue%20Team/Anti-Conti.md#q9-command-line-that-executed-the-web-shell)
- [Q10: CVEs leveraged](https://github.com/gokhan-karadag/cybersecurity-labs-gokhan/blob/main/Blue%20Team/Anti-Conti.md#q10-cves-leveraged)
- [References](https://github.com/gokhan-karadag/cybersecurity-labs-gokhan/blob/main/Blue%20Team/Anti-Conti.md#references)
- [Answers at a Glance](https://github.com/gokhan-karadag/cybersecurity-labs-gokhan/blob/main/Blue%20Team/Anti-Conti.md#answers-at-a-glance)

## Overview

In this room, we investigate the effects of **Conti ransomware** on a company-hosted Exchange server. We mainly use **Splunk** to look for anomalous events created during the malware's execution.

### Learning objectives

- Reconnaissance
- Initial access
- Persistence and privilege escalation
- System enumeration and data theft
- Ransomware deployment

### Attack chain

1. The attacker exploited several vulnerabilities in Microsoft Exchange Server.
2. They gained a foothold via PowerShell.
3. They migrated into another process.
4. They launched a web shell.
5. They executed commands to retrieve the ransomware.

### Environment details

| Field | Value |
|-------|-------|
| Target server | `WIN-AOQKG2AS2Q7` (Exchange) |
| Domain | `bellybear.local` |
| Log date | 2021-09-08 |
| Log sources | Sysmon (`WinEventLog:Microsoft-Windows-Sysmon/Operational`), IIS (`sourcetype=iis`), Windows Security |

## Background: Ransomware and Conti

### What is ransomware?

**Ransomware** is a type of malware that blocks access to a computer or encrypts files and demands a ransom payment (often in cryptocurrency) to restore access.

| Type | Description |
|------|-------------|
| **Crypto-Ransomware** | Encrypts files (documents, photos, databases). |
| **Locker Ransomware** | Locks the user out of the entire device. |
| **Double/Triple Extortion** | Attackers also steal data and threaten to leak or sell it. |

### About Conti

- Conti was run by the Russian group **Wizard Spider**.
- It became active around **2019** and caused many large-scale attacks worldwide.
- It used **double extortion**: files were locked and stolen data was threatened with leaking.
- It mostly targeted large organizations, which are more likely to pay high ransoms.
- The group appeared to stop in 2022, but many members joined other ransomware groups.

## Scenario and Ransom Note

Some employees reported that they couldn't log into Outlook. The Exchange system admin also couldn't log into the Exchange Admin Center. After initial triage, several unusual `readme` files were found on the Exchange server.

In summary, the ransom note says:

- All files are encrypted by the **CONTI** strain.
- The victim must contact the team to recover data; 2 random files are offered for free decryption.
- A TOR (`.onion`) address and an HTTPS address are provided for contact.
- If the victim does not respond, stolen internal data will be published (**double extortion**).
- The note contains a `BEGIN ID / END ID` block identifying the victim.

## Initial Analysis: Task Category

To get an overall view of the attacker's footprint, we first search `index=*` and check the distribution of the **TaskCategory** field. The most frequent values were `None`, `Logon`, `Logoff`, `Special Logon`, `Audit Policy Change`, `Registry value set (rule: RegistryEvent)`, `Configuration`, `General`, `User Account Management`, and `Network connection detected (rule: NetworkConnect)`.

This shows which event types to focus on. For example, `User Account Management` relates to user creation (Q5), and `File created` relates to file creation (Q2, Q4).

---

## Q1: Location of the ransomware

> **Question:** Can you identify the location of the ransomware?

> **Hint:** *Look for a common Windows binary located in an unusual location.*

**Method 1: Using the `readme` file from the scenario as a keyword**

The task scenario mentions that unusual `readme` files were found on the Exchange server. We can use this as a search keyword:

```spl
index=* readme
```

This returns 18 events. In the field list on the left, the `Image` field has only **1 value** (100% of events, 18 count): `c:\Users\Administrator\Documents\cmd.exe`. This means every `readme` file was created by this single executable. The `TargetFilename` field (18 values) confirms the `readme.txt` files were dropped into many different directories. Since a copy of `cmd.exe` running from the Documents folder is not where the legitimate binary lives, this is the ransomware.
<img width="1917" height="980" alt="image" src="https://github.com/user-attachments/assets/2bee7329-c14c-4a56-844b-dca9931098c4" />

**Method 2: Unique listing with `table` and `dedup`**

```spl
index=* sourcetype="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=11
| table Image
| dedup Image
```

The `Image` field contains the paths of executables or scripts that ran. In the results, the only suspicious entry outside the system directories is `cmd.exe`.

**Method 3: Reviewing all processes by `CurrentDirectory`**

```spl
index=* sourcetype="WinEventLog:Microsoft-Windows-Sysmon/Operational"
| dedup CurrentDirectory
| table CurrentDirectory CommandLine Image Hashes ParentCommandLine ParentImage
```

`cmd.exe` normally lives in `C:\Windows\System32`. A copy running from a user's Documents folder is suspicious and suggests masquerading as a legitimate system binary.

**Method 4: Sysmon Event ID 11 (FileCreate) and the `Image` field**

```spl
index=* sourcetype="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=11
```

Sysmon Event ID 11 is logged when a file is created or overwritten. Selecting the `Image` field on the left lists all executables that created files. There are 104 events and 10 distinct `Image` values. Among them, `c:\Users\Administrator\Documents\cmd.exe` (18 events) stands out.

<details>
<summary>Answer</summary>

`C:\Users\Administrator\Documents\cmd.exe`

</details>

> [!NOTE]
> The file sitting in the Administrator profile suggests that account was compromised or used, but it is not proof of privilege escalation on its own.

## Q2: Sysmon event ID for file creation

> **Question:** What is the Sysmon event ID for the related file creation event?

There are three ways to find this:

1. **Sysmon Cheat Sheet:** The [Sysmon Cheat Sheet](https://github.com/gokhan-karadag/cybersecurity-labs-gokhan/blob/main/Blue%20Team/Sysmon-Cheatsheet.md) lists **EventID 11: File create**, with the fields `UtcTime`, `ProcessGuid`, `ProcessId`, `Image`, `TargetFilename`, `CreationUtcTime`, and `User`.
2. **Google search:** Searching "What is the file creation Event ID?" returns Sysmon Event ID 11 (FileCreate).
3. **Ultimate Windows Security:** According to [Sysmon Event ID 11: FileCreate](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventid=90011), this event is logged when a file is created or overwritten. It is especially useful for monitoring locations malware commonly uses on initial infection, such as the Startup folder and temporary and download directories.

It can also be verified in Splunk using the `File created` TaskCategory and the `EventCode` field:

```spl
index=main sourcetype="WinEventLog:Microsoft-Windows-Sysmon/Operational"
```

<details>
<summary>Answer</summary>

`11` (FileCreate)

</details>

## Q3: MD5 hash of the ransomware

> **Question:** Can you find the MD5 hash of the ransomware?

Search for the malware’s file path found in Q1 together with the keyword MD5, then inspect the Hashes field in the result, which contains the MD5, SHA256, and IMPHASH values:

```spl
Image="c:\\Users\\Administrator\\Documents\\cmd.exe" MD5
```

The query returns a single event, and the MD5 value is taken from its Hashes field.

<details>
<summary>Answer</summary>

`290c7dfb01e50cea9e19da81a781af2c`

(Splunk displays it in uppercase as `290C7DFB01E50CEA9E19DA81A781AF2C`; it is the same value.)

</details>
<img width="1915" height="1032" alt="image" src="https://github.com/user-attachments/assets/1ee375cd-8903-4b84-a9ab-1b60e10945f1" />

## Q4: File saved to multiple folders

> **Question:** What file was saved to multiple folder locations?

We list the files created by the ransomware (`cmd.exe`) using Event Code 11 and check the `TargetFilename` field.

**Method 1: Inspecting the `TargetFilename` field**

```spl
index=* Image="C:\\Users\\Administrator\\Documents\\cmd.exe" EventCode=11
```

The search returns 18 events. In the field list on the left, the `TargetFilename` field has 18 distinct values (100% of events). Opening the field shows that every top value is a `readme.txt` file in a different directory.

<img width="1920" height="615" alt="image" src="https://github.com/user-attachments/assets/e8ad57be-ef03-4e9d-9e24-3801c1f74499" />

**Method 2: Counting with `stats`**

```spl
index=* sourcetype="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=11 Image="c:\\Users\\Administrator\\Documents\\cmd.exe"
| stats count by TargetFilename
```

**Method 3: Listing all file creation events**

```spl
index=main sourcetype="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=11
| table Image TargetFilename
```

In all three, `readme.txt` appears in many directories. Examples:

- `C:\Users\.NET v4.5 Classic\Downloads\readme.txt`
- `C:\Users\Administrator\Downloads\readme.txt`
- `C:\Users\Default\AppData\Local\readme.txt`
- `C:\Users\Default\Desktop\readme.txt`
- `C:\Users\Default\Documents\readme.txt`

This shows the ransomware dropped the file into many different directories.

<details>
<summary>Answer</summary>

`readme.txt` (the ransom note, dropped into many directories)

</details>
OR
<img width="1905" height="984" alt="image" src="https://github.com/user-attachments/assets/545e785b-3747-47af-9691-4c22b8594872" />

## Q5: Command used to add a new user

> **Question:** What was the command the attacker used to add a new user to the compromised system?

Searching for the keyword `add` surfaces events involving a user called `securityninja`. Scrolling through the logs reveals the `net` command that created the user. This can be confirmed in three ways.

**Method 1: Searching `CommandLine` for `/add`**

```spl
CommandLine="*/add*"
```

This returns 6 events, and the `CommandLine` field shows these commands:

```
net user /add securityninja hardToHack123$
net localgroup administrators securityninja /add
net localgroup "Remote Desktop Users" "securityninja" /add
```

(The same commands also appear as variants run through `C:\Windows\system32\net1`.) The `net user` command can add, modify, or delete user accounts and display account information on a local computer or domain.
<img width="1915" height="996" alt="image" src="https://github.com/user-attachments/assets/ecb27d78-9e77-42c9-b281-5f8568ac658a" />

**Method 2: Windows Security Event 4720**

```spl
index=* EventCode=4720
```

Event ID **4720** ("A user account was created") is written to the Windows Security log when a new user account is created on a system or domain. It is important for security monitoring and incident response because attackers often create new accounts to maintain persistence. The `Account_Name` field shows the `securityninja` account ([reference](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/default.aspx)).
<img width="1912" height="1011" alt="image" src="https://github.com/user-attachments/assets/a312933d-6e7c-4b86-8f81-ff0014602d2f" />

**Method 3: Searching for the suspicious account name**

```spl
index=* securityninja
```

This query returns **10 events**. The `CommandLine` field shows the commands that were executed. We choose the correct command based on **what it does**, rather than its position in the list.

### Answer

`net user /add securityninja hardToHack123$`

This command creates the `securityninja` account with the specified password. The other commands add the existing account to groups:

| Command | Description |
|---|---|
| `net user /add securityninja hardToHack123$` | Creates the new user account. |
| `net localgroup administrators securityninja /add` | Adds the account to the **Administrators** group, granting administrator privileges. |
| `net localgroup "Remote Desktop Users" "securityninja" /add` | Adds the account to the **Remote Desktop Users** group, granting permission to sign in through RDP. |

The `net1.exe` entries are records of the helper process started by `net.exe`; they do not represent separate account creation actions. Since the question asks for the command the attacker used, we select the original `net user /add ...` command.

> [!NOTE]
> Creating the account and adding it to these groups suggests that the attacker was trying to maintain administrator access and prepare an account for future RDP connections. Successful RDP access also depends on the service, firewall settings, network access, and applicable logon policies.
<img width="1927" height="982" alt="image" src="https://github.com/user-attachments/assets/94d28709-2284-4d77-a5a6-a9840f361bba" />


## Q6: Process migration target

> **Question:** The attacker migrated the process for better persistence. What is the migration target (full image path)?

Filter on Event Code 8 (CreateRemoteThread):
<img width="1907" height="1033" alt="image" src="https://github.com/user-attachments/assets/dee93309-90ef-4ac1-a55c-ff203094b2bd" />

```spl
index=* EventCode=8
| table SourceImage TargetImage
```

Sysmon Event ID 8 detects when a process creates a thread in another process, which malware can use for code injection. The `SourceImage` field holds the full path of the executable that generated the event.

`powershell.exe` (the process used for the initial foothold) created a remote thread in `unsecapp.exe`, so the payload migrated into it.

<details>
<summary>Answer</summary>

`C:\Windows\System32\wbem\unsecapp.exe`

</details>
<img width="1914" height="603" alt="image" src="https://github.com/user-attachments/assets/55645e38-f6b4-458d-9405-14d8bd1a0237" />

> [!NOTE]
> The original process was `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe`. It is not part of the answer but is useful context for your notes.
> Some versions of this room word the question in an older, two-part form: *"What is the migrated process image (executable), and what is the original process image (executable) when the attacker got on the system?"* There the answer is given as `powershell.exe,unsecapp.exe` (original first, then migrated):
>
> - **Original process:** `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe`
> - **Migrated process:** `C:\Windows\System32\wbem\unsecapp.exe`
>
> The current room question asks only for the migration target. Adjust the format to match whichever version you encounter.

## Q7: Process used to retrieve system hashes

> **Question:** The attacker also injected into a system process to retrieve the hashes. What is the target process image used for getting the system hashes?

To retrieve system hashes, attackers typically use tools that access **LSASS** (Local Security Authority Subsystem Service) process memory or the **SAM** database. Since the question describes injection into a system process, the same Event Code 8 (CreateRemoteThread) query from Q6 applies:

```spl
index=* sourcetype="WinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=8
| table SourceImage TargetImage
```

The `TargetImage` field shows two values (50% each):

- `C:\Windows\System32\lsass.exe`
- `C:\Windows\System32\wbem\unsecapp.exe` (the migration target from Q6)

`lsass.exe` is the Windows process that holds authentication material (NTLM hashes, Kerberos tickets, etc.) in memory. It is a common target for credential dumping, and attackers often dump it.

<details>
<summary>Answer</summary>

`C:\Windows\System32\lsass.exe`

</details>
<img width="1917" height="982" alt="image" src="https://github.com/user-attachments/assets/e04b5e14-db67-4a07-a765-199fe8744577" />

> [!NOTE]
> Some versions of this room word the question slightly differently: *"The attacker also retrieved the system hashes. What is the process image used for getting the system hashes?"* The answer is the same.

## Q8: Web shell deployed by the exploit

> **Question:** What is the web shell the exploit deployed to the system?

> **Hint:** *Try looking in the IIS logs for POST requests.*

Filter the IIS logs for POST requests and inspect `cs_uri_stem` for URIs that don't belong to Exchange. Web shells on Exchange/IIS servers use the `.aspx` extension, so filtering on `aspx` narrows the results to a single suspicious file.

**Method 1: IIS POST requests and common web shell extensions**

Change the sourcetype from Sysmon to **IIS**, since it collects events related to web pages. Then filter for POST requests and common web shell file types:

```spl
index=* sourcetype=iis cs_method=POST
| search *.php* OR *.asp* OR *.aspx* OR *.jsp*
```

This returns 187 events. Among the 6 values in `cs_uri_stem`, one is a suspicious file that does not belong to Exchange:

| cs_uri_stem | Count |
|-------------|-------|
| `/ecp/DDI/DDIService.svc/GetList` | 171 |
| `/owa/auth.owa` | 8 |
| `/owa/auth/i3gfPctK1c2x.aspx` | 4 |
| `/ecp/DDI/DDIService.svc/NewObject` | 2 |
| `/ecp/DDI/DDIService.svc/GetObject` | 1 |
| `/owa/lang.owa` | 1 |

**Method 2: Listing unique URIs of POST requests**

```spl
index=* sourcetype=iis cs_method=POST
| dedup cs_uri_stem
| table cs_uri_stem
```

**Method 3: Searching for `.aspx` directly**

```spl
*.aspx*
```

The goal is to find the file name or path of the malicious web shell dropped on the victim machine. In the `cs_uri_stem` field (45 values), `/owa/auth/i3gfPctK1c2x.aspx` stands out.

**Method 4: Searching the Sysmon logs for `aspx`**

```spl
index=* sourcetype="WinEventLog:Microsoft-Windows-Sysmon/Operational" *aspx
```

<details>
<summary>Answer</summary>

`i3gfPctK1c2x.aspx`

</details>

## Q9: Command line that executed the web shell

> **Question:** What is the command line that executed this web shell?

Using the web shell name found in the previous question, search for it and check the `CommandLine` field:

```spl
i3gfPctK1c2x.aspx
```

This returns 6 events. The `CommandLine` field shows a single value, the `attrib.exe` command (the `Image` field is `C:\Windows\System32\attrib.exe`).

<details>
<summary>Answer</summary>

```
attrib.exe -r \\win-aoqkg2as2q7.bellybear.local\C$\Program Files\Microsoft\Exchange Server\V15\FrontEnd\HttpProxy\owa\auth\i3gfPctK1c2x.aspx
```

</details>

> [!NOTE]
> `attrib.exe -r` removes the read-only attribute from the file; it does not execute it. It is the answer the room expects, but in analysis notes it is more accurate to describe it as the attacker modifying the web shell's file attributes while preparing it for use.

> [!NOTE]
> The Splunk interface may display backslashes in escaped form (`\\\\win-...`). The real UNC path starts with `\\win-...`. Check which format the room accepts.

## Q10: CVEs leveraged

> **Question:** What three CVEs did this exploit leverage? Provide the answer in ascending order.

> **Hint:** *External research required.*

The answer comes from the Securin (formerly Cyber Security Works) blog post that lists the vulnerabilities associated with Conti: [Is Conti Ransomware on a Roll?](https://www.securin.io/articles/is-conti-ransomware-on-a-roll) (the original post is also available on the [Cyber Security Works site](https://cybersecurityworks.com/blog/ransomware/is-conti-ransomware-on-a-roll.html)).

Overview of the leveraged vulnerabilities:

| CVE | Short description |
|-----|-------------------|
| CVE-2018-13374 | Fortinet FortiOS vulnerability |
| CVE-2018-13379 | Fortinet FortiOS SSL VPN path traversal |
| CVE-2020-0796 | Windows SMBv3 vulnerability (SMBGhost) |

<details>
<summary>Answer</summary>

`CVE-2018-13374, CVE-2018-13379, CVE-2020-0796`

</details>

## References

- [Securin (formerly Cyber Security Works): Is Conti Ransomware on a Roll?](https://www.securin.io/articles/is-conti-ransomware-on-a-roll)
- [Medium: TryHackMe Conti Write-up](https://medium.com/@kumarishefu.4507/try-hack-me-conti-write-up-c94713193e11)
- [Ultimate Windows Security: Sysmon Event ID 11 (FileCreate)](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventid=90011)
- [Ultimate Windows Security: Event Encyclopedia (4720)](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/default.aspx)
- [Sysmon Cheat Sheet (networkforensic.dk)](https://networkforensic.dk/Sysmon/Files/Sysmon-Cheatsheet.pdf)

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





















