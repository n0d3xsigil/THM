| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **IDS Fundamentals** |

# IDS Fundamentals

## Contents
- [What Is an IDS](#what-is-an-ids)
- [Types of IDS](#types-of-ids)
- [IDS Example: Snort](#ids-example-snort)
- [Snort Usage](#snort-usage)



## 📘What Is an IDS

An IDS is different in from a firewall in that a firewall will deny traffic if it doesnt meet specific criteria. But what if you have port 80 open. Well any traffic that is using port 80 can slip through the net.

Instead, an IDS will monitor this activity and report on it. Rather than preventing (IPS) the intrusion, it detects and reports it.


### ❓ Question

> Can an intrusion detection system (IDS) prevent the threat after it detects it? Yea/Nay

#### 🧪 Process

We are talking about IDS not IPS, so `Nay`.

Trying this as the answer

#### ✅ Answer

- `Nay` ✅



## 📘Types of IDS

Lets go over the _deployment modes_ of the IDS.

### Deployment Modes

#### Host Intrusion Detection System

The **Host Intrusion Detection System** as it sounds is based on the host. It will only detect intrusions on that host. As a result it can make management difficult on large scales.

#### Network Intrusion Detection System

Where as a **Network Intrusion Detection System** will detect poteltially malicious activity across the entire network regardless of specific hosts. The main advangate is managability since it is centralised.


### Detection Modes

#### Signature-Based IDS

Signature based IDS are similar to AV. They use a signature of a previously known attack. If the malicious acitivity falls within this 'signature' it will be classified. However, the signature must exist to be detected. Zero day's wont by definition have a signature.

#### Anomaly-Based ID

An anomaly based IDS will create a baseline of network activity and will then generate alerts on deviations of the baseline. The downside is that they tend to generate larger quantities of _False-Positives_ so these rules must be tuned. 

#### Hybrid IDS

Where as _hybrid IDS_ will utilise the best of both worlds, it will utilise signatures for the known malicious behaviour and will use anomoly based for the rest. Reducing the number of false positives but allowing the flexibility and reassurance over the long term.

### ❓ Question 1

> Which type of IDS is deployed to detect threats throughout the network?

#### 🧪 Process

If we are detecting threts across the network, then we are not using a host based IDS. Therefore the deployment type would be `Network intrusion detection system`.

Trying this as the answer

#### ✅ Answer

- `Network intrusion detection system` ✅


### ❓ Question 2

> Which IDS leverages both signature-based and anomaly-based detection techniques?

#### 🧪 Process

We can use the `Hybrid IDS` to leverage both signature and anomaly based IDS.

Trying this as the answer

#### ✅ Answer

- `Hybrid IDS` ✅



## 📘IDS Example: Snort

Snort is a popular open source IDS. It is a hybrid type IDS where in it will use signature based detections in addition to anomaly to identify potential threats. These definitions are rules in the snort too. The tool comes with built in rules but you can customise to suite the environment.

There are 3 mods that the tool uses

### Packet sniffer mode

The _PS_ mode allows for packets reviewed, however they do not get analysed. It does however allow for troubleshooting and udnerstanding the flow of trafic.

For example, a netops team identfied a performance issue, they use the traffic data that was captured using sort to identify causes.

### Packet logging mode

The _PL_ mode performs detection on realtime traffic and provides alerts on detections at the console. If for some reason the traffic is of some interest we can use the _PL_ mode to log the traffic (PCAP) for later investigations.

For example, the secops team need to perform a forensic investigation. They can use the file captured to perform the forensic analysis

### Network Intrusion Detection System mode

The primary mode for the Snort NIDS and monitors in real time ot idenfity known patterns and log new attacks as signatures. Matches are generated as alerts.

For exmaple, The secops team proactivly monitors the environment to detect potential threats. Snort's NIDS will achieve this.


### ❓ Question 1

> Which mode of Snort helps us to log the network traffic in a PCAP file?

#### 🧪 Process

The `Packet Logging mode` will achive this by taking the packets captured with the _packet sniffer mode_ and saving them as a **PCAP** file.

Trying this as the answer

#### ✅ Answer

- `Packet Logging mode` ✅


### ❓ Question 2

> What is the primary mode of Snort called?

#### 🧪 Process

The primary mode is called the `Network Intrusion Detection System mode`. It is the primary function of Snort as in NIDS.

Trying this as the answer

#### ✅ Answer

- `Network Intrusion Detection System mode` ✅



## 📘Snort Usage

In order for Snort to operate as a NIDS. You must install Snort on a device that has a NIC that is compatible with promiscous mode. 

I have started the VM and it has loaded So I will step throught the section as though I am doing the walk through.

Show the rules by running `ls` against `/etc/snort/rules`
```Shell
ubuntu@tryhackme:~$ ls /etc/snort/rules
attack-responses.rules         community-web-dos.rules   policy.rules
backdoor.rules                 community-web-iis.rules   pop2.rules
bad-traffic.rules              community-web-misc.rules  pop3.rules
chat.rules                     community-web-php.rules   porn.rules
community-bot.rules            ddos.rules                rpc.rules
community-deleted.rules        deleted.rules             rservices.rules
community-dos.rules            dns.rules                 scan.rules
community-exploit.rules        dos.rules                 shellcode.rules
community-ftp.rules            experimental.rules        smtp.rules
community-game.rules           exploit.rules             snmp.rules
community-icmp.rules           finger.rules              sql.rules
community-imap.rules           ftp.rules                 telnet.rules
community-inappropriate.rules  icmp-info.rules           tftp.rules
community-mail-client.rules    icmp.rules                virus.rules
community-misc.rules           imap.rules                web-attacks.rules
community-nntp.rules           info.rules                web-cgi.rules
community-oracle.rules         local.rules               web-client.rules
community-policy.rules         misc.rules                web-coldfusion.rules
community-sip.rules            multimedia.rules          web-frontpage.rules
community-smtp.rules           mysql.rules               web-iis.rules
community-sql-injection.rules  netbios.rules             web-misc.rules
community-virus.rules          nntp.rules                web-php.rules
community-web-attacks.rules    oracle.rules              x11.rules
community-web-cgi.rules        other-ids.rules
community-web-client.rules     p2p.rules
```

I wonder what the files look like inside. I don't want a massive file so I would like to list all, sort in ascending order of size and then only show the first 4 or 5 resuls. 

We can do this by using `ls -la`**`Sr`** and then piping to `head -n5`

```Shell
ubuntu@tryhackme:~$ ls -laSr /etc/snort/rules/ | head -n5
total 1608
-rw-r--r-- 1 root root    249 Apr  3  2018 community-ftp.rules
-rw-r--r-- 1 root root    254 Apr  3  2018 community-web-dos.rules
-rw-r--r-- 1 root root    257 Apr  3  2018 community-mail-client.rules
-rw-r--r-- 1 root root    365 Jul 18  2024 local.rules
```

Perfect, we can see that actually `community-ftp.rules` is only 249 bytes. This should do. Lets `cat` it out and see what we have

```Shell
ubuntu@tryhackme:~$ cat /etc/snort/rules/community-ftp.rules 
# Copyright 2005 Sourcefire, Inc. All Rights Reserved.
# These rules are licensed under the GNU General Public License.
# Please see the file LICENSE in this directory for more details.
# $Id: community-ftp.rules,v 1.6 2005/03/08 14:41:42 bmc Exp $
```

okay, so maybe that was a bad example going specifically for the shortest file. Let's expand it to 14 results and see what is there

```Shell
ubuntu@tryhackme:~$ ls -laSr /etc/snort/rules/ | head -n15
total 1608
-rw-r--r-- 1 root root    249 Apr  3  2018 community-ftp.rules
-rw-r--r-- 1 root root    254 Apr  3  2018 community-web-dos.rules
-rw-r--r-- 1 root root    257 Apr  3  2018 community-mail-client.rules
-rw-r--r-- 1 root root    365 Jul 18  2024 local.rules
-rw-r--r-- 1 root root    621 Apr  3  2018 community-nntp.rules
-rw-r--r-- 1 root root    689 Apr  3  2018 community-icmp.rules
-rw-r--r-- 1 root root    775 Apr  3  2018 community-oracle.rules
-rw-r--r-- 1 root root    948 Apr  3  2018 community-inappropriate.rules
-rw-r--r-- 1 root root   1223 Apr  3  2018 community-deleted.rules
-rw-r--r-- 1 root root   1335 Apr  3  2018 experimental.rules
-rw-r--r-- 1 root root   1376 Apr  3  2018 community-game.rules
-rw-r--r-- 1 root root   1437 Apr  3  2018 x11.rules
-rw-r--r-- 1 root root   1473 Apr  3  2018 community-web-iis.rules
-rw-r--r-- 1 root root   1621 Apr  3  2018 community-policy.rules
```

Okay, lets try and `cat` out  `community-nntp.rules` this time.
```Shell
ubuntu@tryhackme:~$ cat /etc/snort/rules/community-nntp.rules    
# Copyright 2005 Sourcefire, Inc. All Rights Reserved.
# These rules are licensed under the GNU General Public License.
# Please see the file LICENSE in this directory for more details.
# $Id: community-nntp.rules,v 1.3 2006/02/16 15:51:19 akirk Exp $

alert tcp $EXTERNAL_NET 119 -> $HOME_NET any (msg:"COMMUNITY NNTP Lynx overflow attempt"; flow:to_server,established; content:"Subject"; nocase; pcre:"/^Subject\x3a[^\r\n]{100,}/smi"; reference:cve,2005-3120; reference:bugtraq,15117; reference:url,www.osvdb.org/displayvuln.php?osvdb_id=20019; reference:nessus,20035; classtype:attempted-admin; sid:100000172; rev:2;)
```

We have a string of interest below, I will try and break this out to make some sense
```text
alert tcp $EXTERNAL_NET 119 -> $HOME_NET any (msg:"COMMUNITY NNTP Lynx overflow attempt"; flow:to_server,established; content:"Subject"; nocase; pcre:"/^Subject\x3a[^\r\n]{100,}/smi"; reference:cve,2005-3120; reference:bugtraq,15117; reference:url,www.osvdb.org/displayvuln.php?osvdb_id=20019; reference:nessus,20035; classtype:attempted-admin; sid:100000172; rev:2;)
```

The formatting isn't great, but it gives a bit more context.

| Description           | Item                     | Meta           | 
|-----------------------|--------------------------|----------------|
| **Action**:           | alert                    |                |
| **Protocol**:         | tcp                      |                |
| **Source IP**:        | $EXTERNAL_NET            |                |
| **Source Port**:      | 119                      |                |
| **Direction**:        | ->                       |                |
| **Destination IP**:   | $HOME_NET                |                |
| **Destination Port**: | any                      |                |
| **Rule Metadata**:    | **Message (msg)**:       | (msg:"COMMUNITY NNTP Lynx overflow attempt"; flow:to_server,established; content:"Subject"; nocase; pcre:"/^Subject\x3a[^\r\n]{100,}/smi"; reference:cve,2005-3120; reference:bugtraq,15117; reference:url,www.osvdb.org/displayvuln.php?osvdb_id=20019; reference:nessus,20035; classtype:attempted-admin; |
|                       | **Signature ID (sid)**:  | sid:100000172; |
|                       | **Rule Revision (Rev)**: | rev:2;)        |



- **Action** Specifices the action to take
- **Protocol** Protocl we're looking for, in this example `tcp`
- **Source IP** Where the IP is coming from
- **Source port** The specific source port 
- **Destination IP** The destination IP
- **Destination port** The destination port
- **Rule metadata** The real rule stuff
    - **Message (msg)** Describes the message displayed if triggered and what type of activity has occoured
    - **Signature ID (sid)** The unique identifier for this rule
    - **Rule revision (rev)** The revision of the rule


Okay, so that kind of makes sense, the message is I guess an experience thing. The next task is to create a sample rule.

Issue command `sudo vim /etc/snort/rules/local.rules`. 

The local rule definition already has 2 rules.

```text
alert icmp any any -> $HOME_NET any (msg:"Ping Detected"; sid:1000001; rev:1;)
alert tcp any any -> $HOME_NET 22 (msg:"SSH Connection Detected"; sid:1000002; rev:1;)
```

We're going to enter a new one as per below

```text
alert icmp any any -> 127.0.0.1 any (msg:"Loopback Ping Detected"; sid:10003; rev:1;)
```

Enter insert mode in vim by typing `i` and navigate to the last character of the last line and press return. Then **Shift**+**Ctrl**+**v** to paste.

The new line will have now appeared and you can press **Esc** to leave _insert_ mode and type `:x` to _Save and Exit_ vim.

We can `cat` the rule to ensure that actually, it has saved and is valid text.

```Shell
ubuntu@tryhackme:~$ cat /etc/snort/rules/local.rules 
# $Id: local.rules,v 1.11 2004/07/23 20:15:44 bmc Exp $
# ----------------
# LOCAL RULES
# ----------------
# This file intentionally does not come with signatures.  Put your local
# additions here.
alert icmp any any -> $HOME_NET any (msg:"Ping Detected"; sid:1000001; rev:1;)
alert tcp any any -> $HOME_NET 22 (msg:"SSH Connection Detected"; sid:1000002; rev:1;)
alert icmp any any -> 127.0.0.1 any (msg:"Loopback Ping Detected"; sid:10003; rev:1;)
```

Great, there is our rule at the end with _sid_ `10003`.

Next we want to test the rules. First start Snort by issuing the command `sudo snort -q -l /var/log/snort -i lo -A console -c /etc/snort/snort.conf`.

```Shell
ubuntu@tryhackme:~$ sudo snort -q -l /var/log/snort -i lo -A console -c /etc/snort/snort.conf 
▮
```

Next we can issue the `ping` command, I'm using `-A` to be adaptave, quicker, and -`c 1` to limit the count to 1 ping. 

```Shell
ubuntu@tryhackme:~$ ping -A -c 1 127.0.0.1
PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.014 ms

--- 127.0.0.1 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.014/0.014/0.014/0.000 ms
```

As we can see, `1 packets transmitted, 1 received` means it was sucessfull. Lets now take a look at the snort output.

```Shell
07/02-14:06:01.415068  [**] [1:10003:1] Loopback Ping Detected [**] [Priority: 0] {ICMP} 127.0.0.1 -> 127.0.0.1
07/02-14:06:01.415074  [**] [1:10003:1] Loopback Ping Detected [**] [Priority: 0] {ICMP} 127.0.0.1 -> 127.0.0.1
```

I'm wondering why there are 2 'hits'. I'm guessing it is the request and the response. That would work. I wonder if I can prove it. What if I was to test on a differnt loop back address. 

Let's try again this time targeting `127.0.0.127`.

```Shell
ubuntu@tryhackme:~$ ping -A -c 1 127.0.0.127
PING 127.0.0.127 (127.0.0.127) 56(84) bytes of data.
64 bytes from 127.0.0.127: icmp_seq=1 ttl=64 time=0.016 ms

--- 127.0.0.127 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.016/0.016/0.016/0.000 ms
```

Okay so that worked, 1 out 1 in, success. What does snort say?

```Shell
07/02-14:09:42.440841  [**] [1:10003:1] Loopback Ping Detected [**] [Priority: 0] {ICMP} 127.0.0.127 -> 127.0.0.1
```

Okay so that makes sense, the source was `127.0.0.1`, that is the address of adapter 'lo'. I pinged `127.0.0.127`.  However the alert triggered because of the return traffic to the source `127.0.0.1 `. 

_**Fun fact**_: You can ping any address from `127.0.0.1` though `127.255.255.254` to get a loop back response.

We can also run snort against a **PCAP** file for forensic analysis by issuing the command `sudo snort -q -l /var/log/snort -r Task.pcap -A console -c /etc/snort/snort.conf`

I did try this in the terminal but the **PCAP** file didn't exist and I wasn't too bothered about hunting for one.

```Shell
ubuntu@tryhackme:~$ sudo snort -q -l /var/log/snort -r Task.pcap -A console -c /etc/snort/snort.conf
Error getting stat on pcap file: Task.pcap: No such file or directory
ERROR: Error getting pcaps.
Fatal Error, Quitting..
```



### ❓ Question 1

> Where is the main directory of Snort that stores its files?

#### 🧪 Process

The files are saved within `/etc/snort`. such as `/etc/snort/rules`. 

Trying this as the answer

#### ✅ Answer

- `/etc/snort` ✅


### ❓ Question 2

> Which field in the Snort rule indicates the revision number of the rule?

#### 🧪 Process

The `rev` short for _revision_.

Trying this as the answer

#### ✅ Answer

- `rev` ✅


### ❓ Question 3

> Which protocol is defined in the sample rule created in the task?

#### 🧪 Process

We used ping, which issues `ICMP` packets/

Trying this as the answer

#### ✅ Answer

- `ICMP` ✅


### ❓ Question 4

> What is the file name that contains custom rules for Snort?

#### 🧪 Process

We updated the the `local.rules` file. Is this standard? I would have thought we could create any file then have it pull everything within the `/etc/snort/rules' directory.

Trying this as the answer

#### ✅ Answer

- `local.rules` ✅
