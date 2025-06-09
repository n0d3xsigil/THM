## Contents
- [Introduction](#introduction)
- [Basic Packet Capture](#basic-packet-capture)
- [Filtering Expressions](#filtering-expressions)
- [Advanced Filtering](#advanced-filtering)
- [Displaying Packets](#displaying-packets)
- [Conclusion](#conclusion)


## Introduction
Understanding network protocols can be difficult because their operations are hidden behind user-friendly interfaces. For example, users typically don’t see ARP queries or TCP handshakes unless they inspect network traffic directly. Capturing and analyzing this traffic is a valuable way to understand how networks function.

This room introduces Tcpdump, a command-line tool for capturing network packets. Tcpdump, along with its underlying libpcap library (also used in many other tools and ported to Windows as winpcap), is known for its stability and speed.

### Question 1 - What is the name of the library that is associated with `tcpdump`?
#### Process
1. Right in there with the questions. Well this isn't much of a question since the answer is already in the text, in both the room and above. `libpcap`.
2. Trying this as the answer

#### Answer
- `libpcap` ✅

## Basic Packet Capture
### Tcpdump Usage Summary
While you can run tcpdump without arguments to check if it's installed, real use cases require specifying what to capture, where to save it, and how to display it.

### Key Options and Their Uses:
- `-i INTERFACE`: Choose the network interface to listen on (e.g., eth0, ens5, or any for all interfaces). Use ip a s to list available interfaces.
- `-w FILE`: Save captured packets to a file (commonly .pcap) for later analysis (e.g., in Wireshark). This disables live display.
- `-r FILE`: Read and analyze packets from a previously saved file.
- `-c COUNT`: Limit the number of packets captured (e.g., -c 5 captures five packets).
- `-n / -nn`: Prevent resolution of IP addresses (-n) and port numbers (-nn) to avoid DNS lookups and keep output raw.
- `-v, -vv, -vvv`: Increase verbosity of output to show more packet details.

#### Examples:
- `tcpdump -i eth0 -c 50 -v`: Capture 50 packets on eth0 with verbose output.
- `tcpdump -i wlo1 -w data.pcap`: Capture packets on WiFi interface wlo1 and save to data.pcap.
- `tcpdump -i any -nn`: Capture on all interfaces without resolving IPs or ports.

### Question 1 - What option can you add to your command to display addresses only in numeric format?
#### Process
1. Number only would be IP addresses, or otherwise without resolving DNS. This would be `-nn`
2. Trying this as the answer ❌
3. Okay, well I still feel like it is `n` so lets try the more logical `-n` instead 🫢

#### Answer
- `nn` ❌
- `-n` ✅


## Filtering Expressions
## Advanced Filtering
## Displaying Packets
## Conclusion
