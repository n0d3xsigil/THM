| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **IDS Fundamentals** |

# IDS Fundamentals

## Contents
- [What Is an IDS](#what-is-an-ids)
- [Types of IDS](#types-of-ids)
- [IDS Example: Snort](#ids-example-snort)



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



## 📘IDS Example: Snort

Snort is a popular open source IDS. It is a hybrid type IDS where in it will use signature based detections in addition to anomaly to identify potential threats. These definitions are rules in the snort too. The tool comes with built in rules but you can customise to suite the environment.

There are 3 mods that the tool uses

### Packet sniffer mode

The _PS_ mode allows for packets reviewed, however they do not get analysed. It does however allow for troubleshooting and udnerstanding the flow of trafic.

For example, a netops team identfied a performance issue, they use the traffic data that was captured using sort to identify causes.

### Packet logging mode

The _PL_ mode performs detection on realtime traffic and provides alerts on detections at the console. If for some reason the traffic is of some interest we can use the _PL_ mode to log the traffic (PCAP) for later investigations.

For example, the secops team need to perform a forensic investigation. They can use the file captured to perform the forensic analysis

### Network Intrusion Detection System mode

The primary mode for the Snort NIDS and monitors in real time ot idenfity known patterns and log new attacks as signatures. Matches are generated as alerts.

For exmaple, The secops team proactivly monitors the environment to detect potential threats. Snort's NIDS will achieve this.


### ❓ Question 1

> Which mode of Snort helps us to log the network traffic in a PCAP file?

#### 🧪 Process

The `Packet Logging mode` will achive this by taking the packets captured with the _packet sniffer mode_ and saving them as a **PCAP** file.

Trying this as the answer

#### ✅ Answer

- `Packet Logging mode` ✅


### ❓ Question 2

> What is the primary mode of Snort called?

#### 🧪 Process

The primary mode is called the `Network Intrusion Detection System mode`. It is the primary function of Snort as in NIDS.

Trying this as the answer

#### ✅ Answer

- `Network Intrusion Detection System mode` ✅
