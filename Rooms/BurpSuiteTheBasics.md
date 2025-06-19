| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **Burp Suite: The Basics** |

## Contents
- [Introduction](#introduction)
- [What is Burp Suite](#what-is-burp-suite)
- [Features of Burp Community](#features-of-burp-community)
- [Installation](#installation)
- [The Dashboard](#the-dashboard)
- [Navigation](#navigation)
- [Options](#options)
- [Introduction to the Burp Proxy](#introduction-to-the-burp-proxy)
- [Connecting through the Proxy (FoxyProxy)](#connecting-through-the-proxy-foxyproxy)


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
Honestly I'm not going to go through the process of installing burp suite. It is fairly straight forward on Windows, Linux, and Mac. The main thing to remember is to install the Burp CA cert to allow decription of https traffic and to either configure the client to use a proxy or use something like Foxy Proxy.


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


## Navigation
The initial navigation is done via the top menu. You can select select an alternative view by clicking on the tab name. 

Some functions have a sub menu such as:
**Target**
![image](https://github.com/user-attachments/assets/b0e7c79f-d88a-454c-bafc-036e2f117ed5)
**Proxy**
![image](https://github.com/user-attachments/assets/6d5e049d-f151-414f-860d-f1daa64a39e7)
**Sequencer**
![image](https://github.com/user-attachments/assets/06fe400b-9221-488b-9845-50aa8d428399)
**Extensions**
![image](https://github.com/user-attachments/assets/09f52b4b-883d-4afc-af8f-edf0e41f5e80)

You can detach the funcion by clicking and holding the function name and dragging it away from the menu, this will open a new window just for that function
![image](https://github.com/user-attachments/assets/43bfc5b4-df0e-4805-af8e-648335cac394)
![image](https://github.com/user-attachments/assets/a0ff8d14-bfad-46fd-82de-07c656e26809)

**Shortcuts**
There are also some useful shortcuts to help efficency.
- `Ctrl+Shift+D` will take you to the **Dashboard**
- `Ctrl+Shift+T` will take you to the **Target** tab
- `Ctrl+Shift+P` will take you to the **Proxy** tab
- `Ctrl+Shift+I` will take you to the **Intruder** tab
- `Ctrl+Shift+R` will take you to the **Repeater** tab

### ❓ Question
> Which tab **Ctrl + Shift + P** will switch us to?
#### 🧪 Process
As above:- `Ctrl+Shift+P` will take you to the **Proxy** tab 

Trying this as the answer
#### ✅ Answer
- `Proxy tab` ✅


## 📘Options
Within Burp Suite, there are two types of options
- Global Settings (user)
- Project Settings

**Global settings** are basically application settings, they effect how the application runs

**Project settings** are specific to the project that is in session. Think of _Project Settings_ as the active document

If you are running _Burp Suite Community Edition_ you don't have to worry about _Project Settings_ since this edition doesn't support it.

You can get to the **Global settings** by clicking **Burp** followed by **Settings**, or by clicking the **Settings** cog on the right hand side.

![image](https://github.com/user-attachments/assets/ecf54ba1-4e3b-450b-83b7-a29cb7071ba4)

![image](https://github.com/user-attachments/assets/2044405a-f3d4-4160-895f-aeff8ec11ed8)


On the left you have **Search** to fnid a specific tool (`Cookie Jar`)

![image](https://github.com/user-attachments/assets/e3beadeb-c71f-4a34-a345-23fead41f4ad)

You can filter the searches on _User_ or _Project_ results.

Clicking a specific settings within a function is a shortcut to the settings to that specific function. Such as `Sequencer settings`

![image](https://github.com/user-attachments/assets/f19d9489-fc3f-4ff8-9f33-07f41097c6a6)

It is very much worth going through the settings to famaliarise yourself.

### ❓ Question
> In which category can you find a reference to a "Cookie jar"?
#### 🧪 Process
We can either look through the categories for the "Cookie Jar", perform a search for "Cookie Jar" as per the above search example, or take a wild guess that cookies store session data. 

Either way, it is Sessions.

Trying this as the answer
#### ✅ Answer
- `Sessions` ✅

### ❓ Question
> In which base category can you find the "Updates" sub-category, which controls the Burp Suite update behaviour?
#### 🧪 Process
You can find updates under Suite → Updates.

Trying this as the answer
#### ✅ Answer
- `Suite` ✅

### ❓ Question
> What is the name of the sub-category which allows you to change the keybindings for shortcuts in Burp Suite?
#### 🧪 Process
I'm assuming we're calling Hotkeys keybindings. If so this can be found under User Interface → Hotkeys.

Trying this as the answer
#### ✅ Answer
- `Hotkeys` ✅

### ❓ Question
> If we have uploaded Client-Side TLS certificates, can we override these on a per-project basis (yea/nay)?
#### 🧪 Process
Under Tools → Proxy you can see a description of the CA certificate. It states "Each installation of Burp generates its own CA certificate". It doesn't mention project.

I'm going to to with Nay

Trying this as the answer
#### ✅ Answer
- `Nay` ❌

Well, it must be a Yea then.

- `Yea` ✅


## 📘Introduction to the Burp Proxy
The proxy is the crucial function of Burp, when enabled, it captures requests and resposes. Once intercepted we can forward the traffic to the modules for processing.

Click Intercept to enable interception

![image](https://github.com/user-attachments/assets/46784ea1-c2be-4dc7-8801-3d8dd211634c)

With the Burp Proxy enabled testers can take **control** of the traffic to enable testing, the proxy will capture and log traffic where you can decide how to proceed with each request. 

All requests are logged regardless of if the interception is enabled or not. 
![](Images/Pasted%20image%2020250619171156.png)

In addition to that, both **HTTP** and **WebSockets** history is saved providing additional investigation data.

![](Images/Pasted%20image%2020250619171654.png)

![](Images/Pasted%20image%2020250619171711.png)

We can use the **Match and Replace** feature to to modify requests, including dynamic manipulation


## Connecting through the Proxy (FoxyProxy)
We mentioned FoxyProxy earlier. FoxyProxy is an extenesion that allows easy enabling and disabling of a proxy. 



