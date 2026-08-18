**Carnage**
Apply your analytical skills to analyze the malicious network traffic using Wireshark.

**Task 1**
_**Scenario**_

<img width="641" height="458" alt="image" src="https://github.com/user-attachments/assets/76ae4eca-9f28-4c66-88fa-07aabb56a14a" />

Eric Fischer from the Purchasing Department at Bartell Ltd has received an email from a known contact with a Word document attachment.  Upon opening the document, he accidentally clicked on "Enable Content."  The SOC Department immediately received an alert from the endpoint agent that Eric's workstation was making suspicious connections outbound. The pcap was retrieved from the network sensor and handed to you for analysis. 

Task: Investigate the packet capture and uncover the malicious activities. 

*Credit goes to Brad Duncan(opens in new tab) for capturing the traffic and sharing the pcap packet capture with InfoSec community. 

NOTE: DO NOT directly interact with any domains and IP addresses in this challenge. 

Deploy the machine attached to this task; it will be visible in the split-screen view once it is ready.

If you don't see a lab machine load, then click the Show Split View button.
<img width="1083" height="97" alt="image" src="https://github.com/user-attachments/assets/2c2de5ee-f4e0-42a9-aed5-461a4cee1e93" />

**Task 2**
_**Traffic Analysis**_

Are you ready for the journey?

Please, load the pcap file in your Analysis folder on the Desktop into Wireshark to answer the questions below.

**Q1 What was the date and time for the first HTTP connection to the malicious IP?  (answer format: yyyy-mm-dd hh:mm:ss)**

<img width="1910" height="651" alt="image" src="https://github.com/user-attachments/assets/04606224-5c8b-4d04-b45d-7f36b9215192" />
<img width="1912" height="693" alt="image" src="https://github.com/user-attachments/assets/4586f155-f539-44b4-a8e7-0f11d08b2d40" />

_2021-09-24 16:44:38_

**Q2 What is the name of the zip file that was downloaded?**

_documents.zip_

<img width="1919" height="479" alt="image" src="https://github.com/user-attachments/assets/ca2e0b53-a5ab-4552-9000-9292cee19277" />

<img width="1920" height="838" alt="image" src="https://github.com/user-attachments/assets/a5e92767-4bb4-4b19-8aea-47dce6baa692" />


**Q3 What was the domain hosting the malicious zip file?**

attirenepal.com

**Q4 Without downloading the file, what is the name of the file in the zip file?**

_chart-1530076591.xls_

<img width="1914" height="906" alt="image" src="https://github.com/user-attachments/assets/27cf76f9-24fd-443a-9a57-d773bde1bf46" />


**Q5 What is the name of the webserver of the malicious IP from which the zip file was downloaded?**

_LiteSpeed_

<img width="649" height="595" alt="image" src="https://github.com/user-attachments/assets/94043269-a95b-46bf-bb0e-b12950f48ed1" />

**Q6 What is the version of the webserver from the previous question?**

_PHP/7.2.34_

<img width="653" height="459" alt="image" src="https://github.com/user-attachments/assets/066dfe97-1fe6-47e5-8cea-7766f17cac09" />

**Q7 Malicious files were downloaded to the victim host from multiple domains. What were the three domains involved with this activity?**

<img width="472" height="175" alt="image" src="https://github.com/user-attachments/assets/e03c2806-93f4-4138-8053-df1789708b13" />

frame.time >= "Sep 24, 2021 16:45:11" && frame.time <= "Sep 24, 2021 16:45:30"

use TLS filter

finejewels.com.au. thietbiagt.com new.americold.com

7.  Hint asks you to check HTTPs traffic means look for SSL/TLS traffic between timeframe from 16:45:11 to 16:45:30. Go to Edit -> Preferences -> name resolution -> enable network IP addresses to resolve domain names

16:45:12 finejewels.com.au

16:45:25 thietbiagt.com

16:45:27 new.americold.com

<img width="700" height="141" alt="image" src="https://github.com/user-attachments/assets/84b02ac5-779e-49d9-bc3e-438dd4d4b94a" />

**Q8 Which certificate authority issued the SSL certificate to the first domain from the previous question?**

_goDaddy_

