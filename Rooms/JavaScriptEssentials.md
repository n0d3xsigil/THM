| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **JavaScript Essentials** |

## Contents
- [Introduction](#introduction)
- [Essential Concepts](#essential-concepts)


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