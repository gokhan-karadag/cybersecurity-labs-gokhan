# Windows Event Log Cheatsheet

## Introduction

This cheatsheet provides a quick reference to Windows Security and PowerShell Event IDs. It covers logon activity, account and group changes, process creation, scheduled tasks, authentication, and PowerShell script blocks. Each section uses a Field / Description table to explain key fields for threat hunting and incident response.

> Event availability depends on audit policies, logging settings, Windows version, and computer role. The tables include selected investigation fields, not complete event schemas. XML field names may differ from labels shown in Event Viewer or your SIEM. Always review the event provider and log channel together with the Event ID.

## Table of Contents

- [Common Event Metadata](#common-event-metadata)
- [EventID 4624 Successful logon](#eventid-4624-successful-logon)
- [EventID 4625 Failed logon](#eventid-4625-failed-logon)
- [EventID 4688 Process creation](#eventid-4688-process-creation)
- [EventID 4720 User account created](#eventid-4720-user-account-created)
- [EventID 4732 Member added to a local security group](#eventid-4732-member-added-to-a-local-security-group)
- [EventID 4698 Scheduled task created](#eventid-4698-scheduled-task-created)
- [EventID 4768 Kerberos TGT requested](#eventid-4768-kerberos-tgt-requested)
- [EventID 4769 Kerberos service ticket requested](#eventid-4769-kerberos-service-ticket-requested)
- [EventID 4776 Credential validation](#eventid-4776-credential-validation)
- [EventID 1102 Security audit log cleared](#eventid-1102-security-audit-log-cleared)
- [EventID 4104 PowerShell script block logging](#eventid-4104-powershell-script-block-logging)
- [Additional Security Event IDs](#additional-security-event-ids)
- [Collection Notes](#collection-notes)
- [Sources](#sources)

## Common Event Metadata

| Field | Description |
| --- | --- |
| `Provider/@Name` | Event provider; use this with the event ID to identify the event type. |
| `EventID` | Identifier assigned by the provider. IDs alone are not globally unique. |
| `Version` | Event schema version. |
| `TimeCreated/@SystemTime` | UTC timestamp stored in the event XML. Event Viewer can display local time. |
| `EventRecordID` | Record identifier within a particular event log. |
| `Channel` | Log channel containing the event, such as Security. |
| `Computer` | Computer that recorded the event. |
| `Execution/@ProcessID` | Provider execution context; do not assume this is the process investigated in the payload. |

## EventID 4624 Successful logon

**Channel:** `Security`  
**Provider:** `Microsoft-Windows-Security-Auditing`

| Field | Description |
| --- | --- |
| `TargetUserName` | Account receiving the new logon session. |
| `TargetDomainName` | Domain or local computer associated with that account. |
| `TargetUserSid` | SID of the account that logged on. |
| `TargetLogonId` | Session identifier useful for correlation on the same computer. |
| `LogonType` | Category of logon, such as interactive, network, or remote interactive. |
| `IpAddress` | Source address when available. |
| `IpPort` | Source port when available. |
| `WorkstationName` | Originating workstation when supplied by the authentication mechanism. |
| `AuthenticationPackageName` | Authentication package used. |
| `ProcessName` | Process associated with the logon request when available. |

### Common Logon Types

| Value | Description |
| --- | --- |
| `2` | Interactive logon. |
| `3` | Network logon. |
| `4` | Batch logon. |
| `5` | Service logon. |
| `7` | Workstation unlock. |
| `8` | NetworkCleartext; does not by itself prove plaintext transmission over the network. |
| `9` | NewCredentials; local identity retained with alternate outbound credentials. |
| `10` | RemoteInteractive, commonly Remote Desktop. |
| `11` | CachedInteractive logon. |

## EventID 4625 Failed logon

**Channel:** `Security`  
**Provider:** `Microsoft-Windows-Security-Auditing`

| Field | Description |
| --- | --- |
| `TargetUserName` | Account named in the unsuccessful attempt. |
| `TargetDomainName` | Domain or computer named in the attempt. |
| `LogonType` | Requested logon category. |
| `Status` | Main failure status code. |
| `SubStatus` | More specific failure code when present. |
| `FailureReason` | Failure explanation or a message resource identifier in raw XML. |
| `IpAddress` | Originating address when available. |
| `WorkstationName` | Originating workstation when available. |
| `ProcessName` | Process associated with the attempt. |
| `AuthenticationPackageName` | Authentication package handling the request. |

## EventID 4688 Process creation

**Channel:** `Security`  
**Provider:** `Microsoft-Windows-Security-Auditing`

| Field | Description |
| --- | --- |
| `SubjectUserName` | Account requesting process creation. |
| `SubjectDomainName` | Domain or computer of the requesting account. |
| `SubjectLogonId` | Requesting account's logon session identifier. |
| `NewProcessId` | Identifier assigned to the new process; commonly represented in hexadecimal. |
| `NewProcessName` | Path of the newly created executable. |
| `ProcessId` | Creator process identifier in this event's payload. |
| `ParentProcessName` | Creator executable path in event versions that include it. |
| `CommandLine` | Process command line when the command-line audit policy is enabled. |
| `TokenElevationType` | Token elevation category. |
| `MandatoryLabel` | Integrity label in event versions that include it. |

## EventID 4720 User account created

**Channel:** `Security`  
**Provider:** `Microsoft-Windows-Security-Auditing`

| Field | Description |
| --- | --- |
| `SubjectUserName` | Account performing the creation. |
| `SubjectDomainName` | Domain or computer of the actor. |
| `SubjectLogonId` | Actor's session identifier. |
| `TargetUserName` | Newly created account name. |
| `TargetDomainName` | Domain or computer containing the new account. |
| `TargetSid` | New account's SID. |
| `SamAccountName` | SAM account name. |
| `DisplayName` | Display name when assigned. |
| `UserPrincipalName` | User principal name when assigned. |
| `NewUacValue` | User Account Control flags recorded for the new account. |

## EventID 4732 Member added to a local security group

**Channel:** `Security`  
**Provider:** `Microsoft-Windows-Security-Auditing`

| Field | Description |
| --- | --- |
| `SubjectUserName` | Account adding the member. |
| `SubjectDomainName` | Domain or computer of the actor. |
| `SubjectLogonId` | Actor's session identifier. |
| `MemberName` | Added member's distinguished name when available; may be a placeholder. |
| `MemberSid` | SID of the added account or group. |
| `TargetUserName` | Group receiving the new member. |
| `TargetDomainName` | Domain or computer associated with the group. |
| `TargetSid` | SID of the modified group. |

## EventID 4698 Scheduled task created

**Channel:** `Security`  
**Provider:** `Microsoft-Windows-Security-Auditing`

| Field | Description |
| --- | --- |
| `SubjectUserName` | Account creating the task. |
| `SubjectDomainName` | Domain or computer of the actor. |
| `SubjectLogonId` | Actor's session identifier. |
| `TaskName` | Registered task path and name. |
| `TaskContent` | Task definition XML, including actions, triggers, and execution identity. |
| `ClientProcessId` | Requesting client process identifier in newer event versions. |
| `ParentProcessId` | Client parent process identifier in newer event versions. |

## EventID 4768 Kerberos TGT requested

**Channel:** `Security` on a domain controller  
**Provider:** `Microsoft-Windows-Security-Auditing`

| Field | Description |
| --- | --- |
| `TargetUserName` | Account requesting a ticket-granting ticket. |
| `TargetDomainName` | Account realm or domain. |
| `TargetSid` | Requesting account's SID when resolved. |
| `ServiceName` | Kerberos service associated with the TGT request, typically krbtgt. |
| `TicketOptions` | Requested ticket option flags. |
| `TicketEncryptionType` | Encryption type associated with the ticket. |
| `PreAuthType` | Kerberos pre-authentication type. |
| `IpAddress` | Client address. |
| `IpPort` | Client source port. |
| `Status` | Result code. |

## EventID 4769 Kerberos service ticket requested

**Channel:** `Security` on a domain controller  
**Provider:** `Microsoft-Windows-Security-Auditing`

| Field | Description |
| --- | --- |
| `TargetUserName` | Account requesting the service ticket. |
| `TargetDomainName` | Requesting account's domain. |
| `ServiceName` | Account or service for which the ticket was requested. |
| `ServiceSid` | SID of the service account when available. |
| `TicketOptions` | Ticket option flags. |
| `TicketEncryptionType` | Ticket encryption type. |
| `IpAddress` | Requesting client's address. |
| `IpPort` | Client source port. |
| `Status` | Result code. |
| `LogonGuid` | Correlation identifier when populated. |

## EventID 4776 Credential validation

**Channel:** `Security` on the computer authoritative for the account  
**Provider:** `Microsoft-Windows-Security-Auditing`

| Field | Description |
| --- | --- |
| `PackageName` | Authentication package used for credential validation. |
| `TargetUserName` | Account whose credentials were checked. |
| `Workstation` | Source workstation name when available. |
| `Status` | Validation result; 0x0 indicates success. |

This event covers NTLM credential validation. Domain accounts are validated by a domain controller; local accounts are validated by the local computer. A successful validation does not by itself prove that a later logon succeeded.

## EventID 1102 Security audit log cleared

**Channel:** `Security`  
**Provider:** `Microsoft-Windows-Eventlog`

| Field | Description |
| --- | --- |
| `SubjectUserSid` | SID of the account that cleared the log. |
| `SubjectUserName` | Account that cleared the log. |
| `SubjectDomainName` | Domain or computer of that account. |
| `SubjectLogonId` | Account's logon session identifier. |

These fields are stored in the UserData section of the event XML.

## EventID 4104 PowerShell script block logging

**Channel:** `Microsoft-Windows-PowerShell/Operational` for Windows PowerShell  
**Provider:** `Microsoft-Windows-PowerShell`

| Field | Description |
| --- | --- |
| `ScriptBlockText` | Recorded PowerShell script block content. |
| `ScriptBlockId` | Identifier used to associate records belonging to a script block. |
| `MessageNumber` | Sequence number when a script block spans several records. |
| `MessageTotal` | Number of records used to store the script block. |
| `Path` | Script file path when available; can be empty for interactive or in-memory input. |

PowerShell 7 uses the PowerShellCore/Operational channel. Enable script block logging for consistent coverage; reconstruct multi-part records before analyzing the full content.

## Additional Security Event IDs

The following events use the Security channel and Microsoft-Windows-Security-Auditing provider.

| Event ID | Description |
| --- | --- |
| `4634` | Logon session ended. |
| `4647` | User initiated logoff. |
| `4648` | Explicit credentials used in a logon attempt. |
| `4657` | Registry value changed. |
| `4663` | Object access recorded. |
| `4672` | Sensitive privileges assigned at logon. |
| `4689` | Process exited. |
| `4697` | Service installed. |
| `4699` | Scheduled task deleted. |
| `4700` | Scheduled task enabled. |
| `4701` | Scheduled task disabled. |
| `4702` | Scheduled task updated. |
| `4719` | Audit policy changed. |
| `4722` | Account enabled. |
| `4723` | Password change attempted. |
| `4724` | Password reset attempted. |
| `4725` | Account disabled. |
| `4726` | Account deleted. |
| `4728` / `4729` | Global security group member added / removed. |
| `4733` | Local security group member removed. |
| `4738` | Account changed. |
| `4740` | Account locked out. |
| `4756` / `4757` | Universal security group member added / removed. |
| `4767` | Account unlocked. |
| `4771` | Kerberos pre-authentication failed. |
| `5140` | Network share accessed. |
| `5145` | Network share access check performed. |

## Collection Notes

| Setting or Context | Description |
| --- | --- |
| Audit policy | Enable the relevant success and/or failure audit subcategories before relying on event coverage. |
| Process command lines | Audit Process Creation and the policy to include command lines are separate settings. |
| Object auditing | File and registry object auditing typically also requires suitable SACL entries. |
| Event versions | Available fields can change with Windows versions and updates; inspect the actual XML. |
| Correlation | Combine computer, timestamp, provider, account identity, and session/process identifiers. |
| Missing records | Missing events alone do not demonstrate that an action did not happen. |
| Interpretation | These are investigation signals; legitimate administration can generate the same events. |

## Sources

Microsoft documentation; reviewed October 21, 2026.

- [Windows advanced audit policy reference](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/plan/security-best-practices/advanced-audit-policy-configuration)
- [Windows security auditing reference](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/security-auditing-overview)
- [Event 4624](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4624)
- [Event 4625](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4625)
- [Event 4688](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4688)
- [Event 4720](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4720)
- [Event 4732](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4732)
- [Event 4698](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4698)
- [Event 4768](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4768)
- [Event 4769](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4769)
- [Event 4776](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-4776)
- [Event 1102](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/event-1102)
- [Windows PowerShell logging](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_logging?view=powershell-5.1)
- [PowerShell logging on Windows](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_logging_windows)

## Credits

| Field | Description |
| --- | --- |
| `Creator` | Gökhan Karadağ ([LinkedIn](https://www.linkedin.com/in/gokhankaradagk/)) |
