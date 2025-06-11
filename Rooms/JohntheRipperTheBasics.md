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

Page Section
## Introduction








Absolutely! Here's the **functional summary** of the final text, formatted with `###` indentation:

---

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
  ```bash
  echo "TryHackMe" | base64
  # Output: VHJ5SGFja01lCg==
  
  echo "VHJ5SGFja01lCg==" | base64 -d
  # Output: TryHackMe
  ```

- **Encryption**:
  - Protects data confidentiality using a cipher and a key.
  - **Reversible**: only with the correct key and algorithm.
  - Covered in previous modules.

### Next Steps

- Continue with **John the Ripper** for practical password cracking.
- For deeper cryptography knowledge, explore the recommended cryptography-focused room.

---

Would you like me to now compile all these summaries into a single document or export them as a file for you?































## Basic Terms

Certainly! Here's the **functional summary** of the final text, formatted with `###` indentation:

---

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
  - **NP**: Problems where solutions can be verified quickly, but not easily found (e.g., reversing a hash).

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
  - **Cryptography Basics**
  - **Public Key Cryptography Basics**
  - **Hashing Basics**

---

Would you like me to now compile all the summaries into a single document or export them as a file for you?










## Setting Up Your System
Certainly! Here's the **functional summary** of the installation and setup text, formatted with `###` indentation:

---

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
    ```bash
    sudo dnf install john
    ```
  - **Ubuntu**:  
    ```bash
    sudo apt install john
    ```
  - These may install the core version, which lacks tools like `zip2john` and `rar2john`.

- **To Access Full Features**:
  - Build **Jumbo John** from source using the official installation guide.

- **Windows**:
  - Download the appropriate zipped binary (64-bit or 32-bit).
  - Extract and install manually.

### Wordlists

- **Purpose**:
  - Used in dictionary attacks to compare hashed words against target hashes.

- **Common Sources**:
  - **/usr/share/wordlists/** on AttackBox and Kali Linux.
  - **SecLists repository** for a wide variety of wordlists.

- **rockyou.txt**:
  - Used throughout this room.
  - Originated from a 2009 data breach (rockyou.com).
  - If not pre-installed, download from SecLists under `/Passwords/Leaked-Databases/`.
  - Extract with:
    ```bash
    tar xvzf rockyou.txt.tar.gz
    ```

---

Would you like me to now compile all the summaries into a single document or export them as a file for you?








































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

Zip2John
Similarly to the unshadow tool we used previously, we will use the zip2john tool to convert the Zip file into a hash format that John can understand and hopefully crack. The primary usage is like this:

zip2john [options] [zip file] > [output file]

[options]: Allows you to pass specific checksum options to zip2john; this shouldn’t often be necessary
[zip file]: The path to the Zip file you wish to get the hash of
>: This redirects the output from this command to another file
[output file]: This is the file that will store the output
Example Usage

zip2john zipfile.zip > zip_hash.txt

Cracking
We’re then able to take the file we output from zip2john in our example use case, zip_hash.txt, and, as we did with unshadow, feed it directly into John as we have made the input specifically for it.

john --wordlist=/usr/share/wordlists/rockyou.txt zip_hash.txt








































## Cracking Password-Protected RAR Archives
## Cracking SSH Keys with John
## Further Reading
