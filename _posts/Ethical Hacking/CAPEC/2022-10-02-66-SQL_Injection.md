---
title: CAPEC 66 - SQL Injection
categories: [Ethical Hacking, CAPEC]
tag: [sql injection]
comments: true
---

# Overview

When specially crafted user-controlled input containing [SQL](https://darkcybe.github.io/posts/SQL_Overview/) syntax is used as part of SQL queries without proper validation, it is possible to glean information from the database in ways not anticipated during application design. It may also be possible to use injection to have the database execute system-related commands of the attackers' choice, depending on the database and the design of the application. SQL Injection allows an attacker to interact directly with the database, completely bypassing the application. Injection success can result in information disclosure as well as the ability to add or modify data in the database.

[CAPEC](https://capec.mitre.org/data/definitions/66.html)
> This attack exploits target software that constructs SQL statements based on user input. An attacker crafts input strings so that when the target software constructs SQL statements based on the input, the resulting SQL statement performs actions other than those the application intended. SQL Injection results from failure of the application to appropriately validate input.

# Techniques

 1. Explore
    - **Survey Application:** The attacker first takes an inventory of the functionality exposed by the application. 
    > Spider web sites for all available links
    > Sniff network communications with application using a utility such as WireShark.
 2. Experiment
    - **Determine user-controllable input susceptible to injection:** Determine the user-controllable input susceptible to injection. For each user-controllable input that the attacker suspects is vulnerable to SQL injection, attempt to inject characters that have special meaning in SQL (such as a single quote character, a double quote character, two hyphens, a parenthesis, etc.). The goal is to create a SQL query with an invalid syntax.
    > Use web browser to inject input through text fields or through HTTP GET parameters.
    > Use a web application debugging tool such as Tamper Data, TamperIE, WebScarab,etc. to modify HTTP POST parameters, hidden fields, non-freeform fields, etc.
    > Use network-level packet injection tools such as netcat to inject input
    > Use modified client (modified by reverse engineering) to inject input.
    - **Experiment with SQL Injection vulnerabilities:** After determining that a given input is vulnerable to SQL Injection, hypothesize what the underlying query looks like. Iteratively try to add logic to the query to extract information from the database, or to modify or delete information in the database.
    > Use public resources such as [SQL Injection Cheat Sheet](http://ferruh.mavituna.com/makale/sql-injection-cheatsheet/), and try different approaches for adding logic to SQL queries.
    > Add logic to query, and use detailed error messages from the server to debug the query. For example, if adding a single quote to a query causes an error message, try : `' OR 1=1; --`, or something else that would syntactically complete a hypothesized query. Iteratively refine the query.
    > Use "Blind SQL Injection" techniques to extract information about the database schema.
    > If a denial of service attack is the goal, try stacking queries. This does not work on all platforms (most notably, it does not work on Oracle or MySQL). Examples of inputs to try include: "`'; DROP TABLE SYSOBJECTS; --` and `'); DROP TABLE SYSOBJECTS; --`. These particular queries will likely not work because the `SYSOBJECTS` table is generally protected.
 3. Exploit
    - **Exploit SQL Injection vulnerability:** After refining and adding various logic to SQL queries, craft and execute the underlying SQL query that will be used to attack the target system. The goal is to reveal, modify, and/or delete database data, using the knowledge obtained in the previous step. This could entail crafting and executing multiple SQL queries if a denial of service attack is the intent.
    > Craft and Execute underlying SQL query

# Steps to Conduct an SQL Injection

1. Explore
    - **Survey Application:**

2. Experiment
    - **Determine user-controllable input susceptible to injection:**
    - **Experiment with SQL Injection vulnerabilities:**

3. Exploit
    - **Exploit SQL Injection vulnerability:**

# Sources
- [OWASP - SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection)
- [OWASP - Testing for SQL Injection](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05-Testing_for_SQL_Injection.html)