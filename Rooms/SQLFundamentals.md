| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **SQL Fundamentals** |

## Contents
- [Introduction](#introduction)
- [Databases 101](#databases-101)
- [SQL](#sql)
- [Database and Table Statements](#database-and-table-statements)
- [CRUD Operations](#crud-operations)
- [Clauses](#clauses)
- [Operators](#operators)
- [Functions](#functions)


## 📘Introduction
Databases, the bread and butter of applications the world over. It doesn't matter if it is a HR system, Web application, file scanner, SOC, or any other data driven tool, You will be using a database.

On the offensive side understanding databases can help us with SQL vulnerabilities such as SQL injections which can help us craft queries that will tamper or retrieve data or on the defensive side understanding how to navigate databases to find suspicious activity or implement a more secure database.

What ever the side of the fence you find yourself on, it is essential to know about and understand databases and the most common form, SQL (Structured Query Language).

No Microsoft Access here ;)

### Room Prerequsites
- [Linux Fundamentals](Rooms/LinuxFundamentals.md)


## 📘Databases 101
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


## 📘SQL
### What is SQL?
![](Images/Pasted%20image%2020250616191408.png)
- Usually controlled by DMBS (**D**ata**b**ase **M**anagement **S**ystem)
	- Acts as link between user and database
- Examples of DBMS
	- MySQL
	- MongoDB
	- Oracle Database
	- Maria DB
- SQL (**S**tructured **Q**uery **L**anguage)
	- A Programming Language
	- Used to Query, define and manipulate data stored in a relational database.

### The Benefits of SQL and Relational Databases
- Fast
- Easy to learn
- Reliable
- Flexible
### Getting Hands ON
No need. I'm happy with this.

### ❓ Question 1
> What serves as an interface between a database and an end user?
#### 🧪 Process
A DBMS acts as an interface between user and DB.

Trying this as the answer.
#### ✅ Answer
- `DBMS` ✅

### ❓ Question 2
> What query language can be used to interact with a relational database?
#### 🧪 Process
Structured Query Language or SQL.

Trying this as the answer
#### ✅ Answer
- `SQL` ✅


## 📘Database and Table Statements
### Time to Learn


### Database Statements
#### `CREATE DATABASE`
```sql
mysql> CREATE DATABASE thm_bookmarket_db;
Query OK, 1 row affected (0.01 sec)
```

#### `SHOW DATABASES`
```sql
mysql> SHOW DATABASES;
+-----------------------------------------------+
| Database                                      |
+-----------------------------------------------+
| THM{575a947132312f97b30ee5aeebba629b723d30f9} |
| information_schema                            |
| mysql                                         |
| performance_schema                            |
| sys                                           |
| task_4_db                                     |
| thm_bookmarket_db                             |
| thm_books                                     |
| thm_books2                                    |
| tools_db                                      |
+-----------------------------------------------+
10 rows in set (0.01 sec)
```

#### `USE DATABASE`
```sql
mysql> USE thm_bookmarket_db;
Database changed
```

#### `DROP DATABASE`
```sql
mysql> DROP database thm_bookmarket_db;
Query OK, 0 rows affected (0.02 sec)
```

### Table Statements
### `CREATE TABLE`
```sql
mysql> CREATE TABLE book_inventory (
    -> book_id INT AUTO_INCREMENT PRIMARY KEY,
    -> book_name VARCHAR(255) NOT NULL,
    -> publication_date DATE
    -> );
Query OK, 0 rows affected (0.05 sec)
```
- **Table Name**: `book_inventory`
- **Columns**:
	- `book_id`
	- `book_name`
	- `book_id`
- `INT` - Only numbers
- `AUTO_INCREMENT` means increment the number, 1 at a time starting at 1
- `PRIMARY KEY` sets this column as the primary key. The number will be unique
- `VARCHAR(255)` sets the limit of characters to 255.
- `NOT NULL` Can't be empty
- `DATE` sets the field to date format.

### `SHOW TABLES `
```sql
mysql> SHOW TABLES;
+-----------------------------+
| Tables_in_thm_bookmarket_db |
+-----------------------------+
| book_inventory              |
+-----------------------------+
1 row in set (0.00 sec)
```

### `DESCRIBE `
```sql
mysql> DESCRIBE book_inventory;
+------------------+--------------+------+-----+---------+----------------+
| Field            | Type         | Null | Key | Default | Extra          |
+------------------+--------------+------+-----+---------+----------------+
| book_id          | int          | NO   | PRI | NULL    | auto_increment |
| book_name        | varchar(255) | NO   |     | NULL    |                |
| publication_date | date         | YES  |     | NULL    |                |
+------------------+--------------+------+-----+---------+----------------+
3 rows in set (0.01 sec)
```

### `ALTER`
```sql
mysql> ALTER TABLE book_inventory
    -> ADD page_count INT;
Query OK, 0 rows affected (0.05 sec)
Records: 0  Duplicates: 0  Warnings: 0
```

### `DROP`
```sql
mysql> DROP TABLE table_name;
```


### ❓ Question
> Using the statement you've learned to list all databases, it should reveal a database with a flag for a name; what is it?
#### 🧪 Process
```sql
mysql> SHOW DATABASES;
+-----------------------------------------------+
| Database                                      |
+-----------------------------------------------+
| THM{575a947132312f97b30ee5aeebba629b723d30f9} |
```

Trying this as the answer
#### ✅ Answer
- `THM{575a947132312f97b30ee5aeebba629b723d30f9}` ✅

### ❓ Question
> In the list of available databases, you should also see the  `task_4_db` database. Set this as your active database and list all tables in this database; what is the flag present here?
#### 🧪 Process
```sql
mysql> SHOW DATABASES;
+-----------------------------------------------+
| Database                                      |
+-----------------------------------------------+
| THM{575a947132312f97b30ee5aeebba629b723d30f9} |
| information_schema                            |
| mysql                                         |
| performance_schema                            |
| sys                                           |
| task_4_db                                     |
| thm_bookmarket_db                             |
| thm_books                                     |
| thm_books2                                    |
| tools_db                                      |
+-----------------------------------------------+
10 rows in set (0.00 sec)

mysql> USE task_4_db;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> SHOW TABLES;
+-----------------------------------------------+
| Tables_in_task_4_db                           |
+-----------------------------------------------+
| THM{692aa7eaec2a2a827f4d1a8bed1f90e5e49d2410} |
+-----------------------------------------------+
1 row in set (0.00 sec)
```

Trying this as the answer
#### ✅ Answer
- `THM{692aa7eaec2a2a827f4d1a8bed1f90e5e49d2410} ` ✅


## CRUD Operations
### CRUD
Acronym:
- **C**reate
- **R**ead
- **U**pdate
- **D**elete

### Create Operation (INSERT)
Lets validate what is in the `books` table by using `SELECT * FROM books;`
```sql
mysql> SELECT * FROM books;
+----+----------------------------+----------------+--------------------------------------------------------+
| id | name                       | published_date | description                                            |
+----+----------------------------+----------------+--------------------------------------------------------+
|  1 | Android Security Internals | 2014-10-14     | An In-Depth Guide to Android's Security Architecture   |
|  2 | Bug Bounty Bootcamp        | 2021-11-16     | The Guide to Finding and Reporting Web Vulnerabilities |
|  3 | Car Hacker's Handbook      | 2016-02-25     | A Guide for the Penetration Tester                     |
|  4 | Designing Secure Software  | 2021-12-21     | A Guide for Developers                                 |
|  5 | Ethical Hacking            | 2021-11-02     | A Hands-on Introduction to Breaking In                 |
|  6 | Ethical Hacking            | 2021-11-02     |                                                        |
+----+----------------------------+----------------+--------------------------------------------------------+
6 rows in set (0.00 sec)
```
Use `INSERT INTO` to **create** a record

The data we want to insert to the database is as follows:
- id = `7`
- name = `"Android Security Internals revised"`
- published_date = `"2025-10-14"`
- description = `"An In-Depth Guide to Android's Security Architecture (revised)"`

```sql
mysql> INSERT INTO books (id, name, published_date, description)
    ->     VALUES (7, "Android Security Internals revised", "2025-10-14", "An In-Depth Guide to Android's Security Architecture (revised)");
Query OK, 1 row affected (0.01 sec)
```

We can re run the `SELECT * FROM books;` query again to validate the entry

```sql
mysql> SELECT * FROM books;
+----+------------------------------------+----------------+----------------------------------------------------------------+
| id | name                               | published_date | description                                                    |
+----+------------------------------------+----------------+----------------------------------------------------------------+
|  1 | Android Security Internals         | 2014-10-14     | An In-Depth Guide to Android's Security Architecture           |
|  2 | Bug Bounty Bootcamp                | 2021-11-16     | The Guide to Finding and Reporting Web Vulnerabilities         |
|  3 | Car Hacker's Handbook              | 2016-02-25     | A Guide for the Penetration Tester                             |
|  4 | Designing Secure Software          | 2021-12-21     | A Guide for Developers                                         |
|  5 | Ethical Hacking                    | 2021-11-02     | A Hands-on Introduction to Breaking In                         |
|  6 | Ethical Hacking                    | 2021-11-02     |                                                                |
|  7 | Android Security Internals revised | 2025-10-14     | An In-Depth Guide to Android's Security Architecture (revised) |
+----+------------------------------------+----------------+----------------------------------------------------------------+
7 rows in set (0.00 sec)
```

### Read Operation (SELECT)
We have already used the `SELECT` function to read a table. But lets re-cap it here
```sql
mysql> SELECT * FROM books;
+----+------------------------------------+----------------+----------------------------------------------------------------+
| id | name                               | published_date | description                                                    |
+----+------------------------------------+----------------+----------------------------------------------------------------+
|  1 | Android Security Internals         | 2014-10-14     | An In-Depth Guide to Android's Security Architecture           |
|  2 | Bug Bounty Bootcamp                | 2021-11-16     | The Guide to Finding and Reporting Web Vulnerabilities         |
|  3 | Car Hacker's Handbook              | 2016-02-25     | A Guide for the Penetration Tester                             |
|  4 | Designing Secure Software          | 2021-12-21     | A Guide for Developers                                         |
|  5 | Ethical Hacking                    | 2021-11-02     | A Hands-on Introduction to Breaking In                         |
|  6 | Ethical Hacking                    | 2021-11-02     |                                                                |
|  7 | Android Security Internals revised | 2025-10-14     | An In-Depth Guide to Android's Security Architecture (revised) |
+----+------------------------------------+----------------+----------------------------------------------------------------+
7 rows in set (0.00 sec)
```

Maybe we only want the `name` and `published_date` from the `books` table
```sql
mysql> SELECT name, published_date FROM books;
+------------------------------------+----------------+
| name                               | published_date |
+------------------------------------+----------------+
| Android Security Internals         | 2014-10-14     |
| Bug Bounty Bootcamp                | 2021-11-16     |
| Car Hacker's Handbook              | 2016-02-25     |
| Designing Secure Software          | 2021-12-21     |
| Ethical Hacking                    | 2021-11-02     |
| Ethical Hacking                    | 2021-11-02     |
| Android Security Internals revised | 2025-10-14     |
+------------------------------------+----------------+
7 rows in set (0.00 sec)
```

### Update Operation (UPDATE)
How about updating an existing record? 

Let's set the the _Android Security Internals revised_ description to "A revised guide to the best seller"
```sql
mysql> UPDATE books
    -> SET description = "A revised guide to the best seller"
    -> WHERE id = 7;
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```

It looks good but let's validate the change was sucessfull.
```sql
mysql> SELECT * FROM books WHERE id = 7;
+----+------------------------------------+----------------+------------------------------------+
| id | name                               | published_date | description                        |
+----+------------------------------------+----------------+------------------------------------+
|  7 | Android Security Internals revised | 2025-10-14     | A revised guide to the best seller |
+----+------------------------------------+----------------+------------------------------------+
1 row in set (0.00 sec)
```

Perfect!

### Delete Operation (DELETE)
I've noticed there is a duplicate book, but one without a description
```sql
mysql> SELECT * FROM books;
+----+------------------------------------+----------------+--------------------------------------------------------+
| id | name                               | published_date | description                                            |
+----+------------------------------------+----------------+--------------------------------------------------------+
|  1 | Android Security Internals         | 2014-10-14     | An In-Depth Guide to Android's Security Architecture   |
|  2 | Bug Bounty Bootcamp                | 2021-11-16     | The Guide to Finding and Reporting Web Vulnerabilities |
|  3 | Car Hacker's Handbook              | 2016-02-25     | A Guide for the Penetration Tester                     |
|  4 | Designing Secure Software          | 2021-12-21     | A Guide for Developers                                 |
|  5 | Ethical Hacking                    | 2021-11-02     | A Hands-on Introduction to Breaking In                 |
|  6 | Ethical Hacking                    | 2021-11-02     |                                                        |
|  7 | Android Security Internals revised | 2025-10-14     | A revised guide to the best seller                     |
+----+------------------------------------+----------------+--------------------------------------------------------+
7 rows in set (0.00 sec)
```

It looks like `id 6` is the duplicate. Let's delete it.
```sql
mysql> DELETE FROM books WHERE id = 6;
Query OK, 1 row affected (0.01 sec)
```

Let's validate once again. 
```sql
mysql> SELECT * FROM books;
+----+------------------------------------+----------------+--------------------------------------------------------+
| id | name                               | published_date | description                                            |
+----+------------------------------------+----------------+--------------------------------------------------------+
|  1 | Android Security Internals         | 2014-10-14     | An In-Depth Guide to Android's Security Architecture   |
|  2 | Bug Bounty Bootcamp                | 2021-11-16     | The Guide to Finding and Reporting Web Vulnerabilities |
|  3 | Car Hacker's Handbook              | 2016-02-25     | A Guide for the Penetration Tester                     |
|  4 | Designing Secure Software          | 2021-12-21     | A Guide for Developers                                 |
|  5 | Ethical Hacking                    | 2021-11-02     | A Hands-on Introduction to Breaking In                 |
|  7 | Android Security Internals revised | 2025-10-14     | A revised guide to the best seller                     |
+----+------------------------------------+----------------+--------------------------------------------------------+
6 rows in set (0.00 sec)
```

### Summary
- `INSERT` to **C**reate
- `SELECT` to **R**ead
- `UPDATE` to **U**pdate
- `DELETE` to **D**elete


### ❓ Question
> Using the `tools_db` database, what is the name of the tool in the `hacking_tools` table that can be used to perform man-in-the-middle attacks on wireless networks?
#### 🧪 Process
Lets first find what databases are available
```sql
mysql> SHOW DATABASES;
+-----------------------------------------------+
| Database                                      |
+-----------------------------------------------+
| THM{575a947132312f97b30ee5aeebba629b723d30f9} |
| information_schema                            |
| mysql                                         |
| performance_schema                            |
| sys                                           |
| task_4_db                                     |
| thm_books                                     |
| thm_books2                                    |
| tools_db                                      |
+-----------------------------------------------+
9 rows in set (0.00 sec)
```

Okay, so we where told to use `tools_db` and that is there at the bottom. So lets `USE` the `tools_db`
```sql
mysql> USE tools_db;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
```

Now we want to `SHOW` the tables, we're looking for `hacking_tools`.
```sql
mysql> SHOW TABLES;
+--------------------+
| Tables_in_tools_db |
+--------------------+
| hacking_tools      |
+--------------------+
1 row in set (0.00 sec)
```

Well that was easy, only one result. Now we want to `SELECT` the contents to view it.
```sql
mysql> SELECT * FROM hacking_tools;
+----+------------------+----------------------+-------------------------------------------------------------------------+--------+
| id | name             | category             | description                                                             | amount |
+----+------------------+----------------------+-------------------------------------------------------------------------+--------+
|  1 | Flipper Zero     | Multi-tool           | A portable multi-tool for pentesters and geeks in a toy-like form       |    169 |
|  2 | O.MG cables      | Cable-based attacks  | Malicious USB cables that can be used for remote attacks and testing    |    180 |
|  3 | Wi-Fi Pineapple  | Wi-Fi hacking        | A device used to perform man-in-the-middle attacks on wireless networks |    140 |
|  4 | USB Rubber Ducky | USB attacks          | A USB keystroke injection tool disguised as a flash drive               |     80 |
|  5 | iCopy-XS         | RFID cloning         | A tool used for reading and cloning RFID cards for security testing     |    375 |
|  6 | Lan Turtle       | Network intelligence | A covert tool for remote access and network intelligence gathering      |     80 |
|  7 | Bash Bunny       | USB attacks          | A multi-function USB attack device for penetration testers              |    120 |
|  8 | Proxmark 3 RDV4  | RFID cloning         | A powerful RFID tool for reading, writing, and analyzing RFID tags      |    300 |
+----+------------------+----------------------+-------------------------------------------------------------------------+--------+
8 rows in set (0.00 sec)
```

We're looking for a _Man-In-The-Middle_ tool. We can see in the description of the `Wi-Fi Pineapple` that it is "A device used to perform man-in-the-middle attacks on wireless networks".

We have our answer.

Trying this as the answer

#### ✅ Answer
- `Wi-Fi Pineapple` ✅

### ❓ Question
> Using the `tools_db` database, what is the shared category for both **USB Rubber Ducky** and **Bash Bunny**?
#### 🧪 Process
I already know that both **USB Rubber Duck** and **Bash Buhhy** are `USB Attacks`

Trying this as the answer
#### ✅ Answer
- `USB Attacks` ✅


## Clauses
A clasue is a way of refining how we search for data. It is used in conjunction with a statment.

Focusing on:
- `DISTINCT`
- `GROUP BY`
- `ORDER BY`
- `HAVING`

First we should log in to sql and enter our password

```shell
ubuntu@tryhackme:~$ mysql -u root -p
Enter password: tryhackme
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 10
Server version: 8.0.39-0ubuntu0.20.04.1 (Ubuntu)

Copyright (c) 2000, 2024, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```

We want to `USE` the database `thm_books`.
```sql
mysql> USE thm_books;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
```

### DISTINCT Clause
Before distinct clause
```sql
mysql> SELECT * FROM books;
+----+----------------------------+----------------+--------------------------------------------------------+
| id | name                       | published_date | description                                            |
+----+----------------------------+----------------+--------------------------------------------------------+
|  1 | Android Security Internals | 2014-10-14     | An In-Depth Guide to Android's Security Architecture   |
|  2 | Bug Bounty Bootcamp        | 2021-11-16     | The Guide to Finding and Reporting Web Vulnerabilities |
|  3 | Car Hacker's Handbook      | 2016-02-25     | A Guide for the Penetration Tester                     |
|  4 | Designing Secure Software  | 2021-12-21     | A Guide for Developers                                 |
|  5 | Ethical Hacking            | 2021-11-02     | A Hands-on Introduction to Breaking In                 |
|  6 | Ethical Hacking            | 2021-11-02     |                                                        |
+----+----------------------------+----------------+--------------------------------------------------------+
6 rows in set (0.00 sec)
```

Using distinct clause
```sql
mysql> SELECT DISTINCT name FROM books;
+----------------------------+
| name                       |
+----------------------------+
| Android Security Internals |
| Bug Bounty Bootcamp        |
| Car Hacker's Handbook      |
| Designing Secure Software  |
| Ethical Hacking            |
+----------------------------+
5 rows in set (0.01 sec)
```

### GROUP BY Clause
Group allows us to group results
```sql
mysql> SELECT name, COUNT(*) 
    -> FROM books
    -> GROUP BY name;
+----------------------------+----------+
| name                       | COUNT(*) |
+----------------------------+----------+
| Android Security Internals |        1 |
| Bug Bounty Bootcamp        |        1 |
| Car Hacker's Handbook      |        1 |
| Designing Secure Software  |        1 |
| Ethical Hacking            |        2 |
+----------------------------+----------+
5 rows in set (0.00 sec)
```

### ORDER BY Clause
Using `ORDER BY` allows you to sort on a specific field.
Before sort
```sql
mysql> SELECT * FROM books;
+----+----------------------------+----------------+--------------------------------------------------------+
| id | name                       | published_date | description                                            |
+----+----------------------------+----------------+--------------------------------------------------------+
|  1 | Android Security Internals | 2014-10-14     | An In-Depth Guide to Android's Security Architecture   |
|  2 | Bug Bounty Bootcamp        | 2021-11-16     | The Guide to Finding and Reporting Web Vulnerabilities |
|  3 | Car Hacker's Handbook      | 2016-02-25     | A Guide for the Penetration Tester                     |
|  4 | Designing Secure Software  | 2021-12-21     | A Guide for Developers                                 |
|  5 | Ethical Hacking            | 2021-11-02     | A Hands-on Introduction to Breaking In                 |
|  6 | Ethical Hacking            | 2021-11-02     |                                                        |
+----+----------------------------+----------------+--------------------------------------------------------+
6 rows in set (0.00 sec)
```

`ORDER BY` name in Ascending order
```sql
mysql> SELECT *
    -> FROM books
    -> ORDER BY name ASC;
+----+----------------------------+----------------+--------------------------------------------------------+
| id | name                       | published_date | description                                            |
+----+----------------------------+----------------+--------------------------------------------------------+
|  1 | Android Security Internals | 2014-10-14     | An In-Depth Guide to Android's Security Architecture   |
|  2 | Bug Bounty Bootcamp        | 2021-11-16     | The Guide to Finding and Reporting Web Vulnerabilities |
|  3 | Car Hacker's Handbook      | 2016-02-25     | A Guide for the Penetration Tester                     |
|  4 | Designing Secure Software  | 2021-12-21     | A Guide for Developers                                 |
|  5 | Ethical Hacking            | 2021-11-02     | A Hands-on Introduction to Breaking In                 |
|  6 | Ethical Hacking            | 2021-11-02     |                                                        |
+----+----------------------------+----------------+--------------------------------------------------------+
6 rows in set (0.00 sec)
```

Or by Descending order
```sql
mysql> SELECT *
    -> FROM books
    -> ORDER BY name DESC;
+----+----------------------------+----------------+--------------------------------------------------------+
| id | name                       | published_date | description                                            |
+----+----------------------------+----------------+--------------------------------------------------------+
|  5 | Ethical Hacking            | 2021-11-02     | A Hands-on Introduction to Breaking In                 |
|  6 | Ethical Hacking            | 2021-11-02     |                                                        |
|  4 | Designing Secure Software  | 2021-12-21     | A Guide for Developers                                 |
|  3 | Car Hacker's Handbook      | 2016-02-25     | A Guide for the Penetration Tester                     |
|  2 | Bug Bounty Bootcamp        | 2021-11-16     | The Guide to Finding and Reporting Web Vulnerabilities |
|  1 | Android Security Internals | 2014-10-14     | An In-Depth Guide to Android's Security Architecture   |
+----+----------------------------+----------------+--------------------------------------------------------+
6 rows in set (0.00 sec)
```

### HAVING Clause
The `HAVING` clause is a bit like an if statment.
```sql
mysql> SELECT name, COUNT(*)
    -> FROM books
    -> GROUP BY name
    -> HAVING name LIKE '%Hack%';
+-----------------------+----------+
| name                  | COUNT(*) |
+-----------------------+----------+
| Car Hacker's Handbook |        1 |
| Ethical Hacking       |        2 |
+-----------------------+----------+
2 rows in set (0.00 sec)
```

### ❓ Question
> Using the `tools_db` database, what is the total number of distinct categories in the `hacking_tools` table?
#### 🧪 Process
First Change the DB to `tools_db`
```sql
mysql> USE tools_db
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
```

Then get a list of tables withing `tools_db`
```sql
mysql> SHOW TABLES; 
+--------------------+
| Tables_in_tools_db |
+--------------------+
| hacking_tools      |
+--------------------+
1 row in set (0.00 sec)
```

Next, get the contents to idenfity the fields.
```sql
mysql> SELECT * FROM hacking_tools;
+----+------------------+----------------------+-------------------------------------------------------------------------+--------+
| id | name             | category             | description                                                             | amount |
+----+------------------+----------------------+-------------------------------------------------------------------------+--------+
|  1 | Flipper Zero     | Multi-tool           | A portable multi-tool for pentesters and geeks in a toy-like form       |    169 |
|  2 | O.MG cables      | Cable-based attacks  | Malicious USB cables that can be used for remote attacks and testing    |    180 |
|  3 | Wi-Fi Pineapple  | Wi-Fi hacking        | A device used to perform man-in-the-middle attacks on wireless networks |    140 |
|  4 | USB Rubber Ducky | USB attacks          | A USB keystroke injection tool disguised as a flash drive               |     80 |
|  5 | iCopy-XS         | RFID cloning         | A tool used for reading and cloning RFID cards for security testing     |    375 |
|  6 | Lan Turtle       | Network intelligence | A covert tool for remote access and network intelligence gathering      |     80 |
|  7 | Bash Bunny       | USB attacks          | A multi-function USB attack device for penetration testers              |    120 |
|  8 | Proxmark 3 RDV4  | RFID cloning         | A powerful RFID tool for reading, writing, and analyzing RFID tags      |    300 |
+----+------------------+----------------------+-------------------------------------------------------------------------+--------+
8 rows in set (0.00 sec)
```

Let's get a distinct list of records within `category`
```sql
mysql> SELECT DISTINCT name FROM hacking_tools;
+------------------+
| name             |
+------------------+
| Flipper Zero     |
| O.MG cables      |
| Wi-Fi Pineapple  |
| USB Rubber Ducky |
| iCopy-XS         |
| Lan Turtle       |
| Bash Bunny       |
| Proxmark 3 RDV4  |
+------------------+
8 rows in set (0.00 sec)
```

Since there `8` rows, that will mean there are 8 distinct categories in the `hacking_tools` table.

Trying this as the answer

#### ✅ Answer
- `8` ❌

Oh, I see, I used the field `name` rather than `category`. Lets try that again...

```sql
mysql> SELECT DISTINCT category FROM hacking_tools;
+----------------------+
| category             |
+----------------------+
| Multi-tool           |
| Cable-based attacks  |
| Wi-Fi hacking        |
| USB attacks          |
| RFID cloning         |
| Network intelligence |
+----------------------+
6 rows in set (0.00 sec)
```

This time we have 6 rows / results. That must be the answer right ;)

Trying this as the correct answer
- `6`✅

### ❓ Question
> Using the `tools_db` database, what is the first tool (by name) in ascending order from the `hacking_tools` table?
#### 🧪 Process
We can use `ORDER BY` to sort the results from the `SELECT` statement.
```sql
mysql> SELECT * FROM hacking_tools
    -> ORDER BY name;
+----+------------------+----------------------+-------------------------------------------------------------------------+--------+
| id | name             | category             | description                                                             | amount |
+----+------------------+----------------------+-------------------------------------------------------------------------+--------+
|  7 | Bash Bunny       | USB attacks          | A multi-function USB attack device for penetration testers              |    120 |
|  1 | Flipper Zero     | Multi-tool           | A portable multi-tool for pentesters and geeks in a toy-like form       |    169 |
|  5 | iCopy-XS         | RFID cloning         | A tool used for reading and cloning RFID cards for security testing     |    375 |
|  6 | Lan Turtle       | Network intelligence | A covert tool for remote access and network intelligence gathering      |     80 |
|  2 | O.MG cables      | Cable-based attacks  | Malicious USB cables that can be used for remote attacks and testing    |    180 |
|  8 | Proxmark 3 RDV4  | RFID cloning         | A powerful RFID tool for reading, writing, and analyzing RFID tags      |    300 |
|  4 | USB Rubber Ducky | USB attacks          | A USB keystroke injection tool disguised as a flash drive               |     80 |
|  3 | Wi-Fi Pineapple  | Wi-Fi hacking        | A device used to perform man-in-the-middle attacks on wireless networks |    140 |
+----+------------------+----------------------+-------------------------------------------------------------------------+--------+
8 rows in set (0.00 sec)
```

The first result when sorted is `Bash Bunny`.

Trying this as the answer
#### ✅ Answer
- `Bash Bunny` ✅

### ❓ Question
> Using the `tools_db` database, what is the first tool (by name) in descending order from the `hacking_tools` table?
#### 🧪 Process
We can use `ORDER BY` again, this time suffixing the statement with `DESC`
```sql
mysql> SELECT * FROM hacking_tools
    -> ORDER BY name DESC;
+----+------------------+----------------------+-------------------------------------------------------------------------+--------+
| id | name             | category             | description                                                             | amount |
+----+------------------+----------------------+-------------------------------------------------------------------------+--------+
|  3 | Wi-Fi Pineapple  | Wi-Fi hacking        | A device used to perform man-in-the-middle attacks on wireless networks |    140 |
|  4 | USB Rubber Ducky | USB attacks          | A USB keystroke injection tool disguised as a flash drive               |     80 |
|  8 | Proxmark 3 RDV4  | RFID cloning         | A powerful RFID tool for reading, writing, and analyzing RFID tags      |    300 |
|  2 | O.MG cables      | Cable-based attacks  | Malicious USB cables that can be used for remote attacks and testing    |    180 |
|  6 | Lan Turtle       | Network intelligence | A covert tool for remote access and network intelligence gathering      |     80 |
|  5 | iCopy-XS         | RFID cloning         | A tool used for reading and cloning RFID cards for security testing     |    375 |
|  1 | Flipper Zero     | Multi-tool           | A portable multi-tool for pentesters and geeks in a toy-like form       |    169 |
|  7 | Bash Bunny       | USB attacks          | A multi-function USB attack device for penetration testers              |    120 |
+----+------------------+----------------------+-------------------------------------------------------------------------+--------+
8 rows in set (0.00 sec)
```
The first result is now `Wi-Fi Pineapple`

Trying this as the answer
#### ✅ Answer
- `Wi-Fi Pineapple` ✅


## Operators
We will use `thm_books2` this time. So lets `USE` that one.
```sql
mysql> SHOW DATABASES;
+-----------------------------------------------+
| Database                                      |
+-----------------------------------------------+
| THM{575a947132312f97b30ee5aeebba629b723d30f9} |
| information_schema                            |
| mysql                                         |
| performance_schema                            |
| sys                                           |
| task_4_db                                     |
| thm_books                                     |
| thm_books2                                    |
| tools_db                                      |
+-----------------------------------------------+
9 rows in set (0.00 sec)

mysql> USE thm_books2;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> SHOW TABLES;
+----------------------+
| Tables_in_thm_books2 |
+----------------------+
| books                |
+----------------------+
1 row in set (0.00 sec)

mysql> SELECT * FROM books;
+----+----------------------------+----------------+--------------------------------------------------------+--------------------+
| id | name                       | published_date | description                                            | category           |
+----+----------------------------+----------------+--------------------------------------------------------+--------------------+
|  1 | Android Security Internals | 2014-10-14     | An In-Depth Guide to Android's Security Architecture   | Defensive Security |
|  2 | Bug Bounty Bootcamp        | 2021-11-16     | The Guide to Finding and Reporting Web Vulnerabilities | Offensive Security |
|  3 | Car Hacker's Handbook      | 2016-02-25     | A Guide for the Penetration Tester                     | Offensive Security |
|  4 | Designing Secure Software  | 2021-12-21     | A Guide for Developers                                 | Defensive Security |
|  5 | Ethical Hacking            | 2021-11-02     | A Hands-on Introduction to Breaking In                 | Offensive Security |
+----+----------------------------+----------------+--------------------------------------------------------+--------------------+
5 rows in set (0.00 sec)
```

### Logical Operators
- Result in `TRUE` or `FALSE`
#### LIKE Operator
Often used in conjunction with `WHERE`
```sql
mysql> SELECT *
    -> FROM books
    -> WHERE description LIKE '%guide%';
+----+----------------------------+----------------+--------------------------------------------------------+--------------------+
| id | name                       | published_date | description                                            | category           |
+----+----------------------------+----------------+--------------------------------------------------------+--------------------+
|  1 | Android Security Internals | 2014-10-14     | An In-Depth Guide to Android's Security Architecture   | Defensive Security |
|  2 | Bug Bounty Bootcamp        | 2021-11-16     | The Guide to Finding and Reporting Web Vulnerabilities | Offensive Security |
|  3 | Car Hacker's Handbook      | 2016-02-25     | A Guide for the Penetration Tester                     | Offensive Security |
|  4 | Designing Secure Software  | 2021-12-21     | A Guide for Developers                                 | Defensive Security |
+----+----------------------------+----------------+--------------------------------------------------------+--------------------+
4 rows in set (0.00 sec)
```

#### AND Operator
Tying multiple conditions together to filter furhter.
```sql
mysql> SELECT * 
    -> FROM books
    -> WHERE category = "Offensive Security" AND name = "Bug Bounty Bootcamp";
+----+---------------------+----------------+--------------------------------------------------------+--------------------+
| id | name                | published_date | description                                            | category           |
+----+---------------------+----------------+--------------------------------------------------------+--------------------+
|  2 | Bug Bounty Bootcamp | 2021-11-16     | The Guide to Finding and Reporting Web Vulnerabilities | Offensive Security |
+----+---------------------+----------------+--------------------------------------------------------+--------------------+
1 row in set (0.00 sec)
```

#### OR Operator
Check for `This` or `That`
```sql
mysql> SELECT * 
    -> FROM books
    -> WHERE name LIKE '%Android%' OR name LIKE '%iOS%';
+----+----------------------------+----------------+------------------------------------------------------+--------------------+
| id | name                       | published_date | description                                          | category           |
+----+----------------------------+----------------+------------------------------------------------------+--------------------+
|  1 | Android Security Internals | 2014-10-14     | An In-Depth Guide to Android's Security Architecture | Defensive Security |
+----+----------------------------+----------------+------------------------------------------------------+--------------------+
1 row in set (0.00 sec)
```

#### NOT Operator
Excludes specific results
```sql
mysql> SELECT * 
    -> FROM books
    -> WHERE NOT description LIKE '%guide%';
+----+-----------------+----------------+----------------------------------------+--------------------+
| id | name            | published_date | description                            | category           |
+----+-----------------+----------------+----------------------------------------+--------------------+
|  5 | Ethical Hacking | 2021-11-02     | A Hands-on Introduction to Breaking In | Offensive Security |
+----+-----------------+----------------+----------------------------------------+--------------------+
1 row in set (0.00 sec)
```

#### BETWEEN Operator
Filter a result between two numbers
```sql
mysql> SELECT * 
    -> FROM books
    -> WHERE id BETWEEN 2 AND 4;
+----+---------------------------+----------------+--------------------------------------------------------+--------------------+
| id | name                      | published_date | description                                            | category           |
+----+---------------------------+----------------+--------------------------------------------------------+--------------------+
|  2 | Bug Bounty Bootcamp       | 2021-11-16     | The Guide to Finding and Reporting Web Vulnerabilities | Offensive Security |
|  3 | Car Hacker's Handbook     | 2016-02-25     | A Guide for the Penetration Tester                     | Offensive Security |
|  4 | Designing Secure Software | 2021-12-21     | A Guide for Developers                                 | Defensive Security |
+----+---------------------------+----------------+--------------------------------------------------------+--------------------+
3 rows in set (0.00 sec)
```

### Comparison Operators
#### Equal To Operator
- `=`

Checks a value is `=`
```sql
mysql> SELECT * 
    -> FROM books
    -> WHERE name = "Designing Secure Software";
+----+---------------------------+----------------+------------------------+--------------------+
| id | name                      | published_date | description            | category           |
+----+---------------------------+----------------+------------------------+--------------------+
|  4 | Designing Secure Software | 2021-12-21     | A Guide for Developers | Defensive Security |
+----+---------------------------+----------------+------------------------+--------------------+
1 row in set (0.00 sec)
```

#### Not Equal To Operator
- `!=`

When you want to filter out a particular value
```sql
mysql> SELECT * 
    -> FROM books
    -> WHERE category != "Offensive Security";
+----+----------------------------+----------------+------------------------------------------------------+--------------------+
| id | name                       | published_date | description                                          | category           |
+----+----------------------------+----------------+------------------------------------------------------+--------------------+
|  1 | Android Security Internals | 2014-10-14     | An In-Depth Guide to Android's Security Architecture | Defensive Security |
|  4 | Designing Secure Software  | 2021-12-21     | A Guide for Developers                               | Defensive Security |
+----+----------------------------+----------------+------------------------------------------------------+--------------------+
2 rows in set (0.00 sec)
```

#### Less Than Operator
- `<`

Filter on a record less than a given value
```sql
mysql> SELECT *
    -> FROM books
    -> WHERE published_date < "2020-01-01";
+----+----------------------------+----------------+------------------------------------------------------+--------------------+
| id | name                       | published_date | description                                          | category           |
+----+----------------------------+----------------+------------------------------------------------------+--------------------+
|  1 | Android Security Internals | 2014-10-14     | An In-Depth Guide to Android's Security Architecture | Defensive Security |
|  3 | Car Hacker's Handbook      | 2016-02-25     | A Guide for the Penetration Tester                   | Offensive Security |
+----+----------------------------+----------------+------------------------------------------------------+--------------------+
2 rows in set (0.01 sec)
```

#### Greater Than Operator
- `>`

Finding a value greater than a given value
```sql
mysql> SELECT * 
    -> FROM books
    -> WHERE published_date > "2020-01-01";
+----+---------------------------+----------------+--------------------------------------------------------+--------------------+
| id | name                      | published_date | description                                            | category           |
+----+---------------------------+----------------+--------------------------------------------------------+--------------------+
|  2 | Bug Bounty Bootcamp       | 2021-11-16     | The Guide to Finding and Reporting Web Vulnerabilities | Offensive Security |
|  4 | Designing Secure Software | 2021-12-21     | A Guide for Developers                                 | Defensive Security |
|  5 | Ethical Hacking           | 2021-11-02     | A Hands-on Introduction to Breaking In                 | Offensive Security |
+----+---------------------------+----------------+--------------------------------------------------------+--------------------+
3 rows in set (0.00 sec)
```

#### Less Than or Equal To and Greater  Than or Equal To Operators
- `<=`
- `>=`

Less than or Equal
```sql
mysql> SELECT * 
    -> FROM books
    -> WHERE published_date <= "2021-11-15";
+----+----------------------------+----------------+------------------------------------------------------+--------------------+
| id | name                       | published_date | description                                          | category           |
+----+----------------------------+----------------+------------------------------------------------------+--------------------+
|  1 | Android Security Internals | 2014-10-14     | An In-Depth Guide to Android's Security Architecture | Defensive Security |
|  3 | Car Hacker's Handbook      | 2016-02-25     | A Guide for the Penetration Tester                   | Offensive Security |
|  5 | Ethical Hacking            | 2021-11-02     | A Hands-on Introduction to Breaking In               | Offensive Security |
+----+----------------------------+----------------+------------------------------------------------------+--------------------+
3 rows in set (0.00 sec)
```

Greater or Equal
```sql
mysql> SELECT *
    -> FROM books
    -> WHERE published_date >= "2021-11-02";
+----+---------------------------+----------------+--------------------------------------------------------+--------------------+
| id | name                      | published_date | description                                            | category           |
+----+---------------------------+----------------+--------------------------------------------------------+--------------------+
|  2 | Bug Bounty Bootcamp       | 2021-11-16     | The Guide to Finding and Reporting Web Vulnerabilities | Offensive Security |
|  4 | Designing Secure Software | 2021-12-21     | A Guide for Developers                                 | Defensive Security |
|  5 | Ethical Hacking           | 2021-11-02     | A Hands-on Introduction to Breaking In                 | Offensive Security |
+----+---------------------------+----------------+--------------------------------------------------------+--------------------+
3 rows in set (0.00 sec)
```

### ❓ Question
> Using the `tools_db` database, which tool falls under the **Multi-tool** category and is useful for **pentesters** and **geeks**?
#### 🧪 Process
First, we need to change to the `tools_db` database
```sql
mysql> USE tools_db;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
```

We can find this by combining `WHERE` with `AND`, and `LIKE`.
```sql
mysql> SELECT *
    -> FROM hacking_tools
    -> WHERE category = "Multi-Tool" AND description LIKE '%pentesters%' AND description LIKE '%geeks%';
+----+--------------+------------+-------------------------------------------------------------------+--------+
| id | name         | category   | description                                                       | amount |
+----+--------------+------------+-------------------------------------------------------------------+--------+
|  1 | Flipper Zero | Multi-tool | A portable multi-tool for pentesters and geeks in a toy-like form |    169 |
+----+--------------+------------+-------------------------------------------------------------------+--------+
1 row in set (0.00 sec)
```

We have ourselves a `Flipper Zero`.

Trying this as the answer

#### ✅ Answer
- `Flipper Zero` ✅

### ❓ Question
> Using the `tools_db` database, what is the category of tools with an amount **greater than** or **equal** to **300**?
#### 🧪 Process
We can combined `WHERE` and `>=` fo this sone.

```sql
mysql> SELECT * 
    -> FROM hacking_tools
    -> WHERE amount >=300;
+----+-----------------+--------------+---------------------------------------------------------------------+--------+
| id | name            | category     | description                                                         | amount |
+----+-----------------+--------------+---------------------------------------------------------------------+--------+
|  5 | iCopy-XS        | RFID cloning | A tool used for reading and cloning RFID cards for security testing |    375 |
|  8 | Proxmark 3 RDV4 | RFID cloning | A powerful RFID tool for reading, writing, and analyzing RFID tags  |    300 |
+----+-----------------+--------------+---------------------------------------------------------------------+--------+
2 rows in set (0.00 sec)
```

Only one cateogry results, which is `RFID cloning`

Trying this as the answer

#### ✅ Answer
- `RFID cloning` ✅

  ### ❓ Question
> Using the `tools_db` database, which tool falls under the **Network intelligence** category with an amount **less than 100**?
#### 🧪 Process
We can combine `WHERE` with `<=` for the result.
```sql
mysql> SELECT *
    -> FROM hacking_tools
    -> WHERE category = "Network Intelligence" AND amount <= 100;
+----+------------+----------------------+--------------------------------------------------------------------+--------+
| id | name       | category             | description                                                        | amount |
+----+------------+----------------------+--------------------------------------------------------------------+--------+
|  6 | Lan Turtle | Network intelligence | A covert tool for remote access and network intelligence gathering |     80 |
+----+------------+----------------------+--------------------------------------------------------------------+--------+
1 row in set (0.00 sec)
```

The result is `Lan Turtle`.

Trying this as the answer
#### ✅ Answer
- `Lan Turtle` ✅


## Functions
Don't forget to change DB
```sql
mysql> use thm_books2;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
```

### String Functions
Allows us to perform operatins on strings
#### CONCAT() Function
Allows us to pull data from differnt columns to build query.
```sql
mysql> SELECT CONCAT(name, " is a type of ", category, " book.") AS book_info FROM books;
+------------------------------------------------------------------+
| book_info                                                        |
+------------------------------------------------------------------+
| Android Security Internals is a type of Defensive Security book. |
| Bug Bounty Bootcamp is a type of Offensive Security book.        |
| Car Hacker's Handbook is a type of Offensive Security book.      |
| Designing Secure Software is a type of Defensive Security book.  |
| Ethical Hacking is a type of Offensive Security book.            |
+------------------------------------------------------------------+
5 rows in set (0.00 sec)
```

#### GROUP_CONCAT() Function
Allows us to concatenate the book titles based on the category
```sql
mysql> SELECT category, GROUP_CONCAT(name SEPARATOR ", ") AS books
    -> FROM books
    -> GROUP BY category;
+--------------------+-------------------------------------------------------------+
| category           | books                                                       |
+--------------------+-------------------------------------------------------------+
| Defensive Security | Android Security Internals, Designing Secure Software       |
| Offensive Security | Bug Bounty Bootcamp, Car Hacker's Handbook, Ethical Hacking |
+--------------------+-------------------------------------------------------------+
2 rows in set (0.00 sec)
```

#### SUBSTRING() Function
Allows us to return a section of text rather thanthe whole value
Year of release:

```sql
mysql> SELECT SUBSTRING(published_date, 1, 4) AS year FROM books;
+------+
| year |
+------+
| 2014 |
| 2021 |
| 2016 |
| 2021 |
| 2021 |
+------+
5 rows in set (0.00 sec)
```

Or month of release:
```sql
mysql> SELECT SUBSTRING(published_date, 6, 2) AS month FROM books;
+-------+
| month |
+-------+
| 10    |
| 11    |
| 02    |
| 12    |
| 11    |
+-------+
5 rows in set (0.00 sec)
```

#### LENGTH() Function
Returns the number of characters within a string
```sql
mysql> SELECT LENGTH(name) AS length FROM books;
+--------+
| length |
+--------+
|     26 |
|     19 |
|     21 |
|     25 |
|     15 |
+--------+
5 rows in set (0.00 sec)
```

### Aggregate Functions
#### COUNT() Function
Count the number of rows
```sql
mysql> SELECT COUNT(*) as total FROM books;
+-------+
| total |
+-------+
|     5 |
+-------+
1 row in set (0.00 sec)
```

#### SUM() Function
Gets total from all values
```sql
mysql> SELECT SUM(amount) as total_cost FROM hacking_tools;
+------------+
| total_cost |
+------------+
|       1444 |
+------------+
1 row in set (0.01 sec)
```

#### MAX() Function
Find the largest result
```sql
mysql> SELECT MAX(amount) AS most_expensive FROM hacking_tools;
+----------------+
| most_expensive |
+----------------+
|            375 |
+----------------+
1 row in set (0.00 sec)
```

#### MIN() Function
Find the lowest value
```sql
mysql> SELECT MIN(amount) AS least_expensive FROM hacking_tools;
+-----------------+
| least_expensive |
+-----------------+
|              80 |
+-----------------+
1 row in set (0.00 sec)
```


### ❓ Question
> Using the `tools_db` database, what is the tool with the longest name based on character length?
#### 🧪 Process
This one was a bit harder and I had to look up how I would do this.

I found  you can run 'sub queries'
```sql
mysql> SELECT name
    -> from hacking_tools
    -> WHERE LENGTH(name) =(
    ->     SELECT MAX(LENGTH(name)) from hacking_tools);
+------------------+
| name             |
+------------------+
| USB Rubber Ducky |
+------------------+
1 row in set (0.00 sec)
```

Trying this as the answer

#### ✅ Answer
- `USB Rubber Ducky` ✅

### ❓ Question
> Using the `tools_db` database, what is the total sum of all tools?
#### 🧪 Process
Actually, I did this in an example already when we looked at [SUM](#sum-function).
```sql
mysql> SELECT SUM(amount) as total_cost FROM hacking_tools;
+------------+
| total_cost |
+------------+
|       1444 |
+------------+
1 row in set (0.01 sec)
```

Trying this as the answer

#### ✅ Answer
- `1444` ✅

### ❓ Question
> Using the `tools_db` database, what are the tool names where the amount does not end in **0**, and **group** the tool names **concatenated** by " & ".
#### 🧪 Process
Again this one was a bit of a tough one. I kept getting the order wrong. 
```sql
mysql> SELECT GROUP_CONCAT(name SEPARATOR " & ") as result
    -> FROM hacking_tools
    -> WHERE SUBSTRING(amount, 3, 1) != 0;
+-------------------------+
| result                  |
+-------------------------+
| Flipper Zero & iCopy-XS |
+-------------------------+
1 row in set (0.00 sec)
```

Trying this as the answer
#### ✅ Answer
- `Flipper Zero & iCopy-XS` ✅
