| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **CyberChef: The Basics** |

# CyberChef: The Basics

## Contents
- [Introduction](#introduction)
- [Accessing the Tool](#accessing-the-tool)
- [Navigating the Interface](#navigating-the-interface)
- [Before Anything Else](#before-anything-else)
- [Practice, Practice, Practice](#practice-practice-practice)


## 📘Introduction
CyberChef is a web based tool to various conversions such as data formats `UTF-8 -> HEX` or even AES encryption / RSA decryption

You can create recipies that are processed in order.


## 📘Accessing the Tool

So not only can you access it via the website [https://cyberchef.io](https://cyberchef.io) but you can also download it. You can also download it via the [CyberChef GitHub](https://github.com/gchq/CyberChef/releases).

## 📘Navigating the Interface

The interface is fairly intuitave, drag an operation into the Recipe, and do what you need to.

### ❓ Question 1

> In which area can you find "From Base64"?

#### 🧪 Process

In the `Operations` section.

Trying this as the answer

#### ✅ Answer

- `Operations` ✅


### ❓ Question 2

> Which area is considered the heart of the tool?

#### 🧪 Process

The `Recipe` area is considered the heart of the tool.

Trying this as the answer

#### ✅ Answer

- `Recipe` ✅



## 📘Before Anything Else

![image](https://github.com/user-attachments/assets/360ba99a-f45d-46c2-bd1f-a8cda4adf621)

### ❓ Question

> At which step would you determine, "What do I want to accomplish?

#### 🧪 Process

The fundamental rule. Always know what you want to accomplish as the first step `1`

Trying this as the answer
#### ✅ Answer

- `1` ✅



## 📘Practice, Practice, Practice

### ❓ Question 1

> What is the hidden email address?

#### 🧪 Process

Import the file

Search for `email`

Drag **Extract email addresses** to the recipe.

Just the one result which is `hidden@hotmail.com`

Trying this as the answer

#### ✅ Answer

- `hidden@hotmail.com` ✅


### ❓ Question 2

> What is the hidden IP address that ends in .232?

#### 🧪 Process

Remove **Extract email addresses** from the recipe.

Search for IP

Drap **Extract IP addresses** to the recipe.

Results in 2 IP Addresses
- `102.20.11.232`
- `10.10.2.10`

We're interested in the one ending in `.232` which is `102.20.11.232`

Trying this as the answer

#### ✅ Answer

- `102.20.11.232` ✅


### ❓ Question 3

> Which domain address starts with the letter "T"?

#### 🧪 Process

Remove all items from the recipe.

Search for `domain`

Drag **Extract domains** into the recipe.

Results in 2 domians:
- `TryHackMe.com`
- `hotmail.com`

Only one matches the _starts with the letter `T`_ requirement which is `TryHackMe.com`.

Trying this as the answer

#### ✅ Answer

- `TryHackMe.com` ✅


### ❓ Question 4

> What is the binary value of the decimal number 78?

#### 🧪 Process

Remove all recipes.

Search for `decimal`

Drag **From Decimal** into the recipe

Search for `binary`

Drag **To Binary** into the recipe

Clear the input file and type `78`.

The result is `01001110`

Trying this as the answer

#### ✅ Answer

- `01001110` ✅


### ❓ Question 5

> What is the URL encoded value of `https://tryhackme.com/r/careers`?

#### 🧪 Process

clear the recipe

Search for URL 

Drag **URL Encode** into the recipe

Click **Encode all special chars**

Paste the url into the input.

The result is `https%3A%2F%2Ftryhackme%2Ecom%2Fr%2Fcareers`

Trying this as the answer

#### ✅ Answer

- `https%3A%2F%2Ftryhackme%2Ecom%2Fr%2Fcareers` ✅
