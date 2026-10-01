<img width="1038" height="580" alt="image" src="https://github.com/user-attachments/assets/9b453562-c6f0-4205-a0df-de11f2dc453d" />


Hello everyone! Welcome to this presentation about log analysis. Today, we will learn what logs are, why they matter, and how to use them in an investigation. We will look at Windows and Linux logs. Then we will connect an email, log searches, network traffic, and decoded data in one practice case.

<img width="1040" height="575" alt="image" src="https://github.com/user-attachments/assets/3e6834c3-5c80-4c37-9e02-576b6ffdf1ab" />

By the end of this lesson, you should be able to explain logging, identify sources, and read basic events. You will also practice connecting evidence from different tools. You do not need to memorize every event ID. You need to know where to look and which questions to ask. Let’s begin!

<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/58e1e400-5f38-4957-a705-302d65251515" />
A log is a record of something that happened. For example, a user may sign in, a program may fail, or a firewall may block a connection. Log analysis helps us understand these events. We start with a question and use the records to find an answer. An alert points us to possible activity. We still need to read the event details.

<img width="1039" height="575" alt="image" src="https://github.com/user-attachments/assets/73122da5-6338-4339-8f1e-8989ac5ff809" />
Open an event and read the full description. Different logs contain different fields. A source may identify a host, service, or provider. A user and an IP address are separate pieces of context. Some logs record a result without a severity level. This table uses illustrative values.

<img width="1037" height="571" alt="image" src="https://github.com/user-attachments/assets/8f18afc7-9109-4c97-932f-f48b166da89d" />

**0 — Emergency:** The system is unusable. This is the highest severity level.  
**1 — Alert:** Immediate action is required.  
**2 — Critical:** A serious system problem.  
**3 — Error:** An error in an operation or component.  
**4 — Warning:** A potential problem that needs attention.  
**5 — Notice:** A normal but significant event.  
**6 — Informational:** Information about system activity.  
**7 — Debug:** Detailed technical information for troubleshooting.

**The lower the number, the higher the severity and urgency.**

<img width="1033" height="576" alt="image" src="https://github.com/user-attachments/assets/ade818d6-4a89-40ca-b46c-1009def23f83" />

Raw logs can look messy. First, we collect them. Next, we filter the records that relate to our question. We extract fields and use consistent names. Then we compare events and build a timeline or a simple chart. Finally, we explain what the evidence supports and what we still need to verify. This is how scattered records become a useful investigation.

<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/1a7e0012-3af6-4acf-9b63-a2665cc23404" />

Logging means keeping a record of what happens in a system or device. Servers, computers, routers, applications, and cloud services can all create logs. The available detail depends on the configuration. A network log may show addresses and ports without recording the packet contents.

<img width="999" height="556" alt="image" src="https://github.com/user-attachments/assets/0cb7dc4e-f9e3-42e5-a2a4-199912b6616f" />
Ask students to give one example of a log from a server, a workstation, and a network device. Explain that recording file access events may require auditing to be enabled. A system cannot report an event it never recorded. A firewall connection log and a packet capture provide different types of information: the log summarizes connection activity, while the capture shows packet-level details.

<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/99575f89-c172-4dce-a848-5ee6a947fef5" />

Think of yourself as Sherlock Holmes. You are investigating a mystery in your network. Ask what happened, when it happened, and where it happened. Then ask which account or process took the action and whether it worked. An account name alone does not prove which person was responsible. Compare other evidence before making that claim.​

<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/a2572dab-feb5-475c-a3b8-f98a834521c5" />
Logs are like the black box in an aircraft. They help us understand what happened before a problem. They support troubleshooting and security investigations. Some regulations, contracts, and organizational policies require certain records and retention periods. The exact requirements depend on the organization. Logs support a response, but collecting logs alone does not stop an attack.​

<img width="976" height="530" alt="image" src="https://github.com/user-attachments/assets/4181c7c2-dc14-4e00-bd58-9666c8398090" />
These are the first five terms. Ask one student to explain the difference between a log file and a log entry. A file can contain many entries. A timestamp should include enough context to understand its time zone.​

