| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **Web Application Basics** |

## Contents
- [Introduction](#introduction)
- [Web Application Overview](#web-application-overview)
- [Uniform Resource Locator](#uniform-resource-locator)


## 📘Introduction

### Web Application Basics - Introduction Summary
- The room introduces fundamental components of web applications.
- Focus areas include:
	- URLs (Uniform Resource Locators)
	- HTTP requests
	- HTTP responses
- It is designed for:
	- Beginners seeking foundational knowledge
	- Individuals planning to build or interact with web applications


## 📘Web Application Overview
### Web Application Basics - Planet Analogy & Web Components
- A web application is likened to a planet:
	- **Astronauts** = users with web browsers
	- **Planet surface** = what users see and interact with (Front End)
	- **Beneath the surface** = the unseen but essential systems (Back End)

### Front End
- The Front End is the **visible** part of a web application, like the surface of a planet.
- Technologies used:
	- **HTML (HyperText Markup Language)**:
		- Provides structure and content
		- Like DNA of basic organisms it defines layout and presence
	- **CSS (Cascading Style Sheets)**:
        - Adds visual styling (colour, layout, font)
        - Like cosmetic traits in DNA, it controls appearance
    - **JavaScript (JS)**:
        - Adds interactivity and logic to a web page
        - Like a brain that makes decisions based on interaction

### Back End
- The Back End handles everything **behind the scenes**, it's not visible to users but vital.
- Components:
    - **Database**:
        - Stores, retrieves, and modifies information (e.g., user preferences)
        - Like maps, diaries, libraries, organised information storage
    - **Infrastructure**:
        - Includes servers, networking, and systems that keep things running
        - Comparable to roads, cars, and fuel on a planet
    - **WAF (Web Application Firewall)**:
        - Optional but important for security
        - Acts like a planet’s protective atmosphere, it filters harmful traffic

### Summary
- **Front End**: HTML, CSS, and JavaScript create the user experience in the browser.
- **Back End**: Web servers, databases, infrastructure, and WAFs power the application behind the scenes.
- This foundational overview sets the stage for deeper learning in future sections.

### ❓ Question 1 - Which component on a computer is responsible for hosting and delivering content for web applications?
#### 🧪 Process
Try not to think of `computer` as the _local_ client and rather a compute unit generally. 

So the component responsible for hosting a web application on a compute engine would be a `web server`.

Trying this as the answer
#### ✅ Answer 1
- `web server` ✅
### ❓ Question 2 - Which tool is used to access and interact with web applications?
#### 🧪 Process
Generally you would interact with a web site with a `web browser`. 

Trying this as the answer
#### ✅ Answer 2
- `web browser` ✅
### ❓ Question 3 - Which component acts as a protective layer, filtering incoming traffic to block malicious attacks, and ensuring the security of the the web application?
#### 🧪 Process
This would be a firewall. Specifically a `web application firewall`.

Trying this as the answer
#### ✅ Answer 3
- `Web Application Firewall` ✅


## 📘Uniform Resource Locator
### Uniform Resource Locator (URL)
- A URL is a **web address** used to locate online resources (webpages, videos, images, etc.).
- It directs the browser to the correct location on the internet.

### Anatomy of a URL
![](Images/Pasted%20image%2020250613163043.png)
- A URL is made up of multiple parts, each serving a specific function:
	- **Scheme**:
		- Specifies the protocol (e.g., `http`, `https`)
		- `HTTPS` is preferred due to encryption and better security
    - **User**:
	    - Optional login details (e.g., username)
	    - Rarely used now due to security concerns—can expose sensitive info
    - **Host/Domain**:
	    - Identifies the target website (e.g., `example.com`)
	    - Domains must be unique
	    - Look out for typosquatting (slightly altered domains used in phishing)
    - **Port**:
	    - Specifies which service on the server to connect to
	    - Common ports: `80` (HTTP), `443` (HTTPS)
    - **Path**:
	    - Directs to a specific resource or file on the server
	    - Important to restrict access to sensitive paths
    - **Query String**:
	    - Begins with `?` and passes parameters (e.g., search terms)
	    - Must be validated to prevent attacks like injections
	- **Fragment**:
		- Starts with `#` and navigates to a specific section of a webpage
		- Should also be sanitised to avoid security vulnerabilities
- Understanding these components is essential for browsing, development, and secure application design.

### ❓ Question 1 - Which protocol provides encrypted communication to ensure secure data transmission between a web browser and a web server?
#### 🧪 Process
Generally this would be Hyper Text Transfer Protocol Secure. So `https`

Trying this as the answer
#### ✅ Answer 1
- `https` ✅

### ❓ Question 2 - What term describes the practice of registering domain names that are misspelt variations of popular websites to exploit user errors and potentially engage in fraudulent activities?
#### 🧪 Process
The process of registering misspelled domains with a aim of duplicating an official website is `typosquatting`. 

Trying this as the answer
#### ✅ Answer 2
- `typosquatting` ✅

### ❓ Question 3 - What part of a URL is used to pass additional information, such as search terms or form inputs, to the web server?
#### 🧪 Process
I had to look this one again. According to the page it is the `query string`.

Trying this as the answer
#### ✅ Answer 3
- `query string` ✅


## 