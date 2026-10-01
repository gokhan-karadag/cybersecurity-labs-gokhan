# Log Analysis Fundamentals

A practical introduction to collecting, reading, and correlating security evidence from Windows, Linux, email, and network sources.

![Log Analysis Introduction](https://github.com/user-attachments/assets/9b453562-c6f0-4205-a0df-de11f2dc453d)

Logs help analysts understand what happened, when it happened, and which systems or accounts were involved. This lesson introduces common log sources and connects email analysis, log searches, packet inspection, and data transformation through classroom examples.

## Learning Objectives

![Learning Objectives](https://github.com/user-attachments/assets/3e6834c3-5c80-4c37-9e02-576b6ffdf1ab)

By the end of this lesson, you should be able to:

- Explain logging and log analysis.
- Identify common log sources.
- Interpret basic Windows and Linux events.
- Explain collection, parsing, normalization, and correlation.
- Use tools to inspect email headers, logs, and packet captures.
- Separate observed facts from findings that require verification.

You do not need to memorize every event ID. Focus on knowing where to look and which questions to ask.

## 1. What Is Log Analysis?

![What Is Log Analysis](https://github.com/user-attachments/assets/58e1e400-5f38-4957-a705-302d65251515)

A log is a record of an event, such as a user signing in, an application failing, or a firewall blocking a connection.

Log analysis begins with an investigation question. Analysts review relevant records, examine their context, and determine what the evidence supports.

An alert identifies activity that may need attention. The supporting events still require review.

### Common Log Fields

![Common Log Fields](https://github.com/user-attachments/assets/73122da5-6338-4339-8f1e-8989ac5ff809)

Different sources record different fields. Common examples include:

| Field | Investigation Value |
|---|---|
| Timestamp | When the event was recorded |
| Host | Which system recorded or experienced the event |
| Account | Which account was associated with the activity |
| Service or provider | Which component generated the record |
| Source and destination | Where a connection originated and where it went |
| Action or result | What happened and whether it succeeded |
| Severity | The importance assigned by the source, when available |

A username and an IP address provide separate pieces of context. Neither alone proves which person performed an action.

### Syslog Severity Levels

![Syslog Severity Levels](https://github.com/user-attachments/assets/8f18afc7-9109-4c97-932f-f48b166da89d)

| Level | Name | Meaning |
|---|---|---|
| 0 | Emergency | The system is unusable |
| 1 | Alert | Immediate action is required |
| 2 | Critical | A critical condition |
| 3 | Error | An error condition |
| 4 | Warning | A warning condition |
| 5 | Notice | A normal but significant event |
| 6 | Informational | Information about system activity |
| 7 | Debug | Detailed troubleshooting information |

**Lower numbers indicate higher severity.** These are syslog levels; other platforms may use different severity schemes.

## 2. From Raw Data to Investigation Findings

![From Chaos to Clarity](https://github.com/user-attachments/assets/ade818d6-4a89-40ca-b46c-1009def23f83)

A typical analysis workflow includes:

1. Collect relevant records.
2. Filter events that relate to the investigation question.
3. Parse useful fields.
4. Normalize fields where needed.
5. Correlate activity across sources.
6. Build a timeline.
7. Explain supported findings and unresolved questions.

## 3. What Is Logging?

![What Is Logging](https://github.com/user-attachments/assets/1a7e0012-3af6-4acf-9b63-a2665cc23404)

Logging records activity on systems and devices. Sources include servers, workstations, network equipment, applications, and cloud services.

Available detail depends on configuration. A connection log may record addresses and ports without recording packet contents.

### Classroom Discussion

![Logging Examples](https://github.com/user-attachments/assets/0cb7dc4e-f9e3-42e5-a2a4-199912b6616f)

Give one example of a log from:

- A server.
- A workstation.
- A network device.

File access events may require auditing to be enabled. A system cannot report an event it never recorded.

A firewall connection log summarizes connection activity, while a packet capture provides packet-level evidence.

## 4. Investigation Questions

![Investigation Questions](https://github.com/user-attachments/assets/99575f89-c172-4dce-a848-5ee6a947fef5)

Use these questions to guide your review:

- What happened?
- When did it happen?
- Which system was involved?
- Which account or process was associated with the activity?
- Where did the connection originate?
- Did the action succeed?
- What other evidence supports the interpretation?

An account name alone does not establish personal responsibility. Correlate additional evidence before making that claim.

## 5. Why Log Analysis Matters

![Why Log Analysis Matters](https://github.com/user-attachments/assets/a2572dab-feb5-475c-a3b8-f98a834521c5)

Logs support:

- Troubleshooting.
- Security investigations.
- Incident response.
- Operational monitoring.
- Audit and retention requirements.

Like an aircraft’s flight recorder, logs help reconstruct activity before a problem.

Collecting logs alone does not stop an attack. Analysts must review and act on the evidence.

## 6. Key Terminology

![Basic Logging Terms](https://github.com/user-attachments/assets/4181c7c2-dc14-4e00-bd58-9666c8398090)

A **log file** can contain many **log entries**. Each entry describes an individual recorded event.

A timestamp needs enough context to interpret its year, time zone, and relationship to other events.

![Collection and Analysis Terms](https://github.com/user-attachments/assets/33165d1d-8ea3-45a6-8bec-75806ea0cd3b)

| Term | Meaning |
|---|---|
| Collection | Gathering records from sources |
| Aggregation | Bringing records together |
| Parsing | Extracting fields from records |
| Normalization | Mapping data to a shared structure |
| Correlation | Connecting events that may describe related activity |

**Discussion:** Which steps help compare Windows and Linux authentication records?

## 7. Common Log Types and Sources

![Common Log Types](https://github.com/user-attachments/assets/30e5e0d1-05f3-4666-9651-4d2b9967ffa6)

Applications, audit systems, security controls, and operating systems generate different records. These categories can overlap.

For example, an account change may be both an audit event and a security event.

![Additional Log Sources](https://github.com/user-attachments/assets/dba98371-1c36-412b-a993-aaecf159fc3b)

Additional sources include:

- Web access logs.
- Database audit logs.
- Cloud activity logs.
- Identity and authentication logs.
- Email gateway logs.
- Endpoint telemetry.

Each source adds context. Database queries and other detailed activity may require specific logging settings.

## 8. Log Collection

![Log Collection Workflow](https://github.com/user-attachments/assets/156c77a6-51ad-44ce-b9b2-106eefe6c621)

Good analysis starts with reliable collection.

1. Identify the sources and investigation requirements.
2. Select a supported collection method.
3. Configure relevant events and secure transport.
4. Synchronize clocks and record time zones.
5. Generate a test event.
6. Confirm arrival, completeness, and retention.

Collection methods may include rsyslog, syslog-ng, Splunk forwarders, Windows Event Forwarding, and cloud APIs.

![Log Collection Steps](https://github.com/user-attachments/assets/39df76ce-4358-454d-aaf8-c0be57a20acb)

A configured pipeline still needs validation. Confirm that the expected event reaches the collector with usable fields and timestamps.

## 9. Log Normalization

![Log Normalization](https://github.com/user-attachments/assets/3bf01a1d-52f3-4b48-b01b-bed844633dbb)

Different systems use different field names and formats. Normalization maps them to a shared structure so analysts can compare related activity.

Splunk add-ons and Common Information Model (CIM) mappings can support normalization at search time. Suitable configuration is required.

## 10. Manual and Automated Analysis

### Manual Log Analysis

![Manual Log Analysis](https://github.com/user-attachments/assets/28cfb36b-e876-4600-b14f-9cf2a57b66e8)

Manual analysis involves examining records directly with tools such as:

```bash
grep
less
cat
head
tail
```

It is useful for focused investigations and detailed review of individual records.

### Automated Log Analysis

![Automated Log Analysis](https://github.com/user-attachments/assets/763c1094-c2b2-4d41-98ad-4f31f4e64cd5)

Automated tools help analysts collect, search, filter, visualize, and alert on large volumes of events.

An alert starts an investigation. Review supporting records and context before classifying the activity.

## 11. Windows Security Logs

![Windows Security Logs](https://github.com/user-attachments/assets/128e47cb-98eb-468d-a9c5-bfec0cf84a52)

Windows Security logs can record sign-ins, account lockouts, account changes, and other audited activity.

Open:

```text
Event Viewer > Windows Logs > Security
```

Available events depend on audit settings. Review the event ID together with the account, timestamp, source, and result.

### Event Viewer Interface

![Event Viewer Interface](https://github.com/user-attachments/assets/3acd1417-6430-4509-81bf-c9809a031ff7)

| Pane | Purpose |
|---|---|
| Navigation | Select log categories and custom views |
| Main pane | Review events, summaries, and selected event details |
| Actions | Filter, export, and manage available views and logs |

### Failed Logon Example

![Windows Failed Logon Example](https://github.com/user-attachments/assets/fceae1e4-57fb-4aaa-a5be-c43751cb9095)

Event **4625** records a failed logon. Logon type **10** describes RemoteInteractive activity.

Review **Failure Reason**, **Status**, and **SubStatus** before deciding why authentication failed.

A single failed logon does not prove an attack.

### Event IDs as Investigation Starting Points

![Windows Authentication Event IDs](https://github.com/user-attachments/assets/e3ebddf5-d22d-4cd3-89fa-9b502d4e0cda)

Successful and privileged logons may be routine. Compare the account, host, time, and surrounding events.

![Windows Account and Process Events](https://github.com/user-attachments/assets/ffa250cb-5eb3-4ea2-b53b-16530410c4d9)

Process creation and account changes can help explain activity after a sign-in.

- Windows **4688** requires process creation auditing.
- Command-line details require additional configuration.
- Windows **1102** records clearing of the Security audit log.
- Sysmon **1** records process creation.
- Sysmon **3** records network connections when enabled.

Windows and Sysmon use different providers. Their event IDs are not interchangeable.

![Windows Event ID Reference](https://github.com/user-attachments/assets/6344b56a-01ab-4755-86c5-d0a4b41de032)

Reference: [Microsoft — Event Viewer](https://learn.microsoft.com/en-us/shows/inside/event-viewer)

## 12. Linux Logs

![Common Linux Log Locations](https://github.com/user-attachments/assets/6d71b985-00aa-4976-b330-f053b8eef845)

Linux log locations depend on the distribution and logging configuration.

| Location or Command | Typical Use |
|---|---|
| `/var/log/auth.log` | Authentication on suitably configured Debian/Ubuntu systems |
| `/var/log/secure` | Authentication on suitably configured RHEL-like systems |
| `/var/log/syslog` or `/var/log/messages` | General system and service messages |
| `/var/log/audit/audit.log` | auditd events when enabled |
| `journalctl` | Query the systemd journal |
| `journalctl -k` | Query kernel journal messages |
| `dmesg` | Display the kernel ring buffer |

### Linux auth.log Example

![Linux Authentication Log Example](https://github.com/user-attachments/assets/7cfd8313-be00-42d0-ab39-6e0e8fb710c4)

```text
Apr 7 10:42:15 ubuntu sshd[12345]: Failed password for invalid user C3team from 192.168.1.100 port 54321 ssh2
```

| Field | Value | Meaning |
|---|---|---|
| Timestamp | `Apr 7 10:42:15` | Recorded date and time; year and time zone are absent |
| Hostname | `ubuntu` | System that generated the entry |
| Service and PID | `sshd[12345]` | SSH server process and its identifier |
| Authentication result | `Failed password` | Password authentication failed |
| Attempted username | `C3team` | Username considered invalid for this attempt |
| Source IP | `192.168.1.100` | Connecting host’s address as observed by the server |
| Source port | `54321` | Client-side connection port |
| Protocol | `ssh2` | SSH version 2 |

Check for repeated failures, other attempted usernames, and nearby successful logins. Confirm the year, time zone, and clock accuracy before correlating records.

### Windows and Linux Comparison

![Windows and Linux Log Comparison](https://github.com/user-attachments/assets/3c2bdb0c-2d68-4e6b-9db0-8635be2227d3)

Both platforms provide useful evidence. Windows commonly uses structured events and event IDs; Linux may use text files or the systemd journal.

Apply the same investigation questions to both platforms. Consistent fields and accurate timestamps make correlation easier.

Reference: [systemd — journalctl](https://www.freedesktop.org/software/systemd/man/latest/journalctl.html)

## 13. Practice 1 — Phishing Email Review

![Phishing Email Review](https://github.com/user-attachments/assets/eaa99dd0-d69c-405d-be1c-22389bbad322)

Begin with the email itself:

- Compare the display name, From, and Reply-To.
- Inspect the actual link target and attachment details.
- Look for urgency, unusual requests, and mismatched domains.
- Record the recipient, message ID, and delivery time.
- Preserve the original message and headers.

Use the synthetic classroom message for this exercise.

### Email Header Analysis

![Email Header Analysis](https://github.com/user-attachments/assets/779dcb0e-2140-4746-b317-601dbd24824c)

Email headers provide routing and authentication clues.

Read **Received** entries from bottom to top, starting with the earliest listed hop. Focus on records added by trusted mail systems because untrusted fields can be forged.

**SPF, DKIM, and DMARC** perform specific authentication checks. Passing results do not prove that an email is safe.

### MxToolbox Demonstration

![MxToolbox Email Header Demo](https://github.com/user-attachments/assets/f72965b6-7081-4373-8688-5d70020dd469)

1. Open the [Email Header Analyzer](https://mxtoolbox.com/EmailHeaders.aspx).
2. Paste the provided synthetic header.
3. Review delivery hops and timestamp differences.
4. Compare sender-related fields.
5. Review authentication results.
6. Record one mismatch and one finding that needs verification.

**Question:** Does an SPF pass alone make the email safe?

**Expected answer:** No.

## 14. Splunk — Raw Events and Extracted Fields

![Splunk Raw Events and Extracted Fields](https://github.com/user-attachments/assets/e32139cb-8348-41b9-abed-13adb825a0c4)

Splunk supports searching large volumes of events while retaining access to the raw records.

Extracted fields help analysts filter results and connect related activity. Their availability depends on data onboarding and extraction settings.

### Lab Tasks

- Open the class Splunk lab.
- Inspect the raw event.
- Identify `host`, `source`, and `sourcetype`.
- Compare extracted fields with the original record.
- Review sender, recipient, link, attachment, and delivery status.

**Key point:** `delivery_status=delivered` describes delivery, not whether the content is trustworthy.

Reference: [Splunk — Use the CIM to Normalize Data at Search Time](https://help.splunk.com/en/data-management/common-information-model/6.0/using-the-common-information-model/use-the-cim-to-normalize-data-at-search-time)

## 15. Wireshark — Network Traffic Triage

![Wireshark Network Traffic Triage](https://github.com/user-attachments/assets/190f864e-049e-4ab9-b156-a6537491dc82)

Wireshark analyzes packet captures. A PCAP provides packet-level evidence that can be correlated with endpoint, firewall, and application logs.

| Investigation Focus | Display Filter |
|---|---|
| Specific host | `ip.addr == 192.168.1.10` |
| HTTP traffic | `http` |
| TCP port 443 | `tcp.port == 443` |
| Keyword in packet bytes | `frame contains "login"` |

A filter match is a clue that requires context.

### Session Review

![Wireshark Session Review](https://github.com/user-attachments/assets/420054e4-1305-4117-ba91-1b8d921e00c1)

Review:

- The TCP handshake.
- The HTTP request URI.
- The response content and transfer size.
- The start of session closure.
- Related endpoint and application activity.

### Demo Tasks

1. Select a consistent display under **View > Time Display Format**.
2. Open **Statistics > Conversations**.
3. Identify communications involving the case workstation.
4. Review the relevant stream and surrounding packets.

Content visibility depends on encryption, capture position, and completeness.

## 16. CyberChef — Data Transformation and Triage

![CyberChef Log Analysis](https://github.com/user-attachments/assets/8f85a312-2cd8-4796-a2eb-bb10dd7d22c7)

CyberChef supports decoding, extraction, parsing, and transformation during investigations.

| Operation | Investigation Use |
|---|---|
| From Base64 | Decode encoded content for inspection |
| Extract IP Addresses | Identify candidate addresses for validation |
| Parse DateTime | Convert timestamps to a consistent readable format |
| Find / Highlight | Locate relevant terms and patterns |

Validate transformed output against the original input.

### Synthetic Log Dataset

The following records are simplified classroom examples, rather than native product log exports.

```text
Jul 07 09:30:15 mail-gateway postfix/smtpd[12345]: from=<payments@finance-invoice-2026.com>, to=<john.nash@cybertechllc.com>, message-id=<20260703093015.abc123@finance-invoice-2026.com>, status=sent, relay=10.10.15.23[10.10.15.23]:25, size=5832

Jul 07 09:30:16 mail-gateway spamd[23412]: Email from payments@finance-invoice-2026.com scored 5.2 (Phishing heuristics + Suspicious link), severity=medium

Jul 07 09:30:18 webproxy01 squid[41251]: CONNECT finance-invoice-2026.com:80 [185.72.94.11] user=john.nash@cybertechllc.com uri=/invoice-access

Jul 07 09:30:19 webproxy01 squid[41251]: GET http://finance-invoice-2026.com/invoice-access attachment=invoice_98231.pdf status=200 size=87233 content-type=application/pdf

Jul 07 09:30:21 endpoint-win10 Sysmon EventID=1: Process Create - Image=powershell.exe

Jul 07 09:30:25 endpoint-win10 Sysmon EventID=3: Network Connection - Image=powershell.exe DestinationIp=185.72.94.11 DestinationPort=80 DestinationHostname=finance-invoice-2026.com
```

**Review focus:** Connect the email, proxy request, and endpoint activity using timestamps and relevant fields. Temporal proximity alone does not establish that the PDF caused PowerShell execution.

### Domain Extraction

![CyberChef Domain Analysis](https://github.com/user-attachments/assets/05cba7bc-9169-4979-8501-aefe3c6f36d2)

Use CyberChef’s built-in **Domain** regex to identify candidate domain strings.

Additional patterns, such as **Email address**, **URL**, and **Windows file path**, can help locate values for correlation.

Validate each match against the original record. A regex match alone does not establish that a value is a malicious indicator.


## Thank You

![Thank You](https://github.com/user-attachments/assets/58a39ef3-7bb5-4bf6-81f7-5f035287687b)
