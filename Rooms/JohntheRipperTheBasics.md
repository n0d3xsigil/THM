| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **John the Ripper: The Basics** |


## Contents
- [Introduction](#introduction)
- [Basic Terms](#basic-terms)
- [Setting Up Your System](#setting-up-your-system)
- [Cracking Basic Hashes](#cracking-basic-hashes)
- [Cracking Windows Authentication Hashes](#cracking-windows-authentication-hashes)
- [Cracking /etc/shadow Hashes](#cracking-etcshadow-hashes)
- [Single Crack Mode](#single-crack-mode)
- [Custom Rules](#custom-rules)
- [Cracking Password Protected Zip Files](#cracking-password-protected-zip-files)
- [Cracking Password-Protected RAR Archives](#cracking-password-protected-rar-archives)
- [Cracking SSH Keys with John](#cracking-ssh-keys-with-john)
- [Further Reading](#further-reading)


## 📘Introduction
### Hashing vs Encoding vs Encryption
- **Hashing**:
  - Converts input data into a fixed-size string (digest).
  - One-way process: cannot retrieve original data from the hash.
  - Any change in input results in a completely different hash.
  - Used for data integrity verification.
- **Encoding**:
  - Converts data into a different format for compatibility or transmission.
  - Common encodings: ASCII, UTF-8, UTF-16, UTF-32, ISO-8859-1, Windows-1252.
  - Unicode encodings (UTF-8, UTF-16, UTF-32) support multiple languages.
  - Other encodings: Base32, Base64 (used for data transmission/storage).
  - **Reversible**: anyone can decode with the right tools.
  - **Not secure**: does not protect confidentiality.
- **Example (Base64)**:
  ```Shell
  echo "TryHackMe" | base64
  # Output: VHJ5SGFja01lCg==
  
  echo "VHJ5SGFja01lCg==" | base64 -d
  # Output: TryHackMe
  ```
- **Encryption**:
  - Protects data confidentiality using a cipher and a key.
  - **Reversible**: only with the correct key and algorithm.
  - Covered in previous modules.


## 📘Basic Terms
### Cryptography Concepts Refresher
- **What Are Hashes?**
  - A hash converts data of any length into a fixed-length string (digest).
  - Common algorithms: **MD4**, **MD5**, **SHA1**, **NTLM**.
  - Example:
    - `"polo"` → MD5 → `b53759f3ce692de7aff1b5779d3964da`
    - `"polomints"` → MD5 → `584b6e4f4586e136bc280f27f9c64f3b`
### What Makes Hashes Secure?
- **One-Way Function**:
  - Easy to compute a hash from input.
  - Hard (computationally infeasible) to reverse a hash to its original input.
- **P vs NP (Simplified)**:
  - **P**: Problems solvable in polynomial time (e.g., hashing).
    - _Polynomial Time_
  - **NP**: Problems where solutions can be verified quickly, but not easily found (e.g., reversing a hash).
    - _Non-deterministic Polynomial Time_
### Cracking Hashes with John the Ripper
- **Dictionary Attack**:
  - Hash a large list of words (dictionary).
  - Compare each hash to the target hash.
  - If a match is found, the original word is discovered.
- **Tool**: **John the Ripper (John)**
  - Performs fast brute-force and dictionary attacks on various hash types.
  - This room focuses on **Jumbo John**, an extended version with more features.
### Further Learning
- For deeper understanding:
  - **[Cryptography Basics](/CryptographyBasics.md)** (✅ Done)
  - **[Public Key Cryptography Basics](#PublicKeyCryptographyBasics.md)** (✅ Done)
  - **[Hashing Basics](#HashingBasics.md)** (✅ Done)
### ❓ - Question - What is the most popular extended version of John the Ripper?
#### 🧪 - Process
This is too easy, the answer is literally on the last line before the question, "Jumbo John".

Trying this as the answer
#### ✅ - Answer
- `Jumbo John` ✅


## 📘Setting Up Your System
### Tools Used in This Room
- **Primary Tools**:
  - **Jumbo John** (extended version of John the Ripper)
  - **rockyou.txt** password wordlist
- **Pre-installed Environments**:
  - **AttackBox** and **Kali Linux** already include Jumbo John.
  - No installation needed if using these environments.
### Installation Instructions
- **Linux Distributions**:
  - **Fedora**:  
    ```Shell
    sudo dnf install john
    ```
  - **Ubuntu**:  
    ```Shell
    sudo apt install john
    ```
  - These may install the core version, which lacks tools like `zip2john` and `rar2john`.
- **To Access Full Features**:
  - Build **Jumbo John** from source using the [official installation guide](https://github.com/openwall/john/blob/bleeding-jumbo/doc/INSTALL).
- **Windows**:
  - Download the appropriate zipped binary (64-bit or 32-bit).
  - Extract and install manually.
### Wordlists
- **Purpose**:
  - Used in dictionary attacks to compare hashed words against target hashes.
- **Common Sources**:
  - **/usr/share/wordlists/** on AttackBox and Kali Linux.
  - **[SecLists repository](https://github.com/danielmiessler/SecLists)** for a wide variety of wordlists.
- **rockyou.txt**:
  - Originated from a 2009 data breach (rockyou.com).
  - If not pre-installed, download from SecLists under [/Passwords/Leaked-Databases/](https://github.com/danielmiessler/SecLists/tree/master/Passwords/Leaked-Databases).
  - Extract with:
    ```Shell
    tar xvzf rockyou.txt.tar.gz
    ```
### ❓ Question - Which website’s breach was the `rockyou.txt` wordlist created from?
#### 🧪 Process
> a very large common password wordlist obtained from a data breach on a website called rockyou.com in 2009

So I guess it comes from `rockyou.com` :)

Trying this as an answer
#### ✅ Answer
- `rockyou.com` ✅


## 📘Cracking Basic Hashes
### Using John the Ripper to Crack Hashes
- **Basic Syntax**:
  ```Shell
  john [options] [file path]
  ```
  - `john`: Launches the tool.
  - `[options]`: Flags and parameters for the cracking process.
  - `[file path]`: File containing the hash(es) to crack.
### Automatic Cracking
- **Syntax**:
  ```Shell
  john --wordlist=[path to wordlist] [path to file]
  ```
  - `--wordlist=`: Enables wordlist mode using the specified file.
  - Example:
    ```bash
    john --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt
    ```
- **Note**: Automatic hash detection is convenient but not always reliable.
### Identifying Hash Types
- **When Automatic Detection Fails**:
  - Use external tools like:
    - Online hash identifiers.
    - `hash-identifier` (Python-based tool).
- **Using hash-identifier**:
  ```bash
  wget https://gitlab.com/kalilinux/packages/hash-identifier/-/raw/kali/master/hash-id.py
  python3 hash-id.py
  ```
  - Enter the hash to get a list of likely formats.
### Format-Specific Cracking
- **Syntax**:
  ```Shell
  john --format=[format] --wordlist=[path to wordlist] [path to file]
  ```
  - `--format=`: Specifies the hash format explicitly.
  - Example:
    ```Shell
    john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt
    ```
- **Note on Formats**:
  - Prefix standard hash types with `raw-` (e.g., `raw-md5`).
  - To list all supported formats:
    ```Shell
    john --list=formats
    ```
  - To search for a specific format:
    ```Shell
    john --list=formats | grep -iF "md5"
    ```
### ❓ Question 1 - What type of hash is hash1.txt?
#### 🧪 Process
```Shell
user@ip-10-10-175-3:~/John-the-Ripper-The-Basics/Task04$ python3 hash-id.py hash1.txt 
/home/user/John-the-Ripper-The-Basics/Task04/hash-id.py:13: SyntaxWarning: invalid escape sequence '\ '
  logo='''   #########################################################################
   #########################################################################
   #     __  __                     __           ______    _____           #
   #    /\ \/\ \                   /\ \         /\__  _\  /\  _ `\         #
   #    \ \ \_\ \     __      ____ \ \ \___     \/_/\ \/  \ \ \/\ \        #
   #     \ \  _  \  /'__`\   / ,__\ \ \  _ `\      \ \ \   \ \ \ \ \       #
   #      \ \ \ \ \/\ \_\ \_/\__, `\ \ \ \ \ \      \_\ \__ \ \ \_\ \      #
   #       \ \_\ \_\ \___ \_\/\____/  \ \_\ \_\     /\_____\ \ \____/      #
   #        \/_/\/_/\/__/\/_/\/___/    \/_/\/_/     \/_____/  \/___/  v1.2 #
   #                                                             By Zion3R #
   #                                                    www.Blackploit.com #
   #                                                   Root@Blackploit.com #
   #########################################################################
--------------------------------------------------

 Not Found.
--------------------------------------------------
 HASH: ^C

        Bye!
```
Trying again with `./`
```Shell
user@ip-10-10-175-3:~/John-the-Ripper-The-Basics/Task04$ python3 hash-id.py ./hash1.txt 
/home/user/John-the-Ripper-The-Basics/Task04/hash-id.py:13: SyntaxWarning: invalid escape sequence '\ '
  logo='''   #########################################################################
   #########################################################################
   #     __  __                     __           ______    _____           #
   #    /\ \/\ \                   /\ \         /\__  _\  /\  _ `\         #
   #    \ \ \_\ \     __      ____ \ \ \___     \/_/\ \/  \ \ \/\ \        #
   #     \ \  _  \  /'__`\   / ,__\ \ \  _ `\      \ \ \   \ \ \ \ \       #
   #      \ \ \ \ \/\ \_\ \_/\__, `\ \ \ \ \ \      \_\ \__ \ \ \_\ \      #
   #       \ \_\ \_\ \___ \_\/\____/  \ \_\ \_\     /\_____\ \ \____/      #
   #        \/_/\/_/\/__/\/_/\/___/    \/_/\/_/     \/_____/  \/___/  v1.2 #
   #                                                             By Zion3R #
   #                                                    www.Blackploit.com #
   #                                                   Root@Blackploit.com #
   #########################################################################
--------------------------------------------------

 Not Found.
--------------------------------------------------
 HASH: 2e728dd31fb5949bc39cac5a9f066498

Possible Hashs:
[+] MD5
[+] Domain Cached Credentials - MD4(MD4(($pass)).(strtolower($username)))

Least Possible Hashs:
[+] RAdmin v2.x
[+] NTLM
[+] MD4
[+] MD2
[+] MD5(HMAC)
[+] MD4(HMAC)
[+] MD2(HMAC)
[+] MD5(HMAC(Wordpress))
[+] Haval-128
[+] Haval-128(HMAC)
[+] RipeMD-128
[+] RipeMD-128(HMAC)
[+] SNEFRU-128
[+] SNEFRU-128(HMAC)
[+] Tiger-128
[+] Tiger-128(HMAC)
[+] md5($pass.$salt)
[+] md5($salt.$pass)
[+] md5($salt.$pass.$salt)
[+] md5($salt.$pass.$username)
[+] md5($salt.md5($pass))
[+] md5($salt.md5($pass))
[+] md5($salt.md5($pass.$salt))
[+] md5($salt.md5($pass.$salt))
[+] md5($salt.md5($salt.$pass))
[+] md5($salt.md5(md5($pass).$salt))
[+] md5($username.0.$pass)
[+] md5($username.LF.$pass)
[+] md5($username.md5($pass).$salt)
[+] md5(md5($pass))
[+] md5(md5($pass).$salt)
[+] md5(md5($pass).md5($salt))
[+] md5(md5($salt).$pass)
[+] md5(md5($salt).md5($pass))
[+] md5(md5($username.$pass).$salt)
[+] md5(md5(md5($pass)))
[+] md5(md5(md5(md5($pass))))
[+] md5(md5(md5(md5(md5($pass)))))
[+] md5(sha1($pass))
[+] md5(sha1(md5($pass)))
[+] md5(sha1(md5(sha1($pass))))
[+] md5(strtoupper(md5($pass)))
--------------------------------------------------
 HASH: 
```
I'm gonna just assume `MD5`

Trying that as the answer
#### ✅ Answer 1
- `MD5` ✅
### ❓ Question 2 - What is the cracked value of hash1.txt?
#### 🧪 Process
First going to list the formats to see what they meant by `raw-md5`
```Shell
user@ip-10-10-175-3:~/John-the-Ripper-The-Basics/Task04$ john --list=formats
descrypt, bsdicrypt, md5crypt, md5crypt-long, bcrypt, scrypt, LM, AFS, 
tripcode, AndroidBackup, adxcrypt, agilekeychain, aix-ssha1, aix-ssha256, 
aix-ssha512, andOTP, ansible, argon2, armory, as400-des, as400-ssha1, 
asa-md5, AxCrypt, AzureAD, BestCrypt, BestCryptVE4, bfegg, Bitcoin, 
BitLocker, bitshares, Bitwarden, BKS, Blackberry-ES10, WoWSRP, Blockchain, 
cardano, chap, Clipperz, cloudkeychain, dynamic_n, cq, CRC32, cryptoSafe, 
sha1crypt, sha256crypt, sha512crypt, Citrix_NS10, dahua, dashlane, 
diskcryptor, Django, django-scrypt, dmd5, dmg, dominosec, dominosec8, 
DPAPImk, dragonfly3-32, dragonfly3-64, dragonfly4-32, dragonfly4-64, Drupal7, 
eCryptfs, eigrp, electrum, ENCDataVault-MD5, ENCDataVault-PBKDF2, EncFS, 
enpass, EPI, EPiServer, ethereum, fde, Fortigate256, Fortigate, FormSpring, 
FVDE, geli, gost, streebog256crypt, streebog512crypt, gost94crypt, gpg, 
HAVAL-128-4, HAVAL-256-3, hdaa, hMailServer, hsrp, IKE, ipb2, itunes-backup, 
iwork, KeePass, keplr, keychain, keyring, keystore, known_hosts, krb4, krb5, 
krb5asrep, krb5pa-sha1, krb5pa-md5, krb5tgs, krb5tgs-sha1, krb5-17, krb5-18, 
krb5-3, kwallet, lp, lpcli, leet, lotus5, lotus85, LUKS, MD2, mdc2, 
MediaWiki, monero, money, MongoDB, scram, Mozilla, mscash, mscash2, MSCHAPv2, 
mschapv2-naive, mssql, mssql05, mssql12, multibit, mysqlna, mysql-sha1, 
mysql, net-ah, nethalflm, netlm, netlmv2, net-md5, netntlmv2, netntlm, 
netntlm-naive, net-sha1, nk, notes, md5ns, nsec3, NT, NT-long, o10glogon, 
o3logon, o5logon, ODF, Office, oldoffice, OpenBSD-SoftRAID, openssl-enc, 
oracle, oracle11, Oracle12C, osc, ospf, Padlock, Palshop, Panama, 
PBKDF2-HMAC-MD4, PBKDF2-HMAC-MD5, PBKDF2-HMAC-SHA1, PBKDF2-HMAC-SHA256, 
PBKDF2-HMAC-SHA512, PDF, PEM, pfx, pgpdisk, pgpsda, pgpwde, phpass, PHPS, 
PHPS2, pix-md5, PKZIP, po, postgres, PST, PuTTY, pwsafe, qnx, RACF, 
RACF-KDFAES, radius, RAdmin, RAKP, rar, RAR5, Raw-SHA512, Raw-Blake2, 
Raw-Keccak, Raw-Keccak-256, Raw-MD4, Raw-MD5, Raw-MD5u, Raw-SHA1, 
Raw-SHA1-AxCrypt, Raw-SHA1-Linkedin, Raw-SHA224, Raw-SHA256, Raw-SHA3, 
Raw-SHA384, restic, ripemd-128, ripemd-160, rsvp, RVARY, Siemens-S7, 
Salted-SHA1, SSHA512, sapb, sapg, saph, sappse, securezip, 7z, Signal, SIP, 
skein-256, skein-512, skey, SL3, SM3, Snefru-128, Snefru-256, LastPass, SNMP, 
solarwinds, SSH, sspr, Stribog-256, Stribog-512, STRIP, SunMD5, SybaseASE, 
Sybase-PROP, tacacs-plus, tcp-md5, telegram, tezos, Tiger, timeroast, 
tc_aes_xts, tc_ripemd160, tc_ripemd160boot, tc_sha512, tc_whirlpool, vdi, 
OpenVMS, vmx, VNC, vtp, wbb3, whirlpool, whirlpool0, whirlpool1, wpapsk, 
wpapsk-pmk, xmpp-scram, xsha, xsha512, zed, ZIP, ZipMonster, plaintext, 
has-160, HMAC-MD5, HMAC-SHA1, HMAC-SHA224, HMAC-SHA256, HMAC-SHA384, 
HMAC-SHA512, dummy, crypt
430 formats (151 dynamic formats shown as just "dynamic_n" here)
```
Oh yeah,grep for filtering

```Shell
user@ip-10-10-175-3:~/John-the-Ripper-The-Basics/Task04$ john --list=formats | grep -iF "md5"
descrypt, bsdicrypt, md5crypt, md5crypt-long, bcrypt, scrypt, LM, AFS, 
asa-md5, AxCrypt, AzureAD, BestCrypt, BestCryptVE4, bfegg, Bitcoin, 
diskcryptor, Django, django-scrypt, dmd5, dmg, dominosec, dominosec8, 
eCryptfs, eigrp, electrum, ENCDataVault-MD5, ENCDataVault-PBKDF2, EncFS, 
krb5asrep, krb5pa-sha1, krb5pa-md5, krb5tgs, krb5tgs-sha1, krb5-17, krb5-18, 
mysql, net-ah, nethalflm, netlm, netlmv2, net-md5, netntlmv2, netntlm, 
netntlm-naive, net-sha1, nk, notes, md5ns, nsec3, NT, NT-long, o10glogon, 
PBKDF2-HMAC-MD4, PBKDF2-HMAC-MD5, PBKDF2-HMAC-SHA1, PBKDF2-HMAC-SHA256, 
PHPS2, pix-md5, PKZIP, po, postgres, PST, PuTTY, pwsafe, qnx, RACF, 
Raw-Keccak, Raw-Keccak-256, Raw-MD4, Raw-MD5, Raw-MD5u, Raw-SHA1, 
solarwinds, SSH, sspr, Stribog-256, Stribog-512, STRIP, SunMD5, SybaseASE, 
Sybase-PROP, tacacs-plus, tcp-md5, telegram, tezos, Tiger, timeroast, 
430 formats (151 dynamic formats shown as just "dynamic_n" here)                                                                                                                                                        
has-160, HMAC-MD5, HMAC-SHA1, HMAC-SHA224, HMAC-SHA256, HMAC-SHA384,
```
Let's try with `--format=raw-md5`
```Shell
user@ip-10-10-175-3:~/John-the-Ripper-The-Basics/Task04$ john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash1.txt 
Using default input encoding: UTF-8
Loaded 1 password hash (Raw-MD5 [MD5 256/256 AVX2 8x3])
Warning: no OpenMP support for this hash type, consider --fork=2
Note: Passwords longer than 18 [worst case UTF-8] to 55 [ASCII] rejected
Press 'q' or Ctrl-C to abort, 'h' for help, almost any other key for status
biscuit          (?)     
1g 0:00:00:00 DONE (2025-06-11 18:33) 33.33g/s 89600p/s 89600c/s 89600C/s skyblue..nugget
Use the "--show --format=Raw-MD5" options to display all of the cracked passwords reliably
Session completed. 
```
Crap that was quick! We have a result of `biscuit`. 

Trying that as the answer
#### ✅ Answer 2
- `biscuit` ✅
### ❓ Question 3 - What type of hash is hash2.txt?
#### 🧪 Process
First lets `cat` the hash from `hash2.txt` and copy it to the clipboard.
```Shell
user@ip-10-10-175-3:~/John-the-Ripper-The-Basics/Task04$ cat hash2.txt 
1A732667F3917C0F4AA98BB13011B9090C6F8065
```
Next lets fire up `hash-id.py` and paste the hash (`1A732667F3917C0F4AA98BB13011B9090C6F8065`) in.
```Shell
user@ip-10-10-175-3:~/John-the-Ripper-The-Basics/Task04$ python3 hash-id.py             
/home/user/John-the-Ripper-The-Basics/Task04/hash-id.py:13: SyntaxWarning: invalid escape sequence '\ '
  logo='''   #########################################################################
   #########################################################################
   #     __  __                     __           ______    _____           #
   #    /\ \/\ \                   /\ \         /\__  _\  /\  _ `\         #
   #    \ \ \_\ \     __      ____ \ \ \___     \/_/\ \/  \ \ \/\ \        #
   #     \ \  _  \  /'__`\   / ,__\ \ \  _ `\      \ \ \   \ \ \ \ \       #
   #      \ \ \ \ \/\ \_\ \_/\__, `\ \ \ \ \ \      \_\ \__ \ \ \_\ \      #
   #       \ \_\ \_\ \___ \_\/\____/  \ \_\ \_\     /\_____\ \ \____/      #
   #        \/_/\/_/\/__/\/_/\/___/    \/_/\/_/     \/_____/  \/___/  v1.2 #
   #                                                             By Zion3R #
   #                                                    www.Blackploit.com #
   #                                                   Root@Blackploit.com #
   #########################################################################
--------------------------------------------------
 HASH: 1A732667F3917C0F4AA98BB13011B9090C6F8065

Possible Hashs:
[+] SHA-1
[+] MySQL5 - SHA-1(SHA-1($pass))

Least Possible Hashs:
[+] Tiger-160
[+] Haval-160
[+] RipeMD-160
[+] SHA-1(HMAC)
[+] Tiger-160(HMAC)
[+] RipeMD-160(HMAC)
[+] Haval-160(HMAC)
[+] SHA-1(MaNGOS)
[+] SHA-1(MaNGOS2)
[+] sha1($pass.$salt)
[+] sha1($salt.$pass)
[+] sha1($salt.md5($pass))
[+] sha1($salt.md5($pass).$salt)
[+] sha1($salt.sha1($pass))
[+] sha1($salt.sha1($salt.sha1($pass)))
[+] sha1($username.$pass)
[+] sha1($username.$pass.$salt)
[+] sha1(md5($pass))
[+] sha1(md5($pass).$salt)
[+] sha1(md5(sha1($pass)))
[+] sha1(sha1($pass))
[+] sha1(sha1($pass).$salt)
[+] sha1(sha1($pass).substr($pass,0,3))
[+] sha1(sha1($salt.$pass))
[+] sha1(sha1(sha1($pass)))
[+] sha1(strtolower($username).$pass)
--------------------------------------------------
 HASH: 
```
Let's go with `[+] SHA-1`

Trying that as an answer
#### ✅ Answer 3
- `SHA1` ✅
### ❓ Question 4- What is the cracked value of hash2.txt?
#### 🧪 Process
Lets try without spending a second to check the formats...
```Shell
user@ip-10-10-175-3:~/John-the-Ripper-The-Basics/Task04$ john --format=SHA-1 --wordlist=/usr/share/wordlists/rockyou.txt hash2.txt 
Error: No format matched requested name 'sha-1'
```
Shite... Okay lets list the formats
```Shell
user@ip-10-10-175-3:~/John-the-Ripper-The-Basics/Task04$ john --list=formats | grep -iF "sha"
tripcode, AndroidBackup, adxcrypt, agilekeychain, aix-ssha1, aix-ssha256, 
aix-ssha512, andOTP, ansible, argon2, armory, as400-des, as400-ssha1, 
BitLocker, bitshares, Bitwarden, BKS, Blackberry-ES10, WoWSRP, Blockchain, 
sha1crypt, sha256crypt, sha512crypt, Citrix_NS10, dahua, dashlane, 
krb5asrep, krb5pa-sha1, krb5pa-md5, krb5tgs, krb5tgs-sha1, krb5-17, krb5-18, 
430 formats (151 dynamic formats shown as just "dynamic_n" here)
mschapv2-naive, mssql, mssql05, mssql12, multibit, mysqlna, mysql-sha1, 
netntlm-naive, net-sha1, nk, notes, md5ns, nsec3, NT, NT-long, o10glogon, 
PBKDF2-HMAC-MD4, PBKDF2-HMAC-MD5, PBKDF2-HMAC-SHA1, PBKDF2-HMAC-SHA256, 
PBKDF2-HMAC-SHA512, PDF, PEM, pfx, pgpdisk, pgpsda, pgpwde, phpass, PHPS, 
RACF-KDFAES, radius, RAdmin, RAKP, rar, RAR5, Raw-SHA512, Raw-Blake2, 
Raw-Keccak, Raw-Keccak-256, Raw-MD4, Raw-MD5, Raw-MD5u, Raw-SHA1, 
Raw-SHA1-AxCrypt, Raw-SHA1-Linkedin, Raw-SHA224, Raw-SHA256, Raw-SHA3, 
Raw-SHA384, restic, ripemd-128, ripemd-160, rsvp, RVARY, Siemens-S7, 
Salted-SHA1, SSHA512, sapb, sapg, saph, sappse, securezip, 7z, Signal, SIP, 
tc_aes_xts, tc_ripemd160, tc_ripemd160boot, tc_sha512, tc_whirlpool, vdi, 
wpapsk-pmk, xmpp-scram, xsha, xsha512, zed, ZIP, ZipMonster, plaintext, 
has-160, HMAC-MD5, HMAC-SHA1, HMAC-SHA224, HMAC-SHA256, HMAC-SHA384, 
HMAC-SHA512, dummy, crypt
```
Better still lets filter to `raw-sha1`
```Shell
user@ip-10-10-175-3:~/John-the-Ripper-The-Basics/Task04$ john --list=formats | grep -iF "raw-sha1"
430 formats (151 dynamic formats shown as just "dynamic_n" here)
Raw-Keccak, Raw-Keccak-256, Raw-MD4, Raw-MD5, Raw-MD5u, Raw-SHA1, 
Raw-SHA1-AxCrypt, Raw-SHA1-Linkedin, Raw-SHA224, Raw-SHA256, Raw-SHA3, 
```
So we do have `Raw-SHA1`, first line of formats last on the right.

Let's try again with `Raw-SHA1` as the format
```Shell
user@ip-10-10-175-3:~/John-the-Ripper-The-Basics/Task04$ john --format=Raw-SHA1 --wordlist=/usr/share/wordlists/rockyou.txt hash2.txt 
Using default input encoding: UTF-8
Loaded 1 password hash (Raw-SHA1 [SHA1 256/256 AVX2 8x])
Warning: no OpenMP support for this hash type, consider --fork=2
Note: Passwords longer than 18 [worst case UTF-8] to 55 [ASCII] rejected
Press 'q' or Ctrl-C to abort, 'h' for help, almost any other key for status
kangeroo         (?)     
1g 0:00:00:00 DONE (2025-06-11 18:47) 33.33g/s 3904Kp/s 3904Kc/s 3904KC/s karakara..kalinda
Use the "--show --format=Raw-SHA1" options to display all of the cracked passwords reliably
Session completed. 
```
Again, very quick. We have our answer `kangeroo`.

Trying this as the answer
#### ✅ Answer 4
- `kangeroo`
### ❓ Question 5 - What type of hash is hash3.txt?
#### 🧪 Process
We'll `cat` the file `hash3.txt` and copy the hash to the clipboard
```Shell
user@ip-10-10-175-3:~/John-the-Ripper-The-Basics/Task04$ cat hash3.txt 
D7F4D3CCEE7ACD3DD7FAD3AC2BE2AAE9C44F4E9B7FB802D73136D4C53920140A
```
We'll identify potential hash id's
```Shell
user@ip-10-10-175-3:~/John-the-Ripper-The-Basics/Task04$ python3 hash-id.py 
/home/user/John-the-Ripper-The-Basics/Task04/hash-id.py:13: SyntaxWarning: invalid escape sequence '\ '
  logo='''   #########################################################################
   #########################################################################
   #     __  __                     __           ______    _____           #
   #    /\ \/\ \                   /\ \         /\__  _\  /\  _ `\         #
   #    \ \ \_\ \     __      ____ \ \ \___     \/_/\ \/  \ \ \/\ \        #
   #     \ \  _  \  /'__`\   / ,__\ \ \  _ `\      \ \ \   \ \ \ \ \       #
   #      \ \ \ \ \/\ \_\ \_/\__, `\ \ \ \ \ \      \_\ \__ \ \ \_\ \      #
   #       \ \_\ \_\ \___ \_\/\____/  \ \_\ \_\     /\_____\ \ \____/      #
   #        \/_/\/_/\/__/\/_/\/___/    \/_/\/_/     \/_____/  \/___/  v1.2 #
   #                                                             By Zion3R #
   #                                                    www.Blackploit.com #
   #                                                   Root@Blackploit.com #
   #########################################################################
--------------------------------------------------
 HASH: D7F4D3CCEE7ACD3DD7FAD3AC2BE2AAE9C44F4E9B7FB802D73136D4C53920140A

Possible Hashs:
[+] SHA-256
[+] Haval-256

Least Possible Hashs:
[+] GOST R 34.11-94
[+] RipeMD-256
[+] SNEFRU-256
[+] SHA-256(HMAC)
[+] Haval-256(HMAC)
[+] RipeMD-256(HMAC)
[+] SNEFRU-256(HMAC)
[+] SHA-256(md5($pass))
[+] SHA-256(sha1($pass))
--------------------------------------------------
 HASH: 
```
I'm assuming the first possible hash of `[+] SHA-256`

Trying that as the answer
#### ✅ Answer 5
- `SHA256` ✅
### ❓ Question 6 - What is the cracked value of hash3.txt?
#### 🧪 Process
So we know the format is `SHA-256. I'm going to go ahead and assume that the format will be `Raw-SHA256` 
```Shell
user@ip-10-10-175-3:~/John-the-Ripper-The-Basics/Task04$ john --list=formats | grep -iF "Raw-SHA256"
430 formats (151 dynamic formats shown as just "dynamic_n" here)
Raw-SHA1-AxCrypt, Raw-SHA1-Linkedin, Raw-SHA224, Raw-SHA256, Raw-SHA3, 
```
Finally we'll run John with the correct format this time `Raw-SHA256`
```Shell
user@ip-10-10-175-3:~/John-the-Ripper-The-Basics/Task04$ john --format=Raw-SHA256 --wordlist=/usr/share/wordlists/rockyou.txt hash3.txt 
Using default input encoding: UTF-8
Loaded 1 password hash (Raw-SHA256 [SHA256 256/256 AVX2 8x])
Warning: poor OpenMP scalability for this hash type, consider --fork=2
Will run 2 OpenMP threads
Note: Passwords longer than 18 [worst case UTF-8] to 55 [ASCII] rejected
Press 'q' or Ctrl-C to abort, 'h' for help, almost any other key for status
microphone       (?)     
1g 0:00:00:00 DONE (2025-06-11 19:54) 20.00g/s 1966Kp/s 1966Kc/s 1966KC/s rozalia..Dominic1
Use the "--show --format=Raw-SHA256" options to display all of the cracked passwords reliably
Session completed. 
```
It looks like we have found `microphone`.

Trying this as the answer
#### ✅ Answer 6
- `microphone` ✅
### ❓ Question 7 - What type of hash is hash4.txt?
#### 🧪 Process
`cat` file `hash4.txt` and copy the resulting hash to the clipboard.
```Shell
user@ip-10-10-175-3:~/John-the-Ripper-The-Basics/Task04$ cat hash4.txt 
c5a60cc6bbba781c601c5402755ae1044bbf45b78d1183cbf2ca1c865b6c792cf3c6b87791344986c8a832a0f9ca8d0b4afd3d9421a149d57075e1b4e93f90bf
```
Kick off `hash-id.py` and paste in the hash `c5a60cc6bbba781c601c5402755ae1044bbf45b78d1183cbf2ca1c865b6c792cf3c6b87791344986c8a832a0f9ca8d0b4afd3d9421a149d57075e1b4e93f90bf`
```Shell
user@ip-10-10-175-3:~/John-the-Ripper-The-Basics/Task04$ python3 hash-id.py 
/home/user/John-the-Ripper-The-Basics/Task04/hash-id.py:13: SyntaxWarning: invalid escape sequence '\ '
  logo='''   #########################################################################
   #########################################################################
   #     __  __                     __           ______    _____           #
   #    /\ \/\ \                   /\ \         /\__  _\  /\  _ `\         #
   #    \ \ \_\ \     __      ____ \ \ \___     \/_/\ \/  \ \ \/\ \        #
   #     \ \  _  \  /'__`\   / ,__\ \ \  _ `\      \ \ \   \ \ \ \ \       #
   #      \ \ \ \ \/\ \_\ \_/\__, `\ \ \ \ \ \      \_\ \__ \ \ \_\ \      #
   #       \ \_\ \_\ \___ \_\/\____/  \ \_\ \_\     /\_____\ \ \____/      #
   #        \/_/\/_/\/__/\/_/\/___/    \/_/\/_/     \/_____/  \/___/  v1.2 #
   #                                                             By Zion3R #
   #                                                    www.Blackploit.com #
   #                                                   Root@Blackploit.com #
   #########################################################################
--------------------------------------------------
 HASH: c5a60cc6bbba781c601c5402755ae1044bbf45b78d1183cbf2ca1c865b6c792cf3c6b87791344986c8a832a0f9ca8d0b4afd3d9421a149d57075e1b4e93f90bf

Possible Hashs:
[+] SHA-512
[+] Whirlpool

Least Possible Hashs:
[+] SHA-512(HMAC)
[+] Whirlpool(HMAC)
--------------------------------------------------
 HASH: 
```
Now we have the hash format (`SHA512`) we can answer this question and then proceed with cracking the hash in Q8.

Trying this as the answer
#### ✅ Answer 7
- `SHA512` ❌
Assumptions, assumptions, assumptions.
The other option was `Whirlpool` so let's try that as the answer...
- `Whirlpool`
### ❓ Question 8 - What is the cracked value of hash4.txt?
#### 🧪 Process
So now we can search the formats for `whirlpool`
```Shell
user@ip-10-10-175-3:~/John-the-Ripper-The-Basics/Task04$ john --list=formats | grep -iF "whirlpool"
430 formats (151 dynamic formats shown as just "dynamic_n" here)
tc_aes_xts, tc_ripemd160, tc_ripemd160boot, tc_sha512, tc_whirlpool, vdi, 
OpenVMS, vmx, VNC, vtp, wbb3, whirlpool, whirlpool0, whirlpool1, wpapsk, 
```
There are a couple `whirlpool`'s in the list. Let's stick the default one.
```Shell
user@ip-10-10-175-3:~/John-the-Ripper-The-Basics/Task04$ john --format=whirlpool --wordlist=/usr/share/wordlists/rockyou.txt hash4.txt 
Using default input encoding: UTF-8
Loaded 1 password hash (whirlpool [WHIRLPOOL 32/64])
Warning: poor OpenMP scalability for this hash type, consider --fork=2
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, 'h' for help, almost any other key for status
colossal         (?)     
1g 0:00:00:00 DONE (2025-06-11 20:09) 2.083g/s 1416Kp/s 1416Kc/s 1416KC/s cooldog12..chata1994
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```
We have a result! `colossal`

Trying this as the answer
#### ✅ Answer 8
- `colossal` ✅


## 📘Cracking Windows Authentication Hashes
### Cracking Authentication Hashes
- **Context**:
  - Authentication hashes are hashed versions of passwords stored by operating systems.
  - Cracking these hashes is a common task in penetration testing or red team engagements.
  - Access to these hashes typically requires privileged access.
### NTHash / NTLM
- **Definition**:
  - **NTHash** (also known as **NTLM**) is the format used by modern Windows systems to store password hashes.
  - NTLM is a successor to the older **LM (LAN Manager)** hash format.
- **Historical Note**:
  - "NT" originally stood for **New Technology**, used in early Windows versions.
  - Though the name was dropped, it persists in technologies like NTLM.
- **Storage Location**:
  - Password hashes are stored in the **SAM (Security Account Manager)** database on Windows.
  - Can also be found in **NTDS.dit** (Active Directory database).
- **Hash Acquisition Tools**:
  - Tools like **Mimikatz** can be used to dump hashes from SAM or NTDS.dit.
- **Cracking vs. Pass-the-Hash**:
  - Cracking may not always be necessary.
  - **Pass-the-hash** attacks can be used to authenticate using the hash directly.
  - Cracking is useful when weak passwords are suspected or required for further escalation.
### ❓ Question 1 - What do we need to set the `--format` flag to in order to crack this hash?
#### 🧪 Process
I'm trying to refresh my memory quickly. It is the next day now so starting from scratch kinda.

We where talking about `ntlm` so I want to get get the list of formats from john.
```Shell
user@ip-10-10-197-123:~$ john --list=formats
descrypt, bsdicrypt, md5crypt, md5crypt-long, bcrypt, scrypt, LM, AFS, 
tripcode, AndroidBackup, adxcrypt, agilekeychain, aix-ssha1, aix-ssha256, 
aix-ssha512, andOTP, ansible, argon2, armory, as400-des, as400-ssha1, 
asa-md5, AxCrypt, AzureAD, BestCrypt, BestCryptVE4, bfegg, Bitcoin, 
BitLocker, bitshares, Bitwarden, BKS, Blackberry-ES10, WoWSRP, Blockchain, 
cardano, chap, Clipperz, cloudkeychain, dynamic_n, cq, CRC32, cryptoSafe, 
sha1crypt, sha256crypt, sha512crypt, Citrix_NS10, dahua, dashlane, 
diskcryptor, Django, django-scrypt, dmd5, dmg, dominosec, dominosec8, 
DPAPImk, dragonfly3-32, dragonfly3-64, dragonfly4-32, dragonfly4-64, Drupal7, 
eCryptfs, eigrp, electrum, ENCDataVault-MD5, ENCDataVault-PBKDF2, EncFS, 
enpass, EPI, EPiServer, ethereum, fde, Fortigate256, Fortigate, FormSpring, 
FVDE, geli, gost, streebog256crypt, streebog512crypt, gost94crypt, gpg, 
HAVAL-128-4, HAVAL-256-3, hdaa, hMailServer, hsrp, IKE, ipb2, itunes-backup, 
iwork, KeePass, keplr, keychain, keyring, keystore, known_hosts, krb4, krb5, 
krb5asrep, krb5pa-sha1, krb5pa-md5, krb5tgs, krb5tgs-sha1, krb5-17, krb5-18, 
krb5-3, kwallet, lp, lpcli, leet, lotus5, lotus85, LUKS, MD2, mdc2, 
MediaWiki, monero, money, MongoDB, scram, Mozilla, mscash, mscash2, MSCHAPv2, 
mschapv2-naive, mssql, mssql05, mssql12, multibit, mysqlna, mysql-sha1, 
mysql, net-ah, nethalflm, netlm, netlmv2, net-md5, netntlmv2, netntlm, 
netntlm-naive, net-sha1, nk, notes, md5ns, nsec3, NT, NT-long, o10glogon, 
o3logon, o5logon, ODF, Office, oldoffice, OpenBSD-SoftRAID, openssl-enc, 
oracle, oracle11, Oracle12C, osc, ospf, Padlock, Palshop, Panama, 
PBKDF2-HMAC-MD4, PBKDF2-HMAC-MD5, PBKDF2-HMAC-SHA1, PBKDF2-HMAC-SHA256, 
PBKDF2-HMAC-SHA512, PDF, PEM, pfx, pgpdisk, pgpsda, pgpwde, phpass, PHPS, 
PHPS2, pix-md5, PKZIP, po, postgres, PST, PuTTY, pwsafe, qnx, RACF, 
RACF-KDFAES, radius, RAdmin, RAKP, rar, RAR5, Raw-SHA512, Raw-Blake2, 
Raw-Keccak, Raw-Keccak-256, Raw-MD4, Raw-MD5, Raw-MD5u, Raw-SHA1, 
Raw-SHA1-AxCrypt, Raw-SHA1-Linkedin, Raw-SHA224, Raw-SHA256, Raw-SHA3, 
Raw-SHA384, restic, ripemd-128, ripemd-160, rsvp, RVARY, Siemens-S7, 
Salted-SHA1, SSHA512, sapb, sapg, saph, sappse, securezip, 7z, Signal, SIP, 
skein-256, skein-512, skey, SL3, SM3, Snefru-128, Snefru-256, LastPass, SNMP, 
solarwinds, SSH, sspr, Stribog-256, Stribog-512, STRIP, SunMD5, SybaseASE, 
Sybase-PROP, tacacs-plus, tcp-md5, telegram, tezos, Tiger, timeroast, 
tc_aes_xts, tc_ripemd160, tc_ripemd160boot, tc_sha512, tc_whirlpool, vdi, 
OpenVMS, vmx, VNC, vtp, wbb3, whirlpool, whirlpool0, whirlpool1, wpapsk, 
wpapsk-pmk, xmpp-scram, xsha, xsha512, zed, ZIP, ZipMonster, plaintext, 
has-160, HMAC-MD5, HMAC-SHA1, HMAC-SHA224, HMAC-SHA256, HMAC-SHA384, 
HMAC-SHA512, dummy, crypt
430 formats (151 dynamic formats shown as just "dynamic_n" here)
```

Actually, we wanna filter this, lets throw in a `grep -iF <something>`.
```Shell
user@ip-10-10-197-123:~$ john --list=formats | grep -iF "ntlm"
mysql, net-ah, nethalflm, netlm, netlmv2, net-md5, netntlmv2, netntlm, 
netntlm-naive, net-sha1, nk, notes, md5ns, nsec3, NT, NT-long, o10glogon, 
430 formats (151 dynamic formats shown as just "dynamic_n" here)
user@ip-10-10-197-123:~$ 
```

Okay so we've got a few options here. However, we need to take stock a moment. the practical is referring to `~/John-the-Ripper-The-Basics/Task05/`. Lets have a look at the `ntlm.txt` file withing this folder.
```Shell
user@ip-10-10-197-123:~$ cd John-the-Ripper-The-Basics/Task05/
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task05$ cat ntlm.txt 
5460C85BD858A11475115D2DD3A82333
```

Okay, so we've got our hash (`5460C85BD858A11475115D2DD3A82333`). We could quickly pass this through the `hash-id.py` script.
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task04$ python3 hash-id.py 
/home/user/John-the-Ripper-The-Basics/Task04/hash-id.py:13: SyntaxWarning: invalid escape sequence '\ '
  logo=''''`   #########################################################################
   #########################################################################
   #     __  __                     __           ______    _____           #
   #    /\ \/\ \                   /\ \         /\__  _\  /\  _ `\         #
   #    \ \ \_\ \     __      ____ \ \ \___     \/_/\ \/  \ \ \/\ \        #
   #     \ \  _  \  /'__`\   / ,__\ \ \  _ `\      \ \ \   \ \ \ \ \       #
   #      \ \ \ \ \/\ \_\ \_/\__, `\ \ \ \ \ \      \_\ \__ \ \ \_\ \      #
   #       \ \_\ \_\ \___ \_\/\____/  \ \_\ \_\     /\_____\ \ \____/      #
   #        \/_/\/_/\/__/\/_/\/___/    \/_/\/_/     \/_____/  \/___/  v1.2 #
   #                                                             By Zion3R #
   #                                                    www.Blackploit.com #
   #                                                   Root@Blackploit.com #
   #########################################################################
--------------------------------------------------
 HASH: 5460C85BD858A11475115D2DD3A82333

Possible Hashs:
[+] MD5
[+] Domain Cached Credentials - MD4(MD4(($pass)).(strtolower($username)))

Least Possible Hashs:
[+] RAdmin v2.x
[+] NTLM
[+] MD4
[+] MD2
[+] MD5(HMAC)
[+] MD4(HMAC)
[+] MD2(HMAC)
[+] MD5(HMAC(Wordpress))
[+] Haval-128
[+] Haval-128(HMAC)
[+] RipeMD-128
[+] RipeMD-128(HMAC)
[+] SNEFRU-128
[+] SNEFRU-128(HMAC)
[+] Tiger-128
[+] Tiger-128(HMAC)
[+] md5($pass.$salt)
[+] md5($salt.$pass)
[+] md5($salt.$pass.$salt)
[+] md5($salt.$pass.$username)
[+] md5($salt.md5($pass))
[+] md5($salt.md5($pass))
[+] md5($salt.md5($pass.$salt))
[+] md5($salt.md5($pass.$salt))
[+] md5($salt.md5($salt.$pass))
[+] md5($salt.md5(md5($pass).$salt))
[+] md5($username.0.$pass)
[+] md5($username.LF.$pass)
[+] md5($username.md5($pass).$salt)
[+] md5(md5($pass))
[+] md5(md5($pass).$salt)
[+] md5(md5($pass).md5($salt))
[+] md5(md5($salt).$pass)
[+] md5(md5($salt).md5($pass))
[+] md5(md5($username.$pass).$salt)
[+] md5(md5(md5($pass)))
[+] md5(md5(md5(md5($pass))))
[+] md5(md5(md5(md5(md5($pass)))))
[+] md5(sha1($pass))
[+] md5(sha1(md5($pass)))
[+] md5(sha1(md5(sha1($pass))))
[+] md5(strtoupper(md5($pass)))
--------------------------------------------------
 HASH: 
```

Honestly, not that helpful. I guess we already know its ntlm considering the file is `ntlm.txt` so lets go with `NTLM`. Yesterday I was using my work laptop so couldn't look at the hashcat site. 

So `ntlm` is hash-mode 1000. But the answer is `_ _`, or 2 characters. All of the 2 character modes are MD5 related.

I tried 10 to see if I got an extra 'hint'. and I did 

> Your answer is incorrect. Please ensure it follows the answer format represented by underscores, check for typos, and try again.

So lets look at what John expects when passing the `--format` argument.

```Shell
user@ip-10-10-197-123:~$ john --help
John the Ripper 1.9.0-jumbo-1+bleeding-ffd18e6b5d 2024-09-29 04:20:54 +0200 OMP [linux-gnu 64-bit x86_64 AVX2 AC]
Copyright (c) 1996-2024 by Solar Designer and others
Homepage: https://www.openwall.com/john/

Usage: john [OPTIONS] [PASSWORD-FILES]

[ Truncated ]

--format=[NAME|CLASS][,..] Force hash of type NAME. The supported formats can
                           be seen with --list=formats and --list=subformats.
                           See also doc/OPTIONS for more advanced selection of
                           format(s), including using classes and wildcards.
```

Interesting, so I made the assumption that we could only use the numbered format, however it looks like you can use `<NAME>` instead. Given that this is a 2 character answer I'm going to assume one of `NT` or `LM`. Since NTLM is the successor of Lan Manager (LM) I'm going to try that first.
#### ✅ Answer
- `LM` ❌

Okay I should have guessed as much. So let's go with `NT` instead.
- `NT` ✅

On to question 2...
### ❓ Question 2 - What is the cracked value of this password?
#### 🧪 Process
So we have worked out we need to use `--format=nt` let's now give it a try.
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task05$ john --format=nt --wordlist /usr/share/wordlists/rockyou.txt ntlm.txt          
Warning: invalid UTF-8 seen reading /usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 52 password hashes with no different salts (NT [MD4 256/256 AVX2 8x3])
Warning: no OpenMP support for this hash type, consider --fork=2
Note: Passwords longer than 27 rejected
Proceeding with wordlist:/home/user/src/john/run/password.lst
Press 'q' or Ctrl-C to abort, 'h' for help, almost any other key for status
mushroom         (?)     
1g 0:00:00:00 DONE (2025-06-12 13:34) 3.846g/s 6906Kp/s 6906Kc/s 352254KC/s Dontrell..sambarock
Warning: passwords printed above might not be all those cracked
Use the "--show --format=NT" options to display all of the cracked passwords reliably
Session completed. 
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task05$ 
```

And there we are, a suspected result of `mushroom`.

Trying this as the answer
#### ✅ Answer
- `mushroom` ✅


## 📘Cracking /etc/shadow Hashes
### Cracking Hashes from `/etc/shadow`
- **Purpose**:
  - The `/etc/shadow` file on Linux stores password hashes and related metadata.
  - Access requires root privileges.
  - Cracking is possible if hashes are weak and accessible.
### Unshadowing
- **Why It’s Needed**:
  - John the Ripper requires a specific format.
  - Combine `/etc/passwd` and `/etc/shadow` using the `unshadow` tool.

- **Syntax**:
  ```bash
  unshadow [path to passwd] [path to shadow] > unshadowed.txt
  ```

- **Example**:
  ```bash
  unshadow local_passwd local_shadow > unshadowed.txt
  ```

- **File Content Example**:
  - `local_passwd`:
    ```
    root:x:0:0::/root:/bin/bash
    ```
  - `local_shadow`:
    ```
    root:$6$2nwjN454g.dv4HN/$m9Z/...:18576::::::
    ```

### Cracking the Hashes
- **Command**:
  ```bash
  john --wordlist=/usr/share/wordlists/rockyou.txt --format=sha512crypt unshadowed.txt
  ```

- **Notes**:
  - Format flag (`--format=sha512crypt`) may be required depending on the hash type.
  - If the input is correctly formatted, John may auto-detect the format.

### ❓ Question - What is the root password?
#### 🧪 Process
- Folder: `~/John-the-Ripper-The-Basics/Task06/`
- File: `etchashes.txt`

Let's see what we are working with. I'll cat the `etchashes.txt` file
```Shell
user@ip-10-10-197-123:~$ cat ~/John-the-Ripper-The-Basics/Task06/etc_hashes.txt 
This is everything I managed to recover from the target machine before my computer crashed... See if you can crack the hash so we can at least salvage a pa
ssword to try and get back in.

root:x:0:0::/root:/bin/bash
root:$6$Ha.d5nGupBm29pYr$yugXSk24ZljLTAZZagtGwpSQhb3F2DOJtnHrvk7HI2ma4GsuioHp8sm3LJiRJpKfIf7lZQ29qgtH17Q/JDpYM/:18576::::::
```

I had a quick `ls` in the folder to see what was in there
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task06$ ls -la
total 20
drwxr-xr-x 2 user user 4096 Oct 10  2024 .
drwxr-xr-x 9 user user 4096 Oct 10  2024 ..
-rw-r--r-- 1 user user  340 Oct 10  2024 etc_hashes.txt
-rw-r--r-- 1 user user   28 Oct 10  2024 local_passwd
-rw-r--r-- 1 user user  124 Oct 10  2024 local_shadow
```

As expected `local_passwd` has the user info and `local_shadow` has the hash.

copied the `local_passwd > passwd` and `local_shadow > shadow` for simplicity for in a minute
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task06$ cp local_passwd passwd
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task06$ cp local_shadow shadow
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task06$ ls -la
total 28
drwxr-xr-x 2 user user 4096 Jun 12 13:46 .
drwxr-xr-x 9 user user 4096 Oct 10  2024 ..
-rw-r--r-- 1 user user  340 Oct 10  2024 etc_hashes.txt
-rw-r--r-- 1 user user   28 Oct 10  2024 local_passwd
-rw-r--r-- 1 user user  124 Oct 10  2024 local_shadow
-rw-r--r-- 1 user user   28 Jun 12 13:46 passwd
-rw-r--r-- 1 user user  124 Jun 12 13:46 shadow
```

Next lets try `unshadow`.
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task06$ unshadow passwd shadow > unshadowed.txt
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task06$ cat unshadowed.txt 
root:$6$Ha.d5nGupBm29pYr$yugXSk24ZljLTAZZagtGwpSQhb3F2DOJtnHrvk7HI2ma4GsuioHp8sm3LJiRJpKfIf7lZQ29qgtH17Q/JDpYM/:0:0::/root:/bin/bash
```

Finally I think we're ready to give this a try. Assuming `--format=sha512crypt`
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task06$ john --wordlist=/usr/share/wordlists/rockyou.txt --format=sha512crypt unshadowed.txt 
Using default input encoding: UTF-8
Loaded 1 password hash (sha512crypt, crypt(3) $6$ [SHA512 256/256 AVX2 4x])
Cost 1 (iteration count) is 5000 for all loaded hashes
Will run 2 OpenMP threads
Note: Passwords longer than 26 [worst case UTF-8] to 79 [ASCII] rejected
Press 'q' or Ctrl-C to abort, 'h' for help, almost any other key for status
1234             (root)     
1g 0:00:00:02 DONE (2025-06-12 13:53) 0.4274g/s 547.0p/s 547.0c/s 547.0C/s kucing..poohbear1
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```

Oh, the ol' `1234` password huh.

Trying this as the answer
#### ✅ Answer
- `1234` ✅


## 📘Single Crack Mode
### John the Ripper: Single Crack Mode
- **Overview**:
  - Single Crack mode uses usernames and related metadata to generate password guesses.
  - Applies **word mangling** techniques to mutate usernames into likely passwords.
### Word Mangling
- **Example (Username: Markus)**:
  - Variants: `Markus1`, `MArkus`, `MARKus`, `Markus!`, etc.
  - John uses **mangling rules** to generate these variants heuristically.
### GECOS Field Usage
- **GECOS**: General Electric Comprehensive Operating System
  - Found in the 5th field of `/etc/passwd`.
  - Stores user info like full name, office number, etc.
  - John can use this data to enhance wordlist generation in Single Crack mode.
### Using Single Crack Mode
- **Syntax**:
  ```bash
  john --single --format=[format] [path to file]
  ```
  - `--single`: Enables Single Crack mode.
  - `--format=`: Specifies the hash format (e.g., `raw-sha256`).

- **Example**:
  ```bash
  john --single --format=raw-sha256 hashes.txt
  ```

### File Format Requirements
- **Input Format**:
  - Must include the username and hash in the format:
    ```
    username:hash
    ```
  - Example:
    - From:  
      ```
      1efee03cdcb96d90ad48ccc7b8666033
      ```
    - To:  
      ```
      mike:1efee03cdcb96d90ad48ccc7b8666033
      ```

### ❓ Question - What is Joker’s password?
#### 🧪 Process
So this is new to me, lets just try, for fun, `john --single --format=raw-sha256 hashes.txt`
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task07$ john --single --format=raw-sha256 hash07.txt 
Using default input encoding: UTF-8
No password hashes loaded (see FAQ)
```

Let's create a new file where the contents are `user:hash`. First I'll just copy `hash07.txt` to `joker.txt`
```Shell user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task07$ cp hash07.txt joker.txt
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task07$ ls -la
total 16
drwxr-xr-x 2 user user 4096 Jun 12 14:06 .
drwxr-xr-x 9 user user 4096 Oct 10  2024 ..
-rw-r--r-- 1 user user   33 Oct 10  2024 hash07.txt
-rw-r--r-- 1 user user   33 Jun 12 14:06 joker.txt
```

Next I'll open `joker.txt` in vim, pop in `joker:` before the hash and `:x` out.
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task07$ ls -la; cat joker.txt 
total 16
drwxr-xr-x 2 user user 4096 Jun 12 14:07 .
drwxr-xr-x 9 user user 4096 Oct 10  2024 ..
-rw-r--r-- 1 user user   33 Oct 10  2024 hash07.txt
-rw-r--r-- 1 user user   39 Jun 12 14:07 joker.txt
joker:7bf6d9bb82bed1302f331fc6b816aada
```

Lets try `John` again...
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task07$ john --single --format=raw-sha256 joker.txt 
Using default input encoding: UTF-8
No password hashes loaded (see FAQ)
```

No change, however I've just noticed that the user is `Joker` not `joker`, not really sure this will make a difference, but lets try anyway. 
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task07$ cat joker.txt 
Joker:7bf6d9bb82bed1302f331fc6b816aada
```

Trying again with the correct username
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task07$ john --single --format=raw-sha256 joker.txt 
Using default input encoding: UTF-8
No password hashes loaded (see FAQ)
```

Okay, as expected. Maybe we need to go back to basics and understand the hash. Lets run `hash-id.py` and verify what looks like an `MD5` hash.

Let's cat and copy the hash
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task07$ cat hash07.txt               
7bf6d9bb82bed1302f331fc6b816aada
```

Then run the `hash-id.py` script from the `../Task04`, pasting in the has we copied above.
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task07$ python3 ../Task04/hash-id.py 
/home/user/John-the-Ripper-The-Basics/Task07/../Task04/hash-id.py:13: SyntaxWarning: invalid escape sequence '\ '
  logo=''''   #########################################################################
   #########################################################################
   #     __  __                     __           ______    _____           #
   #    /\ \/\ \                   /\ \         /\__  _\  /\  _ `\         #
   #    \ \ \_\ \     __      ____ \ \ \___     \/_/\ \/  \ \ \/\ \        #
   #     \ \  _  \  /'__`\   / ,__\ \ \  _ `\      \ \ \   \ \ \ \ \       #
   #      \ \ \ \ \/\ \_\ \_/\__, `\ \ \ \ \ \      \_\ \__ \ \ \_\ \      #
   #       \ \_\ \_\ \___ \_\/\____/  \ \_\ \_\     /\_____\ \ \____/      #
   #        \/_/\/_/\/__/\/_/\/___/    \/_/\/_/     \/_____/  \/___/  v1.2 #
   #                                                             By Zion3R #
   #                                                    www.Blackploit.com #
   #                                                   Root@Blackploit.com #
   #########################################################################
--------------------------------------------------
 HASH: 7bf6d9bb82bed1302f331fc6b816aada

Possible Hashs:
[+] MD5
[+] Domain Cached Credentials - MD4(MD4(($pass)).(strtolower($username)))

Least Possible Hashs:
[+] RAdmin v2.x
[+] NTLM
[+] MD4
[+] MD2
[+] MD5(HMAC)
[+] MD4(HMAC)
[+] MD2(HMAC)
[+] MD5(HMAC(Wordpress))
[+] Haval-128
[+] Haval-128(HMAC)
[+] RipeMD-128
[+] RipeMD-128(HMAC)
[+] SNEFRU-128
[+] SNEFRU-128(HMAC)
[+] Tiger-128
[+] Tiger-128(HMAC)
[+] md5($pass.$salt)
[+] md5($salt.$pass)
[+] md5($salt.$pass.$salt)
[+] md5($salt.$pass.$username)
[+] md5($salt.md5($pass))
[+] md5($salt.md5($pass))
[+] md5($salt.md5($pass.$salt))
[+] md5($salt.md5($pass.$salt))
[+] md5($salt.md5($salt.$pass))
[+] md5($salt.md5(md5($pass).$salt))
[+] md5($username.0.$pass)
[+] md5($username.LF.$pass)
[+] md5($username.md5($pass).$salt)
[+] md5(md5($pass))
[+] md5(md5($pass).$salt)
[+] md5(md5($pass).md5($salt))
[+] md5(md5($salt).$pass)
[+] md5(md5($salt).md5($pass))
[+] md5(md5($username.$pass).$salt)
[+] md5(md5(md5($pass)))
[+] md5(md5(md5(md5($pass))))
[+] md5(md5(md5(md5(md5($pass)))))
[+] md5(sha1($pass))
[+] md5(sha1(md5($pass)))
[+] md5(sha1(md5(sha1($pass))))
[+] md5(strtoupper(md5($pass)))
--------------------------------------------------
 HASH:

Bye!
```

Okay, so this says MD5. Humm, lets try that shall we. If I remember correctly the hash mode for md5 is `0`.
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task07$ john --single --format=0 joker.txt 
Error: No format matched requested name '0'
```

Nothing named `0`. I'm sure I used 0 previously. Related to `--single` maybe?! Okay lets try `md5` instead.
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task07$ john --single --format=md5 joker.txt 
Error: No format matched requested name 'md5'
```

Oh, yeah, it needs to be `raw-md5` doesn't it.
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task07$ john --single --format=raw-md5 joker.txt 
Using default input encoding: UTF-8
Loaded 1 password hash (Raw-MD5 [MD5 256/256 AVX2 8x3])
Warning: no OpenMP support for this hash type, consider --fork=2
Note: Passwords longer than 18 [worst case UTF-8] to 55 [ASCII] rejected
Press 'q' or Ctrl-C to abort, 'h' for help, almost any other key for status
Warning: Only 18 candidates buffered for the current salt, minimum 24 needed for performance.
Jok3r            (Joker)     
1g 0:00:00:00 DONE (2025-06-12 14:13) 50.00g/s 9750p/s 9750c/s 9750C/s Joker!!..J0ker
Use the "--show --format=Raw-MD5" options to display all of the cracked passwords reliably
Session completed. 
```

Ah hah! There we are, `Jok3r`.

Trying this as the answer
#### ✅ Answer
- `Jok3r` ✅


## 📘Custom Rules
### Custom Rules in John the Ripper
- **Purpose**:
  - Define personalized mangling patterns to generate password guesses.
  - Useful when you know likely password structures or complexity requirements.
### Why Use Custom Rules?
- **Password Complexity Requirements**:
  - Many systems require:
    - Lowercase letters
    - Uppercase letters
    - Numbers
    - Symbols
  - Users often follow predictable patterns (e.g., `Polopassword1!`).
- **Exploiting Predictability**:
  - Attackers can define rules to mimic common user behavior (e.g., capitalizing the first letter, appending numbers and symbols).
### Creating Custom Rules
- **Location**:
  - Typically found in:
    - `/opt/john/john.conf` (TryHackMe AttackBox)
    - `/etc/john/john.conf` (standard installations)
- **Syntax Overview**:
  ```plaintext
  [List.Rules:RuleName]
  rule_definition
  ```
- **Common Modifiers**:
  - `Az`: Append characters
  - `A0`: Prepend characters
  - `c`: Capitalize character(s)
- **Character Sets**:
  - `[0-9]`: Digits
  - `[A-Z]`: Uppercase letters
  - `[a-z]`: Lowercase letters
  - `[!£$%@]`: Symbols
  - `[a]`: Specific character
- **Example Rule**:
  ```plaintext
  [List.Rules:PoloPassword]
  cAz"[0-9][!£$%@]"
  ```
  - Capitalizes the first letter
  - Appends a digit and a symbol
### Using Custom Rules
- **Command Syntax**:
  ```bash
  john --wordlist=[path to wordlist] --rule=PoloPassword [path to file]
  ```
- **Tips**:
  - Talk through the pattern like writing a RegEx.
  - Refer to existing rules in `john.conf` (around line 678) for more examples and troubleshooting.


### ❓ Question - What do custom rules allow us to exploit?
#### 🧪 Process
> This pattern can let us exploit **password complexity predictability**.

Trying this as the answer
#### ✅ Answer
- `password complexity predictability` ✅
### ❓ Question - What rule would we use to add all capital letters to the end of the word?
#### 🧪 Process
There are a few bits here that can help.

* Add **all capitals**: `[A-Z]`
	* `[A-Z]`: Will include only uppercase letters
* to the **End** of the word: `Az`
	* `Az`: Appends to the end of the word
* The pattern needs to be quoted `" "`.

So putting this together we have `Az"[A-Z]"`

Trying this as the answer
#### ✅ Answer
- `Az"[A-Z]"` ✅
### ❓ Question - What flag would we use to call a custom rule called `THMRules`?
#### 🧪 Process
> We could then call this custom rule a John argument using the  `--rule=PoloPassword` flag.

So if we swap out `PoloPassword` for `THMRules` we get `--rule=THMRules`

Trying this as the answer
#### ✅ Answer
- `--rule=THMRules` ✅


## 📘Cracking Password Protected Zip Files
### Cracking Zip Files
#### Overview
John the Ripper can crack password-protected Zip files by first converting them into a compatible hash format using a helper tool. This process allows John to attempt brute-force or dictionary-based attacks on the extracted hash.
#### Tool: `zip2john`
* Converts a `.zip` file into a format that John the Ripper can process.
* Syntax:
  ```
  zip2john [options] [zip file] > [output file]
  ```
  * **\[options]**: Optional checksum parameters (rarely needed).
  * **\[zip file]**: Target archive to extract the hash from.
  * **> \[output file]**: Redirects the hash output to a file.
* **Example**:
  ```bash
  zip2john zipfile.zip > zip_hash.txt
  ```
#### Cracking with John
* Once the hash is extracted, it can be cracked using John with a wordlist.
* **Example**:
  ```bash
  john --wordlist=/usr/share/wordlists/rockyou.txt zip_hash.txt
  ```

### ❓ Question 1 - What is the password for the secure.zip file?
#### 🧪 Process
Okay, context, we're working in `Task09` now.

Let's move into `Task09` and `ls` the contents.
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task09$ ls -la
total 12
drwxr-xr-x 2 user user 4096 Oct 10  2024 .
drwxr-xr-x 9 user user 4096 Oct 10  2024 ..
-rw-r--r-- 1 user user  232 Oct 10  2024 secure.zip
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task09$ 
```

We just have the `secure.zip` file. We're going to need to enlist `zip2john` to get the has. 
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task09$ zip2john secure.zip > hash.txt
ver 1.0 efh 5455 efh 7875 secure.zip/zippy/flag.txt PKZIP Encr: 2b chk, TS_chk, cmplen=38, decmplen=26, crc=849AB5A6 ts=B689 cs=b689 type=0
```

I don't see any errors, if we cat the `hash.txt` we can see what was created.
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task09$ cat hash.txt 
secure.zip/zippy/flag.txt:$pkzip$1*2*2*0*26*1a*849ab5a6*0*48*0*26*b689*964fa5a31f8cefe8e6b3456b578d66a08489def78128450ccf07c28dfa6c197fd148f696e3a2*$/pkzip
$:zippy/flag.txt:secure.zip::secure.zip
```

Going by the guide, we don't need to do anything with this and can pass it directly to John
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task09$ john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt 
Using default input encoding: UTF-8
Loaded 1 password hash (PKZIP [32/64])
Will run 2 OpenMP threads
Note: Passwords longer than 21 [worst case UTF-8] to 63 [ASCII] rejected
Press 'q' or Ctrl-C to abort, 'h' for help, almost any other key for status
pass123          (secure.zip/zippy/flag.txt)     
1g 0:00:00:00 DONE (2025-06-12 14:57) 50.00g/s 409600p/s 409600c/s 409600C/s newzealand..total90
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```

We have our password (`pass123`).

Trying this as the answer
#### ✅ Answer
- `pass123` ✅
### ❓ Question 2 - What is the contents of the flag inside the zip file?
#### 🧪 Process
We now have a password (`pass123`). We need to work out how to unzip an `zip` archive. Not something I've ever done in linux. 

I wonder if I could just `unzip`?
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task09$ unzip
UnZip 6.00 of 20 April 2009, by Debian. Original by Info-ZIP.

Usage: unzip [-Z] [-opts[modifiers]] file[.zip] [list] [-x xlist] [-d exdir]
  Default action is to extract files in list, except those in xlist, to exdir;
  file[.zip] may be a wildcard.  -Z => ZipInfo mode ("unzip -Z" for usage).

  -p  extract files to pipe, no messages     -l  list files (short format)
  -f  freshen existing files, create none    -t  test compressed archive data
  -u  update files, create if necessary      -z  display archive comment only
  -v  list verbosely/show version info       -T  timestamp archive to latest
  -x  exclude files that follow (in xlist)   -d  extract files into exdir
modifiers:
  -n  never overwrite existing files         -q  quiet mode (-qq => quieter)
  -o  overwrite files WITHOUT prompting      -a  auto-convert any text files
  -j  junk paths (do not make directories)   -aa treat ALL files as text
  -U  use escapes for all non-ASCII Unicode  -UU ignore any Unicode fields
  -C  match filenames case-insensitively     -L  make (some) names lowercase
  -X  restore UID/GID info                   -V  retain VMS version numbers
  -K  keep setuid/setgid/tacky permissions   -M  pipe through "more" pager
  -O CHARSET  specify a character encoding for DOS, Windows and OS/2 archives
  -I CHARSET  specify a character encoding for UNIX and other archives

See "unzip -hh" or unzip.txt for more help.  Examples:
  unzip data1 -x joe   => extract all files except joe from zipfile data1.zip
  unzip -p foo | more  => send contents of foo.zip via pipe into program more
  unzip -fo foo ReadMe => quietly replace existing ReadMe if archive file newer
```

Okay, magic. What happens if we just `unzip secure.zip`
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task09$ unzip secure.zip 
Archive:  secure.zip
[secure.zip] zippy/flag.txt password: 
 extracting: zippy/flag.txt
```

After providing the cracked password it worked. 

We should be able to just `cat` the flag file.
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task09$ cat zippy/flag.txt 
THM{w3ll_d0n3_h4sh_r0y4l}
```

And, there, we, are. The flag (`THM{w3ll_d0n3_h4sh_r0y4l}`) has been discovered.

Trying this as the answer
#### ✅ Answer
- `THM{w3ll_d0n3_h4sh_r0y4l} ✅`


## 📘Cracking Password-Protected RAR Archives
### Cracking RAR Files
#### Overview
RAR archives, like Zip files, can be password-protected. John the Ripper can crack these passwords by converting the archive into a hash format it understands using a helper tool.
#### Tool: `rar2john`
* Converts a `.rar` archive into a John-compatible hash format.
* Syntax:
  ```Shell
  rar2john [rar file] > [output file]
  ```
  * **rar2john**: Calls the hash extraction tool.
  * **\[rar file]**: Path to the target RAR archive.
  * **> \[output file]**: Redirects hash output to a specified file.
* **Example**:
  ```Shell
  /opt/john/rar2john rarfile.rar > rar_hash.txt
  ```

#### Cracking with John
* The generated hash file can then be cracked using a wordlist in John.
* **Example**:
  ```Shell
  john --wordlist=/usr/share/wordlists/rockyou.txt rar_hash.txt
  ```
### ❓ Question 1 - What is the password for the secure.rar file?
#### 🧪 Process
Lets start by changing into the `Task10` directory and listing its contents.
```Shell
user@ip-10-10-197-123:~$ cd John-the-Ripper-The-Basics/Task10
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task10$ ls -la
total 12
drwxr-xr-x 2 user user 4096 Oct 10  2024 .
drwxr-xr-x 9 user user 4096 Oct 10  2024 ..
-rw-r--r-- 1 user user  270 Oct 10  2024 secure.rar
```

Okay, just the `secure.rar` file.  Next we want to get our hash by using `rar2john`.
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task10$ rar2john secure.rar > hash.txt
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task10$ ls -la
total 16
drwxr-xr-x 2 user user 4096 Jun 12 15:12 .
drwxr-xr-x 9 user user 4096 Oct 10  2024 ..
-rw-rw-r-- 1 user user  108 Jun 12 15:12 hash.txt
-rw-r--r-- 1 user user  270 Oct 10  2024 secure.rar
```

The output file `hash.txt` has been created, great. Lets `cat` the contents, just out of curiosity of course.
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task10$ cat hash.txt 
secure.rar:$rar5$16$b7b0ffc959b2bc55ffb712fc0293159b$15$4f7de6eb8d17078f4b3c0ce650de32ff$8$ebd10bb79dbfb9f8
```

Lets John it up
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task10$ john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt 
Using default input encoding: UTF-8
Loaded 1 password hash (RAR5 [PBKDF2-SHA256 256/256 AVX2 8x])
Cost 1 (iteration count) is 32768 for all loaded hashes
Will run 2 OpenMP threads
Note: Passwords longer than 10 [worst case UTF-8] to 32 [ASCII] rejected
Press 'q' or Ctrl-C to abort, 'h' for help, almost any other key for status
password         (secure.rar)     
1g 0:00:00:00 DONE (2025-06-12 15:14) 1.667g/s 106.7p/s 106.7c/s 106.7C/s 123456..charlie
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```

_in a dodgy french accent_ "Not long later".

Trying this as the answer
#### ✅ Answer 1
- `password` ✅







### ❓ Question 2 - What are the contents of the flag inside the zip file?
#### 🧪 Process
We know our password is super secret `password` now. We can unrar the rar file. `unrar` maybe?
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task10$ unrar

UNRAR 7.00 freeware      Copyright (c) 1993-2024 Alexander Roshal

Usage:     unrar <command> -<switch 1> -<switch N> <archive> <files...>
               <@listfiles...> <path_to_extract/>

<Commands>
  e             Extract files without archived paths
  l[t[a],b]     List archive contents [technical[all], bare]
  p             Print file to stdout
  t             Test archive files
  v[t[a],b]     Verbosely list archive contents [technical[all],bare]
  x             Extract files with full path

<Switches>
  -             Stop switches scanning
  @[+]          Disable [enable] file lists
  ad[1,2]       Alternate destination path
  ag[format]    Generate archive name using the current date
  ai            Ignore file attributes
  ap<path>      Set path inside archive
  c-            Disable comments show
  cfg-          Disable read configuration
  cl            Convert names to lower case
  cu            Convert names to upper case
  dh            Open shared files
  ep            Exclude paths from names
  ep3           Expand paths to full including the drive letter
  ep4<path>     Exclude the path prefix from names
  f             Freshen files
  id[c,d,n,p,q] Display or disable messages
  ierr          Send all messages to stderr
  inul          Disable all messages
  kb            Keep broken extracted files
  me[par]       Set encryption parameters
  n<file>       Additionally filter included files
  n@            Read additional filter masks from stdin
  n@<list>      Read additional filter masks from list file
  o[+|-]        Set the overwrite mode
  ol[a,-]       Process symbolic links as the link [absolute paths, skip]
  op<path>      Set the output path for extracted files
  or            Rename files automatically
  ow            Save or restore file owner and group
  p[password]   Set password
  r             Recurse subdirectories
  sc<chr>[obj]  Specify the character set
  si[name]      Read data from standard input (stdin)
  sl<size>[u]   Process files with size less than specified
  sm<size>[u]   Process files with size more than specified
  ta[mcao]<d>   Process files modified after <d> YYYYMMDDHHMMSS date
  tb[mcao]<d>   Process files modified before <d> YYYYMMDDHHMMSS date
  tn[mcao]<t>   Process files newer than <t> time
  to[mcao]<t>   Process files older than <t> time
  ts[m,c,a,p]   Save or restore time (modification, creation, access, preserve)
  u             Update files
  v             List all volumes
  ver[n]        File version control
  vp            Pause before each volume
  x<file>       Exclude specified file
  x@            Read file names to exclude from stdin
  x@<list>      Exclude files listed in specified list file
  y             Assume Yes on all queries
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task10$ 
```

Oh wow, lots of options. The first under `<Commands>` is `e`. Worth a try maybe?
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task10$ unrar e secure.rar 

UNRAR 7.00 freeware      Copyright (c) 1993-2024 Alexander Roshal

Enter password (will not be echoed) for secure.rar: 


Extracting from secure.rar

Extracting  flag.txt                                                  OK 
All OK
```

Okay we can cat `flag.txt` now to get our answer.
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task10$ cat flag.txt 
THM{r4r_4rch1ve5_th15_t1m3}
```

And there is our actual flag. 

Trying this as the answer
#### ✅ Answer 2
- `THM{r4r_4rch1ve5_th15_t1m3}` ✅


## 📘Cracking SSH Keys with John
### Cracking SSH Key Passwords
#### Overview
SSH private keys (`id_rsa`) can be password-protected. John the Ripper can be used to crack these passwords, which may allow access to remote machines if key-based authentication is enabled.
#### Tool: `ssh2john`
* Converts an SSH private key file into a John-compatible hash format.
* Syntax:
  ```Shell
  ssh2john [id_rsa private key file] > [output file]
  ```
  * **ssh2john**: Invokes the conversion tool.
  * **\[id\_rsa private key file]**: Path to the target SSH key file.
  * **> \[output file]**: Redirects hash output to a file.
* **Alternative usage** (if `ssh2john` is not directly available):
  * On **AttackBox**:
    ```Shell
    python3 /opt/john/ssh2john.py id_rsa > id_rsa_hash.txt
    ```
  * On **Kali**:
    ```Shell
    python /usr/share/john/ssh2john.py id_rsa > id_rsa_hash.txt
    ```

#### Cracking with John
* Use the generated hash file with a wordlist to attempt cracking the SSH key password.
* **Example**:
  ```Shell
  john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa_hash.txt
  ```

### ❓ Question - What is the SSH private key password?
#### 🧪 Process
We'll use the `ssh2john` tool to convert the hash
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task11$ python3 /opt/john/ssh2john.py id_rsa > hash.txt
```

Now we can cat the hash.
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task11$ cat hash.txt 
id_rsa:$sshng$1$16$3A98F468854BB3836BF689310D864CE9$1200$08ca19b68bc606b07875701174131b9220d23ef968befc1230eeff0d7c0f904e6734765fe562e8671972e409091f32c80b
754ab248976228a5f2c38e8ac63572d7452e75669aeda932275989ce4c077d43287ed227b8f9053e53f2b1c9bb9dfe876378a32e87e7be4e91a845ae8ee4073bf7ac5aad8414253c97cfb73b083
107712907da8c704678f46d0b006f7a77b13a04305a988c8e17d83abd2449ed5c3defc8203d7c5f70cef3470b0bbe3fa5a2e957ac55a57ea08b1de4d3fa5436c6160a14b461ac7bc4a3052ddf85
8de657ecb210989507beb96f7219ac3c3790e89f3af71f7f61ebe23570284a482b1504b067fb1e03ed62201c6db71dab65e5f1577751ddb006fe14ceed4525965fce19f8141373094d1aedbb58c
b903f58f6d80695be0382c31e61baaf366d4f2e722316e91ff4dcb3df15702008b5be3c0b2a81b3f452ef3257c425dd26119324b4de3652e90b91afd87ca2bc41c70abd0d97557d4037952b63c0
a0d7c7ab6ed538c3d76bdf488683213e8d8e897ab51c4990b137d04e5044ccbf8cadbdce9eeec5e50f3d487b1f21e86a2b2785caedbf9503d2d8585b2138d82b35e70d1da03c9c574962cdb6e4d
2de761a594ab8c082d88b43a027649012feb28b6a022c0ab49cf05e8b91e36bda935f188c1bb05925da2168dd15af917ba20a8532010892853da5cb1a8ff80cc5d3aa1dd3fe66543bf14d9b44d0
82261fd61976718bb5eea1d911ddb7fb0cc0505b39cec36ef7bd8e8d9d826eda5f7e1a5a51067ead2f78cf69f85de97be5a8f371174356788554b6bf134072b93bf6728ec26fe19c2485be9e742
8208a66cc1e79329ac16f3034605c63550a424ed8cac39f965b6ffe83240c6709607eaef99b189100ef33e000b4195e07ec5c67bdaf2ca1acbd08327f0c4dcfae322883f7be964cb22393541e88
3c8c5b748237a900aab709b6286cea66a214a9fe4e3a1203f999fd995aa049767355e2658828c4a82d58ca15343f0abe6b2779e880ed2682b4730103a84a3410e6c822098d82b04d665b8bf98bc
3b69cae0c8d8c9d140dc99056279d5f330bc439bfdceaf38a56fd1362ce78e96deb49a9f6756ec9b64eeba8f4725ec056ab206e37823d052d539d38016abf792858a169cbbe0f6f0d0049c6d492
28833aa8ec10ede0c183ac737e54346949485e5ffc1bc3105e5686c8b1f6fb8cdb14949aa97b833757d02b970e96cb1281c472a5cb26cfa7cfda0be5bd45cf14d4bc28ccd2be4dd09c6a2ce0cf6
68035d2aa39a8345ea154543491436bf8f5e605d86e266d40227f48684e696a225877624ddddf0afe05d0aeec29ad28edb0f8cda0f341ddbbd454bd3c238d1c499effa6bf6f796dae0983182f36
fae4781552cb8d9426fc57132c27a735c5365a5d355a4c4f21d5d7ed2ea11bb2ed856156390673545418f47359fd1092767f6dfb3321ee14a0043352fdbaa5cb0e75fde2ec5909c0533251f30bd
56ad7e34a8a31b009b53c64e9f2de9fd57a0f561564e6a961158cc0b54fcfc43046d9641788ac5e25b89bdb7890c4e6532d1bfabd4d49ae7d3740506c4ecb6bc5cb6a12bc24ed2e0f913338c7df
a08ada66a6128e7de56794d1d2170d6324af0cd72bc8abcff177f0942d9df5d99d48c8f946fd856d9ccb424966835aa06c94996abcc169aef6f246bbbd7489ec026a
```

Not quite what I was expecting...

We can try `John` quickly, but I suspect this won't work
```Shell
user@ip-10-10-197-123:~/John-the-Ripper-The-Basics/Task11$ john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt 
Using default input encoding: UTF-8
Loaded 1 password hash (SSH, SSH private key [RSA/DSA/EC/OPENSSH 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 1 for all loaded hashes
Will run 2 OpenMP threads
Note: Passwords longer than 10 [worst case UTF-8] to 32 [ASCII] rejected
Press 'q' or Ctrl-C to abort, 'h' for help, almost any other key for status
mango            (id_rsa)     
1g 0:00:00:00 DONE (2025-06-12 15:59) 50.00g/s 214400p/s 214400c/s 214400C/s praise..mango
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```

I'm blown away it was that easy. Okay we have our password (`mango`).

Trying this as the answer
#### ✅ Answer
- `mango` ✅


## 📘Further Reading
### Conclusion
#### Summary
This room has introduced the core principles of using John the Ripper, including:
* Converting protected files into hash formats
* Cracking those hashes using a wordlist
The consistent workflow can now be applied to a wide range of supported hash types.

#### Further Learning
For continued learning, updates, and in-depth usage tips, refer to the official John the Ripper wiki:
* **Openwall Wiki**: [https://openwall.info/wiki/john](https://openwall.info/wiki/john)