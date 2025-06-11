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


## 📘 Introduction
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


## 📘 Basic Terms
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


## Cracking Basic Hashes
Certainly! Here's the **functional summary** of the John the Ripper usage guide, formatted with `###` indentation:

---

### Using John the Ripper to Crack Hashes

- **Basic Syntax**:
  ```bash
  john [options] [file path]
  ```
  - `john`: Launches the tool.
  - `[options]`: Flags and parameters for the cracking process.
  - `[file path]`: File containing the hash(es) to crack.

### Automatic Cracking

- **Syntax**:
  ```bash
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
  ```bash
  john --format=[format] --wordlist=[path to wordlist] [path to file]
  ```
  - `--format=`: Specifies the hash format explicitly.
  - Example:
    ```bash
    john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt
    ```

- **Note on Formats**:
  - Prefix standard hash types with `raw-` (e.g., `raw-md5`).
  - To list all supported formats:
    ```bash
    john --list=formats
    ```
  - To search for a specific format:
    ```bash
    john --list=formats | grep -iF "md5"
    ```

---

Would you like me to now compile all the summaries into a single document or export them as a file for you?








































## Cracking Windows Authentication Hashes
Certainly! Here's the **functional summary** of the section on cracking authentication hashes, formatted with `###` indentation:

---

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

---

Would you like me to compile all the summaries into a single document or export them as a file now?








































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
