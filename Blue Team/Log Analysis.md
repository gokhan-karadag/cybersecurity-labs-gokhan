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


Applications, audit systems, security controls, and operating systems create different records. These categories can overlap. For example, an account change may be both an audit event and a security event. Each source adds another piece to the investigation.​

