<img width="951" height="526" alt="image" src="https://github.com/user-attachments/assets/33165d1d-8ea3-45a6-8bec-75806ea0cd3b" />

These are the next five terms. Collection gathers the records. Aggregation brings them together. Parsing extracts fields, such as user and IP. Normalization uses a shared structure. Correlation connects events that may describe the same activity. Ask students which step helps compare Windows and Linux authentication records.​

<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/30e5e0d1-05f3-4666-9651-4d2b9967ffa6" />

Applications, audit systems, security controls, and operating systems create different records. These categories can overlap. For example, an account change may be both an audit event and a security event. Each source adds another piece to the investigation.​

<img width="977" height="527" alt="image" src="https://github.com/user-attachments/assets/dba98371-1c36-412b-a993-aaecf159fc3b" />

A web access log can show which path a client requested and the response code. A database may log queries if the correct setting is enabled. Cloud and identity events help us investigate sign-ins and changes. Together, the sources help us build a fuller picture.​

<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/156c77a6-51ad-44ce-b9b2-106eefe6c621" />

Good analysis starts with good collection. Identify the source first. Then choose a supported collection method. Examples include rsyslog, syslog-ng, a Splunk forwarder, Windows Event Forwarding, or a cloud API. Configure the events you need. Check the time settings. Finally, generate a test event and confirm that the collector receives it. Do not assume the pipeline works just because it is configured.​

<img width="949" height="536" alt="image" src="https://github.com/user-attachments/assets/39df76ce-4358-454d-aaf8-c0be57a20acb" />

**Log Collection Steps​:​**

1. Identify Source – Web server, database, router, etc.​

2. Use Collector – Tools like rsyslog, syslog-ng.​

3. Set Parameters – IPs, URLs, request types, timestamps.​

4. Sync Time – Critical for accurate sequencing and security.​

​<img width="970" height="537" alt="image" src="https://github.com/user-attachments/assets/3bf01a1d-52f3-4b48-b01b-bed844633dbb" />

Different systems use different names and formats. Normalization maps those fields to a shared structure. This example uses simple teaching field names. A platform may use a different schema. Splunk can extract fields and use add-ons and the Common Information Model to normalize data at search time. This requires suitable configuration. It does not happen automatically for every log source.​

<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/28cfb36b-e876-4600-b14f-9cf2a57b66e8" />

If you are manually examining(egzamining) logs yourself using commands like grep, cat, or tail line by line, then it is called manual log analysis.  ​

<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/763c1094-c2b2-4d41-98ad-4f31f4e64cd5" />

Automated tools help us work at a larger scale. They can collect, filter, search, and create alerts. Some products also automate response actions when configured. An alert is the start of the investigation. Read the supporting events and check the context before deciding what happened.​

<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/128e47cb-98eb-468d-a9c5-bfec0cf84a52" />

Windows Security logs can show successful and failed sign-ins, account lockouts, and account changes. Open Event Viewer and go to Windows Logs, then Security. The system records events according to the audit settings. Read the event ID together with the account, time, source, and result.​

<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/3acd1417-6430-4509-81bf-c9809a031ff7" />

Now that we understand what Windows Event IDs mean, let’s take a look at where to find them in the Event Viewer interface.​

**Event Viewer has three main sections:**​

Left Panel (Navigation): Lists all log categories like Security, System, and Application logs.​

Medium Panel (Details): Shows summaries and details of selected logs, including recent activity and log sizes.​

Right Panel (Actions): Lets you save logs, create custom views, or connect to remote systems.​

Together, these sections help you easily find, review, and manage system logs.​

So far, we have discussed what logging means, what it reveals, why it is important itself and in investigation, some terms, types of logs,  log collection and normalization, and most importantly, manual and automated log analysis. ​

<img width="991" height="536" alt="image" src="https://github.com/user-attachments/assets/fceae1e4-57fb-4aaa-a5be-c43751cb9095" />

This is an illustrative example, not a screenshot from a real investigation. Event 4625 means a failed logon. Logon type 10 describes RemoteInteractive. The event can include the source address and account. The failure can have several causes. Do not assume the password was wrong without reading Failure Reason, Status, and SubStatus. A single failed login is not enough to prove an attack.​

