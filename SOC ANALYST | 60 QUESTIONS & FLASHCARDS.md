# SOC Analyst Interview Preparation – 60 Questions & Flashcards


## 1. What is the difference between a threat, a vulnerability, and a risk?

Risk depends on both the probability of exploitation and its potential business impact.

| Term | Definition |
|---|---|
| Threat | Potential cause of harm |
| Vulnerability | Exploitable weakness |
| Risk | Likelihood × impact of harm |

**Flashcard · Question:** Threat vs weakness vs impact?
**Flashcard · Answer:** Threat → Vulnerability → Risk; risk = likelihood × impact.
**SOC Reference:** CVE = vulnerability identifier; CVSS = severity scoring.

## 2. What is the difference between TCP and UDP?

TCP emphasizes reliability; UDP minimizes protocol overhead. Actual performance depends on the application.

| TCP | UDP |
|---|---|
| Connection-oriented | Connectionless |
| Reliable, ordered delivery | No built-in delivery guarantee |
| ACKs and retransmission | Low transport overhead |
| Web, email, SSH | DNS, voice, gaming |

**Flashcard · Question:** Which protocol guarantees ordered delivery?
**Flashcard · Answer:** TCP; UDP has no built-in delivery guarantee.
**SOC Reference:** tcp.port / udp.port (Wireshark fields)

## 3. Explain the TCP three-way handshake.

A TCP connection is normally established in three steps: the client sends SYN, the server replies with SYN-ACK, and the client acknowledges with ACK. This exchange synchronizes initial sequence numbers and establishes connection state before application data is exchanged. SOC analysts examine handshake patterns to identify connection failures, SYN scans, and possible SYN-flood activity.

**Flashcard · Question:** Handshake in three words?
**Flashcard · Answer:** SYN → SYN-ACK → ACK.
**SOC Reference:** `Wireshark: tcp.flags.syn == 1`

## 4. What is ICMP?

Internet Control Message Protocol (ICMP) carries network diagnostic and error messages. ICMP Echo Request and Echo Reply are commonly used by ping; Time Exceeded and Destination Unreachable messages support troubleshooting and traceroute. In IPv4, Echo Request is Type 8, Echo Reply is Type 0, and Destination Unreachable is Type 3. ICMP should be controlled according to security requirements rather than universally blocked.

**Flashcard · Question:** Which protocol does ping use?
**Flashcard · Answer:** ICMP; Echo Request → Echo Reply.
**SOC Reference:** IPv4 ICMP types: 8=request, 0=reply, 3=unreachable, 11=time exceeded.

## 5. What are SSL and TLS?

Secure Sockets Layer (SSL) is an obsolete family of security protocols; Transport Layer Security (TLS) is its modern replacement. TLS protects communications in transit by providing confidentiality, integrity, and server authentication through certificates. Modern TLS handshakes use public-key cryptography for authentication and key establishment, then symmetric cryptography to protect application data.

**Flashcard · Question:** What replaced SSL?
**Flashcard · Answer:** TLS; encryption, integrity, authentication.
**SOC Reference:** `TLS 1.2 / 1.3; tcp.port == 443`

## 6. What is the difference between HTTP, HTTPS, SSL, and TLS?

HTTP is an application-layer protocol used to exchange web requests and responses. HTTPS is HTTP protected by TLS, providing encrypted transport and authentication of the server when certificate validation succeeds. SSL is deprecated, while TLS is the modern security protocol. Conventional ports are TCP 80 for HTTP and TCP 443 for HTTPS; HTTP/3 commonly uses UDP 443.

**Flashcard · Question:** HTTP vs HTTPS?
**Flashcard · Answer:** HTTPS = HTTP protected by TLS.
**SOC Reference:** `80/TCP HTTP; 443/TCP HTTPS; 443/UDP HTTP/3.`

## 7. Explain the OSI model.

The OSI model describes seven conceptual networking layers: 1 Physical, 2 Data Link, 3 Network, 4 Transport, 5 Session, 6 Presentation, and 7 Application. Each layer represents a different communication function, from transmitting signals to providing application services. In SOC investigations, the model helps classify issues such as MAC-based activity at Layer 2, IP routing at Layer 3, TCP sessions at Layer 4, and web requests at Layer 7.

