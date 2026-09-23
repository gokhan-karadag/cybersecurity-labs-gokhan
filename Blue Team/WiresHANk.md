<img width="711" height="712" alt="image" src="https://github.com/user-attachments/assets/decbe46d-0133-4cc6-9af6-fbd166c0a766" />
Wireshark

# Wireshark: From Packet Capture to Traffic Analysis

## 1. What Is Wireshark?

**Wireshark** is an open-source packet analyzer. It captures and displays network packets so you can troubleshoot connections, study protocols, and investigate suspicious activity.

A **PCAP** file stores packets recorded during a capture. You can save a capture and reopen it later for analysis.

Wireshark can help you:

* Identify which hosts are communicating and which protocols they use.
* Find systems sending or receiving large amounts of data.
* Investigate connection errors and unusual traffic patterns.
* Read application data when the traffic is unencrypted.
* Examine file transfers when the relevant packets were captured and the content is accessible.

Wireshark shows traffic available to the selected network interface. **Promiscuous mode does not guarantee visibility into every packet on the network.**

## 2. Capture and Save Traffic

1. Open Wireshark and select the appropriate network interface.
2. Click **Start Capturing Packets**.
3. Generate test traffic. For example, run `ping` to a reachable host.
4. Enter `icmp` in the display filter bar to find the ping packets.
5. Stop the capture and select **File → Save As** to save it.

## 3. Apply Display Filters

A **display filter** controls which captured packets appear in the packet list. It does not delete packets from the capture.

| Goal                                                  | Display filter                  |   |                    |
| ----------------------------------------------------- | ------------------------------- | - | ------------------ |
| Show UDP packets                                      | `udp`                           |   |                    |
| Show HTTP requests                                    | `http.request`                  |   |                    |
| Show packets with source or destination TCP port 80   | `tcp.port == 80`                |   |                    |
| Show TCP packets with a window value of at least 8000 | `tcp.window_size_value >= 8000` |   |                    |
| Show TCP packets going to `192.168.1.7`               | `ip.dst == 192.168.1.7 && tcp`  |   |                    |
| Show NTP traffic or UDP traffic on port 20000         | `ntp                            |   | udp.port == 20000` |
| Exclude FTP packets                                   | `not ftp`                       |   |                    |

## 4. How to Write Display Filters

A filter that compares a field with a value usually follows this pattern:

```text
field operator value
```

For example:

```text
ip.addr == 192.168.1.10
```

Here, `ip.addr` is the field, `==` means “equals,” and `192.168.1.10` is the value. The filter shows packets where this address is the source **or** destination.

You can also enter a protocol or field name by itself, such as `udp` or `http.request`, to show packets where it is present.

### Build a Filter Step by Step

Suppose you want to find HTTP traffic **sent by** `192.168.1.10`:

1. Start with HTTP traffic: `http`
2. Specify the source IP: `ip.src == 192.168.1.10`
3. Join the conditions with `&&`:

```text
http && ip.src == 192.168.1.10
```

Both conditions must match. To find HTTP **requests** from that host specifically, use:

```text
http.request && ip.src == 192.168.1.10
```

### Common Fields

| Field         | What it matches                | Example                   |
| ------------- | ------------------------------ | ------------------------- |
| `ip.addr`     | Source or destination IP       | `ip.addr == 192.168.1.10` |
| `ip.src`      | Source IP                      | `ip.src == 192.168.1.10`  |
| `ip.dst`      | Destination IP                 | `ip.dst == 8.8.8.8`       |
| `tcp.port`    | Source or destination TCP port | `tcp.port == 443`         |
| `tcp.dstport` | Destination TCP port           | `tcp.dstport == 443`      |
| `udp.port`    | Source or destination UDP port | `udp.port == 53`          |

### Common Operators

| Operator | Meaning                           | Example                          |
| -------- | --------------------------------- | -------------------------------- |
| `==`     | Equals                            | `tcp.port == 443`                |
| `!=`     | Does not equal                    | `ip.src != 192.168.1.10`         |
| `>`      | Greater than                      | `frame.len > 1000`               |
| `>=`     | Greater than or equal to          | `tcp.window_size_value >= 8000`  |
| `&&`     | Both conditions must match        | `tcp && ip.addr == 192.168.1.10` |
| `\|\|`   | At least one condition must match | `dns \|\| http`                  |
| `!`      | Excludes a condition              | `!arp`                           |

### Practice Filters

