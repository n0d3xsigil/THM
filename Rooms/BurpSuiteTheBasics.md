| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **Burp Suite: The Basics** |

## Contents
- [Introduction](#introduction)
- [What is Burp Suite](#what-is-burp-suite)
- [Features of Burp Community](#features-of-burp-community)
- [Installation](#installation)
- [The Dashboard](#the-dashboard)


## 📘Introduction
This room covers
- Intro to Burp Suite
- Overview of built in tools
- Installation process
- Configuration and navigation of the features

We dive into the core Burp Suite framework the Burp Proxy. Since the room is a foundational resource to the Burp Suite, this room is largly therotical. There are more practical rooms elsewhere within TryHackMe.

If this is your first time with Burp Suite, make sure to take care and get farmillia as you go. Experementation will go along way to learning the tool which will become essential should you take on more rooms within TryHackMe, especially Web Application based rooms


## What is Burp Suite
Burp Suite
- Based on Java
- Conduct WebApp Pentesting
- Indrustry standard
- Both Web and Mobile
 
Burp Suite works by capturing web requests of both https and http traffic going between client and server. By intercepting the requests the investigator is able to forward them to various tools within the framework. This functionality of intercepting requests, viewing, and if desired modifigying is what makes Burp Suite indespensible.

There are 3 editions of Burpsuite
- Burp Suite Community Edition
  - Free to use (non commercially)
- Burp Suite Professional
  - Automated Vuln Scanner
  - No rate limits
  - Save projects
  - API
  - Extensions
  - Burp Suite Collaborator
- Burp Suite Enterprise
  - Continous scanning
  - Similar to Nesus Infra scanner
  - Installed on server

  ### ❓ Question
> Which edition of Burp Suite runs on a server and provides constant scanning for target web apps?
#### ✅ Answer
> - `Burp Suite Enterprise` ✅

### ❓ Question
> Burp Suite is frequently used when attacking web applications and ______ applications.
#### ✅ Answer
- `Mobile` ✅


## 📘Features of Burp Community
![image](https://github.com/user-attachments/assets/d463eb8d-c23b-496b-acf2-ba127cb43977)

**Burp Suite Community Edition** - The basic one. But still a very valid option for your toolkit.
- **Price**: £0.00/$0.00 (for not profit organisations)
 - Great for learning etc.

**Included Features**
- **Proxy**:
 - The core of Burp Suite, acts as a **Man-in-the-Middle** of web requests which enables the functionality below
- **Repeater**:
 - Allows you to modify captured traffic and resend multiple times allowing for an iterative approach to payload crafting, such as SQL injection or Input Validation testing
 - **See**: [Burp Suite: Repeater](https://tryhackme.com/room/burpsuiterepeater)
- **Intruder**:
 - Allows spraying endpoints with requests as part of brute force attacks or fuzzing.
 - **See**: [Burp Suite: Intruder](https://tryhackme.com/room/burpsuiteintruder)
- **Decoder**:
 - Used for encoding payloads before sending them to a tarter also allows for decoding of encoded data for easier review.
 - **See**: [Burp Suite: Other Modules](https://tryhackme.com/room/burpsuiteom)
- **Comparer**:
 - Allows comparison of two bits of data. Can be compared at byte or word level.
 - **See**: [Burp Suite: Other Modules](https://tryhackme.com/room/burpsuiteom)
- **Sequencer**:
 - Assists in assessing randomness of tokens or suspected random data. May identify weaknesses in token randomness allowing for exploitation.
 - **See**: [Burp Suite: Other Modules](https://tryhackme.com/room/burpsuiteom)

Extensions can be written in Java, Python, or Ruby. BApp store has plenty of 3rd party modules suitable for Burp Suite Community Edition. One highlighted extension is **Logger++**

 ### ❓ Question
> Which Burp Suite feature allows us to intercept requests between ourselves and the target?
#### 🧪 Process
The interceptopn is done by the proxy.

Trying this as the answer
#### ✅ Answer
- `proxy` ✅

### ❓ Question
> Which Burp tool would we use to brute-force a login form?
#### 🧪 Process
The process of brute-force attacks can be done using the Intruder module.

Trying this as the answer
#### ✅ Answer
- `Intruder` ✅


## 📘Installation
Honestly I'm not going to go through the process of installing burp suite. It is fairly straight forward on Windows, Linux, and Mac. The main thing to remember is to install the Burp root cert to allow decription of https traffic and to either configure the client to use a proxy or use something like Foxy Proxy.


## 📘The Dashboard
The following is my experience and not that documented on the TryHackMe room.

![image](https://github.com/user-attachments/assets/63ec1b77-1ad2-46f5-b2b9-d309c0711d49)

When first openeing Burp Suite you will be presented with two windows. **Tasks** on the left and **Live passive crawl** on the right. The TryHackMe room suggests that _Event log_ and _Advisory_. To open the **Event log** click the _Event log_ button on the bottom right of the screen.
![image](https://github.com/user-attachments/assets/7cba47f2-979e-4796-bbc3-05281d449b6d)

1. The **Tasks** pane allows you to define tasks that will run whilst using the application.
2. The **Event log** pane provides a view of what actions have been performed by Burp Suite

In addition to these two the room mentiones
1. **Issue Activity** (Burp Suite Professional) displays vulnerabilities idnetified by scanning.
2. **Advisory** provides more details about the vulnerabilities mentioned in the _Issue Activity_. Also not a part of Burp Suite Community Edition

### ❓ Question
> What menu provides information about the actions performed by Burp Suite, such as starting the proxy, and details about connections made through Burp?
#### 🧪 Process
The event log captures information about the activites of Burp Suite

Trying this as the answer
#### ✅ Answer
- `Event log` ✅
