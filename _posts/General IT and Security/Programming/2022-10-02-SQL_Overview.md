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