| Investigation question                                 | Display filter                                              |
| ------------------------------------------------------ | ----------------------------------------------------------- |
| Which packets involve this host?                       | `ip.addr == 192.168.1.10`                                   |
| What traffic did this host send?                       | `ip.src == 192.168.1.10`                                    |
| Did this host send TCP traffic to port 443?            | `ip.src == 192.168.1.10 && tcp.dstport == 443`              |
| Show DNS queries                                       | `dns.flags.response == 0`                                   |
| Show HTTP requests                                     | `http.request`                                              |
| Show TCP reset packets                                 | `tcp.flags.reset == 1`                                      |
| Show packets larger than 1,000 bytes                   | `frame.len > 1000`                                          |
| Show HTTP requests for a host name containing “gokhan” | `http.request && http.host contains "gokhan"`               |
| Show DNS queries for a name containing “gokhan”        | `dns.flags.response == 0 && dns.qry.name contains "gokhan"` |

**Tip:** If you do not know a field name, select a packet and find the field in **Packet Details**. Right-click it and choose **Apply as Filter → Selected**. Wireshark will create the filter for you.
<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/32f5fa05-273b-438c-bcb7-4ff58bd9e02c" />



## 5. Follow a Stream

A conversation may span many packets. **Follow Stream** presents its communication together, making requests and responses easier to read in order.

Right-click a relevant packet and select **Follow → TCP Stream** or another applicable stream type. Wireshark will also apply a filter for that stream.

This is useful for examining unencrypted HTTP traffic. With encrypted TLS traffic, the application content will not be readable unless you have what is needed to decrypt it.

## 6. Add Custom Columns

If you check a packet field often, add it to the main packet list:

**Packet Details → right-click the field → Apply as Column**

For example, a custom TLS field column can help you spot packets with a particular value.

## 7. Review Capture Statistics

The **Statistics** menu gives you an overview of the capture before you inspect packets individually.

| Window                 | What it shows                                | When to use it                                        |
| ---------------------- | -------------------------------------------- | ----------------------------------------------------- |
| **Protocol Hierarchy** | Packet and byte distribution by protocol     | Find unexpected or uncommon protocols                 |
| **Conversations**      | Traffic exchanged between pairs of endpoints | See who communicated and how much data they exchanged |
| **Endpoints**          | Traffic sent and received by each endpoint   | Find systems with high traffic volume                 |

### Protocol Hierarchy

Check which protocols appear in the capture. For example, FTP traffic on a network that does not normally use FTP may be worth investigating. You can right-click a protocol to apply a filter.

### Conversations

Check communication between two endpoints, including ports, packet counts, and byte counts. A large transfer from an internal host to an unfamiliar external IP address may warrant investigation. **Traffic volume alone does not prove data exfiltration**; the transfer could be legitimate.

### Endpoints

Sort endpoints by sent or received bytes to identify active systems. A host receiving a lot of data may be downloading a file; one sending a lot may be uploading or backing up data. Inspect the related packets to understand what happened.

## 8. Quick Analysis Workflow

1. Open **Protocol Hierarchy** to see which protocols are present.
2. Check **Endpoints** for systems with unusual or high traffic volume.
3. Use **Conversations** to see which systems they contacted.
4. Apply **Display Filters** to isolate relevant packets.
5. Use **Follow Stream** to examine the communication in context.

**Key takeaway:** Wireshark’s features help you find and organize evidence. Deciding whether traffic is normal or suspicious requires context, networking knowledge, and practice.

<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/898fac4d-6e1a-4658-b3d3-4c2e004efeab" />


# Task 2: Wireshark `ctf.pcap` Challenge

### Q1. How many packets in total did you find?

#### Method 1: Capture File Properties

In Wireshark, go to **Statistics → Capture File Properties**. Under **Statistics**, find the **Packets** row. The **Captured** value shows the total number of packets: **28,875**.

Capture File Properties also provides the capture’s start and end times, duration, file size, and average packet rate. It is a useful place to get a quick overview before analyzing individual packets.

<img width="1918" height="1041" alt="image" src="https://github.com/user-attachments/assets/2daef788-79e4-4b9a-953e-f99cd9f0b647" />


### Q2. What is the packet number of the `geolocation.onetrust.com` query?

**Hint:** When a host looks up a domain name, check the DNS query and response packets. The **Info** column can help you tell them apart.

**How to find it:**

