| [Home](../README.md) | [SOC Level 1](../SOClevel1.md) | **Threat Intelligence Tools** |

# Threat Intelligence Tools

## Contents
- [Room Outline](#room-outline)
- [Threat Intelligence](#threat-intelligence)
- [UrlScan.io](#urlscanio)
- [Abuse.ch](#abusech)
- [PhishTool](#phishtool)


## 📘Room Outline

This room covers
- Threat intelligence and classifications
- UrlScan.io (Malicious URLs)
- Abuse.ch (Malware and botnets)
- PhishTool
- Talos Intelligence



## 📘Threat Intelligence

### Threat Intelligence Classifications

Threat Intel is geared towards understanding the relationship between your operational environment and your adversary. With this in mind, we can break down threat intel into the following classifications: 

- **Strategic Intel**
  - High-level intel that looks into the organisation's threat landscape and maps out the risk areas based on trends, patterns and emerging threats that may impact business decisions.
- **Technical Intel**
  - Looks into evidence and artefacts of attack used by an adversary. Incident Response teams can use this intel to create a baseline attack surface to analyse and develop defence mechanisms.
- **Tactical Intel**
  - Assesses adversaries' tactics, techniques, and procedures (TTPs). This intel can strengthen security controls and address vulnerabilities through real-time investigations.
- **Operational Intel**
  - Looks into an adversary's specific motives and intent to perform an attack. Security teams may use this intel to understand the critical assets available in the organisation (people, processes, and technologies) that may be targeted.



## 📘UrlScan.io

Our first tool is [urlscan.io](https://urlscan.io/). The tool automates the process of analysing a website. The site has two modes, a site view and a live view. Results may differ day to day.


### ❓ Question 1

> What was TryHackMe's Cisco Umbrella Rank based on the screenshot?

#### 🧪 Process

_process_

#### ✅ Answer

- `345612` ✅



### ❓ Question 2

> How many domains did UrlScan.io identify on the screenshot?

#### 🧪 Process

_process_

#### ✅ Answer

- `13` ✅



### ❓ Question 3

> What was the main domain registrar listed on the screenshot?

#### 🧪 Process

_process_

#### ✅ Answer

- `NAMECHEAP INC` ✅



### ❓ Question 4

> What was the main IP address identified for TryHackMe on the screenshot?

#### 🧪 Process

_process_

#### ✅ Answer

- `2606:4700:10::ac43:1b0a` ✅



## 📘Abuse.ch

The next tool is [Abuse.ch](https://abuse.ch/). A research project of Bern University of Applied Sciences.

Made up of multiple as documented below

- **MalwareBazaar**
  - Find it at [https://bazaar.abuse.ch/](https://bazaar.abuse.ch/).
  - Upload samples for analysis and help build out the intelligence database (API also available)
  - Hunt for samples through alerts using tags, signatures, YARA rules etc

- **FeodoTracker**
  - Find it at [https://feodotracker.abuse.ch/](https://feodotracker.abuse.ch/)
  - Shares information on botnet C2 servers (Dridex, Emotes (aka Heodo), TrickBot, QakBot and BazarLoader/BazarBackdoor)
  - Shares IP's for look ups against IOC.
 
- **SSL Back List**
  - Find it at [https://sslbl.abuse.ch/](https://sslbl.abuse.ch/).
  - Identifies malicious SSL connections.
  - Uses JA3 fingerprints
    - See [https://github.com/salesforce/ja3](https://github.com/salesforce/ja3)

- **URLhaus**
  - Find it at [https://urlhaus.abuse.ch/](https://urlhaus.abuse.ch/).
  - Focuses on sharing URLs used for malware distribution.
  - Can be used to search domains, urls, hashes and filetypes

- **ThreatFox**
  - Find it at [https://threatfox.abuse.ch/](https://threatfox.abuse.ch/).
  - Share and export IOC associated with malware.
  - Exportable in various formats


### ❓ Question 1

> The IOC **212.192.246.30:5555** is identified under which malware alias name on ThreatFox?

#### 🧪 Process

In the search bar search `ioc:212.192.246.30:5555`

Click the IOC link (212.192.246.30:5555)

We can see the alias is `Katana`

Trying this as the answer

#### ✅ Answer

- `Katana` ✅


### ❓ Question 2

> Which malware is associated with the JA3 Fingerprint **51c64c77e60f3980eea90869b68c58a8** on SSL Blacklist?

#### 🧪 Process

Navigate to [https://sslbl.abuse.ch/](https://sslbl.abuse.ch/)
Click **View details >>** under _JA3 Fingerprints_
Paste `51c64c77e60f3980eea90869b68c58a8` into the **Search:** box
We have one result, listing reason `Dridex`.
Click the fingerprint to see more
Sure enough the Malware is `Dridex`.

Trying this as the answer

#### ✅ Answer

- `Dridex` ✅


### ❓ Question 3

> From the statistics page on URLHaus, what malware-hosting network has the ASN number **AS14061**? 

#### 🧪 Process

Navigate to [https://urlhaus.abuse.ch/asn/14061/](https://urlhaus.abuse.ch/asn/14061/).
AS name is `DIGITALOCEAN-ASN`

Trying this as the answer

#### ✅ Answer

- `DIGITALOCEAN-ASN` ✅


### ❓ Question 4

> Which country is the botnet IP address **178.134.47.166** associated with according to FeodoTracker?

#### 🧪 Process

Navigate to [https://feodotracker.abuse.ch/](https://feodotracker.abuse.ch/).
Click **Browse** along the top bar
Paste the IP _`178.134.47.166`_ into the search box and press return to serch
We can see our country is GE. GE is `GEorgia`

Trying this as the answer

#### ✅ Answer

- `Georgia` ✅




## 📘PhishTool

In this task we are looking at a tool called **PhishTool**, you can find it here ([https://www.phishtool.com/](https://www.phishtool.com/)).

Phishing is the most common form of cyber attach. It is used to trick people in to providing sensitive information in the aims to be able to use the information in gaining access to a system of some kind.

See the Phishing rooms under [Rooms of interest](../RoomsOfInterest.md#p)


