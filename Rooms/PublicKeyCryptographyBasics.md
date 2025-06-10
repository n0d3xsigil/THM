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







































## SSH







































## Digital Signatures and Certificates







































## PGP and GPG







































## Conclusion







































