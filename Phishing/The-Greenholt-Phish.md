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

Q1 What is the Transfer Reference Number listed in the email's Subject?
<img width="1224" height="398" alt="image" src="https://github.com/user-attachments/assets/ce82db55-9f37-4aac-b672-bec617ce859f" />


Transfer Reference Number:(09674321)

Q2 Who is the email from? Or who is the email sender?

image
Q3 Who is the email from?
image

info@mutawamarine.com

Q4 What email address will receive a reply to this email?
image

info.mutawamarine@mail.com

Q5 What is the Originating IP?
image
ESMTP, Extended Simple Mail Transfer Protocol

image
Q6 Who is the owner of the Originating IP? (Do not include the "." in your answer.)
image

image
https://whois.domaintools.com/hostwindsdns.com

Q7 What is the SPF record for the Return-Path domain?

image domain name= mutawamarine.com image
Q8 What is the DMARC record for the Return-Path domain?
image

v=DMARC1; p=quarantine; fo=1

Q9 What is the name of the attachment?
image

SWT_#09674321____PDF__.CAB

Q10 What is the SHA256 hash of the file attachment?
image

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

Q11 What is the attachments file size? (Don't forget to add "KB" to your answer, NUM KB)
image

400.26 KB (409868 bytes)
2e91c533615a9bb8929ac4bb76707b2444597ce063d84a4b33525e25074fff3f

Q12 What is the actual file extension of the attachment?
image

rar
