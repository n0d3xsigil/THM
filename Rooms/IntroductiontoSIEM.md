| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **Introduction to SIEM** |

# Introduction to SIEM

## Contents
- [Introduction](#introduction)
- [Network Visibility through SIEM](#network-visibility-through-siem)
- [Log Sources and Log Ingestion](#log-sources-and-log-ingestion)
- [Why SIEM](#Why-siem)


## 📘Introduction
In this section we learned that _SIEM_ stands for  **Security Information and Event Management system**.

### ❓ Question

> What does SIEM stand for?

#### 🧪 Process

I don't think this requires a process 😉

#### ✅ Answer

- `Security Information and Event Management system` ✅



## 📘Network Visibility through SIEM

The purpose of this section is to just describe how large amounts of data is generated in the forms of logs. Some logs are device specific whilst others may be network or connection specific. 

The SIEM is able to collect this data and present it in one system with a holistic view.


### ❓ Question 1

> Is Registry-related activity host-centric or network-centric?

#### 🧪 Process

The registry is on the machine, so this would be a `host-centric` activity.

Trying this as the answer

#### ✅ Answer

- `host-centric` ✅


### ❓ Question 2

> Is VPN related activity host-centric or network-centric?

#### 🧪 Process

A VPN would be an example of where one client connects via another 'clinet' or host. So this would be a `network-centric` activity.

Trying this as the answer

#### ✅ Answer

- `network-centric` ✅



## 📘Log Sources and Log Ingestion

Every device on a network will be generating some kind of logging. Let's go over some of the types of devices that may contribute.

### Windows Machine

One of the most common OS's in office environments is _Microsoft Windows_. Expect to see Windows 11 and Windows 10 running supreme, start to be concerned if you still see some Windows 7 machines and please run for the hills if you're still seeing Windows XP.

Windows logs are generally made available via the **Event Viewer** (`eventvwr.msc`). Events are assigned event id's which help with filtering. These logs can then be forwarded to the SIEM.

![image](https://github.com/user-attachments/assets/52d54886-b3e9-4886-a04f-95b70ec75e90)


### Linux Workstation

Wouldn't it be great if Linux was more prevelent in the office, well it's not, yet. Maybe this is the year of the Linux Desktop. 

Anyway the logs within Linux usually reside withing `/var/log`. I only have container on my machine with Linux so have a sample of a `pacman` log.

```shell
[UserID@HostName log]$ cat pacman.log
[2025-05-01T10:04:03+0000] [ALPM] installed filesystem (2024.11.21-1)
[2025-05-01T10:04:03+0000] [ALPM] installed linux-api-headers (6.14-1)
[2025-05-01T10:04:04+0000] [ALPM] installed tzdata (2025b-1)
[2025-05-01T10:04:05+0000] [ALPM] installed glibc (2.41+r48+g5cb575ca9a3d-1)
[2025-05-01T10:04:05+0000] [ALPM] installed gcc-libs (15.1.1+r7+gf36ec88aa85a-1)
[2025-05-01T10:04:07+0000] [ALPM] installed ncurses (6.5-3)
[2025-05-01T10:04:07+0000] [ALPM] installed readline (8.2.013-1)
[2025-05-01T10:04:07+0000] [ALPM] installed bash (5.2.037-2)
[2025-05-01T10:04:07+0000] [ALPM] installed acl (2.3.2-1)
[2025-05-01T10:04:07+0000] [ALPM] installed attr (2.5.2-1)
[2025-05-01T10:04:07+0000] [ALPM] installed gmp (6.3.0-2)
[2025-05-01T10:04:07+0000] [ALPM] installed zlib (1:1.3.1-2)
[2025-05-01T10:04:07+0000] [ALPM] installed sqlite (3.49.1-1)
[2025-05-01T10:04:07+0000] [ALPM] installed util-linux-libs (2.41-4)
[2025-05-01T10:04:07+0000] [ALPM] installed e2fsprogs (1.47.2-2)
[2025-05-01T10:04:07+0000] [ALPM] installed keyutils (1.6.3-3)
[2025-05-01T10:04:07+0000] [ALPM] installed gdbm (1.25-1)                
```


### Web Server

Since many _web server_'s are run on Linux the log structure can often be the same (`/var/log/apache` or `/var/log/httpd`). Since I don't have web server running on this machine, you'll just have to take my word.

### Log Ingestion

So how does log ingestion work? It's important that the logs are ingested to provide a much bigger picture of what is happening within the enviroment. Some common ingestion methods are


#### Agent / Forwarder

An agent is installed on the client, the agent will then collects the logs and forwards the logs on to the SIEM. 

The term `forwarder` is used by _Splunk_.


#### Syslog

A common tool used  for collecting event data or logs is syslog. This is a 3rd party tool that acts as a host service where devices can forward their events to.


#### Manual Upload

Let's hope this one isn't a common one, we can ingest bulk data manually, this data then gets 'normalised' and made available for analysis.


#### Port-Forwarding

Port-Forwarding is where a port is made available, the SIEM will listen to that port for incomming data. The endpoints will be configured to forward data that host on the specified port.



### ❓ Question

> In which location within a Linux environment are HTTP logs stored?

#### 🧪 Process

Let's do this one from memory. We'll go for `/var/log/httpd`

Trying this as the answer

#### ✅ Answer

- `/var/log/httpd` ✅



## 📘Why SIEM

Without centralising the logs it would be impossible to make informed decisions about acitivies within the environment, malicious or otherwise.

Using a SIEM allows you to detect and protect against threats in the Cyber Security function


### SIEM Capabilities

A major component of a SOC, it collects logs and examines comparing aginst rules, trigging events if required

Common capabilities:
- Correlation of events between sources
- Visability across host and network centric events
- enable efficent investigation of latest threats
- Discover events not detected by rules


### SOC Analyst Responsibilities

Analysts use SIEM solutions for better visability of events across the environment

Responsibilities include:
- Monitoring and Investigating
- Identifying False positives
- Tuning Rules which are causing the noise or False positives
- Reporting and Compliance
- Identifying blind spots in the network visibility and covering them
