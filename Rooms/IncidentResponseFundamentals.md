| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **Incident Response Fundamentals** |

# Incident Response Fundamentals

## Contents
- [Room Pre-Requisites](#room-pre-requisites)
- [Introduction to Incident Response](#introduction-to-incident-response)
- [What are Incidents?](#what-are-incidents)
- [Types of Incidents](#types-of-incidents)
- [Incident Response Process](#incident-response-process)


## 📘Room Pre-Requisites

### ❓[Defensive Security Intro](DefensiveSecurityIntro.md)


## 📘Introduction to Incident Response

The purosed of an Incident response is to handle an incident from beginning to end. From detection, resolution and prevention. 


## 📘What are Incidents?

Have you ever looked at the logging on your computer, for example `syslog` for **Linux** or `EventLog` for **Windows**, if so you will have noticed there are potentially tens or even hundreds of thousands of events. These could contain anything like start up or shutdown events, or logon events, maybe even crash logs. There will also be security events. 

These events could point to some malacious behaviour on the computer. 

How will you know, well there could be a security solution installed that monitors the events and generates alerts. These alerts will then be forwarded to the security and process them accordningly depending on if they are classed as a **False positive** or a **True positive**. 

Lets discuss what the difference between **False** and **True** _positives_ are. 

### ⚠️ False positives

A false positive is when an alert is generated for a malicious activity, but on investigation is an genuine activity. For example a client is perofrming port scanning across the network. on investigation a member of the team is trialing a vulnerability scanner. 

### 🛑 True Positives

A true positive is when an alert is genrated for a malicious activity and up on furhter investigation it is confirmed to be malicious. For example, a user is uploading large quantities of data to an offsite server, when investigating the security team finds that the employee is copying database information to a competitor. 

_True positives_ would then move from an alert to an inciden, it will be given a severity and actioned in order of severity.

### ❓ Question 1

> What is triggered after an event or group of events point at a harmful activity?

#### 🧪 Process

An event would be converted to an `alert

Trying this as the answer

#### ✅ Answer

- `Alert` ✅


### ❓ Question 2

> If a security solution correctly identifies a harmful activity from a set of events, what type of alert is it?

#### 🧪 Process

If the event is confirmed to be malicious, it would be classified as a `True positive`

Trying this as the answer

#### ✅ Answer

- `True positive` ✅


### ❓ Question 3

> If a fire alarm is triggered by smoke after cooking, is it a true positive or a false positive?

#### 🧪 Process

Lets assume that since we have finished cooking, we have turned off any heat sources. In that case it would be a `False positive`

Trying this as the answer

#### ✅ Answer

- `False positive` ✅



## 📘Types of Incidents

### 🦠 Malware Infections

A malware infection is a result of a malicious application or peice of software that has been downloaded on the machine. Untill Recently it was thought that you would have to 'click' something in order to infect a machine, however this is not nessecarily the case any more. The purpose of malware is to often obtain information from the machine, or perhaps cause disruption to the machine or event the wider network.

### 🔓 Security Breaches

A security breach is when as it sounds, the security of a system is circumvented. An attacker has gained access where they should not have. The end result could be the same as with a malware infection. Access to confidential information, cause disruption etc.

### 🩸 Data Leaks

A data leak is interesting, whilst as before with a _malware infection_ and _security breach_ a **data leak** could possibly be an inadvertant action, such as human error, or misconfiguration.

### 🕵️‍♂️ Insider Attacks

An insider threat could be considered as any individual who has internal access to the network. Again my or may not be deliberatly malicious.

### 🌐 Denial Of Service Attacks

DoS is the act of an attacker making a service appear to be unavailable by sending large quantities of data to a target whelming its capability to accept and respont to queries. Since availability is a key tennant of cyber seucirty.


### ❓ Question 1

> A user's system got compromised after downloading a file attachment from an email. What type of incident is this?

#### 🧪 Process

Downloaded malware, that would be a `Malware Infection`.

Trying this as the answer

#### ✅ Answer

- `Malware Infection` ✅


### ❓ Question 2

> What type of incident aims to disrupt the availability of an application?

#### 🧪 Process

Availablilty `Denial of Service`

Trying this as the answer

#### ✅ Answer

- `Denial of Service` ✅


## 📘Incident Response Process

We can use _Incident Response_ frameworks to ensure effective responses to incidents. The two of the common frameworks are **SANS** and **NIST**.

_However_ since I am based in the UK. I also would like to bare in mind the regional specific frameworks such as:
- NCSC
- CREST Framework
- ISO/IEC 27035

Let's continue to focus on the THM perspective first.

Both SANS and NIST contribute to cyber security, SANs has lots of courses and certfication. NIST has been key in developing standards for cyber security. 

Focusing on SANS a moment, there are 6 phases to the response framework. PICERL:

![image](https://github.com/user-attachments/assets/111893fc-790f-49e5-b0a3-e61423e097f6)

|      Phase      |                 Explanation                 |                 Example                 |
|-----------------|---------------------------------------------|-----------------------------------------|
| Preparation     | First phase building tools and teams        | Awareness training etc                  |
| Identification  | Highlight abnormal behaviours using tools   | Data exfiltration, indicates compromise |
| Containment     | Isolate target, disable accounts etc        | Disable user access to machine          |
| Eradication     | Removal of threat, ensure clean environment | Deep malware scan                       |
| Recovery        | rebuilding targets                          | Compromised host rebuild for backup     |
| Lessons Learned | Document, close gaps                        | post incident review and documentation  |

Whilst the NIST response is simplified to just 4 steps.

![image](https://github.com/user-attachments/assets/517ba57e-b135-46b7-88f5-1fb02556a7d7)

As a comparison, Containment, Eradication and Recovery are combined as one phase in NIST compaired with SANS
![image](https://github.com/user-attachments/assets/edae8556-4294-4e36-839e-71c545cfdb90)


### ❓ Question 1

> The Security team disables a machine's internet connection after an incident. Which phase of the SANS IR lifecycle is followed here?

#### 🧪 Process

In this step we are contianing the it the incident

Trying this as the answer

#### ✅ Answer

- `Containment` ✅


### ❓ Question 2

> Which phase of NIST corresponds with the lessons learned phase of the SANS IR lifecycle?

#### 🧪 Process

In NIST, the last step is Post Incident Activity.

Trying this as the answer

#### ✅ Answer

- `Post Incident Activity` ✅



