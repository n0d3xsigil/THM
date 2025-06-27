| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **Digital Forensics Fundamentals** |

# Digital Forensics Fundamentals

## Contents
- [Introduction to Digital Forensics](#introduction-to-digital-forensics)
- [Digital Forensics Methodology](#digital-forensics-methodology)
- [Evidence Acquisition](#evidence-acquisition)
- [Windows Forensics](#windows-forensics)


## 📘Introduction to Digital Forensics
Forensics that fall in the remit of cyber crimes would be **Digital Forensics**. The scope of _cyber crime_ is any criminal activity conducted on or by using a digital device There are lots of tools, techniques, and procedures can be employed to analyse and document evidence in case of legal action.


Whilst digital devices solve many problems, global communication is now just a matter of message or call. Digital devices have made our lives more convenient, not just for the well meaning individual but also cyber criminals.

Consider a scenario. Michael robbed a bank. The police have _reasonable grounds_ to perform a search of the premises.

Multiple digital devices are seized including a **Laptop Computer**, a **Mobile Phone**, an **External Hard Drive**, and a **USB** flash drive.

These digital devices where referred to the **Digital Forensics Unit** for examination. The team followed the **chain of custody** procedures to ensure **Integrity** of the evidence.

All devices where imaged by a **secure forensics laboratory** using specific **digital forensics tools** which resulted in the following **digital evidence** being obtained:
- A **digitised map** of the targeted bank found on the suspects laptop, showing **pre-meditated** actions
- **Entry** and **exit** strategy for the targeted bank showing **escape planning**
- **physical security measures** of the targeted bank showing specifics of physical security and how to **circumvent the controls**
- Also found on the suspects laptop where **multimedia files** including photos and videos of **previous robberies** suspected to be linked based on file metadata and visual similarities
- Forensic analysis of the mobile phone found encrypted messages and calls linked to co-conspirators which included conversations linked to the robbery.  


### ❓ Question
> Which team was handed the case by law enforcement?
#### 🧪 Process
The case was forwarded to the `digital forensics` team.

Trying this as the answer
#### ✅ Answer
- `digital forensics` ✅

## 📘Digital Forensics Methodology
Lets go over the forensics methodology now, this section focuses on the **NIST** (National Institute for Standards and Technology) prescribed framework which is broken down in four phases.


![](Images/Pasted%20image%2020250625174405.png)




### Collection
This is the first phase for digital forensics, we ant to identify all the devices that can be collected and may contain data such as Computers, Cameras, USBs etc. 

We need to ensure that the original data is not tampered with during the collection of evidence and maintain proper documentation of all the items collected into evidence. 

### Examination
The next phase is examination. The shear amount of data collected could quite easily overwhelm investigators so the data will often be filtered to just the relevant data.

As an example,  an investigator may have collected data from an external hard drive from the criminals bedroom, only some of this data will be relevant.

In the examination phase you would only record the files that where relevant and move on to the next phase. 

### Analysis
This phase is critical. It is now when the data will be analysed by investigators and potentially link with other pieces of evidence to build a better picture or case. 

### Reporting
The final phase in digital forensics is the reporting of what was done. The report is to be detailed and contain the methodology and detailed findings.

It may contain recommendations for management or law enforcement. 

An executive summary is an important inclusion to the report to ensure understanding for all parties.


### Computer Forensics
It's important to note that there are different tools and techniques for digital forensics depending on the type of equipment being investigated. 

![](Images/Pasted%20image%2020250625192246.png)



#### Computer forensics
Computer forensics, the most common devices used in crime.

#### Mobile forensics
Mobile forensics could include **call records**, **messages**, **location data**, etc.

#### Network forensics
Network forensics could include the entire network, however could be focused on traffic logs

#### Database forensics
Databases can contain a huge amount of data, database forensics focuses on intrusion, manipulation, and exfiltration of data.
#### Cloud forensics
Cloud forensics can be difficult for investigators, it focuses on data stored on cloud infrastructure, often little evidence is stored.

#### Email forensics
Email forensics focuses on the email systems and investigating potential crimes.


### ❓ Question
> Which phase of digital forensics is concerned with correlating the collected data to draw any conclusions from it?
#### 🧪 Process
This is a apart of the analysis process.

Trying this as the answer
#### ✅ Answer
- `Analysis` ✅

### ❓ Question
> Which phase of digital forensics is concerned with extracting the data of interest from the collected evidence?
#### 🧪 Process
We would want to extract the data in order to examine it.

Trying this as the answer
#### ✅ Answer
- `Examination` ✅


## 📘Evidence Acquisition
Acquisition of evidence is critical part of the process. The forensics team must ensure all evidence is collected whilst preserving integrity. 

There are general practices that must be followed as part of the evidence acquisition.

### Proper Authorization
Forensics teams must have authorisation of the relevant authorities prior to the collection of any evidence. This may be known as a search warrant.

### Chain of Custody
The chain of custody is essential for the preservation of evidence by documenting who has had involvement with the evidence. The chain of custody is a document which records key information about the piece of evidence such as:
- Name and type of evidence
- Individual who collected evidence
- Date and time of collection
- location of evidence
- Access record.

This helps create an audit trail of the evidence and who has been involved in the process. 

NIST have a good example CoC document. 
- https://www.nist.gov/document/sample-chain-custody-formdocx

### Use of Write Blockers
A write blocker prevents the operating system from writing files back to the evidence potentially corrupting data, modifying timestamps, or creating unwanted files. 

### ❓ Question 1
> Which tool is used to ensure data integrity during the collection?
#### 🧪 Process
We want to use a write blocker to prevent modification of the evidence.

Trying this as the answer
#### ✅ Answer

- `write blocker`

### ❓ Question 2

> What is the name of the document that has all the details of the collected digital evidence?

#### 🧪 Process

The whole process is chain of custody

Trying this as the answer

#### ✅ Answer

- `chain of custody`


### Windows Forensics

Evidence captured from personal computers are most commong types of evidence. Devices may have different operating systems, think Windows, MacOS, Linux, Android etc...) the most common however is Windows. We can can take forensic images of the **Windows Operating System** as part of the data caollection phase. These images are essentially bit-for-bit copies. There are two types of forensic image we can obtain.

#### Disk Image

This is arguabliy the easier form of image to capture. A disk image is a bit-for-bit copy of the disk, saved to a disk, generally not the disk being captured for obvious reasons. Since it is an image of the disk it will contain everything that is on that disk, including deleted items.

#### Memory Image

The memory is a little more critical. Where as with Disk the sotrage is no volatile, memeory is. A capture must occour without allowing the system to loose power and as such, we should prioritise taking a mMemory image over the disk. 


#### Tools

##### FTK Imager

FTK imager, very popular for imaging Windows Operating Systems. GUI led and able to mount images for review. Capable of capture and review.

##### Autopsy

**Web Link**: [Autopsy](https://www.autopsy.com/)

Popular and open-source. Great for image review including searching, deleted file recoverymeta data, extension mistmatch etc.

##### DumpIt

**Web Link**: [DumpIt](https://www.toolwar.com/2014/01/dumpit-memory-dump-tools.html)

Captures memory images from Windows Operating Systems. CLI based and the image can be saved in vairous formats.

##### Volatility

**Web Link**: [Volatility](https://volatilityfoundation.org/)

Open source tool for analysing Memory Images. Extensible via plugins, Supports various operating systems.


### ❓ Question

> Which type of forensic image is taken to collect the volatile data from the operating system?

#### 🧪 Process

Non volatile would be the disks whilst RAM is volatile. 

Trying this as the answer

#### ✅ Answer

- `Memory Image` ✅
