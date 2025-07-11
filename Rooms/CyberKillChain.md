| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **Cyber Kill Chain** |

# Cyber Kill Chain

## Contents

- [Introduction](#introduction)
- [Reconnaissance](#reconnaissance)
- [Weaponization](#weaponization)
- [Delivery](#delivery)



## 📘Introduction

The Cyber Kill Chain©️ framework, deveopled by Lockheed Martin in 2011 based on the millitary concept.

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
