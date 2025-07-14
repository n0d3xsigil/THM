| [Home](../README.md) | [SOC Level 1](../SOClevel1.md) | **Diamond Model** |

# Diamond Model

## Contents
- [Introduction](#introduction)
- [Adversary](#adversary)
- [Victim](#victim)
- [Capability](#capability)
- [Infrastructure](#infrastructure)
- [Event Meta Features](#eventmetafeatures)





## 📘Introduction

<img width="1013" height="832" alt="image" src="https://github.com/user-attachments/assets/b71b5969-d820-49fd-874c-7e91f397f3e5" />


**Origin**
- Developed in 2013 by Sergio Caltagirone, Andrew Pendergast, and Christopher Betz.
- See [`diamond.pdf`](https://www.activeresponse.org/wp-content/uploads/2013/07/diamond.pdf) for the full documentation.

**Core Components**
- The model consists of four main features
  - **Adversary** – the threat actor behind the intrusion.
  - **Infrastructure** – the systems and services used by the adversary.
  - **Capability** – the tools and techniques used in the attack.
  - **Victim** – the target of the intrusion.
- These four features are connected at the edges, forming a **diamond shape** to represent their interrelationships.

**Additional Axes**
- Two more contextual dimensions are mentioned
  - **Social-Political**
  - **Technology**

**Purpose and Flexibility**
- Designed to encapsulate key concepts of intrusion analysis and adversary behavior.
- Flexible enough to incorporate new ideas and evolving threats.

**Practical Applications**
- Enables real-time intelligence integration for network defense.
- Supports
  - Automated event correlation.
  - Confident classification of events into adversary campaigns.
  - Forecasting of adversary operations.
  - Planning and simulating mitigation strategies.

**Why Learn It?**
- Helps identify and analyze elements of a cyber intrusion.
- Assists in creating a Diamond Model for incidents like breaches or attacks.
- Useful for analyzing **Advanced Persistent Threats (APTs)**.
- Aids in explaining incidents to **non-technical audiences**.



## 📘Adversary

**Adversary – Core Concept**
- Also referred to as: **attacker**, **enemy**, **cyber threat actor**, or **hacker**.
- The **adversary** is the individual or group **behind a cyberattack**, which could be an **intrusion** or a **breach**.

**Definition (per Diamond Model creators)**
- An **adversary** is an **actor or organization** that uses a **capability** against a **victim** to achieve their **intent**.
- Often, adversary details are **unknown or unclear** at the time of discovery.

**Adversary Roles**
- **Adversary Operator**
  - The **person(s)** actively conducting the intrusion.
  - Often the “hacker” in action.
- **Adversary Customer**
  - The **entity benefiting** from the intrusion.
  - May be the same as the operator or a **separate person/group**.
  - Can **control multiple operators** with different capabilities and infrastructure.

**Why the Distinction Matters**
- Helps in understanding
  - **Intent** behind the attack.
  - **Attribution** of the threat.
  - **Adaptability** and **persistence** of the adversary.
  - The **relationship** between adversary and victim.

**Challenges in Identification**
- Adversaries are **hard to identify early** in an attack.
- Clues can be gathered from
  - Incident data.
  - Breach signatures.
  - Other relevant intelligence.
 

### ❓ Question 1

> What is the term for a person/group that has the intention to perform malicious actions against cyber resources?

#### 🧪 Process

> **`Adversary Operator`**
> - The **person(s)** actively conducting the intrusion.
> - Often the “hacker” in action.

Trying this as the answer

#### ✅ Answer

- `Adversary Operator` ✅


### ❓ Question 2

> What is the term of the person or a group that will receive the benefits from the cyberattacks?

#### 🧪 Process


> **`Adversary Customer`**
> - The **entity benefiting** from the intrusion.
> - May be the same as the operator or a **separate person/group**.
> - Can **control multiple operators** with different capabilities and infrastructure.

Trying this as the answer

#### ✅ Answer

- `Adversary Customer` ✅



## 📘Victim

**Victim – Core Concept**
- The **victim** is the **target** of the adversary.
- Can be
  - An **organization**
  - A **person**
  - A **targeted email address**, **IP address**, **domain**, etc.
- There is **always a victim** in every cyberattack.

**Victim as an Entry Point**
- Victims often serve as **initial footholds** for attackers to infiltrate larger targets.
- Example: A **spear-phishing email** sent to a company; the person who clicks the link is the **victim**.

**Victim Categories**
- **Victim Personae**
  - The **people or organizations** being targeted.
  - Includes:
    - Organization names
    - Individual names
    - Industries
    - Job roles
    - Interests
- **Victim Assets**
  - The **attack surface** targeted by the adversary.
  - Includes:
    - Systems
    - Networks
    - Email addresses
    - Hosts
    - IP addresses
    - Social media accounts

**Why the Distinction Matters**
- **Victim Personae** help understand **who** is being targeted.
- **Victim Assets** help identify **what** is being attacked.
- Both serve **different analytical purposes** in intrusion analysis.


### ❓ Question

> What is the term that applies to the Diamond Model for organizations or people that are being targeted?

#### 🧪 Process

> **`Victim Personae`**
> - The **people or organizations** being targeted.

Trying this as the answer

#### ✅ Answer

- `Victim Personae` ✅



## 📘Capability

**Capability – Core Concept**
- Refers to the **skills, tools, and techniques** used by the adversary.
- Represents the adversary’s **Tactics, Techniques, and Procedures (TTPs)**.

**Range of Techniques**
- Can include
  - **Basic methods** like manual password guessing.
  - **Advanced techniques** such as custom malware development or use of malicious tools.

**Key Terms**
- **Capability Capacity**
  - The **vulnerabilities and exposures** that a specific capability can exploit.
- **Adversary Arsenal**
  - The **collection of capabilities** an adversary possesses.
  - The **combined capacity** of these capabilities defines the **arsenal’s strength**.

**Access to Capabilities**
- Adversaries must **possess or have access to** the required capabilities.
- These can include
  - Skills like **malware or phishing email development**.
  - Access to **capabilities-as-a-service**, such as **malware or ransomware for hire**.


### ❓ Question

> Provide the term for the set of tools or capabilities that belong to an adversary.

#### 🧪 Process

> **`Adversary Arsenal`**
>  - The **collection of capabilities** an adversary possesses.
>  - The **combined capacity** of these capabilities defines the **arsenal’s strength**.

Trying this as the answer

#### ✅ Answer

- `Adversary Arsenal` ✅



## 📘Infrastructure

**Infrastructure – Core Concept**
- Refers to the **physical or logical interconnections** used by the adversary.
- Supports the **delivery and control** of capabilities.
- Can include both **hardware and software** components.

**Examples of Infrastructure**
- **Command and Control (C2) centers**
- **IP addresses**, **domain names**, **email addresses**
- **Malicious USB devices** or other physical media

**Types of Infrastructure**
- **Type 1 Infrastructure**
  - **Owned or directly controlled** by the adversary.
- **Type 2 Infrastructure**
  - **Controlled by an intermediary**, who may or may not be aware.
  - Used to **obfuscate the source** and **hide attribution**.
  - Examples: malware staging servers, malicious domains, compromised email accounts.

**Service Providers**
- Organizations that **enable or support** adversary infrastructure.
- Examples
  - **Internet Service Providers (ISPs)**
  - **Domain registrars**
  - **Webmail providers**
- Critical for the **availability and operation** of both Type 1 and Type 2 infrastructures.

### ❓ Question 1

> To which type of infrastructure do malicious domains and compromised email accounts belong?

#### 🧪 Process

> **`Type 2 Infrastructure`**
> - **Controlled by an intermediary**, who may or may not be aware.

#### ✅ Answer

- `Type 2 Infrastructure` ✅


### ❓ Question 2

> What type of infrastructure is most likely owned by an adversary? 

#### 🧪 Process

> **`Type 1 Infrastructure`**
> - **Owned or directly controlled** by the adversary.

Trying this as the answer

#### ✅ Answer

- `Type 1 Infrastructure` ✅



## 📘Event Meta Features

**Meta-Features – Overview**
- **Optional** enhancements to the core Diamond Model.
- Provide **additional intelligence** and **context** to intrusion analysis.
- Six commonly used meta-features

1. **Timestamp**
- Records the **date and time** of the event.
- Can include **start and end times**.
- Helps identify **patterns**, **correlate events**, and **infer time zones** or **geographic origins** of attacks.

2. **Phase**
- Represents the **stage** of the intrusion or attack.
- Based on **Axiom 4**: every malicious activity has **two or more sequential phases**.
- Often aligned with the **Cyber Kill Chain**
  1. Reconnaissance  
  2. Weaponization  
  3. Delivery  
  4. Exploitation  
  5. Installation  
  6. Command & Control  
  7. Actions on Objective  
- Helps track the **progression** of an attack.

3. **Result**
- Captures the **outcome** or **post-condition** of the event.
- Can be labeled as
  - **Success**
  - **Failure**
  - **Unknown**
- May also reflect impact on the **CIA Triad**
  - **Confidentiality Compromised**
  - **Integrity Compromised**
  - **Availability Compromised**
- Useful for documenting **what was achieved** or **exfiltrated**.

4. **Direction**
- Describes the **flow of the attack** (host-based or network-based).
- Seven possible values
  - Victim-to-Infrastructure  
  - Infrastructure-to-Victim  
  - Infrastructure-to-Infrastructure  
  - Adversary-to-Infrastructure  
  - Infrastructure-to-Adversary  
  - Bidirectional  
  - Unknown

5. **Methodology**
- Describes the **general classification** of the intrusion.
- Examples
  - **Phishing**
  - **DDoS**
  - **Breach**
  - **Port scan**
- Helps categorize and **compare attack types**.

6. **Resources**
- External **requirements** needed for the intrusion to succeed.
- Can include
  - **Software** (e.g., Metasploit, OS)
  - **Knowledge** (e.g., how to use tools)
  - **Information** (e.g., credentials)
  - **Hardware** (e.g., servers, routers)
  - **Funds** (e.g., to buy domains)
  - **Facilities** (e.g., electricity, shelter)
  - **Access** (e.g., network paths, ISP access)


### ❓ Question

> What meta-feature does the axiom "Every malicious activity contains two or more phases which must be successfully executed in succession to achieve the desired result" belong to? 

#### 🧪 Process

> 2. **`Phase`**
> - Represents the **stage** of the intrusion or attack.
> - Based on **Axiom 4**: every malicious activity has **two or more sequential phases**.

Trying this as the answer

#### ✅ Answer

- `Phase` ✅


### ❓ Question

> You can label the event results as "success", "failure", and "unknown". What meta-feature is this related to? 

#### 🧪 Process

> 3. **`Result`**
> [...]
> - **Success**
> - **Failure**
> - **Unknown**

Trying this as the answer

#### ✅ Answer

- `Result` ✅


### ❓ Question

> To what meta-feature is this phrase applicable "Every intrusion event requires one or more external resources to be satisfied prior to success"? 

#### 🧪 Process

> 6. **`Resources`**
> - External **requirements** needed for the intrusion to succeed.

Trying this as the answer

#### ✅ Answer

- `Resources` ✅
