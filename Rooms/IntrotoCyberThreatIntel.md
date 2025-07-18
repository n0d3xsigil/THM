| [Home](../README.md) | [SOC Level 1](../SOClevel1.md) | **Intro to Cyber Threat Intel** |

# Intro to Cyber Threat Intel

## Contents
- [Introduction](#introduction)
- [Cyber Threat Intelligence](#cyberthreatintelligence)
- [CTI Lifecycle](#ctilifecycle)



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
