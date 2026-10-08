# SOC Analyst — Professional Study Notes

**60 Interview Questions and Study Answers**

## 1. What is the difference between a threat, a vulnerability, and a risk?

**A vulnerability is a weakness in a system, network, or application. A threat is anything that can take advantage of that weakness and cause harm. Risk is the possibility and impact of that threat successfully exploiting the vulnerability. For example, an unpatched server is a vulnerability, a hacker is a threat, and the possibility of data being stolen is the risk.**

**For example, an unpatched server is a vulnerability, a hacker is a threat, and the possibility of data being stolen is the risk.**

## 🔵 What is a vulnerability?

**Vulnerability is a weakness in a system, application, network, or configuration.**

### Examples:

- Unpatched software
- Outdated operating system
- Weak password
- Open port
- No MFA
- Misconfigured firewall
- Insecure protocol
- Poor input validation

## 🔴 What is a threat?

**A threat is anything or anyone that has the potential to cause harm.**

**A threat can be hintentional or unintentional. **

**Intentional threats —-**

**include Hacker**

- **Malware**
- **Ransomware**
- **Malicious inside**

**Unintentional threats– can be things like employee mistakes, power outages, fires, or floods.**

Simplified version:

**Threats can be intentional, like hackers or malware, or unintentional, like human mistakes, power outages, or natural disasters.**

## 🟡 What is risk?

**Risk is the possibility of loss or damage if a threat successfully exploits a vulnerability.**

**Risk can lead to things like —-**

**\* Data theft**
** \* Financial loss**
** \* System compromise**
** \* Service outage**
** \* Reputation damage**

**Simplified version:**

**Risk means something bad can happen, like stolen data, money loss, system problems, downtime, or damage to the company’s reputation.**

# What is an Exploit?

**An exploit is a tool, code, or technique used to take advantage of a vulnerability.**

**EternalBlue is an exploit that takes advantage of an SMB vulnerability in Windows.**

**Memory aid:**

**SMB vulnerability = weakness**
** EternalBlue = exploit**
** Attacker = threat**
** Possible system compromise = risk**

**In conversational English:**

**For example, EternalBlue is a tool used to exploit an SMB weakness in Windows. The attacker is the threat, and the risk is that the system could be compromised.**

# What is Vulnerability Assessment?

**1Vulnerability assessment is the process of identifying, evaluating, and prioritizing vulnerabilities in systems, networks, and applications.**

**2A vulnerability assessment is the process of finding, checking, prioritizing, and fixing weaknesses in a company’s systems.**

**Example:**
**For example, a scanner finds a critical vulnerability on a server. The security team reviews it and asks the infrastructure team to patch the server.**

**Memory aid:**

**Find → Check → Prioritize → Fix**

**or**

**Reported → Prioritized → Patched **

# Vulnerability Assessment vs Penetration Testing

**A vulnerability assessment focuses on identifying and prioritizing vulnerabilities, while penetration testing goes one step further and attempts to exploit those vulnerabilities to determine their real-world impact.**

### Vulnerability Assessment

**Finds vulnerabilities.**

**Vulnerability Assessment = FIND**

### Penetration Testing

**Finds vulnerabilities AND tries to exploit them.**

**Penetration Testing = FIND + EXPLOIT**

# Possible interview follow-up questions

**Can you give me an example of a vulnerability?**

**Yes. An outdated or unpatched system is a vulnerability because an attacker may use that weakness to gain access to the system.**

**What is an exploit?**

**An exploit is a tool or technique used to take advantage of a vulnerability.**

**Can a threat exist without a vulnerability?**

**Yes. A threat can exist, but without an exploitable vulnerability it may not be able to cause harm.**

**What is vulnerability assessment?**

**It is the process of identifying, evaluating, and prioritizing vulnerabilities so they can be remediated or patched.**

**What is the difference between vulnerability assessment and penetration testing?**

**Vulnerability assessment identifies vulnerabilities, while penetration testing tries to exploit them to see their real-world impact.**

---

## 🧠 Final memory card

**Vulnerability → What is weak?**
** Threat → What can attack me?**
** Risk → What could happen if it attacks me?**
** Exploit → How can the weakness be used?**
** Vulnerability Assessment → Find the weakness**
** Penetration Testing → Find it + try to exploit it**

## 2. What is the difference between TCP and UDP?

**TCP and UDP are transport layer protocols. TCP is connection-oriented and reliable. It establishes a connection before sending data and checks that the data is delivered correctly. UDP is connectionless and faster because it sends data without waiting for confirmation. TCP is commonly used for things like email and web traffic, while UDP is commonly used for voice, video, DNS, and online gaming.**

### Quick memory aid:

**TCP = Connection + Reliable + Slower**
** UDP = No connection + Faster + Less reliable**

**TCP → email, web**
** UDP → voice, video, gaming, DNS**

### ⭐ One more important point:

**TCP uses a three-way handshake:**

**SYN → SYN-ACK → ACK**

**In simple terms:**

**“Can we connect?” → “Yes.” → “Okay, let’s start.”**

## 3. Explain the three-way handshake. 

**TCP three-way handshake is a process used to establish a connection between a client and a server. First, the client sends a SYN packet to start connection .The server responds with SYN-ACK, and finally, the client sends ACK. After that, the connection is established and data transmission can begin.**

**Client asks → Server answers → Client confirms.**

## 4. What is ICMP? 

