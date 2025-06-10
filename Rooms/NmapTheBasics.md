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
<summary><h2>Introduction</h2></summary>

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

 Once you know which ports are open, you can learn more about the operating system and services using these options:
### Nmap Options
| Option | 	                                            Description                                                           |
|--------|--------------------------------------------------------------------------------------------------------------------|
|   `-O` | 	**OS Detection** – makes an educated guess based on system responses (e.g., Linux 4.x–5.x)                        |
|  `-sV` | 	**Service & Version Detection** – identifies the software and version running on open ports                       |
|   `-A` | 	**Aggressive Scan** – combines `-O`, `-sV`, traceroute, and more                                                  |
|  `-Pn` |  **No Ping** – treats all targets as alive, even if they don’t respond to ICMP; ensures scanning of "silent" hosts |

### Key Concepts
- **OS Detection (`-O`)**: Useful for identifying the OS, but not always 100% accurate.
- **Version Detection (`-sV`)**: Adds a “VERSION” column to results to show exact software (e.g., OpenSSH 8.9p1).
- **Aggressive Scan (`-A`)**: A powerful one-liner to gather comprehensive info, including OS, services, and traceroute.
- **No Ping (`-Pn`)**: Useful when hosts don’t respond to pings or when scanning hardened/filtered systems.


### Question 1 - What is the name and detected version of the web server running on `10.10.226.155`?
#### Process
As mentioned in the options above we can use -Sv to detect the service and version. So lets try `nmap -p8008 -sV`.

```Shell
┌──(root㉿kali)-[~]
└─# nmap -p8008 10.10.226.155 -sV
Starting Nmap 7.93 ( https://nmap.org ) at 2025-06-09 20:42 UTC
Nmap scan report for ip-10-10-226-155.eu-west-1.compute.internal (10.10.226.155)
Host is up (0.00016s latency).

PORT     STATE SERVICE VERSION
8008/tcp open  http    lighttpd 1.4.74
MAC Address: 02:85:FF:2C:27:8D (Unknown)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 7.24 seconds
```
The result we see i nthis list is `lighttpd 1.4.74`. Let's plug this into the answer and hope for the best.

#### Answer 1
- `lighttpd 1.4.74` ✅


## Timing: How Fast is Fast
### Nmap Timing and Performance Options Summary
Nmap allows you to control scan speed and behavior to balance stealth, speed, and reliability.

### 1. Timing Templates (-T0 to -T5)
Control how fast Nmap scans:

| Template |	   Name    |	   Behavior    |  	Example Duration (100 ports)   |
|----------|------------|----------------|----------------------------------|
|  `-T0`   |	Paranoid	  | Very slow	     | ~9.8 hours                       |
|  `-T1`   |	Sneaky	    | Slow	          | ~27.5 minutes                    |
|  `-T2`   |	Polite	    | Moderate       |	~40.5 seconds                    |
|  `-T3`   |	Normal	    | Default speed  |	~0.15 seconds                    |
|  `-T4`   |	Aggressive	| Fast	          | ~0.13 seconds                    |
|  `-T5`   |	Insane	    | Very fast	     | (not shown, but faster than T4)  |

Use `-T<number>` or `-T name` (e.g., `-T4` or `-T aggressive`).

### 2. Parallelism Options
Control how many probes are sent at once:
- `--min-parallelism <num>`: Minimum number of parallel probes.
- `--max-parallelism <num>`: Maximum number of parallel probes.

Nmap adjusts this automatically based on network performance.

### 3. Packet Rate Control
Set how fast packets are sent:
- `--min-rate <number>`: Minimum packets per second.
- `--max-rate <number>`: Maximum packets per second.
Applies to the entire scan, not per host.

### 4. Host Timeout
- `--host-timeout <time>`: Maximum time to wait for a host before skipping it.
 - Useful for slow or unresponsive hosts.

### Quick Reference Table
|                   Option                   | Description                           |
|--------------------------------------------|---------------------------------------|
| `-T<0-5>`                                  | Timing template (paranoid to insane)  |
| `--min-parallelism` / `--max-parallelism`  | Control number of simultaneous probes |
| `--min-rate` / `--max-rate`                | Control packet sending rate           |
| `--host-timeout`                           | Set max wait time per host            |

### Question 1 - What is the non-numeric equivalent of `-T4`?
#### Process
For this question, there is not much of a process. we can just glance at the detail above and see that the name `-T4` is `Aggressive`. 

- Trying this as the answer ❌. I should have read the answer format before suggesting that.

The spaceing is `__ _________` I still think it is agressive, I need to check the arguments. you wouldn't directly replace `t4` with agressive. 

The answer is actually above. When using the name we use the `-T` then the profile name `agressive`

- Trying this as the answer

#### Answer 1
- `Aggressive` ❌
- `-T agressive` ✅


