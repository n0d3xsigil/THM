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
### Advanced Tcpdump Filtering Summary
In real-world scenarios with massive traffic, precise filtering is essential. Tcpdump supports advanced filters, including packet size, header content, and TCP flags.


### 1. Filter by Packet Size
- `greater %LENGTH%`: Captures packets larger than or equal to a specified size.
- `less %LENGTH%`: Captures packets smaller than or equal to a specified size.


### 2. Binary Operations Refresher
Tcpdump uses **bitwise operations** for advanced filtering:
- `&` (AND): True if both bits are 1.
- `|` (OR): True if at least one bit is 1.
- `!` (NOT): Inverts the bit (1 becomes 0, and vice versa).


### 3. Filtering by Header Bytes
Use the format:
**proto[expr:size]**
- `proto`: Protocol (e.g., ip, tcp, udp, etc.)
- `expr`: Byte offset (e.g., 0 is the first byte)
- `size`: Optional (1 by default); can be 1, 2, or 4 bytes

#### Examples:
- `ether[0] & 1 != 0`: Filters multicast Ethernet packets.
- `ip[0] & 0xf != 5`: Filters IP packets with options.


### 4. Filtering by TCP Flags
Use `tcp[tcpflags]` to filter based on TCP control flags:
#### Common Flags:
- `tcp-syn`: Synchronize
- `tcp-ack`: Acknowledge
- `tcp-fin`: Finish
- `tcp-rst`: Reset
- `tcp-push`: Push
#### Examples:
- `tcpdump "tcp[tcpflags] == tcp-syn"`: Only SYN flag is set.
- `tcpdump "tcp[tcpflags] & tcp-syn != 0"`: SYN flag is set (others may be too).
- `tcpdump "tcp[tcpflags] & (tcp-syn|tcp-ack) != 0"`: SYN or ACK flag is set.


### Question 1 - How many packets have only the TCP Reset (RST) flag set?
#### Process
First I'll try with the example above `tcpdump "tcp[tcpflags] == tcp-syn"`. Although this is for SYN flag, we can instead use `tcp-rst`