**So basically… / ICMP stands for Internet Control Message Protocol.**
** I think of it as… / the housekeeping protocol of the internet… / because it helps report network problems.**
** It works at Layer 3… / the Network Layer of the OSI model.**
** For example… / ping and traceroute use ICMP.**
** And I wouldn’t block all ICMP traffic… / because it is useful for troubleshooti**

**A common example is Type 8 for-Echo Request and Type 0 for – Echo Reply. Type 3 means– Destination Unreachable.**

## 5. What is SSL and TLS?

**TLS stands for -Transport Layer Security.**
**SSL stands for -Secure Sockets Layer.**

**SSL and TLS are security protocols that protect data while it travels over a network. TLS is the newer and more secure version of SSL. During the TLS handshake, the client checks the server’s certificate and they establish a secure session. After that, the actual data is protected using symmetric encryption because it is faster.**

### “What type of encryption is used during a TLS handshake?”

**TLS uses both asymmetric and symmetric encryption. Asymmetric cryptography is used for authentication and establishing a secure session. Symmetric encryption is then used to protect the actual data because it is faster.**

### Quick memory formula

**TLS protects data in transit. Asymmetric encryption starts the secure session, and symmetric encryption protects the actual data**

## 6. HTTP, HTTPS, SSL and TLS

**Basically, HTTP stands for Hypertext Transfer Protocol. It is used for communication between a browser and a web server, but it sends data in clear text, so it is not secure.**

**HTTPS is the secure version of HTTP. It uses TLS to encrypt data in transit and provides confidentiality, integrity, and authentication. This means others cannot read the data, the data cannot be changed in transit, and the browser can verify the server’s identity through its certificate.**

**SSL stands for Secure Sockets Layer, and it is the older security protocol. TLS stands for Transport Layer Security, and it is the newer and more secure replacement for SSL. HTTP normally uses port 80, while HTTPS uses port 443.**

### Memory card

- **HTTP → clear text → not secure → port 80**
- **HTTPS → encrypted → secure → port 443**
- **SSL → old security protocol**
- **TLS → newer and more secure replacement **

- **HTTP sends the data.**
** TLS protects the data.**
** HTTPS combines them.**
** SSL is the outdated predecessor of TLS.**

## How does HTTPS work? — Follow-up

**The browser checks the server’s certificate and creates a secure session. Asymmetric cryptography starts the secure session, and symmetric encryption protects the actual data because it is faster.**

## What are HTTP methods?

**HTTP methods tell the server what action we want to perform. For example, GET retrieves data, POST creates data, PUT updates data, and DELETE removes data.**

- **GET → retrieve**
- **POST → create**
- **PUT → update**
- **DELETE → remove**

## What are HTTP status codes?

**HTTP status codes show the result of a request. For example, 200 means success, 301 means redirection, 404 means not found, and 500 means a server error.**

- **1xx → Information**
- **2xx → Success**
- **3xx → Redirection**
- **4xx → Client error**
- **5xx → Server error**

### Most important codes

- **200 → Successful**
- **301 → Moved permanently**
- **401 → Authentication required**
- **403 → No permission**
- **404 → Not found**
- **500 → Server error **

## 7. Explain the OSI model? 

**OSI stands for Open Systems Interconnection. It is a seven-layer model that helps us understand how data travels through a network.**

**So, what are those seven layers?**

**The first layer is the Physical Layer. It transmits raw bits through cables, fiber, Wi-Fi signals, and other physical connections.**

**The second layer is the Data Link Layer. It transfers data in frames within the local network. MAC addresses and switches work at this layer.**

**The third layer is the Network Layer. It handles IP addressing and routing between different networks. Routers, IP, and ICMP work at this layer.**

**The fourth layer is the Transport Layer. It provides end-to-end communication using TCP or UDP. It also works with port numbers.**

**The fifth layer is the Session Layer. It starts, manages, and ends communication sessions between devices.**

**The sixth layer is the Presentation Layer. It translates, encrypts, decrypts, and compresses data.**

**The seventh layer is the Application Layer. It provides network services directly to user applications. Protocols such as HTTP, HTTPS, DNS, SMTP, and FTP work at this layer.**

**From a cybersecurity perspective, the OSI model helps me identify which layer a network issue or attack is happening at.**

## 8. The TCP/IP Model 

**TCP/IP stands for Transmission Control Protocol/Internet Protocol. It is a set of standardized rules that allows computers to communicate over a network, such as the internet. It is very similar to the OSI model, but TCP/IP has four layers: Application, Transport, Internet, and Network Interface.**

**The Application Layer provides network services to users. The Transport Layer delivers data using TCP or UDP. The Internet Layer handles IP addressing and routing, and the Network Interface Layer sends data through Ethernet, Wi-Fi, or cables.**

#### 1. Application Layer

**Protocols:**

- **HTTP/HTTPS – websites**
- **DNS – domain name to IP**
- **SMTP – sending email**
- **FTP – transferring files**

**It combines the Application, Presentation, and Session layers of the OSI model.**

#### 2. Transport Layer

**It is responsible for delivering data between devices.**

**Protocols:**

- **TCP – reliable, connection-oriented**
- **UDP – faster, connectionless**

**For example:**

- **Email and web traffic → TCP**
- **Video calls and online gaming → UDP**

#### 3. Internet Layer

**It handles IP addressing and routing, moving packets from source to destination.**

**Protocols:**

- **IP**
- **ICMP**