1. In Wireshark, select **Edit → Find Packet** or press **Ctrl + F**.
2. Set the search type to **String**. The domain name is text, so this option lets you search for its characters rather than a packet number or hexadecimal value.
3. Search for `geolocation.onetrust.com`.
4. Check the matching packet number. Confirm in the **Info** column that the packet is a DNS **query**, rather than its response.
<img width="1917" height="1147" alt="image" src="https://github.com/user-attachments/assets/ffc9132e-d1e8-41db-a067-f65fb4269f3b" />

**Answer:** `736`

### Q3. What are the first three different protocols?

**Hint:** Read the **Protocol** column from the top of the packet list. If the same protocol appears in several consecutive packets, count it only once.

**How to find it:**

1. Open `ctf.pcap` in Wireshark.
2. Make sure the packet list is ordered by packet number, starting with packet **1**.
3. Read the **Protocol** column from top to bottom and write down each new protocol you encounter.
4. The first three different protocols are **DNS**, **TCP**, and **FTP**.

<img width="1913" height="1031" alt="image" src="https://github.com/user-attachments/assets/40c95399-a80d-42d9-b3ad-c979a9848837" />
**Answer:** `DNS, TCP, FTP`


### Q4. The threat actor tried to access the File Server over FTP using the `bettywriter` account. What password was attempted?

**Hint:** FTP login commands may be visible in an unencrypted capture. Look for `USER bettywriter` and the related `PASS` command. A password attempt does not mean the login succeeded.

#### Method 1: Manual Search

1. Open `ctf.pcap` in Wireshark.
2. Enter `ftp` in the display filter bar.
3. In the **Info** column, locate an FTP `USER` command.
4. Find its corresponding `PASS` command in the same FTP conversation and read the attempted password.

<img width="1920" height="1145" alt="image" src="https://github.com/user-attachments/assets/397d04a3-2912-46fe-a3c4-6536f245a022" />







#### Method 2: Follow the TCP Stream

1. Apply the display filter `ftp`.
2. Right-click a packet from the `bettywriter` login attempt.
3. Select **Follow → TCP Stream**.
4. Find the `USER` and `PASS` commands in the stream.
   
<img width="1914" height="666" alt="image" src="https://github.com/user-attachments/assets/95cfc8bf-5e67-4a60-bb06-60a61cd55ca9" />
<img width="1269" height="531" alt="image" src="https://github.com/user-attachments/assets/96ecd286-5404-4060-91d1-a77235269942" />

#### Method 3: Use the Credentials Window

1. Select **Tools → Credentials**.
2. Find the FTP entry for `bettywriter`.
3. Select its associated password packet and inspect the `PASS` value.

<img width="640" height="166" alt="image" src="https://github.com/user-attachments/assets/0fea3bfe-d5a3-4bc5-8070-ce35b7f7c1b4" />
<img width="1918" height="735" alt="image" src="https://github.com/user-attachments/assets/d36b6eb7-0172-4b82-9d7d-5c6b19407134" />

**Answer:** `123writer`


### Q5. What are the two attackers' usernames and passwords?

> 💡 **Hint:** Use the same approach as Q4. Match each FTP `USER` command with its corresponding `PASS` command in the same conversation.

**How to find them:**

1. Open `ctf.pcap` and apply the display filter `ftp`.
2. Inspect the **Info** column or select **Tools → Credentials** to find the two usernames.
3. Select each associated password packet and read its `PASS` value.
4. Pair each username with the password from its own FTP conversation.

<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/f918b369-639f-4e9d-abde-fd5430eb8962" /> 
**Answer:**

- `bettywriter` — `123writer`
- `skyking` — `han12345`

**Answer format:** `bettywriter, 123writer, skyking, han12345`

### Q6. When did the first DNS query for `ftp.cdc.gov` from `10.0.2.15` occur?

> 💡 **Hint:** Filter for DNS traffic, then find the earliest **query** for the domain. Check that the source IP is `10.0.2.15`. The **Time** column may show time relative to the start of the capture, so check the packet’s absolute arrival time for the date and clock time.

**How to find it:**

1. Open `ctf.pcap` in Wireshark and apply the display filter `dns`.
2. Press **Ctrl + F**, set the search type to **String**, and search for `ftp.cdc.gov`.
3. Select the earliest matching DNS **query**. In this capture, it is packet **1**.
4. Confirm that its source IP is `10.0.2.15`.
5. Open the packet’s **Frame** details and read its absolute arrival time.
6. Round the fractional seconds to three decimal places, as required by the answer format.

