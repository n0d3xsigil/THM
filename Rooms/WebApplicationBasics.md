| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **Web Application Basics** |

## Contents
- [Introduction](#introduction)
- [Web Application Overview](#web-application-overview)
- [Uniform Resource Locator](#uniform-resource-locator)
- [HTTP Messages](#http-messages)
- [HTTP Request: Request Line and Methods](#http-request-request-line-and-methods)
- [HTTP Request: Headers and Body](#http-request-headers-and-body)


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


## 📘HTTP Messages
### HTTP Messages
- HTTP messages are **data packets** sent between a **client** (user) and a **web server** (host).
- They are key to understanding how web applications communicate.

### Two Types of HTTP Messages
- **HTTP Requests**:
	- Sent by the client to initiate actions (e.g., logging in, fetching data)
- **HTTP Responses**:
	- Sent by the server in reply to the client’s request (e.g., confirmation, webpage content)

### Structure of an HTTP Message
- **Start Line**:
	- Indicates the message type (request or response)
	- Provides essential context like HTTP method (e.g., `GET`, `POST`) or status code (`200 OK`, `404 Not Found`)
- **Headers**:
	- Key-value pairs that carry metadata (content type, cookies, security policies)
	- Guide both client and server in processing the message
- **Empty Line**:
	- Separates the headers from the body
	- Acts as a required delimiter to avoid misinterpretation
- **Body**:
	- Contains the main data
	- In a request: may include form inputs or file uploads
	- In a response: includes the requested content like HTML, JSON, etc.

### Why HTTP Messages Matter
- **Foundation** of client-server communication in web applications
- Helps with **diagnosing issues**, ensuring smoother performance
- Crucial for **security**—understanding structure helps protect data in transit

### ❓ Question 1 - Which HTTP message is returned by the web server after processing a client's request?
#### 🧪 Process
 When you perform a request you get a response. So the answer would be `HTTP Response`

Trying this as the answer
#### ✅ Answer 1
- `HTTP Response` ✅

### ❓ Question 2 - What follows the headers in an HTTP message?
#### 🧪 Process
 After the headers the response leaves an empty line to delineate misinterpretation 

Trying this as the answer
#### ✅ Answer 2
- `Empty line` ✅

## 📘HTTP Request: Request Line and Methods
### HTTP Requests
![](Images/Pasted%20image%2020250613171008.png)
- An HTTP request is how a **user interacts** with a web server, triggering actions within a web application.
- These are often the **first point of contact**, making them crucial for both functionality and security.

### Request Line
- The first line of an HTTP request includes:
	- **HTTP Method** (e.g. `GET`, `POST`)
	- **URL Path** (e.g. `/login`)
	- **HTTP Version** (e.g. `HTTP/1.1`)
- **Example**: `METHOD /path HTTP/version`

### Common HTTP Methods and Security Considerations
- **GET**:
	- Retrieves data without making changes
	- Avoid exposing sensitive info (tokens, passwords) in the URL
- **POST**:
	- Sends data to the server (form submission)
	- Validate and sanitise inputs to prevent SQL injection, XSS
- **PUT**:
	- Replaces or updates resources
	- Require proper authorisation checks
- **DELETE**:
	- Removes resources
	- Also requires strict access controls

### Other HTTP Methods
- **PATCH**:
	- Partial updates to resources
	- Validate input to maintain data integrity
- **HEAD**:
	- Like GET but retrieves only headers
- **OPTIONS**:
	- Lists allowed methods for a resource
	- Can assist in method discovery
- **TRACE**:
	- Echoes the received request
	- Often disabled due to security risks
- **CONNECT**:
	- Used to establish encrypted (HTTPS) connections

### URL Path
- Specifies the **resource location** on the server.
	- Example: `/api/users/123` identifies a user by ID
- Security best practices:
	- **Validate** inputs to avoid unauthorised access
	- **Sanitise** paths to prevent injection attacks
	- **Protect** sensitive endpoints through proper access control

### HTTP Versions
- **HTTP/0.9** (1991):
	- Basic, only supported GET
- **HTTP/1.0** (1996):
	- Introduced headers and MIME support
- **HTTP/1.1** (1997):
	- Persistent connections, chunked encoding, better caching
	- Still widely used
- **HTTP/2** (2015):
	- Improved performance with multiplexing and header compression
- **HTTP/3** (2022):
	- Built on QUIC, focuses on speed and encryption

- While HTTP/1.1 remains the standard, upgrading to **HTTP/2 or HTTP/3** improves speed and security where supported.

### ❓ Question 1
Which HTTP protocol version became widely adopted and remains the most commonly used version for web communication, known for introducing features like persistent connections and chunked transfer encoding?
#### 🧪 Process
HTTP/1.1 remains the predominant version due to support and compatibility

Trying this as the answer
#### ✅ Answer 1 - 
- `http/1.1` ✅


### ❓ Question 2
Which HTTP request method describes the communication options for the target resource, allowing clients to determine which HTTP methods are supported by the web server?
#### 🧪 Process
Options allows listing of allowed methods for a resource

Trying this as the answer
#### ✅ Answer 2
- `Options` ✅


### ❓ Question 3
In an HTTP request, which component specifies the specific resource or endpoint on the web server that the client is requesting, typically appearing after the domain name in the URL?
#### 🧪 Process
> The **URL path** tells the server where to find the resource the user is asking for

Trying this as the answer
#### ✅ Answer 3
- `URL Path` ✅

## 📘HTTP Request: Headers and Body
### Request Headers
- HTTP **request headers** send additional information from the client to the server.
- They provide context about the request, such as the source, data type, or user preferences.

### Common Request Headers
- **Host**: (`Host: tryhackme.com`)
	- Specifies the destination web server
- **User-Agent**: (`User-Agent: Mozilla/5.0`)
	- Identifies the client’s browser or tool making the request
- **Referer**: (`Referer: https://www.google.com/`)
	- Indicates the page where the request originated
- **Cookie**: (`Cookie: user_type=student; room=introtowebapplication; room_status=in_progress`)
	- Stores data set by the server, such as session information or user preferences
- **Content-Type**: (`Content-Type: application/json`)
	- Describes the format of the data in the body of the request

### Request Body
- Used in methods like **POST** and **PUT** to send data to the server.
- Several common data formats are used:

- **URL Encoded** (`application/x-www-form-urlencoded`):
	- Data is sent as `key=value` pairs, joined by `&`
	- Spaces and special characters are percent-encoded
	- **Example**: `name=Aleksandra&age=27&country=US`
- **Form Data** (`multipart/form-data`):
	- Data is sent in separate blocks, each separated by a boundary string
	- Suitable for **file uploads** or binary data
		- Example:
			- Username in plain text
			- Profile picture sent as binary image data
- **JSON** (`application/json`):
	- Data is structured using JavaScript Object Notation
	- Readable, widely used in APIs
	- Example:
	    ```json
	    {
		    "name": "Aleksandra",
		    "age": 27,
		    "country": "US"
		}
		```
- **XML** (`application/xml`):
	- Uses nested **tags** to define and organise data
	- Example:
		```xml
		<user>
			<name>Aleksandra</name>
			<age>27</age>
			<country>US</country>
		</user>
		```
- Choosing the correct format depends on the use case and what the server expects to receive.

### ❓ Question 1 - Which HTTP request header specifies the domain name of the web server to which the request is being sent?
#### 🧪 Process
`Host` specifies the destination web server

Trying this as the answer
#### ✅ Answer 1
- `Host` ✅

### ❓ Question 2 - What is the default content type for form submissions in an HTTP request where the data is encoded as key=value pairs in a query string format?
#### 🧪 Process
This would be URL encoded or `application/x-www-form-urlencoded`

Trying this as the answer
#### ✅ Answer 2
- `application/x-www-form-urlencoded` ✅

### ❓ Question 3 - Which part of an HTTP request contains additional information like host, user agent, and content type, guiding how the web server should process the request?
#### 🧪 Process
> **Request Headers** allow extra information to be conveyed to the web server about the request.

Trying this as the answer
#### ✅ Answer 3
- `Request headers` ✅


## .