```shell
user@ip-10-10-99-206:~$ tcpdump -r traffic.pcap "tcp[tcpflags] == tcp-rst"
reading from file traffic.pcap, link-type EN10MB (Ethernet)
07:18:24.477352 IP ip-192-168-124-137.eu-west-1.compute.internal.53660 > 199.232.194.132.https: Flags [R], seq 709170445, w
in 0, length 0
07:18:24.477372 IP ip-192-168-124-137.eu-west-1.compute.internal.53660 > 199.232.194.132.https: Flags [R], seq 709170445, w
in 0, length 0
07:18:24.477381 IP ip-192-168-124-137.eu-west-1.compute.internal.53660 > 199.232.194.132.https: Flags [R], seq 709170445, w
in 0, length 0
07:18:24.478912 IP ip-192-168-124-137.eu-west-1.compute.internal.53660 > 199.232.194.132.https: Flags [R], seq 709170446, w
in 0, length 0
07:18:30.092983 IP ip-192-168-124-148.eu-west-1.compute.internal.61223 > ip-192-168-124-137.eu-west-1.compute.internal.ssh:
 Flags [R], seq 1549842178, win 0, length 0
07:18:30.242611 IP ip-192-168-124-148.eu-west-1.compute.internal.61223 > ip-192-168-124-137.eu-west-1.compute.internal.disc
ard: Flags [R], seq 1549842178, win 0, length 0
07:18:30.246377 IP ip-192-168-124-148.eu-west-1.compute.internal.61223 > ip-192-168-124-137.eu-west-1.compute.internal.dayt
ime: Flags [R], seq 1549842178, win 0, length 0
07:18:30.519505 IP ip-192-168-124-148.eu-west-1.compute.internal.61223 > ip-192-168-124-137.eu-west-1.compute.internal.qotd
: Flags [R], seq 1549842178, win 0, length 0
07:18:30.617113 IP ip-192-168-124-148.eu-west-1.compute.internal.61223 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 1549842178, win 0, length 0
07:19:49.448665 IP ip-192-168-124-148.eu-west-1.compute.internal.41591 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 2160770156, win 0, length 0
07:19:49.548827 IP ip-192-168-124-148.eu-west-1.compute.internal.41592 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 2160770157, win 0, length 0
07:19:49.649558 IP ip-192-168-124-148.eu-west-1.compute.internal.41593 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 2160770158, win 0, length 0
07:19:49.749928 IP ip-192-168-124-148.eu-west-1.compute.internal.41594 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 2160770159, win 0, length 0
07:19:49.849648 IP ip-192-168-124-148.eu-west-1.compute.internal.41595 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 2160770160, win 0, length 0
07:19:49.949911 IP ip-192-168-124-148.eu-west-1.compute.internal.41596 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 2160770161, win 0, length 0
07:19:50.050454 IP ip-192-168-124-148.eu-west-1.compute.internal.41603 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 2160770156, win 0, length 0
07:19:50.125536 IP ip-192-168-124-137.eu-west-1.compute.internal.echo > ip-192-168-124-148.eu-west-1.compute.internal.41607
: Flags [R], seq 1892647832, win 0, length 0
07:19:50.175916 IP ip-192-168-124-137.eu-west-1.compute.internal.tcpmux > ip-192-168-124-148.eu-west-1.compute.internal.416
09: Flags [R], seq 1892647832, win 0, length 0
07:19:51.559244 IP ip-192-168-124-148.eu-west-1.compute.internal.41591 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 4163816068, win 0, length 0
07:19:51.659518 IP ip-192-168-124-148.eu-west-1.compute.internal.41592 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 4163816069, win 0, length 0
07:19:51.759640 IP ip-192-168-124-148.eu-west-1.compute.internal.41593 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 4163816070, win 0, length 0
07:19:51.859847 IP ip-192-168-124-148.eu-west-1.compute.internal.41594 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 4163816071, win 0, length 0
07:19:51.960101 IP ip-192-168-124-148.eu-west-1.compute.internal.41595 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 4163816072, win 0, length 0
07:19:52.060259 IP ip-192-168-124-148.eu-west-1.compute.internal.41596 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 4163816073, win 0, length 0
07:19:52.161103 IP ip-192-168-124-148.eu-west-1.compute.internal.41603 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 4163816068, win 0, length 0
07:19:52.237334 IP ip-192-168-124-137.eu-west-1.compute.internal.echo > ip-192-168-124-148.eu-west-1.compute.internal.41607
: Flags [R], seq 1479907192, win 0, length 0
07:19:52.287877 IP ip-192-168-124-137.eu-west-1.compute.internal.tcpmux > ip-192-168-124-148.eu-west-1.compute.internal.416
09: Flags [R], seq 1479907192, win 0, length 0
07:19:53.672129 IP ip-192-168-124-148.eu-west-1.compute.internal.41591 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 2504347609, win 0, length 0
07:19:53.771771 IP ip-192-168-124-148.eu-west-1.compute.internal.41592 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 2504347610, win 0, length 0
07:19:53.872483 IP ip-192-168-124-148.eu-west-1.compute.internal.41593 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 2504347611, win 0, length 0
07:19:53.972136 IP ip-192-168-124-148.eu-west-1.compute.internal.41594 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 2504347612, win 0, length 0
07:19:54.072723 IP ip-192-168-124-148.eu-west-1.compute.internal.41595 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 2504347613, win 0, length 0
07:19:54.172954 IP ip-192-168-124-148.eu-west-1.compute.internal.41596 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 2504347614, win 0, length 0
07:19:54.273233 IP ip-192-168-124-148.eu-west-1.compute.internal.41603 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 2504347609, win 0, length 0
07:19:54.348320 IP ip-192-168-124-137.eu-west-1.compute.internal.echo > ip-192-168-124-148.eu-west-1.compute.internal.41607
: Flags [R], seq 1238976603, win 0, length 0
07:19:54.398492 IP ip-192-168-124-137.eu-west-1.compute.internal.tcpmux > ip-192-168-124-148.eu-west-1.compute.internal.416
09: Flags [R], seq 1238976603, win 0, length 0
07:19:57.282394 IP ip-192-168-124-148.eu-west-1.compute.internal.41591 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 429594893, win 0, length 0
07:19:57.382573 IP ip-192-168-124-148.eu-west-1.compute.internal.41592 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 429594894, win 0, length 0
07:19:57.482649 IP ip-192-168-124-148.eu-west-1.compute.internal.41593 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 429594895, win 0, length 0
07:19:57.582556 IP ip-192-168-124-148.eu-west-1.compute.internal.41594 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 429594896, win 0, length 0
07:19:57.682818 IP ip-192-168-124-148.eu-west-1.compute.internal.41595 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 429594897, win 0, length 0
07:19:57.782673 IP ip-192-168-124-148.eu-west-1.compute.internal.41596 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 429594898, win 0, length 0
07:19:57.883374 IP ip-192-168-124-148.eu-west-1.compute.internal.41603 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 429594893, win 0, length 0
07:19:57.959015 IP ip-192-168-124-137.eu-west-1.compute.internal.echo > ip-192-168-124-148.eu-west-1.compute.internal.41607
: Flags [R], seq 2409701932, win 0, length 0
07:19:58.009138 IP ip-192-168-124-137.eu-west-1.compute.internal.tcpmux > ip-192-168-124-148.eu-west-1.compute.internal.416
09: Flags [R], seq 2409701932, win 0, length 0
07:19:59.393701 IP ip-192-168-124-148.eu-west-1.compute.internal.41591 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 233106153, win 0, length 0
07:19:59.493697 IP ip-192-168-124-148.eu-west-1.compute.internal.41592 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 233106154, win 0, length 0
07:19:59.593963 IP ip-192-168-124-148.eu-west-1.compute.internal.41593 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 233106155, win 0, length 0
07:19:59.694041 IP ip-192-168-124-148.eu-west-1.compute.internal.41594 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 233106156, win 0, length 0
07:19:59.793918 IP ip-192-168-124-148.eu-west-1.compute.internal.41595 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 233106157, win 0, length 0
07:19:59.894317 IP ip-192-168-124-148.eu-west-1.compute.internal.41596 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 233106158, win 0, length 0
07:19:59.995592 IP ip-192-168-124-148.eu-west-1.compute.internal.41603 > ip-192-168-124-137.eu-west-1.compute.internal.echo
: Flags [R], seq 233106153, win 0, length 0
07:20:00.070848 IP ip-192-168-124-137.eu-west-1.compute.internal.echo > ip-192-168-124-148.eu-west-1.compute.internal.41607
: Flags [R], seq 79966600, win 0, length 0
07:20:00.121718 IP ip-192-168-124-137.eu-west-1.compute.internal.tcpmux > ip-192-168-124-148.eu-west-1.compute.internal.416
09: Flags [R], seq 79966600, win 0, length 0
07:20:00.615679 IP ip-192-168-124-137.eu-west-1.compute.internal.daytime > ip-192-168-124-148.eu-west-1.compute.internal.55
202: Flags [R], seq 2127835777, win 0, length 0
07:20:00.615699 IP ip-192-168-124-137.eu-west-1.compute.internal.qotd > ip-192-168-124-148.eu-west-1.compute.internal.45844
: Flags [R], seq 2369614486, win 0, length 0
07:20:05.626518 IP ip-192-168-124-137.eu-west-1.compute.internal.daytime > ip-192-168-124-148.eu-west-1.compute.internal.59
1: Flags [R], seq 3000745832, win 0, length 0
```
Okay, so perhaps I should run this through `wc` to get the number rather than the actual results. We're aiming for a number ≤ 99 to fit with the expected 2 character answer.

