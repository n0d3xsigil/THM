| [Home](../README.md) | [SOC Level 1](../SOClevel1.md) | **OpenCTI** |

# OpenCTI

## Contents
- [Room Overview](#room-overview)
- [Introduction to OpenCTI](#introduction-to-opencti)
- [OpenCTI Data Model](#opencti-data-model)

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
