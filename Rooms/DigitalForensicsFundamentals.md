| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **Digital Forensics Fundamentals** |

# Digital Forensics Fundamentals

## Contents
- [Introduction to Digital Forensics](#introduction-to-digital-forensics)
- [Digital Forensics Methodology](#digital-forensics-methodology)
- [Evidence Acquisition](#evidence-acquisition)
- [Windows Forensics](#windows-forensics)
- [Practical Example of Digital Forensics](#practical-example-of-digital-forensics)


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


## 📘Practical Example of Digital Forensics

Scenario, the cat has been catnapped. The catnapper has requested a ransom via microsoft word.

Lets do this...

Fire up the Terminal. 

Navigate to the room files (`/root/Rooms/introdigitalforensics').

```shell
root@ip-10-10-192-37:~# cd /root/Rooms/introdigitalforensics/; ls -la
total 664
drwxr-xr-x  2 root root   4096 Mar  4  2022 .
drwxr-xr-x 41 root root   4096 May 23 09:40 ..
-rwxr-xr-x  1 root root 127107 Feb 23  2022 letter-image.jpg
-rwxr-xr-x  1 root root 153088 Feb 23  2022 ransom-letter.doc
-rwxr-xr-x  1 root root  71371 Feb 23  2022 ransom-letter.pdf
-rw-r--r--  1 root root 308063 Mar  4  2022 ransom-lettter-2.zip
```

We have a tool that can help with meta data for pdf files called `pdfinfo`, let's try that

```shell
Title:          Pay NOW
Subject:        We Have Gato
Author:         Ann Gree Shepherd
Creator:        Microsoft® Word 2016
Producer:       Microsoft® Word 2016
CreationDate:   Wed Feb 23 09:10:36 2022 GMT
ModDate:        Wed Feb 23 09:10:36 2022 GMT
Tagged:         yes
UserProperties: no
Suspects:       no
Form:           none
JavaScript:     no
Pages:          1
Encrypted:      no
Page size:      595.44 x 842.04 pts (A4)
Page rot:       0
File size:      71371 bytes
Optimized:      no
PDF version:    1.7
```

Oh, we have some interesting info, the page is `A4` so they are likely from Europe. The author is `Ann Gree Shepherd` and the file was created on `Wed Feb 23 2022` at `09:10:23 GMT`

We also have `EXIF` data as a part of images, this can be reviewed using `exiftool`. So lets do that on `letter-image.jpg`

```shell
root@ip-10-10-192-37:~/Rooms/introdigitalforensics# exiftool letter-image.jpg 
ExifTool Version Number         : 11.88
File Name                       : letter-image.jpg
Directory                       : .
File Size                       : 124 kB
File Modification Date/Time     : 2022:02:23 08:53:33+00:00
File Access Date/Time           : 2025:06:27 16:56:20+01:00
File Inode Change Date/Time     : 2022:03:04 12:15:19+00:00
File Permissions                : rwxr-xr-x
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Exif Byte Order                 : Little-endian (Intel, II)
Compression                     : JPEG (old-style)
Make                            : Canon
Camera Model Name               : Canon EOS R6
Orientation                     : Horizontal (normal)
X Resolution                    : 300
Y Resolution                    : 300
Resolution Unit                 : inches
Software                        : GIMP 2.10.28
Modify Date                     : 2022:02:15 17:23:40
Exposure Time                   : 1/200
F Number                        : 2.8
Exposure Program                : Manual
ISO                             : 640
Sensitivity Type                : Recommended Exposure Index
Recommended Exposure Index      : 640
Exif Version                    : 0231
Date/Time Original              : 2022:02:25 13:37:33
Create Date                     : 2022:02:25 13:37:33
Offset Time                     : +01:00
Offset Time Original            : +03:00
Offset Time Digitized           : +03:00
Shutter Speed Value             : 1/200
Aperture Value                  : 2.8
Exposure Compensation           : 0
Max Aperture Value              : 1.8
Metering Mode                   : Multi-segment
Flash                           : No Flash
Focal Length                    : 50.0 mm
User Comment                    : THM{238956}
Sub Sec Time Original           : 42
Sub Sec Time Digitized          : 42
Color Space                     : sRGB
Exif Image Width                : 7900
Exif Image Height               : 5267
Focal Plane X Resolution        : 1520
Focal Plane Y Resolution        : 1520
Focal Plane Resolution Unit     : cm
Custom Rendered                 : Normal
Exposure Mode                   : Manual
White Balance                   : Auto
Scene Capture Type              : Standard
Serial Number                   : 083021002010
Lens Info                       : 50mm f/?
Lens Model                      : EF50mm f/1.8 STM
Lens Serial Number              : 000029720b
GPS Latitude Ref                : North
GPS Longitude Ref               : West
GPS Time Stamp                  : 13:37:33
Subfile Type                    : Reduced-resolution image
Photometric Interpretation      : YCbCr
Samples Per Pixel               : 3
Thumbnail Offset                : 1214
Thumbnail Length                : 4941
XMP Toolkit                     : XMP Core 4.4.0-Exiv2
Api                             : 2.0
Platform                        : Linux
Time Stamp                      : 1644938627130718
Approximate Focus Distance      : 0.79
Distortion Correction Already Applied: True
Firmware                        : 1.2.0
Flash Compensation              : 0
Image Number                    : 0
Lateral Chromatic Aberration Correction Already Applied: True
Lens                            : EF50mm f/1.8 STM
Vignette Correction Already Applied: True
Color Mode                      : RGB
ICC Profile Name                : Adobe RGB (1998)
Creator Tool                    : GIMP 2.10
Metadata Date                   : 2021:12:02 13:32:48+01:00
Rating                          : 2
Document ID                     : adobe:docid:photoshop:de96cdf3-afbf-664d-9d4c-d5c1d0fdb4e1
Instance ID                     : xmp.iid:b80f5656-424a-4d4d-9cd0-5a36706d26d6
Original Document ID            : D3825C53382EED70DB7435B0CCF756F5
Preserved File Name             : 5L0A2971.CR3
Already Applied                 : True
Auto Lateral CA                 : 1
Blacks 2012                     : 0
Blue Hue                        : 0
Blue Saturation                 : 0
Camera Profile                  : Adobe Standard
Camera Profile Digest           : 441F68BD6BC3369B59256B103CE2CD5C
Clarity 2012                    : 0
Color Grade Blending            : 50
Color Grade Global Hue          : 0
Color Grade Global Lum          : 0
Color Grade Global Sat          : 0
Color Grade Highlight Lum       : 0
Color Grade Midtone Hue         : 0
Color Grade Midtone Lum         : 0
Color Grade Midtone Sat         : 0
Color Grade Shadow Lum          : 0
Color Noise Reduction           : 25
Color Noise Reduction Detail    : 50
Color Noise Reduction Smoothness: 50
Contrast 2012                   : 0
Crop Angle                      : 0
Crop Bottom                     : 1
Crop Constrain To Warp          : 0
Crop Left                       : 0
Crop Right                      : 1
Crop Top                        : 0
Defringe Green Amount           : 0
Defringe Green Hue Hi           : 60
Defringe Green Hue Lo           : 40
Defringe Purple Amount          : 0
Defringe Purple Hue Hi          : 70
Defringe Purple Hue Lo          : 30
Dehaze                          : 0
Exposure 2012                   : -0.40
Grain Amount                    : 0
Green Hue                       : 0
Green Saturation                : 0
Has Crop                        : False
Has Settings                    : True
Highlights 2012                 : -32
Hue Adjustment Aqua             : 0
Hue Adjustment Blue             : 0
Hue Adjustment Green            : 0
Hue Adjustment Magenta          : 0
Hue Adjustment Orange           : 0
Hue Adjustment Purple           : 0
Hue Adjustment Red              : 0
Hue Adjustment Yellow           : 0
Lens Manual Distortion Amount   : 0
Lens Profile Digest             : B23331240701D3B28825B46A4802290C
Lens Profile Distortion Scale   : 100
Lens Profile Enable             : 1
Lens Profile Filename           : Canon EOS-1Ds Mark III (Canon EF 50mm f1.8 STM) - RAW.lcp
Lens Profile Is Embedded        : False
Lens Profile Name               : Adobe (Canon EF 50mm f/1.8 STM)
Lens Profile Setup              : LensDefaults
Lens Profile Vignetting Scale   : 100
Luminance Adjustment Aqua       : 0
Luminance Adjustment Blue       : 0
Luminance Adjustment Green      : 0
Luminance Adjustment Magenta    : 0
Luminance Adjustment Orange     : 0
Luminance Adjustment Purple     : 0
Luminance Adjustment Red        : 0
Luminance Adjustment Yellow     : 0
Luminance Smoothing             : 0
Override Look Vignette          : False
Parametric Darks                : 0
Parametric Highlight Split      : 75
Parametric Highlights           : 0
Parametric Lights               : 0
Parametric Midtone Split        : 50
Parametric Shadow Split         : 25
Parametric Shadows              : 0
Perspective Aspect              : 0
Perspective Horizontal          : 0
Perspective Rotate              : 0.0
Perspective Scale               : 100
Perspective Upright             : 0
Perspective Vertical            : 0
Perspective X                   : 0.00
Perspective Y                   : 0.00
Post Crop Vignette Amount       : 0
Process Version                 : 11.0
Raw File Name                   : 5L0A2971.dng
Red Hue                         : 0
Red Saturation                  : 0
Saturation                      : 0
Saturation Adjustment Aqua      : 0
Saturation Adjustment Blue      : 0
Saturation Adjustment Green     : 0
Saturation Adjustment Magenta   : 0
Saturation Adjustment Orange    : 0
Saturation Adjustment Purple    : 0
Saturation Adjustment Red       : 0
Saturation Adjustment Yellow    : 0
Shadow Tint                     : 0
Shadows 2012                    : 0
Sharpen Detail                  : 25
Sharpen Edge Masking            : 60
Sharpen Radius                  : +1.0
Sharpness                       : 45
Split Toning Balance            : 0
Split Toning Highlight Hue      : 0
Split Toning Highlight Saturation: 0
Split Toning Shadow Hue         : 0
Split Toning Shadow Saturation  : 0
Color Temperature               : 6650
Texture                         : 0
Tint                            : -7
Tone Curve Name 2012            : Linear
Tone Curve PV2012               : 0, 0, 255, 255
Tone Curve PV2012 Blue          : 0, 0, 255, 255
Tone Curve PV2012 Green         : 0, 0, 255, 255
Tone Curve PV2012 Red           : 0, 0, 255, 255
Version                         : 14.0.1
Vibrance                        : 0
Vignette Amount                 : 0
Whites 2012                     : 0
Format                          : image/jpeg
Document Ancestors              : xmp.did:2ec1b1a6-ffae-0a44-90f9-3b6998456cdf, xmp.did:780a63d9-6024-e942-baf4-cae80b62a8c5
Derived From Document ID        : xmp.did:c3f1ef49-6aa6-4441-8800-6afa19131d22
Derived From Instance ID        : xmp.iid:fd37b6b6-4a37-d44a-89e0-3710c289a8db
Derived From Original Document ID: D3825C53382EED70DB7435B0CCF756F5
History Action                  : derived, saved, saved, saved, derived, saved, converted, saved, saved, converted, derived, saved, saved
History Parameters              : converted from image/x-canon-cr3 to image/dng, saved to new location, converted from image/dng to image/vnd.adobe.photoshop, saved to new location, from image/vnd.adobe.photoshop to application/vnd.adobe.photoshop, from application/vnd.adobe.photoshop to image/jpeg, converted from application/vnd.adobe.photoshop to image/jpeg
History Changed                 : /, /metadata, /metadata, /, /, /, /, /
History Instance ID             : xmp.iid:68afaab8-00f8-4a17-880d-04362acf7f59, xmp.iid:a415f140-19e3-dd4f-a523-2a91fd837241, xmp.iid:a732c1b4-c918-d649-91df-a08fd30a3b28, xmp.iid:c3f1ef49-6aa6-4441-8800-6afa19131d22, xmp.iid:e03136da-36b8-4a4f-a00f-4e953a46cb21, xmp.iid:fd37b6b6-4a37-d44a-89e0-3710c289a8db, xmp.iid:b0dfac61-4499-6b47-b061-c79f9c8868d9, xmp.iid:defc8f04-ab7b-4648-b9d4-1da9f1aa9bf9
History Software Agent          : Adobe Photoshop Lightroom Classic 10.2 (Macintosh), Adobe Photoshop Camera Raw 14.0, Adobe Photoshop Camera Raw 14.0.1 (Windows), Adobe Photoshop Camera Raw 14.0.1 (Windows), Adobe Photoshop 22.4 (Windows), Adobe Photoshop 22.4 (Windows), Adobe Photoshop 22.4 (Windows), Gimp 2.10 (Linux)
History When                    : 2021:11:15 15:50:41+03:00, 2021:12:01 11:25:22+01:00, 2021:12:01 12:34:12+01:00, 2021:12:02 10:19:47+01:00, 2021:12:02 12:53:12+01:00, 2021:12:02 13:32:48+01:00, 2021:12:02 13:32:48+01:00, 2022:02:15 17:23:47+02:00
Look Amount                     : 1
Look Copyright                  : © 2018 Adobe Systems, Inc.
Look Group                      : lang="x-default" Profiles
Look Name                       : Adobe Color
Look Supports Amount            : false
Look Supports Monochrome        : false
Look Supports Output Referred   : false
Look Uuid                       : B952C231111CD8E0ECCF14B86BAA7077
Look Parameters Camera Profile  : Adobe Standard
Look Parameters Convert To Grayscale: False
Look Parameters Look Table      : E1095149FDB39D7A057BAB208837E2E1
Look Parameters Process Version : 11.0
Look Parameters Tone Curve PV2012: 0, 0, 22, 16, 40, 35, 127, 127, 224, 230, 240, 246, 255, 255
Look Parameters Tone Curve PV2012 Blue: 0, 0, 255, 255
Look Parameters Tone Curve PV2012 Green: 0, 0, 255, 255
Look Parameters Tone Curve PV2012 Red: 0, 0, 255, 255
Look Parameters Version         : 14.0.1
Profile CMM Type                : Little CMS
Profile Version                 : 4.3.0
Profile Class                   : Display Device Profile
Color Space Data                : RGB
Profile Connection Space        : XYZ
Profile Date Time               : 2022:02:15 14:53:19
Profile File Signature          : acsp
Primary Platform                : Apple Computer Inc.
CMM Flags                       : Not Embedded, Independent
Device Manufacturer             : 
Device Model                    : 
Device Attributes               : Reflective, Glossy, Positive, Color
Rendering Intent                : Perceptual
Connection Space Illuminant     : 0.9642 1 0.82491
Profile Creator                 : Little CMS
Profile ID                      : 0
Profile Description             : GIMP built-in sRGB
Profile Copyright               : Public Domain
Media White Point               : 0.9642 1 0.82491
Chromatic Adaptation            : 1.04788 0.02292 -0.05022 0.02959 0.99048 -0.01707 -0.00925 0.01508 0.75168
Red Matrix Column               : 0.43604 0.22249 0.01392
Blue Matrix Column              : 0.14305 0.06061 0.71393
Green Matrix Column             : 0.38512 0.7169 0.09706
Red Tone Reproduction Curve     : (Binary data 32 bytes, use -b option to extract)
Green Tone Reproduction Curve   : (Binary data 32 bytes, use -b option to extract)
Blue Tone Reproduction Curve    : (Binary data 32 bytes, use -b option to extract)
Chromaticity Channels           : 3
Chromaticity Colorant           : Unknown (0)
Chromaticity Channel 1          : 0.64 0.33002
Chromaticity Channel 2          : 0.3 0.60001
Chromaticity Channel 3          : 0.15001 0.06
Device Mfg Desc                 : GIMP
Device Model Desc               : sRGB
Current IPTC Digest             : b417d6571f8aba97a1e64afbdedafbdb
Coded Character Set             : UTF8
Envelope Record Version         : 4
Date Created                    : 2022:02:15
Digital Creation Date           : 2021:11:05
Digital Creation Time           : 14:06:13+03:00
Application Record Version      : 4
Time Created                    : 17:23:40-17:23
Image Width                     : 1200
Image Height                    : 800
Encoding Process                : Progressive DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:4:4 (1 1)
Aperture                        : 2.8
Image Size                      : 1200x800
Megapixels                      : 0.960
Scale Factor To 35 mm Equivalent: 0.7
Shutter Speed                   : 1/200
Create Date                     : 2022:02:25 13:37:33.42+03:00
Date/Time Original              : 2022:02:25 13:37:33.42+03:00
Modify Date                     : 2022:02:15 17:23:40+01:00
Thumbnail Image                 : (Binary data 4941 bytes, use -b option to extract)
GPS Latitude                    : 51 deg 30' 51.90" N
GPS Longitude                   : 0 deg 5' 38.73" W
Date/Time Created               : 2022:02:15 17:23:40-17:23
Digital Creation Date/Time      : 2021:11:05 14:06:13+03:00
Circle Of Confusion             : 0.043 mm
Depth Of Field                  : 0.06 m (0.76 - 0.82 m)
Field Of View                   : 54.9 deg
Focal Length                    : 50.0 mm (35 mm equivalent: 34.6 mm)
GPS Position                    : 51 deg 30' 51.90" N, 0 deg 5' 38.73" W
Hyperfocal Distance             : 20.58 m
Light Value                     : 7.9
Lens ID                         : Canon EF 50mm f/1.8 STM
```

Loads of beautiful data, should have cleared this before sending a raondom. I like the fact that the're GPS co-ordinates
GPS Latitude                    : 51 deg 30' 51.90" N
GPS Longitude                   : 0 deg 5' 38.73" W

![image](https://github.com/user-attachments/assets/4b13f6ab-8a9a-4841-8ab8-04912c739b54)


Very intersting!

### Photo EXIF Data


### ❓ Question

> Using `pdfinfo`, find out the author of the attached PDF file, `ransom-letter.pdf`.

#### 🧪 Process

We already know this because the info tool returned this:
Author:         Ann Gree Shepherd

Trying this as the answer

#### ✅ Answer

- `Ann Gree Shepherd` ✅

### ❓ Question

> Using `exiftool` or any similar tool, try to find where the kidnappers took the image they attached to their document. What is the name of the street?

#### 🧪 Process

Let's convet `51 deg 30' 51.90" N, 0 deg 5' 38.73" W` 

to: `51°30'51.90"N, 0°5'38.73"W` and see what google makes of this...

Get evidence:
![image](https://github.com/user-attachments/assets/4b13f6ab-8a9a-4841-8ab8-04912c739b54)

The street is `Milk st`. We sometimes shorten _Street_ to `st`. So that would be come `Milk Street`

Trying this as the answer

#### ✅ Answer

- `Milk Street` ✅

### ❓ Question

> What is the model name of the camera used to take this photo?

#### 🧪 Process

The camera or manufacturer is empty however I did find the `Lens Profile Filename`:
`Lens Profile Filename           : Canon EOS-1Ds Mark III (Canon EF 50mm f1.8 STM) - RAW.lcp`

So I'm going to try `Canon EOS 1D` and see how we get on

Trying this as the answer


#### ✅ Answer

- `Canon EOS 1D` ❌

I kind of suspected as much since the model is actually Cannon EOS 1Ds but there wasn't space for the `s`. Lets check again.

I'm looking through the list going up and down and not seeing it. Time to go back to the terminal and throw in a bit of `grep`

```shell
root@ip-10-10-192-37:~/Rooms/introdigitalforensics# exiftool letter-image.jpg | grep -i "Canon"
Make                            : Canon
Camera Model Name               : Canon EOS R6
Lens Profile Filename           : Canon EOS-1Ds Mark III (Canon EF 50mm f1.8 STM) - RAW.lcp
Lens Profile Name               : Adobe (Canon EF 50mm f/1.8 STM)
History Parameters              : converted from image/x-canon-cr3 to image/dng, saved to new location, converted from image/dng to image/vnd.adobe.photoshop, saved to new location, from image/vnd.adobe.photoshop to application/vnd.adobe.photoshop, from application/vnd.adobe.photoshop to image/jpeg, converted from application/vnd.adobe.photoshop to image/jpeg
Lens ID                         : Canon EF 50mm f/1.8 STM
```

Oh, what do we have here:
```text
Camera Model Name               : Canon EOS R6
```

Lets try that

- `Canon EOS R6` ✅
