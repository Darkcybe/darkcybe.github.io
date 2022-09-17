---
title: C# Overview
categories: [General IT and Security, Programming]
tags: [c#,]
comments: true
---

# Overview

OOP is the foundation of many development languages, building classes and organizing the code into a structured way.  Many approaches, patterns and architectures are compatible with OOP. Learning the fundamentals of programming in OOP will allow a security analyst to have a greater understanding of how executable files are constructed and give huge insight when starting to delve into the world of Reverse Engineering and Malware Analysis. There are many OOP languages including JavaScript, C++, Java, Python and C#, for this post we will focus on C#.

C# leverages the Microsoft .NET framework, It is made up of a family of products including the runtime, compilers, and common code libraries. The code libraries provide a lot of reusable core functionality that can be used directly in a program. The most common library used is the 'System' library which allows applications to interact with the underlying system.

The general focus of developing an application using OOP is an iterative process and will not always logically flow between stages one after the other. Generally, the stages include:
- Identifying Classes
- Separating Responsibilities
- Establishing Relationships
- Leveraging Reuse

# Classes and Objects
OOP is used to structure a software application into simple, reusable pieces of code blueprints, called classes, which are used to created individual instances of objects, an example of this can be seen below accompanying the descriptions of each term. 
- **Classes:** A class is a concept used in OOP languages that provides values for state (member variables) and implementations of behavior (member functions, methods) in programs. A class is defined as a blueprint of an object, an extensible guide used for creating objects. A class does not represent and object; it represents all the information and methods an object should have.
    - The below code snippet is an example of a class, called 'Darkcybe' which contains a method called 'mainMethod' which invokes the .NET 'Console.WriteLine' method to print 'var1' to the console.

    ```c#
    Class Darkcybe
    {
        Public static void mainMethod(string[] args)
        {
            String var1 = "var1Value";
            Console.WriteLine(var1);
        }
    }
    ```
- **Objects:** An object is defined as an entity that can be utilized by using commands in a programming language. An object can be a variable, value, data structure or function. In OOP, an object is referred to as an instance of a class. An object contains properties and methods which are needed to make a certain type of data useful.

![Class and Object Diagram](/assets/img/posts/GEN/Programming/CS_Overview_Class_Object.png "Class and Object Diagram")

# Methods

Unlike other programming languages, C# does not use functions, however uses something similar called methods. A method cannot exist without a class in, therefore the class created in the above example 'Darkcybe' houses a static method called 'mainMethod'. Camel case is generally used for the Class and method name, whilst little camel case is used for the argument names. A Method generally contains one or more statements which are the basic unit of execution of a program.

## Access Modifiers

An item of note within Methods are Access Modifiers, of which there are six. The example above uses the 'Public' access modifier listed before the static modifier. The static modifiers purpose is purely to ensure that each instance created under the 'Darkcybe' class will belong to the class itself, not the object of the class.

| Keyword | Description |
|:---:|---|
| Public | Accessible without restriction from within a project |
| Private | Only accessible inside a class |
| Protected | Accessible inside a class or in all classes derived from that class |
| Internal | Accessible only to the current assembly |
| Protected Internal | A combination of Protected and Internal, <br> can only be accessed in the same assembly or in a derived class in other assemblies. |
| Private Protected | A combination of Private and Protected, <br> accessible only by members inside of a containing class or in a class that derives from a containing class. |

# Data Types

Variables in C# are categorized into three difference types which define the type of data being represented by the variable. Ensuring the correct type is implied is important when coding application in C#. For reference tin the above code excerpt, the type 'Void' (an example of a reference type) declares that the entire method does not have an assigned type.

| Keyword | Description |
|:---:|---|
| Value | Assigned a direct value and are derived from the class 'System.ValueType'. <br> Value types contain numerical, alphabetical for floating point numbers. <br> There are multiple types of data which can be assigned as per the 'Value Data Types' table below. |
| Reference | Reference types do not contain actual data, rather they contain a reference to the variable that does. <br> References can be to either an object, a string or dynamic. |
| Pointer | Pointers store memory addresses of another type. |

## Value Data Types

| Keyword | Description | Range | Data Example |
|:---:|---|---|---|
| bool | Boolean value | True or False |   |
| byte | 8-bit unsigned integer | 0 to 255 |   |
| char | Stores a single character/letter, surrounded by single quotes | U +0000 to U +ffff | 'A' |
| decimal | 128-bit precise decimal values with 28-29 significant digits | (-7.9 x 1028 to 7.9 x 1028) / 100 to 28 | 10.1 |
| double | Stores fractional numbers. Sufficient for storing 15 decimal points | (+/-)5.0 x 10-324 to (+/-)1.7 x 10308 |   |
| float | Stores fractional numbers. Sufficient for storing 6 to 7 decimal digits | -3.4 x 1038 to + 3.4 x 1038 | ﻿ |
| int | Stores whole numbers | -2,147,483,648 to 2,147,483,647 | 10 |
| long | Stores long whole numbers | -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 |   |
| sbyte | 8-bit signed integer type | -128 to 127 |   |
| short | 16-bit signed integer type | -32,768 to 32,767 |   |
| uint | 32-bit unsigned integer type | 0 to 4,294,967,295 |   |
| ulong | 64-bit unsigned integer type | 0 to 18,446,744,073,709,551,615 |   |
| ushort | 16-bit unsigned integer type | 0 to 65,535 |   |
| String | Stores a sequence of characters, surrounded by double quotes |   | "string" |

# Statements

Statements are the core component of a programs execution, the logic behind what operations are conducted. There are a few different types of statements that are used in C#, the above code example showcases the use of a Declaration statement to assign the value of 'var1calue' to the variable 'var1' and print it to the console. Statements are written using Keywords and Expressions to perform operations.

| Keyword | Description | Statement Example |
|:---:|---|---|
| Selection | Enables logical branches to different sections of code depending one one nor more specific conditions. | `if (condition) { do something }`<br><br>`switch (example) { case <1: do this; break; }` |
| Iternation | Enables code to loop through collections such as arrays or perform the same set of statement <br> repeatedly until a specified condition is met | `foreach (item in list) { do something }`<br><br>`int d = 999;`<br>`while ( d < 999) { do this; d++; }` |
| Declaration | Used to declare and initialize variables | `string darkcybe;` |
| Expression | Calculate a value and must store the value in a variable | `int sum = 900 + 99;` |
| Exception Handling | Enable graceful recovery from exceptional conditions that occur at run time. | `try { this } catch { stop }`<br><br>`throw [e];` |
| Jump | Transfer control to another section of code. | `break;`<br><br>`goto section2;` |

# Expressions and Operators

Expressions are similar to statements however are not executable on their own, statements use expressions and keywords in order to give expressions meaning in to operationalize them. Operators are useful keys that allow operations to be performed on variables and values. There are several different categories that operators are broken into; Arithmetic, Relational/Comparison, Logical, Bitwise, Assignment and Miscellaneous. An operator performs an action to one or more operands.

![Expression, Operand, and Operator](/assets/img/posts/GEN/Programming/CS_Overview_Expression_Operators.png "Expression, Operand, and Operator")

## Arithmetic Operators

| Operator | Description | Data Example |
|:---:|---|---|
| + | Adds two operands | A + B = 30 |
| - | Subtracts second operand from the first | A - B = -10 |
| * | Multiplies both operands | A * B = 200 |
| / | Divides numerator by de-numerator | B / A = 2 |
| % | Modulus Operator and remainder of after an integer division | B % A = 0 |
| ++ | Increment operator increases integer value by one | A++ = 11 |
| -- | Decrement operator decreases integer value by one | A-- = 9 |

## Relational/Comparison Operators

| Operator | Description | Data Example |
|:---:|---|---|
| == | Checks if the values of two operands are equal or not, if yes then condition becomes true. | (A == B) is not true. |
| != | Checks if the values of two operands are equal or not, if values are not equal then <br> condition becomes true. | (A != B) is true. |
| > | Checks if the value of left operand is greater than the value of right operand, if yes <br> then condition becomes true. | (A > B) is not true. |
| < | Checks if the value of left operand is less than the value of right operand, if yes then <br> condition becomes true. | (A < B) is true. |
| >= | Checks if the value of left operand is greater than or equal to the value of right <br> operand, if yes then condition becomes true. | (A >= B) is not true. |
| <= | Checks if the value of left operand is less than or equal to the value of right operand, <br> if yes then condition becomes true. | (A <= B) is true. |

## Logical Operators

| Operator | Description | Data Example |
|:---:|---|---|
| && | Called Logical AND operator. If both the operands are non zero then condition becomes true. | (A && B) is false. |
| \|\| | Called Logical OR Operator. If any of the two operands is non zero then condition becomes <br> true. | (A \|\| B) is true. |
| ! | Called Logical NOT Operator. Use to reverses the logical state of its operand. If a condition <br> is true then Logical NOT operator will make false. | !(A && B) is true. |

## Bitwise Operators

| Operator | Description | Data Example |
|:---:|---|---|
| & | Binary AND Operator copies a bit to the result if it exists in both operands. | (A & B) = 12, which is 0000 1100 |
| \| | Binary OR Operator copies a bit if it exists in either operand. | (A \| B) = 61, which is 0011 1101 |
| ^ | Binary XOR Operator copies the bit if it is set in one operand but not both. | (A ^ B) = 49, which is 0011 0001 |
| ~ | Binary Ones Complement Operator is unary and has the effect of 'flipping' bits. | (~A ) = -61, which is 1100 0011 in 2's complement due to a signed binary number. |
| << | Binary Left Shift Operator. The left operands value is moved left by the <br> number of bits specified by the right operand. | A << 2 = 240, which is 1111 0000 |
| >> | Binary Right Shift Operator. The left operands value is moved right by the <br> number of bits specified by the right operand. | A >> 2 = 15, which is 0000 1111 |

## Assignment Operators

| Operator | Description | Data Example |
|:---:|---|---|
| = | Simple assignment operator, Assigns values from right side operands to left side operand | C = A + B assigns value of A + B into C |
| += | Add AND assignment operator, It adds right operand to the left operand and assign the <br> result to left operand | C += A is equivalent to C = C + A |
| -= | Subtract AND assignment operator, It subtracts right operand from the left operand and <br> assign the result to left operand | C -= A is equivalent to C = C - A |
| *= | Multiply AND assignment operator, It multiplies right operand with the left operand and <br> assign the result to left operand | C *= A is equivalent to C = C * A |
| /= | Divide AND assignment operator, It divides left operand with the right operand and assign <br> the result to left operand | C /= A is equivalent to C = C / A |
| %= | Modulus AND assignment operator, It takes modulus using two operands and assign the result <br> to left operand | C %= A is equivalent to C = C % A |
| <<= | Left shift AND assignment operator | C <<= 2 is same as C = C << 2 |
| >>= | Right shift AND assignment operator | C >>= 2 is same as C = C >> 2 |
| &= | Bitwise AND assignment operator | C &= 2 is same as C = C & 2 |
| ^= | bitwise exclusive OR and assignment operator | C ^= 2 is same as C = C ^ 2 |
| \|= | bitwise inclusive OR and assignment operator | C \|= 2 is same as C = C \| 2 |

## Miscellaneous Operators

| Operator | Description | Data Example |
|:---:|---|---|
| sizeof() | Returns the size of a data type. | sizeof(int), returns 4. |
| typeof() | Returns the type of a class. | typeof(StreamReader); |
| & | Returns the address of an variable. | `&a;` returns actual address of the variable. |
| * | Pointer to a variable. | `*a;` creates pointer named 'a' to a variable. |
| ? : | Conditional Expression | If Condition is true ? Then value X : Otherwise value Y |
| is | Determines whether an object is of a certain type. | If( Ford is Car) // checks if <br> Ford is an object of the Car class. |
| as | Cast without raising an exception if the cast fails. | Object obj = new StringReader("Hello"); <br> StringReader r = obj as StringReader; |

# Sources
- [Tutorials Point - C Sharp](https://www.tutorialspoint.com/csharp/index.htm)
- [W3 Schools - CS](https://www.w3schools.com/cs/index.php)
- [Microsoft - Dotnet Framework](https://docs.microsoft.com/en-us/dotnet/framework/)
- [Microsoft - C Sharp](https://docs.microsoft.com/en-us/dotnet/csharp/)