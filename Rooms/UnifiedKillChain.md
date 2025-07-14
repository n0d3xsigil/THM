| [Home](../README.md) | [SOC Level 1](../SOClevel1.md) | **Unified Kill Chain** |

# Unified Kill Chain

## Contents

- [What is a "Kill Chain"](#what-is-a-kill-chain)
- [What is "Threat Modelling"](#what-is-threat-modelling)
- [Introducing the Unified Kill Chain](#introducing-the-unified-kill-chain)
- [Phase: In (Initial Foothold)](#phase-in-initial-foothold)
- [Phase: Through (Network Propagation)](#phasethroughnetworkpropagation)
- [Phase: Out (Action on Objectives)](#phaseoutactiononobjectives)
- [Practical](#practical)


## 📘What is a "Kill Chain"

- The term **"Kill Chain"** comes from the military and outlines the steps of an attack.
- In cybersecurity, it maps how attackers (e.g. hackers, APTs) approach and breach a target.
- Example steps: **scanning → exploiting → privilege escalation**.
- The goal is to **understand the attack process** to
  - Strengthen defences ahead of time.
  - Detect and **disrupt the attacker** during the intrusion.
- The **Unified Kill Chain** expands on this concept to cover full campaign-level activity.

### ❓ Question 1

> Where does the term "Kill Chain" originate from?

For this answer, you must fill in the blank!: The ********

#### 🧪 Process

>  **"Kill Chain"** comes from the `military`.

Trying this as the answer

#### ✅ Answer

- `military` ✅



  ## 📘What is "Threat Modelling"

- **Threat modelling** is a structured process to identify and reduce risks in systems or applications.
- Key steps include
  - Identifying **critical systems/assets** and their roles (e.g. store sensitive data or support operations).
  - Assessing **vulnerabilities** and how they could be exploited.
  - Developing an **action plan** to secure those systems.
  - Implementing **preventive measures** (e.g. secure development practices, staff training)
- It provides a **high-level view of IT assets** and outlines how to resolve weaknesses.
- The **Unified Kill Chain supports threat modelling** by highlighting likely attack paths and exploitable surfaces.
- Common threat modelling frameworks: **STRIDE**, **DREAD**, **CVSS**.
- For more, explore the **“Principles of Security”** room on TryHackMe.


### ❓ Question 1

> What is the technical term for a piece of software or hardware in IT (Information Technology?)

#### 🧪 Process

An `asset`.

Trying this as the answer

#### ✅ Answer

- `asset` ✅



## 📘Introducing the Unified Kill Chain

**Useful Link**: [The Unified Kill Chain](https://www.unifiedkillchain.com/assets/The-Unified-Kill-Chain.pdf)

- **Paul Pols' Unified Kill Chain (UKC)** was introduced in **2017** to **complement**, not replace, existing models like **Lockheed Martin’s Kill Chain** and **MITRE ATT\&CK**.
- It defines **18 detailed attack phases**, covering the full scope of an attack — from **reconnaissance to motivation**.
- For simplicity, this room groups those phases into broader focus areas.

### ✅ **Benefits of the UKC over other frameworks**
- **Modern & Updated**: Released in **2017**, updated in **2022** — reflects today’s threat landscape.
- **Extensive Detail**: Contains **18 phases**, while others may only have a few.
- **Full Attack Coverage**: Includes **pre-attack, post-exploitation**, and **attacker motives**.
- **Realistic Attack Flow**: Recognizes attackers **loop between phases** (e.g. re-running recon after gaining access).


### ❓ Question 1

> In what year was the Unified Kill Chain framework released?

#### 🧪 Process

> **Unified Kill Chain (UKC)** was introduced in **`2017`**

Trying this as the answer

#### ✅ Answer

- `2017` ✅


### ❓ Question 2

> According to the Unified Kill Chain, how many phases are there to an attack?

#### 🧪 Process

See the page, there are `18` phases.

Trying this as the answer

#### ✅ Answer

- `18` ✅


### ❓ Question 3

> What is the name of the attack phase where an attacker employs techniques to evade detection?

#### 🧪 Process

You'll need to see the page, see phase 7, `Defense Evasion`.

Trying this as the answer

#### ✅ Answer

- `Defense Evasion` ✅


### ❓ Question 4

> What is the name of the attack phase where an attacker employs techniques to remove data from a network?

#### 🧪 Process

Again, you'll need to see the page, phase 16, `Exfiltration`.

Trying this as the answer

#### ✅ Answer

- `Exfiltration` ✅


### ❓ Question 5

> What is the name of the attack phase where an attacker achieves their objectives?

#### 🧪 Process

See Phase 18, `Objectives`.

Trying this as the answer

#### ✅ Answer

- `Objectives` ✅



## 📘Phase: In (Initial Foothold)

Certainly! Here's a clean, bullet-point summary of that section, grouped by phase, and tied into the **Unified Kill Chain (UKC)** context:

### 🔐 **UKC Phases: Gaining and Maintaining Initial Access**

**📌 Focus**: These phases describe how attackers **gain a foothold**, **maintain persistence**, and **prepare for lateral movement** within a system or network.

**Reconnaissance** (MITRE [TA0043](https://attack.mitre.org/tactics/TA0043/))
- Adversary gathers info to support later phases.
- Techniques include **active and passive scanning**.
- May collect
  - Open ports/services.
  - Employee or contact lists (useful for phishing/social engineering).
  - Leaked credentials.
  - Network topology and connected systems.
**Weaponization** (MITRE [TA0001](https://attack.mitre.org/tactics/TA0001/))
- Attacker prepares infrastructure and payloads.
- Examples
  - Setting up **Command & Control (C2)** servers.
  - Creating malware or exploit tools for delivery.
**Social Engineering** (MITRE [TA0001](https://attack.mitre.org/tactics/TA0001/))
- Manipulating people to help compromise systems.
- Examples:
  - Convincing a user to open a malicious attachment.
  - Credential harvesting via fake websites.
  - Impersonating trusted individuals (e.g. utility engineers, IT staff).
**Exploitation** (MITRE [TA0002](https://attack.mitre.org/tactics/TA0002/))
- Execution of payloads to exploit vulnerabilities.
- Examples
  - Uploading a **reverse shell**.
  - Exploiting a vulnerable **script or web app**.
  - Triggering code execution via weak configurations.
**Persistence** (MITRE [TA0003](https://attack.mitre.org/tactics/TA0003/))
- Ensures ongoing access to compromised systems.
- Examples:
  - Installing a **backdoor or startup service**.
  - Registering the system with a **C2 server**.
  - Triggers that run on specific actions (e.g. admin login).
**Defence Evasion** (MITRE [TA0005](https://attack.mitre.org/tactics/TA0005/))
- Avoiding detection by security tools.
- Examples:
  - Evading **WAFs, firewalls, antivirus, IDS/IPS**.
  - Critical for understanding how attackers bypass defences and **improving future resilience**.
**Command & Control (C2)** (MITRE [TA0011](https://attack.mitre.org/tactics/TA0011/))
- Establishes communication with the compromised host.
- Enables:
  - Remote command execution.
  - Data theft or credential harvesting.
  - **Pivoting** to other network systems.
**Pivoting** (MITRE [TA0008](https://attack.mitre.org/tactics/TA0008/))
- Moving laterally to reach internal systems.
- Example:
  - Attacker compromises a **public web server**, then uses it to access **internal-only systems** that are otherwise unreachable.


### ❓ Question 1

> What is an example of a tactic to gain a foothold using emails?

#### 🧪 Process

We would use `Phishing` to gain a foothold.

Trying this as the answer

#### ✅ Answer

- `Phishing` ✅


### ❓ Question 2

> Impersonating an employee to request a password reset is a form of what?

#### 🧪 Process

This is an example of `Social Engineering`.

Trying this as the answer

#### ✅ Answer

- `Social Engineering` ✅


### ❓ Question 3

> An adversary setting up the Command & Control server infrastructure is what phase of the Unified Kill Chain?

#### 🧪 Process

This would fall under `Weaponization`.

Trying this as the answer

#### ✅ Answer

- `Weaponization` ✅


### ❓ Question 4

> Exploiting a vulnerability present on a system is what phase of the Unified Kill Chain?

#### 🧪 Process

It's in the name, `Exploitation`.

Trying this as the answer

#### ✅ Answer

- `Exploitation` ✅


### ❓ Question 5

> Moving from one system to another is an example of?

#### 🧪 Process

We would perform `Pivoting` from one system to another.

Trying this as the answer

#### ✅ Answer

- `Pivoting` ✅


### ❓ Question 6

> Leaving behind a malicious service that allows the adversary to log back into the target is what?

#### 🧪 Process

We would persist to have `Persistence`. ;)

Trying this as the answer

#### ✅ Answer

- `Persistence` ✅



## 📘Phase: Through (Network Propagation)

**Post-Foothold Phase Overview**
- After gaining initial access, the attacker aims to expand control and access within the network.
- A compromised system is used as a **pivot point** to explore and exploit the internal network.
**Pivoting (MITRE Tactic [TA0008](https://attack.mitre.org/tactics/TA0008/))**
- The compromised system becomes a **staging site** and **tunnel** for attacker operations.
- Used to **distribute malware and backdoors** across the network.
**Discovery (MITRE Tactic [TA0007](https://attack.mitre.org/tactics/TA0007/))**
- The attacker gathers intelligence on:
  - **User accounts** and their permissions.
  - **Installed applications and software**.
  - **Web browser activity**.
  - **Files, directories, and network shares**.
  - **System configurations**.
**Privilege Escalation (MITRE Tactic [TA0004](https://attack.mitre.org/tactics/TA0004/))**
- The attacker attempts to gain higher-level access using:
  - **Vulnerabilities** and **misconfigurations**.
- Target privilege levels include:
  - **SYSTEM/ROOT**.
  - **Local Administrator**.
  - **Admin-like user accounts**.
  - **Users with specific access or functions**.
**Execution (MITRE Tactic [TA0002](https://attack.mitre.org/tactics/TA0002/))**
- Malicious code is deployed via the pivot system.
- Techniques include:
  - **Remote trojans**.
  - **Command and Control (C2) scripts**.
  - **Malicious links**.
  - **Scheduled tasks** for persistence.
**Credential Access (MITRE Tactic [TA0006](https://attack.mitre.org/tactics/TA0006/))**
- The attacker steals credentials using:
  - **Keylogging**.
  - **Credential dumping**.
- Enables stealthier movement using **legitimate credentials**.
**Lateral Movement (MITRE Tactic [TA0008](https://attack.mitre.org/tactics/TA0008/))**
- With elevated privileges and credentials, the attacker:
  - **Moves across the network** to other systems.
  - Uses **stealthy techniques** to avoid detection.


### ❓ Question 1

> As a SOC analyst, you pick up numerous alerts pointing to failed login attempts from an administrator account. What stage of the kill chain would an attacker be seeking to achieve?

#### 🧪 Process

> **`Privilege Escalation` (MITRE Tactic [TA0004](https://attack.mitre.org/tactics/TA0004/))**
> - The attacker attempts to gain higher-level access

Trying this as the answer

#### ✅ Answer

- `Privilege Escalation` ✅


### ❓ Question 2

> Mimikatz, a known post-exploitation tool, was recently detected running on the IT Manager’s computer. Security logs show that Mimikatz attempted to access memory spaces typically used by Windows to store user authentication secrets. Considering the usual capabilities and purpose of Mimikatz, what is the primary objective of this tool in such an attack scenario?

#### 🧪 Process

> The attacker steals credentials using:
> - **Keylogging**.
> - **`Credential dumping`**

Trying this as the answer

#### ✅ Answer

- `Credential dumping` ✅



## 📘Phase: Out (Action on Objectives)

**Final Phase Overview**
- The attacker has achieved deep access to critical assets.
- Their actions now focus on fulfilling their **strategic goals**, often targeting the **CIA triad**:
  - **Confidentiality**
  - **Integrity**
  - **Availability**
**Collection (MITRE Tactic [TA0009](https://attack.mitre.org/tactics/TA0009/))**
- The attacker gathers valuable data from:
  - **Drives**
  - **Browsers**
  - **Audio/Video**
  - **Emails**
- This compromises **confidentiality** and prepares for **exfiltration**.
**Exfiltration (MITRE Tactic [TA0010](https://attack.mitre.org/tactics/TA0010/))**
- Data is **stolen** and **extracted** from the environment.
- Techniques include:
  - **Encryption and compression** to evade detection.
  - Use of **C2 channels and tunnels** established earlier.
**Impact (MITRE Tactic [TA0040](https://attack.mitre.org/tactics/TA0040/))**
- The attacker disrupts **integrity** and **availability** by:
  - **Manipulating, deleting, or encrypting data**.
  - **Removing access**, **wiping disks**, or launching:
    - **Ransomware**
    - **Defacement**
    - **Denial of Service (DoS)** attacks
**Objectives**
- The attacker executes their **strategic intent**, such as:
  - **Financial gain** (e.g., ransomware for payment).
  - **Reputation damage** (e.g., leaking confidential data).

### ❓ Question

> While monitoring the network as a SOC analyst, you realise that there is a spike in the network activity, and all the traffic is outbound to an unknown IP address. What stage could describe this activity?

#### 🧪 Process

A spike in traffic could mean that more data than usual is traversing the network, since the traffic is outbount, we could be whitnesing `Exfiltration`.

Trying this as your answer

#### ✅ Answer

- `Exfiltration` ✅


### ❓ Question

> Personally identifiable information (PII) has been released to the public by an adversary, and your organisation is facing scrutiny for the breach. What part of the CIA triad would be affected by this action?

#### 🧪 Process

If PII has been released to the public, this would be a breach of `Confidentiality`.

Trying this as the answer

#### ✅ Answer

- `Confidentiality` ✅



## 📘Practical

### ❓ Question

> Match the scenario prompt to the correct phase of the Unified Kill Chain to reveal the flag at the end. What is the flag?

#### 🧪 Process

See the page on THM to fin the answers you are looking for

#### ✅ Answer

- `THM{UKC_SCENARIO}` ✅
