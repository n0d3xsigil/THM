| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **JavaScript Essentials** |

## Contents
- [Introduction](#introduction)
- [Essential Concepts](#essential-concepts)
- [JavaScript Overview](#javascript-overview)
- [Integrating JavaScript in HTML](#integrating-javascript-in-html)


## 📘Introduction
### JavaScript Essentials
- JavaScript (JS) is a scripting language used to add interactivity to websites built with HTML and CSS.
- It enables features like form validation, animations, and button click actions.
- JS is often used in tandem with HTML and is just as important as HTML and CSS in web development.
- This room serves as a beginner-friendly introduction to JS, focusing on:
	- Fundamental JS concepts
	- How attackers can abuse legitimate JS features for malicious purposes
- **Prerequisites:**
	- ❓[Linux Fundamentals Module](LinuxFundamentalsModule.md)
	- ✅[Web Application Basics](WebApplicationBasics.md)
	- ❓[How Websites Work](HowWebsitesWork.md)
- **Learning Objectives:**
	- Learn core JavaScript concepts
	- Understand how to embed JS into HTML
	- Explore misuse of JS dialog functions
	- Discover how to bypass control flow logic
	- Analyse and understand minified JS files


## 📘Essential Concepts
### Variables, Functions & Control Flow
- **Variables** are containers used to store data. Each variable has a name to allow easy reference later.
	- There are **three main ways to declare variables in JS**:
		- `var`: function-scoped
		- `let`: block-scoped
		- `const`: block-scoped, and cannot be reassigned
	- **Data Types** in JS include:
		- `string` – text (e.g., "hello")
		- `number` – numeric values (e.g., 42)
		- `boolean` – true/false values
		- `null` – an intentional absence of value
		- `undefined` – a variable declared but not assigned
		- `object` – used for complex structures like arrays or key-value pairs
	- **Functions** group reusable blocks of code to perform a specific task.
		- Example: `PrintResult(rollNum)` displays a student's result based on roll number.
		- Functions accept parameters and help avoid code repetition.
			``` javascript
			javascriptfunction PrintResult(rollNum) {
				alert("Username with roll number " + rollNum + " has passed the exam");
				}
			```
    - **Loops** are used to repeat a task as long as a condition is met.
	    - Common loop types: `for`, `while`, `do...while`
	    - Ideal for iterating over arrays or repeating logic without duplicating code.
	    ```javascript
	    for (let i = 0; i < 100; i++) {
	        PrintResult(rollNumbers[i]);
	    }
	    ```
	- **Request-Response Cycle** describes the interaction between a client (browser) and a server:
		- The client sends a **request** (e.g., clicking a link or submitting a form)
		- The server sends a **response** (e.g., an HTML page, JSON data, or error code)

### ❓ Question 1
> What term allows you to run a code block multiple times as long as it is a condition?
#### 🧪 Process
It's a loop, you would loop through a block of code multiple times until a condition is met.
#### ✅ Answer
- `loop`


## 📘JavaScript Overview
### Writing our first JavaScript program
- **JavaScript is an interpreted language**, meaning it runs directly in the browser without needing compilation.
- The first sample program introduces several foundational concepts:
    ```javascript
    // Hello, World! program
    console.log("Hello, World!");
    
    // Variable and Data Type
    let age = 25;
    
    // Control Flow Statement
    if (age >= 18) {
        console.log("You are an adult.");
    } else {
        console.log("You are a minor.");
    }
    
    // Function
    function greet(name) {
        console.log("Hello, " + name + "!");
    }
    
    // Calling the function
    greet("Bob");
    ```
- **Concepts demonstrated:**
	- Logging output with `console.log`
	- Defining variables (`let age = 25`)
	- Using conditional statements (`if...else`)
	- Creating and invoking functions
- **Client-side Execution:**
	- JavaScript primarily runs in the browser, making it easy to interact with and inspect via developer tools.
- **Running JavaScript in Chrome Console:**
	- Launch Google Chrome from the VM Desktop.
	- Press `Ctrl + Shift + I` or right-click and choose **Inspect**.
	- Navigate to the **Console** tab to begin writing JavaScript directly.
- **Example: Adding Two Numbers in Console**
    ```javascript
    let x = 5;
    let y = 10;
    let result = x + y;
    console.log("The result is: " + result);
    ```
    - Paste the code into the Console with `Ctrl + V`, then press `Enter`.
    - You should see: `The result is: 15`

### ❓ Question 1
> What is the code output if the value of x is changed to 10?
#### 🧪 Process
I don't need to run the app to see that if you change `5 + 10 = 15` to 10 + 10 the answer would change to 20

The output would then be `The result is: 20`

Trying this as the answer
#### ✅ Answer
- `The result is: 20` ✅

### ❓ Question 2
> Is JavaScript a compiled or interpreted language?
#### 🧪 Process
This is the first line, it is not compiled as it is run as it is. So you don't use a compiler to generate a binary. Therefore it is an interpreted language.

Trying this as my answer.
#### ✅ Answer
- `interpreted` ✅



## 📘Integrating JavaScript in HTML
### Integrating JavaScript
- JavaScript is used alongside HTML and CSS to create dynamic, interactive websites.
- JavaScript does not render content by itself, it manipulates and enhances what’s already defined in HTML.
- **Two primary methods to integrate JavaScript into HTML:**
	- **Internal JavaScript**
	- **External JavaScript**

#### Internal JavaScript
- Internal JS is written **within** the HTML file using `<script>` tags.
- It can be placed in the `<head>` (loaded before content) or `<body>` (executed with content).
- Ideal for beginners and small projects.

**Example:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>Internal JS</title>
</head>
<body>
    <h1>Addition of Two Numbers</h1>
    <p id="result"></p>

    <script>
        let x = 5;
        let y = 10;
        let result = x + y;
        document.getElementById("result").innerHTML = "The result is: " + result;
    </script>
</body>
</html>
```
- This script:
	- Declares two variables
	- Adds them
	- Updates the `<p>` element with the result using `document.getElementById().innerHTML`
![](Images/Pasted%20image%2020250614095443.png)

#### External JavaScript
- External JS separates the script from HTML for **better readability and maintainability**.
- The JS code is stored in a `.js` file and loaded into HTML via the `src` attribute in the `<script>` tag.
**Example:**
1. Create `script.js` and save file on the Desktop
```javascript
let x = 5;
let y = 10;
let result = x + y;
document.getElementById("result").innerHTML = "The result is: " + result;
```
2. Create `external.html` and save file on the Desktop
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>External JS</title>
</head>
<body>
    <h1>Addition of Two Numbers</h1>
    <p id="result"></p>

    <script src="script.js"></script>
</body>
</html>
```
- The output is the **same** as the internal version, but the JS logic is now cleanly separated.
	![](Images/Pasted%20image%2020250614095912.png)

#### Verifying Internal vs. External JavaScript
- Right-click on a webpage and choose **View Page Source** in Chrome.
	- **Internal JS**: Code appears between `<script>` tags _without_ a `src` attribute.
	- **External JS**: `<script>` tag uses the `src` attribute to link a `.js` file.

### ❓ Question 1
> Which type of JavaScript integration places the code directly within the HTML document?
#### 🧪 Process
Directly in the HTML document would be `internal`

Trying this as the answer
#### ✅ Answer
- `internal` ✅

### ❓ Question 2
> Which method is better for reusing JS across multiple web pages?
#### 🧪 Process
If using across multiple pages then surly `external`.

Trying this as the answer
#### ✅ Answer
- `external` ✅

### ❓ Question 3
> What is the name of the external JS file that is being called by **external_test.html**?
#### 🧪 Process
I had to actually look at the exercise files on the desktop. Turns out there is a file called `thme_external.js`.

Trying this as the answer
#### ✅ Answer
- `thm_external.js` ✅

### ❓ Question 4
> What attribute links an external JS file in the `<script>` tag?
#### 🧪 Process
To include an external script you would use `<script src=...>` so lets assume that we just want `src` as our answer. 

Trying this as the answer
#### ✅ Answer
- `src` ✅
