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
### Packet Dissection in Wireshark
Packet dissection (or protocol dissection) is the process of analyzing packet contents by decoding the protocols and fields within them. Wireshark supports many protocols and even allows custom dissection scripts.

### Analyzing Packet Details
When you click a packet in the **Packet List Pane**, its details appear in the **Details Pane**, and the corresponding bytes are highlighted in the **Bytes Pane**. Packets typically contain **5 to 7 layers**, aligned with the OSI model:

#### OSI Layer Breakdown in Wireshark:
1. **Frame (Layer 1 – Physical)**: Basic frame info and metadata.
2. **Source MAC (Layer 2 – Data Link)**: Source and destination MAC addresses.
3. **Source IP (Layer 3 – Network)**: Source and destination IP addresses.
4. **Protocol (Layer 4 – Transport)**: TCP/UDP details, including ports.
5. **Protocol Errors**: TCP segment reassembly or issues.
6. **Application Protocol (Layer 5 – Application)**: Protocol-specific info (e.g., HTTP, FTP).
7. **Application Data**: Actual data sent by the application.

Each layer provides a deeper view into how the packet was constructed and transmitted.

### Question 1 - **View packet number 38**. Which markup language is used under the HTTP protocol?
#### Process
1. Open `Exercise.pcapng` from the desktop
2. Find and **select** `packet 38` from the _Packet List_
3. Find HTTP protocol (`Hypertext Transfer Protocol`)
4. The markup Language below is `eXtensible Markup Language`.
5. Trying this as the answer.

#### Answer 1
- `eXtensible Markup Language` ✅


### Question 2 - What is the arrival date of the packet? (Answer format: Month/Day/Year)
#### Process
1. Expand Frame 38
2. See that there is an arrival time is `May 13, 2004 10:17:12.158193000 UTC`
3. Answer needs format MM/DD/YYYY so this would be `05/13/2004`.
4. Trying this as the answer

#### Answer 2
- `05/13/2004` ✅


### Question 3 - What is the TTL value?
#### Process
- **Note**: TTL or Time To Live (hops not time) is a function of the IPv4 protocol.

1. Expand **Intenet Protocol Version 4** within the _Packet Details_
2. Find _Time to live_
3. This shows `47` which matches the two characters expected by the answer
4. Trying this as the answer

#### Answer 3
- `47` ✅


### Question 4 - What is the TCP payload size?
#### Process
- **Note**: The question is asking for TCP payload size, TCP is `Transmission Control Protcocol`

1. Expand Transmission Control Protocol within the _Packet Details_
2. Find TCP payload.
3. The answer is expecting 3 characters, the tcp payload is 424, 3 characters
4. Trying this as the answer

#### Answer 4
- `424` ✅

### Question 5 - What is the e-tag value? _(For example: 82ecb-6321-9e904585)_
#### Process
- **Note**: I've never heard of an e-tag value. So I'll have to be methodical

1. Start by expanding `Frame 38`
2. Nothing there, next expand `Ethernet II`
3. Nothing there, next expand `Internet Protocol Version 4`
4. Nothing there, next expand `Transmission Control Protocol`
5. Nothing there, next expand the TCP segments
6. Nothing there, next expand `Hypertext Transfer Protocol`
7. Found `ETag` not _`e-tag`_ with a value of `"9a01a-4696-7e354b00"`
8. Trying this as the answer

#### Answer 5
- `9a01a-4696-7e354b00` ✅


## Packet Navigation


## Packet Filtering


## Conclusion