```Shell
user@ip-10-10-99-206:~$ tcpdump -r traffic.pcap "tcp[tcpflags] == tcp-rst" | wc
reading from file traffic.pcap, link-type EN10MB (Ethernet)
     57     741    9457
```

That looks better to me, so I am going to try `57` as the answer.

#### Answer 1
- `57` ✅

### Question 2 - What is the IP address of the host that sent packets larger than 15000 bytes?
#### Process
So in the room we can see the argument `greater LENGTH` and the description is _Filters packets that have a length greater than or equal to the specified length_

We can try `tcpdump -r traffic.pcap greater 15000` and see what comes back.

```shell
user@ip-10-10-99-206:~$ tcpdump -r traffic.pcap greater 15000
reading from file traffic.pcap, link-type EN10MB (Ethernet)
07:18:24.967023 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60518: Flags [.], seq 21408760
81:2140896901, ack 741991605, win 235, options [nop,nop,TS val 2226566282 ecr 3054280184], length 20820: HTTP
07:18:25.778012 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60518: Flags [.], seq 1293616:
1308884, ack 1, win 235, options [nop,nop,TS val 2226567095 ecr 3054280994], length 15268: HTTP
07:18:25.861724 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60518: Flags [.], seq 1378284:
1397716, ack 1, win 235, options [nop,nop,TS val 2226567176 ecr 3054281078], length 19432: HTTP
07:18:26.457422 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60518: Flags [.], seq 2356824:
2373480, ack 1, win 235, options [nop,nop,TS val 2226567777 ecr 3054281682], length 16656: HTTP
07:18:26.746414 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60492: Flags [.], seq 96437621
8:964395650, ack 1034282473, win 235, options [nop,nop,TS val 2226568063 ecr 3054281963], length 19432: HTTP
07:18:26.978560 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60502: Flags [.], seq 37860237
52:3786039020, ack 3169565691, win 235, options [nop,nop,TS val 2226568298 ecr 3054282201], length 15268: HTTP
07:18:27.195761 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60492: Flags [.], seq 570468:5
89900, ack 1, win 235, options [nop,nop,TS val 2226568513 ecr 3054282418], length 19432: HTTP
07:18:27.391916 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60492: Flags [.], seq 831412:8
48068, ack 1, win 235, options [nop,nop,TS val 2226568707 ecr 3054282611], length 16656: HTTP
07:18:28.010550 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60518: Flags [.], seq 4778884:
4798316, ack 1, win 235, options [nop,nop,TS val 2226569330 ecr 3054283233], length 19432: HTTP
07:18:28.310408 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60518: Flags [.], seq 5200836:
5217492, ack 1, win 235, options [nop,nop,TS val 2226569621 ecr 3054283524], length 16656: HTTP
07:18:28.710415 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60492: Flags [.], seq 2702436:
2717704, ack 1, win 235, options [nop,nop,TS val 2226570023 ecr 3054283928], length 15268: HTTP
07:18:28.896088 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60518: Flags [.], seq 5844868:
5862912, ack 1, win 235, options [nop,nop,TS val 2226570211 ecr 3054284115], length 18044: HTTP
07:18:30.408334 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60502: Flags [.], seq 5042604:
5057872, ack 1, win 235, options [nop,nop,TS val 2226571725 ecr 3054285629], length 15268: HTTP
07:18:30.656265 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60502: Flags [.], seq 5450676:
5474272, ack 1, win 235, options [nop,nop,TS val 2226571966 ecr 3054285871], length 23596: HTTP
07:18:30.964178 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60518: Flags [.], seq 8409892:
8434876, ack 1, win 235, options [nop,nop,TS val 2226572277 ecr 3054286180], length 24984: HTTP
07:18:31.374026 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60518: Flags [.], seq 8992852:
9010896, ack 1, win 235, options [nop,nop,TS val 2226572684 ecr 3054286587], length 18044: HTTP
07:18:31.424145 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60502: Flags [.], seq 6741516:
6762336, ack 1, win 235, options [nop,nop,TS val 2226572743 ecr 3054286647], length 20820: HTTP
07:18:31.496604 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60502: Flags [.], seq 6855332:
6873376, ack 1, win 235, options [nop,nop,TS val 2226572813 ecr 3054286718], length 18044: HTTP
07:18:31.692912 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60518: Flags [.], seq 9443952:
9460608, ack 1, win 235, options [nop,nop,TS val 2226573010 ecr 3054286913], length 16656: HTTP
07:18:31.966553 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60502: Flags [.], seq 7702012:
7718668, ack 1, win 235, options [nop,nop,TS val 2226573285 ecr 3054287187], length 16656: HTTP
07:18:32.088431 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60492: Flags [.], seq 7235644:
7255076, ack 1, win 235, options [nop,nop,TS val 2226573406 ecr 3054287311], length 19432: HTTP
07:18:32.331304 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60492: Flags [.], seq 7578480:
7597912, ack 1, win 235, options [nop,nop,TS val 2226573637 ecr 3054287540], length 19432: HTTP
07:18:32.501675 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60492: Flags [.], seq 7818604:
7833872, ack 1, win 235, options [nop,nop,TS val 2226573814 ecr 3054287710], length 15268: HTTP
07:18:32.716659 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60518: Flags [.], seq 10898576
:10915232, ack 1, win 235, options [nop,nop,TS val 2226574036 ecr 3054287940], length 16656: HTTP
07:18:32.825803 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60502: Flags [.], seq 9044208:
9065028, ack 1, win 235, options [nop,nop,TS val 2226574144 ecr 3054288048], length 20820: HTTP
07:18:34.605726 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60492: Flags [.], seq 10509936
:10541860, ack 1, win 235, options [nop,nop,TS val 2226575924 ecr 3054289827], length 31924: HTTP
07:18:36.146076 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60492: Flags [.], seq 12246324
:12262980, ack 1, win 235, options [nop,nop,TS val 2226577460 ecr 3054291365], length 16656: HTTP
07:18:36.184349 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60518: Flags [.], seq 14346368
:14365800, ack 1, win 235, options [nop,nop,TS val 2226577504 ecr 3054291408], length 19432: HTTP
07:18:37.756320 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60502: Flags [.], seq 14482392
:14499048, ack 1, win 235, options [nop,nop,TS val 2226579071 ecr 3054292976], length 16656: HTTP
07:18:39.601370 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60502: Flags [.], seq 16903064
:16919720, ack 1, win 235, options [nop,nop,TS val 2226580919 ecr 3054294822], length 16656: HTTP
07:18:39.601405 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60518: Flags [.], seq 18248036
:18264692, ack 1, win 235, options [nop,nop,TS val 2226580920 ecr 3054294823], length 16656: HTTP
07:18:39.803749 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60518: Flags [.], seq 18482608
:18503428, ack 1, win 235, options [nop,nop,TS val 2226581120 ecr 3054295023], length 20820: HTTP
07:18:40.793165 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60492: Flags [.], seq 17504068
:17519336, ack 1, win 235, options [nop,nop,TS val 2226582110 ecr 3054296012], length 15268: HTTP
07:18:43.371176 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60502: Flags [.], seq 20013327
:20032759, ack 206, win 243, options [nop,nop,TS val 2226584688 ecr 3054298591], length 19432: HTTP
07:18:44.411963 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60518: Flags [.], seq 23757008
:23776440, ack 1, win 235, options [nop,nop,TS val 2226585732 ecr 3054299635], length 19432: HTTP
07:18:44.966187 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60492: Flags [.], seq 22813168
:22828436, ack 1, win 235, options [nop,nop,TS val 2226586283 ecr 3054300188], length 15268: HTTP
07:18:45.117327 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60518: Flags [.], seq 24705012
:24728608, ack 1, win 235, options [nop,nop,TS val 2226586429 ecr 3054300333], length 23596: HTTP
07:18:45.277212 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60518: Flags [.], seq 24917376
:24936808, ack 1, win 235, options [nop,nop,TS val 2226586595 ecr 3054300499], length 19432: HTTP
07:18:45.828885 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60518: Flags [.], seq 25529484
:25546140, ack 1, win 235, options [nop,nop,TS val 2226587143 ecr 3054301046], length 16656: HTTP
07:18:46.579453 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60518: Flags [.], seq 26445564
:26467772, ack 1, win 235, options [nop,nop,TS val 2226587897 ecr 3054301801], length 22208: HTTP
07:18:47.231571 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60518: Flags [.], seq 27289468
:27313064, ack 1, win 235, options [nop,nop,TS val 2226588539 ecr 3054302443], length 23596: HTTP
07:18:47.233482 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60518: Flags [.], seq 27320004
:27342212, ack 1, win 235, options [nop,nop,TS val 2226588544 ecr 3054302447], length 22208: HTTP
07:18:47.244223 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60502: Flags [.], seq 23412784
:23434992, ack 412, win 252, options [nop,nop,TS val 2226588561 ecr 3054302463], length 22208: HTTP
07:18:48.436664 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60518: Flags [.], seq 28565040
:28581696, ack 1, win 235, options [nop,nop,TS val 2226589755 ecr 3054303658], length 16656: HTTP
07:18:51.200951 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60502: Flags [.], seq 26325468
:26347676, ack 824, win 269, options [nop,nop,TS val 2226592521 ecr 3054306424], length 22208: HTTP
07:18:52.964080 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60502: Flags [.], seq 28272832
:28288100, ack 824, win 269, options [nop,nop,TS val 2226594283 ecr 3054308187], length 15268: HTTP
07:18:55.767225 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60518: Flags [.], seq 34540015
:34555283, ack 217, win 243, options [nop,nop,TS val 2226597086 ecr 3054310989], length 15268: HTTP
07:18:55.769967 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60518: Flags [.], seq 34563611
:34583043, ack 217, win 243, options [nop,nop,TS val 2226597086 ecr 3054310989], length 19432: HTTP
07:18:57.631742 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60518: Flags [.], seq 36445739
:36461007, ack 217, win 243, options [nop,nop,TS val 2226598949 ecr 3054312853], length 15268: HTTP
07:18:59.225696 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60518: Flags [.], seq 37557527
:37579735, ack 217, win 243, options [nop,nop,TS val 2226600544 ecr 3054314448], length 22208: HTTP
07:19:01.274785 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60502: Flags [.], seq 38252634
:38272066, ack 1240, win 285, options [nop,nop,TS val 2226602588 ecr 3054316492], length 19432: HTTP
07:19:01.276871 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60502: Flags [.], seq 38284558
:38306766, ack 1240, win 285, options [nop,nop,TS val 2226602592 ecr 3054316496], length 22208: HTTP
07:19:01.432873 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60502: Flags [P.], seq 3847193
8:38488594, ack 1240, win 285, options [nop,nop,TS val 2226602750 ecr 3054316654], length 16656: HTTP
07:19:02.923067 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60502: Flags [.], seq 40484538
:40499806, ack 1240, win 285, options [nop,nop,TS val 2226604239 ecr 3054318144], length 15268: HTTP
07:19:03.321925 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60502: Flags [.], seq 40992546
:41011978, ack 1240, win 285, options [nop,nop,TS val 2226604636 ecr 3054318540], length 19432: HTTP
07:19:04.561931 IP mirrors.storpool.com.http > ip-192-168-124-137.eu-west-1.compute.internal.60502: Flags [.], seq 42717830
:42737262, ack 1240, win 285, options [nop,nop,TS val 2226605871 ecr 3054319774], length 19432: HTTP
```

