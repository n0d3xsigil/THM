| [Home](../README.md) | [SOC Level 1](../SOClevel1.md) | **OpenCTI** |

# OpenCTI

## Contents
- [Room Overview](#room-overview)
- [Introduction to OpenCTI](#introduction-to-opencti)
- [OpenCTI Data Model](#opencti-data-model)
- [OpenCTI Dashboard 1](#opencti-dashboard-1)

## 📘Room Overview

Nothing much of note here



## 📘Introduction to OpenCTI

- **OpenCTI** is a free, open-source platform for managing Cyber Threat Intelligence (CTI).
- It helps organisations **store**, **analyse**, **visualise**, and **link** threat data (like malware, threat actors, IOCs).
- Created by **ANSSI** (France’s cybersecurity agency) and **CERT-EU**.
  - [Agence nationale de la sécurité des systèmes d'information](https://cyber.gouv.fr/)
- Uses standards like **STIX2** and supports **MITRE ATT&CK** for structuring intel.
- Can integrate with other tools like **MISP** (threat sharing) and **TheHive** (incident response).
- Designed to make complex threat data easier to understand and act on.


## 📘OpenCTI Data Model

- **Data Model:**
  - OpenCTI is built around **STIX2**, a standard format for sharing cyber threat intelligence using **entities** and **relationships** (e.g. threat actor → uses → malware).

- **Architecture Highlights:**
  - **GraphQL API:** Connects users to the database and messaging system.
  - **Write Workers:** Python processes that handle write operations via RabbitMQ.
  - **Connectors:** Extend OpenCTI by importing, enriching, or exporting data.

**Connector Types:**

  | Class                   | Description                       | Examples              |
  | ----------------------- | --------------------------------- | --------------------- |
  | **External Input**      | Ingests external threat data      | MISP, CVE, TheHive    |
  | **Stream**              | Subscribes to live data changes   | Tanium, History       |
  | **Internal Enrichment** | Adds context to new entries       | Observable enrichment |
  | **Internal Import**     | Extracts data from uploaded files | PDFs, STIX2           |
  | **Internal Export**     | Outputs data in various formats   | CSV, PDF, STIX2       |

> Together, the STIX2 model and modular architecture make OpenCTI flexible and scalable for real-world CTI workflows.



## 📘OpenCTI Dashboard 1


### **Dashboard**

- Displays widgets summarising platform activity:
  - Total entities, reports, observables, relationships
  - Changes in the past 24 hours



### **Activities & Knowledge**

- **Activities:** Shows incidents and reports for investigation
- **Knowledge:** Displays threat actors, campaigns, tools, and targets



### **Analysis**

- Hosts reports that group intel on threats
- Analysts can add notes, external links, and enrich content
- Example: Review the Triton Software report from MITRE



### **Events**

- Analysts document and track suspicious/malicious activity
- Enables incident tracking and linking to known threats



### **Observations**

- Focuses on technical artefacts and detection rules
- Helps correlate internal findings with threat intel feeds



### **Threats**

- Houses core adversarial elements:
  - **Threat Actors** – Individuals or groups behind attacks
  - **Intrusion Sets** – Known attack patterns (e.g. APTs)
  - **Campaigns** – Coordinated attack series with specific goals



### **Arsenal**

- Lists tools and techniques used by attackers:
  - **Malware** – e.g., 4H RAT
  - **Attack Patterns** – TTPs used in campaigns
  - **Courses of Action** – Defensive strategies
  - **Tools** – Legitimate software used (and misused)
  - **Vulnerabilities** – CVEs from MITRE via connector



### **Entities**

- Categorises data by:
  - **Sectors**, **countries**, **organisations**, and **individuals**
  - Useful for profiling targets or mapping attack surfaces



### ❓ Question 1

> What is the name of the group that uses the **4H RAT** malware?

#### 🧪 Process

Navigate and login to the OpenCTI dashboard.
Click **Arsenal**
In the _search_ box type `4H` and **Press** return on your keyboard
Click the **4H RAT** result
On the right-hand side you'll see **Details**

Under this you'll see the following text

> _4H RAT is malware that has been used by `Putter Panda`_
> _since at least 2007. (Citation: CrowdStrike Putter Panda)_

Trying this as the answer

#### ✅ Answer

- `Putter Panda` ✅



### ❓ Question 2

> What kill-chain phase is linked with the **Command-Line Interface** Attack Pattern? 

#### 🧪 Process

This one had me stumped for a while, even the hint didn't really give me the pointer I was hoping for. However, the truth is, I had looked multiple times, but hadn't notice a subtle layout change. So:

Search for **`Command-Line Interface`**
Expand **Attack Pattern**
Review the matches
- Hint, its the second one.

 I was looking for _kill-chain_ at the bottom of the **Details** section, in this attack pattern, it is at the top, `execution-ics`

Trying this as the answer

#### ✅ Answer

- `Execution-ics` ✅



### ❓ Question 3

> Within the Activities category, which tab would house the **Indicators**?

#### 🧪 Process

_process_

#### ✅ Answer

- `Observations` ✅