**Flashcard · Question:** Seven OSI layers, bottom up?
**Flashcard · Answer:** Physical → Data Link → Network → Transport → Session → Presentation → Application.
**SOC Reference:** Layers 2=MAC, 3=IP, 4=TCP/UDP, 7=HTTP/DNS.

## 8. Explain the TCP/IP model.

The TCP/IP model commonly has four layers: Network Interface, Internet, Transport, and Application. Network Interface covers local link transmission; Internet provides IP addressing and routing; Transport includes TCP and UDP; and Application includes protocols such as HTTP, DNS, and SMTP. It is a practical model for understanding and investigating network traffic.

**Flashcard · Question:** Four TCP/IP layers?
**Flashcard · Answer:** Network Access → Internet → Transport → Application.
**SOC Reference:** `IP/ICMP = Internet; TCP/UDP = Transport.`

## 9. What is the CIA triad?

The CIA triad defines the three fundamental goals of information security.

| Principle | Goal | Example control |
|---|---|---|
| Confidentiality | Prevent unauthorized disclosure | Encryption, access control |
| Integrity | Prevent unauthorized changes | Hashes, audit logs |
| Availability | Maintain access | Backups, redundancy |

**Flashcard · Question:** CIA stands for?
**Flashcard · Answer:** Confidentiality, Integrity, Availability.
**SOC Reference:** Encryption / hashing / backups.

## 10. What is IP addressing, and how do IPv4 and IPv6 differ?

An IP address identifies a network interface for logical addressing and packet routing. IPv4 uses 32-bit addresses, such as 192.168.1.10; IPv6 uses 128-bit addresses and offers a substantially larger address space. Private IPv4 ranges include 10.0.0.0/8, 172.16.0.0/12, and 192.168.0.0/16. The address 127.0.0.1 is IPv4 loopback. Modern networks use CIDR rather than relying on historical classful addressing.

**Flashcard · Question:** IPv4 vs IPv6 address length?
**Flashcard · Answer:** 32 bits vs 128 bits.
**SOC Reference:** `RFC1918: 10/8, 172.16/12, 192.168/16; loopback 127.0.0.1.`

## 11. What are common network port numbers?

Common defaults include FTP 20/21, SSH 22, Telnet 23, SMTP 25, DNS 53 (UDP and TCP), HTTP 80, POP3 110, NetBIOS 137–139, IMAP 143, HTTPS 443, SMB 445, and RDP 3389. A port number helps identify a likely service, but analysts must confirm the actual protocol because applications can run on nonstandard ports.

**Flashcard · Question:** Ports to recall first?
**Flashcard · Answer:** 22 SSH, 53 DNS, 80 HTTP, 443 HTTPS, 445 SMB, 3389 RDP.
**SOC Reference:** 88 Kerberos; 389 LDAP; 636 LDAPS; 25 SMTP.

## 12. What is the difference between symmetric and asymmetric encryption?

TLS combines public-key techniques for authentication and key establishment with symmetric encryption for data.

| Symmetric | Asymmetric |
|---|---|
| One shared secret key | Public/private key pair |
| Efficient bulk encryption | Signatures and key establishment |
| Example: AES | Example: RSA |

**Flashcard · Question:** Which encryption uses one shared key?
**Flashcard · Answer:** Symmetric (AES); asymmetric uses a public/private pair.
**SOC Reference:** AES-128/192/256; RSA.

## 13. What is a network switch?

A switch connects devices within a local network and forwards Ethernet frames based on learned MAC addresses. Most access switches operate primarily at OSI Layer 2, while multilayer switches can also route Layer 3 traffic. Switching reduces unnecessary forwarding compared with a hub, but does not by itself guarantee security.

**Flashcard · Question:** Switch forwards using what address?
**Flashcard · Answer:** Destination MAC address; learns from source MAC.
**SOC Reference:** `OSI Layer 2; VLAN; MAC address table.`

## 14. What is a DMZ?

Not included in this source edition.

**Flashcard · Question:** What is missing from the source?
**Flashcard · Answer:** Question 14 has no source answer.
**SOC Reference:** No flashcard fact or command added without source.

## 15. What anomalies may indicate a compromised system?

