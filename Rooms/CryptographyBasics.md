## Contents
- [Introduction](#introduction)
- [Importance of Cryptography](#importance-of-cryptography)
- [Plaintext to Ciphertext](#plaintext-to-ciphertext)
- [Historical Ciphers](#historical-ciphers)
- [Types of Encryption](#types-of-encryption)
- [Basic Math](#basic-math)
- [Summary](#summary)

## Introduction

### Introduction to Cryptography
This is the first of three beginner-friendly lessons on cryptography, requiring only basic Linux command line skills. It explores how cryptography ensures secure communication by preventing unauthorized access and verifying the identity of servers.
### Topics Covered in This Room:
- Key cryptographic terms
- Why cryptography is essential
- The Caesar Cipher (a simple encryption method)
- Standard symmetric encryption techniques
- Common asymmetric encryption methods
- Basic math used in cryptography
### Purpose:
To help you understand how cryptography builds trust in digital communication and lays the groundwork for secure systems.
### Question 1 - I’m ready to start learning about cryptography!
#### Process
Nothing to do here. 
#### Answer 1
 - **Click** 😉


## Importance of Cryptography
### Purpose of Cryptography
> Cryptography’s ultimate purpose is to ensure _secure communication in the presence of adversaries_.

Cryptography ensures secure communication even when adversaries are present. It protects:
- **Confidentiality** – keeping data private
- **Integrity** – ensuring data isn’t altered
- **Authenticity** – verifying identities
### Everyday Uses of Cryptography:
- **Logging in securely** (e.g., TryHackMe credentials)
- **SSH** connections for private remote access
- **Online banking** with server identity verification
- **File downloads** verified using hash functions
### Legal & Industry Standards:
- **PCI DSS**: Required for handling credit card data (encryption at rest and in transit)
- **Medical Data Regulations**:
  - **HIPAA** (Health Insurance Portability and Accountability Act) - USA
  - **HITECH** (Health Information Technology for Economic and Clinical Health) - USA
  - **GDPR** (General Data Protection Regulation) - Europe
  - **DPA** (Data Protection Act) - UK
These laws highlight that cryptography is essential but often invisible in digital systems.
### Question 1 - What is the standard required for handling credit card information?
#### Process
We already know that the PCI DSS is the payment regulation in the US. The answer calls for `___ ___` so is the only answer possible.
Trying this as the answer
#### Answer 1
- `PCI DSS` ✅


## Plaintext to Ciphertext
### Core Concepts of Cryptography
This section introduces the basic flow of encryption and decryption using key terms and a simple process:
### Encryption Process Overview:
- **Plaintext** – The original, readable data (e.g., text, images, files).
- **Encryption Function** – Uses a key and a cipher to convert plaintext into:
- **Ciphertext** – The scrambled, unreadable version of the message.
![image](https://github.com/user-attachments/assets/b5b1ab97-16c5-4a66-92a8-87d78aef6a68)
To reverse the process:
- **Decryption Function** – Uses the **same key** (or a related one) and the cipher to convert ciphertext back into plaintext.
![image](https://github.com/user-attachments/assets/551ad1e0-5607-4c66-9e06-c14f35ea53b2)
### Key Terms Defined:
- **Plaintext**: Original, readable data before encryption.
- **Ciphertext**: Encrypted, unreadable data.
- **Cipher**: The algorithm used to encrypt and decrypt data.
- **Key**: A secret string of bits used by the cipher to perform encryption/decryption.
- **Encryption**: The process of converting plaintext into ciphertext.
- **Decryption**: The process of converting ciphertext back into plaintext.
**Note**: The cipher is usually public, but the key must remain secret (unless it's a public key in asymmetric encryption, which will be covered later).
### Question 1 - What do you call the encrypted plaintext?
#### Process
Plain Text when it has been encrypted becomes `ciphertext`.
Trying this as the anser
#### Answer 1
-`Ciphertext` ✅
### Question 2 - What do you call the process that returns the plaintext?
#### Process
The reverse of encryption is `decryption`.
Trying this as the answer
#### Answer 2
- `Decryption`✅


## Historical Ciphers
### Historical Cryptography & the Caesar Cipher
- Cryptography dates back to **ancient Egypt (1900 BCE)**, but one of the earliest and simplest ciphers is the Caesar Cipher, used in the 1st century BCE.
- **Caesar Cipher** works by **shifting each letter** in the plaintext by a fixed number of positions in the alphabet.
#### Example:
**Plaintext**: `TRYHACKME`
**Key**: `3` (right shift)
![image](https://github.com/user-attachments/assets/109afc4d-43e6-4285-8795-90b205715bcb)
**Ciphertext**: `WUBKDFNPH`
To **decrypt**, shift letters left by the same key.
![image](https://github.com/user-attachments/assets/39a44080-a3a3-48af-9d35-ea49ed801bc7)
### Security Weakness:
![image](https://github.com/user-attachments/assets/aeb62482-21e4-42f1-b7b1-26fd777cee6c)
- Only **25 possible keys**, making it vulnerable to **brute-force attacks**.
- By modern standards, it's **insecure** because the cipher is public and easily breakable.
### Other Historical Ciphers:
- **Vigenère Cipher** (16th century)
- **Enigma Machine** (WWII)
- **One-Time Pad** (Cold War)
These ciphers appear in **movies**, **books**, and are foundational to understanding modern cryptography.
### Question 1 - Knowing that `XRPCTCRGNEI` was encrypted using Caesar Cipher, what is the original plaintext?
#### Process
Initially I had made the assumtion because it was specified previously in the section that the _encrypt_ was +3 so the _decrypt_ would be -3. I spent way too long looking at that.

I tried multiple things but in the end I ended up hitting the hint. It gave me two options
> You can use an online tool such as the ones available at
> https://cryptii.com/pipes/caesar-cipher and
> https://www.dcode.fr/caesar-cipher

After multiple revisions I ended up with +11 `ICANENCRYPT`.
Trying this as the answer
#### Answer 1
- `ICANENCRYPT` ✅


## Types of Encryption
### Two Main Types of Encryption
#### 1. Symmetric Encryption (Private Key Cryptography)
![image](https://github.com/user-attachments/assets/4352e627-fd75-4d69-bb9b-73fc4f681495)
- **Same key** is used for both encryption and decryption.
- Key must be kept **secret** and securely shared.
- **Challenge**: Securely distributing the key, especially to multiple recipients.
- **Example scenario**: Sending a password-protected document (emailing the password is insecure).
- Common Algorithms:
  - **DES** (Data Encryption Standard)
    - 56-bit, now insecure
  - **3DES** (Triple Data Encryption Standard)
    - 168-bit, deprecated in 2019
  - **AES** (Advanced Encryption Standard)
    - 128, 192, or 256-bit; current standard
#### 2. Asymmetric Encryption (Public Key Cryptography)
![image](https://github.com/user-attachments/assets/28b38882-bc22-40c6-b7cb-b72076817665)
- Uses a **pair of keys**:
  - **Public key**: used to encrypt
  - **Private key**: used to decrypt
  - Only the **private key** must be kept secret.
- **Slower** than symmetric encryption but solves the key distribution problem.
- Common Algorithms:
  - **RSA** (Rivest-Shamir-Adleman)
    - 2048–4096-bit keys
  - **Diffie-Hellman**
  - **ECC** (Elliptic Curve Cryptography)
    - offers strong security with shorter keys
### Key Insight:
Asymmetric encryption relies on mathematical problems that are easy to compute in one direction but infeasible to reverse (e.g., factoring large primes).
### New Terms Introduced
- Alice and Bob: Fictional characters used to explain cryptographic communication.
- Symmetric Encryption: One key for both encryption and decryption.
- Asymmetric Encryption: Public key for encryption, private key for decryption.
### Question 1 - Should you trust DES? (Yea/Nay)
#### Process
`Nay`. The DES standard was created back in 1977 and only had a 56-bit key. It is easy to break with modern hardware. 
Trying this as the answer
#### Answer 1
- `Nay` ✅
### Question 2 - When was AES adopted as an encryption standard?
#### Process
Way back in 2001
Trying this as the answer
#### Answer 2
- `2001` ✅


## Basic Math
### Mathematical Foundations of Cryptography
Modern cryptography relies heavily on mathematical operations. Two fundamental operations used in many cryptographic algorithms are:
### 1. XOR Operation (Exclusive OR)
- **Definition**: A binary operation that returns:
  - `1` if the bits are different
  - `0` if the bits are the same
- Truth Table:

|   A  	|   B  	| A ⊕ B |
|-------|-------|-------|
|  `0`  |  `0`  |  `0`  |
|  `0`	|  `1`  |  `1`  |
|  `1`	|  `0`  |  `1`  |
|  `1`	|  `1`  |  `0`  |

- Properties:
  - `A` ⊕ `A` = `0`
  - `A` ⊕ `0` = `A`
  - **Commutative**: `A` ⊕ `B` = `B` ⊕ `A`
  - **Associative**: (`A` ⊕ `B`) ⊕ `C` = `A` ⊕ (`B` ⊕ `C`)
- **Use in Cryptography**:
  - Simple symmetric encryption:
    - **Encryption**: `C = P ⊕ K`
    - **Decryption**: `P = C ⊕ K`
  - Requires the **key (**`K`**)** to be as long as the **plaintext (**`P`**)** for security.
### 2. Modulo Operation
- **Definition**: The remainder after division.
  - Written as % or mod
  - Example: 23 % 6 = 5 because 23 = 3 × 6 + 5
- **Key Points**:
  - **Not reversible**: Many values can satisfy the same modulo result.
  - **Always returns a non-negative** result in the range 0 to n - 1 for a % n.
- **Importance in Cryptography**:
  - Used in **key generation**, **hashing**, and **asymmetric encryption**.
  - Essential for working with **large numbers** and **cyclic structures**.
### Question 1 - What’s 1001 ⊕ 1010?
#### Process
I had to YouTube this one. After searching just `xor` I found the following video:
- [The power of XOR - Gary Explains](https://www.youtube.com/watch?v=3Kvv7VEM3uc)

Essentially you can only have 1 line being true otherwise it is false. as mentioned before, 0,0 would be 0 because neither are exclusive, 1,0 would be 1 because `a` is exclusive, 0,1 would be 1 because `b` is exclusive and finally 1,1 would be 0 because again neither are exclusive.

So the answer for the question `1001 ⊕ 1010` can be calculated as follows:

|      A: | 1 | 0 | 0 | 1 |
|      B: | 1 | 0 | 1 | 0 |
| Result: | 0 | 0 | 1 | 1 |

Trying This as the answer
#### Answer 1
- `0011` ✅
### Question 2 - What’s 118613842%9091?
#### Process
I could use `PowerShell`, `Bash` or even a calculator to give me the answer but I really would like to step through it. 

divisor:        9,091
dividend: 118,613,842

`9,091` will fit in `118,613,842` just  `13,047` times. 
`9,091` multipled by `13,047` is `118,610,277`

To find the remainder we subtract `118,610,277` from `118,613,842` to get a reminader of `3,565`

The answer calls for the format `____` which matches the number of characters the answer has.

Trying this as the answer
#### Answer 2
- `3565` ✅
### Question 3 - What’s 60%12?
#### Process
Well `60` is divisable by `12`.  The answer is `0`

Trying this as the answer
#### Answer 3
- `0` ✅


## Summary
### Room Recap: Cryptography Basics
In this room, we learned:
- **Why cryptography is essential** for secure digital communication.
- The difference between **symmetric** and **asymmetric encryption**.
- How **XOR** and **modulo operations** serve as foundational tools in cryptographic algorithms.
### Next Step:
In the upcoming room, **Public Key Cryptography Basics**, you'll explore real-world **asymmetric cryptosystems** and how they address modern digital security challenges.