<img width="992" height="536" alt="image" src="https://github.com/user-attachments/assets/e3ebddf5-d22d-4cd3-89fa-9b502d4e0cda" />

Use these IDs as starting points. A successful logon may be routine. A privileged logon may also be routine for a service or administrator. Compare the account, host, time, and surrounding events. Availability depends on audit policy. Use Microsoft’s auditing documentation and the event ID reference shared on Discord.​

<img width="978" height="535" alt="image" src="https://github.com/user-attachments/assets/ffa250cb-5eb3-4ea2-b53b-16530410c4d9" />

Account changes and new processes can help explain what happened after a sign-in. Event 4688 requires process creation auditing. Command-line detail requires an additional setting. Event 1102 can be important, but confirm whether a planned maintenance task explains it. Windows 4688 and Sysmon event 1 come from different providers. Do not mix their IDs.​

<img width="1445" height="720" alt="image" src="https://github.com/user-attachments/assets/6344b56a-01ab-4755-86c5-d0a4b41de032" />

 This slide explains important Windows Event IDs to help detect logins, account issues, and suspicious activity. We also have a website that explains these in detail, which we share on our Discord channel as an extra resource for students and actively use during Splunk CTFs.​
https://learn.microsoft.com/en-us/shows/inside/event-viewer ​
​
<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/6d71b985-00aa-4976-b330-f053b8eef845" />

Linux log locations depend on the distribution and configuration. Some systems use the journal without separate text files. auth.log is common on Debian and Ubuntu systems with suitable logging. secure is common on RHEL-like systems. syslog and messages are alternatives used by different distributions. auditd can record detailed audit events. dmesg displays the kernel ring buffer. journalctl -k queries kernel journal messages.​

<img width="1419" height="736" alt="image" src="https://github.com/user-attachments/assets/7cfd8313-be00-42d0-ab39-6e0e8fb710c4" />

## Linux auth.log Example

Now, let’s examine a sample Linux authentication log entry and break down its fields.

On systems configured to use this file, authentication logs are stored in **/var/log/auth.log**. These logs record authentication-related events, including SSH login attempts and sudo activity.

In this example, the system named **ubuntu** recorded a failed SSH password authentication attempt for **C3team**. The SSH service considered the username invalid. The connection came from **192.168.1.100**, using source port **54321** and SSH version 2.

The entry tells us when the event was recorded, which system and process recorded it, which username was attempted, and where the connection originated.

One failed attempt does not prove an attack. We should check for repeated failures, attempts against other usernames, and any successful logins nearby. Before comparing this entry with other logs, we should also confirm the year, time zone, and clock accuracy.

```text
Apr 7 10:42:15 ubuntu sshd[12345]: Failed password for invalid user C3team from 192.168.1.100 port 54321 ssh2
```

**Explanation:**

- *Apr 7 10:42:15* — **Timestamp:** The recorded date and time. The year and time zone are not included.
- *ubuntu* — **Hostname:** The system that generated the log entry.
- *sshd[12345]* — **Service name and process ID:** The SSH server process, with PID **12345**.
- *Failed password* — **Authentication result:** Password authentication was unsuccessful.
- *invalid user C3team* — **Attempted username:** The SSH service did not recognize **C3team** as a valid user for this attempt.
- *from 192.168.1.100* — **Source IP address:** The connecting host’s address as observed by the server.
- *port 54321* — **Source port:** The client-side port used for the connection.
- *ssh2* — **Protocol version:** SSH version 2.

<img width="1444" height="736" alt="image" src="https://github.com/user-attachments/assets/3c2bdb0c-2d68-4e6b-9db0-8635be2227d3" />
​
Both systems record useful events. Windows often uses structured events and event IDs. Linux may use text files or the systemd journal. We ask the same investigation questions on both: who, what, when, where, and result. Consistent field names and accurate time help us compare the records.​
Sources:​
https://www.freedesktop.org/software/systemd/man/latest/journalctl.html​

<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/eaa99dd0-d69c-405d-be1c-22389bbad322" />

