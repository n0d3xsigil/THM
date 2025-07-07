| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **FlareVM: Arsenal of Tools** |

# FlareVM: Arsenal of Tools

## Contents
- [Introduction](#introduction)
- [Arsenal of Tools](#arsenal-of-tools)
- [Commonly Used Tools for Investigation: Overview](#commonly-used-tools-for-investigation-overview)



## 📘Introduction

FlareVM stands for "Forensics, Logic Analysis, and Reverse Engineering" developed by the _FLARE_ team at fireeye.

It allows you to analyse malware, processes etc.


## 📘Arsenal of Tools

### Reverse Engineering & Debugging

> Reverse engineering is like solving a puzzle backward: you take a finished product apart to understand how it works. Debugging is identifying errors, understanding why they happen, and correcting the code to prevent them.

- **`Ghidra`** - NSA-developed open-source reverse engineering suite.
- **`x64dbg`** - Open-source debugger for binaries in x64 and x32 formats.
- **`OllyDbg`** - Debugger for reverse engineering at the assembly level.
- **`Radare2`** - A sophisticated open-source platform for reverse engineering.
- **`Binary Ninja`** - A tool for disassembling and decompiling binaries.
- **`PEiD`** - Packer, cryptor, and compiler detection tool.


### Disassemblers & Decompilers

> Disassemblers and Decompilers are crucial tools in malware analysis. They help analysts understand malicious software’s behaviour, logic, and control flow by breaking it into a more understandable format. The tools mentioned below are commonly used in this category.

- **`CFF Explorer`** - A PE editor designed to analyze and edit Portable Executable (PE) files.
- **`Hopper Disassembler`** - A Debugger, disassembler, and decompiler.
- **`RetDec`** - Open-source decompiler for machine code.


### Static & Dynamic Analysis

> Static and dynamic analysis are two crucial methods in cyber security for examining malware. Static analysis involves inspecting the code without executing it, while dynamic analysis involves observing its behaviour as it runs. The tools mentioned below are commonly used in this category.

- **`Process Hacker`** - Sophisticated memory editor and process watcher.
- **`PEview`** - A portable executable (PE) file viewer for analysis.
- **`Dependency Walker`** - A tool for displaying an executable’s DLL dependencies.
- **`DIE` (Detect It Easy)** - A packer, compiler, and cryptor detection tool.


### Forensics & Incident Response

> Digital Forensics involves the collection, analysis, and preservation of digital evidence from various sources like computers, networks, and storage devices. At the same time, Incident Response focuses on the detection, containment, eradication, and recovery from cyberattacks. The tools mentioned below are commonly used in this category.

- **`Volatility`** - RAM dump analysis framework for memory forensics.
- **`Rekall`** - Framework for memory forensics in incident response.
- **`FTK Imager`** - Disc image acquisition and analysis tools for forensic use.


### Network Analysis

> Network Analysis includes different methods and techniques for studying and analysing networks to uncover patterns, optimize performance, and understand the underlying structure and behaviour of the network.

- **`Wireshark`** - Network protocol analyzer for traffic recording and examination.
- **`Nmap`** - A vulnerability detection and network mapping tool.
- **`Netcat`** - Read and write data across network connections with this helpful tool.


### File Analysis

> File Analysis is a technique used to examine files for potential security threats and ensure proper file permissions.

- **`FileInsight`** - A program for looking through and editing binary files.
- **`Hex Fiend`** - Hex editor that is light and quick.
- **`HxD`** - Binary file viewing and editing with a hex editor.


### Scripting & Automation

> Scripting and Automation involve using scripts such as PowerShell and Python to automate repetitive tasks and processes, making them more efficient and less prone to human error.

- **`Python`** - Mainly automation-focused on Python modules and tools.
- **`PowerShell Empire`** - Framework for PowerShell post-exploitation.


### Sysinternals Suite

> The Sysinternals Suite is a collection of advanced system utilities designed to help IT professionals and developers manage, troubleshoot, and diagnose Windows systems.

- **`Autoruns`** - Shows what executables are configured to run during system boot-up.
- **`Process Explorer`** - Provides information about running processes.
- **`Process Monitor`** - Monitors and logs real-time process/thread activity.



### ❓ Question 1

> Which tool is an Open-source debugger for binaries in x64 and x32 formats?

#### 🧪 Process

```Text
**`x64dbg`** - Open-source debugger for binaries in x64 and x32 formats.
```

Trying this as the answer

#### ✅ Answer

- `x64dbg` ✅


### ❓ Question 2

> What tool is designed to analyze and edit Portable Executable (PE) files?

#### 🧪 Process

```Text
**`CFF Explorer`** - A PE editor designed to analyze and edit Portable Executable (PE) files.
```

Trying this as the answer

#### ✅ Answer

- `CFF Explorer` ✅


### ❓ Question 3

> Which tool is considered a sophisticated memory editor and process watcher?

#### 🧪 Process

```Text
**`Process Hacker`** - Sophisticated memory editor and process watcher.
```

Trying this as the answer

#### ✅ Answer

- `Process Hacker` ✅


### ❓ Question 4

> Which tool is used for Disc image acquisition and analysis for forensic use?

#### 🧪 Process

```Text
- **`FTK Imager`** - Disc image acquisition and analysis tools for forensic use.
```

Trying this as the answer

#### ✅ Answer

- `FTK Imager` ✅


### ❓ Question 5

> What tool can be used to view and edit a binary file?

#### 🧪 Process

```Text
- **`HxD`** - Binary file viewing and editing with a hex editor.
```

Trying this as the answer

#### ✅ Answer

- `HxD` ✅



## 📘Commonly Used Tools for Investigation: Overview

Let's look at some of the basic toosl that are commonly used

### Process Monitor (Procmon)

Procmon or Process Monitor is a part of the [SysInternals](https://learn.microsoft.com/en-us/sysinternals/downloads/procmon) utilities. It allows you to monitor the activity of an application, I've often used it to troubleshoot crashes, permissions issues so I am fairly familiar with how it works.

It's important to note that this tool generates a huge amount of data. Just opening `Notepad.exe` for example generates 1,260 events, ranging from Process Start, through Registry queries, File Reads and DLL loading etc.

![image](https://github.com/user-attachments/assets/049f7ab2-08e8-42d8-8749-9dcc26fbbbdb)


### Process Explorer (Procexp)

Procexp or Process Explorer allows you to review the process tree of a particular process. You can also customise the columns available

![image](https://github.com/user-attachments/assets/1cbe821f-7142-49ca-a260-b4121b79b395)


### HxD

Is a great Hex Editor that allows you to open files or pull in processes memory, drives etc. It can be used for foresnic analysis, data recovery ro debugging. 

![image](https://github.com/user-attachments/assets/5cde785f-819b-4cb6-8bf4-9a1f9da2ebcb)

Note, the hex `4D 5A` indicates an executable.


### CFF Explorer

I've not come across the CFF before, but it looks as though you can get indepth information on a file, generate file hashes. With this we can compare potentially malicious files against known authentic files.

![image](https://github.com/user-attachments/assets/4a90bd69-d97d-46cd-847d-4251600b9724)


### Wireshark

Wire shark is another common network troubleshooting tool. You can capture packets to a _PCAP_ file or analyse in real time. With this you can trace packets, combine convesations to get a better picture of what 'conversations' are happening between endpoints. 

![image](https://github.com/user-attachments/assets/12649b5e-955c-4f55-bccc-e19b68f206e5)



### PEStudio

Used for static analysis (analyses the file without running it) Provides advanced detail about the file without putting the host in danger.

![image](https://github.com/user-attachments/assets/3d7632fe-74d3-4fd1-aead-d197d9e3794a)


### FLOSS

Floss is another static analysis utility. (FLARE Obfuscated String Solver)

Automates the extraction of obsfsucated strings in an attempt to de-obfuscate the malware.

I won't copy the results since the formatting is not great, so have a screen shot instead

![image](https://github.com/user-attachments/assets/d479ca13-1c1f-43ce-af39-84c54b08f986)



### ❓ Question 01

> Which tool was formerly known as FireEye Labs Obfuscated String Solver?

#### 🧪 Process

This is the last tool we looked in to, `FLOSS`.

Trying this as the answer

#### ✅ Answer

- `FLOSS` ✅


### ❓ Question 02

> Which tool offers in-depth insights into the active processes running on your computer?

#### 🧪 Process

This is `Process Explorer`

Trying this as the answer

#### ✅ Answer

- `Process Explorer` ✅


### ❓ Question 03

> By using the Process Explorer (procexp) tool, under what process can we find smss.exe?

#### 🧪 Process

We can see from the following screen shot that `smss.exe` is under the `System`

![image](https://github.com/user-attachments/assets/1182372b-401c-4133-9517-31d6d77a91f9)

Trying this as the answer


#### ✅ Answer

- `System` ✅


### ❓ Question 04

> Which powerful Windows tool is designed to help you record issues with your system's apps?

#### 🧪 Process

We can use Process Monitor (`ProcMon`) to record issues with the system.

Trying this as the answer

#### ✅ Answer

- `ProcMon` ✅


### ❓ Question 05

> Which tool can be used for Static analysis or studying executable file properties without running the files?

#### 🧪 Process

One of the tools we can use for static analysis is PE studio or (`PEStudio`)

Trying this as the answer

#### ✅ Answer

- `PEStudio` ✅


### ❓ Question 06

> Using the tool PEStudio to open the file **cryptominer.bin** in the Desktop\Sample folder, what is the sha256 value of the file?

#### 🧪 Process

Double click **`pestudio`** on the Desktop

Maximise (only for the screenshot, you don't need to do this 😉)

Click **open file**

Double click **`cryptominer.bin`**

Find the `sha256` entry on the right and then right click the **value**

Click **Copy Value(s)**

This is our value `E9627EBAAC562067759681DCEBA8DDE8D83B1D813AF8181948C549E342F67C0E`

![image](https://github.com/user-attachments/assets/47a80702-84b1-4067-bb7e-f645c2052394)

Trying this as the answer

#### ✅ Answer

- `E9627EBAAC562067759681DCEBA8DDE8D83B1D813AF8181948C549E342F67C0E` ✅


### ❓ Question 07

> Using the tool PEStudio to open the file **cryptominer.bin** in the Desktop\Sample folder, how many functions does it have?

#### 🧪 Process

I spent way to long looking at the values trying to find functions. It is on the left hand navigation pane. functions (`102`)

![image](https://github.com/user-attachments/assets/a265d877-419b-4423-8f3e-121909802da5)

Trying this as the answer

#### ✅ Answer

- `102` ✅


### ❓ Question 08

> What tool can generate file hashes for integrity verification, authenticate the source of system files, and validate their validity?

#### 🧪 Process

Well PEStudio could do this, so let's go with another tool. `CFF Explorer`

Trying this as the answer

#### ✅ Answer

- `CFF Explorer` ✅


### ❓ Question 09

> Using the tool CFF Explorer to open the file **possible_medusa.txt** in the Desktop\Sample folder, what is the MD5 of the file?

#### 🧪 Process

![image](https://github.com/user-attachments/assets/fbe2fd35-c4ad-4761-9b2e-5f876559369c)

We can see the MD5 is `646698572AFBBF24F50EC5681FEB2DB7`

Trying this as the answer

#### ✅ Answer

- `646698572AFBBF24F50EC5681FEB2DB7` ✅


### ❓ Question 10

> Use the CFF Explorer tool to open the file **possible_medusa.txt** in the Desktop\Sample folder. Then, go to the **DOS Header Section**. What is the e_magic value of the file?

#### 🧪 Process

This is `5A4D`

Trying this as the answer

#### ✅ Answer

- `5A4D` ✅
