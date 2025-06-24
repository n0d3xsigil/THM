| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **SOC Fundamentals** |

## Contents
- [Introduction to SOC](#introduction-to-soc)
- [Purpose and Components](#purpose-and-components)
- [People](#people)
- [Process](#process)
- [Technology](#technology)


## 📘Introduction to SOC
A **Security Operations Centre** is a team who specialise in security. They monitor the organisation network and resources 24/7. with a view to identify suspicious activities.

### ❓ Question
> What does the term SOC stand for?
#### 🧪 Process
My first line, `Security Operations Centre`
#### ✅ Answer
- `Security Operations Center`


## 📘Purpose and Components
![](Images/Pasted%20image%2020250622202510.png)

### Detection
**Detect Vulnerabilities**
- Vulnerabilities may occur in OS, Software, Firmware. On PC's, Servers, Printers etc. The SOC will scan for vulnerabilities and report on mitigation progress, perhaps get involved should vulnerabilities not be patched.
**Detect unauthorised activity**
- Identify unauthorised activity such as log-ins, downloads, application usage etc. using hints such as geographic usage may help.
**Detect policy violations**
- Policies are put in place to protect the environment. Policy violations may include unauthorised software downloads, sharing user credentials, uploading sensitive company information
**Detect intrusion**
- An intrusion is an unauthorised access of the organisation resources. Maybe it may be a mis-configuration, or a exploited vulnerability.
### Response
**Support with the incident response**
- A process is followed should an incident be detected. The primary objective is to minimise impact and root cause analysis. 
- The SOC will also support the Incident Response Team

A mature SOC will have 3 pillars
![](Images/Pasted%20image%2020250622204609.png)

1. People
2. Process
3. Technology

### ❓ Question 1
> The SOC team discovers an unauthorized user is trying to log in to an account. Which capability of SOC is this?
#### 🧪 Process
Discovery would be a part of `Detection`

Trying this as the answer
#### ✅ Answer
- `Detection` ✅

### ❓ Question 2
> What are the three pillars of a SOC?
#### 🧪 Process
The three pillars
1) People
2) Process
3) Technology
#### ✅ Answer
- `People, Process, Technology`


## 📘People
![](Images/Pasted%20image%2020250623202817.png)


#### SOC Analyst (Level 1)
Analysts use security solutions to to detect threats. The _SOC Analyst (Level 1)_ would be the first point of response. They would triage the detection to determine if it was harmful. They would report detection's as per SOC procedures.

This could be considered a parallel to IT End User support where Level one would be the service desk, they will triage incoming incidents and forward them on to the correct team for the reported incident.

#### SOC Analyst (Level 2)
A _SOC Analyst (Level 2)_ would be responsible for investigating threats that that have been triaged by _Level 1_ but still need further analysis. They may access to additional experience and tools to allow a more thorougher  analysis. 

In my IT support example, this could be considered a Level 2 support agent, Whether that be a Desk Side support technician or a Network engineer, or even a software engineer, the level two agent will process the incident in accordance with their experience and speciality. 

#### SOC Analyst (Level 3)
Finally in the analyst role we have the _SOC Analyst (Level 3)_. The are proactive investigators, their experience allows them to look for threat indicators support response activities, will often handle critical severity detections which need swift responses, containment, eradication, and recovery.

In the IT end user support world, the 3rd line support would likely be Subject Matter Experts (SME) and will have the experience and expertise to support incidents and problems that 1st & 2nd line where unable to resolve. 

#### Security Engineer
The _Security Engineer_ would be responsible for deploying, configuring and maintaining the security solutions that the _SOC Analysts_ use to perform their investigations. It is their responsibility to ensure smooth operation of the solutions.

#### Detection Engineer
The security solutions need logic in order to detect malicious behaviours. The _Detection Engineer_  works with the analysts to implement the detection rules.

#### SOC Manager
At the top of the team we have the _SOC Manager_. They are responsible for the overall operation of the SOC by maintaining the processes and supporting the team. The _SOC Manager_ is also responsible for reporting to the CISO. 

### ❓ Question
> Alert triage and reporting is the responsibility of?
#### 🧪 Process
The triage is the responsibility of `Level 1`

Trying this as the answer
#### ✅ Answer
- `SOC Analyst (Level 1)` ✅

### ❓ Question
> Which role in the SOC team allows you to work dedicatedly on establishing rules for alerting security solutions?
#### 🧪 Process
The responsibility of tooling is that of the `Detection Engineer`

Trying this as the answer
#### ✅ Answer
- `Detection Engineer` ✅


## 📘Process
### Alert Triage
![](Images/Pasted%20image%2020250623213458.png)
The first function of the SOC team is triage. An alert will come in, the analyst determines the severity which in turn prioritises it. 

The `5w's` will help with this, using the example **Alert**: Malware detected on Host: `GEORGE PC`

|  `5 Ws`  | Answer                                                             |  
|----------|--------------------------------------------------------------------| 
| `What?`  | A malicious file was detected                                      |
| `When?`  | The file was detected at 13:20 on June 5, 2024                     |
| `Where?` | The file was detected  in the directory of the host `GEORGE PC`    |
| `Who?`   | The file was detected for the user George                          |
| `Why?`   | After the investigation, it was found that the file was downloaded |
|          | from a pirated software-selling website. The investigation with    |
|          | the user revealed that they downloaded the file as they wanted to  |
|          | use a software for free.                                           |

### Reporting
Malicious reports must be escalated to the next level analyst in a "_timely_" manor, often as incidents or tickets. The report should include details of all 5Ws including thorough analysis with screenshots as evidence.

### Incident Response and Forensics
**Reference Room**: [Incident Response Fundamentals](https://tryhackme.com/room/incidentresponsefundamentals)
Should the incident indicate a detection of "_highly malicious_" activity of a critical nature, high level teams will start an incident response. In such cases a detailed forensics activity will be performed which will determine the root cause.

### ❓ Question
> At the end of the investigation, the SOC team found that John had attempted to steal the system's data. Which 'W' from the 5 Ws does this answer?
#### 🧪 Process
This would be `who`. John had attempted an action.

Trying this as the answer
#### ✅ Answer
- `who` ✅

### ❓ Question
> The SOC team detected a large amount of data exfiltration. Which 'W' from the 5 Ws does this answer?
#### 🧪 Process
Data exfiltration is the process of moving data off of the network. This would be a `what` is happening kinda scinario.

Trying this as the answer
#### ✅ Answer
- `What` ✅


## 📘Technology
The third pillar of the SOC is _Technology_. We've spoken of the _technology_ already in terms of the _security solutions_. The _security solutions_ can be summarised as follows;

### SIEM
A _**S**ecurity **I**nformation_ and _**E**vent **M**anagement_ is a common solution used in the vast majority of SOCs. It collects logs from networked devices across the organisation. 

The SIEM solution will have been configured with logic to identify suspicious activity within the environment. The SIEM will log the detetions after verifying them across multiple sources (including logs, alerts etc) and should any match with the relevent rules will present alerts.

It isn't unusual for detections to be enhanced and processed with the aid of Machine Learning on more modern instances of SIEM installations.

### EDR
The _**E**ndpoint **D*etection_ and _**R*esponse_ provides detailed visability of networked devices activies, both in real-time and historical. The **EDR** is run on the endpoint (Users equipment) and can handle automated responses. It also has extensive capabiltiy for detections allowing for efficient processing.

### Firewall
The _firewall_ is a form of security (software or hardware) that acts as a barrier between the interior and exterior of a system, whether  that be an entire network or a single host. The _firewall_ can be configured with detection rules to help identify and if nessecary block suspicious traffic before entering the system.


### ❓ Question 1
> Which security solution monitors the incoming and outgoing traffic of the network?
#### 🧪 Process
The firewall is responsible for monitoring the traversal of traffic

Trying this as the answer
#### ✅ Answer
- `Firewall` ✅

### ❓ Question 2
> Do SIEM solutions primarily focus on detecting and alerting about security incidents? (yea/nay)
#### 🧪 Process
That is the primary function of the SIEM

Trying this as the answer
#### ✅ Answer
- `Yea` ✅

