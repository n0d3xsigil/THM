| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **SQL Fundamentals** |

## Contents
- [Introduction](#introduction)
- [Databases 101](#databases-101)


## Introduction
Databases, the bread and butter of applications the world over. It doesn't matter if it is a HR system, Web application, file scanner, SOC, or any other data driven tool, You will be using a database.

On the offensive side understanding databases can help us with SQL vulnerabilities such as SQL injections which can help us craft queries that will tamper or retrieve data or on the defensive side understanding how to navigate databases to find suspicious activity or implement a more secure database.

What ever the side of the fence you find yourself on, it is essential to know about and understand databases and the most common form, SQL (Structured Query Language).

No Microsoft Access here ;)

### Room Prerequsites
- [Linux Fundamentals](Rooms/LinuxFundamentals.md)


## Databases 101
### Introducing Databases
> Okay, so you’ve been told just how important they are. Now, it's time to understand what they are in the first place. As mentioned in the introduction, databases are so ubiquitous that you very likely interact with systems that are using them. Databases are an organised collection of structured information or data that is easily accessible and can be manipulated or analysed. That data can take many forms, such as user authentication data (such as usernames and passwords), which are stored and checked against when authenticating into an application or site (like TryHackMe, for example), user-generated data on social media (Like Instagram and Facebook) where data such as user posts, comments, likes etc are collected and stored, as well as information such as watch history which is stored by streaming services such as Netflix and used to generate recommendations. 

> I’m sure you get the point: databases are used extensively and can contain many different things. It’s not just massive-scale businesses that use databases. Smaller-scale businesses, when setting up, will almost certainly have to configure a database to store their data. Speaking of kinds of databases, let’s take a look now at what those are.


Databases are a way of structuring data to make it easily accessed, modified, and analysed. 

The data can take many forms such as:
- **Authentication data**: Usernames, Emails, passwords etc
- **User Generated**: Such as blog posts, social media posts, comments etc
- **Telemetries**: Such as watch history, recommendations, system info.

All businesses, large or small, will have established databases or have had to set up databases to store data.

### Different Types of Databases
![](Images/Pasted%20image%2020250616183048.png)
- **Relational databases**
	- Inserted following predefined structure
		- Example (last_name, first_name, username, password, email_address)
	- Data is added to a table
	- Each record is saved as a row in the table.
	- **relationships** can be made between tables
- **Non-relational databases**
	- Data stored in tabular format
	- Great for unstructured data
	- Example
		```json
		 {
		    _id: ObjectId("4556712cd2b2397ce1b47661"),
		    name: { first: "Thomas", last: "Anderson" },
		    date_of_birth: new Date('Sep 2, 1964'),
		    occupation: [ "The One"],
		    steps_taken : NumberLong(4738947387743977493)
		}
		```


### Tables, Rows and Columns
![](Images/Pasted%20image%2020250616183102.png)
As mentioned, relational databases use tables to store data. The fields used will be created when the table is created. See above example.

- **Structure**
	- Table: Stored data
	- Column: defined information to make up a record
	- Row: a number of columns, or fields making up a record.

### Primary and Foreign Keys
![](Images/Pasted%20image%2020250616183110.png)
- **Primary Key**
	- Ensures a unique record
- **Foreign Key**
	- Allows one table to link to another
	- This key is what allows the **relationship** to work

### ❓ Question 1
> What type of database should you consider using if the data you're going to be storing will vary greatly in its format?
#### 🧪 Process
**Non-relational databases** use unstructured data.

Trying this as the answer
#### ✅ Answer
- `Non-relational database` ✅

### ❓ Question 2
> What type of database should you consider using if the data you're going to be storing will reliably be in the same structured format?
#### 🧪 Process
Relational database uses a predefined structure.

Trying this as the answer
#### ✅ Answer
- `Relational database` ✅

### ❓ Question 3
> In our example, once a record of a book is inserted into our "Books" table, it would be represented as a ___ in that table?
#### 🧪 Process
A record would be considered a row.

Trying this as the answer
#### ✅ Answer
- `row` ✅

### ❓ Question 4
> Which type of key provides a link from one table to another?
#### 🧪 Process
A foreign key links a record across multiple tables.

Trying this as the answer
#### ✅ Answer
- `foreign key` ✅

### ❓ Question 5
> which type of key ensures a record is unique within a table?
#### 🧪 Process
A primary key ensures records are unique.

Trying this as the answer
#### ✅ Answer
- `primary key` ✅
