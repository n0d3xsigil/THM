| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **Public Key Cryptography Basics** |

## Contents
- [Introduction](#introduction)
- [Common Use of Asymmetric Encryption](#common-use-of-asymmetric-encryption)
- [RSA](#rsa)
- [Diffie-Hellman Key Exchange](#diffie-hellman-key-exchange)
- [SSH](#ssh)
- [Digital Signatures and Certificates](#digital-signatures-and-certificates)
- [PGP and GPG](#pgp-and-gpg)
- [Conclusion](#conclusion)
## Introduction
### Real-Life Analogy for Cybersecurity Concepts
Using a casual meeting with a business partner, the text introduces four core cybersecurity principles:
- **Authentication**: Confirming the identity of the person you're communicating with.
- **Authenticity**: Verifying the message truly comes from the claimed sender.
- **Integrity**: Ensuring the message hasn't been tampered with.
- **Confidentiality**: Keeping the communication private from unauthorized access.
### How Cryptography Supports These Principles
- **Symmetric Cryptography**: Primarily ensures confidentiality.
- **Asymmetric Cryptography**: Supports authentication, authenticity, and integrity.
### Learning Focus: Public Key Cryptography
This module introduces key tools and systems used in public key cryptography:
- **RSA**
- **Diffie-Hellman**
- **SSH**
- **SSL/TLS Certificates**
- **PGP** and **GPG**

It is the second module in a three-part series:
- [Cryptography Basics](/CryptographyBasics.md)
- Public Key Cryptography Basics (this one)
- [Hashing Basics](/HashingBasics.md)


## Common Use of Asymmetric Encryption
### Key Exchange Using Asymmetric Cryptography
#### Why Use Asymmetric Encryption for Key Exchange?
- **Asymmetric encryption is slower**, so it's mainly used to **securely exchange symmetric keys**.
- Once the symmetric key is exchanged, **faster symmetric encryption** is used for ongoing communication.
### Analogy: The Locked Box

To explain secure key exchange, the text uses a metaphor:
![image](https://github.com/user-attachments/assets/4ec9e6b7-4ba1-4112-9ce5-c4af6ca6ad19)

|   Analogy   |         Cryptographic Concept         |
|-------------|---------------------------------------|
| Secret Code	| Symmetric Encryption Cipher and Key   |
| Lock	      | Public Key (used to encrypt the box)  |
| Lock’s Key	| Private Key (used to decrypt the box) |

- You ask your friend (the server) for a **lock** (public key).
- You place your **secret code** (symmetric key) in a **box** and lock it.
- Your friend uses their **private key** to unlock the box and retrieve the key.
- Now, both of you can communicate securely using symmetric encryption.
  
### In Practice
- Real-world systems also need to **verify identities**.
- This is done using **digital signatures and certificates**, which will be covered later.

### Question 1 - In the analogy presented, what real object is analogous to the public key?
#### Process
Well the answer format is `____` so it must the `Lock`

Trying this as the answer
#### Answer 1
- `Lock` ✅


## RSA
### RSA: Public-Key Encryption for Secure Communication
**RSA** is an asymmetric encryption algorithm that enables secure data exchange over insecure channels, where eavesdropping is expected.
### Mathematical Foundation of RSA
- **Security Basis**: RSA relies on the difficulty of **factoring large numbers**.
  - Multiplying two large primes is easy.
  - Factoring their product is computationally hard.
#### Example:
- **Prime 1**: `982451653031` or `982,451,653,031`
- **Prime 2**: `169743212279` or `169,743,212,279`
- **Product**: `166764499494295486767649` or `166,764,499,494,295,486,767,649`
- Factoring this product is much harder than multiplying the primes.
### RSA Key Generation & Usage
![image](https://github.com/user-attachments/assets/9905c6a4-ff2b-4c19-ae28-19603b7c4a22)

#### 1. Key Generation:
- Choose two primes: $\text{p = 157,q = 199}$
- Compute $\text{n = p × q = 31243}$
- Compute $\text{ϕ(n) = (p−1)(q−1) = 30888}$
- Choose $\text{e = 163 such that gcd(e,ϕ(n)) = 1}$
- Compute $\text{d = 379 such that e × d ≡ 1}\mod\text{ϕ(n)}$
#### 2. Keys:
- Public Key:  $\text{(n,e) = (31243,163)}$
- Private Key: $\text{(n,d) = (31243,379)}$
#### 3. Encryption:
- Message $\text{x = 13}$
- Ciphertext $\text{y = x^2 mod n = 13^163 mod 31243 = 16341}$
#### 4. Decryption:
- $\text{ x = y^d mod n = 16341^379 mod 31243 = 13}$
> RSA uses modular arithmetic, which is essential for its functionality.
### RSA in CTFs (Capture The Flag Challenges)
- RSA is common in cryptography-based CTFs.
- You may be given variables like:
  - `p`, `q`: prime numbers
  - `n`: modulus (p × q)
  - `e`: public exponent
  - `d`: private exponent
  - `m`: plaintext
  - `c`: ciphertext
- Goal: Use these to decrypt messages or break weak implementations.
### Tools:
- **RsaCtfTool**: Automates attacks on weak RSA setups.
- **rsatool**: Helps generate or analyze RSA keys.
### Question 1 - Knowing that p = 4391 and q = 6659. What is n?
#### Process
1. $\text{p = 4391}$ and $\text{q = 6659}$
2. $\text{p x q = n}$
3. $\text{4391 x 6659 = n}$
4. $\text{n = 29239669}$

The answer format is `________` or `8` digits. 

Trying this as the answer
### Answer
- `29239669` ✅
### Question 2 - Knowing that p = 4391 and q = 6659. What is ϕ(n)?
#### Process
This one is hurting my head a little, and I really wonder if I come back to this at a later date, will I have any idea what this means?!
1. $\text{p = 4391}$ and $\text{q = 6659}$
2. $\text{ϕ(n) = (4391 - 1)(6659 - 1)}$
3. $\text{ϕ(n) = (4390)(6658)}$
4. $\text{ϕ(n) = 29,228,620 }$

Trying this as the answer
### Answer
- `29228620` ✅


## Diffie-Hellman Key Exchange
### The Challenge of Symmetric Encryption
When using **symmetric encryption**, both parties need to share the same secret key. But how do you share that key securely over an insecure channel?
### Diffie-Hellman Key Exchange: A Solution
Diffie-Hellman allows two people to **create a shared secret key** over an insecure channel, **without ever sending the key itself**.
### How It Works (Simplified):
- **Public Setup**: Alice and Bob agree on two public numbers:
  - A large **prime number** $\text{p}$
  - A **generator** $\text{g, where 0 < g < p}$
- **Private Secrets**:
  - Alice picks a secret number $\text{a}$
  - Bob picks a secret number $\text{b}$
- **Public Keys**:
  - Alice computes $\text{A = g}^\text{a}\text{ mod p}$
  - Bob computes  $\text{B = g}^\text{b}\text{ mod p}$
- **Exchange**:
  - Alice sends $\text{A}$ to Bob
  - Bob sends $\text{B}$ to Alice
- **Shared Secret**:
  - Alice computes $\text{B}^\text{a}\text{ mod p}$
  - Bob computes $\text{A}^\text{b}\text{ mod p}$
  - Both get the same result: $\text{g}^\text{ab}\text{ mod p}$

![image](https://github.com/user-attachments/assets/e1661b65-940a-4158-bc6f-f3db6e810b0b)

This shared value becomes the symmetric key for secure communication.

### Real-World Use
- In practice, **much larger numbers** are used for security.
- **Diffie-Hellman** is often combined with **RSA**:
  - **Diffie-Hellman**: for secure key exchange
  - **RSA**: for authentication and digital signatures

Together, they help prevent attacks like **man-in-the-middle** and are used in many modern security protocols.
### Question 1 - Consider p = 29, g = 5, a = 12. What is A?
#### Process
I had to do some digging on this. Even after reading the page several times it wasn't clicking. The calcualtion is actually fairly straight forward.
1. $\text{A = g}^\text{a}\text{ mod p}$
2. $\text{A = 5}^\text{12}\text{ mod 29}$
3. $\text{A = 244,140,625 mod 29}$
5. $\text{A = 7}$

Trying this as the answer
#### Answer 1
- `7` ✅
### Question 2 - Consider p = 29, g = 5, b = 17. What is B?
#### Process
I'm hoping we can use the same process as before
1. $\text{B = g}^\text{b}\text{ mod p}$
2. $\text{B = 5}^\text{17}\text{ mod 29}$
3. $\text{B = 762,939,453,125 mod 29}$
4. $\text{B = 9}$

Trying this as the answer
#### Answer 2
- `9` ✅
### Question 3 - Knowing that p = 29, a = 12, and you have B from the second question, what is the key calculated by Bob? (key = Ba mod p)
#### Process
Let's try first
Where B = 9
1. $\text{K = B}^\text{a}\text{ mod p}$
2. $\text{K = 9}^\text{12}\text{ mod 29}$
3. $\text{K = 282,429,536,481 mod 29}$
4. $\text{K = 24}$

Trying this as the answer
#### Answer 3
- `24` ✅
### Question 4 - Knowing that p = 29, b = 17, and you have A from the first question, what is the key calculated by Alice? (key = Ab mod p)
#### Process
I think we're almost there...
Where A = 7
1. $\text{K = A}^\text{b}\text{ mod p}$
2. $\text{K = 7}^\text{17}\text{ mod 29}$
3. $\text{K = 232,630,513,987,207 mod 29}$
4. $\text{K = 24}$

That is amazing, so K is also `24` as it should be.

Trying this as the answer
#### Answer 4
- `24` ✅


## SSH
### SSH Authentication: Server and Client
#### Authenticating the Server
- When you connect to a server via SSH, your client checks the server’s **public key fingerprint**.
- If it’s unknown, you’re prompted to confirm the connection.
- This protects against **man-in-the-middle attacks**.
- Once accepted, the key is saved and trusted for future connections.
#### Authenticating the Client
- After verifying the server, the client must authenticate.
- This is often done with **usernames and passwords**, but **SSH key authentication** is more secure.
- SSH keys use **public/private key pairs**. The private key stays on your machine; the public key is shared with the server.
### Generating SSH Keys
- Use `ssh-keygen` to create key pairs.
- Supported algorithms include:
  - RSA
  - DSA
  - ECDSA
  - Ed25519
  - Variants with hardware security keys (e.g., -sk)

Example:
```Shell
ssh-keygen -t ed25519
```
- You can add a passphrase to encrypt the private key for extra protection.

### SSH Key Files
- **Public key**: `id_ed25519.pub` — shared with the server.
- **Private key**: `id_ed25519` — must be kept secret.
- Never share your private key. It’s like a password that grants access to any server that trusts it.
### Security Practices
- The **passphrase** protects the private key but is never sent to the server.
- Tools like **John the Ripper** can attempt to crack weak passphrases.
- Always generate keys on your own machine and use `ssh-copy-id` to transfer the public key.
- Set correct permissions: private keys should be readable only by the owner (`chmod 600`).
### Key Storage and Usage
- Keys are stored in `~/.ssh/`
- The server stores trusted public keys in `~/.ssh/authorized_keys`
- Use `ssh -i privateKeyFile user@host` to specify a key manually.
### CTFs and Reverse Shells
- SSH keys can be used to **upgrade reverse shells** during CTFs or red teaming.
- Adding a key to `authorized_keys` can act as a **backdoor** for persistent access.
- This avoids unstable shells and gives full terminal features.
### Question 1 - Check the SSH Private Key in `~/Public-Crypto-Basics/Task-5`. What algorithm does the key use?
#### Process
I'm lazy I just ran `ls -la  Public-Crypto-Basics/Task-5/` from `~`. The results are below:
```Shell
user@ip-10-10-237-255:~$ ls -la  Public-Crypto-Basics/Task-5/
total 12
drwxrwxr-x 2 user user 4096 Sep  1  2024 .
drwxrwxr-x 4 user user 4096 Sep  2  2024 ..
-rw-r--r-- 1 user user 1767 Sep  1  2024 id_rsa_1593558668558.id_rsa
```
It's quite clear that the rsa's are here. 

Trying this as the answer
#### Answer 1
- `RSA` ✅


## Digital Signatures and Certificates







































## PGP and GPG







































## Conclusion







































