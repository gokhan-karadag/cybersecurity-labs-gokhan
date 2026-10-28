<img width="1067" height="1474" alt="975b5439-2e94-4a02-b07b-53463ea6f05e" src="https://github.com/user-attachments/assets/842f44d8-ac8b-4069-8ba0-f0ecb3516db9" />
[README.md](https://github.com/user-attachments/files/34089606/README.md)

# Phishing Analysis and Investigation

![SOC analyst examining a suspicious email](assets/phishing-investigation.png)

A practical SOC study guide combining BTL1 fundamentals with an evidence-based investigation workflow.

## Purpose

Investigate reported phishing emails, assess user impact, and document an evidence-based verdict and response.

Phishing uses impersonation or deceptive requests to steal information, compromise accounts or devices, or commit fraud. Messages may request credentials, payments, file execution, or MFA approval without containing a malicious link.

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

![Conceptual illustration of trust, authority, and urgency in social engineering](assets/social-engineering.png)

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

![Conceptual illustration of evidence preservation and isolated attachment analysis](assets/evidence-analysis.png)

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

## 9. Greenholt PLC exercise

A Sales Executive receives an unexpected email appearing to come from a customer. It uses an unusual generic greeting, mentions an unexpected money transfer, and includes an unsolicited attachment. The employee reports it to the SOC.

**Task:** Investigate `challenge.eml` using the workflow above. Review a working copy in a controlled environment with remote content blocked. Use static analysis first and dynamic analysis only when justified.

**Deliverables:** A concise case report with message and attachment hashes, header and authentication findings, artifact analysis, user-impact assessment, timeline, verdict, confidence, and recommended response.

**Evidence status:** The email and supporting artifacts were not supplied with this guide; the scenario has no confirmed verdict.

## 10. Tool reference

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

## Key Reminders

- Clean reputation, successful authentication, or no observed sandbox activity does not prove safety.
- Branding, HTTPS, and familiar display names do not establish legitimacy.
- Missing log events do not establish that no interaction occurred; document coverage and retention limits.
- Consider business impact before blocking shared infrastructure or parent domains.
- Keep confirmed facts, assumptions, and recommended actions clearly separated.

## Technical references

- **R1 NIST NCCoE — Static analysis definition:** https://pages.nist.gov/nccoe-devsecops/appendix-c.html
- **R2 MITRE D3FEND — Dynamic Analysis:** https://d3fend.mitre.org/technique/d3f%3ADynamicAnalysis/
- **R3 Microsoft — How email authentication works:** https://learn.microsoft.com/en-gb/defender-office-365/email-authentication-about
- **R4 Microsoft — DMARC configuration and alignment:** https://learn.microsoft.com/en-au/microsoft-365/security/office-365-security/email-authentication-dmarc-configure?view=o365-worldwide
- **R5 VirusTotal — How it works and report sharing:** https://docs.virustotal.com/docs/how-it-works
- **R6 URLscan — Scan visibility levels:** https://docs.urlscan.io/pages/visibility
- **R7 ICANN — Launching RDAP and sunsetting WHOIS:** https://www.icann.org/en/announcements/details/icann-update-launching-rdap-sunsetting-whois-27-01-2025-en
- **R8 URLhaus — API and submission policy:** https://urlhaus.abuse.ch/api
- **R9 Thunderbird — Remote content in messages:** https://support.mozilla.org/en-US/kb/remote-content-in-messages

---

*Combines a condensed summary of Security Blue Team BTL1 Phishing Analysis material, the Phishing Investigation SOC Student Guide, and the Phishing Email Analysis presentation.*

*Illustrations were created specifically for this guide using AI image generation; they are conceptual artwork, not third-party screenshots or technical evidence.*

































































































<img width="1067" height="1474" alt="9e945a4c-8592-4a59-b5ab-23cb76efcd17" src="https://github.com/user-attachments/assets/3ef3f8e8-4d17-414d-abd8-9a8036cc375e" />

