Indicators include repeated authentication failures followed by success, logins at unusual times or locations, suspicious parent-child process relationships, unexpected scheduled tasks or services, abnormal DNS queries, unusual outbound connections, and unexpected file or registry changes. Analysts compare these observations with a known baseline and correlate endpoint, identity, and network evidence before concluding that compromise occurred.

**Flashcard · Question:** Three compromise signals?
**Flashcard · Answer:** Suspicious logons, unusual processes, abnormal outbound traffic.
**SOC Reference:** Windows: 4624 successful logon, 4625 failed logon, 4688 process creation.

## 16. How would you strengthen user authentication?

I would enforce phishing-resistant MFA where feasible, apply strong password and passphrase policies, disable legacy authentication, use least privilege, and monitor anomalous sign-ins. Conditional access, device compliance, account lockout or throttling, and regular review of privileged access further reduce account-compromise risk.

**Flashcard · Question:** Most important authentication control?
**Flashcard · Answer:** Phishing-resistant MFA, plus monitoring and least privilege.
**SOC Reference:** `MITRE ATT&CK: T1078 Valid Accounts; T1110 Brute Force.`

## 17. How would you respond to a compromised endpoint?

I would validate the alert, identify the affected host and user, preserve relevant evidence, and assess the scope and severity. Following the incident-response plan, I would coordinate containment, such as isolating the endpoint, then support eradication of the threat and root cause. After recovery, I would verify normal operation, increase monitoring, document actions and timestamps, and contribute to lessons learned.

**Flashcard · Question:** Endpoint response sequence?
**Flashcard · Answer:** Validate → Contain → Investigate → Eradicate → Recover.
**SOC Reference:** `Windows: 4688 process, 7045 service install; Sysmon 1 process, 3 network.`

## 18. How would you manage security patches?

I would maintain an accurate asset inventory, identify missing updates through authenticated scanning and vendor advisories, and prioritize remediation based on exploitability, exposure, asset criticality, and business impact. Patches should be tested, deployed through change management with rollback plans, verified after installation, and documented. Exceptions require compensating controls and risk acceptance.

**Flashcard · Question:** Patch management sequence?
**Flashcard · Answer:** Inventory → Prioritize → Test → Deploy → Verify.
**SOC Reference:** `CVE, CVSS; track asset criticality and exposure.`

## 19. What is a watering hole attack?

A watering hole attack targets a group by compromising a website or online resource that its members frequently visit. The attacker uses the trusted destination to deliver malicious content or redirect visitors. SOC teams investigate affected browsing sessions, web telemetry, endpoint detections, and indicators of compromise.

**Flashcard · Question:** What is a watering hole attack?
**Flashcard · Answer:** Compromise a website frequented by the intended victims.
**SOC Reference:** MITRE ATT&CK: T1189 Drive-by Compromise (related technique).

## 20. What are SIM and SIEM?

Security Information Management (SIM) emphasizes collection, retention, and reporting of security logs. Security Information and Event Management (SIEM) combines log management with event correlation, search, detection, and alerting. Platforms such as Splunk Enterprise Security and IBM QRadar help SOC teams investigate activity, but alerts require analyst validation and contextual analysis.

**Flashcard · Question:** SIEM's key purpose?
**Flashcard · Answer:** Centralize, search, correlate, and alert on security events.
**SOC Reference:** `Splunk: index=* | stats count by sourcetype`

## 21. How can an organization reduce the risk of zero-day attacks?

Zero-day exploitation cannot always be prevented before a fix exists. Risk reduction relies on defense in depth: endpoint behavioral detection, attack-surface reduction, least privilege, segmentation, application controls, monitoring, and rapid incident response. When mitigations or vendor patches become available, they should be assessed and deployed according to exposure and business risk.

**Flashcard · Question:** How to reduce zero-day exposure?
**Flashcard · Answer:** Defense in depth, least privilege, monitoring, segmentation.
**SOC Reference:** `EDR alerts; exploit protection; network segmentation.`

## 22. What is the difference between a firewall and a proxy?

A firewall enforces network traffic policy using attributes such as source, destination, port, protocol, connection state, and, for next-generation products, application content. A proxy intermediates requests between clients and services and may provide filtering, logging, caching, or inspection. Both can contribute to security, but they perform different roles.

**Flashcard · Question:** Firewall vs proxy?
**Flashcard · Answer:** Firewall filters traffic; proxy intermediates requests.
**SOC Reference:** `Ports 80/443; proxy access logs; firewall allow/deny logs.`

