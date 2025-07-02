| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **IDS Fundamentals** |

# IDS Fundamentals

## Contents
- [What Is an IDS](#what-is-an-ids)
- [Types of IDS](#types-of-ids)



## 📘What Is an IDS

An IDS is different in from a firewall in that a firewall will deny traffic if it doesnt meet specific criteria. But what if you have port 80 open. Well any traffic that is using port 80 can slip through the net.

Instead, an IDS will monitor this activity and report on it. Rather than preventing (IPS) the intrusion, it detects and reports it.


### ❓ Question

> Can an intrusion detection system (IDS) prevent the threat after it detects it? Yea/Nay

#### 🧪 Process

We are talking about IDS not IPS, so `Nay`.

Trying this as the answer

#### ✅ Answer

- `Nay` ✅



## 📘Types of IDS

Lets go over the _deployment modes_ of the IDS.

### Deployment Modes

#### Host Intrusion Detection System

The **Host Intrusion Detection System** as it sounds is based on the host. It will only detect intrusions on that host. As a result it can make management difficult on large scales.

#### Network Intrusion Detection System

Where as a **Network Intrusion Detection System** will detect poteltially malicious activity across the entire network regardless of specific hosts. The main advangate is managability since it is centralised.


### Detection Modes

#### Signature-Based IDS

Signature based IDS are similar to AV. They use a signature of a previously known attack. If the malicious acitivity falls within this 'signature' it will be classified. However, the signature must exist to be detected. Zero day's wont by definition have a signature.

#### Anomaly-Based ID

An anomaly based IDS will create a baseline of network activity and will then generate alerts on deviations of the baseline. The downside is that they tend to generate larger quantities of _False-Positives_ so these rules must be tuned. 

#### Hybrid IDS

Where as _hybrid IDS_ will utilise the best of both worlds, it will utilise signatures for the known malicious behaviour and will use anomoly based for the rest. Reducing the number of false positives but allowing the flexibility and reassurance over the long term.

### ❓ Question 1

> Which type of IDS is deployed to detect threats throughout the network?

#### 🧪 Process

If we are detecting threts across the network, then we are not using a host based IDS. Therefore the deployment type would be `Network intrusion detection system`.

Trying this as the answer

#### ✅ Answer

- `Network intrusion detection system` ✅


### ❓ Question 2

> Which IDS leverages both signature-based and anomaly-based detection techniques?

#### 🧪 Process

We can use the `Hybrid IDS` to leverage both signature and anomaly based IDS.

Trying this as the answer

#### ✅ Answer

- `Hybrid IDS` ✅
