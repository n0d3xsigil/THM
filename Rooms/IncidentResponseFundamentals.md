| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **Incident Response Fundamentals** |

# Incident Response Fundamentals

## Contents
- [Room Pre-Requisites](#room-pre-requisites)
- [Introduction to Incident Response](#introduction-to-incident-response)
- [What are Incidents?](#what-are-incidents)


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




