Phishing is a social engineering attack. It is a campaign done by attackers to lure the victim to click a malicious URL or have them download/execute a malicious file.

What is the weakest link in the cyber world?
human!
Because people sometimes act emotionally, they can be easily hacked with a simple trick.

What is the purpose of Attacker?

The purpose of the attacker in a phishing attack is typically to gain unauthorized access to sensitive information, such as login credentials, financial details, or personal information, for malicious purposes.

A Sales Executive at Greenholt PLC received an email that he didn't expect to receive from a customer. He claims that the customer never uses generic greetings such as "Good day" and didn't expect any amount of money to be transferred to his account. The email also contains an attachment that he never requested.(Very Suspicious) He forwarded the email to the SOC (Security Operations Center) department for further investigation.

Investigate(Static / Dynamic) the email sample to determine if it is legitimate.

Tip: Open the EML file with Thunderbird. To do so, right-click on the challenge.eml file and select Open With Other Application. From there, scroll down to select Thunderbird Mail and click Open. It may take a few moments to open the application. You will then see the email and its contents appear in the app.

How do you work on a Phishing email?
What is static analysis? and What is dynamic analysis?
If we do not use tools, it will be static analysis. If we use the tool, it will be dynamic analysis.

Static analysis:

Is the mail from public domain (like Gmail, yahoo, etc.)

Is the domain name misspelled (like amazone.com)?

Is the email poorly written (grammar mistakes, incorrect use of words etc.)? Attackers do this to bypass standard filtering.

Does the email create a sense of urgency?

Dynamic analysis:
Submit the URL to www.urlvoid.com and check the reputation.

Check the domain in WHOIS lookup to identify the IP address of the domain.
Check the reputation of the IP at www.ipvoid.com

Paste the Internet Header to www.mxtoolbox.com (Analyze
Headers)
Check for DMARC compliance
Check for SPF Alignment and Authentication
Check the DKIM Alignment and Authentication

Check the reputation of IP addresses and domain names that appear in the header information

SPF (Sender Policy Framework), DKIM (DomainKeys Identified Mail), and DMARC (Domain-based Message Authentication, Reporting, and Conformance) are all email security protocols used to enhance the security and authentication of email communication.

To investigate an email header using MXToolbox, you typically follow these steps:

Access MXToolbox:

