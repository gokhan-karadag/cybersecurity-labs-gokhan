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

Kali Linux wordlists:

ls /usr/share/wordlists/
```

**RockYou:**

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

## Disclaimer

This lab was performed for cybersecurity training and authorized security testing only.
