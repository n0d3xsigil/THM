| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **Shells Overview** |

## Contents
- [Room Introduction](#room-introduction)


## Room Introduction
Shells, an important function in the attack chain. This room teaches us how to use different sells in offsec, and how to detect them.

There is no question for this task.


## Shell Overview
This section covers how users interact with Shells, with out a GUI. 

The types of shell are **Remove System Control**, having control over a target system, **Privilege Escalation**, having exploited a vulnerability to gain elevated permission, **Data Exfiltration** is where potentially sensitive data can be read and copied off of the target, **Persistence and Maintenaance access** describes continued access after the initial _Privilege Escalation_, **Post-Exploitation Activities** describes anything after the initial privilege escalation such as malware installation, data removal, account creation etc., **Access Other Systems on the Network** could be classed as pivoting, accessing other systems on the network.

### ❓ Question
> What is the command-line interface that allows users to interact with an operating system?
#### 🧪 Process
The command-line interface is often classed as a terminal, but in this context we are referring to a `Shell`.

Trying this as the answer
#### ✅ Answer
- `Shell` ✅

### ❓ Question
> What process involves using a compromised system as a launching pad to attack other machines in the network?
#### 🧪 Process
When we have established access, potentially elevated privilages, we may wish to pivot to other systems, otherwise known as `pivoting`.

Trying this as the answer
#### ✅ Answer
- `pivoting` ✅

### ❓ Question
> What is a common activity attackers perform after obtaining shell access to escalate their privileges?
#### 🧪 Process
Once you have established shell access, you will be limited to what you an do until you perform `Privilege Escalation`.

Trying this as the answer
#### ✅ Answer
- `Privilege Escalation` ✅