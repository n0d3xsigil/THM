| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **Logs Fundamentals** |

# Logs Fundamentals

## Contents
- [Introduction to Logs](#introduction-to-logs)
- [Types of Logs](#types-of-logs)
- [Windows Event Logs Analysis](#windows-event-logs-analysis)
- [Web Server Access Logs Analysis](#web-server-access-logs-analysis)
- [Conclusion](#conclusion)



## 📘Introduction to Logs

Logging is a key part of discovering attacks, but many attackers will cover their tracks on the target system. Often attackers will try and conceil tracks. However systems often have many places logs are saved.

Lets take a look at some use cases of logs.

|               Use Case               |                   Description                   |
|--------------------------------------|-------------------------------------------------|
| Security Events Monitoring           | Logs help detect anomolies                      |
| Incident Investigation and Forensics | Captures traces and detailed information        |
| Troubleshooting                      | Captures errors which can aid with resolutions  |
| Performance Monitoring               | Insights into performance                       |
| Audting and Compliance               | audit trails                                    |



### ❓ Question

> Where can we find the majority of attack traces in a digital system?

#### 🧪 Process

Since the scope of this room is logging, I guess we can say `Logs`

Trying this as the answer

#### ✅ Answer

- `Logs` ✅



## 📘Types of Logs

Each system will have an overwhelmning number of loggs of various categories. Thankfully the Windows Operating System has a feature called the Eventlog, or Event Viewer. This tool helps categorise logs of different sources into a log type. Let's take a look at the categories below

- **System Logs** - Usefull in troubleshooting OS level issues.
    - System startups
    - System shutdowns
    - Driver events
    - System errors
    - Hardware events

- **Security Logs** - Events around the security of the system, helps detect and investigate
    - Authentication events
    - Authorisation events
    - Security policy changes
    - User account changes
    - Abnormal activity

- **Application Logs** - Events specifically around application events. Interactive or non interactive
    - User interaction events
    - Application changes events
    - Application update events
    - Application error events

- **Audit Logs** - Detailed information about system and user changes. Often not enabled.
    - Data Access events
    - System Change events
    - User Activity events
    - Policy Enforcement events

- **Network Logs** - Information around inbound and outbound traffic. key troubleshooting and investigation events
    - Incoming network traffic events
    - Outgoing network traffic events
    - Network connection logs
    - Network firewall logs


- **Access Logs** - Information about the locations being accessed on and off-prem
    - Webserver access logs
    - Database acccess logs
    - Application access logs
    - API access logs


### ❓ Question 1

> Which type of logs contain information regarding the incoming and outgoing traffic in the network?

#### 🧪 Process

If it is relating to the network, it is probably `Network logs`

Trying this as the answer

#### ✅ Answer

- `Network logs` ✅


### ❓ Question

> Which type of logs contain the authentication and authorization events?

#### 🧪 Process

Authorisation and authorisation events fall under the `Security logs` category.

Trying this as the answer

#### ✅ Answer

- `Security logs` ✅




## 📘Windows Event Logs Analysis

I am quite familiar windows event logs so I won't go in to too much detail with this section. 

Use **Event ID**'s

| Event ID |                    Description                     |
|----------|----------------------------------------------------|
| 4624     | A user account successfully logged in              |
| 4625     | A user account failed to login                     |
| 4634     | A user account successfully logged off             |
| 4720     | A user account was created                         |
| 4724     | An attempt was made to reset an account’s password |
| 4722     | A user account was enabled                         |
| 4725     | A user account was disabled                        |
| 4726     | A user account was deleted                         |

### ❓ Question 1

> What is the name of the last user account created on this system?

#### 🧪 Process

01. Click **Start Machine**
02. In the machine, click **Start** and type `eventvwr.msc`
03. Expand **Windows Logs**
04. Click **Security**
05. Once within the _Security_ event logs click **Filter Current Log...**
06. Paste `4720` in _`<All Event IDs>`_ and click **OK**
07. The most recent account creation was on the 6th July 2024 at 12:56:27.
08. **Account Name:** `hacked`

Trying this as the answer

#### ✅ Answer

- `hacked` ✅


### ❓ Question 2

> Which user account created the above account?

#### 🧪 Process

09. We can see the account making the change was `Administrator`

Trying this as the answer

#### ✅ Answer

- `Administrator` ✅


### ❓ Question 3

> On what date was this user account enabled? Format: M/D/YYYY

#### 🧪 Process

10. Okay, so I assumed it was D/M/YYYY so its 7th June 2024

Trying this as the answer

#### ✅ Answer

- `6/7/2024` ✅


### ❓ Question 4

> Did this account undergo a password reset as well? Format: Yes/No

#### 🧪 Process

11. Click **Filter Current Log...**
12. Paste `4724` in _`<All Event IDs>`_ and click **OK**
13. 2 entried suggest and attempt was made

Trying this as the answer

#### ✅ Answer

- `Yes` ✅




## 📘Web Server Access Logs Analysis




## 📘Conclusion