Okay so again that is alot of responses. There seems to be a common host which is `mirrors.storpool.com.http`. We can of course use `-n` to drop the DNS resolution.

```Shell

user@ip-10-10-99-206:~$ tcpdump -r traffic.pcap greater 15000 -n
reading from file traffic.pcap, link-type EN10MB (Ethernet)
07:18:24.967023 IP 185.117.80.53.80 > 192.168.124.137.60518: Flags [.], seq 2140876081:2140896901, ack 741991605, win 235, 
options [nop,nop,TS val 2226566282 ecr 3054280184], length 20820: HTTP
07:18:25.778012 IP 185.117.80.53.80 > 192.168.124.137.60518: Flags [.], seq 1293616:1308884, ack 1, win 235, options [nop,n
op,TS val 2226567095 ecr 3054280994], length 15268: HTTP
07:18:25.861724 IP 185.117.80.53.80 > 192.168.124.137.60518: Flags [.], seq 1378284:1397716, ack 1, win 235, options [nop,n
op,TS val 2226567176 ecr 3054281078], length 19432: HTTP
07:18:26.457422 IP 185.117.80.53.80 > 192.168.124.137.60518: Flags [.], seq 2356824:2373480, ack 1, win 235, options [nop,n
op,TS val 2226567777 ecr 3054281682], length 16656: HTTP
07:18:26.746414 IP 185.117.80.53.80 > 192.168.124.137.60492: Flags [.], seq 964376218:964395650, ack 1034282473, win 235, o
ptions [nop,nop,TS val 2226568063 ecr 3054281963], length 19432: HTTP
07:18:26.978560 IP 185.117.80.53.80 > 192.168.124.137.60502: Flags [.], seq 3786023752:3786039020, ack 3169565691, win 235,
 options [nop,nop,TS val 2226568298 ecr 3054282201], length 15268: HTTP
07:18:27.195761 IP 185.117.80.53.80 > 192.168.124.137.60492: Flags [.], seq 570468:589900, ack 1, win 235, options [nop,nop
,TS val 2226568513 ecr 3054282418], length 19432: HTTP
07:18:27.391916 IP 185.117.80.53.80 > 192.168.124.137.60492: Flags [.], seq 831412:848068, ack 1, win 235, options [nop,nop
,TS val 2226568707 ecr 3054282611], length 16656: HTTP
07:18:28.010550 IP 185.117.80.53.80 > 192.168.124.137.60518: Flags [.], seq 4778884:4798316, ack 1, win 235, options [nop,n
op,TS val 2226569330 ecr 3054283233], length 19432: HTTP
[truncated to save space]
```

