| [Home](../README.md) | [SOC Level 1](../SOClevel1.md) | **Summit** |

# Summit

## Contents
- [Challenge](#challenge)



## 📘Challenge

-**Context**: PicoSecure has faced multiple incident response situations.
- **Action Taken**: They are now conducting a **threat simulation and detection engineering engagement**.
- **Your Role**
  - Collaborate with an **external penetration tester**.
  - Participate in an **iterative purple-team scenario**.
- **Penetration Tester’s Task**: Attempt to execute **malware samples** on a simulated internal user workstation.
- **Your Task**
  - Configure PicoSecure’s **security tools**.
  - Detect and **prevent malware execution**.
- **Goal**
  - Follow the **Pyramid of Pain** framework.
  - Increase the **cost of operations** for simulated adversaries.
  - Ultimately, **deter attackers** by making their efforts ineffective.
- **Approach**: Use different levels of the pyramid to detect and block various **indicators of attack**.

### Pyramid of Pain
<img width="600" height="439" alt="image" src="https://github.com/user-attachments/assets/a5a68060-7a7e-4ba2-be59-ff300ed6b03c" />


### ❓ Question 1

> What is the first flag you receive after successfully detecting **sample1.exe**?

#### 🧪 Process

We'll start with the **`Trivial`** phase

Ensure the machine has started and the click the link `https://1.2.3.4.p.thmlabs.com`

Read the email. 

When ready click **`sample1.exe`** to _`Scan with Malware Sandbox`_.

<img width="1511" height="1060" alt="image" src="https://github.com/user-attachments/assets/7dd61f18-4288-4bf8-b9bd-d4d5153321c2" />

Click **Submit for Analysis**
<img width="1511" height="1060" alt="image" src="https://github.com/user-attachments/assets/000a0d85-1414-41a5-8a90-12fcd8c825e3" />

There's limited information here, however we're interested in `Hashes`. Let's grab one and make a note of it.

I'm liking **_SHA265_** of `9c550591a25c6228cb7d74d970d133d75c961ffed2ef7180144859cc09efca8c`.
<img width="1511" height="1060" alt="image" src="https://github.com/user-attachments/assets/c547071f-225c-425d-a79d-c73a7b26dc23" />

Remebering that our objective is to configure the security tools, lets look to see what we can do.

> _configure PicoSecure's security tools to detect and prevent the malware from executing_

If we click the menu on the top left, we can see an item relating to hashes. Click **Manage Hashes**
<img width="1511" height="1060" alt="image" src="https://github.com/user-attachments/assets/1c59637d-d013-4b65-b51d-e707d2f2620c" />

This looks promising, click **SHA256** and type in the **Hash Value** of `9c550591a25c6228cb7d74d970d133d75c961ffed2ef7180144859cc09efca8c`. Be careful not to make any mistakes otherwise it won't work ;). Once **_pasted_**, click **Submit Hash**.
<img width="1511" height="1060" alt="image" src="https://github.com/user-attachments/assets/a2195f3c-8882-4460-89a2-ea32b2566c9c" />

Okay, great, we get confirmation `sample1.exe` is being prevented, we can see our new reule in the list and we have a new email. 
<img width="1511" height="1060" alt="image" src="https://github.com/user-attachments/assets/c30cae29-9302-48cf-a413-0d50b163c77d" />

Let's head back to our inbox, click the **Menu** followed by **Mail (1)**
<img width="1511" height="1060" alt="image" src="https://github.com/user-attachments/assets/05764f97-c821-4ef3-8b30-d445736b9f14" />

And here, we have our first flag.
<img width="1511" height="1060" alt="image" src="https://github.com/user-attachments/assets/0e355ad8-896f-4a5d-a533-b37e6efeaac4" />

#### ✅ Answer

- `THM{f3cbf08151a11a6a331db9c6cf5f4fe4}` ✅


### ❓ Question 2

> What is the second flag you receive after successfully detecting **sample2.exe**?

#### 🧪 Process

The next phase is the **Easy** phase, Let's take a look at what options we have that could match this. Click the **Menu**

In this this menu we have the following options
- **Mail**
- **Malware Sandbox**
- **Manage Hashes**
- **Firewall Manager**
- **DNS Filter**
- **Sigma Rule Builder**

I am liking the look of **Firewall Manager**, we'll come back to this shortly. 
<img width="1511" height="1060" alt="image" src="https://github.com/user-attachments/assets/545f94bf-4600-4533-a2bd-8d0d87d8fdbf" />

First however, we need to _scan_ `sample2.exe` with our sandbox. Click **`sample2.exe`**
<img width="1511" height="1060" alt="image" src="https://github.com/user-attachments/assets/385eecd1-fd94-455b-bc60-1c61a73e06c2" />

Click the **_file dropdown** and select **`sample2.exe`** followed by **Submit for Analysis**
<img width="1511" height="1060" alt="image" src="https://github.com/user-attachments/assets/3d8994dc-f159-456d-9858-36054a1cd26c" />

Reviewing the information, this time with our _IP_ head on. We have a couple of IP addresses:
- `154.35.10.113` (Intrabuzz Hosting Limited)
- `40.97.128.3` (Microsoft Corporation)
- `40.97.128.4` (Microsoft Corporation)

In this instance, I like the first IP, looking at the `GET` request against that IP looks suspect (`http://154.35.10.113:4444/uvLk8YI32`).

So let's take note of this IP (`154.35.10.113`).
<img width="1511" height="1539" alt="image" src="https://github.com/user-attachments/assets/60a3d674-d3f3-4e43-a0a9-30118e17d1ae" />

Click the **Menu** followed by **Firewall Manager**
<img width="1511" height="1060" alt="image" src="https://github.com/user-attachments/assets/a6b4aed3-0f25-46a7-bf08-7a8638132c0d" />

Next click **Create Firewall Rule**
<img width="1511" height="641" alt="image" src="https://github.com/user-attachments/assets/318b912e-ea92-4706-9d84-91aab1b1a623" />

Set the options as follows
- **Type**: `Egress` - To stop the access to the attacker
- **Source IP**: `Any` - All IP's within the Firewalled network
- **Destination IP**: `154.35.10.113` - The target in questions
- **Action**: `Deny`- Prevent the connection occouring

Then click **Save Rule**
<img width="1511" height="1060" alt="image" src="https://github.com/user-attachments/assets/73ff0371-ad18-4adc-94ef-d132999d8dfe" />

Great, that is what we want. Now click the **Menu** followed by **Mail (1)**

Again, we have our flag.
<img width="1511" height="1060" alt="image" src="https://github.com/user-attachments/assets/3071d15f-c5f2-4374-80cb-734bc3f4c11c" />

#### ✅ Answer

- `THM{2ff48a3421a938b388418be273f4806d}` ✅


### ❓ Question 3

> What is the third flag you receive after successfully detecting **sample3.exe**?

#### 🧪 Process

Our next phase is the **Simple** phase.

Let's get started by clicking **`sample3.exe`**
<img width="1511" height="1060" alt="image" src="https://github.com/user-attachments/assets/695c53e1-1b44-4e37-b7d7-e53814c48579" />

Again, click the **File** dropdown, but this time select **`sample3.exe`** and click **Submit for Analysis**
<img width="1511" height="462" alt="image" src="https://github.com/user-attachments/assets/fb783597-0a3d-4beb-ad59-3dd322dacdac" />

This time, we are looking for domain names. If we review the bottom of the file for _DNS requests_ we can see the following DNS requests and their response.
- **`services.microsoft.com`** → `40.97.128.4`
- **`emudyn.bresonicz.info`** → `62.123.140.9`

I would like to think that _`services.microsoft.com'_ is fairly safe. That leaves our suspect domain **`emudyn.bresonicz.info`**.
<img width="1511" height="1796" alt="image" src="https://github.com/user-attachments/assets/79e9afc7-fcb6-4d10-b8de-0d65d4817549" />

Let's copy the domain `emudyn.bresonicz.info`, then we can click the **Menu** followed by 
<img width="1511" height="445" alt="image" src="https://github.com/user-attachments/assets/c1b7102a-8fb9-42f9-97ff-806817e2083b" />

Click **Create DNS Rule**
<img width="1511" height="747" alt="image" src="https://github.com/user-attachments/assets/7edea81f-71a0-461f-a61a-99ab327ad824" />

Set the options as follows
- **Rule Name**: `Something meaningful` - To identify the rule
- **Category**: `Malware` - The reason for the rule
- **Domain Name**: `emudyn.bresonicz.info` - The target in questions
- **Action**: `Deny`- Prevent the connection occouring

Then click **Save Rule**
<img width="1511" height="746" alt="image" src="https://github.com/user-attachments/assets/e0db9894-7698-477c-9964-7ddd895bbc5e" />

Result!
<img width="1511" height="883" alt="image" src="https://github.com/user-attachments/assets/0dc03d5c-2666-403c-a233-bfa010d76fa9" />
Great, that is what we want. Now click the **Menu** followed by **Mail (1)**

Once again, we have our flag
<img width="1511" height="1060" alt="image" src="https://github.com/user-attachments/assets/e6832b71-acdc-46f5-9866-eaef2229b5bd" />

#### ✅ Answer

- `THM{4eca9e2f61a19ecd5df34c788e7dce16}` ✅


### ❓ Question 4

> What is the fourth flag you receive after successfully detecting **sample4.exe**?

#### 🧪 Process

Now on to the **Annoying** phase

Kick off the process by clicking **`sample4.exe`**
<img width="1511" height="1060" alt="image" src="https://github.com/user-attachments/assets/8e9b7431-f938-45e9-88b0-eda2ceee89e6" />

Again, click the **File** dropdown, but this time select **`sample4.exe`** and click **Submit for Analysis**
<img width="1511" height="445" alt="image" src="https://github.com/user-attachments/assets/e27417a4-1d4b-4762-9eb2-f2991d399bc8" />

In this instance I thnk we can take a look at something happening at the host level. I think first, I am going to look at blocking `backdoor.exe` from running.
<img width="1619" height="1451" alt="image" src="https://github.com/user-attachments/assets/09a111f6-c3cf-449c-999e-2175b9a593aa" />

Let's copy `backdoor.exe` and then click **Menu** followed by **Sigma Rule Builder**
<img width="1619" height="579" alt="image" src="https://github.com/user-attachments/assets/166c5e0d-316a-415f-b356-47009ff62000" />

Click **Create Sigma Rule**
<img width="1619" height="416" alt="image" src="https://github.com/user-attachments/assets/0830dbef-8b95-4638-bd87-b5948bd80665" />

Now we are presented with a list of options:
- **Sysmon Event Logs**
- **Web Server Logs**
- **VPN Logs**
- **Application Logs**

Since I am on the `*.exe` route, I can see that "Sysmon Event Logs" contains **`process creations`**. So I'm going with that, click **Sysmon Event Logs**
<img width="1619" height="857" alt="image" src="https://github.com/user-attachments/assets/ef826acd-3074-4967-9265-7d083620c761" />

Great, now we have more information. Click **Process Creation**
<img width="1619" height="1003" alt="image" src="https://github.com/user-attachments/assets/d9b09fb1-8db8-4679-af88-168f3e776824" />

Okay, so I'm not sure this is the right option, there is a _mandatory_ string required, which we don't have. Lets look back at the other options...

Click the **Menu** followed by **Mail**
<img width="1619" height="297" alt="image" src="https://github.com/user-attachments/assets/419be806-df7b-43f3-abc2-cf231179d23e" />

Click **`sample4.exe`** again
<img width="1511" height="1060" alt="image" src="https://github.com/user-attachments/assets/17e6ff3b-30e3-47d5-9cab-2f6016328e47" />

Previously I noticed that there where some registry activity at the bottom (scroll to the bottom).
<img width="1619" height="931" alt="image" src="https://github.com/user-attachments/assets/7dbc89f1-0d44-4a7b-8e33-ad5155056bfc" />

We can see that actually, `sample4.exe` has **_modified_** the registry. Let's run with this.
<img width="1619" height="931" alt="image" src="https://github.com/user-attachments/assets/ad82b0bd-e51b-45e9-9301-4042bf58db1d" />

Once again click the **Menu** followed by **Sigma Rule Builder**
<img width="1619" height="456" alt="image" src="https://github.com/user-attachments/assets/f779f593-8e85-48f6-b50e-3717033e37f3" />

Click **Create Sigma Rule**
<img width="1619" height="416" alt="image" src="https://github.com/user-attachments/assets/3641de4f-2ad0-4027-9b19-0a01daa010d1" />

Again, click **Sysmon Event Logs**
<img width="1619" height="446" alt="image" src="https://github.com/user-attachments/assets/5a628310-2a13-4f22-866b-af6122c7154b" />

Click **Registry Modifications** at the bottom
<img width="1619" height="1390" alt="image" src="https://github.com/user-attachments/assets/7615c3ca-427c-459e-ab26-9a278c89eeb0" />

Set the options as follows
- **Registry Key**: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Defender\Real-Time Protection`
- **Registry Name**: `DisableRealtimeMonitoring`
- **Value**: `1`
- **ATT&CK ID**: `Defense Evasion (TA0005)`

Once populated, click **Validate Rule**
<img width="1619" height="567" alt="image" src="https://github.com/user-attachments/assets/6f55e81a-3e6b-4e16-a505-4a8b4b39918f" />

Next Click the **Menu** followed by **Mail (1)**
<img width="1607" height="353" alt="image" src="https://github.com/user-attachments/assets/bcbbe127-1b70-4b30-b6ca-3ad2ac712d55" />

Success, here we are once again with our flag.
<img width="1619" height="1059" alt="image" src="https://github.com/user-attachments/assets/617adf13-35f8-4ded-9ef8-2fcba058e4df" />

#### ✅ Answer

- `THM{c956f455fc076aea829799c0876ee399}` ✅


### ❓ Question 5

> What is the fifth flag you receive after successfully detecting **sample5.exe**?

#### 🧪 Process

We're onto the **Challenging** phase.

Click the **`outgoing_connections.log`**
<img width="1619" height="1059" alt="image" src="https://github.com/user-attachments/assets/001695f7-dd5d-4b8f-96df-77e2a0f6056d" />

We a list of connections, we have a common source `10.10.15.12` and a variaty of destnations. It looks like a port scan, many of the results are `97` bytes, whilst theoher svary from `805` - `95021 bytes`
<img width="1619" height="1015" alt="image" src="https://github.com/user-attachments/assets/6fbff773-7bcb-4fe2-9e01-b872619865e0" />

The timing is interesting, for the results with 97 bytes there is a 30 minute delay. 

**_example_**: 

  ```text
  2023-08-15 18:00:00 | Source: 10.10.15.12 | Destination: 51.102.10.19 | Port: 443 | Size: 97 bytes
  2023-08-15 18:30:00 | Source: 10.10.15.12 | Destination: 51.102.10.19 | Port: 443 | Size: 97 bytes
  2023-08-15 19:00:00 | Source: 10.10.15.12 | Destination: 51.102.10.19 | Port: 443 | Size: 97 bytes
  2023-08-15 19:30:00 | Source: 10.10.15.12 | Destination: 51.102.10.19 | Port: 443 | Size: 97 bytes
  2023-08-15 20:00:00 | Source: 10.10.15.12 | Destination: 51.102.10.19 | Port: 443 | Size: 97 bytes
  2023-08-15 20:30:00 | Source: 10.10.15.12 | Destination: 51.102.10.19 | Port: 443 | Size: 97 bytes
  2023-08-15 21:00:00 | Source: 10.10.15.12 | Destination: 51.102.10.19 | Port: 443 | Size: 97 bytes
  ```
Oh and also, just the two ports `80`, and `443`.

So let's click the **Menu** and click **Sigma Rule Builder**
<img width="1619" height="1015" alt="image" src="https://github.com/user-attachments/assets/3f29925c-ea07-4515-9ed8-b1a9b5937ad1" />

Click **Create Sigma Rule**
<img width="1619" height="416" alt="image" src="https://github.com/user-attachments/assets/3641de4f-2ad0-4027-9b19-0a01daa010d1" />

After some searching around, it looks like we can use **Sysmon Event Logs** again, since we want _network connections_
<img width="1619" height="446" alt="image" src="https://github.com/user-attachments/assets/5a628310-2a13-4f22-866b-af6122c7154b" />

Next click **Network Connections**
<img width="1619" height="1423" alt="image" src="https://github.com/user-attachments/assets/d0aeb290-b97b-48a8-a749-f7a38d0ecca4" />

Set the options as follows
- **Remote IP**: `Any`
- **Remote Port**: `Any`
- **Size** (bytes): `97`
- **Frequency** (seconds): `1800` (60 seconds * 30 minutes)
- **ATT&CK ID**: `Command and Control (TA0011)`

Then click **Validate Rule**
<img width="1619" height="815" alt="image" src="https://github.com/user-attachments/assets/419f1be3-956b-4188-856d-f6a1e900e38f" />

Click **Menu** followed by **Mail (1)**
<img width="1619" height="491" alt="image" src="https://github.com/user-attachments/assets/35d66361-ae09-4202-ade1-f1d51f3c909f" />

We have our penultimate flag
<img width="1619" height="845" alt="image" src="https://github.com/user-attachments/assets/3f4b6763-5895-4930-b921-91624d80ac14" />

#### ✅ Answer

- `THM{46b21c4410e47dc5729ceadef0fc722e}` ✅


### ❓ Question 6

> What is the final flag you receive from Sphinx?

#### 🧪 Process

We now enter our final **Tough** phase.

Click **`commands.log`**
<img width="1619" height="845" alt="image" src="https://github.com/user-attachments/assets/6d282d2c-0955-475f-94d8-e37ca3cb5b9c" />

```BAT
dir c:\ >> %temp%\exfiltr8.log
dir "c:\Documents and Settings" >> %temp%\exfiltr8.log
dir "c:\Program Files\" >> %temp%\exfiltr8.log
dir d:\ >> %temp%\exfiltr8.log
net localgroup administrator >> %temp%\exfiltr8.log
ver >> %temp%\exfiltr8.log
systeminfo >> %temp%\exfiltr8.log
ipconfig /all >> %temp%\exfiltr8.log
netstat -ano >> %temp%\exfiltr8.log
net start >> %temp%\exfiltr8.log
```

Let's break this down, we have `cmd` >> `file`. This will run the command, then append the output to the file on the right.

I'm thinking we can look out the file `%temp%\exfiltr8.log`

Click the **Menu** followed by **Sigma Rule Builder**
<img width="1619" height="483" alt="image" src="https://github.com/user-attachments/assets/f1ba59ae-5faa-41c8-a0ce-a98e7100cb87" />

Click **Create Sigma Rule**
<img width="1619" height="416" alt="image" src="https://github.com/user-attachments/assets/3641de4f-2ad0-4027-9b19-0a01daa010d1" />

Once again, click **Sysmon Event Logs**, this contains _file creation_
<img width="1619" height="446" alt="image" src="https://github.com/user-attachments/assets/5a628310-2a13-4f22-866b-af6122c7154b" />

Click **File Creation and Modification**
<img width="1619" height="1130" alt="image" src="https://github.com/user-attachments/assets/d3e924fb-0874-48a7-b5e5-7588bc92f366" />

Set the options as follows
- **File Path**: `%temp%`
- **File Name**: `exfiltr8.log`
- **ATT&CK ID**: `Collection (TA0009)`

Then click **Validate Rule**
<img width="1619" height="766" alt="image" src="https://github.com/user-attachments/assets/95a65cc8-77c3-451d-aca1-7567d6bae850" />

Great that worked
<img width="1619" height="515" alt="image" src="https://github.com/user-attachments/assets/a54c90e0-7bce-4c9e-aeac-2919d6a26fa5" />

Click the **Menu** followed by **Mail (1)**
<img width="1619" height="650" alt="image" src="https://github.com/user-attachments/assets/f6ae6964-8c98-4fa3-b052-0bb50b9cd869" />

And there we have it, our final flag
<img width="1476" height="636" alt="image" src="https://github.com/user-attachments/assets/481d8ae7-cd88-4af6-9239-811848de24d4" />

#### ✅ Answer

- `THM{c8951b2ad24bbcbac60c16cf2c83d92c}` ✅