## 23. What is a salted password hash?

A salt is a unique, randomly generated value combined with a password before hashing. Different salts produce different stored password hashes even when users choose identical passwords, making precomputed rainbow tables ineffective at scale. Passwords should be processed with purpose-built, slow password-hashing algorithms such as Argon2id, scrypt, or bcrypt rather than a fast general-purpose hash alone.

**Flashcard · Question:** Why salt a password hash?
**Flashcard · Answer:** Unique salt makes precomputed hash attacks less effective.
**SOC Reference:** Argon2id / bcrypt / scrypt (password hashing).

## 24. What happens when you enter a website address in a browser?

The browser resolves the hostname using cache and DNS as needed, establishes a network connection, and negotiates TLS for HTTPS. It then sends an HTTP request, receives the response, and loads supporting resources such as CSS, JavaScript, and images. Depending on the negotiated protocol, HTTPS may use TCP with TLS or QUIC over UDP.

**Flashcard · Question:** Browser navigation order?
**Flashcard · Answer:** DNS → connection → TLS (HTTPS) → HTTP request → response.
**SOC Reference:** `nslookup example.com; curl -I https://example.com`

## 25. When does DNS use TCP instead of UDP?

Traditional DNS queries commonly use UDP port 53, while TCP port 53 is used for zone transfers and may be used when responses are truncated or transport requirements demand it. EDNS supports larger UDP messages, so large responses do not always require TCP. DNS over TLS and DNS over HTTPS use other transports and ports.

**Flashcard · Question:** When does DNS use TCP?
**Flashcard · Answer:** When needed for larger responses or zone transfers.
**SOC Reference:** `UDP/53 and TCP/53; dig +tcp example.com`

## 26. How do TCP and UDP differ in reliability and performance?

TCP provides reliable, ordered byte-stream delivery with flow and congestion control. UDP sends independent datagrams without built-in delivery, ordering, or retransmission guarantees. UDP has lower transport overhead, but application protocols can implement their own reliability; therefore, UDP is not automatically faster in every real-world scenario.

**Flashcard · Question:** Reliability difference?
**Flashcard · Answer:** TCP acknowledges/retransmits; UDP does not.
**SOC Reference:** Wireshark: tcp.analysis.retransmission

## 27. What is traceroute?

Traceroute is a diagnostic technique used to discover the network path toward a destination. It sends probes with increasing IP Time to Live (TTL) or IPv6 Hop Limit values and observes responses from intermediate routers. It can help identify routing changes or potential points of delay, although firewalls and rate limiting may hide hops.

**Flashcard · Question:** How does traceroute find hops?
**Flashcard · Answer:** TTL/hop limit expiration elicits router responses.
**SOC Reference:** `Windows: tracert example.com; Linux: traceroute example.com`

## 28. What is Data Loss Prevention (DLP)?

Data Loss Prevention (DLP) uses policies and detection methods to identify and control unauthorized or accidental exposure of sensitive information. It can monitor data at rest, in motion, and in use, and may alert, quarantine, or block transfers. During a DLP investigation, I would verify the user, data classification, destination, policy match, business justification, and action taken.

**Flashcard · Question:** DLP's purpose?
**Flashcard · Answer:** Detect or prevent unauthorized movement of sensitive data.
**SOC Reference:** DLP alert: user, file, destination, policy, action.

## 29. What is a Security Operations Center (SOC)?

A Security Operations Center is the people, processes, and technologies responsible for monitoring an organization's environment, detecting suspicious activity, investigating alerts, and coordinating response to security incidents. The SOC uses telemetry from endpoints, identity services, networks, applications, and cloud platforms to reduce organizational security risk.

**Flashcard · Question:** What does SOC stand for?
**Flashcard · Answer:** Security Operations Center.
**SOC Reference:** Monitor → Triage → Investigate → Respond.

## 30. What are the main responsibilities of a SOC team?

A SOC team monitors security events, triages alerts, investigates potential threats, documents evidence, escalates confirmed or high-risk incidents, and supports containment and recovery. The broader team also improves detection content, maintains playbooks, performs threat hunting, reports operational metrics, and collaborates with IT and risk stakeholders.