Going by the common sender of `185.117.80.53.80` I'm tempted to suggest this is the answer.
#### Answer 2
- `185.117.80.53.80` ✅


## Displaying Packets
### Tcpdump Output Display Options Summary
Tcpdump offers several options to customize how packet data is displayed, helping you analyze traffic more effectively depending on your needs.
#### 1. -q: Quick Output
- Displays **brief packet** info (e.g., IPs, ports, and protocol).
- Useful for a **clean, minimal view**.
#### 2. -e: Include Link-Level Header
- Adds **MAC** addresses and Ethernet details.
- Helpful for analyzing **ARP, DHCP**, or tracking devices on a LAN.
#### 3. -A: ASCII Output
- Displays **packet contents as ASCII text.**
- Best for inspecting **plain-text protocols** like HTTP.
#### 4. -xx: Hexadecimal Output
- Shows **raw packet data in hex** (octet by octet).
- Useful for analyzing **binary or encrypted data**.
#### 5. -X: Hex + ASCII Output
- Combines **hexadecimal and ASCII** views.
- Ideal for **deep inspection** of both structure and readable content.
#### Example Commands:

|    Command    |	       Description        |
|---------------|---------------------------|
| `tcpdump -q`  |	Quick, minimal output     |
| `tcpdump -e`  |	Show MAC addresses        |
| `tcpdump -A`  |	Show ASCII content        |
| `tcpdump -xx` |	Show hex content          |
| `tcpdump -X`  |	Show both hex and ASCII   |



