| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **Security Principles** |

# Security Principles

## Contents
- [Introduction](#introduction)
- [CIA](#cia)
- [DAD](#dad)
- [Fundamental Concepts of Security Models](#fundamental-concepts-of-security-models)
- [Defence-in-Depth](#defence-in-depth)
- [ISO/IEC 19249](#isoiec-19249)



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

In the scenario where an malicious actor manages to compomise a system we need to use the **DAD Triad**.

Within the triad we have **Disclosure**, **Alteration**, and **Destruction/Denial**. It is literally the oposite of the CIA triad.

- **Disclosure** of information after a breach, such as credit card inforamtion being leaked after a security breach on a website.
- **Alteration** of medical data after a breach that means patients medical records have been compromised putting the patients safety at risk.
- **Destruction** of data after a breack could mean that confidential data is erased
- **Denial** of a service after a denial of service attack.


### ❓ Question 1

> The attacker managed to gain access to customer records and dumped them online. What is this attack?

#### 🧪 Process

If data is made public, the personal data is subject to a `Disclosure`

Trying this as the answer

#### ✅ Answer

- `Disclosure` ✅


### ❓ Question 2

> A group of attackers were able to locate both the main and the backup power supply systems and switch them off. As a result, the whole network was shut down. What is this attack?

#### 🧪 Process

This would be `Destruction` of property and `Denial` of service.

Trying this as the answer

#### ✅ Answer

- `Destruction/Denial` ✅



## 📘Fundamental Concepts of Security Models

In this section we will review 3 foundational security models

- Bell-LaPadula Model
- The Biba Integrity Model
- The Clark-Wilson Model


### Bell-LaPadula Model

Primary focus is **confidentiality**, acheived by specifying 3 simple rules:

- **Simple Security Property**
    - The "no read up" property, no member should be able to read the the level above their own, preventing access to confidential information not authorised to view.
- **Star Security Property**
    - The "no write down" property, no member should be able to write the level below their own, preventing accidentally making confidential information available to those of a lower authorisation.
- **Discretionary-Security Property**
    - An access matrix to enable read/write operations.

**_Example access matrix_**

|  Subjects  |  Object A  |  Object B  |
|------------|------------|------------|
| Subject 1  | Write      | No Access  |
| Subject 2  | Read/Write | Read       |

We can look at this as "write up, read down". This will allow a security clearence to provide information to a higher level and allow that higher level to read information from a lower level. This model does not work well for file sharing scenarios. 


### Biba Integrity Model

Primary focus is **Integrity**, acheived by specifiying 2 main rules:


- **Simple Integrity Property**
    - The "no read down" property, the subject can't read from a lower level than their own
- **Star Integrity Property**
    - The "no write up" property, the subject can't write to a lever higher than their own

We can look at this as "read up, write down". The rule is the oposite of the Bell-LaPadula model where the focus is confidentiality. This model has a few down sides, one of which is that it does handle insider threats.

### Clark-Wilson Model

Primary focus is **integrity**. This model uses 4 concepts


- **Constrained Data Item (CDI)**
    - The data type requiring preservation of integrity
- **Unconstrained Data Item (UDI)**
    - Data type outside of CDI 
- **Transformation Procedures (TPs)**
    - Programmed operations that maintain integrity of CDIs
- **Integrity Verification Procedures (IVPs)**
    - Checks validity of CDIs
 

### Other models

There are many other security models. Take a look at:


- Brewer and Nash model
- Goguen-Meseguer model
- Sutherland model
- Graham-Denning model
- Harrison-Ruzzo-Ullman model



### ❓ Question

> Click on "View Site" and answer the four questions. What is the flag that you obtained at the end?

#### 🧪 Process

Basic Q&A

#### ✅ Answer

- `THM{SECURITY_MODELS}`



## 📘Defence-in-Depth

Defence in depth can be considered layers of security, think, a building with doors, we could add locks or a form of access control. Next we could add an alarm system to detect movement when the building is secured or broken in to. We could add a Security guard to act as a deterent and to monitor the building. We could add CCTV to allow us to capture events that the security guard missed. All of these on their own, may be enough. But evertying together provides defence in depth.



## 📘ISO/IEC 19249

The **[`International Organization for Standardization`](https://www.iso.org/home.html)** and the **[`International Electrotechnical Commission`](https://www.iec.ch/homepage)** joined forces to create the ISO/IEC 19249, or "_Information technology - Security techniques - Catalogue of architectural and design principles for secure products, systems and applications_".

You can buy it of 132 CEF don't you know (~$166 / £122).

We can split the standard into two areas of principles, _architectural_ and _design_.

### Architectural

- **Domain Seperation**

> Every set of related components is grouped as a single entity; components can be applications, data, or other resources.
>
> Each entity will have its own domain and be assigned a common set of security attributes.
>
> For example, consider the x86 processor privilege levels: the operating system kernel can run in ring 0 (the most privileged level). In contrast, user-mode applications can run in ring 3 (the least privileged level). Domain separation is included in the Goguen-Meseguer Model.

- **Layering**

- 
- **Encapsulaton**
- **Redundancy**
- **Virtualisation**