<img width="1918" height="675" alt="image" src="https://github.com/user-attachments/assets/a0ba05d6-4afa-4cc9-a96a-c5ae56124680" />

**Q9 What are the two IP addresses of the Cobalt Strike servers? Use VirusTotal (the Community tab) to confirm if IPs are identified as Cobalt Strike C2 servers. (answer format: enter the IP addresses in sequential order)**

The default port for Cobalt Strike is PORT 8080, look for IP addresses using this port

IP address 1- 185.106.96.158 at port 80

<img width="1892" height="923" alt="image" src="https://github.com/user-attachments/assets/1046a9a9-2c9e-46d3-aeb6-827bb1bebe9a" />

_185.106.96.158, 185.125.204.174_

**Q10 What is the Host header for the first Cobalt Strike IP address from the previous question?**

_ocsp.verisign.com_
<img width="1920" height="562" alt="image" src="https://github.com/user-attachments/assets/730f7b12-567f-41da-9bc9-c40dcfba4661" />

**Q11 What is the domain name for the first IP address of the Cobalt Strike server? You may use VirusTotal to confirm if it's the Cobalt Strike server (check the Community tab).**

using ip.addr == 185.106.96.158 find the domain name, also enable network IP addresses for name resolution (see point 7)

<img width="1310" height="77" alt="image" src="https://github.com/user-attachments/assets/25f5f5d6-abfd-4323-9d7e-770ffe5d35cc" />

Do same as mentioned in point 11

_survmeter.live_

<img width="952" height="153" alt="image" src="https://github.com/user-attachments/assets/e752c04b-2ab2-4b3d-9bdf-72de27f60ea1" />

**Q12 What is the domain name of the second Cobalt Strike server IP?  You may use VirusTotal to confirm if it's the Cobalt Strike server (check the Community tab).**

_securitybusinpuff.com_

13366 2021-09-24 16:57:37.126301 securitybusinpuff.com DESKTOP-IOJC6RB.goingfortune.com TCP 54

<img width="960" height="535" alt="image" src="https://github.com/user-attachments/assets/ab563a79-696a-44b2-a276-5c44faac6322" />

**Q13 What is the domain name of the post-infection traffic?**

http.request.method == POST

_maldivehost.net_

**Q14 What are the first eleven characters that the victim host sends out to the malicious domain involved in the post-infection traffic?** 

_zLIisQRWZI9_

<img width="886" height="229" alt="image" src="https://github.com/user-attachments/assets/228a4ede-212b-4eef-9eb9-f733cbeb39b1" />

**Q15 What was the length for the first packet sent out to the C2 server?**

<img width="1573" height="240" alt="image" src="https://github.com/user-attachments/assets/039392b0-7fcc-4e26-9e11-c709946f12fb" />

_281_

**Q16 What was the Server header for the malicious domain from the previous question?**
_Apache/2.4.49 (cPanel) OpenSSL/1.1.1l mod_bwlimited/1.4_

<img width="1723" height="473" alt="image" src="https://github.com/user-attachments/assets/de0b7a59-d03e-4490-acb6-7a18a12c33a0" />

**Q17 The malware used an API to check for the IP address of the victim’s machine. What was the date and time when the DNS query for the IP check domain occurred? (answer format: yyyy-mm-dd hh:mm:ss UTC)**

_2021-09-24 17:00:04_

<img width="957" height="583" alt="image" src="https://github.com/user-attachments/assets/cade6f5d-5172-427f-ab77-e5267478f3fc" />

**Q18 What was the domain in the DNS query from the previous question?**
_api.ipify.org_

**Q19 Looks like there was some malicious spam (malspam) activity going on. What was the first MAIL FROM address observed in the traffic?**
Apply smtp filter or we can also use frame contains “MAIL FROM”
_farshin@mailfa.com_

<img width="959" height="664" alt="image" src="https://github.com/user-attachments/assets/4b84dfe9-ed55-4286-b59a-d213bdfb6b81" />

**Q20 How many packets were observed for the SMTP traffic?**

<img width="1803" height="503" alt="image" src="https://github.com/user-attachments/assets/d5bb7b8d-e293-4cb7-8c87-afede3e9940d" />

_1439_

 
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/d1feed66-251e-4d4e-ad48-8184a6cd83cc" />