**Flashcard · Question:** Core SOC duties?
**Flashcard · Answer:** Alert triage, investigation, escalation, response, documentation.
**SOC Reference:** `SIEM, EDR, ticketing; record timeline and IOCs.`

## 31. What is attacker dwell time?

Dwell time is the period between an attacker's initial compromise of an environment and detection of that intrusion. Shorter dwell time generally limits opportunities for persistence, lateral movement, and data theft. Analysts reduce dwell time through effective telemetry, detection engineering, timely triage, and threat hunting.

**Flashcard · Question:** What is dwell time?
**Flashcard · Answer:** Time attacker remains undetected in the environment.
**SOC Reference:** Dwell time = detection time − initial compromise time.

## 32. What is incident response?

Incident response is the structured process of preparing for, detecting, analyzing, containing, eradicating, and recovering from cybersecurity incidents. Teams preserve evidence, coordinate communications, restore services safely, and document lessons learned. The commonly taught SANS lifecycle includes Preparation, Identification, Containment, Eradication, Recovery, and Lessons Learned; NIST guidance organizes these activities within its own incident-response framework.

**Flashcard · Question:** Incident response lifecycle?
**Flashcard · Answer:** Prepare → Detect/Analyze → Contain → Eradicate → Recover → Lessons.
**SOC Reference:** `NIST incident response; preserve evidence and timeline.`

## 33. What is the Cyber Kill Chain?

The Cyber Kill Chain is a model describing seven stages of an intrusion: Reconnaissance, Weaponization, Delivery, Exploitation, Installation, Command and Control, and Actions on Objectives. Analysts use it to identify opportunities for detection and disruption at multiple stages. Not every intrusion follows the model exactly, so it should be used alongside other analytical frameworks.

**Flashcard · Question:** Seven Cyber Kill Chain stages?
**Flashcard · Answer:** Recon → Weaponization → Delivery → Exploitation → Installation → C2 → Actions.
**SOC Reference:** `C2 = command and control; MITRE ATT&CK provides behavior-level mapping.`

## 34. What is a PCAP file, and how would you analyze it?

A PCAP file contains captured network packets. I would first establish the capture time range and identify key hosts, then review protocols, conversations, DNS activity, TCP behavior, and unusual destinations or traffic patterns using Wireshark. I would correlate suspicious findings with endpoint and SIEM logs, document packet-level evidence, and escalate when appropriate. Repeated encrypted connections alone do not prove command-and-control activity.

**Flashcard · Question:** What is a PCAP?
**Flashcard · Answer:** Packet capture for inspecting network communications.
**SOC Reference:** `tcpdump -r capture.pcap; Wireshark: dns or http or tcp.flags.syn == 1`

## 35. What is penetration testing?

Penetration testing is an authorized assessment in which testers attempt to exploit security weaknesses within an agreed scope to evaluate real-world impact. Typical activities include planning, reconnaissance, enumeration, vulnerability validation, controlled exploitation, impact assessment, and reporting. Rules of engagement define permissions, limitations, and safety requirements.

**Flashcard · Question:** What is penetration testing?
**Flashcard · Answer:** Authorized testing that attempts to exploit weaknesses.
**SOC Reference:** `Scope and rules of engagement; Nmap, Burp Suite.`

## 36. What is the OWASP Top 10, and why is it important?

The OWASP Top 10 is an awareness and prioritization resource covering major categories of web application security risk. It helps security teams and developers focus on weaknesses such as broken access control, injection, cryptographic failures, and security misconfiguration. It is not a complete testing methodology or an exhaustive list of every vulnerability.

**Flashcard · Question:** What does OWASP Top 10 cover?
**Flashcard · Answer:** Major web application security risk categories.
**SOC Reference:** Examples: injection, broken access control, XSS.

## 37. How should sensitive data be protected?

Sensitive data should be protected through layered controls, including data classification, least-privilege access, strong authentication, encryption at rest and in transit, secure key management, monitoring, backups, and retention policies. No single technology is universally the most secure solution; protections should match the data's sensitivity and business requirements.

**Flashcard · Question:** Protect sensitive data how?
**Flashcard · Answer:** Classify, restrict access, encrypt, monitor, and back up.
**SOC Reference:** `DLP; least privilege; TLS; encryption at rest.`

## 38. How does data encryption work?