Go to the MXToolbox website (https://mxtoolbox.com/).

Access the Email Header Analyzer:

MXToolbox provides various tools for analyzing email-related information. Look for the "Email Header Analyzer" tool. You may find it in the "Email Tools" section or by searching for it in the search bar.

Copy the email header:

In your email client (e.g., Gmail, Outlook, eM Client), open the email for which you want to analyze the header.

Find the option to view the full email header. This option is usually labeled as "View Source", "Show Original", or "View Header".

Copy the entire email header. This is typically a long string of text.

Paste the header into MXToolbox:

Return to MXToolbox and navigate to the Email Header Analyzer tool.

Paste the copied email header into the designated field provided by MXToolbox.

Analyze the header:

Once you've pasted the email header, MXToolbox will analyze it and provide you with detailed information about the email's source, routing, sender information, and any potential issues or anomalies detected.

Interpret the results:

Review the information provided by MXToolbox to understand the email's origin and any potential red flags or suspicious elements. Look for details such as the sender's IP address, the mail servers involved in delivering the email, and any authentication mechanisms (SPF, DKIM, DMARC) used.

Take action if necessary:

Depending on the results of the analysis, you may need to take further action, such as reporting the email as spam or phishing, blocking the sender's domain or IP address, or implementing additional security measures.

By following these steps, you can effectively investigate email headers using MXToolbox's Email Header Analyzer tool.

To investigate an email header using eM Client, follow these steps:

Open the email: Launch eM Client and open the email for which you want to view the header.

View the email source: Look for an option to view the email source or message details. This option is usually found in the menu or toolbar when you have the email open. It may be labeled as "View", "Options", or "Message Details".

Find the header: Once you're viewing the email source or message details, you should see the email header. This section typically contains detailed information about the email's routing, sender, recipient, timestamps, and more.

Analyze the header: Look for relevant information in the header, such as the sender's IP address, the path the email took through various servers, and any other details that might help you investigate the email's origin or legitimacy.

Use external tools if needed: If eM Client's built-in tools don't provide enough information or if you need to perform a deeper analysis, you can copy the email header and use online email header analysis tools or other email forensic tools to gather more insights.

Remember that email headers can be complex, so it may take some time to understand all the information they contain. If you're investigating a suspicious email, be cautious and consider seeking advice from IT professionals or security experts.

Phishing Resources:

https://hybrid-analysis.com/
https://mxtoolbox.com/
https://phishtank.com/
https://any.run/
https://urlscan.io/
https://checkphish.ai/
https://www.urlvoid.com/
https://www.ipvoid.com/
https://www.abuseipdb.com/
https://exchange.xforce.ibmcloud.com/
https://mailheader.org/
https://malshare.com/
https://www.shodan.io/
https://www.thunderbird.net/en-GB/

**Q1 What is the Transfer Reference Number listed in the email's Subject?**
<img width="1224" height="398" alt="image" src="https://github.com/user-attachments/assets/ce82db55-9f37-4aac-b672-bec617ce859f" />


Transfer Reference Number:(09674321)

**Q2 Who is the email from? Or who is the email sender?**

<img width="427" height="41" alt="image" src="https://github.com/user-attachments/assets/938c670c-d869-40b8-99be-4b208a6d73bf" />

**Q3 Who is the email from?**
<img width="447" height="100" alt="image" src="https://github.com/user-attachments/assets/e852301f-75b2-475f-a83d-b6b1950d2d5e" />


info@mutawamarine.com

**Q4 What email address will receive a reply to this email?**
<img width="817" height="241" alt="image" src="https://github.com/user-attachments/assets/cc0cdcfd-995d-4d9a-b05e-6a5112bc67a7" />


info.mutawamarine@mail.com

**Q5 What is the Originating IP?**
Identify the First "Received" Header: Email headers usually contain multiple "Received" lines, which track the servers that handled the email as it was transmitted.The first "Received" line typically indicates the originating server.

Locate the IP Address: In the first "Received" line, look for the IP address following the from keyword. This is the IP address from which the email was originally sent.

Based on the provided headers:

Received from hwsrv-737338.hostwindsdns.com ([192.119.71.157]:51810 helo=mutawamarine.com): This line indicates that the email was originally sent from the server hwsrv-737338.hostwindsdns.com with the IP address 192.119.71.157.

Therefore, the Originating IP is 192.119.71.157.

OR

"([192.119.71.157]:51810)": This part shows the IP address of the email-sending server. The IP address of the server that first sent the email, i.e., hwsrv-737338.hostwindsdns.com, is 192.119.71.157. This is the originating IP address.

In email headers, the "originating IP" typically refers to the IP address of the first server that sent the email.
<img width="586" height="105" alt="image" src="https://github.com/user-attachments/assets/32112a56-13bb-4dfa-88f7-7b06cb109d50" />

ESMTP, Extended Simple Mail Transfer Protocol
<img width="1337" height="925" alt="image" src="https://github.com/user-attachments/assets/1b2e358f-2ee3-4d97-b6b5-67d7a87fc54e" />

**Q6 Who is the owner of the Originating IP? (Do not include the "." in your answer.)**
<img width="1903" height="1023" alt="image" src="https://github.com/user-attachments/assets/1ccc539f-950b-416e-bf3b-843ef9763658" />


<img width="1146" height="786" alt="image" src="https://github.com/user-attachments/assets/a9b2cd6d-5894-4f56-b5cd-2f0ceb9b14da" />

https://whois.domaintools.com/hostwindsdns.com

**Q7 What is the SPF record for the Return-Path domain?**

<img width="821" height="135" alt="image" src="https://github.com/user-attachments/assets/15072fdc-c392-48c5-b92b-6b6d0233f7f8" />

domain name= mutawamarine.com 

<img width="1529" height="665" alt="image" src="https://github.com/user-attachments/assets/a7d4a3b5-8e95-4f40-89ed-5c415ecf1d73" />

**Q8 What is the DMARC record for the Return-Path domain?**
<img width="706" height="388" alt="image" src="https://github.com/user-attachments/assets/ceddab9d-9164-4910-a8aa-4daeb24f233d" />


v=DMARC1; p=quarantine; fo=1

**Q9 What is the name of the attachment?**
<img width="857" height="755" alt="image" src="https://github.com/user-attachments/assets/333bf035-2e73-42bd-a77c-390f7c43942c" />


SWT_#09674321____PDF__.CAB

**Q10 What is the SHA256 hash of the file attachment?**
<img width="739" height="244" alt="image" src="https://github.com/user-attachments/assets/ba9f5025-c42d-454d-a995-c21cd9e6ba6a" />


ubuntu@ip-10-10-171-105:/Desktop$ ls
SWT_#09674321____PDF__.CAB Tools challenge.eml
ubuntu@ip-10-10-171-105:/Desktop$ sha256sum SWT_#09674321____PDF__.CAB
2e91c533615a9bb8929ac4bb76707b2444597ce063d84a4b33525e25074fff3f SWT_#09674321____PDF__.CAB
ubuntu@ip-10-10-171-105:~/Desktop$

For PowerShell:

The command get-filehash will generate a SHA256 hash. If we want to obtain MD5 or SHA1 values, we need to add the '-algorithm' flag to specify which hashes we desire. Using:

get-filehash -algorithm md5
get-filehash -algorithm SHA1

we can obtain the MD5 hash value, and the same method can be applied for SHA1.

For Linux:

SHA256, MD5, and SHA1 hashes respectively:

sha256sum
md5sum
sha1sum

**Q11 What is the attachments file size? (Don't forget to add "KB" to your answer, NUM KB)**
<img width="1657" height="866" alt="image" src="https://github.com/user-attachments/assets/8cef1579-1285-40ff-ae10-17d5e798b7b3" />


400.26 KB (409868 bytes)
2e91c533615a9bb8929ac4bb76707b2444597ce063d84a4b33525e25074fff3f

**Q12 What is the actual file extension of the attachment?**
<img width="1706" height="413" alt="image" src="https://github.com/user-attachments/assets/c2a09c56-8eaa-4faa-aef4-9e5290c97719" />


rar
