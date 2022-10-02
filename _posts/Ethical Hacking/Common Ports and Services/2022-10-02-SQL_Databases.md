---
title: SQL Databases
categories: [Ethical Hacking, Common Ports and Services]
tags: [sql, mysql, mariadb, port:1433, port:3306, nmap, metasploit]
comments: true
---

# Overview

Structured Query Language (SQL) is a language specifically designed for managing structured data within relational database systems and is commonly used as backend storage for many software and web applications.

There are multiple relational database management systems (RDBMS) that use SQL, some of the most commonly encountered are:
- **Microsoft MSSQL**
- **MySQL**
- **MariaDB (Fork of MySQL)**
- **PostgreSQL**
- **MongoDB (NoSQL Solution)**

SQL platforms are  a valuable target for attackers, with sensitive information often stored within the database tables such as Personally Identifiable Information (PII) and credentials. There are also several vulnerabilities that can be exploited by an attacker to gain remote shell access to the host running the platform.

# Reconnaissance

## Port Scanning and Enumeration

Nmap has various scripts that can be run against the different versions, running a generic initial scan and including the `-sV` flag.

### Default Ports

| Service       | Port  | Handy Nmap Scripts |
| ------------- | :---: | ------------------ |
| MSSQL         | 1433  | ms-sql-info <br> ms-sql-config <br> ms-sql-empty-password <br> |
| MySQL/MariaDB | 3306  | mysql-info <br> mysql-enum <br> mysql-empty-password |
| PostgreSQL    | 5432  | |
| MongoDB       | 27027 | |

### MSSQL
- Metasploit modules `admin/mssql/mssql_ping` and `admin/mssql/mssql_enum` can be run to gain verbose details on the MSSQL instance. A default credential of `sa`, representative of Sysadmin, can be attempted with a blank password or `sa` can be attempted. However, it should be noted that this could cause accounts to be locked.

### MySQL/MariaDB
- Metasploit module `admin/mysql/mysql_enum` can be run to gain verbose details on the MySQL/MariaDB instance. A default credential of `root` with a blank password can be used for authentication.
- The MySQL command line tool can be used to interface with a remote MySQL/MariaDB instance.
	
    ```bash
	# Accessing a Remote Server
	mysql -h 10.10.10.10 -u root
	
	# Database Interaction
	show databases; # Shows all available databases
	use %DATABASE%; # Enter a select database
	show tables; # Show tables under a select database
	describe %TABLE%; # Show details of a select table
	select * from %TABLE% # Show all stored data under a select table
	```

# Exploitation

## SQL Injection