Encryption transforms readable plaintext into ciphertext using a cryptographic algorithm and key. Authorized recipients use the appropriate key to decrypt the ciphertext. Symmetric encryption uses a shared secret key, while asymmetric encryption involves public and private keys. Secure implementation also requires proper key generation, storage, rotation, and authenticated encryption where appropriate.

**Flashcard · Question:** Encryption transforms what?
**Flashcard · Answer:** Plaintext + key → ciphertext; decrypt with authorized key.
**SOC Reference:** `AES; TLS protects data in transit.`

## 39. What is the difference between encoding, encryption, and hashing?

Encoding changes data representation for compatibility or transport and is normally reversible without a secret, as with Base64. Encryption protects confidentiality using cryptographic keys and can be reversed by authorized parties. Cryptographic hashing produces a fixed-length digest designed to resist reversal and collisions; it is used in integrity checking and, with suitable password-hashing schemes, credential protection.

**Flashcard · Question:** Encoding vs encryption vs hashing?
**Flashcard · Answer:** Format conversion vs reversible secrecy vs one-way digest.
**SOC Reference:** `Base64 ≠ encryption; SHA-256 is a hash.`

## 40. What is a DDoS attack?

A Distributed Denial-of-Service (DDoS) attack uses multiple sources to overwhelm a service, network, or application and degrade availability. Attacks may target bandwidth, network protocols, or application resources. Defenses can include traffic filtering, rate limiting, CDN or scrubbing services, autoscaling where appropriate, and coordinated response with network providers.

**Flashcard · Question:** What is DDoS?
**Flashcard · Answer:** Distributed traffic overload affecting availability.
**SOC Reference:** `Traffic rate, source distribution, HTTP 429/503; CDN/WAF.`

## 41. How does a router work?

A router forwards packets between IP networks by examining destination addresses and consulting routing information. It selects a next hop based on routes and policy. Many home routers also provide NAT, DHCP, and firewall capabilities, although those functions are separate from basic routing.

**Flashcard · Question:** Router forwards using what?
**Flashcard · Answer:** Destination IP address and routing table.
**SOC Reference:** `Linux: ip route; Windows: route print.`

## 42. How do you write a SQL query to retrieve data?

SQL is used to query and manage relational databases. A `SELECT` statement chooses columns, `FROM` specifies a table, and `WHERE` filters rows. For example, `SELECT name, email FROM employees WHERE department = 'IT';` retrieves the specified fields for employees in the IT department. In security investigations, read-only queries can support authorized data analysis.

**Flashcard · Question:** Basic SQL retrieval syntax?
**Flashcard · Answer:** SELECT columns FROM table WHERE condition;
**SOC Reference:** `SELECT username FROM users WHERE active = 1;`

## 43. What is DNS?

The Domain Name System (DNS) is a distributed naming system that maps domain names to records such as IPv4 A and IPv6 AAAA addresses. It also supports mail routing, aliases, and other service records. DNS logs can help SOC analysts identify suspicious domains, unusual query volumes, and potential tunneling behavior.

**Flashcard · Question:** DNS resolves what?
**Flashcard · Answer:** Domain names to IP addresses and other records.
**SOC Reference:** `A, AAAA, MX, TXT, CNAME; nslookup example.com`

## 44. Which transport protocols does DNS use?

Conventional DNS uses both UDP and TCP on port 53. UDP is common for ordinary queries; TCP is used for zone transfers and certain responses or operational requirements. Encrypted DNS variants include DNS over TLS and DNS over HTTPS, so analysts must consider the specific deployment when reviewing traffic.

**Flashcard · Question:** DNS transport?
**Flashcard · Answer:** Usually UDP/53; TCP/53 when required.
**SOC Reference:** `dig +tcp example.com; Wireshark: dns`

## 45. How would you defend against cross-site scripting (XSS)?

The strongest defenses are context-aware output encoding, safe templating, secure DOM APIs, and sanitization when untrusted HTML must be allowed. Content Security Policy provides additional defense in depth. Input validation and web application firewalls can help, but they do not replace correct output handling. Session cookies should use appropriate security attributes.

**Flashcard · Question:** Best XSS defense?
**Flashcard · Answer:** Context-aware output encoding; safe templating and CSP.
**SOC Reference:** `CSP = Content-Security-Policy; HttpOnly cookies reduce exposure.`

