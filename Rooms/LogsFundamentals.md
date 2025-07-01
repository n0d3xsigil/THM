| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **Logs Fundamentals** |

# Logs Fundamentals

## Contents
- [Introduction to Logs](#introduction-to-logs)
- [Types of Logs](#types-of-logs)
- [Windows Event Logs Analysis](#windows-event-logs-analysis)
- [Web Server Access Logs Analysis](#web-server-access-logs-analysis)
- [Conclusion](#conclusion)



## 📘Introduction to Logs

Logging is a key part of discovering attacks, but many attackers will cover their tracks on the target system. Often attackers will try and conceil tracks. However systems often have many places logs are saved.

Lets take a look at some use cases of logs.

|               Use Case               |                   Description                   |
|--------------------------------------|-------------------------------------------------|
| Security Events Monitoring           | Logs help detect anomolies                      |
| Incident Investigation and Forensics | Captures traces and detailed information        |
| Troubleshooting                      | Captures errors which can aid with resolutions  |
| Performance Monitoring               | Insights into performance                       |
| Audting and Compliance               | audit trails                                    |



### ❓ Question

> Where can we find the majority of attack traces in a digital system?

#### 🧪 Process

Since the scope of this room is logging, I guess we can say `Logs`

Trying this as the answer

#### ✅ Answer

- `Logs` ✅



## 📘Types of Logs

Each system will have an overwhelmning number of loggs of various categories. Thankfully the Windows Operating System has a feature called the Eventlog, or Event Viewer. This tool helps categorise logs of different sources into a log type. Let's take a look at the categories below

- **System Logs** - Usefull in troubleshooting OS level issues.
    - System startups
    - System shutdowns
    - Driver events
    - System errors
    - Hardware events

- **Security Logs** - Events around the security of the system, helps detect and investigate
    - Authentication events
    - Authorisation events
    - Security policy changes
    - User account changes
    - Abnormal activity

- **Application Logs** - Events specifically around application events. Interactive or non interactive
    - User interaction events
    - Application changes events
    - Application update events
    - Application error events

- **Audit Logs** - Detailed information about system and user changes. Often not enabled.
    - Data Access events
    - System Change events
    - User Activity events
    - Policy Enforcement events

- **Network Logs** - Information around inbound and outbound traffic. key troubleshooting and investigation events
    - Incoming network traffic events
    - Outgoing network traffic events
    - Network connection logs
    - Network firewall logs


- **Access Logs** - Information about the locations being accessed on and off-prem
    - Webserver access logs
    - Database acccess logs
    - Application access logs
    - API access logs


### ❓ Question 1

> Which type of logs contain information regarding the incoming and outgoing traffic in the network?

#### 🧪 Process

If it is relating to the network, it is probably `Network logs`

Trying this as the answer

#### ✅ Answer

- `Network logs` ✅


### ❓ Question

> Which type of logs contain the authentication and authorization events?

#### 🧪 Process

Authorisation and authorisation events fall under the `Security logs` category.

Trying this as the answer

#### ✅ Answer

- `Security logs` ✅




## 📘Windows Event Logs Analysis

I am quite familiar windows event logs so I won't go in to too much detail with this section. 

Use **Event ID**'s

| Event ID |                    Description                     |
|----------|----------------------------------------------------|
| 4624     | A user account successfully logged in              |
| 4625     | A user account failed to login                     |
| 4634     | A user account successfully logged off             |
| 4720     | A user account was created                         |
| 4724     | An attempt was made to reset an account’s password |
| 4722     | A user account was enabled                         |
| 4725     | A user account was disabled                        |
| 4726     | A user account was deleted                         |

### ❓ Question 1

> What is the name of the last user account created on this system?

#### 🧪 Process

01. Click **Start Machine**
02. In the machine, click **Start** and type `eventvwr.msc`
03. Expand **Windows Logs**
04. Click **Security**
05. Once within the _Security_ event logs click **Filter Current Log...**
06. Paste `4720` in _`<All Event IDs>`_ and click **OK**
07. The most recent account creation was on the 6th July 2024 at 12:56:27.
08. **Account Name:** `hacked`

Trying this as the answer

#### ✅ Answer

- `hacked` ✅


### ❓ Question 2

> Which user account created the above account?

#### 🧪 Process

09. We can see the account making the change was `Administrator`

Trying this as the answer

#### ✅ Answer

- `Administrator` ✅


### ❓ Question 3

> On what date was this user account enabled? Format: M/D/YYYY

#### 🧪 Process

10. Okay, so I assumed it was D/M/YYYY so its 7th June 2024

Trying this as the answer

#### ✅ Answer

- `6/7/2024` ✅


### ❓ Question 4

> Did this account undergo a password reset as well? Format: Yes/No

#### 🧪 Process

11. Click **Filter Current Log...**
12. Paste `4724` in _`<All Event IDs>`_ and click **OK**
13. 2 entried suggest and attempt was made

Trying this as the answer

#### ✅ Answer

- `Yes` ✅




## 📘Web Server Access Logs Analysis

When it comes to websites may interactions are logged such as time date, status, agent and pages. Most webpages are hosted on Linux. Thankfully, linux has some tools we can use to parse the logs

we have `cat`, `grep`, and `less` to start. We already know how to use these so let's get on with the questions :)


### ❓ Question 1

> What is the IP which made the last GET request to URL: “/contact”?

#### 🧪 Process

Oh crap, I'm on Windows, let's use PowerShell instead... 

 01. Click **Download Task Files**
 02. On the keyboard perform `Win+X` followed by `i` to launch the terminal
 03. type `cd .\Downloads\` and press **return** on the keyboard

```PowerShell
PS C:\Users\UserID> cd .\Downloads\
PS C:\Users\UserID\Downloads>
```

04. Type `Get-ChildItem access*.zip` and press **return** on the keyboard

```PowerShell
PS C:\Users\UserID\Downloads> Get-ChildItem access*.zip

    Directory: C:\Users\UserID\Downloads

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---          01/07/2025    15:57           1777 access-1718351133518.log.zip
```

05. Type the command `Expand-Archive .\access-1718351133518.log.zip` and press **return** on the keyboard

```PowerShell
PS C:\Users\UserID\Downloads> Expand-Archive .\access-1718351133518.log.zip
```

06. Type the command `Get-ChildItem access*` and press **return** on the keyboard

```PowerShell
PS C:\Users\UserID\Downloads> Get-ChildItem access

    Directory: C:\Users\UserID\Downloads

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d----          01/07/2025    16:05                access-1718351133518.log
-a---          01/07/2025    15:57           1777 access-1718351133518.log.zip
```

07. Type the command `Set-Location .\access-1718351133518.log\` and press **return** on the keyboard

```PowerShell
PS C:\Users\UserID\Downloads> Set-Location .\access-1718351133518.log\
PS C:\Users\UserID\Downloads\access-1718351133518.log>
```

In linux we can use the command `cat` to get the content of a file, we have this as an alias in PowerShell, however the command is actually `Get-Content`

```PowerShell
PS C:\Users\UserID\Downloads\access-1718351133518.log> Get-Alias cat

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Alias           cat -> Get-Content
```

08. Type the command `Get-Content .\access.log` and press **return** on the keyboard

```PowerShell
PS C:\Users\UserID\Downloads\access-1718351133518.log> Get-Content .\access.log
172.16.0.1 - - [06/Jun/2024:13:58:44] "GET /products HTTP/1.1" 404 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
10.0.0.1 - - [06/Jun/2024:13:57:44] "GET / HTTP/1.1" 404 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
192.168.1.1 - - [06/Jun/2024:13:56:44] "GET /about HTTP/1.1" 500 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
10.0.0.1 - - [06/Jun/2024:13:56:44] "PUT / HTTP/1.1" 500 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
172.16.0.1 - - [06/Jun/2024:13:55:44] "POST /contact HTTP/1.1" 500 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
10.0.0.1 - - [06/Jun/2024:13:54:44] "GET /contact HTTP/1.1" 500 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
192.168.1.1 - - [06/Jun/2024:13:53:44] "GET /products HTTP/1.1" 404 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
172.16.0.1 - - [06/Jun/2024:13:53:44] "GET /contact HTTP/1.1" 500 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
172.16.0.1 - - [06/Jun/2024:13:52:44] "GET /products HTTP/1.1" 404 "-" "Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
10.0.0.1 - - [06/Jun/2024:13:48:44] "GET /about HTTP/1.1" 404 "-" "Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
192.168.1.1 - - [06/Jun/2024:13:46:44] "GET /about HTTP/1.1" 200 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
[Truncated]
```

This isn't very helpful, in Linux we would 'pipe' the content into grep. We don't have an alias for grep, but we do have `Select-String`. So let's do this to hopfully expose our first answer.

09. Type the command `Get-Content .\access.log | Select-String "/contact"` and press **return** on the keyboard

```PowerShell
PS C:\Users\UserID\Downloads\access-1718351133518.log> Get-Content .\access.log | Select-String "/contact"

172.16.0.1 - - [06/Jun/2024:13:55:44] "POST /contact HTTP/1.1" 500 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
10.0.0.1 - - [06/Jun/2024:13:54:44] "GET /contact HTTP/1.1" 500 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
172.16.0.1 - - [06/Jun/2024:13:53:44] "GET /contact HTTP/1.1" 500 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
10.0.0.1 - - [05/Jun/2024:18:58:44] "PUT /contact HTTP/1.1" 500 "-" "Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
172.16.0.1 - - [05/Jun/2024:18:55:44] "POST /contact HTTP/1.1" 200 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
192.168.1.1 - - [05/Jun/2024:18:53:44] "POST /contact HTTP/1.1" 200 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
192.168.1.1 - - [05/Jun/2024:18:52:44] "POST /contact HTTP/1.1" 500 "-" "Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
10.0.0.1 - - [05/Jun/2024:18:50:44] "PUT /contact HTTP/1.1" 404 "-" "Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
192.168.1.1 - - [05/Jun/2024:16:51:44] "GET /contact HTTP/1.1" 200 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
10.0.0.1 - - [05/Jun/2024:16:50:44] "GET /contact HTTP/1.1" 500 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
10.0.0.1 - - [05/Jun/2024:15:57:44] "PUT /contact HTTP/1.1" 404 "-" "Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
172.16.0.1 - - [05/Jun/2024:15:52:44] "POST /contact HTTP/1.1" 500 "-" "Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
10.0.0.1 - - [05/Jun/2024:15:33:44] "GET /contact HTTP/1.1" 200 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
172.16.0.1 - - [05/Jun/2024:13:56:44] "PUT /contact HTTP/1.1" 404 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
10.0.0.1 - - [04/Jun/2024:13:58:44] "PUT /contact HTTP/1.1" 404 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
10.0.0.1 - - [04/Jun/2024:13:57:44] "POST /contact HTTP/1.1" 500 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
10.0.0.1 - - [04/Jun/2024:13:56:44] "GET /contact HTTP/1.1" 404 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
10.0.0.1 - - [04/Jun/2024:13:48:44] "GET /contact HTTP/1.1" 200 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
192.168.1.1 - - [04/Jun/2024:13:47:44] "POST /contact HTTP/1.1" 500 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
10.0.0.1 - - [04/Jun/2024:12:54:44] "GET /contact HTTP/1.1" 500 "-" "Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
172.16.0.1 - - [04/Jun/2024:12:52:44] "PUT /contact HTTP/1.1" 404 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
172.16.0.1 - - [04/Jun/2024:11:55:44] "POST /contact HTTP/1.1" 404 "-" "Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
192.168.1.1 - - [04/Jun/2024:11:53:44] "GET /contact HTTP/1.1" 500 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
172.16.0.1 - - [04/Jun/2024:11:52:44] "POST /contact HTTP/1.1" 200 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
192.168.1.1 - - [04/Jun/2024:10:58:44] "PUT /contact HTTP/1.1" 500 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
```

It looks like the dates are in descending order, as in the first 'GET' entry is the most recent. We can see the IP address is `10.0.0.1`.

Trying this as the answer

#### ✅ Answer

- `10.0.0.1` ✅


### ❓ Question 2

> When was the last POST request made by IP: “172.16.0.1”? 

#### 🧪 Process

This time I am going to try to filter on `POST` and then the IP `172.16.0.1` by nesting the `Where-Object` and `Select-String` cmdlets.

10. Type the command ` Get-Content .\access.log | Where-Object { $_ | Select-String "POST" } | Where-Object { $_ | Select-String "172.16.0.1" }` and press **return** on the keyboard

```PowerShell
PS C:\Users\UserID\Downloads\access-1718351133518.log> Get-Content .\access.log | Where-Object { $_ | Select-String "POST" } | Where-Object { $_ | Select-String "172.16.0.1" }
172.16.0.1 - - [06/Jun/2024:13:55:44] "POST /contact HTTP/1.1" 500 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
172.16.0.1 - - [06/Jun/2024:13:44:44] "POST / HTTP/1.1" 404 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
172.16.0.1 - - [06/Jun/2024:13:38:44] "POST /about HTTP/1.1" 200 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
172.16.0.1 - - [05/Jun/2024:18:55:44] "POST /contact HTTP/1.1" 200 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
172.16.0.1 - - [05/Jun/2024:16:45:44] "POST /about HTTP/1.1" 500 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
172.16.0.1 - - [05/Jun/2024:15:52:44] "POST /contact HTTP/1.1" 500 "-" "Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
172.16.0.1 - - [04/Jun/2024:11:55:44] "POST /contact HTTP/1.1" 404 "-" "Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
172.16.0.1 - - [04/Jun/2024:11:52:44] "POST /contact HTTP/1.1" 200 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
172.16.0.1 - - [04/Jun/2024:10:56:44] "POST /products HTTP/1.1" 500 "-" "Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
172.16.0.1 - - [04/Jun/2024:10:51:44] "POST /about HTTP/1.1" 500 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
```

Again, in descending order. We have a date time of `06/Jun/2024:13:55:44`

Trying this as the answer

#### ✅ Answer

- `06/Jun/2024:13:55:44` ✅


### ❓ Question 3

> Based on the answer from question number 2, to which URL was the POST request made?

#### 🧪 Process

We can pipe the previous command to `Select-String` and suffix the command with `-First 1` to reduce the results to the first line

11. Type the command `Get-Content .\access.log | Where-Object { $_ | Select-String "POST" } | Where-Object { $_ | Select-String "172.16.0.1" } | Select-Object -First 1` and press **return** on the keyboard

```PowerShell
PS C:\Users\UserID\Downloads\access-1718351133518.log> Get-Content .\access.log | Where-Object { $_ | Select-String "POST" } | Where-Object { $_ | Select-String "172.16.0.1" } | Select-Object -First 1
172.16.0.1 - - [06/Jun/2024:13:55:44] "POST /contact HTTP/1.1" 500 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
```

We can see the request was posted to `/contact`.

Trying this as the answer

#### ✅ Answer

- `/contact` ✅





## 📘Conclusion

Nothing to see here.
