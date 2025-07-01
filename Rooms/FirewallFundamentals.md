| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **Firewall Fundamentals** |

# Firewall Fundamentals

## Contents
- [What Is the Purpose of a Firewall](#what-is-the-purpose-of-a-firewall)
- [Types of Firewalls](#types-of-firewalls)
- [Rules in Firewalls](#rules-in-firewalls)
- [Windows Defender Firewall](#windows-defender-firewall)


## 📘What Is the Purpose of a Firewall

### Room Prerequisites
Complete Room:
- ❓ [Networking Concepts](https://tryhackme.com/room/networkingconcepts)

Again, firewalls are largely a subject I am familia with. 

### ❓ Question 1

> Which security solution inspects the incoming and outgoing traffic of a device or a network?

#### 🧪 Process

The security solution is a `firewall`. 

Trying this as the answer

#### ✅ Answer

- `Firewall` ✅


## 📘Types of Firewalls

### Stateless Firewall
- Basic filtering
- No track of previous connections
- Efficient for high-speed networks

### Stateful Firewall
- Recognize traffic by patterns
- Complex rules can be applicable
- Monitor the network connections

### Proxy Firewall
- Inspect the data inside the packets as well
- Provides content filtering options
- Provides application control
- Decrypts and inspects SSL/TLS data packets

### Next-Generation Firewall (NGFW)
- Provides advanced threat protection
- Comes with an intrusion prevention system
- Identify anomalies based on heuristic analysis
- Decrypts and inspects SSL/TLS data packets


### ❓ Question 1

> Which type of firewall maintains the state of connections?

#### 🧪 Process

A `Stateful-Firewall`.

Trying this as the answer

#### ✅ Answer

- `Stateful-Firewall` ✅


### ❓ Question 2

> Which type of firewall offers heuristic analysis for the traffic?

#### 🧪 Process

A NGFW will do this.

Trying this as the answer

#### ✅ Answer

- `Next-generation firewall` ✅


### ❓ Question 3

> Which type of firewall inspects the traffic coming to an application?

#### 🧪 Process

A `Proxy firewall` will do

Trying this as the answer

#### ✅ Answer

- `Proxy firewall` ✅


## 📘Rules in Firewalls

Rules give administrators control how traffic flows in and out of the environemnt. 


### Types of Actions

#### Allow

_**Example**: Allow all traffic over port 443_

#### Deny

_**Example**: Deny all traffic over 22_

#### Forward

_**Example**: Forward traffic on port 80 to host `1.2.3.4`_

### Directionality of Rules

#### Inbound

Rules that allow or deny incomming traffic, most rules match this type of rule

#### Outbound

Rules that allow or deny outgoing traffic, maybe you want to block outbound telnet traffic (23)

#### Forward

I guess like NAT. Incomming traffic wants to hit 1.2.3.4:80 so you forward it to a specific web server within the environment.


### ❓ Question 1

> Which type of action should be defined in a rule to permit any traffic?

#### 🧪 Process

Any traffic? well permit would be `allow`

Trying this as the answer

#### ✅ Answer

- `Allow` ✅


### ❓ Question 2

> What is the direction of the rule that is created for the traffic leaving our network?

#### 🧪 Process

Entering `-eq` _Inbound_
Exiting `-eq` _Outbound_
#### ✅ Answer

- `Outbound` ✅


## 📘Windows Defender Firewall

### ❓ Question 1

> What is the name of the rule that was created to block all incoming traffic on the SSH port?

#### 🧪 Process

01. Open **Windows Defender Firewawith Advanced Security**
02. Click **Inbound Rules**
03. Scroll across to expose _Local Port_
04. Find port `22`
05. Verify _Action_ is **Block**
06. Take note of the Name (`Core OP`)

Trying this as the answer

#### ✅ Answer

- `Core OP` ✅


### ❓ Question 2

> A rule was created to allow SSH from one single IP address. What is the rule name?

#### 🧪 Process

07. Scroll across to expose _Local Port_
08. Find port `22`
09. Verify _Action_ is **Allow**
10. Take note of the Name (`Infra team`)

Trying this as the answer

#### ✅ Answer

- `Infra team` ✅


### ❓ Question 3

> Which IP address is allowed under this rule?

#### 🧪 Process
11. Take note of the IP (`192.168.13.7`)

Trying this as the answer

#### ✅ Answer

- `192.168.13.7` ✅
