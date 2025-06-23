| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **SOC Fundamentals** |

## Contents
- [Introduction to SOC](#introduction-to-soc)
- [Purpose and Components](#purpose-and-components)
- [People](#people)


## Introduction to SOC
A **Security Operations Centre** is a team who specialise in security. They monitor the organisation network and resources 24/7. with a view to identify suspicious activities.

### ❓ Question
> What does the term SOC stand for?
#### 🧪 Process
My first line, `Security Operations Centre`
#### ✅ Answer
- `Security Operations Center`


## Purpose and Components
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


## People
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