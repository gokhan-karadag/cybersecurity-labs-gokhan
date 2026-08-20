<img width="1215" height="269" alt="image" src="https://github.com/user-attachments/assets/929ada74-a7e3-4962-a33b-a67f11b79081" />

https://tryhackme.com/room/anthem

# Anthem - Introduction

**Anthem** is a beginner-friendly TryHackMe room focused on **reconnaissance, enumeration, initial access, and privilege escalation**.

In this room, we will approach the target like a penetration tester while thinking like a SOC analyst:

> **Enumerate → Identify → Analyze → Exploit → Escalate → Validate**

The objective is not just to capture flags. We want to understand the **attack path** and recognize the artifacts each stage could leave behind.

---

## Attack Objectives

- **Reconnaissance** – Discover exposed ports and services.
- **Enumeration** – Identify users, directories, technologies, and potential attack vectors.
- **Web Analysis** – Inspect the web application for useful information and credentials.
- **Initial Access** – Use discovered credentials to gain access via RDP.
- **Post-Exploitation** – Enumerate the compromised host and locate flags.
- **Privilege Escalation** – Identify misconfigurations and escalate privileges.

---

## Hacker Mindset

Don't attack blindly.

Every exposed service is a potential **attack surface**, and every piece of information can become an **IOC, credential, or attack vector**.

```text
Target
  ↓
Port Scan
  ↓
Service Enumeration
  ↓
Web Enumeration
  ↓
Credential Discovery
  ↓
Initial Access
  ↓
Host Enumeration
  ↓
Privilege Escalation
  ↓
Root / Administrator
```

---

## SOC Mindset

While attacking the machine, also think about what a defender could detect:

```text
Nmap Scan          → Network Recon Activity
Web Enumeration    → Repeated HTTP Requests
Credential Access  → Authentication Activity
RDP Login          → Remote Logon Event
Privilege Esc.     → Suspicious Process / Permission Activity
```
---

# Task 1 - Website Analysis

Start with reconnaissance.

```bash
sudo nmap -sV -sC -O -p80,3389 -Pn 10.10.15.255 -vvv
```

Our first mission:

**Identify the attack surface before choosing the attack path.**

<img width="1322" height="784" alt="image" src="https://github.com/user-attachments/assets/02e65723-320e-4ccf-b5ca-7b66eebf8a1d" />

## 1-1. Nmap Port Scanning

Let's run **Nmap** to identify open ports, running services, service versions, and the target operating system.

### Targeted Port Scan

```bash
sudo nmap -sV -sC -O -p80,3389 -Pn 10.10.15.255 -vvv
```

### Options Explained

- `-sV` → Detect service versions
- `-sC` → Run default Nmap scripts
- `-O` → Detect the operating system
- `-p80,3389` → Scan ports **80 (HTTP)** and **3389 (RDP)**
- `-Pn` → Skip host discovery and treat the target as online
- `-vvv` → Show very verbose output

<img width="832" height="866" alt="image" src="https://github.com/user-attachments/assets/199520f6-2842-4351-ae60-03b69b48a38f" />

### Top 1000 Ports Scan

By default, Nmap scans the **top 1000 most commonly used TCP ports**.

```bash
sudo nmap -sV -sC -O -Pn 10.10.15.255
```

### 1–2. What port is the web server running on?

Run the following Nmap scan:

```bash
sudo nmap -sV -sC -O -p80,3389 -Pn 10.10.15.255 -vvv
```

<img width="832" height="866" alt="Nmap scan showing HTTP port" src="https://github.com/user-attachments/assets/228973e3-7da1-4f93-b577-a5cfb153cc4a" />

**Answer:** `80 (HTTP)`

---

### 1–3. What port is used for the Remote Desktop service?

Run the same Nmap scan:

```bash
sudo nmap -sV -sC -O -p80,3389 -Pn 10.10.15.255 -vvv
```

<img width="832" height="866" alt="Nmap scan showing RDP port" src="https://github.com/user-attachments/assets/0083d276-3f19-4100-b0e9-51a6d2662cd9" />

**Answer:** `3389 (RDP)`

---

