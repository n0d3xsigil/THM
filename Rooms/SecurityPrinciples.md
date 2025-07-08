| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **Security Principles** |

# Security Principles

## Contents
- [Introduction](#introduction)
- [CIA](#cia)
- [DAD](#dad)



## 📘Introduction

This room discusses the throughts behind implementing security considerations suitable for the environment and potential threat actors.



## 📘CIA

The CIA Triad

- **Confidentiality**
- **Integrity**
- **Availability**

![image](https://github.com/user-attachments/assets/64606ceb-1326-41ca-abd6-958cbe648b90)

The fundamental principle of security.

We have **confidentiality**, the act of ensuring that information is secure and only accessible by those meant to see it, **Integrity**, ensuring that the data can't be modified, and any changes are captured, and finally **Availablility**, the aim of any service is to be accessible when needed.

- **Examples**:
    - **Online Shopping**
        - **Confidentiality** in an online shopping scinario would require that your personal information (Credit card details, Address, Name, etc.) be kept confidential between you (the shopper) and the store. However it's important to note that other parties may be required to know specific pieces of information, for example the payment processor will need to know your payment card information ('long number', expiry, security number, address, name), whilst the courier may need your name, address and potentially a contact number. These 3rd parties may need the nessecary information in order to complete their part of the process and to be compliant in regulatory comitments. 
        - **Integrity** for an online shop may look like using some form of encryption to ensure that the information from your browser and the store is not intercepted and potentially modified enroute. Also there should be protections on any servers or databases that ensure the data can't be modifed inadvertantly or by unauthorised parties.
        - **Availability** is essential if you want to be able to offer services or products to a customer. If the site is not available or there are errors logging in because a database is down then the reputation will be impacted. What if the back end infrastrcutre is up but the website is inaccessible because of a Denyal of Service attack.
    - **Paitent Records**
        - **Confidentiality** in patient records are a serious matter. Imagine if your test results where to be leaked, or psychotherapy records leaked (See **Vastaamo** data breach). Medical providers are legally bound to keep health care information away from prying eyes. Think **HIPPA** in the US and **GDPR** in Europe.  
        - **Integrity** failures could have a real world impact to a paitent. What if a diagnosis where modified, or a medication adjusted. The concquences could be dire.
        - **Availability** is critical, ensuring the paitent data is available at the time of an appointment will ensure health care providers are best informed and able to provide treatment to the best of the information available to them. Should the information not be available for some reason diagnoses could be incorrect or delayed at best.
  
### Beyond CIA

**Authenticity** means ensuring that the data is from who it is calimed to be.
**Nonrepudiation** is making sure that the source of something is not able to deny they are the source.

These two are almost the two sides of the same coin, we make sure that this document is from this person. They have signed to say "_Yes, I created it, it is correct_" and then later on they decide "_I never said that_". We can evidence that in fact they did.

### Parkerian Hexad

Proposed in 1998 by Donn Parker. 
1. Availability
2. Utility
3. Integrity
4. Authenticity
5. Confidentiality
6. Possession

1, 3, 5, and 4 have already been covered. This leaves 2 and 6
- **Utility** focuses on the usefulness of the information. For instance, a user might have lost the decryption key to access a laptop with encrypted storage. Although the user still has the laptop with its disk(s) intact, they cannot access them. In other words, although still available, the information is in a form that is not useful, i.e., of no utility.
- **Possession** is the security element requires that we protect the information from unauthorized taking, copying, or controlling. For instance, an adversary might take a backup drive, meaning we lose possession of the information as long as they have the drive. Alternatively, the adversary might succeed in encrypting our data using ransomware; this also leads to the loss of possession of the data.

### ❓ Question

> Click on "View Site" and answer the five questions. What is the flag that you obtained at the end?

#### 🧪 Process

Just a Q&A, but be careful. There's a timer. 

#### ✅ Answer

- `THM{CIA_TRIAD}` ✅



### 📘DAD
