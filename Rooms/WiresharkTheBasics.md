## Contents
- [Introduction](#introduction)
- [Tool Overview](#tool-overview)
- [Packet Dissection](#packet-dissection)
- [Packet Navigation](#packet-navigation)
- [Packet Filtering](#packet-filtering)
- [Conclusion](#conclusion)


## Introduction
Wireshark is an open source and cross platform packet analyser that can be used to capture live network for analysis. Often the goto tool for package analysis. 

From a personal perspective, I have used wireshark a few times but I have never gotten the true value out of it. Hopfully this basics will help.

There are two questions in this intoduction

> **Question 1**: _Which file is used to **simulate** the screenshots_

Process - The text above states _You can use the `"http1.pcapng"` file to simulate the actios shown in the **screenshots**_ so this will be the answer to question 1.

- **Answer 1**: `http1.pcapng` ✅

> **Question 2**: _Which file is use to **answer** the questions?_

Process - The text above states _Please note that you need to use the `"Exercise.pcapng"` file to answe the questions._, so this will be the answer to question 2.

- **Answer 2**: `Exercise.pcapng` ✅


## Tool Overview
### Use cases
- Troubleshooting
- Detecting security anonalies
- Learning protocols

### GUI Overview
Wireshark’s interface includes five main sections:
- **Toolbar** – Tools for filtering, exporting, and managing captures.
- **Display Filter Bar** – For querying and filtering packets.
- **Recent Files** – Quick access to previously opened files.
- **Capture Filter & Interfaces** – Select network interfaces for sniffing.
- **Status Bar** – Displays capture status and packet counts.

### Packet Colouring
- Helps visually identify protocols and anomalies.
- Two types:
  - **Temporary rules** – Session-only.
  - **Permanent rules** – Saved in user profiles.
- Access via **right-click** or **View → Coloring Rules**.

### Merging PCAP Files
- Combine files via **File → Merge**.
- After merging, save the new file before analysis.

### Viewing File Details
- Useful for managing multiple PCAPs.
- Access via **Statistics → Capture File Properties** or the **PCAP icon**.
- Shows metadata like file hash, capture time, and interface info.

### Question 1
> _Read the **"capture file comments"**. What is the flag?_

#### Process
1. Double click `Exercise.pcapng` file on the attackbox desktop
2. Click **Statistics** followed by **Capture File Properties**
3. Within _Capture file comments_ read through the comments to find the flag

- **Answer 1**: `TryHackMe_Wireshark_Demo` ✅
 
### Question 2
> _What is the total number of packets?_

#### Process
1. If the Exercise.pcapng capture file properties are not still open, follow the process outlined in [question 1](#question-1)
2. Find the the count of packets.

- **Answer 2**: `58620` ✅

### Question 3
> _What is the **SHA256 hash** value of the capture file?_

#### Process
1. If the Exercise.pcapng capture file properties are not still open, follow the process outlined in [question 1](#question-1)
2. Find the SHA256 hash value

- **Answer 3**: `f446de335565fb0b0ee5e5a3266703c778b2f3dfad7efeaeccb2da5641a6d6eb` ✅


## Packet Dissection


## Packet Navigation


## Packet Filtering


## Conclusion

