# Password Cracking & Hash Analysis

## Overview

Hands-on password security lab focused on **hash identification, password cracking, wordlist attacks, and password strength analysis** in an authorized environment.

## Objectives

* Identify common hash types
* Perform dictionary-based password cracking
* Analyze password strength
* Work with custom wordlists
* Understand mutation/rule-based attacks
* Understand bcrypt resistance to brute-force attacks


## Tools

### Hashcat

Password recovery and hash auditing.

**Official:** https://hashcat.net/hashcat/

# MD5
hashcat -m 0 -a 0 hashes.txt rockyou.txt

# SHA-1
hashcat -m 100 -a 0 hashes.txt rockyou.txt

# SHA-256
hashcat -m 1400 -a 0 hashes.txt rockyou.txt

# bcrypt
hashcat -m 3200 -a 0 hashes.txt rockyou.txt
```
**### John the Ripper**

Password auditing and dictionary attacks.

**Official:** https://www.openwall.com/john/

john --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt

john --show hashes.txt
```
### CrackStation

Hash lookup and password recovery.

https://crackstation.net/

### Hashes.com


Known-hash lookup.

https://hashes.com/en/decrypt/hash

### Hash Analyzer

Identify possible hash algorithms.

https://www.tunnelsup.com/hash-analyzer/

## Wordlists
<img width="845" height="646" alt="image" src="https://github.com/user-attachments/assets/6af2a8b1-a10f-4de0-8715-fe1a0180ad7b" />

<img width="837" height="646" alt="image" src="https://github.com/user-attachments/assets/05843c20-8aa2-4c36-bbf9-1fe670efdda6" />

Kali Linux wordlists:

ls /usr/share/wordlists/
```

**RockYou:**

<img width="684" height="541" alt="image" src="https://github.com/user-attachments/assets/499fe652-bbad-4b18-8f45-9ddc5f974a21" />


/usr/share/wordlists/rockyou.txt
```

Create a custom wordlist:

nano custom-wordlist.txt
```

**Remove duplicates:**

sort -u custom-wordlist.txt > unique-wordlist.txt
```

## 🔄 Rule-Based Attack

Hashcat rules can generate password variations from an existing wordlist.


hashcat -m 0 -a 0 hashes.txt rockyou.txt -r rules/best64.rule
```

Example mutations:

```text
password
Password
Password1
Password123
P@ssword
```

## bcrypt

bcrypt is designed specifically for password storage and is intentionally computationally expensive.

Instead of decrypting bcrypt, candidate passwords are hashed and compared against the stored hash.

hashcat -m 3200 -a 0 bcrypt.txt rockyou.txt
```

## Analyst Takeaways

* Hash identification determines the appropriate analysis method.
* Weak passwords remain vulnerable to dictionary and rule-based attacks.
* Custom wordlists can improve password-auditing efficiency.
* bcrypt significantly increases the computational cost of password guessing.
* Password length, uniqueness, and MFA reduce credential-based risk.

**Crack the hash**
Cracking hashes challenges

https://tryhackme.com/room/crackthehash
<img width="1510" height="292" alt="image" src="https://github.com/user-attachments/assets/8ebfdec4-d74c-4986-9db1-ef4b40aa3503" />

**Task 1 Level 1**
Can you complete the level 1 tasks by cracking the hashes?

Answer the questions below
_**48bb6e862e54f2a795ffc4e541caed4d**_

<img width="1919" height="763" alt="image" src="https://github.com/user-attachments/assets/1ebf224d-603a-4394-a9dc-20bbde216210" />

**easy**

**Q2 _CBFDAC6008F9CAB4083784CBD1874F76618D2A97 **_

<img width="1918" height="819" alt="image" src="https://github.com/user-attachments/assets/f8aec15c-4d0e-4db0-abdc-9a1cd3dcac6c" />

**password123**

**Q3 _1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032**_

<img width="1919" height="813" alt="image" src="https://github.com/user-attachments/assets/97a873be-da85-434d-8ee8-2fcc4988e34b" />

**letmein**

_**Q4 $2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom**_

<img width="1912" height="925" alt="image" src="https://github.com/user-attachments/assets/edd9d039-1fe7-4286-b877-d068a4a3a90d" />

We try to crack it with Crackstation, but the result is unsuccessful.

Let's try to crack it using hashcat with Kali Linux. m-3200 for bcrypt

Let's try to crack it with the →hashcat -m 3200 hash.txt rockyou.txt command.

<img width="1911" height="1042" alt="image" src="https://github.com/user-attachments/assets/3c28d720-46ad-47db-a6ce-b6d07aee7f59" />

https://hashes.com/en/decrypt/hash

$2y1212Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom:**bleh**

<img width="1913" height="985" alt="image" src="https://github.com/user-attachments/assets/ce1c8888-63af-47c7-8f7c-620e176fe5ae" />

_**Q5 279412f945939ba78ce0758d3fd83daa**_

<img width="1920" height="852" alt="image" src="https://github.com/user-attachments/assets/26149ce8-47c7-47f1-ab01-91c8cbb2969d" />

_**Eternity22**_

<img width="1920" height="656" alt="image" src="https://github.com/user-attachments/assets/45934143-a498-4758-8953-9e074f2892b0" />


**Task 2 Level 2**

This task increases the difficulty. All of the answers will be in the classic rock you(opens in new tab) password list.

You might have to start using hashcat here and not online tools. It might also be handy to look at some example hashes on hashcats page(opens in new tab).

Answer the questions below
**Hash: F09EDCB1FCEFC6DFB23DC3505A882655FF77375ED8AA2D1C13F640FCCC2D0C85**

<img width="1920" height="1007" alt="image" src="https://github.com/user-attachments/assets/256af5b9-73a5-4c33-b799-4dd16ce7feba" />

_**paule**_

_**Q2 Hash: 1DFECA0C002AE40B8619ECF94819CC1B**_

<img width="1920" height="833" alt="image" src="https://github.com/user-attachments/assets/fe49e7e2-cb55-4aa4-a6b2-374a49a48b76" />

**n63umy8lkf4i**

**Q3 Hash: $6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02.  Salt: aReallyHardSalt**

<img width="1920" height="846" alt="image" src="https://github.com/user-attachments/assets/8b98c598-5abc-49da-9f5a-60963961713c" />

_**waka99**_

<img width="1919" height="879" alt="image" src="https://github.com/user-attachments/assets/80dbaa49-3c8b-41a5-9305-5eecd72624c8" />

**Q4 Hash: e5d8870e5bdd26602cab8dbe07a942c8669e56d6  Salt: tryhackme **

<img width="1918" height="869" alt="image" src="https://github.com/user-attachments/assets/ad40ee39-6c5d-4961-a1df-7eb57ffca133" />

481616481616




