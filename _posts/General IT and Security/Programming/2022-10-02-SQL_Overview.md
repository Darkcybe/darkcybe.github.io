---
title: SQL Overview
categories: [General IT and Security,Programming]
tag: [sql]
comments: true
---

# Overview

Structured Query Language (SQL) is designed for managing data held in a [relational database management system](https://en.wikipedia.org/wiki/Relational_database_management_system) (RDBMS). It is particularly useful in handling structured data, i.e. data incorporating relations among entities and variables. SQL Server is a product developed by Microsoft to manage relational databases, storing and retrieving data requested by other applications. There are various topics regarding SQL and cyber security ranging from the protection of databases, SQL Injection attacks and database manipulation. This post however, serves as a fundamental overview of the SQL database programming and server.

# Database Concepts

A relational database is designed to organize a collection of data, typically stored and accessed electronically from a computer system or electronic device and is regularly referenced and altered by software applications. There are three generic styles of databases, with SQL focusing on the later, Relational Databases.
- Hierarchical: Think directory structure
- Flat-file: CSV, Excel, Delimited
- Relational: MSSQL, MySQL, and more
  - Tables: Think an excel page
  - Columns: Data headers
  - Rows: Data values

# SQL Sub-languages

## Data Manipulation Language (DML)

DML is an SQL sub-language that deals with the manipulation of data records stored within the database tables. It does not deal with changes to database objects or its structure. Some commonly used examples of DML are as in the table below:

| Statement | Description | Code Example |
|---|---|---|
| SELECT | Retrieve rows from database and enables select of one or many rows or columns | SELECT SalesOrderID, SUM(LineTotal) AS SubTotal<br>FROM Sales.SalesOrderDetail |
| INSERT | Adds one or more rows to a table or view | INSERT INTO table_name (column1, column2)<br>VALUES (value1, value2)<br> <br>INSERT INTO table_name<br>VALUES (value1, value2, value3) |
| UPDATE | Changes existing data in a table or view |   |
| DELETE | Removes one or more rows from a table or view | DELETE FROM table_name<br>WHERE [condition]; |
| MERGE | Performs insert, update, or deletes on a target table based on the results of a join with a source table |   |
| BULK INSERT | Imports a data file into a database table or view |   |

## Data Definition Language (DML)

Unlike DML above, DDL defines data structures and is used to update or modify objects rather than the data contained within them. The below table lists the more common DDL statements used:

| Statement | Description | Code Example |
|---|---|---|
| CREATE | Enables creating an item in SQL Server. For example, database, table, view, users, index and more | CREATE DATABASE database_name |
| ALTER | Enables modification of an existing object |   |
| DROP | Drops and existing object | DROP TABLE table_name |
| COLLATION | Defines the collation of a database or table column, or a collation cast operation when applied toa  character string expression. Assigns certain characteristics to data. |   |
| USE | Changes the database context to the specified database (Switch between databases) | USE database_name |

# Database Objects

## Data Types

Data Types are an attribute that specifies the type of data an object can hold, such as date and time, binary strings, monetary data, integer data. Data Types have several characteristics:
- Each column, local variable, expression, and parameter has a related data type.
- Data type of the results is determined by the applying rules of data type precedence to data types of input expressions (handled by the SQL Server engine).
- Collation of result is determined by rules of the collation precedence when result data type is char, varchar, text, nchar, ntext.
- Precision, scale, and length of a result depend on precision, scale, and length of input expressions.

| Data Type | Statement | Description | Storage |
|---|---|---|---|
| Approximate Numeric | FLOAT | Approximate-number data type to store the mantissa of the float number in scientific notation and dictates the precision and storage size. The default value of n = 53. | FLOAT[(n)]: (n = 1 to 24) 7 digits and 4 bytes<br>FLOAT[(n)]: (n = 25 to 53) 15 digits and 8 bytes |
| Binary Strings | BINARY | Fixed-length binary data. | BINARY[(n)]: (n = 1 to 8000) n bytes |
| Character Strings | CHAR | Fixed-length string data. | CHAR[(n)]: (n =1 to 8000 bytes) n x 2 bytes |
| Date and Time | DATE | Defines  a date in SQL Server |   |
| Exact Numeric | TINYINT, SMALLINT, INT, BIGINT | Exact-number data types that use integer data. Always use the smallest data type for your purpose! | TINYINT: 0 to 255 : 1 byte<br>SMALLINT: -2^15 to 2^15 : 2 bytes<br>INT: 2^31 to 2^32 : 4 bytes<br>BIGINT: -2^63 to 2^63 : 8 bytes |
| Miscellaneous | CURSOR | For variables or stored procedure OUTPUT parameters that contain a reference to a cursor. |   |
| Unicode Character Strings | NCHAR | Fixed-length string data. | NCHAR[(n)]: (n = string length in byte-pairs, n = 1 to 4000) n x 2 bytes |
|   | DECIMAL, NUMERIC | Fixed precision and scale. Scale is number of decimals to the right of a decimal point (1 to 38 bytes with a default of 18) | -10^38 + 1 to 10^38 -1 |
|   | SMALLMONEY, MONEY | Accurate to a ten-thousandth. Use a period to separate monetary units such as cents. | SMALLMONEY: 4 bytes<br>MONEY: 8 bytes |
|   | REAL | 7 digits of precision making it identical to float(24). | 4 bytes |
|   | NVARCHAR | Variable-length string data. | NVARCHAR[(n)]: (n = string length in byte-pairs, n = 1 to 4000) n x 2 bytes |
|   | IMAGE | Variable-length binary blob data (images, documents, files, etc). | IMAGE[(n)]: (n = 0 to 2^31 -1 bytes) 2GB |
|   | VARBINARY | Variable-length binary data. | VARBINARY[(n)]: (n = 1 to 8000)<br>VARBINARY[(MAX)]: (n = 1 to 2^31 -1) 2GB, storage is the actual length of the data + 2 bytes. |
|   | DATETIME | Date with time of day with fractional seconds and based on 24-hour clock. |   |
|   | DATETIME2 | Expanded datetime with larder date range, default fractional precision, and user-specified precision. |   |
|   | DATETIMEOFFSET | Date and time of day that has the time zone awareness and based on 24-hour clock. |   |
|   | SMALLDATETIME | Datetime without seconds or fractional seconds. |   |
|   | VARCHAR | Variable-length string data. | VARCHAR[(n)]: (n = 1 to 8000)<br>VARCHAR[(MAX)]: (n = 1 to 2^31 -1) 2GB, storage is the actual length of the data + 2 bytes. |
|   | TEXT | Variable-length non-Unicode data in the code page of the server | 2^31 -1 (2GB) |
|   | ROWVERSION | Exposes automatically generated, unique binary numbers within a database that are generally used a mechanism for version-stamping table rows. |   |
|   | HIERARCHYID | Variable-length, system data type that represents a position in a tree hierarchy. |   |
|   | UNIQUEGUIDIDENTIFIER | 6-byte GUID created from NEWID or NEWSEQUENTIALID functions or converting a string constant in a certain format. |   |
|   | SQL_VARIANT | Stores values of various SQL Server-supported data types. |   |
|   | XML | Stores XML data |   |
|   | SPATIAL GEOMETRY AND GEOGRAPHY TABLES | Implemented as a .NET common language runtime (CLR) data type for storing location specific data. |   |
|   | TABLE | Primarily used for temporary storage of a set of rows returned as the result of a table-valued function. |   |

## Tables and Views

**Tables**
- Tables contain all the data in the database in a row-and-column format
- A table can have 1024 columns, and 30000 columns if using SPARSE
- Assign properties to a table and columns to use compression (by row or page)

**Views**
- Virtual table defined by a query
- Similar to a table with named columns and rows that are produced dynamically when referenced
- Acts like a filter on tables and can reference multiple tables and provide a level of security (access to pre-created views can be assigned to a user instead of giving a user access to tables)

## Stored Procedures and Functions

### Stored Procedures

```plaintext
(%DATABASE%\Programmability\Stored Procedures)
```

A subroutine available to applications that access a DB System, when creating a stored procedure `CTL+SHIFT+M` can be used to enter a templated view.
- A group of one or more Transact-SQL (T-SQL) statements or a reference to a Microsoft .Net Framework CLR method.
- Can accept input parameters and return multiple values as output parameters
- Have programming statements that perform operations within the database
- Return a status value indicating success or failure

Using stored procedures has a few benefits.
- Reduces server/client network traffic by saving the procedures on the server and only return the result to the client.
- Stronger security by containing stored procedures that have been validated and tested. Avoids SQL injection attackers where a client could manifest their own query.
- Reuse of Code – Write a stored procedure once and use it with multiple applications instead of writing the same code into multiple applications.
- Easier maintenance for applications, no changes in the client application
- Improved performance, speeds up network utilization and server execution.

### Functions

```plaintext
(%DATABASE%\Programmability\Functions)
```

Functions are routines that accept parameters, perform and action, and return the result of that action as a value.
- Allows for modular programming, faster execution, reduction of network traffic, scalar function, table-valued functions and system functions to return needed information.

![Stored Procedure Example](/assets/img/posts/GEN/Programming/SQL_Overview_SP.png "Stored Procedure Example")

# Data Manipulation

## Data Selection

Selecting data is the most common statement used by applications to query data in the database. The below table displays the common statement and query clauses.

| Statement | Description | Example |
|---|---|---|---|
| SELECT | Retrieves rows from the database and enables you to select one or more columns or rows from one or many tables. | SELECT * INTO copyoftableName FROM tableName |
| FROM | Specifies the tables, views, derived tables, and joined tables using in DELETE, SELECT, and UPDATE statements. | SELECT SalesOrderID, SUM(LineTotal) AS SubTotal<br>FROM Sales.SalesOrderDetail |
| GROUP BY | Clause that divides the query result into groups of rows, typically for performing aggregations. | SELECT SalesOrderID, SUM(LineTotal) AS SubTotal<br>FROM Sales.SalesOrderDetail<br>GROUP BY SalesOrderID |
| HAVING | Typically used with GROUP BY in a SELECT statement as a search condition for a group or an aggregate. | SELECT SalesOrderID, SUM(LineTotal) AS SubTotal<br>FROM Sales.SalesOrderDetail<br>GROUP BY SalesOrderID<br>HAVING SUM(LineTotal) > 100000.00 |
| ORDER BY | Orders result set of a query by the specific column list. | SELECT SalesOrderID, SalesOrderItem<br>FROM Sale.SalesOrderID<br>ORDER BY SalesOrderID |
| UNION | Combines results of two or more queries into a single results set that has all rows of each query in the union. |   |
| EXCEPT | Returns distinct rows from left input query that aren’t output by right input query. |   |
| INTERSECT | Returns distinct rows that are output by both left and righty input queries. |   |
| JOIN | Defines the way two tables are related in a query. |   |
| TRUNCATE | Removes all rows from a table or specified partitions of a table but doesn’t log row deletions making it much faster than DELETE if removing all rows in a table by using fewer system and transaction log resources. | TRUNCATE TABLE TableName |

## Data Modification

Changes existing data in a table or view, covered in above section on DML.

## Deleting Data

Used to delete existing records in a table, covered in above section on DML.

DELETE 
: Permissions are required on the target table as well as SELECT permissions if the statement contains a WHERE clause (and if no WHERE clause is used, all records will be deleted).

TRUNCATE 
: Is another statement used to delete rows and requires the ALTER permission on the specific table.

## Inserting Data

Allows you to insert new records into a table. If adding values for all columns in a table, there is no need to specify the column names, otherwise order of values must match order of columns.

> INSERT permission is required.

# Data Storage

## Normalization

Database design technique that makes databases more efficient. Resolves issues of data redundancy and improves storage, data integrity and scalability. Also helps reduce INSERT, UPDATE, and DELETE anomalies.

Normalization is structured under certain ‘Normal Forms’ as per the table below. Most organizations will design their database to 3NF.

| Statement | Description |
|---|---|---|---|
| First Normal Form (1NF) | Eliminated repeating groups. No duplicate records exist in table and no multi-valued attributes. All entries in the column are of the same data type. |
| Second Normal Form (2NF) | Table is already 1NF, all partial dependencies are removed to another table(s). |
| Third Normal Form (3NF) | Table is already 2NF, eliminate non-dependent columns and transient dependencies. |
| Fourth Normal Form (4NF) | Already 3NF, no independent multiple relationships. |
| Fifth Normal Form (5NF) | Already 4NF, isolate semantically-related relationships. |

## Keys

Keys are the set of attributes that used to identify the specific row in a table and to find or create the relation between two or more tables i.e keys identify the rows by combining one or more columns. The table below identifies the main three keys:

| Key Type | Description |
|---|---|
| Primary Key (PK) | Uniquely identifies each record in a table.<br>– Cannot contain NULL values, must be UNIQUE<br>– Only one Primary Key per table, using single or multiple fields |
| Foreign Key (FK) | Not just to a primary key constraint in another table, can reference columns of a UNIQUE constraint in another table.<br>– Can only reference tables within the same database on the same server<br>– Self-reference is allowed to reference another column in the same table<br>– Column-level constraint must list only one reference column and be the same data type<br>– Table-level constraint needs same number of reference columns and data types |
| Composite Key | A key that includes multiple columns. Can be designated as the primary key if the combination of the columns result in a unique composite value for every row in the table. Commonly used in tables designed with many to many relationships. |

# Database Administration

## Authentication, Users, Roles, and Permissions

How to connect and login to access resources. There are multiple methods to authenticate.

```plaintext
%SQL_SERVER%\Security\Logins\
```

| Authentication Method | Description | Additional |
|---|---|---|
| Windows Authentication | Commonly referred to as Integrated Security | Preferred method as it provides a single sign-on for the OS and SQL Server. Windows Groups can be used to manage access. |
| SQL Server and Windows Authentication | Commonly referred to as Mixed-mode Security | SQL Logins allow application-specific security and mixes OS access.<br>– SQL Server Login: Server Role<br>– Database User: Role, Application Role, or Group |

**SQL Server Authentication**

| Account | Description |
|---|---|
| Sysadmin | Superuser role for the Server |
| Database Users | Windows Logins<br>SQL Logins<br>Windows Groups (AD)<br>User without login |
| Database Owner (DBO) | Database Owner, mapped to Sysadmin |
| Guest | Logins without mapping are guest accounts. Recommended to disable. |

**SQL Server Accounts**

| Role | Description |
|---|---|
| Db_owner | Perform all configuration and maintenance, including dropping a database |
| Db_securityadmin | Modify role membership and manage permissions |
| Db_accessadmin | Add or remove access to the database for Windows logins or groups, and SQL Server logins |
| Db_backupoperator | Can backup the database |
| Db_ddladmin | Can run any DDL command in a database |
| Db_datawriter | Can add, delete, or change data in all user tables |
| Db_datareader | Can read all data from all user tables |
| Db_denydatawriter | Restricts modification of data explicitly |
| Db_denydatareader | Restricts the reading of data from a database |
| Public | Assigned to all users |

**Database Roles**

| Permission | Description |
|---|---|
| GRANT | Positive privilege |
| DENY | Negative privilege |
| REVOKE | Negates a GRANT or DENY |
| ALTER | Make changes to database objects |
| BACKUP | Backup Objects |
| CONTROL | Grants full access to objects |
| CREATE | Create Objects |
| DELETE | Delete objects |
| DROP | Drop databases or tables |
| SELECT | Select or read data from a table or view |
| ENDPOINTs (Instance-Level) | Sets configuration for access to the SQL Server instance. TCP/IP or network restrictions. |
| ASSEMBLY and SERVICEs (Database-Level) | ASSEMBLY references a .DLL, uploading an ASSEMBLY is the first step to allowing application roles and client access. |
| TABLEs, VIEWs, PROCEDUREs, and QUEUEs (Schema-Level) | Allows the locking down of access to specific tables, views, etc. |
| (Column-Level) | Restrict permissions to only specific columns |

**Permissions**

There are many permissions that can be applied to an account in a granular access model. ~200 exposed permissions. Common permissions are as above in the Permissions table.

## Maintenance Tasks

Considerations to take when maintaining a SQL Server instance are typically based in the following table:

| Task | Description |
|---|---|
| Statistics | Used by the Query Optimize to compile an optimal execution plan. “Auto Update Statistics” is built-in and available to automatically do this. |
| Index | Fragmentation naturally occurs due to inserts, updates, and deletes and can impact performance negatively. To control fragmentation, the following methods can be employed.<br>– Rebuild, reorganize, or disable-and-rebuild the index<br>– Database Maintenance Plans<br>– Custom Scripts in a SQL Agent Job<br>– Third party tools |
| Consistency Checks | Corruption can occur, typically based on IO (disk Input/output). There are built-in checks to assist in identifying database corruption.<br>– DBCC CHECKDB<br>– DBCC CHECKALLOC<br>– DBCC CHECKCATALOG<br>– DBCC CHECKFILEGROUP<br>– DBCC CHECKTABLE |

**Backups and Disaster Recovery (DR)**

| Backup Type | Description | Code Example |
|---|---|---|
| Full | All data in the database is backed up | BACKUP DATABASE |
| Differential | Only backs up changed data from the last Full Backup | BACKUP DATABASE<br>WITH DIFFERENTIAL |
| Transaction Log | A log of all transactions carried out on the database | BACKUP LOG |
| Copy-Only Backup | An ‘out-of-band’ backup that can be used to an immediate backup that doesn’t impact previously set backup strategies. |   |