### 1–4. What is a possible password found on one of the pages that web crawlers check?

Check the website's `robots.txt` file:

```text
robots.txt
```

<img width="525" height="291" alt="robots.txt containing possible password" src="https://github.com/user-attachments/assets/c293dc10-13e4-4715-a5d7-2f1bc64cee66" />

**Answer:** `UmbracoIsTheBest!`

---

### 1–5. What CMS is the website using?

<img width="1282" height="856" alt="Website CMS identification" src="https://github.com/user-attachments/assets/e0ae4165-8afc-4c6b-9c6e-bec1c8970c55" />

**Answer:** `Umbraco`

---

### 1–6. What is the domain of the website?

<img width="1281" height="853" alt="Website domain information" src="https://github.com/user-attachments/assets/9df59906-8c76-4351-941c-429de67edadb" />

**Answer:** `Anthem.com`

---

### 1–7. What is the name of the Administrator?

<img width="656" height="822" alt="Administrator information" src="https://github.com/user-attachments/assets/27474ff6-4813-42cc-af5e-ac4987ac806e" />

<img width="1094" height="358" alt="Administrator identification" src="https://github.com/user-attachments/assets/093d2889-0b84-458e-b4d2-e0a23825f671" />

**Answer:** `Solomon Grundy`

---

### 1–8. Can we find the email address of the Administrator?

<img width="799" height="728" alt="Email enumeration" src="https://github.com/user-attachments/assets/b69b1be5-8f85-4125-8d18-84960dd0c652" />

<img width="803" height="718" alt="Administrator email identification" src="https://github.com/user-attachments/assets/33e68682-7345-46e0-a89c-ca79ee72e2e4" />

During enumeration, the following email address was found:

```text
JD@anthem.com
```

Based on the Administrator's name, **Solomon Grundy**, and the observed email naming convention:

**Answer:** `SG@anthem.com`

## Task 2 – Spot the Flags

Our beloved admin left some flags behind that we need to gather before proceeding to the next task.

---

### 2–1. Anthem are hiring! What is Flag 1?

The flag can be found by checking the page source of:

```text
/archive/we-are-hiring/
```

Since the clue mentions **metadata**, inspect the page source and look for information stored inside the HTML metadata.

> **Hint:** Metadata means "data about data."

<img width="1795" height="343" alt="image" src="https://github.com/user-attachments/assets/d056a7c3-dbc8-4132-a3a4-6cee8201a8a6" />

**Answer:**

```text
THM{L0L_WH0_US3S_M3T4}
```

**Location:** Page source → `/archive/we-are-hiring/`

````markdown

### 2–2. What is Flag 2?

The second flag can be found by inspecting the page source of:

```text
/blog/
````

Open the page source and search through the HTML for the hidden flag.

<img width="996" height="247" alt="image" src="https://github.com/user-attachments/assets/9bd843ab-fab8-4da1-803b-af9c6ab99965" />

**Answer:**

```text
THM{G!T_G00D}
```

**Location:** Page source → `/blog/`

```
```
````markdown
### 2–3. What is Flag 3?

The third flag can be found by navigating to the following author page:

```text
/authors/jane-doe/
````

Inspect the page and look for the hidden flag.

<img width="1216" height="791" alt="image" src="https://github.com/user-attachments/assets/710caa46-e85d-4f60-9f32-9e5e4239586b" />

**Answer:**

```text
THM{L0L_WH0_D15}
```

**Location:** `/authors/jane-doe/`

```
```

````markdown
### 2–4. What is Flag 4?

The fourth flag can be found by inspecting the page source of:

