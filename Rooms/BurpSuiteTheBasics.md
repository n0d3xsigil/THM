| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **Burp Suite: The Basics** |

## Contents
- [Introduction](#introduction)
- [What is Burp Suite](#what-is-burp-suite)


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