## 46. What is the difference between stored and reflected XSS?

Stored XSS occurs when malicious content is persisted by an application and later rendered in a victim's browser. Reflected XSS occurs when attacker-controlled input from a request is immediately included in a response without safe handling. Both can execute script in the application's security context and should be investigated through request data, application behavior, and browser evidence.

**Flashcard · Question:** Stored vs reflected XSS?
**Flashcard · Answer:** Stored persists on server; reflected appears in a response.
**SOC Reference:** Inspect URL parameters, rendered HTML, browser console.

## 47. What is a subnet?

A subnet is a logical subdivision of an IP network defined by an address prefix. Subnetting supports address allocation, routing, segmentation, and operational management. Communication between separate IP subnets generally requires a router or Layer 3 switch; security isolation additionally depends on access-control policies.

**Flashcard · Question:** Subnet definition?
**Flashcard · Answer:** Logical IP network identified by CIDR prefix.
**SOC Reference:** `192.168.1.0/24; 255.255.255.0.`

## 48. How many IPv4 addresses are in /24 and /32 networks?

An IPv4 /24 prefix leaves eight host bits, giving 256 total addresses and typically 254 usable host addresses in a conventional subnet. An IPv4 /32 prefix identifies exactly one address and is commonly used for host routes or precise filtering. Address usability depends on the network design and use case.

**Flashcard · Question:** /24 vs /32?
**Flashcard · Answer:** /24 has 256 addresses (typically 254 usable); /32 is one host address.
**SOC Reference:** IPv4 address count = 2^(32 − prefix).

## 49. What information is contained in a TCP header?

A TCP header contains source and destination ports, sequence and acknowledgment numbers, flags, window information, checksum, and optional fields. Its minimum length is 20 bytes, and it can extend to 60 bytes when options are present. Analysts inspect these fields to troubleshoot connections and identify abnormal traffic patterns.

**Flashcard · Question:** TCP header key fields?
**Flashcard · Answer:** Ports, sequence/ACK numbers, flags, window, checksum.
**SOC Reference:** Wireshark: tcp.seq, tcp.ack, tcp.window_size.

## 50. What are TCP flags?

TCP flags indicate control functions and connection state. SYN initiates synchronization, ACK acknowledges data, FIN requests orderly closure, RST resets a connection, PSH requests prompt delivery to the receiving application, and URG marks urgent-pointer use. Packet patterns involving these flags can help identify scanning, resets, and connection anomalies.

**Flashcard · Question:** TCP flags to memorize?
**Flashcard · Answer:** SYN, ACK, FIN, RST, PSH, URG.
**SOC Reference:** `tcp.flags.reset == 1; tcp.flags.syn == 1`

## 51. What are HTTP headers?

HTTP headers are metadata fields included in requests and responses. Examples include `Host`, `User-Agent`, `Content-Type`, `Authorization`, `Cookie`, and `Set-Cookie`. SOC analysts review them to investigate suspicious requests, unusual clients, session behavior, and potential data exposure, while handling credentials and tokens as sensitive information.

**Flashcard · Question:** HTTP header examples?
**Flashcard · Answer:** Host, User-Agent, Authorization, Cookie, Content-Type.
**SOC Reference:** `curl -I https://example.com; HTTP 200/301/403/404/500.`

## 52. What is SQL injection?

SQL injection occurs when untrusted input is incorporated into a database query in a way that changes its intended structure or logic. It may allow unauthorized reading, modification, or deletion of data. Primary defenses include parameterized queries, least-privilege database accounts, and secure application design; input validation is a supplementary control.

**Flashcard · Question:** What is SQL injection?
**Flashcard · Answer:** Untrusted input changes a SQL query's intended behavior.
**SOC Reference:** `Defense: parameterized queries; monitor WAF/application logs.`

## 53. How does vulnerability scanning differ from penetration testing?

Vulnerability scanning identifies potential weaknesses, such as missing patches, exposed services, and known vulnerable software. Penetration testing validates selected weaknesses through authorized exploitation and assesses their practical impact. Scanning provides broad coverage, while penetration testing adds contextual evidence and attack-path analysis.

**Flashcard · Question:** Scanning vs pentesting?
**Flashcard · Answer:** Scan finds possible flaws; pentest validates exploitability within scope.
**SOC Reference:** `CVE/CVSS; vulnerability scanner vs authorized exploitation.`