**It corresponds to the Network Layer of the OSI model.**

#### 4. Network Interface Layer

**It transmits data within the same local network through Wi-Fi, Ethernet, and physical cables.**

**The following operate at this layer:**

- **MAC addresses**
- **Switches**
- **Ethernet**
- **Wi-Fi**

**It combines the Data Link and Physical layers of the OSI model.**

| **TCP/IP**            | **OSI**                                  |
| --------------------- | ---------------------------------------- |
| **Application**       | **Application + Presentation + Session** |
| **Transport**         | **Transport**                            |
| **Internet**          | **Network**                              |
| **Network Interface** | **Data Link + Physical**                 |

### Interview answer

**TCP/IP stands for Transmission Control Protocol/Internet Protocol. It is a four-layer model that explains how computers communicate over a network such as the internet. Its four layers are Application, Transport, Internet, and Network Interface.**

## 9. What is the CIA Triad? (What is information security) 

**The CIA Triad stands for Confidentiality, Integrity, and Availability.**

**Confidentiality means only authorized people should have access to the data.**
** For example, using encryption or access controls.**

**Integrity means the data should not be changed without authorization.**
** For example, using hashing to detect if a file was modified.**

**Availability means systems and data should be accessible when needed.**
** For example, using backups and redundant systems.**

### Interview answer

**CIA stands for Confidentiality, Integrity, and Availability. Confidentiality means that data is accessible only to authorized users. Integrity means that data is not changed without permission, and availability means that systems and data are accessible when needed.**

## 10. What is IP Addressing?

### What is IP Addressing?

**An IP address is a unique logical address used to identify a device on a network. It also identifies the source and destination of network traffic.**

**There are two main versions: IPv4 and IPv6. IPv4 is 32 bits long, while IPv6 is 128 bits long. IPv6 was created because the number of available IPv4 addresses is limited.**

### If asked about IP address classes

**In traditional classful addressing, there are five IPv4 classes: A, B, C, D, and E. Classes A, B, and C are used for networks of different sizes. Class D is used for multicast, and Class E is reserved for experimental purposes.**

**If asked about the address ranges:**

**Class A ranges from 1 to 126.**
** Class B ranges from 128 to 191.**
** Class C ranges from 192 to 223.**
** Class D ranges from 224 to 239.**
** Class E ranges from 240 to 255.**

**These ranges refer to the first octet of an IPv4 address.**

**What is the difference between IPv4 and IPv6?**

**IPv4 is 32 bits long and uses addresses like 192.168.1.10. IPv6 is 128 bits long and provides a much larger number of addresses.**

**What is a private IP address?**

**A private IP address is used inside a local network and is not directly accessible from the internet. Common private ranges start with 10, 172.16 through 172.31, and 192.168.**

**What is a public IP address?**

**A public IP address identifies a network or device on the internet. It is normally assigned by an Internet Service Provider.**

**What is 127.0.0.1?**

**127.0.0.1 is the loopback address, also called localhost. It allows a device to communicate with and test itself.**

**IP address → identifies a device**

**Source IP → where traffic comes from**

**Destination IP → where traffic goes**

**IPv4 → 32 bits**

**IPv6 → 128 bits**

**Private IP → local network**

**Public IP → internet**

**127.0.0.1 → loopback or localhost**

## 11. Common Port Numbers

| **Port** | **Protocol** | **Purpose**                         |
| -------- | ------------ | ----------------------------------- |
| 20/21    | FTP          | File transfer                       |
| 22       | SSH          | Secure remote access                |
| 23       | Telnet       | Unencrypted remote access           |
| 25       | SMTP         | Sending email                       |
| 53       | DNS          | Domain name resolution              |
| 80       | HTTP         | Unencrypted web traffic             |
| 110      | POP3         | Receiving/downloading email         |
| 137–139  | NetBIOS      | Legacy Windows file/printer sharing |
| 143      | IMAP         | Accessing email on a server         |
| 443      | HTTPS        | Encrypted web traffic               |
| 445      | SMB          | Windows file and printer sharing    |
| 3389     | RDP          | Remote Desktop                      |

**Web:**

- 80 → HTTP
- 443 → HTTPS

**Remote access:**

- 22 → SSH, secure
- 23 → Telnet, insecure
- 3389 → RDP

**Email:**

- 25 → SMTP, send
- 110 → POP3, download
- 143 → IMAP, manage email on server

**Files:**

- 20/21 → FTP
- 445 → SMB

**Network:**

- 53 → DNS
- 137–139 → NetBIOS

**What is the purpose of netbios and why it should be closed? **

NetBIOS allows computers on a local network to communicate and access shared resources, such as files and printers.

If NetBIOS is exposed or not needed, it should be disabled or blocked because attackers may use it to collect information such as:

- Computer names
- User information
- Shared folders
- Network information

## 12. What is the difference between symmetric and asymmetric encryption?

**Symmetric encryption:**

- Uses the same key to encrypt and decrypt data
- Faster
- Used for bulk data encryption
- Example: AES

**Asymmetric encryption:**

- Uses a public key and a private key
- Slower
- Used for authentication, digital signatures, and secure key exchange
- Examples: RSA and ECC

**Memory:**

- Symmetric = Same key
- Asymmetric = A pair of keys

**TLS uses both:**

- Asymmetric cryptography helps establish the secure session.
- Symmetric encryption protects the actual

## 13. What is a switch? 

