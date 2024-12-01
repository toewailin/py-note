# Python Crash Course

This Python crash course covers fundamental topics, starting from the basics and progressing to more advanced features. It is suitable for beginners and intermediate learners.

## Table of Contents
1. [Introduction](#introduction)
2. [Getting Started](#getting-started)
3. [Basic Syntax](#basic-syntax)
4. [Variables and Data Types](#variables-and-data-types)
5. [Control Flow](#control-flow)
6. [Functions](#functions)
7. [Modules and Packages](#modules-and-packages)
8. [Object-Oriented Programming (OOP)](#object-oriented-programming-oop)
9. [Error Handling](#error-handling)
10. [File I/O](#file-io)
11. [Working with Libraries](#working-with-libraries)
12. [Conclusion](#conclusion)

## Introduction

Welcome to the Python Crash Course! This guide is designed to give you a quick overview of Python, covering all of its essential features with examples. Whether you're just starting out or need a refresher, this course will help you master Python quickly.

---

## Getting Started

To get started, you need to have Python installed on your system. You can download and install Python from the official website: [https://www.python.org/downloads/](https://www.python.org/downloads/).

Once installed, you can check the version by running:

```bash
python --version
```

For interactive work, you can use **IDLE**, **Jupyter Notebooks**, or any text editor (e.g., **VSCode**, **PyCharm**).

---

## Basic Syntax

Python is known for its simple and readable syntax. Here's a basic example:

```python
print("Hello, World!")
```

### Comments

Use `#` for single-line comments and `'''` or `"""` for multi-line comments:

```python
# This is a single-line comment

'''
This is a 
multi-line comment
'''
```

---

## Variables and Data Types

In Python, variables are dynamically typed. You don't need to declare their type.

### Example:

```python
# Integer
age = 25

# String
name = "Alice"

# Float
height = 5.6

# Boolean
is_student = True

# List
fruits = ["apple", "banana", "cherry"]

# Tuple
coordinates = (4, 5)

# Dictionary
person = {
    "name": "Alice",
    "age": 25
}

# Set
unique_numbers = {1, 2, 3, 4}
```

### Type Checking:

```python
print(type(name))  # <class 'str'>
print(type(age))  # <class 'int'>
```

---

## Control Flow

Python supports typical control flow statements such as `if`, `else`, `elif`, loops, and more.

### If-Else Statements:

```python
age = 18

if age >= 18:
    print("You are an adult.")
else:
    print("You are a minor.")
```

### Loops:

#### For Loop:

```python
for fruit in fruits:
    print(fruit)
```

#### While Loop:

```python
count = 0
while count < 3:
    print(count)
    count += 1
```

### List Comprehension:

```python
squared_numbers = [x**2 for x in range(5)]
print(squared_numbers)  # [0, 1, 4, 9, 16]
```

---

## Functions

Functions are reusable blocks of code that perform a specific task.

### Function Definition:

```python
def greet(name):
    print(f"Hello, {name}!")

greet("Alice")  # Hello, Alice!
```

### Return Statement:

```python
def add(a, b):
    return a + b

result = add(2, 3)
print(result)  # 5
```

---

## Modules and Packages

Modules are Python files that contain functions, classes, and variables. You can import these to reuse their functionality.

### Importing Modules:

```python
import math
print(math.sqrt(16))  # 4.0
```

### Importing Specific Functions:

```python
from math import sqrt
print(sqrt(16))  # 4.0
```

### Creating a Custom Module:

Create a file called `my_module.py`:

```python
def hello():
    print("Hello from my module!")

```

Then, import it in your main Python file:

```python
from my_module import hello
hello()  # Hello from my module!
```

---

## Object-Oriented Programming (OOP)

OOP in Python allows you to model real-world entities using classes and objects.

### Class Definition:

```python
class Dog:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def bark(self):
        print(f"{self.name} says Woof!")

# Create an object (instance of the class)
my_dog = Dog("Buddy", 3)
my_dog.bark()  # Buddy says Woof!
```

### Inheritance:

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        print(f"{self.name} makes a sound.")

class Dog(Animal):
    def speak(self):
        print(f"{self.name} barks.")

dog = Dog("Rex")
dog.speak()  # Rex barks.
```

---

## Error Handling

Python uses `try`, `except`, and `finally` to handle errors.

### Example:

```python
try:
    num = int(input("Enter a number: "))
    print(10 / num)
except ZeroDivisionError:
    print("Cannot divide by zero.")
except ValueError:
    print("Invalid input. Please enter a valid number.")
finally:
    print("Execution completed.")
```

---

## File I/O

Python provides built-in functions to read from and write to files.

### Writing to a File:

```python
with open("sample.txt", "w") as file:
    file.write("Hello, file!")
```

### Reading from a File:

```python
with open("sample.txt", "r") as file:
    content = file.read()
    print(content)  # Hello, file!
```

### Appending to a File:

```python
with open("sample.txt", "a") as file:
    file.write("\nAppended text.")
```

---

## Working with Libraries

Python has a vast collection of libraries for various tasks.

### Example: Working with `requests` Library (HTTP Requests):

```bash
pip install requests
```

```python
import requests

response = requests.get("https://jsonplaceholder.typicode.com/posts")
data = response.json()
print(data[0])  # Output the first post
```

---

## Conclusion

This crash course provides a solid foundation for understanding Python and its core features. Whether you're building small scripts or large applications, Python's simplicity and power make it an excellent choice for any developer.

To continue your learning, explore more advanced topics such as:

- Web development (Flask, Django)
- Data analysis (Pandas, NumPy)
- Machine learning (TensorFlow, Scikit-learn)
- Asynchronous programming (asyncio)

---

# Python Crash Course Code Structure

Below is the structure of the Python Crash Course project:

```
python-crash-course/
    ├── README.md          # This file
    ├── my_module.py       # Example custom module
    ├── main.py            # Main script that runs examples
    ├── sample.txt         # File used for file I/O example
    └── requirements.txt   # List of libraries to install
```

---

### **requirements.txt**

```
requests==2.26.0
```

---
