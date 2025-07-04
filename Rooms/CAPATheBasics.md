| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **CAPA: The Basics** |

# CAPA: The Basics

## Contents
- [Introduction](#introduction)
- [Tool Overview: How CAPA Works](#tool-overview-how-capa-works)
- [Dissecting CAPA Results Part 1: General Information, MITRE and MAEC](#dissecting-capa-results-part-1-general-information-mitre-and-maec)
- [Dissecting CAPA Results Part 2: Malware Behavior Catalogue](#dissecting-capa-results-part-2-malware-behavior-catalogue)


## 📘Introduction

I've neer heard of CAPA so this one is new to me. 

So **Common Analysis Platform for Artifacts** is a tool that is actually developed by Mandiant.  It allows discovery of capabilites of an executable files. 



## 📘Tool Overview: How CAPA Works


Lets walk through the process step by step.

We should have started th VM in the Introduction, so if not go backm, do that and wait 5 minutes for the VM to start.

Once in double click **Windows PowerShell** on the desktop
![image](https://github.com/user-attachments/assets/1249d976-8efb-4c2e-a4af-797910544cd6)

The guide tells us that that it may take some time for the prompt to show
![image](https://github.com/user-attachments/assets/13200a3b-5158-4c2c-8d11-dd1eff42ff06)

The guide also notes we should be in the folder `C:\Users\Administrator\Desktop\capa`, which we are.
![image](https://github.com/user-attachments/assets/1f71a3dd-3a73-421c-8736-f8428aad4ca4)

Next run the command `capa.exe .\cryptbot.bin`. The guide advises it may take some time to complete
![image](https://github.com/user-attachments/assets/6095dc07-fd7e-457a-abc0-a3af2653f4f2)



Some arguments whilst we are waiting:
- **`-h`**: Shows us the help
- **`-v`**: Shows verbose result document
- **`-vv`**: Shows very verbose result document

Whilst I'm waiting for the analysis to complete, I downloaded the example file and ran the command `Get-Content .\cryptbot-1726590367927.txt`

Here is the result
```PowerShell
PS C:\Users\UserID\Downloads> Get-Content .\cryptbot-1726590367927.txt
 -----------------------+-------------------------------------------------------------------------------------
¦ md5                    ¦ 3b9d26d2e7433749f2c32edb13a2b0a2                                                   ¦
¦ sha1                   ¦ 969437df8f4ad08542ce8fc9831fc49a7765b7c5                                           ¦
¦ sha256                 ¦ ae7bc6b6f6ecb206a7b957e4bb86e0d11845c5b2d9f7a00a482bef63b567ce4c                   ¦
¦ analysis               ¦ static                                                                             ¦
¦ os                     ¦ windows                                                                            ¦
¦ format                 ¦ pe                                                                                 ¦
¦ arch                   ¦ i386                                                                               ¦
¦ path                   ¦ C:\Users\Administrator\Desktop\capa                                                ¦
 ------------------------+------------------------------------------------------------------------------------

 ------------------------+------------------------------------------------------------------------------------
¦ ATT&CK Tactic          ¦ ATT&CK Technique                                                                   ¦
 ------------------------+------------------------------------------------------------------------------------
¦ DEFENSE EVASION        ¦ Obfuscated Files or Information T1027                                              ¦
¦                        ¦ Obfuscated Files or Information::Indicator Removal from Tools T1027.005            ¦
¦                        ¦ Virtualization/Sandbox Evasion::System Checks T1497.001                            ¦
+------------------------+------------------------------------------------------------------------------------¦
¦ DISCOVERY              ¦ File and Directory Discovery T1083                                                 ¦
+------------------------+------------------------------------------------------------------------------------¦
¦ EXECUTION              ¦ Command and Scripting Interpreter::PowerShell T1059.001                            ¦
¦                        ¦ Shared Modules T1129                                                               ¦
+------------------------+------------------------------------------------------------------------------------¦
¦ IMPACT                 ¦ Resource Hijacking T1496                                                           ¦
+------------------------+------------------------------------------------------------------------------------¦
¦ PERSISTENCE            ¦ Scheduled Task/Job::At T1053.002                                                   ¦
¦                        ¦ Scheduled Task/Job::Scheduled Task T1053.005                                       ¦
 ------------------------+------------------------------------------------------------------------------------

 ------------------------+------------------------------------------------------------------------------------
¦ MAEC Category               ¦ MAEC Value                                                                    ¦
 ------------------------+------------------------------------------------------------------------------------
¦ malware-category            ¦ launcher                                                                      ¦
 ------------------------+------------------------------------------------------------------------------------

 ------------------------+------------------------------------------------------------------------------------
¦ MBC Objective               ¦ MBC Behavior                                                                  ¦
 ------------------------+------------------------------------------------------------------------------------
¦ ANTI-BEHAVIORAL ANALYSIS    ¦ Virtual Machine Detection [B0009]                                             ¦
+-----------------------------+-------------------------------------------------------------------------------¦
¦ ANTI-STATIC ANALYSIS        ¦ Executable Code Obfuscation::Argument Obfuscation [B0032.020]                 ¦
¦                             ¦ Executable Code Obfuscation::Stack Strings [B0032.017]                        ¦
+-----------------------------+-------------------------------------------------------------------------------¦
¦ COMMUNICATION               ¦ HTTP Communication [C0002]                                                    ¦
¦                             ¦ HTTP Communication::Read Header [C0002.014]                                   ¦
+-----------------------------+-------------------------------------------------------------------------------¦
¦ DATA                        ¦ Check String [C0019]                                                          ¦
¦                             ¦ Encode Data::Base64 [C0026.001]                                               ¦
¦                             ¦ Encode Data::XOR [C0026.002]                                                  ¦
+-----------------------------+-------------------------------------------------------------------------------¦
¦ DEFENSE EVASION             ¦ Obfuscated Files or Information::Encoding-Standard Algorithm [E1027.m02]      ¦
+-----------------------------+-------------------------------------------------------------------------------¦
¦ DISCOVERY                   ¦ File and Directory Discovery [E1083]                                          ¦
+-----------------------------+-------------------------------------------------------------------------------¦
¦ EXECUTION                   ¦ Command and Scripting Interpreter [E1059]                                     ¦
+-----------------------------+-------------------------------------------------------------------------------¦
¦ FILE SYSTEM                 ¦ Create Directory [C0046]                                                      ¦
¦                             ¦ Delete File [C0047]                                                           ¦
¦                             ¦ Read File [C0051]                                                             ¦
¦                             ¦ Writes File [C0052]                                                           ¦
+-----------------------------+-------------------------------------------------------------------------------¦
¦ MEMORY                      ¦ Allocate Memory [C0007]                                                       ¦
+-----------------------------+-------------------------------------------------------------------------------¦
¦ PROCESS                     ¦ Create Process [C0017]                                                        ¦
 ------------------------+------------------------------------------------------------------------------------

 ------------------------+------------------------------------------------------------------------------------
¦ Capability                                           ¦ Namespace                                            ¦
 ------------------------+------------------------------------------------------------------------------------
¦ reference anti-VM strings                            ¦ anti-analysis/anti-vm/vm-detection                   ¦
¦ reference anti-VM strings targeting VMWare           ¦ anti-analysis/anti-vm/vm-detection                   ¦
¦ reference anti-VM strings targeting VirtualBox       ¦ anti-analysis/anti-vm/vm-detection                   ¦
¦ contain obfuscated stackstrings (2 matches)          ¦ anti-analysis/obfuscation/string/stackstring         ¦
¦ reference HTTP User-Agent string                     ¦ communication/http                                   ¦
¦ check HTTP status code                               ¦ communication/http/client                            ¦
¦ reference Base64 string                              ¦ data-manipulation/encoding/base64                    ¦
¦ encode data using XOR                                ¦ data-manipulation/encoding/xor                       ¦
¦ contain a thread local storage (.tls) section        ¦ executable/pe/section/tls                            ¦
¦ get common file path                                 ¦ host-interaction/file-system                         ¦
¦ create directory                                     ¦ host-interaction/file-system/create                  ¦
¦ delete file                                          ¦ host-interaction/file-system/delete                  ¦
¦ read file on Windows (4 matches)                     ¦ host-interaction/file-system/read                    ¦
¦ write file on Windows (5 matches)                    ¦ host-interaction/file-system/write                   ¦
¦ get thread local storage value                       ¦ host-interaction/process                             ¦
¦ create process on Windows                            ¦ host-interaction/process/create                      ¦
¦ allocate or change RWX memory                        ¦ host-interaction/process/inject                      ¦
¦ reference cryptocurrency strings                     ¦ impact/cryptocurrency                                ¦
¦ link function at runtime on Windows (5 matches)      ¦ linking/runtime-linking                              ¦
¦ parse PE header (4 matches)                          ¦ load-code/pe                                         ¦
¦ resolve function by parsing PE exports (186 matches) ¦ load-code/pe                                         ¦
¦ run PowerShell expression                            ¦ load-code/powershell/                                ¦
¦ schedule task via at                                 ¦ persistence/scheduled-tasks                          ¦
¦ schedule task via schtasks                           ¦ persistence/scheduled-tasks                          ¦
 ------------------------+------------------------------------------------------------------------------------
```


### ❓ Question 1

> What command-line option would you use if you need to check what other parameters you can use with the tool? Use the shortest format.

#### 🧪 Process

If I wasn't clear on what options are available for any commandline tool I would issue `/?`, `-h`, `--help` or some other variation. However in this instance, we know its `-h`.

Trying this as the answer

#### ✅ Answer

- `-h` ✅


### ❓ Question 2

What command-line options are used to find detailed information on the malware's capabilities? Use the shortest format.

#### 🧪 Process

The shortest format was just `-v` for verbose report document.

Trying this as the answer

#### ✅ Answer

- `-v` ✅


### ❓ Question 3

> What command-line options do you use to find very verbose information about the malware's capabilities? Use the shortest format.

#### 🧪 Process

Very verbose would be `-vv`.

Trying this as the answer

#### ✅ Answer

- `-vv` ✅


### ❓ Question 4

> What PowerShell command will you use to read the content of a file?

#### 🧪 Process

On of my favorites `cat`. Only joking, `Get-Content`.

Trying this as the answer

#### ✅ Answer

- `Get-Content` ✅


## 📘Dissecting CAPA Results Part 1: General Information, MITRE and MAEC

In this section, we pull the report apart and discuss it.

### Basic information

Here we can see the hashes (**md5**, **sha1**, and **sha256**) and the type of **analysis**, the **OS**, the binary **format**, **arch**itecture and finally the **path** of the binary
```text
 -----------------------+-------------------------------------------------------------------------------------
¦ md5                    ¦ 3b9d26d2e7433749f2c32edb13a2b0a2                                                   ¦
¦ sha1                   ¦ 969437df8f4ad08542ce8fc9831fc49a7765b7c5                                           ¦
¦ sha256                 ¦ ae7bc6b6f6ecb206a7b957e4bb86e0d11845c5b2d9f7a00a482bef63b567ce4c                   ¦
¦ analysis               ¦ static                                                                             ¦
¦ os                     ¦ windows                                                                            ¦
¦ format                 ¦ pe                                                                                 ¦
¦ arch                   ¦ i386                                                                               ¦
¦ path                   ¦ C:\Users\Administrator\Desktop\capa                                                ¦
 ------------------------+------------------------------------------------------------------------------------
```
### MITRE ATT&CK

MITRE is just the company, not an acronym whilst `ATT&CT` stands for **Adversarial Tactics, Techniques, and Common Knowledge**.

The ATT&CK framework is a representation of global knowledge that is documented tatics and techniques that is deployed by treat actors at each stage of an attack. 

Below we can see 

```text
 ------------------------+------------------------------------------------------------------------------------
¦ ATT&CK Tactic          ¦ ATT&CK Technique                                                                   ¦
 ------------------------+------------------------------------------------------------------------------------
¦ DEFENSE EVASION        ¦ Obfuscated Files or Information T1027                                              ¦
¦                        ¦ Obfuscated Files or Information::Indicator Removal from Tools T1027.005            ¦
¦                        ¦ Virtualization/Sandbox Evasion::System Checks T1497.001                            ¦
+------------------------+------------------------------------------------------------------------------------¦
¦ DISCOVERY              ¦ File and Directory Discovery T1083                                                 ¦
+------------------------+------------------------------------------------------------------------------------¦
¦ EXECUTION              ¦ Command and Scripting Interpreter::PowerShell T1059.001                            ¦
¦                        ¦ Shared Modules T1129                                                               ¦
+------------------------+------------------------------------------------------------------------------------¦
¦ IMPACT                 ¦ Resource Hijacking T1496                                                           ¦
+------------------------+------------------------------------------------------------------------------------¦
¦ PERSISTENCE            ¦ Scheduled Task/Job::At T1053.002                                                   ¦
¦                        ¦ Scheduled Task/Job::Scheduled Task T1053.005                                       ¦
 ------------------------+------------------------------------------------------------------------------------
```

The tactic can be used to filter the technique.

So for example **`EXECUTION`** → **`Command and Scripting Interpreter`** [[T1059](https://attack.mitre.org/techniques/T1059/)]

![image](https://github.com/user-attachments/assets/08dc6e93-5967-4461-9349-5f761e588a53)

Can lead us to **`PowerShell`** [[001](https://attack.mitre.org/techniques/T1059/001/)]

![image](https://github.com/user-attachments/assets/22fa070d-bf0e-43bb-90f7-51f0569bcecc)

### MAEC

MAEC stands for **Malware Attribute Enumeration and Characterization**. It's a way of describing how the malware behaves.

```text
 ------------------------+------------------------------------------------------------------------------------
¦ MAEC Category               ¦ MAEC Value                                                                    ¦
 ------------------------+------------------------------------------------------------------------------------
¦ malware-category            ¦ launcher                                                                      ¦
 ------------------------+------------------------------------------------------------------------------------
```
CAPA has catagorised this as `launcher`. SO it is anticipated that it may:
- Dropping additional payloads
- Activating persistence mechanisms
- Connecting to command-and-control (C2) servers
- Executing specific functions

Useful links:
- [Malware Behavior Catalog](https://github.com/MBCProject/mbc-markdown)
- [Malware Behavior Catalog Matrix](https://maecproject.github.io/ema/)
    - [MAEC Core Specification, Version 5.0](https://maecproject.github.io/releases/5.0/MAEC_Core_Specification.pdf)
    - [MAEC Vocabularies Specification, Version 5.0
](https://maecproject.github.io/releases/5.0/MAEC_Vocabularies_Specification.pdf)


### ❓ Question 1

> What is the sha256 of cryptbot.bin?

#### 🧪 Process

We can find this in our basic information

```text
│ sha256      │ ae7bc6b6f6ecb206a7b957e4bb86e0d11845c5b2d9f7a00a482bef63b567ce4c                   │
```
Result = `ae7bc6b6f6ecb206a7b957e4bb86e0d11845c5b2d9f7a00a482bef63b567ce4c`

Trying this as the answer

#### ✅ Answer

- `ae7bc6b6f6ecb206a7b957e4bb86e0d11845c5b2d9f7a00a482bef63b567ce4c` ✅


### ❓ Question 2

> What is the **Technique** Identifier of **Obfuscated Files or Information**?

#### 🧪 Process

Visit the [MITRE ATT&CK](https://attack.mitre.org/) site and look under **Defense Evasion**.

Scroll down and find **Obfuscated Files or Information**. 

Here we can find information on _Obfuscated Files or Information_.

![image](https://github.com/user-attachments/assets/dd324845-d19d-4e57-9d8f-3c267d3186be)

As we can see from the above screenshot the ID is `T1027`. 

Trying this as the answer

#### ✅ Answer

- `T1027` ✅


### ❓ Question 3

> What is the **Sub-Technique** Identifier of **Obfuscated Files or Information::Indicator Removal from Tools**?

#### 🧪 Process

Since we're already in the correct page, we need to find _Indicator Removal from Tools_ and click
![image](https://github.com/user-attachments/assets/3c073426-0001-43da-9329-429130f6637a)

Now we can see the Technique and sub technique ID. 
![image](https://github.com/user-attachments/assets/6045e637-d571-4bf1-a83e-68cbb18298e2)

Our result is `T1027.005`

Trying this as the answer

#### ✅ Answer

- `T1027.005` ✅


### ❓ Question 4

> When CAPA tags a file with this MAEC value, it indicates that it demonstrates behaviour similar to, but not limited to, **Activating persistence mechanisms**?


#### 🧪 Process

The event `Activating persistence mechanisms` is under `launcher`

Trying this as the answer

#### ✅ Answer

- `launcher` ✅


### ❓ Question 5

> When CAPA tags a file with this MAEC value, it indicates that the file demonstrates behaviour similar to, but not limited to, **Fetching additional payloads or resources from the internet**?

#### 🧪 Process

`Fetching additional payloads` would fall under `Downloader`

Trying this as the answer

#### ✅ Answer

- `Downloader` ✅



## 📘Dissecting CAPA Results Part 2: Malware Behavior Catalogue

Here we have the next block

```text
 ------------------------+------------------------------------------------------------------------------------
¦ MBC Objective               ¦ MBC Behavior                                                                  ¦
 ------------------------+------------------------------------------------------------------------------------
¦ ANTI-BEHAVIORAL ANALYSIS    ¦ Virtual Machine Detection [B0009]                                             ¦
+-----------------------------+-------------------------------------------------------------------------------¦
¦ ANTI-STATIC ANALYSIS        ¦ Executable Code Obfuscation::Argument Obfuscation [B0032.020]                 ¦
¦                             ¦ Executable Code Obfuscation::Stack Strings [B0032.017]                        ¦
+-----------------------------+-------------------------------------------------------------------------------¦
¦ COMMUNICATION               ¦ HTTP Communication [C0002]                                                    ¦
¦                             ¦ HTTP Communication::Read Header [C0002.014]                                   ¦
+-----------------------------+-------------------------------------------------------------------------------¦
¦ DATA                        ¦ Check String [C0019]                                                          ¦
¦                             ¦ Encode Data::Base64 [C0026.001]                                               ¦
¦                             ¦ Encode Data::XOR [C0026.002]                                                  ¦
+-----------------------------+-------------------------------------------------------------------------------¦
¦ DEFENSE EVASION             ¦ Obfuscated Files or Information::Encoding-Standard Algorithm [E1027.m02]      ¦
+-----------------------------+-------------------------------------------------------------------------------¦
¦ DISCOVERY                   ¦ File and Directory Discovery [E1083]                                          ¦
+-----------------------------+-------------------------------------------------------------------------------¦
¦ EXECUTION                   ¦ Command and Scripting Interpreter [E1059]                                     ¦
+-----------------------------+-------------------------------------------------------------------------------¦
¦ FILE SYSTEM                 ¦ Create Directory [C0046]                                                      ¦
¦                             ¦ Delete File [C0047]                                                           ¦
¦                             ¦ Read File [C0051]                                                             ¦
¦                             ¦ Writes File [C0052]                                                           ¦
+-----------------------------+-------------------------------------------------------------------------------¦
¦ MEMORY                      ¦ Allocate Memory [C0007]                                                       ¦
+-----------------------------+-------------------------------------------------------------------------------¦
¦ PROCESS                     ¦ Create Process [C0017]                                                        ¦
 ------------------------+------------------------------------------------------------------------------------
```

### Malware Behavior Catalogue (MBC)

The MBC is like a library of malware behaviours. I it can describe behaviours and report findings in a standardised way. Often ties in with the MITRE **ATT&CK** framework, but not a copy. 

**Reference**: [Malware Behavior Catalog v3.1](https://github.com/MBCProject/mbc-markdown)

### Objective

**See**: [Malware Objectives](https://github.com/MBCProject/mbc-markdown/blob/main/mbc_summary.md#malware-objectives)

MBC malware objectives are based on MITRE ATT&CK tactics but tailored for malware analysis. It also adds two unique objectives not found in ATT&CK: Anti-Behavioral Analysis and Anti-Static Analysis, which focus on evading analysis techniques.


### Micro-Objective

MBC includes micro-behaviors that are low-level actions like creating sockets or checking strings. Often aren't malicious but commonly appear in malware and support higher-level objectives almost like signatures.


### MBC Behaviors

**See**: [Malware Behaviors](https://github.com/MBCProject/mbc-markdown/blob/main/mbc_summary.md#malware-behaviors)

MBC lists behaviors under each objective, linking to MITRE ATT&CK where relevant but not duplicating its content. The behavior names may differ, and MBC is meant to complement ATT&CK by adding extra detail specific to malware analysis.


### Micro-Behavior

**See**: [Malware Micro-behaviors](https://github.com/MBCProject/mbc-markdown/blob/main/mbc_summary.md#malware-micro-behaviors)

MBC captures low-level behaviors such as creating sockets or checking strings that aren't always malicious but are commonly seen in malware and support broader objectives.


### Methods

Methods in MBC are tied to behaviors, they either refine the behavior or show how it’s implemented. They’re similar to ATT&CK sub-techniques, but a method can’t exist on its own without being linked to a behavior.


### ❓ Question 1

> What serves as a catalogue of malware objectives and behaviours?

#### 🧪 Process

The `Malware Behavior Catalog`,

Trying this as the answer

#### ✅ Answer

- `Malware Behavior Catalogue` ✅

_**Note:** the spelling difference of `Catalog` and `Catalogue`._


### ❓ Question 2

>  Which field is based on ATT&CK tactics in the context of malware behaviour?

#### 🧪 Process

> _`objectives` are based on MITRE ATT&CK tactics_

Trying this as the answer

#### ✅ Answer

- `objectives` ✅


### ❓ Question 3

> What is the Identifier of "**Create Process**" micro-behavior?

#### 🧪 Process

Navigate to [#malware-micro-behaviors](https://github.com/MBCProject/mbc-markdown/blob/main/mbc_summary.md#malware-micro-behaviors)
Scroll down to **create process**
![image](https://github.com/user-attachments/assets/1dfadba0-7955-4773-b403-4ef61a3b7548)

Noice the identifyer of `C0017`. 

Trying this as the answer

#### ✅ Answer

- `C0017` ✅


### ❓ Question 4

> What is the behaviour with an Identifier of **B0009**?

#### 🧪 Process

Navigate to [MBC Summary](https://github.com/MBCProject/mbc-markdown/blob/main/mbc_summary.md)
Perform a search for `B0009`
![image](https://github.com/user-attachments/assets/a80d352e-f8a6-4a43-9964-18e9044b1d01)

This is not the result you are looking for. Hit **F3** to find the next result
![image](https://github.com/user-attachments/assets/a620af83-00aa-4870-b59c-5abec61eabb4)

This is the result we are looking for (`Virtual Machine Detection`)

Trying this as the answer

#### ✅ Answer

- `Virtual Machine Detection`✅


### ❓ Question 5

> Malware can be used to obfuscate data using base64 and XOR. What is the related **micro-behavior** for this?

#### 🧪 Process

Navigate to [#malware-micro-behaviors](https://github.com/MBCProject/mbc-markdown/blob/main/mbc_summary.md#malware-micro-behaviors

If I remember correctly that was "Encode Data". Lets see if that is there, Press **F3** to search and type "Encode Data" followed by enter.
![image](https://github.com/user-attachments/assets/824bf45b-d8ff-481b-beff-03c442f3b55d)

Great, that is just the one result, so probably on the right track, let's validate by clicking on the [`Encode Data`](https://github.com/MBCProject/mbc-markdown/blob/main/micro-behaviors/data/encode-data.md) link
![image](https://github.com/user-attachments/assets/63d7c430-dac5-4b47-8024-a1697fc991e2)

Perfect, it looks like `Encode Data` is what we're after

Trying this as the answer

#### ✅ Answer

- `Encode Data` ✅


### ❓ Question 6

> Which micro-behavior refers to "**Malware is capable of initiating HTTP communications**"?

#### 🧪 Process

Again, let's Navigate to [#malware-micro-behaviors](https://github.com/MBCProject/mbc-markdown/blob/main/mbc_summary.md#malware-micro-behaviors)

I'm not 100% sure, but lets hit **F3** and type "HTTP comm" followed by return to find the first result.
![image](https://github.com/user-attachments/assets/35ba828a-e4e4-4eec-9e4a-f5e3f4d03221)

Again, there is just the one result. I would be fairly confident in using this as the answer, however lets click that [`HTTP Communication`](https://github.com/MBCProject/mbc-markdown/blob/main/micro-behaviors/communication/http-communication.md) link and validate.
![image](https://github.com/user-attachments/assets/ab23a8ba-c8f2-4065-82cc-b685f790664a)

I won't go over everything here butterms such as "_connects to HTTP server_", "_creates request_", "_downloads URL to file_" all indicate HTTP communications initiated by the malware if desired.

The micro-behaviour in question looks to be `HTTP Communication`.

Trying this as the answer

#### ✅ Answer

- `HTTP Communication` ✅
