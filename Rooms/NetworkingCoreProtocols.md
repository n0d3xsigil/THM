## Contents
- [Introduction](#Introductiomteams)
- [DNS: Remembering Addresses](#DNS-Remembering-Address)
- [WHOIS](#WHOIS)
- [HTTP(S): Accessing the web](#http(S)-accessing-the-web)
- [FTP: Transferring Files](#tfp-transferring-files)
- [SMTP: Sending Email](#smtp-sending-email)
- [POP3: Receiving Email](#pop3-receiving-email)
- [IMAP: Synchronizing Email](#imap-synchronizing-email)
- [Conclusion](#conclusion)

## Introduction
## DNS: Remembering Address
## WHOIS
## HTTP(S): Accessing the Web
### Something new
I've not used telnet much to get web pages. This is actually quote cool, easy to pipe in to a PowerShell or Bash script and do something with it.


Telnet into the webserver IP.
```sh
telnet 10.20.30.40 80
```

Then get a known page
```sh
GET / HTTP/1.1
Host: Me
GET /index.html
```

output
```sh
HTTP/1.1 200 OK
Server: nginx/1.18.0 (Ubuntu)
Date: Fri, 06 Jun 2025 12:54:13 GMT
Content-Type: text/html
Content-Length: 2252
Last-Modified: Thu, 27 Jun 2024 06:58:01 GMT
Connection: keep-alive
ETag: "667d0d79-8cc"
Accept-Ranges: bytes

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>The Art of Espresso</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            line-height: 1.6;
            margin: 0;
            padding: 20px;
            max-width: 800px;
            margin: 0 auto;
        }
        h1, h2 {
            color: #4a2c2a;
        }
        img {
            max-width: 100%;
            height: auto;
        }
    </style>
</head>
<body>
    <h1>The Art of Espresso</h1>

    <h2>What is Espresso?</h2>
    <p>Espresso is a concentrated form of coffee served in small, strong shots. It is made by forcing pressurized hot water through finely-ground coffee beans. Espresso is the base for many coffee drinks, such as cappuccino, latte, and Americano.</p>

    <h2>How to Make Espresso</h2>
    <p>Making the perfect espresso requires practice and attention to detail. Here's a basic guide:</p>

    <ol>
        <li><strong>Choose your beans:</strong> Use freshly roasted, high-quality espresso beans.</li>
        <li><strong>Grind the beans:</strong> Grind them fine, almost to a powder consistency.</li>
        <li><strong>Measure and tamp:</strong> Use about 7-9 grams of ground coffee for a single shot. Tamp it down firmly and evenly.</li>
        <li><strong>Prepare your machine:</strong> Ensure your espresso machine is clean and heated up.</li>
        <li><strong>Pull the shot:</strong> Run hot water through the grounds. It should take about 25-30 seconds to extract 1-1.5 ounces of espresso.</li>
        <li><strong>Serve immediately:</strong> Espresso is best enjoyed right away while it's hot and the crema is intact.</li>
    </ol>

    <h2>The Perfect Espresso</h2>
    <p>A well-made espresso should have a rich, dark color with a golden-brown crema on top. It should have a full-bodied flavor with a balance of sweetness, acidity, and bitterness. The aroma should be strong and pleasing.</p>

    <p>Remember, making great espresso takes practice. Don't be discouraged if your first attempts aren't perfect. With time and experience, you'll be crafting delicious espresso shots at home!</p>
</body>
</html>
```
## FTP: Transferring Files
## SMTP: Sending Email
## POP3: Receiving Email
I didn't know it was possible to pull email over telnet. So this is quite interesting to me. I'll have to do more digging on this, can this be done with a secure connection? What about gmail, or outlook if pop is enabled.

```sh
┌──(root㉿kali)-[~]
└─# telnet 10.10.128.210 110
Trying 10.10.128.210...
Connected to 10.10.128.210.
Escape character is '^]'.
+OK [XCLIENT] Dovecot (Ubuntu) ready.
USER linda
+OK
PASS Pa$$123
+OK Logged in.
stat
+OK 4 2216
list
+OK 4 messages:
1 690
2 589
3 483
4 454
.
retr 1
+OK 690 octets
Return-path: <user@client.thm>
Envelope-to: linda@server.thm
Delivery-date: Thu, 12 Sep 2024 20:06:30 +0000
Received: from [10.11.81.126] (helo=client.thm)
        by example.thm with smtp (Exim 4.95)
        (envelope-from <user@client.thm>)
        id 1soq4J-0007lF-2G
        for linda@server.thm;
        Thu, 12 Sep 2024 20:06:30 +0000
From: user@client.thm
To: linda@server.thm
Subject: Cyber Security

Cybersecurity refers to the practices, technologies, and processes designed to protect systems, networks, and data from cyberattacks, damage, or unauthorized access. It is essential for safeguarding sensitive information across various sectors, including personal, corporate, and governmental domains.
.
```

The commands that are taught in this room are:
- `USER` Username
- `PASS` password
- `STAT` displays the number and size of messages in the inbox
- `LIST` Lists the emails and size of each email
- `RETR` Retrieves the numbered message
- `DELE` marks the message for deletion. (Applied on next step)
- `QUIT` Ends session and processes actions such as deletions.

## IMAP: Synchronizing Email
## Conclusion