<img width="1922" height="766" alt="image" src="https://github.com/user-attachments/assets/21ad1913-cae3-4885-ac31-d73fbd6613f5" />


**Answer:** `2024-07-15 15:18:21.676`

### Q7. When did the final QUIC packet communicating with `10.0.2.15` occur?

**Hint:** Filter for QUIC traffic involving `10.0.2.15`, then inspect the last matching packet.

**How to find it:**

1. In Wireshark, apply the filter `quic && ip.addr == 10.0.2.15`.
2. Go to the bottom of the filtered packet list.
3. The final matching packet is **No. 28875**.
4. **Protocol:** QUIC.
5. **Destination IP:** `10.0.2.15`.
6. **Timestamp:** `2024-07-15 15:21:44.93246791`.
7. According to the required format, use six digits after the decimal point: **`2024-07-15 15:21:44.932467`**.
<img width="1920" height="1147" alt="image" src="https://github.com/user-attachments/assets/cecaf628-d44d-413d-96af-111be53c5e5c" />

**Answer:** `2024-07-15 15:21:44.932467`

### How to Change the Time Format in Wireshark

Wireshark may initially show the time elapsed since the capture began. To see the date and clock time directly in the **Time** column:

1. Open **View → Time Display Format**.
2. Select **Date and Time of Day**.
3. If you need more or fewer digits after the decimal point, adjust **View → Time Display Format → Time Resolution**.

**Tip:** Check the time zone before reporting a timestamp. Wireshark can display local time or UTC depending on the selected format.
<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/ade4c5f6-b3f5-44cb-adc9-9b1568abb3e6" />

# Challenge 1: 

### Q1. What was the first password attempted for the `Teton` account?

**Hint:** FTP login attempts contain `USER` and `PASS` commands. Find the first `USER` command for the account, then inspect the related `PASS` command.

**How to find it:**

1. Open the challenge PCAP in Wireshark.
2. Apply the display filter `ftp`.
3. Find the first `USER Teton` entry in the **Info** column.
4. Read the following `PASS` command in the same FTP conversation.

**Answer:** `Zeliha2025#Pcap`

### Q2. What is the second username?

**Hint:** Continue reading the FTP `USER` commands in packet order. Count each new login attempt.

**How to find it:**

1. Keep the display filter `ftp` applied.
2. Locate the second `USER` command in the **Info** column.
3. Read the username that follows `USER`.

**Answer:** `ZelihaHacks4Fun`

### Q3. What are the third and fourth usernames and passwords?

**Hint:** Match each `USER` command with the related `PASS` command in the same FTP conversation.

**How to find them:**

1. Continue through the FTP packets in order.
2. Locate the third and fourth `USER` commands.
3. Read the corresponding `PASS` command for each username.
4. Keep each username paired with its own attempted password.

**Answer:**

- `Seeker` — `PeakABoo_It'sC3`
- `Rich_Hunter` — `CTF#2025!`

**Answer format:** `Seeker:PeakABoo_It'sC3, Rich_Hunter:CTF#2025!`

### Q4. What is the last packet number in the capture?

**Hint:** Go to the end of the unfiltered packet list and check the **No.** column.

**How to find it:**

1. Clear the display filter so all packets are visible.
2. Go to the bottom of the packet list.
3. Read the **No.** value of the final packet.

**Answer shown in the provided material:** `134`

> **Verification needed:** The screenshot highlights packet `132`, not `134`. Check the final packet directly in Wireshark before submitting or publishing this answer.

<img width="1774" height="887" alt="image" src="https://github.com/user-attachments/assets/8595ce1a-155c-43e4-8b82-0c9c94ebbc74" />


## Challenge 2

### How to Merge PCAP Files in Wireshark

Merging capture files lets you analyze traffic from multiple PCAPs in one chronological view.

1. Open the first capture file in Wireshark.
2. Select **File → Merge** and choose the second capture file.
3. Select **Chronologically** to arrange packets by timestamp, then complete the merge.
4. To add another file, repeat **File → Merge**.
5. Select **File → Save As** to save the combined capture under a new name.

> **Tip:** If Wireshark asks you to save packets before merging, save the current capture and continue.

<img width="1672" height="941" alt="image" src="https://github.com/user-attachments/assets/7858b76f-244e-4c23-898e-9f2aecf7212f" />
