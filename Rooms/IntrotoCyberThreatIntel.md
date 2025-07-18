| [Home](../README.md) | [SOC Level 1](../SOClevel1.md) | **Intro to Cyber Threat Intel** |

# Intro to Cyber Threat Intel

## Contents
- [Introduction](#introduction)
- [Cyber Threat Intelligence](#cyberthreatintelligence)
- [CTI Lifecycle](#ctilifecycle)
- [CTI Standards & Frameworks](#cti-standards--frameworks)
- [Practical Analysis](#practical-analysis)



## 📘Introduction

In this room we go over CTI "_Cyber Threat Intelligence_" and the frameworks used to share intelligence. 


## 📘Cyber Threat Intelligence

Typical terms include the following

**Data** - indicators such as IP, hashes, urls etc.
**Information** - Multiple datapoints, such as how many times a site is accessed
**Intelligence** - The link between data and information

CTI's primary concern is understanding the relationship between the environment and the adversary. Greater context can be obtianed by _trying_ to answer the following questions
- **Who**'s attacking
- **What**s' their motivation
- **What**'s their capability
- **What** artificats and IOCs (Indications of Compromise) should we look out for?

An analyst would use intelligence from multiple sources from the following categories
- **Internal**
  - Internal security events (vulnerability assessments, incident reports)
  - Cyber awareness Training reports
  - Logs and events
- **Community**
  - Web forums
  - Dark web communities
- **External**
  - Threat intel feeds
  - Marketplaces
  - Public sources (Government, publications, social media, financial / industrial assesments)

Once we have our intelligence, we want to categorise them
- **Strategic Intel**
  - High-level intel that looks into the organisation’s threat landscape and maps out the risk areas based on trends, patterns and emerging threats that may impact business decisions.
- **Technical Intel**
  - Looks into evidence and artefacts of attack used by an adversary. Incident Response teams can use this intel to create a baseline attack surface to analyse and develop defence mechanisms.
- **Tactical Intel**
  - Assesses adversaries’ tactics, techniques, and procedures (TTPs). This intel can strengthen security controls and address vulnerabilities through real-time investigations.

Operational Intel: Looks into an adversary’s specific motives and intent to perform an attack. Security teams may use this intel to understand the critical assets available in the organisation (people, processes and technologies) that may be targeted.

### ❓ Question 1

> What does CTI stand for?

#### 🧪 Process

> `CTI -eq "Cyber Threat Intelligence"`

Trying this as the answer

#### ✅ Answer

- `Cyber Threat Intelligence` ✅


### ❓ Question 2

> IP addresses, Hashes and other threat artefacts would be found under which Threat Intelligence classification?

#### 🧪 Process

> _`Technical Intel`: Looks into evidence and artefacts of attack used by an adversary._

Trying this as the answer

#### ✅ Answer

- `Technical Intel` ✅



## 📘CTI Lifecycle

### 1. Direction

Define objectives and goals for the threat intelligence program.

- Critical information assets and business processes
- Potential impacts of losing assets or disrupting processes
- Data and intelligence sources for protection
- Tools and resources needed for defence
- Analysts also formulate key investigative questions during this phase.

### 2. Collection

Gather relevant data based on the defined objectives.

- Commercial sources
- Private feeds
- Open-source intelligence (OSINT)

Due to high data volumes, automation is recommended to free up analyst time for triage.

### 3. Processing

Standardise and structure diverse raw data (e.g., logs, malware, traffic).

- Extracting, sorting, tagging, and visualising data.
- Correlating and formatting for analyst use.
- SIEM tools are commonly used to support this stage.

### 4. Analysis

Analysts interpret the processed data to draw meaningful conclusions.

- Investigate threats using indicators and attack patterns
- Create defensive action plans
- Strengthen existing controls or justify further investment

### 5. Dissemination

Share intelligence with stakeholders in tailored formats:

- Executives: High-level summaries, trends, financial risks, strategic advice
- Technical teams: Detailed IOCs, TTPs, and tactical actions

### 6. Feedback

Stakeholders provide input to refine the intelligence cycle.

- Improving intelligence quality
- Enhancing security control implementation

Continuous communication between teams is essential to keep the cycle effective.

### ❓ Question 1

> At which phase of the CTI lifecycle is data converted into usable formats through sorting, organising, correlation and presentation?

#### 🧪 Process

> ### 3. `Processing`
> 
> Standardise and structure diverse raw data (e.g., logs, malware, traffic).

Trying this as the answer

#### ✅ Answer
 
- `Processing` ✅


### ❓ Question

> During which phase do security analysts get the chance to define the questions to investigate incidents?

#### 🧪 Process

> ### 1. Direction
> [...]
> - Analysts also formulate key investigative questions during this phase.

#### ✅ Answer

- `Direction` ✅



## 📘CTI Standards & Frameworks

Standards and frameworks **standardise threat intel sharing**, **enable collaboration**, and ensure **consistent terminology** across the cybersecurity industry. Below are key ones to know:


### MITRE ATT&CK

- A **knowledge base of adversary behaviour**, tactics, and techniques
- Helps analysts
  - Investigate attacks thoroughly
  - Track and understand adversarial behaviour
- Organised by **tactics** (the "why") and **techniques** (the "how")


### TAXII

The **T**rusted **A**utomated E**x**change of **I**ndicator **I**nformation framework

- A **protocol** for securely sharing threat intelligence
- Enables **near real-time** detection and response
- Supports two sharing models
  - **Collection**: Users pull intel from a server (request-response)
  - **Channel**: Server pushes intel to subscribers (publish-subscribe)


### STIX

The **S**tructured **T**hreat **I**nformation E**x**pression framework

- A **standard language** for representing cyber threat intel
- Allows consistent structuring of
  - Observables
  - Indicators
  - TTPs (Tactics, Techniques, and Procedures)
  - Attack campaigns and related threat data
- Enables better **interoperability** and **automation** of threat intel sharing.


### Cyber Kill Chain

- Breaks down cyber attacks into **seven sequential phases** to aid detection and defence:

| Phase                      | Purpose                                       | Examples                                 |
| -------------------------- | --------------------------------------------- | ---------------------------------------- |
| **Reconnaissance**         | Gather info about target                      | OSINT, network scans, social media       |
| **Weaponisation**          | Create attack tools tailored to the objective | Exploit kits, backdoors, malicious docs  |
| **Delivery**               | Transmit attack payload to victim             | Phishing emails, infected USBs           |
| **Exploitation**           | Execute code on the victim’s system           | EternalBlue, Zero-Logon                  |
| **Installation**           | Deploy malware and tools                      | RATs, credential dumpers                 |
| **Command & Control (C2)** | Remote control, lateral movement, escalation  | Cobalt Strike, Empire                    |
| **Actions on Objectives**  | Achieve attacker’s end goals                  | Data exfiltration, ransomware, espionage |

- Often **combined with ATT&CK** in modern analysis via the **Unified Kill Chain**.


### Diamond Model of Intrusion Analysis

- A framework for understanding and **pivoting between key elements** of an intrusion
  - **Adversary**: Who is conducting the attack? What’s their motive?
  - **Victim**: Who or what is being targeted?
  - **Infrastructure**: What systems or tools are used in the attack?
  - **Capabilities**: What TTPs and methods are being employed?
- Promotes **correlation of indicators** and **holistic attack understanding** over time.


### ❓ Question 1

> What sharing models are supported by TAXII?

#### 🧪 Process

> - Supports two sharing models
>   - **Collection**: Users pull intel from a server (request-response)
>   - **Channel**: Server pushes intel to subscribers (publish-subscribe)

So combining these we have `Collection and Channel`.

Trying this as the answer

#### ✅ Answer

- `Collection and Channel` ✅


### ❓ Question 2

> When an adversary has obtained access to a network and is extracting data, what phase of the kill chain are they on?

#### 🧪 Process

> **`Actions on Objectives`**
> - Achieve attacker’s end goals
> - Data exfiltration, ransomware, espionage

Trying this as the answer

#### ✅ Answer

- `Actions on Objectives`



## 📘Practical Analysis


### ❓ Question 1

> What was the source email address?

#### 🧪 Process

It is fiarly straight forward. But you will need to see the site

#### ✅ Answer

- `vipivillain@badbank.com` ✅


### ❓ Question 2

> What was the name of the file downloaded?

#### 🧪 Process

It is fiarly straight forward. But you will need to see the site

#### ✅ Answer

- `flbpfuh.exe` ✅


### ❓ Question 3

> After building the threat profile, what message do you receive?

#### 🧪 Process

It is fiarly straight forward. But you will need to see the site

#### ✅ Answer

- `THM{NOW_I_CAN_CTI}` ✅
