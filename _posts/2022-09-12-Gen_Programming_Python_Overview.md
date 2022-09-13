---
title: Python Overview
categories: [General IT and Security,Programming]
tags: [Python,]
comments: true
---

Python is an easy to understand, interpreted programming language that is commonly used across the cybersecurity community.

# Data Types
## Integers and Strings

Integers are set without quotation and can also include mathematical operators (+-/*). Strings are required to be wrapped in quotation marks (singular (') or double(“)) however cannot contain special characters unless they are explicitly escaped using a backslash (\). Multi line strings can be used instead of escaping characters (''') and can also carry the single string to multiple lines.

```python
a = 10
aa = a * 2
b = "string"
bb = "Escaping \"special characters using backslash\""
c = '''Multi-line strings
or strings that contain special characters "can be used like this"'''
```
{: .nolineno }

Strings and Integers can look similar to humans, for example the string ‘10’ and integer 10, however are completely different when being interpreted by a computer. Python interrupts user input as strings, therefore if the input is required to be parsed as an integer or other value they will need to be converted.

```python
a = '10'
converted_a = int(a)
b = '10.5'
converted_b = float(b)
```
{: .nolineno }

## Empty Values

In Python, an empty value is referred to as ‘None’. Its important to understand that the value None is different than a value of 0. These are used to create a variable without assigning a value.

```python
a = None
```
{: .nolineno }

## Lists

A list is a data type that can contain more than one value and are created using the square bracket ([). Entries inside of a list are referred to an indexes and begin with the first entry at index 0. Lists are modifiable, with indexes being able to changed. Lists can contain any values, including other lists.

```python
d = ['darkcybe', 'security']
mylist = ['one', 2, 'three' 4]
converged = [[1, 2, 3, 4], ['one', 'two', 'three', 'four']]
```
{: .nolineno }

Calling a specific index is done by its index number, as in the example below where we want to return ‘security’ from the d list above. In the case of nested lists, such as the ‘converged’ list, two indexes are required; the first being the list index, and the second the value index. An example is show below of calling the value 'three; from the nested list.

```python
d[1]
converged[1][2]
```
{: .nolineno }

Lists can be modified using methods. Methods are similar to function in which they allow certain actions to be taken on an object. Some of the commonly used methods relating to lists can be found here [Data Structures](https://docs.python.org/3/tutorial/datastructures.html) .

| Method | Description | Example |
| --- | --- | --- |
| .append | Add an item to the end of a list | `converged.append('five')` |
| .inset | Insert an item to a specific index, the argument consists of (index, value) | `mylist.insert(2, 'five')` |
| .remove | Removes the first object equal to the argument value | `d.remove('security')` |
| .pop | Removes an item from a specific index | `d.remove([1])` |
| .clear | Remove all items from a list | `d.clear()` |

## Tuples

Tuples are similar to lists in construction, replacing the square bracket with a round bracket ((). However, unlike the indexes of a list which are able to be modified, indexes inside of a tuple are static and cannot be changed.

```python
3 = ('darkcybesec', 'security blog')
```
{: .nolineno }

## Ranges

Ranges represent an immutable sequence of numbers and is commonly used for looping a specific number of times in for loops. Ranges can accept two arguments as depicted in the below example. The first example depicts a ‘stop’ argument, creating a list with 10 indexes. The second involves three arguments; start, stop and step. Start is the beginning of the count, stop the finish, and step being the increment in which the values are counted.

```python
list(range(10)) # 0, 1, 2, 3, 4, 5, 6, 7, 8, 9
list2(range(0, 50, 5)) # 0, 5, 10, 15, 20...
```
{: .nolineno }

## Dictionaries

Dictionaries are similar to lists, however include pairs of data, a key and a value. Dictionaries can hold anything, numbers, strings, a mix and even lists or nested dictionaries. Unlike lists and tuples where values are called via index, dictionaries do not contain index values and storage of key-value pairs is random. To call a value or pair within a dictionary, they are accessed via the key.

```python
protocols = {'HTTP': 80,
             'HTTPS': 443,
             'FTP': 21,
             'SSH': 22}

traffic = {'Allowed': ['HTTP', 'HTTPS'],
           'Blocked': ['FTP', 'SSH']}

contacts = {"number":2,
            "students": [{"name":"Harry Potter", "email":"hpotter@gmail.com"},
                         {"name":"Ronald Weasley", "email":"rweasley@gmail.com"}]}
```
{: .nolineno }

Dictionaries can be modified using methods, some of the commonly used methods relating to dictionaries can be found at the [Python Dictionary Methods](https://www.w3schools.com/python/python_ref_dictionary.asp) by W3Schools.

| Method | Description | Example |
| --- | --- | --- |
| clear() | Removes all the elements from the dictionary | `protocols.clear()` |
| fromkeys() | Returns a dictionary with the specified keys and value (value is optional) | `x = ('HTTP', 'HTTPS') protocols = dict.fromkeys(x)` |
| get() | Returns the value of the specified key | `protocols.get('SSH')` |
| keys() | Returns a list containing the dictionary's keys | `protocols.keys()` |
| pop() | Removes the element with the specified key | `protocols.pop('SSH')` |
| popitem() | Removes the last inserted key-value pair | `protocols.popitem()` |
| setdefault() | Returns the value of the specified key. If the key does not exist: insert the key, with the specified value | `protocols.setdefault('SMB')  protocols.setdefault('ELASTICSEARCH', 9200)` |
| update() | Updates the dictionary with the specified key-value pairs | `protocols.update({'SYSLOG': 514})` |
| values() | Returns a list of all the values in the dictionary | `protocols.values()` |


# Functions

Functions are pre-compiled instructions that can be executed to perform certain actions. Python contains a set of built-in functions, however they can be extended by importing modules. In Python, defining a function is done using the ‘def’ keyword. Functions are called by entering the function name followed by a set of parenthesis, any arguments that are to be passed to the function reside within the parenthesis.

## Standard Functions

Some of the built-in and commonly used functions that exist in Python are listed below, a more through list of examples can be found atwithin the Python Documentation for [Built-in Functions](https://docs.python.org/3/library/functions.html).

| Method | Description | Example |
| --- | --- | --- |
| print() | Prints an argument to STDOUT | `print("darkcybe")` |
| int() | Returns an integer number. If a float is passed as an argument, the decimal will be removed. | `int(10.9)` |
| float() | Returns a floating point number | `float(99.9)` |
| str() | Returns a string object | `str(variable)` |
| input() | Reads user input `\n` drops the terminal input to the next line | `input("What is your name?"\n)`

## Defining Functions

Functions can be created to perform and repeat code blocks. To do this, the keyword ‘def’ for define, is used to create a function. The below example shows the process of creating and calling a defined function that prints a customized greeting to the terminal.

Note that the flow of execution through the program does not execute the function until it is called. The variable ‘name’ within the defined function only resides within the function itself and cannot be called, this is referred to as local scope. Typical variables such as ‘input_name’ can be referenced throughout the code and is known as a global variable.

```python
def greeting(name):
    print('Hello', name)

input_name = input('Enter your name: \n')

greeting(input_name)
```
{: .nolineno }

Defining functions structures code and allows for better code reuse. The below example shows a simple dice rolling game that contains two functions; roll_dice which uses the random module to generate two random numbers (simulating two dice being rolled), and the play_game function which is the main function to initiate the game. At the very bottom, the play_game function is called which executes the code.

```python
import random

def roll_dice():
    dice_total = random.randint(1,6) + random.randint(1,6)
    return dice_total

def play_game():
    player1 = input("Enter Player 1's name: ")
    player2 = input("Enter Player 2's name: ")

    roll1 = roll_dice()
    roll2 = roll_dice()

    print(player1, 'rolled', roll1)
    print(player2, 'rolled', roll2)

    if roll1 > roll2:
        print(player1, 'Wins!')
    elif roll1 < roll2:
        print(player2, 'Wins!')
    else:
        print('Tie')

play_game()
```
{: .nolineno }

# Modules/Libraries/Packages

A module in Python is a way of providing useful code to be used by another program, containing functions and other resources. Python contains an already extensive library which can be accessed natively, more information can be found at [The Python Standard Library Documentation](https://docs.python.org/3/library/index.html). More advanced or third-party modules can be found at the [PyPI · The Python Package Index](https://pypi.org/), which maintains a repository of software for the Python Language: 

## Importing a Standard Module

The below example imports the ‘random’ module into Python for use, the Random module implements pseudo-random number generators.

```python
import random
```
{: .nolineno }

## Module Functions

Modules will typically contain functions that allow for actions to be performed. To call a function within a module, first explicitly call the module and then select a function, Visual Studio and other IDEs will populate a list of available functions.

The below example uses a function within the random module to generate a random integer between the numbers 1 and 6 which are set as arguments to the instruction. The value is then stores and is callable via the ‘roll’ variable.

```python
roll = random.randint(1,6)
```
{: .nolineno }

Using the random module as an example again, we can see that there a numerous function available via the [random — Generate pseudo-random numbers](https://docs.python.org/3/library/random.html) document by Python.

```python
choices = ["rock", "paper", "scissors"]
computer_choice = random.choice(choices)
```
{: .nolineno }

## Installing Third-Party Packages

Apart from the extensive libraries available by default with Python, there exist a vat number of specialized packages that have been created by the Python community to extend the capabilities of modules. As previously mentioned, a repository of these packages can be found at [PyPI · The Python Package Index](https://pypi.org/), and also via custom repositories on GitHub.

To install third-party packages, the Python Install Package (PIP) is required, PIP manages the installation of packages from the Python Package Index and can be interacted with via the Python Terminal. In the below example, the package ‘requests’ is downloaded and installed via PIP. The Requests package allows HTTP requests to be made to webservers in order to interact with APIs and retrieve/post data.

```python
pip --version # Returns the version of PIP installed

pip install requests

pip --upgrade requests # If already installed, packages can be upgraded
```
{: .nolineno }

Once installed, a package can then be imported and used in the same manner as built-in modules or libraries. In the example below, the requests package is used to make an outbound connection to a webserver [https://randomuser.me/api/](https://randomuser.me/api/), returning and parsing the data in .json format.

```python
import requests

retrieve = requests.get('https://randomuser.me/api/')
output = retrieve.json()

print(output)
```
{: .nolineno }


An example is shown below of a practical use of the requests package. User information is retrieved from the webserver and parsed to print the username and password. Due to the JSON data being quite overwhelming, online tools such as [JSON Parser](https://jsonparser.org/) can be used to break the dictionaries and lists into a more readable format.

```python
import requests

retrieve = requests.get('https://randomuser.me/api/')
output = retrieve.json()

login = output.get('results')[0].get('login')
print("Username is: ", login.get('username'))
print("Password is: ", login.get('password'))
```
{: .nolineno }

In some circumstances, an API key may be required when interacting with a webserver. In such circumstances, the following can be included in the code. Some webservers will provide instructions on how to structure API calls, including the specific implementation of the API key.

```python
import requests

api_key = "asdfklj43po587sadkfjn3487"
city = "Narnia"
url = "https://api.locationweather.org/data/weather?q="+city+"&appid="+api_key

retrieve = weather.get(url)
json = weather.get()
print(json)
```
{: .nolineno }

## Virtual Environments and Packages

In some circumstances, packages and modules may require specific versions of libraries in order to function. This means that one Python installation may not meet the requirements of every application. The solution is to create a self-contained directory contain specific versions of Python and the required packages. This self-contained Python structure is called a virtual environment (VENV).

To create a virtual environment, the following command is run to create a standalone environment called ‘example’

```python
python -m venv example # A file path such as "C:\Python\Venv\example" can also be used
```
{: .nolineno }

Once the virtual environment has been created, the next step is to set the environment to be used instead of the local Python instance. To activate and deactivate a virtual environment, the following commands can be run on Windows. Once the PowerShell script is run, the venv name, in this case ‘example’ will be highlighted at the beginning of the terminal. Deactivating is done by executing the deactivate batch script and by running exit on the Python terminal.

```powershell
C:\Python\Venv\example\Scripts\activate.ps1
C:\Python\Venv\example\Scripts\deactivate.bat
```
{: .nolineno }

When operating inside of the virtual environment, instructions and commands are as they would be if using the Python instance installed on the host. Once a virtual environment is no longer required, they can be deleted from disk, an empty environment takes up around 15-20MB of space.

# Logical Statements (IF and ELSE)

Programs utilize logical statements to determine execution of code based on conditions. For example, if an condition is true do this, or is false do that.

## IF Statements

IF statements are made up of two parts, the IF keyword followed by a condition and colon (:). The example below is a basic statement that only has one condition, if the condition is met a string will be returned to the console using the ‘print’ function.

```python
age = 33
if age > 29:
    print('''You're in your thirties!''')
```
{: .nolineno }

Additional conditions can be added to logical statements in what is referred to as a ‘block' of code. A block is simply a set of statements. In the below example, additional print functions will be executed upon the condition being met. Note that there are 4 spaces between the starting line and the beginning of the function. Python uses whitespace (singles spaces or tab) to create blocks, code that is in the same position will be grouped, every positional change will create a new group.

```python
age = 33
if age > 29:
    print('''You're in your thirties!''')
    print('''Thats not great''')
```
{: .nolineno }

If statements can be used to check data types such as the below example which checks a list for a specific value.

```python
acronyms = ['HTTPS', 'SSL', 'TLS']
user_input = 'HTTPS'
if user_input in acronyms:
    print(user_input + "Is an encypted channel")
```
{: .nolineno }

## IF-then-ELSE Statements

The above IF statements only perform an action when the condition is true. IF-then-ELSE statements can be introduced to creation actionable conditions for statements that are true or false.

```python
age = 33
if age > 29:
    print('''You're in your thirties!''')
    print('''Thats not great''')
else:
    print('''You're still young, cherish it''')
```
{: .nolineno }

## IF and ELIF Statements

Logical conditions can be extended even further using ELIF (else-if). Unlike the IF-then-ELSE statement which relies of a condition being met once and everything else being treated as false, ELIF can provide more verbose conditioning.

```python
age = 33
if age > 29:
    print('''You're in your thirties!''')
    print('''Thats not great''')
elif age < 29:
    print('''You're still young, cherish it''')
elif age > 39:
    print("Yikes!")
else:
    print('''Weow! You're actually ancient''')
```
{: .nolineno }

## Combining Conditions

Conditions can be combined by using keywords ‘and’ and ‘or’ to provide shorter and simpler code, both statements must be true for an and statement to be true in the below example. Or only requires one of the conditions to be true.

```python
age = 24
if age => 20 and age < 30
    print('''You're in your twenties''')

colour = "red"
if colour == "red" or colour == "yellow"
    print('''Stop''')
```
{: .nolineno }

# Loops

Loops are used to carry out an instruction or set of instructions over a data set. The below example depicts a simple for loop that will print each value within the protocols list. the variable ‘p’ acts as a temporary variable to store the protocols in for each run of the loop.

```python
protocols = ['HTTP', 'HTTPS', 'FTP', 'SSH']
for p in protocols:
    print(p)
```
{: .nolineno }

Loops often make use of Ranges to create lists that can be populated based upon user input, such as the example below.

```python
study_hours = []
x = int(input("How many days of study did you perform this week?\n"))

for i in range(x):
    study_hours.append(float(input("Enter the amount of hours for each day:\n")))
    total = sum(study_hours)

print("Your total amount of study hours this week is: ", total)
```
{: .nolineno }

Loops can be used with the Dictionary data type also, including dictionaries that contained lists. By default however, when a dictionary is printed using a loop it will only return the key and not the attached value. To return both the key and value pair, the below loop can be used. Note that the method .items is used to return the key and value pair.

```python
traffic = {'Allowed': ['HTTP', 'HTTPS'],
           'Blocked': ['FTP', 'SSH']}

for v, k in traffic.items():
    print(v, ':', k)
```
{: .nolineno }

More granular calls can be made to return specific data from nested dictionaries also, such is the example below which contains a dictionary ‘contacts’ containing a key ‘students’ with a nested dictionary containing key and value pairs for ‘name’ and ‘email’. The example depicts how to use a loop to return the ‘email’ from the nested dictionary.

```python
contacts = {
    "number":2,
    "students":
    [
        {"name":"Harry Potter", "email":"hpotter@gmail.com"},
        {"name":"Ronald Weasley", "email":"rweasley@gmail.com"}
    ]
}

for s in contacts["students"]:
    print(s["email"])
```
{: .nolineno }

The above example discuss the ‘for’ loop which is great when a loop is running over a specific length, such as the range inclusions. There is another type of loop however than will run until a specific condition is met, called a ‘while’ loop.

```python
brute_force = 0

while brute_force < 10000
    print(brute_force)
    if authentication == True:
        break
    elif locked_out == True:
        break
    else:
        brute_force = brute_force + 1
```
{: .nolineno }