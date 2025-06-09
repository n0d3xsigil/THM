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
### Tcpdump Filtering Summary
Capturing all network traffic at once is overwhelming and inefficient. Tcpdump allows filtering to focus on specific traffic types, making analysis more manageable.


### 1. Filter by Host
- Use host IP or host HOSTNAME to capture traffic to/from a specific host.
- Use src host or dst host to filter by source or destination specifically.
#### Example:
```shell
sudo tcpdump host example.com -w http.pcap
```


### 2. Filter by Port
- Use port PORT_NUMBER to capture traffic on a specific port (e.g., DNS on port 53).
- Use src port or dst port for more precise filtering.
#### Example:
```shell
sudo tcpdump -i ens5 port 53 -n
```


### 3. Filter by Protocol
- Capture only specific protocols like ip, ip6, tcp, udp, or icmp.
#### Example:
```shell
sudo tcpdump -i ens5 icmp -n
```


### 4. Use Logical Operators
- Combine filters using:
  - and (both conditions must be true)
  - or (either condition can be true)
  - not (exclude a condition)
#### Example:
```shell
tcpdump host 1.1.1.1 and tcp
```


### 5. Reading from Capture Files
- Use -r FILE to read from a .pcap file.
- Combine with -c to limit output and -n to skip DNS lookups.
#### Example:
```shell
tcpdump -r traffic.pcap -c 5 -n
```


### 6. Counting Packets
- Pipe output to wc -l to count matching packets.
#### Example:
```shell
tcpdump -r traffic.pcap src host 192.168.124.1 -n | wc -l
```


### Question 1 - How many packets in `traffic.pcap` use the ICMP protocol?
#### Process
1. First, to preserve data I copied the traffic.pcap file to traffic.pcap.jic. Just in case I mess something up.
  - `user@ip-10-10-99-206:~$ cp traffic.pcap traffic.pcap.jic`
2. Next run `tcpdump -r traffic.pcap ICMP`
3. Of course this shows raw data. Lets pipe this into wordcout (`wc`)
4. `tcpdump -r traffic.pcap icmp | wc`
5. This gives us the result `     26     358    4466`.
6. Since each line is a packet we can assume there is 26 packets.
7. Trying this as the answer

#### Answer 1
- `26` ✅


### Question 2 - What is the IP address of the host that asked for the MAC address of 192.168.124.137?
#### Process
What do we know.
- We know the destination (192.168.124.137)
  - We can translate that to `dst host 192.168.124.137`
- We want a translation (DNS)
  - We can translate that to `dst port 53`
- We want an IP address (`-n`)

Combining this we get the following:

 ```shell
user@ip-10-10-99-206:~$ tcpdump -r traffic.pcap dst host 192.168.124.137 and dst port 53 -n
reading from file traffic.pcap, link-type EN10MB (Ethernet)
07:18:30.092730 IP 192.168.124.148.61223 > 192.168.124.137.53: Flags [S], seq 1549842177, win 1024, options [mss 1460], len
gth 0
user@ip-10-10-99-206:~$ 
```
I can see the source IP was `192.168.124.148`

- Trying this as the answer



#### Answer 2
- `192.168.124.148` ✅

❓but! I misread the question. It is asking _"What is the IP address of the host that asked for the **MAC address** of 192.168.124.137?"_. I only looked for the IP address. I might need to revisit this one at some point.


### Question 3 - What hostname (subdomain) appears in the first DNS query?
#### Process
This one is pretty straight forward. We know we want a DNS query so will use `dst port 53` and we're looking for the first response

Enter `tcpdump -r traffic.pcap dst port 53` to find all DNS queries within `trafic.pcap`
```shell
user@ip-10-10-99-206:~$ tcpdump -r traffic.pcap dst port 53
reading from file traffic.pcap, link-type EN10MB (Ethernet)
07:18:24.058626 IP ip-192-168-124-137.eu-west-1.compute.internal.33672 > ip-192-168-124-1.eu-west-1.compute.internal.domain
: 39913+ A? mirrors.rockylinux.org. (40)
07:18:24.058652 IP ip-192-168-124-137.eu-west-1.compute.internal.33672 > ip-192-168-124-1.eu-west-1.compute.internal.domain
: 27109+ AAAA? mirrors.rockylinux.org. (40)
07:18:24.430844 IP ip-192-168-124-137.eu-west-1.compute.internal.49804 > ip-192-168-124-1.eu-west-1.compute.internal.domain
: 61459+ A? mirrors.storpool.com. (38)
07:18:24.430854 IP ip-192-168-124-137.eu-west-1.compute.internal.49804 > ip-192-168-124-1.eu-west-1.compute.internal.domain
: 49429+ AAAA? mirrors.storpool.com. (38)
07:18:24.430999 IP ip-192-168-124-137.eu-west-1.compute.internal.54750 > ip-192-168-124-1.eu-west-1.compute.internal.domain
: 8531+ A? mirrors.storpool.com. (38)
07:18:24.431005 IP ip-192-168-124-137.eu-west-1.compute.internal.54750 > ip-192-168-124-1.eu-west-1.compute.internal.domain
: 16466+ AAAA? mirrors.storpool.com. (38)
07:18:24.431097 IP ip-192-168-124-137.eu-west-1.compute.internal.42123 > ip-192-168-124-1.eu-west-1.compute.internal.domain
: 6874+ A? mirrors.storpool.com. (38)
07:18:24.431103 IP ip-192-168-124-137.eu-west-1.compute.internal.42123 > ip-192-168-124-1.eu-west-1.compute.internal.domain
: 44506+ AAAA? mirrors.storpool.com. (38)
07:18:30.092730 IP ip-192-168-124-148.eu-west-1.compute.internal.61223 > ip-192-168-124-137.eu-west-1.compute.internal.doma
in: Flags [S], seq 1549842177, win 1024, options [mss 1460], length 0
user@ip-10-10-99-206:~$
```

The first entry is `mirrors.rockylinux.org`.

- Trying this as the answer
#### Answer 4
- `mirrors.rockylinux.org` ✅


## Advanced Filtering
## Displaying Packets
## Conclusion
