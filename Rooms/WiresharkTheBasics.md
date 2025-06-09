[Home](#../README.md)

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
### Packet Numbers and Navigation
- **Unique Packet Numbers**: Wireshark assigns a unique number to each packet, making it easier to track and revisit specific events in large captures.
- **Go to Packet**: Navigate directly to a packet using the "Go" menu or toolbar. This includes tracking packets within conversations.


### Finding Packets
- Use **Edit → Find Packet** to search for specific content.
- Supports four input types:
  - **Display filter**
  - **Hex**
  - **String**
  - **Regex**
- Searches can be done in:
  - **Packet List**
  - **Packet Details**
  - **Packet Bytes**
- Be sure to match the search type with the correct pane.


### Marking and Commenting Packets
- **Mark Packets**: Highlight packets for later review (shown in black). Marks are session-based and not saved.
- **Packet Comments**: Add persistent notes to packets. Comments are saved with the capture file and useful for collaboration or future analysis.


### Exporting Packets and Objects
- **Export Packets**: Save selected packets to a new file for focused analysis or sharing.
- **Export Objects**: Extract files transferred over protocols like HTTP, SMB, TFTP, etc., for deeper investigation.


### Time Display Format
- Default time format is **"Seconds Since Beginning of Capture"**.
- You can switch to **UTC** or other formats via **View → Time Display Format** for better context.


### Expert Info
- Wireshark flags potential issues using **Expert Info**, categorized by severity:
  - **Chat (Blue)** – Normal workflow info
  - **Note (Cyan)** – Notable events
  - **Warn (Yellow)** – Warnings
  - **Error (Red)** – Serious issues like malformed packets
  - View via **Analyze → Expert Information** or the **status bar**.


### Question 1 - Search the "r4w" string in packet details. What is the name of artist 1?
#### Process
1. Press Ctrl+F on the keybaord to show find field
2. Leave type as string and enter `r4w`
3. Click **Find**
4. Right click the packet within Packet List
5. In the context menu click **Follow**
6. Click HTTP Stream (Or press and hold Ctrl+Alt+Shift and press H)
7. Within the follow HTTP stream search `artist`
8. Keep clicking **Find Next** until something interesting arrives
9. Perhaps worth trying `r4w8173`?
10. Trying this as the answer

#### Answer 1
- `r4w8173` ✅


### Question 2 - Go to packet 12 and read the packet comments. What is the answer? _Note: use md5sum <filename> terminal command to get MD5 hash_
#### Process
1. Go to packet 12 as advised.
2. Press **Ctrl+Alt+C** on the keyboard.
3. `This_is_Not_a _Flag`. Very helpful. Notice the scroll bar though...
4. Go to packet number 39765
5. Look at the "packet details pane".
6. Right-click on the JPEG section and "Export packet bytes".
7. Save the file to the desktop
8. Open terminal
9. Navigate to ~/Desktop/
10. Type command `md5sum filename.jpg`
11. My result was `911cd574a42865a956ccde2d04495ebf  funtimes.jpg`
12. Trying this as the answer

#### Answer 2
- `911cd574a42865a956ccde2d04495ebf` ✅

### Question 3 - There is a ".txt" file inside the capture file. Find the file and read it; what is the alien's name?
#### Process
1. Clear any filters
2. Press Ctrl+F to show the find toolbar
3. Search for .txt and click **Find**
4. In Info you will see `GET /note.text HTTP/1.1`
5. Below this you shoudl see `HTTP/1.0 200 OK` to signify the request was OK
6. Select this packet
7. Right click `Line-based text data:`
8. Click `Export Packet Bytes...` within the context menu
9. Save the file as `alient.txt` on the Desktop
10. Navigate to the file downloaded and double click it.
11. What is PACKETMASTER?
12. Trying this as the answer

#### Answer 3
- `PACKETMASTER` ✅


### Question 4 - Look at the expert info section. What is the number of warnings?
#### Process
1. Ensure filters are cleared to show all packets
2. Click **Analyze** → **Expert Information**
3. Since we're looking for Warning, expand Warning
4. Count each individual packets to get the count.
5. Okay, that can't be right.
6. Expand the window to show all fields
7. Ah, Warnings `1636`
8. Trying this as the answer

#### Answer 4
- `1636` ✅

## Packet Filtering
### Packet Filtering in Wireshark
Wireshark offers two types of filters:
- **Capture Filters**: Apply before capturing traffic to limit what is recorded.
- **Display Filters**: Apply after capturing to narrow down what is shown.

This section focuses on Display Filters, which help analysts isolate relevant packets from large captures.


### Filtering Methods
1. **Apply as Filter**
  - Right-click on a field and choose **“Apply as Filter”**.
  - Wireshark auto-generates and applies the filter.
  - Only matching packets are shown; others are hidden.
2. **Conversation Filter**
  - Filters all packets involved in a specific conversation (e.g., between two IPs or ports).
  - Useful for tracking full sessions.
  - Access via **right-click** or **Analyze → Conversation Filter**.
3. **Colourise Conversation**
  - Highlights related packets without filtering them out.
  - Helps visually track conversations.
  - Use **View → Colourise Conversation** to apply or reset.
4. **Prepare as Filter**
  - Similar to “Apply as Filter” but doesn’t apply it immediately.
  - Adds the filter to the bar for manual editing or combining with other filters.
5. **Apply as Column**
  - Adds a selected field as a new column in the packet list.
  - Helps compare values across packets.
  - Use **right-click → Apply as Column**.


### Follow Stream
- Reconstructs and displays full application-level conversations (e.g., HTTP, TCP).
- Shows client (red) and server (blue) data.
- Access via **right-click → Follow Stream**.
- Automatically applies a filter for the stream; use the **X button** to clear it.


### Question 1 - See below
> Use the "Exercise.pcapng" file to answer the questions.
> Go to packet number 4. Right-click on the "Hypertext Transfer Protocol" and apply it as a filter.
> Now, look at the filter pane. What is the filter query?
#### Process
1. In the _Packet List_ click **Packet 4**
2. Within the _Packet Details_ of _Packet 4_ right click `Hypertext Transfer Protocol`
3. In the context menu expand **Apply as Filter** followed by Selected
4. The filter will be come `http`
5. Trying this as the answer

#### Answer 1
- `http` ✅


### Question 2 - What is the number of displayed packets?
#### Process
1. Notice at the bottom right `Displayed`
2. The filtered packets shows `1089`
3. Trying this as the answer

#### Answer 2
- `1089` ✅


### Question 3 - See below
> Go to packet number 33790, follow the HTTP stream, and look carefully at the responses.
> Looking at the web server's response, what is the total number of artists?
#### Process
1. Clear existing filters
2. Scroll down to packet **33790**
3. First thing I notice is that tere is `Line-based text data`
4. Right click and select **Export Packet Bytes...**
5. Save to the Desktop as `page.html`
6. Goto Desktop and open `page.html`
  - **Disclaimer** not the best option all things considered. But since the box is sandboxed ;)
7. The formatted HTML page contains 3 artists (`r4w8173`, `Blad3`, and `lyzae`).
8. Trying this as the answer

#### Answer 3
- `3` ✅


### Question 4 - What is the name of the second artist?
#### Process
1. Well we already know the second artist listed above is `Blad3` so....
2. Trying this as the answer

#### Answer 4
- `Blad3` ✅


## Conclusion
Okay so that was really helpful. My Concern is that actually, it's probably easy to find 'something-you-know', but finding the unknown could be a little more difficult.

Additionally, this focuses on clear text. What about secure data? I guess it's great for identifying the misconfigued stuff still transferrring data in clear text atleast.
