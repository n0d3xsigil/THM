| [Home](../README.md) | Cyber Security 101 | **NMAP: The Basics** |
<details open>
<summary><h2>Contents</h2></summary>

- [Introduction](#introduction)
- [Host Discovery: Who is Online](#host-discovery-Who-is-online)
- [Port Scanning: Who is Listening](#port-scanning-who-is-listening)
- [Version Detection: Extract More Information](#version-detection-extract-more-information)
- [Timing: How Fast is Fast](#timing-how-fast-is-fast)
- [Output: Controlling What You See](#output-controlling-what-you-see)
- [Conclusion and Summary](#conclusion-and-summary)

</details>

<details open>
<summary><h2>Introduction</h2>

Nmap (Network Mapper) is a powerful open-source tool used for network discovery, security auditing, and vulnerability assessment. It helps identify live hosts, open ports, running services, operating systems, and potential security issues across local or remote networks. With features like stealth scanning, service version detection, and the Nmap Scripting Engine (NSE), it’s widely used by system administrators and cybersecurity professionals for everything from routine audits to penetration testing. Nmap runs on Linux, Windows, and macOS, and also includes a graphical interface called Zenmap for users who prefer a GUI.

### Prerequisites:
A good understanding of the TCP/IP model and protocols is needed. Recommended prep rooms:
 - [Networking Concepts](Rooms/NetworkConcepts.md)
 - [Networking Essentials](Rooms/NetworkingEssentials.md)
 - [Networking Core Protocols](Rooms/NetworkingCoreProtocols)
 - [Networking Secure Protocols](Rooms/NetworkingSecureProtocols)

</details>

<details open>
 <summary><h2>Host Discovery: Who is Online</h2></summary>

The task focuses on identifying online devices using Nmap's ping scan (-sn), which detects live hosts without probing for open ports or services.
### Target Specification Methods in Nmap
- IP Range: `192.168.0.1-10`
- Subnet: `192.168.0.1/24`
- Hostname: e.g., `example.thm`

### Local Network Scanning
- Run Nmap with root or `sudo` to avoid restricted scans. On local networks (e.g., connected via Ethernet/WiFi), Nmap sends ARP requests.
- Example: `nmap -sn 192.168.66.0/24` discovers live devices and their MAC addresses, often useful for identifying device types.

### Remote Network Scanning
- For remote networks (separated by routers), ARP doesn’t work.
- Nmap uses various methods like:
  - **ICMP echo (ping)**
  - **SYN to port 443**
  - **TCP ACK to port 80**
- Example: `nmap -sn 192.168.11.0/24` detects responsive hosts via these methods.

### Additional Features
- **-sL**: List scan – shows which IPs would be scanned without sending any packets.
- More advanced scan types like `-PS`, `-PA`, and `-PU` allow specifying exact probe types (outside this lesson's scope).

### Use Case of -sn
- Low-noise scan ideal for just identifying devices on a network.
- Doesn’t reveal running services, a more aggressive scan is needed in the next task.

### Question 1 - What is the last IP address that will be scanned when your scan target is `192.168.0.1/27`?
#### Process
For this one I calculated the Network, First, Last and Broadcast addresses using CIDR (Classless Inter-Domain Routing).

Converting /27 to Binary equates to `11111111.11111111.11111111.11100000` or `255.255.255.112`

This results in the following addresses
- Network Address: `192.168.0.0`
- First Address: `192.168.0.1`
- Last Address: `192.168.0.30`
- Broadcast Address: `192.168.0.31`

Since the last usable address would be `192.168.0.30`. So I will go with that as my answer.

It turns out that `*.30` was not the answer, thus it must be the broadcast address (`*.31`) instead.
#### Answer 1
- `192.168.1.30` ❌
- `192.168.1.31` ✅

</details>

<details open>
<summary><h2>Port Scanning: Who is Listening</h2></summary>

After identifying live hosts with `-sn`, the next step is to find out **which network services are listening** on those hosts, services running on TCP or UDP ports (e.g., web servers, DNS, etc.).

### Types of Nmap Scans

|Option|Description|
|------|-----------|
|`-sT`|	TCP Connect Scan – completes full TCP handshake; easy to detect|
|`-sS`|	TCP SYN Scan (Stealth) – only sends SYN packet; less likely to be logged|
|`-sU`| UDP Scan – checks for services using UDP (e.g., DNS, DHCP, SNMP)|

### Port Selection Options

|    Option    |	Description                                 |
|--------------|----------------------------------------------|
|  `-F`        |	Fast scan – scans the 100 most common ports |
|  `-p10-1024` |  Scans ports 10 through 1024                 |
|  `-p-25`     |  Scans ports 1 to 25                         | 
|  `-p-`       |  Scans all ports (1–65535) – most thorough   |


### Key Concepts
 - **TCP Connect Scan (`-sT`)**: Tries to establish a full connection. Easy to detect but reliable.
 - **TCP SYN Scan (`-sS`)**: Sends only a SYN, gets a SYN-ACK if port is open, then sends RST to avoid completing the handshake. More stealthy.
 - **UDP Scan (`-sU`)**: No connection required. Nmap sends UDP packets; closed ports typically reply with "ICMP port unreachable".
 - **Default Behavior**: Nmap scans the 1,000 most common ports unless otherwise specified.


### Question 1 - How many TCP ports are open on the target system at `10.10.226.155`?
#### Process
Since we're just after a number of open ports we could just use the `-p-` argument.

```Shell
┌──(root㉿kali)-[~]
└─# nmap -p- 10.10.226.155
Starting Nmap 7.93 ( https://nmap.org ) at 2025-06-09 19:00 UTC
Nmap scan report for ip-10-10-226-155.eu-west-1.compute.internal (10.10.226.155)
Host is up (0.0029s latency).
Not shown: 65529 closed tcp ports (reset)
PORT     STATE SERVICE
7/tcp    open  echo
9/tcp    open  discard
13/tcp   open  daytime
17/tcp   open  qotd
22/tcp   open  ssh
8008/tcp open  http
MAC Address: 02:85:FF:2C:27:8D (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 3.86 seconds
```

As we can see we have ports `7`, `9`, `13`, `17`, `22`, and `8008` that are open, therefore we can go with `6` TCP ports being open.

#### Answer 1
- `6` ✅


### Question 2 - Find the listening web server on `10.10.226.155` and access it with your browser. What is the flag that appears on its main page?
#### Process
This one is made a little easier by the fact we have already scanned for ports in the previous question. If we look at the services we can see there is a `http` on port `8008`. We can open the browser and head over to `http://10.10.226.155:8008` and see what comes up.

#### Answer 2
- `THM{SECRET_PAGE_38B9P6}` ✅

</details>

<details open>
<summary><h2>Version Detection: Extract More Information</h2></summary>

Something

</details>

<details open>
<summary><h2>Timing: How Fast is Fast</h2></summary>

something

</details>

<details open>

<summary><h2>Output: Controlling What You See</h2></summary>

something

</details>


<details open>
<summary><h2>Conclusion and Summary</h2></summary>

something

</details>