### Interview answer

A switch is a network device that connects devices within the same local network.

It mainly works at Layer 2, the Data Link Layer, and uses MAC addresses to forward data to the correct device. Unlike a hub, which sends data to every connected device, a switch sends it only where it needs to go. This makes the network more efficient and secure.

### Memory formula

**Switch = LAN + Layer 2 + MAC address + correct device**

## 14. What can be typically found in a DMZ? 

### Interview answer

A DMZ stands for Demilitarized Zone. It is a separate network placed between the internet and the organization’s internal network.

Public-facing servers, such as web, email, and DNS servers, are usually placed in the DMZ.

This adds an extra layer of security because if a public server is compromised, the attacker cannot directly access the internal network.

### Memory formula

**DMZ = A protective network zone between the internet and the private network**

## 15. What sorts of anomalies would you look for to identify a compromised system

### Interview answer

To identify a compromised system, I would look for activity that is different from the system’s normal behavior.

For example, unusual outbound traffic, repeated failed logins followed by a successful login, access from unexpected locations, suspicious processes, and unusual file or registry changes. I would also check for unusual DNS requests and connections to known malicious IPs or domains.

### Memory formula

**Compromise = unusual login + unusual traffic + system changes**

## 16. How would you strengthen user authentication? 

**To make user accounts more secure, I would use multi-factor authentication.**

MFA requires two or more different factors, such as something you know, something you have, or something you are. For example, a password combined with a security token or fingerprint. I would also use strong password policies, account lockout rules, and monitor for unusual login activity.

## 17. How would you handle a compromised endpoint? 

### Interview answer

As an incident responder, I need to know how to handle different security incidents, such as malware, ransomware, a successful phishing attack, or a lost laptop containing sensitive information.

To handle a compromised endpoint, I would follow the SANS six-step incident response process:

The first step is Preparation, followed by Identification, Containment, Eradication, Recovery, and Lessons Learned.

Preparation means having the right plan, tools, and team ready before an incident happens.

Identification means confirming whether the alert is a real security incident and determining which systems are affected.

Containment means isolating the affected endpoint to limit the damage and prevent the threat from spreading.

Eradication means finding the root cause and completely removing the threat from the environment.

Recovery means safely returning the system to normal operation and monitoring it to make sure the threat is gone.

Finally, Lessons Learned means documenting the incident, reviewing the response, and improving the process for future incidents.

1. **Preparation** — Have a plan
2. **Identification** — Confirm the incident
3. **Containment** — Isolate the system
4. **Eradication** — Remove the threat
5. **Recovery** — Restore and monitor
6. **Lessons Learned** — Document and improve

## 18. How would you handle patch management? 

### Interview answer

To handle patch management effectively, I would first maintain an inventory of systems and run vulnerability scans to identify missing patches.

Then, I would prioritize patches based on severity and business risk. Critical vulnerabilities and internet-facing systems would receive the highest priority. Before deploying a patch to production, I would test it in a controlled environment to make sure it does not cause operational problems.

After testing, I would schedule and deploy the patch with a backup and rollback plan in place. Finally, I would verify that the patch was installed successfully, continue monitoring the system, and document the results.

## 19. What is a Watering Hole Attack? 

A watering hole attack is a targeted attack where an attacker compromises a website that a specific organization or group frequently visits.

First, the attacker chooses a target and observes which websites the target commonly uses. Then, they compromise one of those trusted websites by adding malicious code. When the victim visits the site, their device may become infected, allowing the attacker to gain access to the system or network.

It is called a watering hole attack because the attacker waits in a place where the victim is likely to visit.

### Quick note

**Target → Observe → Infect the website → Wait → Gain access**

### Memory formula

**Don’t chase the victim — infect the place they visit.**

**Instead of chasing the victim, compromise a website the victim is likely to visit.**

## 20. What is a SIM/SIEM? 

SIM stands for Security Information Management. It mainly collects and stores security logs for reporting and investigation.

SIEM stands for Security Information and Event Management. It is a security tool used by SOC teams to collect logs from different sources, such as computers, servers, applications, firewalls, and EDR systems.

It analyzes and correlates these logs to identify suspicious activity and generate alerts. For example, many failed login attempts from the same IP address may trigger a possible brute-force alert.

However, a SIEM alert does not prove that an incident happened. A security analyst still needs to investigate it. Splunk and IBM QRadar are common SIEM tools.

## 21. How would you prevent a zero-day attack? 

### Interview answer

A zero-day attack uses a vulnerability that does not have a patch yet, so it is difficult to prevent completely.

**To reduce the risk**, I would use EDR with behavioral detection and sandboxing to identify unusual activity. I would also monitor alerts through the SIEM and use least privilege and network segmentation to limit the damage.

**Finally**, once the vendor releases a security patch, I would test and apply it as soon as possible.

## 22. Difference between firewall and proxy? 

A firewall and a proxy are both used for network security, but they work differently.

A firewall controls network traffic and allows or blocks connections based on security rules, such as IP addresses, ports, and protocols. A traditional firewall mainly works at Layers 3 and 4, while a Next-Generation Firewall can also 		inspect application traffic at Layer 7.

A proxy acts as a middleman between the user and the internet. It mainly works at Layer 7, sends requests on behalf of the user, and can hide internal IP addresses, filter websites, and log web activity.

**Firewall**