## Output: Controlling What You See
### Nmap: Verbosity, Debugging, and Output Formats
This section focuses on two key areas:
- Getting more feedback during scans
- Saving scan results in different formats

### 1. Verbosity and Debugging
#### Verbosity (-v)
- Use `-v` to get **real-time updates** during a scan.
 - Shows scan stages like ARP ping, DNS resolution, and port scanning.
- Increase detail with more vs (e.g., `-vv`, `-vvvv`) or use `-v2`, `-v4`.
 - You can even press v during a scan to increase verbosity on the fly.

#### Debugging (-d)
- Use `-d` for **debug-level output**.
 - Add more ds (e.g., -`dd`, `-ddd`) or use `-d9` for **maximum detail**.
- Useful for troubleshooting or understanding Nmap’s internal behavior.
- _**Be cautious**_: high debug levels produce very large outputs.

### 2. Saving Scan Reports
Nmap supports multiple output formats:


|      Option      | Format Type |                    Description                    |
|------------------|-------------|---------------------------------------------------|
| `-oN <file>`     |	Normal	     | Human-readable output                             |
| `-oX <file>`     |	XML	        | Structured XML format                             |
| `-oG <file>`     |	Grepable	   | Easy to parse with grep or awk                    |
| `-oA <basename>` |	All	        | Saves in all three formats (.nmap, .xml, .gnmap)  |

Example:
```Shell
nmap -sS 192.168.139.1/24 -oA scan_results
```
This creates:
- `scan_results.nmap` (normal)
- `scan_results.xml` (XML)
- `scan_results.gnmap` (grepable)


### Question 1 - What option must you add to your nmap command to enable debugging?
#### Process
I was rather hopeing for a more complicated question here. This time we will read the answer format which is `__`. Since we are referring to debugging. It looks like we just need the most basic debugging level.

Let's go with `-d`

- Trying this as the answer

#### Answer 1
- `-d` ✅


## Conclusion and Summary
### Nmap Summary: Key Features and Arguments
This room covered essential Nmap capabilities for discovering hosts, scanning ports, detecting services, controlling scan speed, and saving results.

### Privileges
- **Run with sudo** to unlock full functionality (e.g., SYN scans).
- Without root, Nmap defaults to **TCP connect scan** (`-sT`) instead of **SYN scan** (`-sS`).

### Key Nmap Arguments
#### Target Listing & Host Discovery
|                   Option                   |                      Description                      |
|--------------------------------------------|-------------------------------------------------------|
| `-sL`                                      |	List scan (no packets sent)                           |
| `-sn`                                      |	Ping scan (host discovery only)                       |

#### Port Scanning
|                   Option                   |                      Description                      |
|--------------------------------------------|-------------------------------------------------------|
| `-sT`                                      |	TCP Connect scan                                      |
| `-sS`                                      |	TCP SYN scan (stealthy)                               |
| `-sU`                                      |	UDP scan                                              |
| `-F`                                       |	Fast scan (top 100 ports)                             |
| `-p-`                                      |	Scan all 65535 ports                                  |
| `-Pn`                                      |	Treat all hosts as online (skip ping)                 |

#### Service & OS Detection
|                   Option                   |                      Description                      |
|--------------------------------------------|-------------------------------------------------------|
| `-sV`                                      |	Detect service versions                               |
| `-O`                                       |	OS detection                                          |
| `-A`                                       |	Aggressive scan (includes -O, -sV, traceroute, etc.)  |

#### Timing & Performance
|                   Option                   |                      Description                      |
|--------------------------------------------|-------------------------------------------------------|
| `-T0` to `-T5`                             |	Timing templates (from paranoid to insane)            |
| `--min-parallelism` / `--max-parallelism`  |	Control number of parallel probes                     |
| `--min-rate` / `--max-rate`                |	Control packet send rate (packets/sec)                |
| `--host-timeout`                           |	Max time to wait per host                             |

#### Output Control
|                   Option                   |                      Description                      |
|--------------------------------------------|-------------------------------------------------------|
| `-v`, `-vv`, `-v4`                         |	Verbosity levels                                      |
| `-d`, `-d9`                                | Debugging levels                                      |

#### Saving Scan Results
|                   Option                   |                      Description                      |
|--------------------------------------------|-------------------------------------------------------|
| `-oN <file>`                               |	Normal (human-readable) output                        |
| `-oX <file>`                               |	XML output                                            |
| `-oG <file>`                               |	Grepable output                                       |
| `-oA <basename>`                           |	Save in all formats (.nmap, .xml, .gnmap)             |


### Question 1 - What kind of scan will Nmap use if you run `nmap 10.10.89.108` with local user privileges?
#### Process
When we run nmap with user priviliges we can only use the _TCP Connect Scan_ as the user is not allowed access to the full network stack. 

The answer format is `_______ ____`, it looks as through `Connect Scan` will fit this answer. 

- Trying this as the answer 

#### Answer 1
- `Connect Scan` ✅
