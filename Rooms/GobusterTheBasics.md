| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **Gobuster: The Basics** |

## Contents
- [Introduction](#introduction)
- [Environment and Setup](#environment-and-setup)
- [Gobuster: Introduction](#gobuster-introduction)
- [Use Case: Directory and File Enumeration](#use-case-directory-and-file-enumeration)


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
