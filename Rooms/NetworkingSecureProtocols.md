# Networking Secure Protocols
## Contents
- [TLS](#TLS)
- [HTTPS](#HTTPS)
- [SMTPS, POP3S, and IMAPS](#SMTPS-POP3S-and-IMAPS)
- [SSH](#SSH)
- [SFTP and FTPS](#SFTP-and-FTPS)
- [VPN](#VPN)
## TLS
## HTTPS
## SMTPS, POP3S, and IMAPS
## SSH
Tatu Yimnen released in SSH-1 in 1995. SSH-2 was defined in 1996 and OpenBSD released OpenSSH in 1999.

>**Question**: _What is the name of the open-source implementation of the SSH protocol?_

>**Answer**: `OpenSSH`

## SFTP and FTPS
SFTP and FTPS are not the same beast. 
- SFTP = SSH FTP and uses port 21
- FTPS = File Transfer Protocol Secure and uses port port 990

| CPT | Description | SPT | Description |
|-----|-------------|-----|-------------|
|  21 |  FTP        | 990 |  FTPS       |
|  23 |  Telnet     |  22 |  SSH        |
|  25 |  SMTP       | 587 |  SMTPS      |
|  80 |  HTTP       | 443 |  HTTPS      |
| 110 |  POP3       | 995 |  POP3S      |
| 143 |  IMAP       | 993 |  IMAPS      |


> **Question**: _Click on the **View Site** button to access the related site. Please follow the instructions on the site to obtain the flag._

- **Answer**: `THM{Protocols_secur3d}`


## VPN
