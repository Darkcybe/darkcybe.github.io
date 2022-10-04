---
title: Database Stores
categories: [Ethical Hacking, Common Ports and Services]
tags: [sql, mysql, nosql, mariadb, port:1433, port:3306, nmap, metasploit, hydra, sqlmap]
comments: true
---

# Overview

Relational and Non-Relational databases are a valuable target for attackers, with sensitive information often stored within the database tables such as Personally Identifiable Information (PII) and credentials. There are also several vulnerabilities and misconfigurations that can be exploited by an attacker to gain remote shell access to the host running the platform, privilege escalation and malicious program execution.

## Relational Databases

Structured Query Language (SQL) is a language specifically designed for managing structured data within relational database systems and is commonly used as backend storage for many software and web applications. See the post [SQL Overview](https://darkcybe.github.io/posts/SQL_Overview/) for more background information.

There are multiple relational database management systems (RDBMS), some of the most commonly encountered, their default ports and tool applicability are listed in the below table:

| Service       | Port  	 | Nmap   | MSF   | Hydra   | SQLmap | Other        |
| ------------- | :--------: | ------ | :---: | :-----: | :----: | ------------ |
| MSSQL         | 1433-1434  | -[x]  | -[x] | -[x]   | -[x]  | Crackmapexec <br> Impacket |
| MySQL/MariaDB | 3306  	 | -[x]  | -[x] | -[x]   | -[x]  |			    |
| PostgreSQL    | 5432  	 | -[x]  | -[x] | -[x]   | -[x]  |			    |
| Oracle        | 1521  	 | -[x]  | -[x] | -[x]   | -[x]  | Oscanner <br> ODAT |

### SQL Injection

See [CAPEC 66 - SQL Injection](https://darkcybe.github.io/posts/../../../../CAPEC/2022-10-02-66-SQL_Injection.md-Overflow_Buffers/) for verbose information and steps involved in conducting a SQL Injection attack.

## Non-Relational Databases

Non-Relational databases store data in a different format than Relational databases that rely on SQL. Non-Relational databases are often categorized as Not Only SQL (NoSQL) and are generally defined as a database that does not use tables, fields, and columns that structured data required. There are a few different catagories of Non-Relational databases such as; Document, Key-Value, Graph, File Systems, etc.

As with the various RDBMS solutions, there are a number of Non-Relational systems, some of the most commonly encountered, their default ports and tool applicability are listed in the below table. Other solutions which aren't typically listed as NoSQL databases are added here also, such as Redis and Memcached which are memory storage solutions and Hadoop, NFS, AFP, and iSCSI which are file systems.

| Service          | Port  	    | Nmap | MSF | Hydra | Other | 
| ---------------- | :--------: | ---- | :-: | :---: | ----- |
| MongoDB          | 27017 	 	| [x]  | [x] | [x]   | |
| Cassandra        | 9042      	| [x]  | [x] | []    | |
| Redis			   | 6579	 	| [x]  | [x] | [x]   | |
| Memcached		   | 11211	 	| [x]  | [x] | [x]    | |
| Hadoop Mapreduce | 50030 <br> 50060  | [x] | [] | []  | |
| Hadoop HDFS	   | 50070 <br> 50075 <br> 50090 | [x] | [] | []  | |
| NFS			   | 2049    	| [x]  | [x] | []    | NFSshell |
| AFP			   | 548     	| [x]  | [x] | [x]   | |
| iSCSI			   | 3260    	| [x]  | []  | []    | Open-iSCSI <br> iSCSIadm |

# MSSQL

Microsoft SQL Server (MSSQL) often exposes two ports:
1.	1433 - Used by clients to interact with the database
2.	1434 - Used to list available instances (a Server can run multiple instances on high ports)

Default credentials are often set to `sa:sa`, which sa equivalent to Sysadmin.

## MSSQL Scanning and Enumeration

| Tool | Script/Module | Auth | MITRE ATT&CK Tactic             | Command                                    |
| ---- | ------------- | :--: | ------------------------------- | ------------------------------------------ |
| Nmap | ms-sql-info   | N    | Reconnaissance        		    | `sudo nmap -A -p 1433,1434 -n 10.10.10.10` |
| MSF  | mssql_ping    | ?    | Reconnaissance                  | |           
| MSF  | mssql_enum    | ?    | Reconnaissance                  | |

## MSSQL Exploitation

| Tool | Script/Module           | Auth | MITRE ATT&CK Tactic             | Command                                    |
| ---- | ----------------------- | :--: | ------------------------------- | ------------------------------------------ |
| MSF  | mssql_escalate_dbowner <br> mssql_escalate_escalate_as | Y | Privilege Escalation | |
| MSF  | mssql_hashdump			 | Y    | Credential Access	     		  | |
| MSF  | mssql_local_auth_bypass | Y    | Persistence <br> Privilege Escalation  | |
| MSF  | mssql_ntlm_stealer      | Y    | Credential Access      		  | |
| MSF  | mssql_idf               | Y    | Discovery                       | |
| MSF  | mssql_payload           | Y    | Execution                       | |
| MSF  | mssql_sql_file          | Y    | Execution                       | |

## MSSQL Database Interaction

the `mssqlclient.py` python tool that comes pre-installed on Kali Linux as part of the Impacket suite, can be used to interact with a remote MSSQL server.

```bash
# Connecting to a Remote MSSQL Server (Requires Database selection, Domain, Username, Password, and IP address entry.)
mssqlclient.py -db %DATABASE% -windows-auth %DOMAIN%/%USERNAME%:%PASSWORD%@%IP%

# Database Enumeration
SELECT * from %TABLE% # Show all stored data under a select table
SELECT * FROM %DATABASE%.INFORMATION_SCHEMA.TABLES; # Show tables under a select database

# Exploitation
CREATE LOGIN &USERNAME% WITH PASSWORD = '&PASSWORD%' # Create a new user and assign sysadmin privileges
sp_addsrvrolemember '%USERNAME%', 'sysadmin'
```

# MySQL/MariaDB

MySQL is commonly found running on either windows or linux servers. The original MySQL solution was bought by Oracle, the previous open-source variant was forked and is referred to as MariaDB.

Default credentials are often set to `root:`, within some instances as per the example not requiring a password.

## MySQL/MariaDB Scanning and Enumeration

| Tool | Script/Module | Auth | MITRE ATT&CK Tactic             | Command                                    |
| ---- | ------------- | :--: | ------------------------------- | ------------------------------------------ |
| Nmap | mysql-info    | N    | Reconnaissance                  | `sudo nmap -A -p 3306 -n 10.10.10.10`
| MSF  | mysql_enum    | Y    | Reconnaissance                  | |

## MySQL/MariaDB Exploitation

## MSSQL Database Interaction

The MySQL command line tool can be used to interface with a remote MySQL/MariaDB instance.
	
```bash
# Accessing a Remote Server
mysql -h 10.10.10.10 -u root
mysql -h 10.10.10.10 -u root -e 'show databases;'
	
# Database Interaction
show databases; # Shows all available databases
use %DATABASE%; # Enter a select database
show tables; # Show tables under a select database
describe %TABLE%; # Show details of a select table
select * from %TABLE% # Show all stored data under a select table

# Exploitation
\! sh # Drop into a shell
mysql -h 10.10.10.10 -u root --password=%PASSWORD% -e "SELECT * FROM mysql.user;" # Credential Dumping
create user test identified by 'test'; # Create a new user and assign admin privileges
grant SELECT,CREATE,DROP,UPDATE,DELETE,INSERT on *.* to mysql identified by 'mysql' WITH GRANT OPTION;
```