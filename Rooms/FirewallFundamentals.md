| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **Firewall Fundamentals** |

# Firewall Fundamentals

## Contents
- [What Is the Purpose of a Firewall](#what-is-the-purpose-of-a-firewall)
- [Types of Firewalls](#types-of-firewalls)


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


