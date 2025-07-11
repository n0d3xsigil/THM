| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **Cyber Kill Chain** |

# Cyber Kill Chain

## Contents

- [Introduction](#introduction)
- [Reconnaissance](#reconnaissance)
- [Weaponization](#weaponization)
- [Delivery](#delivery)
- [Installation](#installation)
- [Command & Control](#commandcontrol)



## 📘Introduction

The Cyber Kill Chain© framework, deveopled by Lockheed Martin in 2011 based on the millitary concept.

In order for an adversary to suceed in defeating the defenses they will need to beat every phase of the Kill Chain.

Any _SOC Analyst_, _Security Researcher_, _Threat Hunter_ or _Incident Responder_ should have a good undertanding of the Kill Chain in order to recognise an intrusion.



## 📘Reconnaissance

Reconnaissance is the process of discovering and collecting information on a subject. Such as a system, victim, target etc. Think of it as the planning phase.

**OSINT** can be considered Reconnaissance, OSINT is Open Source Intelligence. With OSINT we can find information about people, businesses etc with just information publically available. 


**Email harvesting** is sourcing emails from services, may that be public, paid or free services. These addresses are then used for email harvesting for phishing attacks. 

Some sites
- **[`theHarvester`](https://github.com/laramies/theHarvester)**
- **[`hunter.io`](https://hunter.io/)**
- **[`OSINT Framework`](https://osintframework.com/)**
- [Exploitation](#exploitation)



### ❓ Question 1

> What is the name of the Intel Gathering Tool that is a web-based interface to the common tools and resources for open-source intelligence?

#### 🧪 Process

One popular tool is the `OSINT Framework`.

Trying this as the answer

#### ✅ Answer

- `OSINT Framework` ✅


### ❓ Question 2

> What is the definition for the email gathering process during the stage of reconnaissance?

#### 🧪 Process

This would be `Email Harvesting`.

Trying this as the answer

#### ✅ Answer

- `Email Harvesting` ✅



## 📘Weaponization

- After initial reconnaissance, the attacker enters the **weaponization phase**.
- The goal is to prepare a **malicious payload** that exploits system vulnerabilities.
- Instead of developing custom malware, the attacker may **purchase pre-made payloads** from underground sources to save time.
- Common weaponization methods include:
  - Embedding malware and exploits into **infected documents** (e.g., with malicious macros or scripts).
  - Distributing **USB drives** containing worms or other malicious code.
- The attacker also selects:
  - Command and Control (**C2**) techniques to remotely execute commands or deliver additional payloads.
  - **Backdoor implants** to maintain unauthorized access to the target system.

### ❓ Question

> This term is referred to as a group of commands that perform a specific task. You can think of them as subroutines or functions that contain the code that most users use to automate routine tasks. But malicious actors tend to use them for malicious purposes and include them in Microsoft Office documents. Can you provide the term for it?

#### 🧪 Process

I use these every day to automate processes in documents. Usually Excel and I would call it a VBA `Macro`.

Trying this as the answer

#### ✅ Answer

- `Macro` ✅



## 📘Delivery

- **Phishing Emails**
  - Crafted to appear legitimate and target specific individuals (spearphishing) or broader groups.
  - May impersonate trusted contacts and include malicious attachments or links.
  - Example tactic: mimicking a known contact’s email to send a fake invoice containing malware.
- **Infected USB Drives**
  - Left in public places (e.g., coffee shops, parking lots) or mailed to targets.
  - May be branded with company logos to appear trustworthy.
  - Once plugged in, the USB executes the malicious payload.
- **Watering Hole Attacks**
  - The attacker compromises a website frequently visited by the target group.
  - Victims are redirected to a malicious site or prompted to download malware.
  - Often involves exploiting known website vulnerabilities.
  - May include drive-by downloads, such as fake browser extension pop-ups.


### ❓ Question

> What is the name of the attack when it is performed against a specific group of people, and the attacker seeks to infect the website that the mentioned group of people is constantly visiting.

#### 🧪 Process

This is a `Watering Hole Attack`.

Trying this as the answer

#### ✅ Answer

- `Watering Hole Attack` ✅



## 📘Exploitation


- In this phase, the attacker **activates the malicious payload** to exploit a vulnerability and gain access to the system.
- Example tactics include:
  - Sending **phishing emails** with:
    - A link to a **fake login page** (e.g., mimicking Office 365).
    - A **malicious attachment** (e.g., a macro-enabled file that executes ransomware).
  - Victims trigger the exploit by **clicking links** or **opening attachments**.

- Once access is gained, the attacker may:
  - **Exploit software, system, or server vulnerabilities** to escalate privileges or move laterally within the network.
  - Use **[lateral movement](https://www.crowdstrike.com/en-us/cybersecurity-101/cyberattacks/lateral-movement/) techniques** to access deeper parts of the network and extract sensitive data.

- The attacker might also use a **[zero-day exploit](https://www.trellix.com/security-awareness/cybersecurity/what-is-a-zero-day-exploit/)**, which:
  - Targets unknown vulnerabilities.
  - Offers **no initial detection opportunity**, making it especially dangerous.

- Common exploitation methods:
  - Triggering exploits via **email links or attachments**.
  - Leveraging **zero-day vulnerabilities**.
  - Exploiting **software, hardware, or human weaknesses**.
  - Attacking **server-based vulnerabilities** directly.


### ❓ Question

> Can you provide the name for a cyberattack targeting a software vulnerability that is unknown to the antivirus or software vendors?

#### 🧪 Process

If it's not known it is a `Zero-day`.

Trying this as the answer

#### ✅ Answer

- `Zero-day` ✅



## 📘Installation

**Links**
- [Persistent Backdoors](https://www.offsec.com/metasploit-unleashed/persistent-backdoors/)
- ❌ [Windows Local Persistence](Rooms/WindowsLocalPersistence.md)
  - [https://tryhackme.com/room/windowslocalpersistence](https://tryhackme.com/room/windowslocalpersistence)
- [Web shell attacks continue to rise](https://www.microsoft.com/en-us/security/blog/2021/02/11/web-shell-attacks-continue-to-rise/)
- [Meterpreter Backdoor](https://www.offsec.com/metasploit-unleashed/meterpreter-backdoor/)
- [T1543.003](https://attack.mitre.org/techniques/T1543/003/)
- [Reg](https://attack.mitre.org/software/S0075/)
- [Masquerading](https://attack.mitre.org/techniques/T1036/)
- [T1547.001](https://attack.mitre.org/techniques/T1547/001/)
- [Timestomp](https://attack.mitre.org/techniques/T1070/006/)

- After gaining access, the attacker aims to **maintain long-term access** to the compromised system, even if the initial entry point is removed or patched.
- This is achieved by installing a **persistent backdoor**, also known as an **access point**, which allows re-entry without re-exploitation.
- Common persistence techniques include:
  - **Web Shells**:
    - Malicious scripts (e.g., `.php`, `.asp`, `.jsp`) installed on web servers.
    - Allow remote access and are often hard to detect due to their simplicity and file format.
  - **Backdoors on Victim Machines**:
    - Tools like **Meterpreter** (from Metasploit) provide an interactive shell for remote control and code execution.
  - **Modifying or Creating Windows Services**:
    - Known as technique **T1543.003** in MITRE ATT&CK.
    - Attackers use tools like `sc.exe` and `reg` to configure services that run malicious payloads.
    - Services may be disguised with legitimate-sounding names.
  - **Registry Run Keys / Startup Folder Entries**:
    - Malicious payloads are set to execute at user login.
    - Can be configured for individual users or system-wide execution.
- Additional stealth technique:
  - **Timestomping**:
    - Alters file timestamps (created, modified, accessed) to blend malware with legitimate files and evade forensic detection.


### ❓ Question 1

> Can you provide the technique used to modify file time attributes to hide new or changes to existing files?

#### 🧪 Process

This is `timestomping`.

Trying this as the answer

#### ✅ Answer

- `Timestomping` ✅


### ❓ Question 2

> Can you name the malicious script planted by an attacker on the webserver to maintain access to the compromised system and enables the webserver to be accessed remotely?

#### 🧪 Process

I had to read that a few times, _webserver_ should tell you all you need to know. It's a `Web shell`.

Trying this as the answer

#### ✅ Answer

- `Web shell` ✅



5## 📘Command & Control

- After establishing persistence and executing malware, the attacker sets up a **Command and Control (C2)** channel to remotely manage the compromised system.
- This communication is often referred to as **C2 beaconing**, where the infected host regularly contacts the attacker's server.
- **Purpose of C2**:
  - Allows the attacker to **maintain control**, issue commands, and potentially deliver additional payloads.
  - Ensures continued access even after initial compromise.
- **How it works**:
  - The infected machine communicates with an **external server** controlled by the attacker.
  - Once connected, the attacker gains **full remote access** to the system.
- **Common C2 communication methods**
  - **HTTP (port 80)** and **HTTPS (port 443)**
    - Blends malicious traffic with normal web traffic to **evade detection**.
  - **DNS tunneling**
    - The infected host sends **frequent DNS requests** to a domain controlled by the attacker.
    - Used to **bypass firewalls** and maintain stealth.
  - **Note**
    - The C2 infrastructure may be operated by the attacker directly or by another **compromised system** acting as a relay.


### ❓ Question 1

> What is the C2 communication where the victim makes regular DNS requests to a DNS server and domain which belong to an attacker. 

#### 🧪 Process

I wanted to say this was 'DNS Poisining', however I remembered we didn't cover that! So it's `DNS Tunneling`

#### ✅ Answer

- `DNS Tunneling` ✅
