# Phishing Analysis and Investigation

<img width="1536" height="1024" alt="social-engineering" src="https://github.com/user-attachments/assets/85ac358a-88de-4211-ad01-f7579a77a0af" />

## Table of Contents

- [Introduction](#introduction)
- [Email Fundamentals](#email-fundamentals)
- [Phishing Types and Techniques](#phishing-types-and-techniques)
  - [Social Engineering Tactics](#social-engineering-tactics)
- [1. Preserve and Prepare](#1-preserve-and-prepare)
- [2. Triage the Message](#2-triage-the-message)
- [3. Review Headers and Authentication](#3-review-headers-and-authentication)
  - [Authentication Result Reference](#authentication-result-reference)
- [4. Analyze URLs and Infrastructure](#4-analyze-urls-and-infrastructure)
- [5. Analyze Attachments](#5-analyze-attachments)
  - [Static Analysis](#static-analysis)
  - [File Hash Collection](#file-hash-collection)
  - [Dynamic Analysis](#dynamic-analysis)
- [6. Determine Scope and Impact](#6-determine-scope-and-impact)
- [7. Decide and Respond](#7-decide-and-respond)
  - [Preventive Controls](#preventive-controls)
- [8. Case Report Template](#8-case-report-template)
- [9. Tool Reference](#10-tool-reference)
- [Key Reminders](#key-reminders)
- [Challenge: Identify the Phishing Email](#challenge-identify-the-phishing-email)
  - [Scenario](#scenario)
  - [Instructions](#instructions)
  - [Questions and Answers](#questions-and-answers)
    
## Introduction

**Phishing is a social engineering attack** that uses impersonation and deceptive messages to trick people into revealing sensitive information, clicking malicious links, executing harmful files, or approving fraudulent requests. An attack may also request payments or MFA approval without including a malicious link.

Attackers exploit **trust, curiosity, fear, and urgency** to influence decisions. Their goal is to steal credentials, financial details, or personal information; gain unauthorized access to accounts and systems; or commit fraud. Even well-informed users can be targeted by convincing messages, making user awareness and technical controls essential.

This practical SOC study guide combines **BTL1 fundamentals** with an **evidence-based investigation workflow**. It covers how to investigate reported emails, analyze artifacts, assess user impact, and document a clear verdict and appropriate response.

## Email Fundamentals

| Component | Purpose |
| --- | --- |
| SMTP | Transfers email between mail systems. |
| POP3 | Retrieves messages from a mail server. |
| IMAP | Provides access to messages stored on a mail server. |
| Headers | Contain sender, recipient, routing, and timestamp information. |
| Body | Contains message text, HTML, links, and references to embedded content. |

**Key headers:** `From`, `To`, `Reply-To`, `Return-Path`, `Subject`, `Date`, `Message-ID`, and `Received`.

## Phishing Types and Techniques

| Type or technique | Main indicator or objective |
| --- | --- |
| Reconnaissance | Tests mailbox activity through replies, delivery results, or tracking pixels. |
| Credential harvesting | Uses fake login pages to steal credentials. |
| Malware delivery | Distributes malicious attachments or links to hosted payloads. |
| Spear phishing | Uses information about a specific target to build a convincing message. |
| Whaling | Targets executives or other high-value individuals. |
| SPIM | Delivers unsolicited messages through instant messaging; these may contain phishing lures. |
| Impersonation and BEC | Poses as a trusted person or organization to request money, data, or access. |
| Sender spoofing | Falsifies the visible sender address. |
| Typosquatting and homographs | Uses misspellings or similar-looking characters in domains. |
| Vishing and smishing | Uses voice calls or text messages for social engineering. |

Common warning signs include urgency, threats, unexpected requests, mismatched links, shortened URLs, misleading filenames, and password-protected attachments.

**Spam** is unsolicited email and is not always malicious. A **false positive** is a legitimate message incorrectly flagged as malicious.

### Social engineering tactics

<img width="1536" height="1024" alt="phishing-investigation" src="https://github.com/user-attachments/assets/47a455b7-5dd6-4da8-a54c-578b3b146931" />

| Tactic | How it influences the recipient |
| --- | --- |
| Authority | Impersonates an executive, administrator, or trusted institution. |
| Intimidation | Threatens penalties, account closure, or other consequences. |
| Consensus | Claims that colleagues or other people already complied. |
| Scarcity | Presents a limited opportunity or resource. |
| Urgency | Pressures the recipient to act before checking the request. |
| Familiarity and trust | Uses known names, relationships, branding, or conversation context. |

## 1. Preserve and prepare

- Create a case with the analyst, reporting user, affected mailbox, report time, and business context.
- Preserve the original `.eml` or `.msg`, including full headers, body, and attachments. Record acquisition details and SHA-256; analyze a working copy.
- Use a controlled analysis environment. Block remote content and avoid opening links or attachments during initial review.
- Check data-handling requirements before submitting messages, URLs, or files to external services. Prefer existing reports and hash lookups.
- Record timeline events in UTC while retaining original timestamps and offsets.

## 2. Triage the message

Check:

- **Context:** Was the message, payment, attachment, or conversation expected?
- **Identity:** Compare display name, `From`, `Reply-To`, and `Return-Path`. Look for impersonation and lookalike domains.
- **Request:** Identify urgency, secrecy, unusual payment instructions, credential requests, or attempts to bypass normal procedures.
- **Links:** Extract actual destinations from message source and compare them with displayed text. Preserve exact URLs in restricted evidence and defang reporting copies.
- **Attachments:** Record names, sizes, extensions, actual file types, and hashes.

Verify unusual requests through a known independent channel. Public email providers, spelling errors, and unusual wording are contextual signals; none establishes a verdict alone.

## 3. Review headers and authentication

Preserve complete headers and record `From`, `Reply-To`, `Return-Path`, recipients, subject, `Message-ID`, `Date`, `Received`, and `Authentication-Results`.

Read `Received` headers from bottom to top for apparent delivery order, then identify the trusted receiving infrastructure. Earlier headers may be forged. Prioritize the connecting IP recorded by a trusted receiver.

| Mechanism | Check | Limitation |
| --- | --- | --- |
| SPF | Sending server authorization for the envelope sender domain | Does not independently authenticate the visible `From` |
| DKIM | Signature validity for the signing domain | A valid signature does not establish safe content |
| DMARC | Visible `From` alignment with passing SPF or DKIM | Requires at least one passing, aligned mechanism |

Record results from trusted receiving infrastructure, including relevant domains and alignment. Forwarding or message modification can affect results; compromised legitimate accounts can pass authentication. [R3, R4]

### Authentication result reference

**SPF (Sender Policy Framework)** evaluates the connecting server against the DNS-published policy for the envelope sender domain, or HELO identity when applicable. It does not directly verify the visible sender address.

| SPF result | Meaning |
| --- | --- |
| `pass` | The evaluated policy authorizes the sending server. |
| `fail` | The policy explicitly does not authorize the server, commonly through `-all`. |
| `softfail` | The policy indicates the server is probably unauthorized, commonly through `~all`. |
| `neutral` | The policy makes no assertion about authorization. |
| `none` | No applicable SPF policy was available. |
| `temperror` / `permerror` | A temporary evaluation problem or a permanent policy error occurred. |

**DKIM (DomainKeys Identified Mail)** verifies a signature using a public key published in DNS. The sender retains the private key; the message carries a `DKIM-Signature` header, not the private key.

| DKIM result | Meaning |
| --- | --- |
| `pass` | The evaluated signature verified successfully. |
| `fail` | Verification failed; investigate modification, signing issues, or forgery. |
| `none` | The message was not signed. |
| `neutral` | A signature was present but could not be processed, such as because of syntax problems. |
| `temperror` / `permerror` | A temporary or permanent verification problem occurred. |

**DMARC (Domain-based Message Authentication, Reporting, and Conformance)** checks whether the visible `From` domain aligns with at least one passing SPF or DKIM identity. Its published policy requests handling for messages that fail DMARC:

| Policy | Requested handling |
| --- | --- |
| `p=none` | No specific disposition requested; supports monitoring and reporting. |
| `p=quarantine` | Treat failing messages as suspicious, such as by placing them in spam. |
| `p=reject` | Reject failing messages. |

Receiver policy can affect the final disposition. Record authentication results and delivery outcome separately.

## 4. Analyze URLs and infrastructure

1. Review existing reputation reports for the full URL, hostname, domain, and relevant IPs.
2. Use RDAP/WHOIS for registration context and DNS for address resolution. Record lookup times.
3. Review existing web captures first. If necessary, use an approved isolated browser or web sandbox.
4. Record redirects, final destination, page content, downloads, and relevant network requests.
5. Correlate findings with other evidence before classifying or blocking indicators.

Clean reputation results do not prove safety. Domain age, registration privacy, shared hosting, and screenshots require context. Preserve the distinction between a malicious URL and its parent domain.

## 5. Analyze attachments

<img width="1536" height="1024" alt="evidence-analysis" src="https://github.com/user-attachments/assets/e0ae1d4c-53c9-43db-afbb-f3f9a56d6d86" />

### Static analysis

Without executing the file:

- Confirm actual file type and inspect archive contents.
- Calculate SHA-256; retain MD5 when needed for legacy report matching.
- Review metadata, strings, embedded URLs, macros, scripts, and document structure.
- Search existing reputation by hash before considering upload.

Common formats include executables and scripts (`.exe`, `.vbs`, `.js`, `.bat`, `.ps1`), HTML, archives, disk images, Office documents, and PDFs. File extensions alone do not establish safety. `.ps1` is PowerShell; `.ps` usually identifies PostScript.

### File hash collection

Calculate attachment hashes to identify files, compare them with threat intelligence, and document evidence without executing the attachment. Use SHA-256 as the primary identifier; collect MD5 or SHA-1 when needed to match existing reports.

Replace `attachment.bin` with the actual filename. Quote paths containing spaces.

**Windows PowerShell**

```powershell
# SHA-256 (default algorithm)
Get-FileHash -Path ".\attachment.bin"

# Explicit SHA-256
Get-FileHash -Path ".\attachment.bin" -Algorithm SHA256

# MD5
Get-FileHash -Path ".\attachment.bin" -Algorithm MD5

# SHA-1
Get-FileHash -Path ".\attachment.bin" -Algorithm SHA1
```

**Linux**

```bash
# SHA-256
sha256sum "attachment.bin"

# MD5
md5sum "attachment.bin"

# SHA-1
sha1sum "attachment.bin"
```

Record the filename, algorithm, and complete hash value in the investigation report. Search the hash in approved reputation tools before considering a file upload.

> A hash identifies file content; it does not establish whether the file is malicious. MD5 and SHA-1 are retained for compatibility with older intelligence, not as secure integrity guarantees.

### Dynamic analysis

When behavior must be established, use an approved sandbox suited to the artifact. Record process activity, command lines, file or registry changes, persistence, DNS queries, network connections, and downloads.

No observed malicious behavior is not proof of safety; execution may depend on user interaction, network access, or environmental conditions.

| Method | Meaning |
| --- | --- |
| Static analysis | Examine content without executing the payload |
| Reputation enrichment | Review existing intelligence |
| Active web analysis | Contact or render the destination in a controlled environment |
| Dynamic analysis | Execute the artifact and observe behavior |

Tools can support both static and dynamic analysis. Rendering documents or HTML may activate content or generate network requests. [R1, R2]

## 6. Determine scope and impact

Establish whether the user received, opened, clicked, downloaded, executed, entered credentials, approved MFA, granted application access, or transferred money.

Correlate email, SIEM, EDR, DNS, proxy, firewall, and identity logs. Search related messages using sender, subject, URL, attachment hash, recipients, delivery time, and Message-ID. Build a timeline and distinguish delivery from confirmed compromise.

## 7. Decide and respond

State the verdict, confidence, decisive evidence, conflicting findings, and remaining gaps. Use the organization's categories, such as legitimate, spam, suspicious, or malicious phishing.

Follow the applicable response playbook:

- Preserve evidence and quarantine or remove malicious messages.
- Block validated indicators after assessing scope and business impact.
- Escalate suspected endpoint compromise and isolate affected devices when warranted.
- For account compromise, coordinate password reset, session revocation, and identity review.
- Escalate payment fraud promptly through established business channels.
- Verify containment, search for related activity, and update detections.

Document actions, owners, authorization where required, and results. Thank the reporting user and explain relevant warning signs without blame.

### Preventive controls

- Configure SPF, DKIM, and DMARC for organizational domains.
- Apply spam filtering and content and sender-reputation checks at the email gateway.
- Label external messages to help users recognize messages from outside the organization.
- Restrict risky attachment types and use attachment sandboxing where appropriate.
- Maintain validated sender, domain, and URL blocks, considering business impact.
- Provide awareness training and controlled phishing simulations, with an easy reporting process.

## 8. Case report template

| Field | Required information |
| --- | --- |
| Summary | Case ID, analyst, severity, affected user, business context |
| Evidence | Original message location, acquisition details, SHA-256, attachment inventory |
| Email analysis | Sender fields, Message-ID, trusted routing, authentication results |
| Artifact analysis | URLs, domains, IPs, file hashes, report timestamps, key findings |
| Impact and scope | User actions, affected entities, correlated events, timeline |
| Assessment | Verdict, confidence, supporting evidence, limitations |
| Response | Actions, owners, approvals where required, outcomes, follow-up |

Separate observed facts from hypotheses. Defang indicators in reports and restrict access to sensitive evidence.

```text
URL:    hxxps://login[.]example[.]com/verify
IP:     192[.]0[.]2[.]10
Email:  sender@example[.]com
```

## 9. Tool reference

Use tools permitted by the organization's data-handling rules. External submissions and active scans can disclose information or contact attacker infrastructure. [R5, R6]

| Purpose | Tool | Official website |
| --- | --- | --- |
| Message review | Thunderbird | https://www.thunderbird.net/ |
| Email analysis | PhishTool | https://www.phishtool.com/ |
| Header analysis | MXToolbox | https://mxtoolbox.com/EmailHeaders.aspx |
| Header routing analysis | Google Admin Toolbox Messageheader | https://toolbox.googleapps.com/apps/messageheader/analyzeheader |
| Header routing analysis | Message Header Analyzer | https://mha.azurewebsites.net/ |
| SPF policy inspection | dmarcian SPF Survey | https://dmarcian.com/spf-survey/ |
| Decoding and indicator defanging | CyberChef | https://gchq.github.io/CyberChef/ |
| File and URL reputation | VirusTotal | https://www.virustotal.com/ |
| File reputation | Talos File Reputation | https://talosintelligence.com/ |
| Phishing intelligence | PhishTank | https://phishtank.com/ |
| Web reports and captures | URLscan | https://urlscan.io/ |
| Webpage screenshots | URL2PNG | https://www.url2png.com/ |
| Phishing URL analysis | CheckPhish | https://checkphish.ai/ |
| IP abuse intelligence | AbuseIPDB | https://www.abuseipdb.com/ |
| IP reputation | IPVoid | https://www.ipvoid.com/ |
| Registration lookup | ICANN Lookup | https://lookup.icann.org/ |
| Registration context | DomainTools | https://whois.domaintools.com/ |
| Dynamic analysis | Hybrid Analysis | https://hybrid-analysis.com/ |
| Interactive sandbox | ANY.RUN | https://any.run/ |
| Malware distribution intelligence | URLhaus | https://urlhaus.abuse.ch/ |

URLhaus focuses on malware distribution; use phishing-specific intelligence for credential-phishing sites. [R8]

## Challenge: Identify the Phishing Email

### Scenario

XYZ Corporation has recently onboarded two employees, **John Joe** and **John Smith**. Both received Gmail accounts for communicating with colleagues.

On the same day, the cybersecurity team conducted a phishing simulation as part of a security awareness review. The employees were unaware of the exercise.

Your task is to review John Smith’s emails in **`Mail.zip`**, identify the phishing message, and document the evidence supporting your findings.

### Instructions

1. Extract `Mail.zip` and review the messages using a text editor or an email client such as Mozilla Thunderbird or eM Client.
2. Inspect the headers, message content, and actual link destinations.
3. Use passive research and existing reputation reports to investigate the suspicious domain.
4. Record your findings and explain your reasoning.

> Treat all links and attachments as untrusted. Do not visit the linked website. Although the exercise originally described the links as non-malicious, their current status is unknown.

### Questions and Answers

### Questions and Answers

**Q1. Sender:** What email address sent the phishing message?

**Answer:** `jjoe96374@g.mail.com`

The suspicious email is titled **Weekly Meeting** and uses the display name **John Joe**. Although the address resembles Gmail, its domain is `g.mail.com`, not `gmail.com`.

![Sender address displayed in Thunderbird](assets/sender.png)

---

**Q2. Recipient:** What email address received it?

**Answer:** `smithyOG59@gmail.com`

The `To` header identifies John Smith’s mailbox as the recipient.

![Message headers showing the recipient](assets/headers.png)

---

**Q3. Sender domain:** What is the domain of the sender’s email address?

**Answer:** `g.mail.com`

The domain is the part after the `@` symbol:

```text
jjoe96374@g.mail.com → g.mail.com
```

The address can be mistaken for a Gmail account at a glance. Compare the complete address with the expected sender identity; a familiar display name does not establish legitimacy.

> Earlier notes list `gmail.com`, but the sender screenshots show `g.mail.com`. The source screenshot also contains authentication results referring to `gmail.com`; this inconsistency requires the original email for further verification.

![Sender evidence showing g.mail.com](assets/sender.png)

---

**Q4. Timestamp:** On what day and at what time was the message sent? Include the timezone or UTC offset.

**Answer:** Sunday, **5 November 2023 at 22:27:32 (UTC−07:00)**.

The message’s `Date` header records:

```text
Date: Sun, 5 Nov 2023 22:27:32 -0700
UTC equivalent: Mon, 6 Nov 2023 05:27:32 +0000
```

This is the stated send time. Delivery timestamps in `Received` headers should be examined separately.

![Message source showing the Date header](assets/headers.png)

---

**Q5. Linked domain:** What domain appears in the phishing link?

**Answer:** `101[.]com`

The recorded link contains the hostname `www[.]trusted[.]download[.]101[.]com`:

```text
hxxp://www[.]trusted[.]download[.]101[.]com/download.html
```

The presentation shows this destination in Thunderbird’s status bar when hovering over **“Please use this link.”** The registrable domain is `101[.]com`; `www.trusted.download` consists of subdomain labels.

> The earlier source screenshot’s plain-text body contains a different URL. Check the original email’s HTML to resolve the difference between message views.

![Actual hyperlink destination shown in Thunderbird](assets/link-destination-slide.png)

---

**Q6. Country:** What country is associated with the linked domain? State whether your evidence concerns registration or hosting location.

**Answer:** **United States**, according to the additional presentation’s hosting lookup. Earlier evidence shows **China**.

| Evidence | Associated IP | Recorded hosting location |
| --- | --- | --- |
| Additional presentation | `18[.]161[.]6[.]14` | Portland, Oregon, United States — Amazon |
| Earlier DomainTools screenshot | `36[.]42[.]77[.]164` | Weinan, Shaanxi, China — China Telecom |

These results describe **hosting/IP locations**, not the domain registrant’s or attacker’s location.

Different lookup times, DNS responses, or hosting changes could explain the difference. The supplied evidence does not establish which IP served the exact linked hostname when the email was delivered.

![Additional presentation showing United States hosting](assets/hosting-us-slide.png)

![Earlier DomainTools evidence showing China hosting](assets/domain-profile.png)

---

**Q7. Domain assessment:** Was the linked site more likely a compromised legitimate website or a domain created for malicious purposes? Support your conclusion with evidence and note any uncertainty.

**Answer:** More likely a **compromised legitimate domain**, consistent with the exercise’s expected answer. Confidence is limited.

The saved DomainTools profile shows approximately **20 years of IP history**, **19 years of hosting history**, and a preview of an established website. This supports an existing domain rather than one newly created for this campaign.

However, a long history does not prove legitimacy or compromise. The evidence does not establish who controlled the linked subdomain or what the page served during the incident.

The saved URL reputation report shows no vendor detections at that time. **A clean reputation result does not prove that a link is safe.**

![Domain history supporting the assessment](assets/domain-profile.png)

![Saved URL reputation results](assets/url-report.png)

---

### Investigation Summary

The **Weekly Meeting** email uses a familiar colleague’s identity and a routine meeting request to encourage the recipient to follow a link. The exercise identifies this message as the phishing attempt.

Key findings include:

- A sender address that resembles Gmail but uses `g.mail.com`.
- A meeting link pointing to a hostname under `101[.]com`.
- Conflicting historical hosting locations that require source and timestamp context.
- An existing domain that may have been abused or compromised.

The supplied evidence does not confirm credential harvesting, user interaction, or account or endpoint compromise.

![Weekly Meeting phishing message](assets/message.png)


<img width="1920" height="1055" alt="headers" src="https://github.com/user-attachments/assets/3c57087c-d005-4164-a7d5-78ecdb472f91" />
<img width="1908" height="982" alt="domain-profile" src="https://github.com/user-attachments/assets/84c48dfb-5a82-47e7-b83f-c551b0c3db72" />
<img width="1910" height="957" alt="url-report" src="https://github.com/user-attachments/assets/c6aff552-ed49-4601-8462-0ba2c994778e" />
<img width="894" height="361" alt="sender" src="https://github.com/user-attachments/assets/94226e3d-5dca-45e6-9e5b-223f80e5394b" />
<img width="1300" height="732" alt="recipient-slide" src="https://github.com/user-attachments/assets/f0aba83e-d085-4366-94a8-0b49a46f1e82" />
<img width="1287" height="1013" alt="network-country" src="https://github.com/user-attachments/assets/3bf79fcb-1d79-464b-a2dc-d4558738f35d" />
<img width="1917" height="1056" alt="message" src="https://github.com/user-attachments/assets/db94fb77-ae0d-4ad2-a461-a010fe06f63e" />
<img width="1300" height="732" alt="link-destination-slide" src="https://github.com/user-attachments/assets/ad15f296-7f4e-4919-8beb-6e7813af8719" />































































































<img width="1067" height="1474" alt="9e945a4c-8592-4a59-b5ab-23cb76efcd17" src="https://github.com/user-attachments/assets/3ef3f8e8-4d17-414d-abd8-9a8036cc375e" />

































