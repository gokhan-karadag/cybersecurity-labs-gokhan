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

## Rule-Based Attack

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

```

## 🔎 Analyst Takeaways

- Hash identification determines the appropriate analysis method.
- Weak passwords remain vulnerable to dictionary and rule-based attacks.
- Custom wordlists can improve password-auditing efficiency.
- bcrypt significantly increases the computational cost of password guessing.
- Password length, uniqueness, and MFA reduce credential-based risk.

---

# 🔐 Crack the Hash

Hands-on hash identification and password auditing challenge.

**TryHackMe Lab:**  
https://tryhackme.com/room/crackthehash

<img width="1510" height="292" alt="Crack the Hash Challenge" src="https://github.com/user-attachments/assets/8ebfdec4-d74c-4986-9db1-ef4b40aa3503" />

---

## Task 1 — Level 1

Complete the Level 1 challenges by identifying and cracking the provided password hashes.

### Q1

**Hash:**

`48bb6e862e54f2a795ffc4e541caed4d`

<img width="1359" height="551" alt="Hash Analysis Q1" src="https://github.com/user-attachments/assets/80a4a8de-1f9c-4fa0-b6f7-cf774dfc786c" />

**Recovered Password:** `easy`

---

### Q2

**Hash:**

`CBFDAC6008F9CAB4083784CBD1874F76618D2A97`

<img width="1918" height="819" alt="Hash Analysis Q2" src="https://github.com/user-attachments/assets/f8aec15c-4d0e-4db0-abdc-9a1cd3dcac6c" />

**Recovered Password:** `password123`

---

### Q3

**Hash:**

`1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032`

<img width="1919" height="813" alt="Hash Analysis Q3" src="https://github.com/user-attachments/assets/97a873be-da85-434d-8ee8-2fcc4988e34b" />

**Recovered Password:** `letmein`

---

### Q4 — bcrypt

**Hash:**

`$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom`

<img width="1912" height="925" alt="bcrypt Hash Analysis" src="https://github.com/user-attachments/assets/edd9d039-1fe7-4286-b877-d068a4a3a90d" />

The hash could not be recovered using CrackStation, so Hashcat was used for additional password auditing.

**Hashcat Mode:** `3200` — bcrypt

```bash
hashcat -m 3200 hash.txt rockyou.txt
```

<img width="1911" height="1042" alt="Hashcat bcrypt Analysis" src="https://github.com/user-attachments/assets/3c28d720-46ad-47db-a6ce-b6d07aee7f59" />

**Additional Resource:**  
https://hashes.com/en/decrypt/hash

<img width="1913" height="985" alt="bcrypt Recovery Result" src="https://github.com/user-attachments/assets/ce1c8888-63af-47c7-8f7c-620e176fe5ae" />

**Recovered Password:** `bleh`

---

### Q5

**Hash:**

`279412f945939ba78ce0758d3fd83daa`

<img width="1920" height="852" alt="Hash Analysis Q5" src="https://github.com/user-attachments/assets/26149ce8-47c7-47f1-ab01-91c8cbb2969d" />

**Recovered Password:** `Eternity22`

<img width="1920" height="656" alt="Password Recovery Result Q5" src="https://github.com/user-attachments/assets/45934143-a498-4758-8953-9e074f2892b0" />

---
# Task 2 — Level 2

Level 2 increases the difficulty and requires additional password-auditing techniques. The challenge indicates that the passwords can be found in the `rockyou.txt` wordlist.

### Q1

**Hash:**

`F09EDCB1FCEFC6DFB23DC3505A882655FF77375ED8AA2D1C13F640FCCC2D0C85`

<img width="1920" height="1007" alt="Level 2 hash analysis - Q1" src="https://github.com/user-attachments/assets/256af5b9-73a5-4c33-b799-4dd16ce7feba" />

**Recovered Password:** `paule`

---

### Q2

**Hash:**

`1DFECA0C002AE40B8619ECF94819CC1B`

<img width="1920" height="833" alt="Level 2 hash analysis - Q2" src="https://github.com/user-attachments/assets/fe49e7e2-cb55-4aa4-a6b2-374a49a48b76" />

**Recovered Password:** `n63umy8lkf4i`

---

### Q3 — Salted Hash

**Hash:**

`$6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02.`

**Salt:** `aReallyHardSalt`

<img width="1920" height="846" alt="Salted hash analysis - Q3" src="https://github.com/user-attachments/assets/8b98c598-5abc-49da-9f5a-60963961713c" />

**Recovered Password:** `waka99`

<img width="1919" height="879" alt="Salted hash recovery result" src="https://github.com/user-attachments/assets/80dbaa49-3c8b-41a5-9305-5eecd72624c8" />

---

### Q4 — Salted Hash

**Hash:**

`e5d8870e5bdd26602cab8dbe07a942c8669e56d6`

**Salt:** `tryhackme`

<img width="1918" height="869" alt="Salted hash analysis - Q4" src="https://github.com/user-attachments/assets/ad40ee39-6c5d-4961-a1df-7eb57ffca133" />

**Recovered Password:** `481616481616`

---

## Analyst Takeaways

* Hash type identification is critical before selecting an attack mode.
* Online lookup services may work for known hashes but are not effective against every hash type.
* Hashcat provides greater flexibility for offline password auditing.
* Wordlists such as `rockyou.txt` can identify weak or commonly reused passwords.
* Salted and computationally expensive hashes require different cracking strategies.
* Strong, unique passwords significantly increase resistance to password-recovery attacks.

> **Note:** All password auditing activities documented here were performed in an authorized cybersecurity training environment.





