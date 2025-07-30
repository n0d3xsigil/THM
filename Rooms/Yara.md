| [Home](../README.md) | [SOC Level 1](../SOClevel1.md) | **Yara** |

# Yara

## Contents
- [Introduction](#introduction)
- [What is Yara?](#what-is-yara)
- [Deploy](#deploy)
- [Introduction to Yara Rules](#introduction-to-yara-rules)
- [Expanding on Yara Rules](#expanding-on-yara-rules)
- [Yara Modules](#yara-modules)
- [Other tools and Yara](#other-tools-and-yara)
- [Using LOKI and its Yara rule set](#using-loki-and-its-yara-rule-set)
- [Creating Yara rules with yarGen](#creating-yara-rules-with-yarGen)



## 📘Introduction

YARA the pattern recognition tool to aid security personnel analyse potential malware and categorise within a specific rulesets



## 📘What is Yara?

YARA standing for _**Y**et **A**nother **R**idiculous **A**cronym_ is _"The pattern matching swiss knife for malware researchers (and everyone else)"_.

YARA is a tool that will perform pattern matching against a particular rule.

_**Example rule:**_
```yara
rule Hello_World
{
    meta:
        description = "Flags any sign of the world famous 'Hello World'"
        author = "n0d3xsigil"
    strings:
        $s1 = "Hello World"
        $s2 = "Hello_World"
        $s3 = "HelloWorld"
        $hex1 = { 4D 5A 90 00 }
    condition:
        $s1 or $s2 or $s3 and $hex1
}
```

- **YARA** is a pattern-matching tool widely used in **malware research**, capable of identifying binary and textual patterns (e.g. hex, strings) in files.
- **YARA rules** are used to detect threats by defining specific patterns (features) found in malicious files.
- **Strings** are a core part of programming, used to store text — malware also uses them to store critical data.
- Examples of string data in malware:
  - **Ransomware**: Bitcoin wallet addresses (e.g. `12t9YDPg...`)
  - **Botnets**: Command & Control (C\&C) server IPs (e.g. `12.34.56.7`)
  - You could write a YARA rule to find programs containing a specific string like `"hello world"` across your system.

### ❓ Question 1

> What is the name of the base-16 numbering system that Yara can detect?

#### 🧪 Process

`hexadecimal` is 16-bit (0123456789ABCDEF).

Trying this as the answer

#### ✅ Answer 1

- `hexadecimal` ✅


### ❓ Question 2

> Would the text "Enter your Name" be a string in an application? (Yay/Nay)

#### 🧪 Process

Any text would be a string so `Yay`.

Trying this as as the answer


#### ✅ Answer

- `Yay` ✅



## 📘Deploy

This is just to deploy the VM.


## 📘Introduction to Yara Rules


### Your First YARA Rule – Summary

- **YARA rules** are written in a simple proprietary language easy to start, harder to master.
    - To **run YARA**, you need:
      1. A `.yar` rule file
      2. A target (file, directory, or process ID)
        - Example: `yara myrule.yar somedirectory`


### Basic Rule Creation Steps

1. **Create a test file** `touch somefile`
2. **Create a rule file** `touch myfirstrule.yar`
3. **Edit the rule file with this content**
   ```yara
   rule example_rule {
       condition: true
   }
   ```
   - `example_rule` = name of the rule
   - `condition: true` = always matches if the file existsatlantis2k
4. **Run the rule**
   ```bash
   yara myfirstrule.yar somefile
   ```
   - If `somefile` exists, you'll see `examplerule somefile`
   - If it doesn't, you'll get an error (e.g. "could not open file")



## 📘Expanding on Yara Rules



### YARA Conditions & Rule Structure – Summary

* **Basic Structure** of a YARA rule includes:

  * `meta` - Optional. Used for descriptive info like `desc = "Detects Hello World string"`.
  * `strings` - Defines patterns to search (e.g. text, hex) using variables like `$varname`.
  * `condition` - Required. Logic that determines when the rule triggers.



### String Matching Examples

- **Single string match**:
  ```yara
  rule helloworld_checker {
    strings:
      $hello_world = "Hello World!"
    condition:
      $hello_world
  }
  ```
  - Matches only exact string: `"Hello World!"`

- **Multiple case-insensitive matches**:
  ```yara
  rule helloworld_checker {
    strings:
      $a = "Hello World!"
      $b = "hello world"
      $c = "HELLO WORLD"
    condition:
      any of them
  }
  ```
  - Triggers if *any* of the defined strings are found.



### Using Operators & File Properties
- Example with **occurrence count**:
  ```yara
  rule count_limiter {
    strings:
      $a = "Hello World!"
    condition:
      #a <= 10
  }
  ```
  - Only matches if `"Hello World!"` appears 10 times or fewer.

- Example with **combined conditions**:
  ```yara
  rule size_and_string_check {
    strings:
      $a = "Hello World!"
    condition:
      $a and filesize < 10KB
  }
  ```
  - Matches only if the file has `"Hello World!"` *and* is smaller than 10KB.



### Tips

- Combine conditions using logical operators: `and`, `or`, `not`.
- Use **`any of them`** or **`all of them`** to simplify matching multiple strings.
- Refer to the **fr0gger\_ YARA cheatsheet** for a visual breakdown of rule components.

<img width="875" height="1212" alt="image" src="https://github.com/user-attachments/assets/1c10fd4a-80f9-4f8f-aa39-7dc5c68e89cf" />



## 📘Yara Modules

**YARA can integrate with other tools/libraries** to enhance rule precision and depth of analysis.



#### Cuckoo Sandbox Module

- **[Cuckoo Sandbox](https://github.com/cuckoosandbox/cuckoo)** is an automated malware analysis system.
- The YARA module allows you to **generate rules based on dynamic behaviour** (e.g. runtime strings).
- Useful for detecting malware by how it behaves during execution.



#### Python PE Module

- Leverages Python's **[PE (Portable Executable) module](https://pypi.org/project/pefile/)** to write rules based on Windows file structure.
- Targets elements within `.exe` and `.dll` files (common for malware).
- Useful for detecting:
    - Use of **cryptography libraries**
    - Signs of **worming or self-replication**
- Allows rule creation **without needing to run or reverse-engineer** the sample.



## 📘Other tools and Yara

**You don't need to write every rule yourself** — many open-source tools and public YARA rule sets exist to speed up detection and threat hunting.

**Download**: [https://github.com/InQuest/awesome-yara](https://github.com/InQuest/awesome-yara)



#### LOKI

- A **free IOC scanner** by Florian Roth.
- Uses multiple detection methods:
    - Filename IOC
    - **YARA rule checks**
    - Hash comparisons
    - C2 backconnect checks
- Cross-platform (Windows & Linux).
- CLI-based, with many scan options and flags.
- Ideal for incident response and lightweight scanning.

**Download**: [https://github.com/Neo23x0/Loki](https://github.com/Neo23x0/Loki)



#### THOR Lite

- A **multi-platform IOC and YARA scanner** (free version of THOR).
- Offers **precompiled binaries** for Windows, Linux, macOS.
- Features **CPU-friendly scan throttling**.
- Requires email sign-up for download.
- Targeted at corporate environments but accessible for individuals via THOR Lite.

**Download**: [https://www.nextron-systems.com/thor-lite/](https://www.nextron-systems.com/thor-lite/).



#### FENRIR

- A **simple Bash script IOC checker** by the same author (Florian Roth).
- Runs on **any system with Bash** (Linux, macOS, Windows WSL).
- No dependencies or setup required.
- Good for quick, portable IOC scans.

**Download**: [https://github.com/Neo23x0/Fenrir](https://github.com/Neo23x0/Fenrir).



#### YAYA (Yet Another Yara Automaton)

- Created by the **Electronic Frontier Foundation (EFF)**.
- Helps manage and run multiple **YARA rulesets**.
- Features:
    - Import existing rulesets
    - Add/remove custom rules
    - Run scans on directories
**Linux-only** at present.

**Download**: [https://github.com/EFForg/yaya](https://github.com/EFForg/yaya)



## 📘Using LOKI and its Yara rule set

- **LOKI** is a threat-hunting tool that combines built-in YARA rules and IOCs to scan for known malicious activity.
- It's ideal for **threat intelligence-based detection** and **incident response**, especially when:
    - You gather new IOCs from reports or blogs.
    - Your current security tools fail to detect something suspicious.




### ❓ Question 1

> Scan file 1. Does Loki detect this file as suspicious/malicious or benign?

#### 🧪 Process

First I want to see what is in the home folder (`~`). Issue the `ls -l` command to show the results in the list format.

```bash
cmnatic@ip-10-10-203-222:~$ ls -l
total 20
drwxrwxr-x 4 cmnatic cmnatic 4096 Nov  5  2020 go
-rw-rw-r-- 1 cmnatic cmnatic   45 Jul 27 15:40 myfirstrule.yar
drwxrwxr-x 4 cmnatic cmnatic 4096 Jul 27 15:47 suspicious-files
drwxrwxr-x 4 cmnatic cmnatic 4096 Nov  9  2020 tools
drwxrwxr-x 9 cmnatic cmnatic 4096 Nov  6  2020 yara-python
```

Okay so we `suspicious-files`. Let's `cd` into it and see what the contents are.

```bash
cmnatic@ip-10-10-203-222:~$ cd suspicious-files/; ls -l
total 8
drwxrwxr-x 2 cmnatic cmnatic 4096 Jul 27 15:42 file1
drwxrwxr-x 2 cmnatic cmnatic 4096 Nov 10  2020 file2
```

We have two folders, `file1` and `file2`. Let's issue `ls -l` on both files

```bash
cmnatic@ip-10-10-203-222:~/suspicious-files$ ls -l file*
file1:
total 84
-rw-rw-r-- 1 cmnatic cmnatic 80992 Nov  9  2020 ind3x.php
-rw-rw-r-- 1 cmnatic cmnatic  3601 Jul 27 15:43 loki_ip-10-10-32-16_2025-07-27_15-42-59.log

file2:
total 220
-rw-rw-r-- 1 cmnatic cmnatic 223978 Nov  9  2020 1ndex.php
cmnatic@ip-10-10-203-222:~/suspicious-files$
```


We can see we have `ind3x.php`, `1ndex.php`, and a log file (`loki_ip-10-10-32-16_2025-07-27_15-42-59.log`)

I wonder what the log file is, we can `cat` that out.

```bash
cmnatic@ip-10-10-203-222:~/suspicious-files$ cat file1/loki_ip-10-10-32-16_2025-07-27_15-42-59.log 
20250727T15:42:59Z ip-10-10-32-16 LOKI: Notice: MODULE: Init MESSAGE: Starting Loki Scan VERSION: 0.32.1 SYSTEM: ip-10-10-32-16 TIME: 20250727T15:42:59Z PLATFORM:     PROC: x86_64 ARCH: 64b
it 
20250727T15:42:59Z ip-10-10-32-16 LOKI: Notice: MODULE: Init MESSAGE: Registered plugin PluginWMI
20250727T15:42:59Z ip-10-10-32-16 LOKI: Notice: MODULE: Init MESSAGE: Loaded plugin /home/cmnatic/tools/Loki/plugins/loki-plugin-wmi.py
20250727T15:42:59Z ip-10-10-32-16 LOKI: Notice: MODULE: PESieve MESSAGE: PE-Sieve successfully initialized BINARY: /home/cmnatic/tools/Loki/tools/pe-sieve64.exe SOURCE: https://github.com/h
asherezade/pe-sieve
20250727T15:42:59Z ip-10-10-32-16 LOKI: Info: MODULE: Init MESSAGE: File Name Characteristics initialized with 2841 regex patterns
20250727T15:42:59Z ip-10-10-32-16 LOKI: Info: MODULE: Init MESSAGE: C2 server indicators initialized with 1541 elements
20250727T15:43:02Z ip-10-10-32-16 LOKI: Info: MODULE: Init MESSAGE: Malicious MD5 Hashes initialized with 19034 hashes
20250727T15:43:02Z ip-10-10-32-16 LOKI: Info: MODULE: Init MESSAGE: Malicious SHA1 Hashes initialized with 7159 hashes
20250727T15:43:02Z ip-10-10-32-16 LOKI: Info: MODULE: Init MESSAGE: Malicious SHA256 Hashes initialized with 22841 hashes
20250727T15:43:02Z ip-10-10-32-16 LOKI: Info: MODULE: Init MESSAGE: False Positive Hashes initialized with 30 hashes
20250727T15:43:02Z ip-10-10-32-16 LOKI: Info: MODULE: Init MESSAGE: Processing YARA rules folder /home/cmnatic/tools/Loki/signature-base/yara
20250727T15:43:05Z ip-10-10-32-16 LOKI: Info: MODULE: Init MESSAGE: Initializing all YARA rules at once (composed string of all rule files)
20250727T15:43:06Z ip-10-10-32-16 LOKI: Info: MODULE: Init MESSAGE: Initialized 653 Yara rules
20250727T15:43:06Z ip-10-10-32-16 LOKI: Info: MODULE: Init MESSAGE: Reading private rules from binary ...
20250727T15:43:06Z ip-10-10-32-16 LOKI: Notice: MODULE: Init MESSAGE: Program should be run as 'root' to ensure all access rights to process memory and file objects.
20250727T15:43:06Z ip-10-10-32-16 LOKI: Notice: MODULE: Init MESSAGE: Running plugin PluginWMI
20250727T15:43:06Z ip-10-10-32-16 LOKI: Notice: MODULE: Init MESSAGE: Finished running plugin PluginWMI
20250727T15:43:06Z ip-10-10-32-16 LOKI: Info: MODULE: FileScan MESSAGE: Scanning . ...  
20250727T15:43:06Z ip-10-10-32-16 LOKI: Warning: MODULE: FileScan MESSAGE: FILE: ./ind3x.php SCORE: 70 TYPE: PHP SIZE: 80992 FIRST_BYTES: 3c3f7068700a2f2a0a09623337346b20322e320a / <?php/*b
374k 2.2 MD5: 1606bdac2cb613bf0b8a22690364fbc5 SHA1: 9383ed4ee7df17193f7a034c3190ecabc9000f9f SHA256: 5479f8cd1375364770df36e5a18262480a8f9d311e8eedb2c2390ecb233852ad CREATED: Mon Nov  9 15
:15:32 2020 MODIFIED: Mon Nov  9 13:06:56 2020 ACCESSED: Sun Jul 27 15:43:06 2025 REASON_1: Yara Rule MATCH: webshell_metaslsoft SUBSCORE: 70 DESCRIPTION: Web Shell - file metaslsoft.php RE
F: - MATCHES: Str1: $buff .= "<tr><td><a href=\\"?d=".$pwd."\\">[ $folder ]</a></td><td>LINK</t
20250727T15:43:06Z ip-10-10-32-16 LOKI: Notice: MODULE: Results MESSAGE: Results: 0 alerts, 1 warnings, 7 notices
20250727T15:43:06Z ip-10-10-32-16 LOKI: Result: MODULE: Results MESSAGE: Suspicious objects detected!
20250727T15:43:06Z ip-10-10-32-16 LOKI: Result: MODULE: Results MESSAGE: Loki recommends a deeper analysis of the suspicious objects.
20250727T15:43:06Z ip-10-10-32-16 LOKI: Info: MODULE: Results MESSAGE: Please report false positives via https://github.com/Neo23x0/signature-base
20250727T15:43:06Z ip-10-10-32-16 LOKI: Notice: MODULE: Results MESSAGE: Finished LOKI Scan SYSTEM: ip-10-10-32-16 TIME: 20250727T15:43:06Z
```

It looks as through this is a scan of `ind3x.php`;

```log
20250727T15:43:06Z ip-10-10-32-16 LOKI: Warning: MODULE: FileScan MESSAGE: FILE: ./ind3x.php SCORE: 70 TYPE: PHP SIZE: 80992 FIRST_BYTES: 3c3f7068700a2f2a0a09623337346b20322e320a / <?php/*b
```


At this point I'm happy to say the object is `Suspicious` due the the following line;

```log
20250727T15:43:06Z ip-10-10-32-16 LOKI: Result: MODULE: Results MESSAGE: Suspicious objects detected!
```


However I do feel I have cheated a little, I wonder if this is a remnent of the machine being set up. So I'm going scan the file myself and see if I can get the same result.

Let's navigate back home.

```bash
cmnatic@ip-10-10-203-222:~/suspicious-files$ cd ~; ls -l
total 20
drwxrwxr-x 4 cmnatic cmnatic 4096 Nov  5  2020 go
-rw-rw-r-- 1 cmnatic cmnatic   45 Jul 27 15:40 myfirstrule.yar
drwxrwxr-x 4 cmnatic cmnatic 4096 Jul 27 15:47 suspicious-files
drwxrwxr-x 4 cmnatic cmnatic 4096 Nov  9  2020 tools
drwxrwxr-x 9 cmnatic cmnatic 4096 Nov  6  2020 yara-python
```


This segment is about **LOKI**, and I beleive _LOKI_ was located in `tools` so let's take a look.

```bash
cmnatic@ip-10-10-203-222:~$ ls -l tools/
total 8
drwxrwxr-x 10 cmnatic cmnatic 4096 Nov 10  2020 Loki
drwxrwxr-x  7 cmnatic cmnatic 4096 Nov 10  2020 yarGen
```


And there it is, `Loki` (under tools). Navigate to `Loki`.

```bash
cmnatic@ip-10-10-203-222:~$ cd tools/Loki/; ls -l
total 316
-rw-rw-r-- 1 cmnatic cmnatic  1962 Nov  6  2020 build.bat
drwxrwxr-x 2 cmnatic cmnatic  4096 Nov  6  2020 config
drwxrwxr-x 3 cmnatic cmnatic  4096 Nov  9  2020 lib
-rw-rw-r-- 1 cmnatic cmnatic 35141 Nov  6  2020 LICENSE
-rw-rw-r-- 1 cmnatic cmnatic 99678 Nov  6  2020 loki.ico
-rw-rw-r-- 1 cmnatic cmnatic  2155 Nov  6  2020 lokiicon.jpg
-rw-rw-r-- 1 cmnatic cmnatic   484 Nov  6  2020 loki-noprivrules.spec
-rw-rw-r-- 1 cmnatic cmnatic   551 Nov  6  2020 loki-noprivrules-win2003sup.spec
-rw-rw-r-- 1 cmnatic cmnatic  1484 Nov  6  2020 loki-package-builder.py
-rw-rw-r-- 1 cmnatic cmnatic   570 Nov  6  2020 loki-privrules.spec
-rw-rw-r-- 1 cmnatic cmnatic   637 Nov  6  2020 loki-privrules-win2003sup.spec
-rw-rw-r-- 1 cmnatic cmnatic 76513 Nov  6  2020 loki.py
-rw-rw-r-- 1 cmnatic cmnatic 10905 Nov  6  2020 loki-upgrader.py
-rw-rw-r-- 1 cmnatic cmnatic   523 Nov  6  2020 loki-upgrader.spec
-rw-rw-r-- 1 cmnatic cmnatic   590 Nov  6  2020 loki-upgrader-win2003sup.spec
drwxrwxr-x 2 cmnatic cmnatic  4096 Nov  6  2020 plugins
-rwxrwxr-x 1 cmnatic cmnatic    59 Nov  6  2020 prepare_push.sh
-rw-rw-r-- 1 cmnatic cmnatic 17097 Nov  6  2020 README.md
-rw-rw-r-- 1 cmnatic cmnatic    76 Nov  6  2020 requirements.txt
drwxrwxr-x 2 cmnatic cmnatic  4096 Nov  6  2020 screens
drwxrwxr-x 5 cmnatic cmnatic  4096 Nov  6  2020 signature-base
drwxrwxr-x 4 cmnatic cmnatic  4096 Nov  6  2020 test
drwxrwxr-x 2 cmnatic cmnatic  4096 Nov  6  2020 tools
```


OK, actually this is quite alot, I can see `loki.py` so that's good, however I would feel more comfortable running this from the relevent folder if it's going to dump a log file. So back to `suspicious-files/file1`.

```bash
cmnatic@ip-10-10-203-222:~/tools/Loki$ cd ~/suspicious-files/file1; ls -l
total 84
-rw-rw-r-- 1 cmnatic cmnatic 80992 Nov  9  2020 ind3x.php
-rw-rw-r-- 1 cmnatic cmnatic  3601 Jul 27 15:43 loki_ip-10-10-32-16_2025-07-27_15-42-59.log
```

Perfect. Let's try and run the python file.

```bash
cmnatic@ip-10-10-203-222:~/suspicious-files/file1$ python ~/tools/Loki/loki.py .
usage: loki.py [-h] [-p path] [-s kilobyte] [-l log-file] [-r remote-loghost]
               [-t remote-syslog-port] [-a alert-level] [-w warning-level]
               [-n notice-level] [--printall] [--allreasons] [--noprocscan]
               [--nofilescan] [--nolevcheck] [--scriptanalysis] [--rootkit]
               [--noindicator] [--reginfs] [--dontwait] [--intense] [--csv]
               [--onlyrelevant] [--nolog] [--update] [--debug]
               [--maxworkingset MAXWORKINGSET] [--syslogtcp]
               [--logfolder log-folder] [--nopesieve] [--pesieveshellc]
               [--nolisten] [--excludeprocess EXCLUDEPROCESS]
loki.py: error: unrecognized arguments: .
```


Humm, I've misremembered the syntax. I'll check the room again, is it `-p` for path?

> `cmnatic@thm-yara:~/suspicious-files/file1$ python ../../tools/Loki/loki.py -p .`

Yup, `-p`. Let's try again...

```log
cmnatic@ip-10-10-203-222:~/suspicious-files/file1$ python ~/tools/Loki/loki.py -p .
                                                                               
      __   ____  __ ______                            
     / /  / __ \/ //_/  _/                            
    / /__/ /_/ / ,< _/ /                              
   /____/\____/_/|_/___/                              
      ________  _____  ____                           
     /  _/ __ \/ ___/ / __/______ ____  ___  ___ ____ 
    _/ // /_/ / /__  _\ \/ __/ _ `/ _ \/ _ \/ -_) __/ 
   /___/\____/\___/ /___/\__/\_,_/_//_/_//_/\__/_/    

   Copyright by Florian Roth, Released under the GNU General Public License
   Version 0.32.1
  
   DISCLAIMER - USE AT YOUR OWN RISK
   Please report false positives via https://github.com/Neo23x0/Loki/issues
  
                                                                               

[NOTICE] Starting Loki Scan VERSION: 0.32.1 SYSTEM: ip-10-10-203-222 TIME: 20250730T14:52:42Z PLATFORM:     PROC: x86_64 ARCH: 64bit 
[NOTICE] Registered plugin PluginWMI
[NOTICE] Loaded plugin /home/cmnatic/tools/Loki/plugins/loki-plugin-wmi.py
[NOTICE] PE-Sieve successfully initialized BINARY: /home/cmnatic/tools/Loki/tools/pe-sieve64.exe SOURCE: https://github.com/hasherezade/pe-sieve
[INFO] File Name Characteristics initialized with 2841 regex patterns
[INFO] C2 server indicators initialized with 1541 elements
[INFO] Malicious MD5 Hashes initialized with 19034 hashes
[INFO] Malicious SHA1 Hashes initialized with 7159 hashes
[INFO] Malicious SHA256 Hashes initialized with 22841 hashes
[INFO] False Positive Hashes initialized with 30 hashes
[INFO] Processing YARA rules folder /home/cmnatic/tools/Loki/signature-base/yara
[INFO] Initializing all YARA rules at once (composed string of all rule files)
[INFO] Initialized 653 Yara rules
[INFO] Reading private rules from binary ...
[NOTICE] Program should be run as 'root' to ensure all access rights to process memory and file objects.
[NOTICE] Running plugin PluginWMI
[NOTICE] Finished running plugin PluginWMI
[INFO] Scanning . ...  
[WARNING] 
FILE: ./ind3x.php SCORE: 70 TYPE: PHP SIZE: 80992 
FIRST_BYTES: 3c3f7068700a2f2a0a09623337346b20322e320a / <?php/*b374k 2.2 
MD5: 1606bdac2cb613bf0b8a22690364fbc5 
SHA1: 9383ed4ee7df17193f7a034c3190ecabc9000f9f 
SHA256: 5479f8cd1375364770df36e5a18262480a8f9d311e8eedb2c2390ecb233852ad CREATED: Mon Nov  9 15:15:32 2020 MODIFIED: Mon Nov  9 13:06:56 2020 ACCESSED: Wed Jul 30 14:52:47 2025 
REASON_1: Yara Rule MATCH: webshell_metaslsoft SUBSCORE: 70 
DESCRIPTION: Web Shell - file metaslsoft.php REF: - 
MATCHES: Str1: $buff .= "<tr><td><a href=\\"?d=".$pwd."\\">[ $folder ]</a></td><td>LINK</t
[NOTICE] Results: 0 alerts, 1 warnings, 7 notices
[RESULT] Suspicious objects detected!
[RESULT] Loki recommends a deeper analysis of the suspicious objects.
[INFO] Please report false positives via https://github.com/Neo23x0/signature-base
[NOTICE] Finished LOKI Scan SYSTEM: ip-10-10-203-222 TIME: 20250730T14:52:47Z
 
Press Enter to exit ...
```


Great! This is in beautiful colour in real life, so I'm afraid you'll have to put up with whatever GitHub decides :). It looks like our result was correct.

```log
[RESULT] Suspicious objects detected!
[RESULT] Loki recommends a deeper analysis of the suspicious objects.
```

> `Suspicious` objects detected

Trying this as the answer

#### ✅ Answer

- `Suspicious` ✅


### ❓ Question 2

> What Yara rule did it match on?

#### 🧪 Process

Using the results from "_Question 1_", we can see the following match;

```log
REASON_1: Yara Rule MATCH: webshell_metaslsoft SUBSCORE: 70
```

The **match** is `webshell_metaslsoft`.

Trying this as the answer

#### ✅ Answer

- `webshell_metaslsoft` ✅


### ❓ Question 3

> What does Loki classify this file as?

#### 🧪 Process

Again using the results from "_Question 1_", we can see the following match;

```log
DESCRIPTION: Web Shell - file metaslsoft.php REF: -
```


The space is only really enough for `Web shell`.

Trying this as the answer

#### ✅ Answer

- `Web Shell` ✅


### ❓ Question 4

> Based on the output, what string within the Yara rule did it match on?

#### 🧪 Process

Again using the results from "_Question 1_", we can see the following **match**;

```log
MATCHES: Str1: $buff .= "<tr><td><a href=\\"?d=".$pwd."\\">[ $folder ]</a></td><td>LINK</t
```


The space for the answer is only 4 characters. So it could be _`$pwd`_, but I think it means the rule, so probably `Str1`.

Trying this as the answer.

#### ✅ Answer

- `Str1` ✅


### ❓ Question 5

> What is the name and version of this hack tool?

#### 🧪 Process

Again using the results from "_Question 1_", we can see the following match;

The answer is asking for _5 1.1_. The only thing that matches that is the following line

```log
FIRST_BYTES: 3c3f7068700a2f2a0a09623337346b20322e320a / <?php/*b374k 2.2
```


So I am wondering if `b374k 2.2` is what we're looking for.

Trying this as the answer

#### ✅ Answer

- `b374k 2.2` ✅


### ❓ Question 6

> Inspect the actual Yara file that flagged file 1. Within this rule, how many strings are there to flag this file?

#### 🧪 Process

This one has confused me a bit, I don't see a specific file being used. Can see;

```log
[INFO] Processing YARA rules folder /home/cmnatic/tools/Loki/signature-base/yara
[INFO] Initializing all YARA rules at once (composed string of all rule files)
[INFO] Initialized 653 Yara rules
```


So the folder is `/home/cmnatic/tools/Loki/signature-base/yara` and it has processed `653 Yara rules`. 

The match was ` webshell_metaslsoft`, so may be a look for `Web` or `Shell` in that folder? 

```bash
cmnatic@ip-10-10-203-222:~/suspicious-files/file1$ ls -l ~/tools/Loki/signature-base/yara
total 5520
-rw-rw-r-- 1 cmnatic cmnatic  25681 Nov  6  2020 airbnb_binaryalert.yar
-rw-rw-r-- 1 cmnatic cmnatic    849 Nov  6  2020 apt_aa19_024a.yar
-rw-rw-r-- 1 cmnatic cmnatic   4358 Nov  6  2020 apt_agent_btz.yar
-rw-rw-r-- 1 cmnatic cmnatic   1645 Nov  6  2020 apt_alienspy_rat.yar
-rw-rw-r-- 1 cmnatic cmnatic   1863 Nov  6  2020 apt_apt10_redleaves.yar
-rw-rw-r-- 1 cmnatic cmnatic  65176 Nov  6  2020 apt_apt10.yar
-rw-rw-r-- 1 cmnatic cmnatic    807 Nov  6  2020 apt_apt12_malware.yar
-rw-rw-r-- 1 cmnatic cmnatic  10194 Nov  6  2020 apt_apt15.yar
-rw-rw-r-- 1 cmnatic cmnatic   4014 Nov  6  2020 apt_apt17_mal_sep17.yar
[a lot of results removed]
```


Slightly refined search (`*web*`)

```bash
cmnatic@ip-10-10-203-222:~/suspicious-files/file1$ ls -l ~/tools/Loki/signature-base/yara/*web*
-rw-rw-r-- 1 cmnatic cmnatic  14202 Nov  6  2020 /home/cmnatic/tools/Loki/signature-base/yara/apt_laudanum_webshells.yar
-rw-rw-r-- 1 cmnatic cmnatic   1453 Nov  6  2020 /home/cmnatic/tools/Loki/signature-base/yara/apt_webmonitor_rat.yar
-rw-rw-r-- 1 cmnatic cmnatic    457 Nov  6  2020 /home/cmnatic/tools/Loki/signature-base/yara/apt_webshell_chinachopper.yar
-rw-rw-r-- 1 cmnatic cmnatic  46319 Nov  6  2020 /home/cmnatic/tools/Loki/signature-base/yara/cn_pentestset_webshells.yar
-rw-rw-r-- 1 cmnatic cmnatic  27492 Nov  6  2020 /home/cmnatic/tools/Loki/signature-base/yara/gen_cn_webshells.yar
-rw-rw-r-- 1 cmnatic cmnatic   1086 Nov  6  2020 /home/cmnatic/tools/Loki/signature-base/yara/gen_exploit_cve_2017_10271_weblogic.yar
-rw-rw-r-- 1 cmnatic cmnatic 344722 Nov  6  2020 /home/cmnatic/tools/Loki/signature-base/yara/thor-webshells.yar
```


Without guessing, how do I know which rule was triggered? I've got a gut feeling of `thor-webshells.yar`. But why?

The log doesn't specify which rule the result was triggered by.

```log
cmnatic@ip-10-10-203-222:~/suspicious-files/file1$ cat loki_ip-10-10-203-222_2025-07-30_14-52-42.log 
20250730T14:52:42Z ip-10-10-203-222 LOKI: Notice: MODULE: Init MESSAGE: Starting Loki Scan VERSION: 0.32.1 SYSTEM: ip-10-10-203-222 TIME: 20250730T14:52:42Z PLATFORM:     PROC: x86_64 ARCH:
 64bit 
20250730T14:52:42Z ip-10-10-203-222 LOKI: Notice: MODULE: Init MESSAGE: Registered plugin PluginWMI
20250730T14:52:42Z ip-10-10-203-222 LOKI: Notice: MODULE: Init MESSAGE: Loaded plugin /home/cmnatic/tools/Loki/plugins/loki-plugin-wmi.py
20250730T14:52:42Z ip-10-10-203-222 LOKI: Notice: MODULE: PESieve MESSAGE: PE-Sieve successfully initialized BINARY: /home/cmnatic/tools/Loki/tools/pe-sieve64.exe SOURCE: https://github.com
/hasherezade/pe-sieve
20250730T14:52:42Z ip-10-10-203-222 LOKI: Info: MODULE: Init MESSAGE: File Name Characteristics initialized with 2841 regex patterns
20250730T14:52:42Z ip-10-10-203-222 LOKI: Info: MODULE: Init MESSAGE: C2 server indicators initialized with 1541 elements
20250730T14:52:44Z ip-10-10-203-222 LOKI: Info: MODULE: Init MESSAGE: Malicious MD5 Hashes initialized with 19034 hashes
20250730T14:52:44Z ip-10-10-203-222 LOKI: Info: MODULE: Init MESSAGE: Malicious SHA1 Hashes initialized with 7159 hashes
20250730T14:52:44Z ip-10-10-203-222 LOKI: Info: MODULE: Init MESSAGE: Malicious SHA256 Hashes initialized with 22841 hashes
20250730T14:52:44Z ip-10-10-203-222 LOKI: Info: MODULE: Init MESSAGE: False Positive Hashes initialized with 30 hashes
20250730T14:52:44Z ip-10-10-203-222 LOKI: Info: MODULE: Init MESSAGE: Processing YARA rules folder /home/cmnatic/tools/Loki/signature-base/yara
20250730T14:52:46Z ip-10-10-203-222 LOKI: Info: MODULE: Init MESSAGE: Initializing all YARA rules at once (composed string of all rule files)
20250730T14:52:47Z ip-10-10-203-222 LOKI: Info: MODULE: Init MESSAGE: Initialized 653 Yara rules
20250730T14:52:47Z ip-10-10-203-222 LOKI: Info: MODULE: Init MESSAGE: Reading private rules from binary ...
20250730T14:52:47Z ip-10-10-203-222 LOKI: Notice: MODULE: Init MESSAGE: Program should be run as 'root' to ensure all access rights to process memory and file objects.
20250730T14:52:47Z ip-10-10-203-222 LOKI: Notice: MODULE: Init MESSAGE: Running plugin PluginWMI
20250730T14:52:47Z ip-10-10-203-222 LOKI: Notice: MODULE: Init MESSAGE: Finished running plugin PluginWMI
20250730T14:52:47Z ip-10-10-203-222 LOKI: Info: MODULE: FileScan MESSAGE: Scanning . ...  
20250730T14:52:47Z ip-10-10-203-222 LOKI: Warning: MODULE: FileScan MESSAGE: FILE: ./ind3x.php SCORE: 70 TYPE: PHP SIZE: 80992 FIRST_BYTES: 3c3f7068700a2f2a0a09623337346b20322e320a / <?php/
*b374k 2.2 MD5: 1606bdac2cb613bf0b8a22690364fbc5 SHA1: 9383ed4ee7df17193f7a034c3190ecabc9000f9f SHA256: 5479f8cd1375364770df36e5a18262480a8f9d311e8eedb2c2390ecb233852ad CREATED: Mon Nov  9 
15:15:32 2020 MODIFIED: Mon Nov  9 13:06:56 2020 ACCESSED: Wed Jul 30 14:52:47 2025 REASON_1: Yara Rule MATCH: webshell_metaslsoft SUBSCORE: 70 DESCRIPTION: Web Shell - file metaslsoft.php 
REF: - MATCHES: Str1: $buff .= "<tr><td><a href=\\"?d=".$pwd."\\">[ $folder ]</a></td><td>LINK</t
20250730T14:52:47Z ip-10-10-203-222 LOKI: Notice: MODULE: Results MESSAGE: Results: 0 alerts, 1 warnings, 7 notices
20250730T14:52:47Z ip-10-10-203-222 LOKI: Result: MODULE: Results MESSAGE: Suspicious objects detected!
20250730T14:52:47Z ip-10-10-203-222 LOKI: Result: MODULE: Results MESSAGE: Loki recommends a deeper analysis of the suspicious objects.
20250730T14:52:47Z ip-10-10-203-222 LOKI: Info: MODULE: Results MESSAGE: Please report false positives via https://github.com/Neo23x0/signature-base
20250730T14:52:47Z ip-10-10-203-222 LOKI: Notice: MODULE: Results MESSAGE: Finished LOKI Scan SYSTEM: ip-10-10-203-222 TIME: 20250730T14:52:47Z
```


Let's take a look at my hunch `thor-webshells.yar`.

Okay, I'm not going to cat the result, but there was a lot there ;). So moving on, I've googled how to find `x` within a folder and omg, I've learned something new - `grep -ril "xyz"` will tell you all the files that contain a given string. Perfect! now to put this into practice.

First, what am I going to search for? Going back to _**Question 1**_ we had the following result;

```log
REASON_1: Yara Rule MATCH: webshell_metaslsoft SUBSCORE: 70 
```


I like the match string of "webshell_metaslsoft", so I'm going to try that first, however there is also the following, so maybe this aswell if I have no luck with the webshell.

```log
DESCRIPTION: Web Shell - file metaslsoft.php REF: - 
```


```bash
cmnatic@ip-10-10-203-222:~/tools/Loki/signature-base/yara$ grep -ril "webshell_metaslsoft"
thor-webshells.yar
```

Okay, great, this confirms my hunch. Now how many? Let's cat the file but filter on "`metaslsoft`".

```bash
cmnatic@ip-10-10-203-222:~/tools/Loki/signature-base/yara$ cat thor-webshells.yar | grep "metaslsoft"
rule webshell_metaslsoft {
description = "Web Shell - file metaslsoft.php"
```


Interesting, so we have a rule named `webshell_metaslsoft` which matches our "Rule **Match"**

We can use `grep -n` to find the line number of the result. 

```bash
cmnatic@ip-10-10-203-222:~/tools/Loki/signature-base/yara$ cat thor-webshells.yar | grep -n "webshell_metaslsoft"
1930:rule webshell_metaslsoft {
```


But actually, it may be just as easy to open it in `vim` and do a search (`/`) for `webshell_metaslsoft`.

```yara
rule webshell_metaslsoft {
meta:
description = "Web Shell - file metaslsoft.php"
license = "https://creativecommons.org/licenses/by-nc/4.0/"
author = "Florian Roth"
date = "2014/01/28"
score = 70
hash = "aa328ed1476f4a10c0bcc2dde4461789"
strings:
$s7 = "$buff .= \"<tr><td><a href=\\\"?d=\".$pwd.\"\\\">[ $folder ]</a></td><td>LINK</t"
condition:
all of them
}
```


This is our result, the question was;

> "_Within this rule, how many strings are there to flag this file?_"

We only have the `1` string (`$s7`). So...

Trying this as the answer


#### ✅ Answer

- `1` ✅


### ❓ Question 7

> Scan file 2. Does Loki detect this file as suspicious/malicious or benign?

#### 🧪 Process

Okay so let's navigate to `file2`. 

```bash
cmnatic@ip-10-10-203-222:~/tools/Loki/signature-base/yara$ cd ~/suspicious-files/file2; ls -l
total 220
-rw-rw-r-- 1 cmnatic cmnatic 223978 Nov  9  2020 1ndex.php
```

Run the `loki.py` against `1ndex.php`.


```bash
cmnatic@ip-10-10-203-222:~/suspicious-files/file2$ python ~/tools/Loki/loki.py -p .
                                                                               
      __   ____  __ ______                            
     / /  / __ \/ //_/  _/                            
    / /__/ /_/ / ,< _/ /                              
   /____/\____/_/|_/___/                              
      ________  _____  ____                           
     /  _/ __ \/ ___/ / __/______ ____  ___  ___ ____ 
    _/ // /_/ / /__  _\ \/ __/ _ `/ _ \/ _ \/ -_) __/ 
   /___/\____/\___/ /___/\__/\_,_/_//_/_//_/\__/_/    

   Copyright by Florian Roth, Released under the GNU General Public License
   Version 0.32.1
  
   DISCLAIMER - USE AT YOUR OWN RISK
   Please report false positives via https://github.com/Neo23x0/Loki/issues
  
                                                                               

[NOTICE] Starting Loki Scan VERSION: 0.32.1 SYSTEM: ip-10-10-203-222 TIME: 20250730T15:37:49Z PLATFORM:     PROC: x86_64 ARCH: 64bit 
[NOTICE] Registered plugin PluginWMI
[NOTICE] Loaded plugin /home/cmnatic/tools/Loki/plugins/loki-plugin-wmi.py
[NOTICE] PE-Sieve successfully initialized BINARY: /home/cmnatic/tools/Loki/tools/pe-sieve64.exe SOURCE: https://github.com/hasherezade/pe-sieve
[INFO] File Name Characteristics initialized with 2841 regex patterns
[INFO] C2 server indicators initialized with 1541 elements
[INFO] Malicious MD5 Hashes initialized with 19034 hashes
[INFO] Malicious SHA1 Hashes initialized with 7159 hashes
[INFO] Malicious SHA256 Hashes initialized with 22841 hashes
[INFO] False Positive Hashes initialized with 30 hashes
[INFO] Processing YARA rules folder /home/cmnatic/tools/Loki/signature-base/yara
[INFO] Initializing all YARA rules at once (composed string of all rule files)
[INFO] Initialized 653 Yara rules
[INFO] Reading private rules from binary ...
[NOTICE] Program should be run as 'root' to ensure all access rights to process memory and file objects.
[NOTICE] Running plugin PluginWMI
[NOTICE] Finished running plugin PluginWMI
[INFO] Scanning . ...  
[NOTICE] Results: 0 alerts, 0 warnings, 7 notices
[RESULT] SYSTEM SEEMS TO BE CLEAN.
[INFO] Please report false positives via https://github.com/Neo23x0/signature-base
[NOTICE] Finished LOKI Scan SYSTEM: ip-10-10-203-222 TIME: 20250730T15:37:52Z
 
Press Enter to exit ...
```


This has far less information in it, but it does have the following line:

```log
[RESULT] SYSTEM SEEMS TO BE CLEAN.
```

So this would indicate that _`Loki`_ has reborted this file as `benign`.

Trying this as the answer

#### ✅ Answer

- `Benign` ✅


### ❓ Question 8

> Inspect file 2. What is the name and version of this web shell?

#### 🧪 Process

According to `ls` this file is ~220kb in size, I'm not going to `cat` that out.


```bash
cmnatic@ip-10-10-203-222:~/suspicious-files/file2$ ls -l 1ndex.php 
-rw-rw-r-- 1 cmnatic cmnatic 223978 Nov  9  2020 1ndex.php
```


We can use `vim` instead and see if we can find any file versions...

```php
<?php
/*
        b374k shell 3.2.3
        Jayalah Indonesiaku
        (c)2014
        https://github.com/b374k/b374k

*/
```


Oh, well that was easy, is it too easy? 

I'm curious, what if I get the hash (`sha256sum`) and pop it into [VirusTotal](https://www.virustotal.com/gui/home/upload)?

```bash
cmnatic@ip-10-10-203-222:~/suspicious-files/file2$ sha256sum 1ndex.php 
53fe44b4753874f079a936325d1fdc9b1691956a29c3aaf8643cdbd49f5984bf  1ndex.php
```


<img width="1912" height="920" alt="image" src="https://github.com/user-attachments/assets/46f22895-6d92-44a5-a911-eba5f457b34b" />


Interesting, so from Loki this would be a false negative, we would need to create yara rule to match this file. 

So let's get back to answering the question. The answer is wanting another "5 1.1.1" answer, so would `b374k 3.2.3` fit?

Trying this as the answer

#### ✅ Answer

- `b374k 3.2.3` ✅


## 📘Creating Yara rules with yarGen
