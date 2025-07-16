| [Home](../README.md) | [SOC Level 1](../SOClevel1.md) | **Eviction** |

# Eviction

## Contents
- [Understand the adversary](#understandtheadversary)



## 📘Understand the adversary

We're told to use the MITRE

### ❓ Question 01

> What is a technique used by the APT to both perform recon and gain initial access?

#### 🧪 Process

This took a moment to realise that actually the items that are highlighted are infact the TTP that the APT28 used.

Once I realise that I then compaired common entries between **Reconnaissance** and **Initial Access**.

The only common item was `Spearphishing link`.

Trying this as the answer

#### ✅ Answer

- `Spearphishing link`✅


### ❓ Question 02

> Sunny identified that the APT might have moved forward from the recon phase. Which accounts might the APT compromise while developing resources?

#### 🧪 Process

Okay, hopfully I have my swing now...

Looking at **Iniital Access** ➡️ **Valid Accounts** ➡️ `Cloud Accounts`

Trying this as the answer

#### ✅ Answer

- `Cloud Accounts` ❌

Okay, rethink time.

> _APT might have moved forward from the recon phase_

What is the next phase? **Resource Development** ➡️ **Compromise Accounts** ➡️ `Email Accounts`

Trying this as the answer

- `Email Accounts` ✅


### ❓ Question 03

> E-corp has found that the APT might have gained initial access using social engineering to make the user execute code for the threat actor. Sunny wants to identify if the APT was also successful in execution. What two techniques of user execution should Sunny look out for? (Answer format: <technique 1> and <technique 2>)

#### 🧪 Process

> _What two techniques of user execution should Sunny look out for?_

So there is no **User Execution** in Initial Access, however under **Execution** we have `Malicious File` and `Malicious Link`

Trying this as the answer

#### ✅ Answer

- `Malicious file and malicious link` ✅


### ❓ Question 04

> If the above technique was successful, which scripting interpreters should Sunny search for to identify successful execution? (Answer format: <technique 1> and <technique 2>)

#### 🧪 Process

> _which scripting interpreters should Sunny search for to identify successful execution_

Key words, "_Scripting Interpreters_" and "_execution_"

Under **Execution** we have "_Command and Scripting Interpreter_" so thats Promising. 

This leaves us with `PowerShell and Windows Command Shell`

Trying this as the answer

#### ✅ Answer

- `PowerShell and Windows Command Shell` ✅


### ❓ Question 05

> While looking at the scripting interpreters identified in Q4, Sunny found some obfuscated scripts that changed the registry. Assuming these changes are for maintaining persistence, which registry keys should Sunny observe to track these changes?

#### 🧪 Process

I'll just pick out the key words here
- "**registry**"
- "**changed**"
- "**maintaining persistence**"

I'm going to go with **Persistence** ➡️ `Registry Run Keys`.

Trying this as the answer

#### ✅ Answer

- `Registry Run Keys` ✅


### ❓ Question 06

> Sunny identified that the APT executes system binaries to evade defences. Which system binary's execution should Sunny scrutinize for proxy execution?

#### 🧪 Process

I'm thinking about:
- "**Defense evasion**"
- "**Proxy**"

We've got **Defense Evasion** ➡️ **System Binary Proxy Execution** ➡️ `Rundll32`.

Trying this as the answer

#### ✅ Answer

- `Rundll32` ✅


### ❓ Question 07

> Sunny identified tcpdump on one of the compromised hosts. Assuming this was placed there by the threat actor, which technique might the APT be using here for discovery?

#### 🧪 Process

We're going to be moving in to the "_discovery_" phase. TCP Dump is a packet capture tool. I think it's safe to say the technique is `Network Sniffing`.

Trying this as the answer

#### ✅ Answer

- `Network sniffing` ✅


### ❓ Question 08

> It looks like the APT achieved lateral movement by exploiting remote services. Which remote services should Sunny observe to identify APT activity traces?

#### 🧪 Process

So "_Lateral Movement_" or pivoting means to attempt to access another machine. We already know it is likely we are talking about a **Windows** envionment given `powershell` etc.

As we're looking at **Remove Services** it can only be `SMB/Windows Admin Shares`.

Trying this as the answer

#### ✅ Answer

- `SMB/Windows Admin Shares` ✅


### ❓ Question 09

> It looked like the primary goal of the APT was to steal intellectual property from E-corp's information repositories. Which information repository can be the likely target of the APT?

#### 🧪 Process

So we're now in the **collection** phase. I can only see one "_information repository_" and that is `Sharepoint`.

Trying this as the answer

#### ✅ Answer

- `answer`


### ❓ Question 10

> Although the APT had collected the data, it could not connect to the C2 for data exfiltration. To thwart any attempts to do that, what types of proxy might the APT use? (Answer format: <technique 1> and <technique 2>)

#### 🧪 Process

Under **Command and Control** we have two types of "_Proxy_"'s `External Proxy` and `Multi-hop Proxy`. 

Trying this as the answer

#### ✅ Answer

- `External Proxy and Multi-hop Proxy` ✅


### ❓ Question 11

> Congratulations! You have helped Sunny successfully thwart the APT's nefarious designs by stopping it from achieving its goal of stealing the IP of E-corp.

#### 🧪 Process

I didn't realise this wasn't a question ``.

Not trying this as the answer

#### ✅ Answer

- `` 😉
