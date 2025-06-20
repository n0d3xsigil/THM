| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **Gobuster: The Basics** |

## Contents
- [Introduction](#introduction)
- [Environment and Setup](#environment-and-setup)
- [Gobuster: Introduction](#gobuster-introduction)
- [Use Case: Directory and File Enumeration](#use-case-directory-and-file-enumeration)
- [Use Case: Subdomain Enumeration](#use-case-subdomain-enumeration)
- [Use Case: Vhost Enumeration](#use-case-vhost-enumeration)
- [Conclusion](#conclusion)


## Introduction
GoBuster, a tool to resolve directories, sub domains and virutal hosts/

No question for this section.


## Environment and Setup
add target as nameserver for dns.
restart dns service

No question for task


## Gobuster: Introduction
Oh Gobuster is written in Golang

> It enumerates web directories, DNS subdomains, vhosts, Amazon S3 buckets, and Google Cloud Storage by brute force

> **Enumeration** is the act of listing all the available resources, whether they are accessible or not. For example, Gobuster enumerates web directories.

Help page `gobuster --help`.
```shell
root@ip-10-10-46-45:~# gobuster --help
Usage:
  gobuster [command]

Available Commands:
  completion  Generate the autocompletion script for the specified shell
  dir         Uses directory/file enumeration mode
  dns         Uses DNS subdomain enumeration mode
  fuzz        Uses fuzzing mode. Replaces the keyword FUZZ in the URL, Headers and the request body
  gcs         Uses gcs bucket enumeration mode
  help        Help about any command
  s3          Uses aws bucket enumeration mode
  tftp        Uses TFTP enumeration mode
  version     shows the current version
  vhost       Uses VHOST enumeration mode (you most probably want to use the IP address as the URL parameter)

Flags:
      --debug                 Enable debug output
      --delay duration        Time each thread waits between requests (e.g. 1500ms)
  -h, --help                  help for gobuster
      --no-color              Disable color output
      --no-error              Don't display errors
  -z, --no-progress           Don't display progress
  -o, --output string         Output file to write results to (defaults to stdout)
  -p, --pattern string        File containing replacement patterns
  -q, --quiet                 Don't print the banner and other noise
  -t, --threads int           Number of concurrent threads (default 10)
  -v, --verbose               Verbose output (errors)
  -w, --wordlist string       Path to the wordlist. Set to - to use STDIN.
      --wordlist-offset int   Resume from a given position in the wordlist (defaults to 0)

Use "gobuster [command] --help" for more information about a command.
root@ip-10-10-46-45:~# 
```

Notice `gobuster [command] --help` for more help on a particular command.


### ❓ Question
> What flag do we use to specify the target URL?
#### 🧪 Process
I don't see it in help. so lets start by checking the commands

Starting with `gobuster completion --help`

```shell
root@ip-10-10-46-45:~# gobuster completion --help
Generate the autocompletion script for gobuster for the specified shell.
See each sub-command's help for details on how to use the generated script.

Usage:
  gobuster completion [command]

Available Commands:
  bash        Generate the autocompletion script for bash
  fish        Generate the autocompletion script for fish
  powershell  Generate the autocompletion script for powershell
  zsh         Generate the autocompletion script for zsh

Flags:
  -h, --help   help for completion

Global Flags:
      --debug                 Enable debug output
      --delay duration        Time each thread waits between requests (e.g. 1500ms)
      --no-color              Disable color output
      --no-error              Don't display errors
  -z, --no-progress           Don't display progress
  -o, --output string         Output file to write results to (defaults to stdout)
  -p, --pattern string        File containing replacement patterns
  -q, --quiet                 Don't print the banner and other noise
  -t, --threads int           Number of concurrent threads (default 10)
  -v, --verbose               Verbose output (errors)
  -w, --wordlist string       Path to the wordlist. Set to - to use STDIN.
      --wordlist-offset int   Resume from a given position in the wordlist (defaults to 0)

Use "gobuster completion [command] --help" for more information about a command.
```

Not seing anything about URL's there. Next `gobuster dir --help`

```shell
root@ip-10-10-46-45:~# gobuster dir --help
Uses directory/file enumeration mode

Usage:
  gobuster dir [flags]

Flags:
  -f, --add-slash                         Append / to each request
      --client-cert-p12 string            a p12 file to use for options TLS client certificates
      --client-cert-p12-password string   the password to the p12 file
      --client-cert-pem string            public key in PEM format for optional TLS client certificates
      --client-cert-pem-key string        private key in PEM format for optional TLS client certificates (this key needs to have no password)
  -c, --cookies string                    Cookies to use for the requests
  -d, --discover-backup                   Also search for backup files by appending multiple backup extensions
      --exclude-length string             exclude the following content lengths (completely ignores the status). You can separate multiple lengths by comma and it also supports ranges like 203-206
  -e, --expanded                          Expanded mode, print full URLs
  -x, --extensions string                 File extension(s) to search for
  -X, --extensions-file string            Read file extension(s) to search from the file
  -r, --follow-redirect                   Follow redirects
  -H, --headers stringArray               Specify HTTP headers, -H 'Header1: val1' -H 'Header2: val2'
  -h, --help                              help for dir
      --hide-length                       Hide the length of the body in the output
  -m, --method string                     Use the following HTTP method (default "GET")
      --no-canonicalize-headers           Do not canonicalize HTTP header names. If set header names are sent as is.
  -n, --no-status                         Don't print status codes
  -k, --no-tls-validation                 Skip TLS certificate verification
  -P, --password string                   Password for Basic Auth
      --proxy string                      Proxy to use for requests [http(s)://host:port] or [socks5://host:port]
      --random-agent                      Use a random User-Agent string
      --retry                             Should retry on request timeout
      --retry-attempts int                Times to retry on request timeout (default 3)
  -s, --status-codes string               Positive status codes (will be overwritten with status-codes-blacklist if set). Can also handle ranges like 200,300-400,404.
  -b, --status-codes-blacklist string     Negative status codes (will override status-codes if set). Can also handle ranges like 200,300-400,404. (default "404")
      --timeout duration                  HTTP Timeout (default 10s)
  -u, --url string                        The target URL
  -a, --useragent string                  Set the User-Agent string (default "gobuster/3.6")
  -U, --username string                   Username for Basic Auth

Global Flags:
      --debug                 Enable debug output
      --delay duration        Time each thread waits between requests (e.g. 1500ms)
      --no-color              Disable color output
      --no-error              Don't display errors
  -z, --no-progress           Don't display progress
  -o, --output string         Output file to write results to (defaults to stdout)
  -p, --pattern string        File containing replacement patterns
  -q, --quiet                 Don't print the banner and other noise
  -t, --threads int           Number of concurrent threads (default 10)
  -v, --verbose               Verbose output (errors)
  -w, --wordlist string       Path to the wordlist. Set to - to use STDIN.
      --wordlist-offset int   Resume from a given position in the wordlist (defaults to 0)
```
Result, `-u, --url string (The target URL)`

Trying this as the answer
#### ✅ Answer
- `-u` ✅

### ❓ Question
> What **command** do we use for the subdomain enumeration mode?
#### 🧪 Process
In the help file above we can see `dns (Uses DNS subdomain enumeration mode)`

Trying this as the answer
#### ✅ Answer
- `dns` ✅


## Use Case: Directory and File Enumeration
So, I'm kinda skip this one because I reviewed most of it as part of the previous question.

Let's skip to the questins

### ❓ Question
> Which flag do we have to add to our command to skip the TLS verification? Enter the long flag notation.
#### 🧪 Process
According to the help file abouve we have ``  -k, --no-tls-validation (Skip TLS certificate verification)

The question stipulates the long flag notation, so `--no-tls-validation`.

Trying this as the answer

#### ✅ Answer
- `--no-tls-validation` ✅

### ❓ Question
> Enumerate the directories of www.offensivetools.thm. Which directory catches your attention?
#### 🧪 Process
I am going with the example and using `gobuster dir -u "http://www.offensivetools.thm" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt`

```shell
root@ip-10-10-46-45:~# gobuster dir -u "http://www.offensivetools.thm" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt 
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://www.offensivetools.thm
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/images               (Status: 301) [Size: 333] [--> http://www.offensivetools.thm/images/]
/home                 (Status: 200) [Size: 8818]
/media                (Status: 301) [Size: 332] [--> http://www.offensivetools.thm/media/]
/templates            (Status: 301) [Size: 336] [--> http://www.offensivetools.thm/templates/]
/modules              (Status: 301) [Size: 334] [--> http://www.offensivetools.thm/modules/]
/plugins              (Status: 301) [Size: 334] [--> http://www.offensivetools.thm/plugins/]
/includes             (Status: 301) [Size: 335] [--> http://www.offensivetools.thm/includes/]
/language             (Status: 301) [Size: 335] [--> http://www.offensivetools.thm/language/]
/components           (Status: 301) [Size: 337] [--> http://www.offensivetools.thm/components/]
/api                  (Status: 301) [Size: 330] [--> http://www.offensivetools.thm/api/]
/cache                (Status: 301) [Size: 332] [--> http://www.offensivetools.thm/cache/]
/libraries            (Status: 403) [Size: 287]
/tmp                  (Status: 301) [Size: 330] [--> http://www.offensivetools.thm/tmp/]
/layouts              (Status: 301) [Size: 334] [--> http://www.offensivetools.thm/layouts/]
/secret               (Status: 301) [Size: 333] [--> http://www.offensivetools.thm/secret/]
/administrator        (Status: 301) [Size: 340] [--> http://www.offensivetools.thm/administrator/]
Progress: 13166 / 218276 (6.03%)^C
[!] Keyboard interrupt detected, terminating.
Progress: 13167 / 218276 (6.03%)
===============================================================
Finished
===============================================================
```

So its still scanning its at 3%, I think the likely target here is `secret` so...

Trying this as the answer
#### ✅ Answer
- `secret` ✅

### ❓ Question
> Continue enumerating the directory found in question 2. You will find an interesting file there with a .js extension. What is the flag found in this file?
#### 🧪 Process
Going with `gobuster dir -u "http://www.offensivetools.thm/secret" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .js`.

```shell
root@ip-10-10-46-45:~# gobuster dir -u "http://www.offensivetools.thm/secret" -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .js
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://www.offensivetools.thm/secret
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Extensions:              js
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/content              (Status: 301) [Size: 341] [--> http://www.offensivetools.thm/secret/content/]
/uploads              (Status: 301) [Size: 341] [--> http://www.offensivetools.thm/secret/uploads/]
/flag.js              (Status: 200) [Size: 22]
Progress: 4014 / 436552 (0.92%)^C
[!] Keyboard interrupt detected, terminating.
Progress: 4014 / 436552 (0.92%)
===============================================================
Finished
===============================================================
```

Oh looksie here. flag.js. I wonder if this will contain the flag...

Let's find out with `curl`

```shell
root@ip-10-10-46-45:~# curl http://www.offensivetools.thm/secret/flag.js
THM{ReconWasASuccess}
```

There we are!

Trying this as the answer

#### ✅ Answer
- `THM{ReconWasASuccess}` ✅


## Use Case: Subdomain Enumeration
On to DNS mode. Basically it's just as important to check subdmains as the normal domain. There could be vulnerable services in the subdomains just as with the main domain.

Lets get the help `gobuster dns --help`

```shell
root@ip-10-10-46-45:~# gobuster dns --help
Uses DNS subdomain enumeration mode

Usage:
  gobuster dns [flags]

Flags:
  -d, --domain string      The target domain
  -h, --help               help for dns
      --no-fqdn            Do not automatically add a trailing dot to the domain, so the resolver uses the DNS search domain
  -r, --resolver string    Use custom DNS server (format server.com or server.com:port)
  -c, --show-cname         Show CNAME records (cannot be used with '-i' option)
  -i, --show-ips           Show IP addresses
      --timeout duration   DNS resolver timeout (default 1s)
      --wildcard           Force continued operation when wildcard found

Global Flags:
      --debug                 Enable debug output
      --delay duration        Time each thread waits between requests (e.g. 1500ms)
      --no-color              Disable color output
      --no-error              Don't display errors
  -z, --no-progress           Don't display progress
  -o, --output string         Output file to write results to (defaults to stdout)
  -p, --pattern string        File containing replacement patterns
  -q, --quiet                 Don't print the banner and other noise
  -t, --threads int           Number of concurrent threads (default 10)
  -v, --verbose               Verbose output (errors)
  -w, --wordlist string       Path to the wordlist. Set to - to use STDIN.
      --wordlist-offset int   Resume from a given position in the wordlist (defaults to 0)
```

Room example command `gobuster dns -d offensivetools.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt`

### ❓ Question
> Apart from the dns keyword and the -w flag, which **shorthand flag** is required for the command to work?
#### 🧪 Process
you need to pass the domain `-d`

trying this as the answer

#### ✅ Answer
- `-d` ✅

### ❓ Question
> Use the commands learned in this task, how many subdomains are configured for the offensivetools.thm domain?
#### 🧪 Process
```shell
root@ip-10-10-46-45:~# gobuster dns -d offensivetools.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt 
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Domain:     offensivetools.thm
[+] Threads:    10
[+] Timeout:    1s
[+] Wordlist:   /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
===============================================================
Starting gobuster in DNS enumeration mode
===============================================================
Found: www.offensivetools.thm

Found: forum.offensivetools.thm

Found: store.offensivetools.thm

Found: WWW.offensivetools.thm

Found: primary.offensivetools.thm

Progress: 4997 / 4998 (99.98%)
===============================================================
Finished
===============================================================
```

There are 5 results.

Trying this as the answer
#### ✅ Answer
- `5`❌

result `www` is duplicated as `WWW`. Web addresses are not case sensitive.
- `4` ✅


## Use Case: Vhost Enumeration
Vhost help (`gobuster vhost --help`).

```shell
root@ip-10-10-46-45:~# gobuster vhost --help
Uses VHOST enumeration mode (you most probably want to use the IP address as the URL parameter)

Usage:
  gobuster vhost [flags]

Flags:
      --append-domain                     Append main domain from URL to words from wordlist. Otherwise the fully qualified domains need to be specified in the wordlist.
      --client-cert-p12 string            a p12 file to use for options TLS client certificates
      --client-cert-p12-password string   the password to the p12 file
      --client-cert-pem string            public key in PEM format for optional TLS client certificates
      --client-cert-pem-key string        private key in PEM format for optional TLS client certificates (this key needs to have no password)
  -c, --cookies string                    Cookies to use for the requests
      --domain string                     the domain to append when using an IP address as URL. If left empty and you specify a domain based URL the hostname from the URL is extracted
      --exclude-length string             exclude the following content lengths (completely ignores the status). You can separate multiple lengths by comma and it also supports ranges like 203-206
  -r, --follow-redirect                   Follow redirects
  -H, --headers stringArray               Specify HTTP headers, -H 'Header1: val1' -H 'Header2: val2'
  -h, --help                              help for vhost
  -m, --method string                     Use the following HTTP method (default "GET")
      --no-canonicalize-headers           Do not canonicalize HTTP header names. If set header names are sent as is.
  -k, --no-tls-validation                 Skip TLS certificate verification
  -P, --password string                   Password for Basic Auth
      --proxy string                      Proxy to use for requests [http(s)://host:port] or [socks5://host:port]
      --random-agent                      Use a random User-Agent string
      --retry                             Should retry on request timeout
      --retry-attempts int                Times to retry on request timeout (default 3)
      --timeout duration                  HTTP Timeout (default 10s)
  -u, --url string                        The target URL
  -a, --useragent string                  Set the User-Agent string (default "gobuster/3.6")
  -U, --username string                   Username for Basic Auth

Global Flags:
      --debug                 Enable debug output
      --delay duration        Time each thread waits between requests (e.g. 1500ms)
      --no-color              Disable color output
      --no-error              Don't display errors
  -z, --no-progress           Don't display progress
  -o, --output string         Output file to write results to (defaults to stdout)
  -p, --pattern string        File containing replacement patterns
  -q, --quiet                 Don't print the banner and other noise
  -t, --threads int           Number of concurrent threads (default 10)
  -v, --verbose               Verbose output (errors)
  -w, --wordlist string       Path to the wordlist. Set to - to use STDIN.
      --wordlist-offset int   Resume from a given position in the wordlist (defaults to 0)
```

Section example
`gobuster vhost -u "http://10.10.202.144" --domain offensivetools.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain --exclude-length 250-320 `

### ❓ Question
> Use the commands learned in this task to answer the following question: How many vhosts on the offensivetools.thm domain reply with a status code 200?
#### 🧪 Process
```shell
root@ip-10-10-46-45:~# gobuster vhost -u http://10.10.202.144 --domain offensivetools.thm -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt --append-domain --exclude-length 250-320
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:              http://10.10.202.144
[+] Method:           GET
[+] Threads:          10
[+] Wordlist:         /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
[+] User Agent:       gobuster/3.6
[+] Timeout:          10s
[+] Append Domain:    true
[+] Exclude Length:   258,281,297,316,261,264,267,272,276,290,254,265,270,273,287,279,305,309,320,293,295,303,250,257,269,280,286,310,319,262,296,298,304,314,253,255,302,307,268,288,294,299,278,289,291,318,292,306,266,277,300,311,259,252,271,282,313,315,308,312,256,260,275,284,285,251,274,301,317,263,283
===============================================================
Starting gobuster in VHOST enumeration mode
===============================================================
Found: forum.offensivetools.thm Status: 200 [Size: 2635]
Found: store.offensivetools.thm Status: 200 [Size: 3014]
Found: www.offensivetools.thm Status: 200 [Size: 8806]
Found: WWW.offensivetools.thm Status: 200 [Size: 8806]
Found: secret.offensivetools.thm Status: 200 [Size: 1550]
Progress: 4997 / 4998 (99.98%)
===============================================================
Finished
===============================================================
```
 5x codes of `200`

 Trying this as the answer, but remember the 2x `www`'s

#### ✅ Answer
- `4`


## Conclusion
We covered `dns`, `dir`, and `vhost`. Good stuff.