- Allows or blocks traffic
- Uses security rules
- Checks IP addresses, ports and protocols
- Traditional firewall: mainly Layer 3 and Layer 4
- NGFW can also inspect Layer 7 traffic

**Proxy**

- Works as a middleman
- Mainly works at the Application Layer — Layer 7
- Sends requests on behalf of users
- Can hide internal IP addresses
- Filters and logs web activity

### Memory formula

**Firewall = Allow or block**
 **Proxy = Middleman**

## 23. Can you describe a salted hash? 

**Basically, a salt is a unique random value added to a password before the password is hashed. For example, if two users have the same password, their salts will be different, so their stored hashes will also be different. This helps protect against rainbow table attacks and makes password cracking more difficult. The salt does not need to be secret, so it is usually stored together with the hash.**

## 24. What happens when you type google.com in your browser?

Basically, when I type[ google.com](http://google.com) into my browser, the browser first checks its cache. If it cannot find the IP address there, it sends a DNS request to translate the domain name into an IP address.

Then, the browser establishes a connection with the web server. It normally performs a TCP three-way handshake and then a TLS handshake to create a secure HTTPS connection.

After that, the browser sends an HTTPS request, and the server sends back an HTTPS response. Finally, the browser receives the HTML and downloads other resources, such as CSS, JavaScript, and images, and then displays the webpage.

**Cache → DNS → TCP → TLS → HTTPS request/response → Webpage**

## 25. When would DNS use TCP instead of UDP? 

**Basically, DNS normally uses UDP on port 53 because it is fast and does not require a connection. However, if the DNS response is too large or truncated, it can switch to TCP.**

**DNS also uses TCP for zone transfers between DNS servers. For example, DNSSEC may create larger responses that sometimes require TCP.**

### Quick Notes
- **DNS = Domain Name System**
- **Converts domain names into IP addresses**
- **Uses port 53 with both UDP and TCP**
- **UDP = normal DNS queries**
- **TCP = large or truncated responses**
- **TCP = DNS zone transfers**
- **DNSSEC can create larger responses**

### Memory formula

**Normal query = UDP**
** Large/truncated response or zone transfer = TCP**

## 26. What is the difference between UDP and TCP? 

### TCP — Transmission Control Protocol

- Connection-oriented
- Uses the three-way handshake
- Reliable
- Guarantees delivery
- Keeps data in the correct order
- Slower than UDP
- Examples: HTTPS, email and file transfer

### UDP — User Datagram Protocol

- Connectionless
- No handshake
- Faster
- Does not guarantee delivery
- Does not guarantee the correct order
- Examples: DNS, streaming, gaming and voice calls

### Memory formula

**TCP = reliable but slower**
 **UDP = faster but less reliable**

DNS normally uses UDP because it is faster. However, it can use TCP when the response is too large or truncated, or during a zone transfer.

## 27. What is a traceroute? 

So, traceroute is basically a network diagnostic tool. It shows the path that packets take from a source device to a destination.

Each router along that path is called a hop, and traceroute also shows the approximate response time for each hop. For example, if there is a connection problem or delay, I can use traceroute to see where along the network path the problem may be happening.

### Quick Notes
- Network diagnostic tool
- Shows the path to a destination
- Each router = one **hop**
- Shows approximate response time
- Uses **TTL — Time to Live**
- Helps investigate connectivity problems and delays
- Windows command: `tracert`
- Linux/macOS command: `traceroute`

### Memory formula

**Traceroute = Path + Hops + Response time**

## Possible follow-up questions

### What is a hop?

A hop is a router or network device that a packet passes through on its way to the destination.

### What is TTL?

TTL stands for Time to Live. It limits how many hops a packet can pass through before it is discarded, helping prevent packets from circulating forever.

### What is the difference between ping and traceroute?

Ping checks whether a destination is reachable and measures the response time. Traceroute shows the path and individual hops used to reach that destination.

**Remember:**

- Ping = Can I reach it?
- Traceroute = How do I reach it?

## 28. What is DLP? 

DLP stands for Data Loss Prevention. Basically, it helps protect sensitive information from being shared, transferred, or accessed without authorization.

For example, if an employee tries to send confidential customer information to a personal email address, DLP can detect the sensitive data, block the email, and generate an alert. It can protect data at rest, in transit, and in use.l

### Quick Notes
- DLP = Data Loss Prevention
- Protects sensitive information
- Uses security policies
- Monitors how data is accessed and transferred
- Can detect, alert, block and log
- Protects against accidental and intentional data loss
- Protects data at rest, in transit and in use

### Memory formula

**DLP = Detect + Alert + Block sensitive data**

## Possible follow-up questions

### What types of data can DLP protect?

DLP can protect sensitive information such as customer data, financial information, health records, credentials and confidential company documents.

### Can DLP prevent accidental data loss?

Yes. DLP can prevent both accidental and intentional data loss. For example, it may block an employee from accidentally sending confidential information to the wrong email address.

### What is the difference between data loss and data leakage?

Data loss means information is lost, deleted or becomes unavailable. Data leakage means sensitive information is exposed or transferred to an unauthorized person or location.

### What would you do if you received a DLP alert?

First, I would review the alert and identify the user, the sensitive data involved, the destination and the action taken by DLP. Then, I would determine whether the activity was authorized, accidental or potentially malicious. If necessary, I would document and escalate the incident.

## 29. What is a SOC (Security Operations Center)? 

SOC stands for Security Operations Center. Basically, it is a security team responsible for continuously monitoring an organization’s environment, detecting and investigating threats, and responding to security incidents.

The SOC monitors alerts and logs from different sources, such as endpoints, servers, firewalls, applications and cloud services. Analysts commonly use a SIEM to collect and correlate this data, prioritize alerts and respond to incidents more efficiently.

The main goal of a SOC is to reduce security risk and protect the organization’s systems and data.

### What is the role of SIEM in a SOC?

A SIEM collects and correlates security logs from different sources and generates alerts for suspicious activity. It helps SOC analysts investigate and respond to potential incidents more efficiently.

### Does a SIEM prove that an incident happened?

No. A SIEM provides alerts and supporting evidence, but an analyst must investigate the activity and determine whether it is a true security incident.

### What is the difference between a SOC and a SIEM?

A SOC is the security team or function responsible for monitoring and incident response. A SIEM is one of the tools used by the SOC to collect logs, correlate events and generate alerts.

## 30. What are the basic responsibilities of a SOC Team? 

Basically, a SOC team is responsible for continuously monitoring the organization’s environment and protecting its systems and data.

The team manages security tools, reviews alerts, and investigates suspicious activity. When a real security incident is confirmed, the SOC works to contain the threat, reduce the impact, and prevent it from happening again.

The team also helps reduce downtime, supports business continuity, improves security strategies, and provides information for audits and compliance.

### Important distinction

**SOC Analyst responsibilities** focus primarily on daily alert investigation:

- Review alerts
- Check logs
- Collect evidence
- Decide true or false positive
- Document and escalate

**SOC Team responsibilities** cover a broader set of functions:

- Manage security operations and tools
- Respond to incidents
- Reduce business impact
- Improve security strategy
- Support audit and compliance

## 31. What is dwell time? 

Dwell time is the amount of time an attacker stays inside a system before being detected. A shorter dwell time is better because the SOC team can respond quickly and reduce the damage.

**Example:**

If an attacker enters the network on Monday but is detected on Friday, the dwell time is four days.

## 32. What is Incident Response? 

**Incident response is the process of identifying, containing, removing, and recovering from a security incident. Its main goal is to reduce damage and restore normal operations as quickly as possible.**

**According to the NIST framework, incident response has four main phases: preparation, detection and analysis, containment, eradication and recovery, and post-incident activity. During the final phase, the team reviews the incident, documents what happened, and identifies ways to prevent it from happening again.**

### Short Answer
**Incident response is how a security team handles a cyber incident. The main steps are preparation, detection and analysis, containment, eradication and recovery, and lessons learned. The goal is to stop the threat, reduce damage, and restore normal operations.**

## 33. What is The Cyber Kill Chain? 

**The Cyber Kill Chain is a model that describes the stages of a cyberattack from initial reconnaissance to the attacker’s final objective. It has seven stages: reconnaissance, weaponization, delivery, exploitation, installation, command and control, and actions on objectives.**

**For example, an attacker may collect information about a company, create a malicious attachment, and deliver it through a phishing email. If the victim opens it, the attacker may exploit a vulnerability, install malware, establish command-and-control communication, and finally steal or encrypt data.**

## Cyber Kill Chain — quick study outline

1. **Reconnaissance — gathering information about the target**
** *IP, domain, employees, vulnerabilities***
2. **Weaponization — malware or preparing a malicious file**
** *Malicious document, payload***
3. **Delivery — delivering a malicious file or link to the target**
** *Phishing email, website, USB***
4. **Exploitation — exploiting a weakness to execute malicious code**
** *Exploit, PowerShell execution***
5. **Installation — installing malware on the system**
** *Malware installation, persistence, backdoor***
6. **Command and Control (C2/C&C) — remotely controlling the compromised system**
** *C2 server, beaconing, suspicious outbound connection***
7. **Actions on Objectives — the attacker’s final objective**
** *Data exfiltration, encryption, ransomware***

## 34. What is a PCAP file? Can you read a PCAP and explain what you saw there? 

### What is a PCAP file? Can you read it?

**A PCAP file contains captured network traffic. Yes, I can analyze it using Wireshark. I usually check source and destination IP addresses, ports, protocols, timestamps, DNS queries, and connection patterns to identify suspicious activity.**

### Can you explain what you saw there?

**In one of my training labs, I noticed repeated connections from an internal host to a suspicious external IP address over port 443. I checked the connection times and frequency. The activity looked like possible C2 communication, so I documented my findings and escalated the case for further investigation.**

### Quick memory note

**PCAP = recorded network traffic**

**I check:**
 **IP → Port → Protocol → Time → DNS → Connection pattern**

**I find suspicious activity → document → escalate.**

## 35. Tell me about penetration testing? 

Penetration testing is an authorized security test used to identify and safely exploit vulnerabilities in a system, network, or web application. Ethical hackers use techniques similar to real attackers to understand how far an attacker could go and what data could be accessed. The main stages are planning and reconnaissance, scanning, exploitation, post-exploitation, and reporting. Penetration testing can be black-box, gray-box, or white-box, depending on how much information is provided to the tester.

If the answer is too long, use this shorter version for memorization:

Penetration testing is an authorized security test used to find and safely exploit vulnerabilities. Ethical hackers act like real attackers to understand the possible impact of an attack. The main steps are planning, scanning, exploitation, checking the impact, and reporting the findings.

## 36. What is the OWASP Top 10 list and why is it important? 

OWASP stands for **Open Worldwide Application Security Project**. It is a nonprofit organization focused on web application security.

The OWASP Top 10 is a list of the most critical security risks affecting web applications. It helps organizations and developers:

- Understand common web security risks
- Prioritize the most serious risks
- Identify and fix vulnerabilities
- Build more secure applications

### Important Examples

- **Broken Access Control:** A user can access information or functions without permission.
- **Injection:** An attacker sends malicious input, such as an SQL command, to an application.
- **Security Misconfiguration:** The application has unsafe settings, default passwords, or unnecessary services.
- **Cryptographic Failures:** Sensitive information is not properly encrypted or protected.

The OWASP Top 10 is updated because web application risks change over time.

### Short memory formula

**OWASP Top 10 = Critical web risks → Prioritize → Identify → Fix**

## 37. What is the most secure way of protecting data? 

The best way to protect sensitive data is to use encryption together with strong access controls.

Encryption converts readable data into an unreadable format. Only someone with the correct key can decrypt and read it. Data should be encrypted both at rest and in transit.

There are two main types of encryption: symmetric and asymmetric. Symmetric encryption uses the same key for encryption and decryption, while asymmetric encryption uses a public key and a private key.

## 38. The process of data encryption? 

Encryption starts with readable data called plaintext. An encryption algorithm uses a key to convert the plaintext into an unreadable format called ciphertext.

To read the information again, the ciphertext must be decrypted with the correct key. In symmetric encryption, the same secret key is used for encryption and decryption. In asymmetric encryption, a public key and a private key are used.

### Key notes

- **Plaintext** → readable data
- **Encryption algorithm** → a mathematical process that transforms data
- **Encryption key** → a key used to encrypt data
- **Ciphertext** → unreadable encrypted data
- **Decryption** → converting ciphertext back into plaintext

**Memory formula:**

**Plaintext → Encryption → Ciphertext → Decryption → Plaintext**

## 39. What is the difference between encoding, encrypting, and hashing? 

Encoding, encryption, and hashing all transform data, but they have different purposes.

Encoding changes data into another format for compatibility or transmission. It does not require a secret key and can easily be reversed.

Encryption protects confidentiality by converting readable data into ciphertext. It is reversible only with the correct key.

Hashing creates a fixed-length hash value and is designed to be one-way. It is commonly used for password storage and integrity checking.

**Memory formula:**

- **Encoding = Change format**
- **Encryption = Keep secret**
- **Hashing = Create fingerprint**

Important: **Base64 is not encryption.** It is only encoding.

## 40. What is a DDoS Attack? 

### Interview answer

### DDoS stands for Distributed Denial of Service. It is a cyberattack where multiple compromised devices flood a server, network, or service with a large amount of traffic.

### The goal is to overwhelm the target and make the service slow or unavailable to legitimate users. A DoS attack usually comes from one source, while a DDoS attack comes from many distributed sources.

## 41. How do routers work? 

### Interview answer

**A router connects different networks and forwards data packets between them. It checks the destination IP address of each packet and uses its routing table to choose the best available path.**

**For example, a home router connects local devices to the internet and sends incoming data to the correct device. It may also use DHCP to assign local IP addresses and NAT to allow multiple devices to share one public IP address.**

**Router checks the destination IP and forwards the packet to the correct network.**

## 42. Explain how to make a SQL query to search a database? ?? 

**SQL stands for Structured Query Language, and it is used to communicate with relational databases. To search for data, I use a SELECT statement, specify the columns and table, and add a WHERE clause if I need to filter the results.**

**For example, `SELECT name, email FROM employees WHERE department = 'IT';` returns the names and email addresses of employees in the IT department.**

### Key noteslər

- **SQL = Structured Query Language**
- **`SELECT` → selects data**
- **`FROM` → specifies the table**
- **`WHERE` → filters the results**
- **`*` → all columns**
- **`INSERT` → adds new data**
- **`UPDATE` → modifies existing data**
- **`DELETE` → deletes data**

## 43. Describe DNS? 

**DNS stands for Domain Name System. It works like the internet’s phone book. It translates domain names, such as[ google.com](http://google.com), into IP addresses that computers can understand. DNS uses port 53.**

## 44. What protocol does DNS use? 

**DNS mainly uses UDP on port 53 because it is fast and efficient for standard queries. It can also use TCP when the response is too large or for zone transfers.**

## 45. How would you defend against a cross-site scripting (XSS) attack? 

**XSS stands for Cross-Site Scripting. It happens when an attacker injects malicious client-side code, usually JavaScript, into a trusted website. The code runs in another user’s browser and may steal cookies or redirect the user to a malicious website. To prevent XSS, I would use input validation, output encoding, sanitization, Content Security Policy, regular vulnerability scanning, and a Web Application Firewall.**

## 46. Difference between stored and reflected XSS? 

**Stored XSS happens when a malicious script is permanently saved on the server, such as in a database, comment field, or forum post. It can affect every user who opens the infected page. Reflected XSS is not permanently stored. It is usually delivered through a malicious link or request and runs when the victim opens it.**

## 47. Subnet?

**A subnet is a smaller logical section of a larger IP network. Organizations use subnets to separate devices or departments, reduce network traffic, and improve security and management. Devices in different subnets normally communicate through a router or Layer 3 switch.**

## 48. /32 and /24 how many hosts?

IPv4 addresses contain 32 bits.

A /24 network leaves 8 bits for hosts, so it has 256 total IP addresses and normally 254 usable host addresses.

A /32 represents one specific IP address and is commonly used to identify a single host.

## 49. TCP Header? 

**TCP header is the first 24 bytes of the TCP segment that help diagnose communication issues between two endpoints. **

**TCP header = packet control information**

**Minimum size = 20 bytes**

**Maximum size = 60 bytes**

**SYN = initiates a connection**

**ACK = acknowledges data**

**FIN = closes a connection normally**

**RST = immediately resets a connection**

### Interview answer

**A TCP header is the control information at the beginning of a TCP segment. It contains fields such as source and destination ports, sequence and acknowledgment numbers, TCP flags, window size, and checksum. It is normally 20 bytes, but it can be up to 60 bytes when options are included. These fields help us understand and troubleshoot communication between two endpoints.**

## 50. TCP Flags? 

**TCP flags show the state of a TCP connection.**
** The most common flags are SYN, ACK, PSH, FIN, RST, and URG.**
** For example, SYN starts a connection, ACK confirms received data, and FIN closes the connection.**
** As a SOC analyst, I can review TCP flags in Wireshark to understand connection activity and identify suspicious behavior.**

### Memory Aid
**SYN starts — ACK confirms — FIN finishes — RST stops.**

## 51. HTTP Header? 

**HTTP headers contain additional information exchanged between a client and a web server.**
** They can include the host, user-agent, content type, cookies, and authorization information.**
** As a SOC analyst, I review HTTP headers to investigate suspicious web requests and identify unusual activity.**

### Memory Aid
**HTTP header tells us information about the request or response.**

## 52. SQL Injection? 

**SQL injection is a web attack where an attacker inserts malicious SQL code into an input field.**
** The goal may be to bypass authentication or read, modify, or delete database information.**
** It can be prevented by using parameterized queries, input validation, and limited database permissions.**

### Memory Aid
**Attacker puts SQL code into an input field to access the database.**

## 53. What are the differences between vulnerability scanning and penetration testing? 

**Vulnerability scanning looks for known vulnerabilities in a system and reports potential security exposures.**
** Penetration testing goes further by attempting to exploit those weaknesses and determine how much unauthorized access an attacker could gain.**
** In simple terms, vulnerability scanning identifies weaknesses, while penetration testing tests their real impact.**

### Shortest memory aid

**Vulnerability scanning finds weaknesses. Penetration testing attempts to exploit them.**

## 54. Authenticated (credentialed) and unauthenticated (uncredentialed) vulnerability scanning

An unauthenticated scan checks a system from the outside without using login credentials. It shows vulnerabilities that an external attacker may be able to see.
 An authenticated scan uses authorized credentials to access the system and perform a deeper assessment.
 It can identify missing patches, outdated software, configuration issues, and local vulnerabilities.

### Shortest memory aid

An unauthenticated scan checks from the outside. An authenticated scan uses credentials to check the system more deeply.

## 55. Port scanning and tools? 

**Port Scanning is one of the most popular techniques attackers use to discover Which ports on a network are open for receiving and sending data. **

**It's a way to find out if there is any open or close ports for sending and receiving data **

**tools that I know can be used for Port scanning are: **

**Nmap **

**Netcat **

**Zenmap **

## 56. Vulnerability scanning and tools? 

Vulnerability scanning is the process of checking systems, networks, and applications for security weaknesses. It can identify open ports, outdated software, misconfigurations, and known vulnerabilities. Common tools include Nessus, Qualys, OpenVAS, Nmap, Burp Suite, and OWASP ZAP. After the scan, I review the findings, prioritize them by severity, and recommend remediation.

### Short answer to memorize

Vulnerability scanning checks systems for security weaknesses. It can find open ports, outdated software, misconfigurations, and known vulnerabilities. Common tools are Nessus, Qualys, OpenVAS, and Nmap.

## 57. What is phishing? 

**Phishing is a type of social engineering attack often used to steal user data, including login credentials and credit card numbers. It could be done through an email, instant message or text message. **

**Types of phishing attacks **

**Spear phishing **

**Whaling **

**Smishing **

**Vishing **

**Email phishing **

## 58. Where are logs on Linux and Windows hosts? 

On Windows hosts, logs can be viewed through Event Viewer, including Security, System, and Application logs. The log files are usually stored in `C:\Windows\System32\winevt\Logs`. On Linux hosts, logs are generally stored in the `/var/log` directory. For example, authentication events may be found in `/var/log/auth.log` or `/var/log/secure`.

### Short answer to memorize

On Windows, I check logs in Event Viewer, especially Security, System, and Application logs. On Linux, logs are usually stored in the `/var/log` directory, such as `auth.log`, `syslog`, or `secure`.

## 59. Rainbow Table Attack? 

**Rainbow table attacks are a type of attack that attempts to discover the password from the hash and they use rainbow tables, which are huge databases of precomputed hashes to discover the password.**

### Shortest answer to memorize

A rainbow table attack compares stolen password hashes with a database of precomputed hashes to find the original password. Salting helps prevent this attack.

## 60. Walk me through your day-to-day activities at your current job.

“I start by checking my email and reviewing any open cases from the previous shift. During my shift, I monitor security alerts in QRadar and use Splunk to search logs and investigate suspicious activity. I also review reported phishing emails and help with vulnerability scans using Nessus. I document my findings in the ticketing system and escalate serious incidents to the appropriate team.
