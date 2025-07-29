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

## Introduction

YARA the pattern recognition tool to aid security personnel analyse potential malware and categorise within a specific rulesets



## What is Yara?

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



## Deploy

This is just to deploy the VM.


## Introduction to Yara Rules


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



## Expanding on Yara Rules



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



## Yara Modules



## Other tools and Yara

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



## Using LOKI and its Yara rule set
