| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **John the Ripper: The Basics** |

# !!!! R E M O V E 　 O N C E 　 C O M P L E T E !!!!
### ❓ Question
#### 🧪 Process
#### ✅ Answer

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


## Cracking /etc/shadow Hashes
Certainly! Here's the **functional summary** of the section on cracking hashes from `/etc/shadow`, formatted with `###` indentation:

---

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

---

Would you like me to now compile all the summaries into a single document or export them as a file for you?








































## Single Crack Mode
Certainly! Here's the **functional summary** of the section on John the Ripper's Single Crack mode, formatted with `###` indentation:

---

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

---

Would you like me to now compile all the summaries into a single document or export them as a file for you?









































## Custom Rules
Absolutely! Here's the **functional summary** of the section on custom rules in John the Ripper, formatted with `###` indentation:

---

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

---

Would you like me to now compile all the summaries into a single document or export them as a file for you?








































## Cracking Password Protected Zip Files
Here’s the functional summary for the **Zip File Cracking** section:

---

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






































## Cracking Password-Protected RAR Archives
Here’s your functional summary for the **RAR File Cracking** section:

---

### Cracking RAR Files

#### Overview

RAR archives, like Zip files, can be password-protected. John the Ripper can crack these passwords by converting the archive into a hash format it understands using a helper tool.

#### Tool: `rar2john`

* Converts a `.rar` archive into a John-compatible hash format.

* Syntax:

  ```
  rar2john [rar file] > [output file]
  ```

  * **rar2john**: Calls the hash extraction tool.
  * **\[rar file]**: Path to the target RAR archive.
  * **> \[output file]**: Redirects hash output to a specified file.

* **Example**:

  ```bash
  /opt/john/rar2john rarfile.rar > rar_hash.txt
  ```

#### Cracking with John

* The generated hash file can then be cracked using a wordlist in John.
* **Example**:

  ```bash
  john --wordlist=/usr/share/wordlists/rockyou.txt rar_hash.txt
  ```



































## Cracking SSH Keys with John
Here’s the functional summary for the **SSH Key Cracking** section:

---

### Cracking SSH Key Passwords

#### Overview

SSH private keys (`id_rsa`) can be password-protected. John the Ripper can be used to crack these passwords, which may allow access to remote machines if key-based authentication is enabled.

#### Tool: `ssh2john`

* Converts an SSH private key file into a John-compatible hash format.

* Syntax:

  ```
  ssh2john [id_rsa private key file] > [output file]
  ```

  * **ssh2john**: Invokes the conversion tool.
  * **\[id\_rsa private key file]**: Path to the target SSH key file.
  * **> \[output file]**: Redirects hash output to a file.

* **Alternative usage** (if `ssh2john` is not directly available):

  * On **AttackBox**:

    ```bash
    python3 /opt/john/ssh2john.py id_rsa > id_rsa_hash.txt
    ```
  * On **Kali**:

    ```bash
    python /usr/share/john/ssh2john.py id_rsa > id_rsa_hash.txt
    ```

#### Cracking with John

* Use the generated hash file with a wordlist to attempt cracking the SSH key password.
* **Example**:

  ```bash
  john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa_hash.txt
  ```



































## Further Reading
Here’s the final functional summary for the **Conclusion** section:

---

### Conclusion

#### Summary

This room has introduced the core principles of using John the Ripper, including:

* Converting protected files into hash formats
* Cracking those hashes using a wordlist

The consistent workflow can now be applied to a wide range of supported hash types.

#### Further Learning

For continued learning, updates, and in-depth usage tips, refer to the official John the Ripper wiki:

* **Openwall Wiki**: [https://openwall.info/wiki/john](https://openwall.info/wiki/john)