```text
/archive/a-cheers-to-our-it-department/
````

Open the page source and search through the HTML for the hidden flag.

<img width="1257" height="475" alt="image" src="https://github.com/user-attachments/assets/309a409b-7c9b-442e-884e-eadaa1bf15cc" />

<img width="1257" height="475" alt="image" src="https://github.com/user-attachments/assets/4685022d-ac8d-434a-9ef0-dc7d49ee4d6d" />

**Answer:**

```text
THM{AN0TH3R_M3TA}
```

**Location:** Page source → `/archive/a-cheers-to-our-it-department/`

```
```
merhaba gardas


## Task 3 – Final Stage

Let's get into the box using the intelligence we gathered during the previous enumeration stages.

---

### 3–1. Let's figure out the username and password to log in to the box

Based on the information gathered during the enumeration phase, we identified the following credentials:

```text
Username: SG
Password: UmbracoIsTheBest!
```

Since the target is **not joined to a domain** and RDP is available on port `3389`, we can attempt to connect using the discovered credentials.

#### Connect via RDP from Kali Linux

```bash
xfreerdp /v:10.10.189.188 /u:SG /p:'UmbracoIsTheBest!' /dynamic-resolution
```

<img width="1672" alt="RDP connection using SG credentials" src="https://github.com/user-attachments/assets/ec49c5fb-78d5-457c-82ac-06adf7677fa8" />

**Credentials:**

```text
SG:UmbracoIsTheBest!
```

<img width="1672" alt="RDP access to the target machine" src="https://github.com/user-attachments/assets/83aacaac-5149-4398-bdb2-b930d0724f90" />

---

#### Administrator RDP Access

After obtaining the Administrator credentials, connect to the target again using:

```bash
xfreerdp /v:10.10.99.229 /u:Administrator /p:'ChangeMeBaby1MoreTime' /dynamic-resolution
```

**Administrator Credentials:**

```text
Username: Administrator
Password: ChangeMeBaby1MoreTime
```

---

### 3–2. Gain initial access to the machine. What are the contents of `user.txt`?

After gaining initial access to the target machine, locate and read the `user.txt` file.

<img width="1027" height="802" alt="image" src="https://github.com/user-attachments/assets/91309e3f-e0b8-450a-a9e0-253cd9026070" />


**Answer:**

```text
THM{N00T_NO0T}
```

**Flag:** `user.txt`

---

### 3–3. Can we spot the admin password?

Let's look for recently used files.

<!-- Upload this image to GitHub and replace the URL below -->

<img width="1043" height="817" alt="image" src="https://github.com/user-attachments/assets/56f74f72-17f0-4796-9053-0ead6bfdc01e" />


We find a file named `restore`, but we don't have access. Let's see who does.

<img width="1041" height="818" alt="image" src="https://github.com/user-attachments/assets/d7e7cdd5-6f6b-46b9-872d-180432b8a47a" />


We can see that nobody has permissions, but the **Add...** button isn't grayed out.

<img width="1051" height="816" alt="image" src="https://github.com/user-attachments/assets/d9b6f64f-8835-422a-98b8-afe2c3a60452" />


We can add `SG` as someone with permissions, so we add the hostname in the box and then give **Full Access** rights to that user.

```text
WIN-LU09299160F/SG
```

or

```text
SG
```

<img width="1047" height="820" alt="image" src="https://github.com/user-attachments/assets/0a7fa0ee-eac5-4f04-bd78-d5453ee63b0a" />

<img width="1053" height="818" alt="image" src="https://github.com/user-attachments/assets/7b3fed61-ce64-43de-b6fd-35880121529e" />

<img width="889" height="722" alt="image" src="https://github.com/user-attachments/assets/1e230561-7090-4a0f-a199-5fee42ab4cf7" />


Now we can access the file and see that its contents look like a password:

```text
ChangeMeBaby1MoreTime
```

<img width="1048" height="816" alt="image" src="https://github.com/user-attachments/assets/eb9ca08c-11cb-41f3-bd8d-c85c0b54fb72" />


**Answer:**

```text
ChangeMeBaby1MoreTime
```

---

### 3–4. Escalate your privileges to root. What are the contents of `root.txt`?

Let's try to open `cmd.exe` as Administrator and enter the password when prompted.

We get access and can see that we are now the Administrator.

<img width="994" height="541" alt="image" src="https://github.com/user-attachments/assets/b3abda4a-4666-44ff-9a3c-4945b7b13180" />


Now let's find the flag.

> **Note:** In Windows CTFs, flags are often located in a user's **Desktop** or **Documents** folder, so those are good places to check.

#### Check the Current Directory

In Windows Command Prompt, `cd` can be used to display the current directory:

```cmd
cd
```

#### Search for `root.txt`

First, navigate to the root of the `C:` drive:

```cmd
cd C:\
```

Then search recursively for `root.txt`:

```cmd
dir /s root.txt
```

<img width="1056" height="824" alt="image" src="https://github.com/user-attachments/assets/60632ad3-564f-41ef-b808-3a12421a4c79" />

<img width="1042" height="814" alt="image" src="https://github.com/user-attachments/assets/5a154fc2-b1cb-4812-90cc-58f6b004eb70" />

<img width="1045" height="810" alt="image" src="https://github.com/user-attachments/assets/cad5bcbd-7d26-45c3-a00d-63204239f942" />

**Answer:**

```text
THM{Y0U_4R3_1337}
```

---

### 3–4.1. Another Way to Find `root.txt`

Another way to find the `root.txt` flag is by using the Windows graphical interface.

#### Method 1 – Using File Explorer

Open **Local Disk (C:)** and navigate to:

```text
C:\Users
```

<img width="847" height="421" alt="image" src="https://github.com/user-attachments/assets/74fa8a26-1721-472a-9f0f-d77d54fd9e5a" />

<img width="792" height="262" alt="image" src="https://github.com/user-attachments/assets/ccb0092e-28d0-42a2-8f78-06ab079a6b32" />


Open the **Administrator** folder.

<img width="787" height="312" alt="image" src="https://github.com/user-attachments/assets/d8ade7ef-e43c-42ef-aafd-d10db873b3a7" />

<img width="1896" height="354" alt="image" src="https://github.com/user-attachments/assets/c3f16794-132e-4cfe-b82a-6a7e71d68616" />


Use the Windows search box and search for:

```text
root.txt
```

<img width="1896" height="354" alt="image" src="https://github.com/user-attachments/assets/2a5557ec-6780-432a-ba55-d12776fc5d9f" />


Once `root.txt` appears in the search results, click and open the file.

---

#### Method 2 – Check the Administrator Desktop

Another option is to navigate directly to the Administrator's **Desktop** and open:

```text
root.txt
```

<img width="836" height="330" alt="image" src="https://github.com/user-attachments/assets/0ee5325b-1c05-4a53-b60b-0d841f62ff5d" />


---

#### Method 3 – Another Approach

A third way to locate and access the flag is shown below:

<img width="1000" alt="Alternative method for finding root.txt" src="YOUR_GITHUB_IMAGE_URL">

<img width="1224" height="78" alt="image" src="https://github.com/user-attachments/assets/a8619423-ef15-4cfa-b8b9-e94dfc87c8eb" />

**Answer:**

```text
THM{Y0U_4R3_1337}
```

---

# Q/A Summary

## Task 1 – Website Enumeration

<img width="1000" alt="Task 1 Q&A" src="YOUR_GITHUB_IMAGE_URL">

| Question | Answer |
|---|---|
| What port is for the web server? | `80` |
| What port is for remote desktop service? | `3389` |
| What is a possible password in one of the pages web crawlers check for? | `UmbracoIsTheBest!` |
| What CMS is the website using? | `Umbraco` |
| What is the domain of the website? | `Anthem.com` |
| What's the name of the Administrator? | `Solomon Grundy` |
| Can we find the email address of the Administrator? | `SG@anthem.com` |

---

## Task 2 – Spot the Flags

| Question | Answer |
|---|---|
| What is Flag 1? | `THM{L0L_WH0_US3S_M3T4}` |
| What is Flag 2? | `THM{G!T_G00D}` |
| What is Flag 3? | `THM{L0L_WH0_D15}` |
| What is Flag 4? | `THM{AN0TH3R_M3TA}` |

---

## Task 3 – Final Stage

| Question | Answer |
|---|---|
| Username | `SG` |
| Password | `UmbracoIsTheBest!` |
| What are the contents of `user.txt`? | `THM{N00T_NO0T}` |
| Can we spot the admin password? | `ChangeMeBaby1MoreTime` |
| What are the contents of `root.txt`? | `THM{Y0U_4R3_1337}` |

---

> **Room completed!**