## 54. What is the difference between authenticated and unauthenticated vulnerability scanning?

Unauthenticated scanning evaluates externally observable services and exposures without logging into the target. Authenticated scanning uses authorized credentials to inspect internal configurations, installed software, missing patches, and local weaknesses in greater depth. Both approaches are useful and should be scoped carefully.

**Flashcard · Question:** Authenticated vs unauthenticated scan?
**Flashcard · Answer:** With credentials = deeper internal visibility; without = external view.
**SOC Reference:** Scan account permissions and asset coverage.

## 55. What is port scanning, and which tools can perform it?

Port scanning probes network endpoints to determine which ports appear open, closed, or filtered and to infer exposed services. Common tools include Nmap, its graphical interface Zenmap, and Netcat for targeted connectivity checks. SOC analysts may detect scanning through repeated probes across many ports or hosts, especially when correlated with firewall and network telemetry.

**Flashcard · Question:** Which tool scans ports?
**Flashcard · Answer:** Nmap; results indicate open, closed, or filtered.
**SOC Reference:** `nmap -sT -p 22,80,443 192.0.2.10 (authorized targets only).`

## 56. What is vulnerability scanning, and which tools are commonly used?

Vulnerability scanning systematically checks assets for known weaknesses, insecure configurations, and missing updates. Common platforms include Tenable Nessus, Qualys, and Greenbone/OpenVAS; tools such as Nmap, Burp Suite, and OWASP ZAP support related discovery and application assessment tasks. Findings must be validated, prioritized, assigned for remediation, and rescanned.

**Flashcard · Question:** Name vulnerability scanners?
**Flashcard · Answer:** Nessus, Qualys, OpenVAS/Greenbone.
**SOC Reference:** Track CVE, severity, affected host, remediation.

## 57. What is phishing, and what are its main types?

Phishing is social engineering designed to persuade victims to disclose information, authenticate to fraudulent sites, open malicious content, or perform unauthorized actions. Common forms include broad email phishing, spear phishing, whaling, smishing (SMS), and vishing (voice). Analysts review sender infrastructure, URLs, attachments, authentication results, user activity, and endpoint evidence.

**Flashcard · Question:** Phishing categories?
**Flashcard · Answer:** Bulk phishing, spearphishing, whaling, smishing, vishing.
**SOC Reference:** `MITRE ATT&CK: T1566 Phishing; SPF/DKIM/DMARC.`

## 58. Where are logs stored on Windows and Linux?

On Windows, Event Viewer exposes logs such as Security, System, and Application; EVTX files are commonly stored under `C:\Windows\System32\winevt\Logs`. On Linux, many logs reside under `/var/log`, including `auth.log`, `secure`, and `syslog`, depending on distribution and configuration. Systems using systemd may store events in the journal, accessible through `journalctl`.

**Flashcard · Question:** Key Windows and Linux logs?
**Flashcard · Answer:** Windows Event Logs; Linux auth and system logs.
**SOC Reference:** `4624, 4625, 4688, 4720, 1102; /var/log/auth.log or /var/log/secure.`

## 59. What is a rainbow table attack?

A rainbow table attack attempts to recover passwords by looking up stolen password hashes in precomputed tables. Unique random salts make these tables impractical to reuse across many accounts. Modern defenses combine unique salts with computationally expensive password-hashing algorithms and strong authentication controls.

**Flashcard · Question:** Why do salts defeat rainbow tables?
**Flashcard · Answer:** Unique salts prevent reuse of precomputed password hashes.
**SOC Reference:** Use unique salts + Argon2id/bcrypt/scrypt.

## 60. Walk me through your typical day as a SOC analyst.

I begin by reviewing shift handover notes, open incidents, and priority alerts. I monitor SIEM and endpoint security dashboards, investigate suspicious authentication, network, and process activity, and correlate evidence across relevant data sources. I also triage reported phishing messages, enrich indicators with threat intelligence, document findings in the case-management system, and escalate confirmed or high-impact threats.

**Flashcard · Question:** Daily SOC workflow?
**Flashcard · Answer:** Review queue → Triage → Correlate → Escalate/Contain → Document.
**SOC Reference:** Capture alert ID, timestamp, user, host, IP, IOC, verdict.