### Question 1 - What is the MAC address of the host that sent an ARP request?
#### Process
The examples above shows that if we want to show **MAC addresses** we should use the `-e` argument. We are also looking for the source of the ARP request. 

With this in mind I want to see what that looks like before proceeding too far by running `tcpdump -r traffic.pcap -e`.

I won't dump the contents in here, it was gnarly. 

After some digging I have noticed that I could just filter by `arp` after having tried `proto[arp:...]` and various other perminations.

So lets try again with `tcpdump -r traffic.pcap arp -q -n -e`

```Shell
user@ip-10-10-99-206:~$ tcpdump -r traffic.pcap arp -q -n -e
reading from file traffic.pcap, link-type EN10MB (Ethernet)
07:18:29.940761 52:54:00:7c:d3:5b > ff:ff:ff:ff:ff:ff, ARP, length 42: Request who-has 192.168.124.137 tell 192.168.124.148
, length 28
07:18:29.940776 52:54:00:23:60:2b > 52:54:00:7c:d3:5b, ARP, length 42: Reply 192.168.124.137 is-at 52:54:00:23:60:2b, lengt
h 28
```
Here we can see `52:54:00:7c:d3:5b > ff:ff:ff:ff:ff:ff` which is the equivelent of `192.168.124.137 > 255.255.255.255` or otherwise a broadcast. We can assume this is the host reaching out for an answer.

We can test the MAC address `52:54:00:7c:d3:5b` to see if this is the answer.


#### Answer 1
- `52:54:00:7c:d3:5b` ✅


## Conclusion