Begin with the email itself. A familiar display name can hide a different address. Check the actual link target. An urgent invoice request is a clue, not proof. Record the message ID and recipient so you can connect the message to mail gateway and endpoint records. For a classroom demo, use the synthetic message. Do not open the suspected link to test it.​

<img width="978" height="529" alt="image" src="https://github.com/user-attachments/assets/779dcb0e-2140-4746-b317-601dbd24824c" />

Email headers provide clues about a message’s route and authentication results. Read the **Received** entries from bottom to top, starting with the earliest listed hop. Focus on entries added by trusted mail systems, because untrusted header fields can be forged.

**SPF, DKIM, and DMARC** perform specific authentication checks. A passing result does not prove that an email is safe. Phishing messages can also come from legitimate senders or compromised accounts.

<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/f72965b6-7081-4373-8688-5d70020dd469" />

DEMO: Open https://mxtoolbox.com/EmailHeaders.aspx and paste a synthetic header. Show how the tool makes the header easier to read. Compare Received times and sender fields. Ask students whether an SPF pass alone makes the email safe. Expected answer: no. Use only the synthetic example for the online demonstration.​
Sources:​
https://mxtoolbox.com/EmailHeaders.aspx​

​<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/e32139cb-8348-41b9-abed-13adb825a0c4" />

Splunk helps search large volumes of events. We can still view the raw record and examine extracted fields. Data onboarding and field extraction settings determine which fields are available. Add-ons and Common Information Model mappings support normalization. Open the class Splunk lab and show the raw event, host, source, sourcetype, and extracted fields.​
Sources:​
https://help.splunk.com/en/data-management/common-information-model/6.0/using-the-common-information-model/use-the-cim-to-normalize-data-at-search-time​

<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/190f864e-049e-4ab9-b156-a6537491dc82" />

<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/420054e4-1305-4117-ba91-1b8d921e00c1" />

Wireshark analyzes packet captures. A PCAP provides packet-level evidence that we can correlate with endpoint, firewall, and application logs.

In the first slide, we use display filters to focus on a host, HTTP traffic, a TCP port, or a keyword in packet bytes. A filter match is an investigation clue, not a verdict.

In the second slide, we review the TCP handshake, HTTP request and response, and the start of session closure. We examine the requested URI and response content, then compare the activity with expected behavior and related logs.

For the demo, select a consistent time display under **View > Time Display Format**. Then open **Statistics > Conversations** to identify communications involving the case workstation.

Content visibility depends on encryption, capture position, and capture completeness. Confirm the available evidence before classifying the activity.

<img width="1812" height="868" alt="image" src="https://github.com/user-attachments/assets/8f85a312-2cd8-4796-a2eb-bb10dd7d22c7" />


_Jul 07 09:30:15 mail-gateway postfix/smtpd[12345]: from=<payments@finance-invoice-2026.com>, to=<john.nash@cybertechllc.com>, message-id=<20260703093015.abc123@finance-invoice-2026.com>, status=sent, relay=10.10.15.23[10.10.15.23]:25, size=5832

Jul 07 09:30:16 mail-gateway spamd[23412]: Email from payments@finance-invoice-2026.com scored 5.2 (Phishing heuristics + Suspicious link), severity=medium

Jul 07 09:30:18 webproxy01 squid[41251]: CONNECT finance-invoice-2026.com:80 [185.72.94.11] user=john.nash@cybertechllc.com uri=/invoice-access

Jul 07 09:30:19 webproxy01 squid[41251]: GET http://finance-invoice-2026.com/invoice-access attachment=invoice_98231.pdf status=200 size=87233 content-type=application/pdf

Jul 07 09:30:21 endpoint-win10 sysmon[13]: Process Create - powershell.exe invoked with command to open invoice_98231.pdf

Jul 07 09:30:25 endpoint-win10 sysmon[3]: Network Connection - powershell.exe made connection to 185.72.94.11:80 (finance-invoice-2026.com)_


<img width="1744" height="902" alt="image" src="https://github.com/user-attachments/assets/05cba7bc-9169-4979-8501-aefe3c6f36d2" />

​





















