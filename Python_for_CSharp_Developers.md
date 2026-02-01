# Python for C# Developers
## A Complete Transition Guide

---

## Introduction

This guide is designed for C# developers transitioning to Python. It highlights key differences, similarities, and best practices to help you become productive quickly.

---

## Key Differences Overview

| Aspect | C# | Python |
|--------|----|----|
| **Typing** | Static | Dynamic (with optional type hints) |
| **Structure** | Braces `{ }` | Indentation |
| **Compilation** | Compiled to IL | Interpreted (with bytecode) |
| **Paradigm** | Primarily OOP | Multi-paradigm (OOP, functional, procedural) |
| **Memory Management** | Garbage Collection | Garbage Collection (reference counting + GC) |
| **File Extension** | `.cs` | `.py` |

---

## Syntax Comparison

### 1. Variables and Types

**C#:**
```csharp
int age = 25;
string name = "John";
var price = 99.99;  // Type inference
double salary = 50000.0;
bool isActive = true;
```

**Python:**
```python
age = 25
name = "John"
price = 99.99

# With type hints (optional but recommended)
age: int = 25
name: str = "John"
price: float = 99.99
is_active: bool = True
```

---

### 2. Functions and Methods

**C#:**
```csharp
public int Add(int a, int b)
{
    return a + b;
}

public string Greet(string name = "World")
{
    return $"Hello, {name}!";
}

// Expression-bodied member
public int Square(int x) => x * x;
```

**Python:**
```python
def add(a: int, b: int) -> int:
    return a + b

def greet(name: str = "World") -> str:
    return f"Hello, {name}!"

# Lambda (single expression)
square = lambda x: x * x
```

**Key Differences:**
- Python uses `def` keyword for functions
- No access modifiers (public/private) needed at function level
- Return type hints come after `->` 
- Indentation defines the function body

---

### 3. Classes and Objects

**C#:**
```csharp
public class Person
{
    public string Name { get; set; }
    private int age;
    
    public Person(string name, int age)
    {
        Name = name;
        this.age = age;
    }
    
    public void Introduce()
    {
        Console.WriteLine($"I'm {Name}, {age} years old");
    }
    
    public int GetAge()
    {
        return age;
    }
}

// Usage
var person = new Person("Alice", 30);
person.Introduce();
```

**Python:**
```python
class Person:
    def __init__(self, name: str, age: int):
        self.name = name
        self.__age = age  # Private (name mangling)
    
    def introduce(self):
        print(f"I'm {self.name}, {self.__age} years old")
    
    @property
    def age(self):
        return self.__age
    
    @age.setter
    def age(self, value: int):
        if value > 0:
            self.__age = value

# Usage
person = Person("Alice", 30)
person.introduce()
print(person.age)  # Uses property getter
```

**Key Differences:**
- Constructor is `__init__` instead of class name
- `self` is explicit first parameter (like `this` in C#)
- Use `@property` decorator for getters/setters
- No true private members; use `__` prefix for name mangling
- Single underscore `_` indicates "protected" by convention

---

### 4. Inheritance

**C#:**
```csharp
public class Animal
{
    public virtual void MakeSound()
    {
        Console.WriteLine("Some sound");
    }
}

public class Dog : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("Woof!");
    }
}
```

**Python:**
```python
class Animal:
    def make_sound(self):
        print("Some sound")

class Dog(Animal):
    def make_sound(self):  # Override (no keyword needed)
        print("Woof!")
    
    def call_parent(self):
        super().make_sound()  # Call parent method
```

---

### 5. Collections

| Type | C# | Python | Example |
|------|-------|--------|---------|
| **List** | `List<T>` | `list` | `[1, 2, 3]` |
| **Dictionary** | `Dictionary<K,V>` | `dict` | `{'key': 'value'}` |
| **Set** | `HashSet<T>` | `set` | `{1, 2, 3}` |
| **Tuple** | `(T1, T2)` | `tuple` | `(1, 'a')` |
| **Array** | `T[]` | `list` or `numpy.array` | `[1, 2, 3]` |

**C#:**
```csharp
// List
var numbers = new List<int> { 1, 2, 3, 4, 5 };
numbers.Add(6);

// Dictionary
var scores = new Dictionary<string, int>
{
    ["Alice"] = 95,
    ["Bob"] = 87
};

// Set
var uniqueNumbers = new HashSet<int> { 1, 2, 3 };
```

**Python:**
```python
# List
numbers = [1, 2, 3, 4, 5]
numbers.append(6)

# Dictionary
scores = {
    "Alice": 95,
    "Bob": 87
}
scores["Charlie"] = 92

# Set
unique_numbers = {1, 2, 3}
unique_numbers.add(4)

# Tuple (immutable)
coordinates = (10, 20)
```

---

### 6. Control Flow

#### Conditionals

**C#:**
```csharp
if (age >= 18)
{
    Console.WriteLine("Adult");
}
else if (age >= 13)
{
    Console.WriteLine("Teen");
}
else
{
    Console.WriteLine("Child");
}

// Ternary
string status = age >= 18 ? "Adult" : "Minor";
```

**Python:**
```python
if age >= 18:
    print("Adult")
elif age >= 13:
    print("Teen")
else:
    print("Child")

# Ternary (conditional expression)
status = "Adult" if age >= 18 else "Minor"
```

#### Loops

**C# foreach:**
```csharp
foreach (var item in items)
{
    Console.WriteLine(item);
}

// With index
for (int i = 0; i < items.Count; i++)
{
    Console.WriteLine($"{i}: {items[i]}");
}
```

**Python for loop:**
```python
for item in items:
    print(item)

# With index
for i, item in enumerate(items):
    print(f"{i}: {item}")

# Range-based loop
for i in range(10):
    print(i)  # 0 to 9

for i in range(5, 10):
    print(i)  # 5 to 9
```

**C# while:**
```csharp
while (condition)
{
    // Do something
}
```

**Python while:**
```python
while condition:
    # Do something
    pass
```

---

### 7. Exception Handling

**C#:**
```csharp
try
{
    int result = Divide(10, 0);
}
catch (DivideByZeroException ex)
{
    Console.WriteLine($"Error: {ex.Message}");
}
catch (Exception ex)
{
    Console.WriteLine($"General error: {ex.Message}");
}
finally
{
    Console.WriteLine("Cleanup");
}
```

**Python:**
```python
try:
    result = divide(10, 0)
except ZeroDivisionError as ex:
    print(f"Error: {ex}")
except Exception as ex:
    print(f"General error: {ex}")
finally:
    print("Cleanup")

# Raising exceptions
raise ValueError("Invalid value")
```

---

### 8. Null Handling

**C#:**
```csharp
string? name = null;

// Null coalescing
string displayName = name ?? "Guest";

// Null conditional
int? length = name?.Length;

// Null-forgiving operator
string nonNullName = name!;
```

**Python:**
```python
name = None

# Default value
display_name = name if name is not None else "Guest"
# Or using 'or'
display_name = name or "Guest"

# Check for None
if name is None:
    print("Name is not set")

# Optional type hint
from typing import Optional
name: Optional[str] = None
```

---

## Important Python Features

### 1. List Comprehensions

Python offers concise syntax for creating lists, similar to LINQ but more integrated into the language.

**C# LINQ:**
```csharp
var squares = numbers.Select(x => x * x).ToList();
var evens = numbers.Where(x => x % 2 == 0).ToList();
var filtered = numbers.Where(x => x > 5).Select(x => x * 2).ToList();
```

**Python List Comprehensions:**
```python
squares = [x * x for x in numbers]
evens = [x for x in numbers if x % 2 == 0]
filtered = [x * 2 for x in numbers if x > 5]

# Dictionary comprehension
squared_dict = {x: x*x for x in range(5)}  # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

# Set comprehension
unique_squares = {x*x for x in numbers}

# Generator expression (lazy evaluation)
squares_gen = (x * x for x in numbers)
```

---

### 2. Duck Typing

Python follows the principle: "If it walks like a duck and quacks like a duck, it's a duck." Objects are defined by behavior, not explicit interfaces.

**C#:**
```csharp
public interface IAnimal
{
    void Speak();
}

public class Dog : IAnimal
{
    public void Speak() => Console.WriteLine("Woof!");
}

public void MakeSound(IAnimal animal)
{
    animal.Speak();
}
```

**Python:**
```python
# No interface needed!
class Dog:
    def speak(self):
        print("Woof!")

class Cat:
    def speak(self):
        print("Meow!")

def make_sound(animal):
    animal.speak()  # Works with any object that has speak()

make_sound(Dog())  # Works!
make_sound(Cat())  # Also works!
```

---

### 3. Multiple Return Values (Tuple Unpacking)

**C#:**
```csharp
// Using tuple
public (string, int, string) GetUserInfo()
{
    return ("John", 25, "Developer");
}

var (name, age, role) = GetUserInfo();
```

**Python:**
```python
def get_user_info():
    return "John", 25, "Developer"  # Returns a tuple

name, age, role = get_user_info()  # Tuple unpacking

# Can also return as explicit tuple
def get_coordinates():
    return (10, 20)

x, y = get_coordinates()
```

---

### 4. Context Managers (with statement)

**C#:**
```csharp
using (var file = File.OpenRead("data.txt"))
{
    // Use file
    var content = file.ReadToEnd();
}  // Automatically disposed
```

**Python:**
```python
with open("data.txt", "r") as file:
    content = file.read()
# File automatically closed

# Multiple context managers
with open("input.txt") as infile, open("output.txt", "w") as outfile:
    outfile.write(infile.read())
```

---

### 5. Decorators

Decorators are a powerful feature for modifying function/method behavior.

**Python:**
```python
# Simple decorator
def log_execution(func):
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__}")
        result = func(*args, **kwargs)
        print(f"Finished {func.__name__}")
        return result
    return wrapper

@log_execution
def greet(name):
    print(f"Hello, {name}")

greet("Alice")
# Output:
# Calling greet
# Hello, Alice
# Finished greet

# Built-in decorators
class MyClass:
    @staticmethod
    def static_method():
        print("Static method")
    
    @classmethod
    def class_method(cls):
        print(f"Class method of {cls}")
    
    @property
    def value(self):
        return self._value
```

---

### 6. Generators

**Python:**
```python
# Generator function (lazy evaluation)
def fibonacci(n):
    a, b = 0, 1
    for _ in range(n):
        yield a
        a, b = b, a + b

# Usage
for num in fibonacci(10):
    print(num)

# Generator expression
squares = (x*x for x in range(1000000))  # Doesn't create list in memory
```

---

## Common Gotchas for C# Developers

### 1. Mutable Default Arguments

**❌ DON'T do this:**
```python
def add_item(item, items=[]):  # BAD! Default is created once
    items.append(item)
    return items

print(add_item(1))  # [1]
print(add_item(2))  # [1, 2] - Unexpected!
```

**✅ DO this instead:**
```python
def add_item(item, items=None):  # GOOD!
    if items is None:
        items = []
    items.append(item)
    return items

print(add_item(1))  # [1]
print(add_item(2))  # [2] - Expected!
```

---

### 2. Integer Division

**C#:**
```csharp
int result = 5 / 2;      // 2 (integer division)
double result = 5.0 / 2; // 2.5 (float division)
```

**Python:**
```python
result = 5 / 2   # 2.5 (always float division in Python 3)
result = 5 // 2  # 2 (integer division - floor division)
```

---

### 3. No True Private Members

Python doesn't enforce encapsulation like C#. Use naming conventions:

```python
class Example:
    def __init__(self):
        self.public_var = "I'm public"
        self._protected_var = "I'm protected (by convention)"
        self.__private_var = "I'm private (name mangled)"
    
    def public_method(self):
        pass
    
    def _protected_method(self):
        pass
    
    def __private_method(self):
        pass

obj = Example()
print(obj.public_var)      # OK
print(obj._protected_var)  # Works, but indicates "don't use externally"
# print(obj.__private_var) # AttributeError
print(obj._Example__private_var)  # Name mangled - can still access!
```

---

### 4. Pass by Reference vs Pass by Value

**C#:**
```csharp
// Value types passed by value
void ModifyInt(int x) { x = 10; }
int num = 5;
ModifyInt(num);  // num is still 5

// Reference types
void ModifyList(List<int> list) { list.Add(1); }
var myList = new List<int>();
ModifyList(myList);  // myList now contains 1
```

**Python:**
```python
# Immutable objects (int, str, tuple) - similar to value types
def modify_int(x):
    x = 10  # Creates new object

num = 5
modify_int(num)  # num is still 5

# Mutable objects (list, dict, set) - similar to reference types
def modify_list(lst):
    lst.append(1)  # Modifies the original

my_list = []
modify_list(my_list)  # my_list now contains 1
```

---

### 5. Indentation Matters!

**C#:**
```csharp
if (condition)
{
    DoSomething();
    DoSomethingElse();  // Still inside if block
}
```

**Python:**
```python
if condition:
    do_something()
    do_something_else()  # Must be indented to be in if block

# This is outside the if block
do_another_thing()

# Mixing tabs and spaces causes errors!
# Use 4 spaces (PEP 8 standard)
```

---

## Python Best Practices

### 1. Follow PEP 8 Style Guide

```python
# Naming conventions
class MyClass:           # PascalCase for classes
    pass

def my_function():       # snake_case for functions
    pass

MY_CONSTANT = 100        # UPPER_CASE for constants

my_variable = 10         # snake_case for variables

# Spacing
x = 5                    # Space around operators
my_list = [1, 2, 3]      # Space after commas

# Line length: max 79 characters (72 for docstrings)

# Imports at top, grouped:
# 1. Standard library
# 2. Third-party
# 3. Local application
import os
import sys

import numpy as np
import requests

from myapp import models
```

---

### 2. Use Type Hints

```python
from typing import List, Dict, Optional, Union, Tuple, Any

def process_data(
    items: List[str],
    config: Dict[str, int],
    max_count: Optional[int] = None
) -> Optional[str]:
    """
    Process a list of items based on configuration.
    
    Args:
        items: List of items to process
        config: Configuration dictionary
        max_count: Maximum number of items to process (optional)
    
    Returns:
        Processed result or None if empty
    """
    if not items:
        return None
    return items[0]

# Complex types
def complex_function(
    data: Union[int, str],
    coordinates: Tuple[float, float],
    metadata: Dict[str, Any]
) -> List[Tuple[str, int]]:
    pass
```

---

### 3. Use Virtual Environments

Always use virtual environments for project dependencies:

```bash
# Create virtual environment
python -m venv venv

# Activate (Windows)
venv\Scripts\activate

# Activate (Linux/Mac)
source venv/bin/activate

# Install packages
pip install requests numpy pandas

# Save dependencies
pip freeze > requirements.txt

# Install from requirements
pip install -r requirements.txt

# Deactivate
deactivate
```

---

### 4. Use Pythonic Idioms

**❌ Non-Pythonic (C#-style):**
```python
# Don't do this
i = 0
while i < len(items):
    print(items[i])
    i += 1

# Don't do this
result = []
for item in items:
    if item > 0:
        result.append(item * 2)

# Don't do this
if x == True:
    pass
```

**✅ Pythonic:**
```python
# Do this
for item in items:
    print(item)

# Do this
result = [item * 2 for item in items if item > 0]

# Do this
if x:
    pass

# Check for empty collection
if not my_list:  # Instead of: if len(my_list) == 0
    print("Empty")

# String concatenation
name = "Alice"
greeting = f"Hello, {name}"  # Instead of: "Hello, " + name
```

---

### 5. Use Context Managers

```python
# File handling
with open("file.txt", "r") as f:
    data = f.read()

# Custom context manager
from contextlib import contextmanager

@contextmanager
def database_connection():
    conn = create_connection()
    try:
        yield conn
    finally:
        conn.close()

with database_connection() as conn:
    conn.execute("SELECT * FROM users")
```

---

## Essential Libraries

| Category | Library | Purpose | C# Equivalent |
|----------|---------|---------|---------------|
| **Web Client** | `requests` | HTTP client | `HttpClient` |
| **Web Framework** | `Flask`, `Django`, `FastAPI` | Web development | ASP.NET Core |
| **Testing** | `pytest`, `unittest` | Unit testing | xUnit, NUnit, MSTest |
| **Data Science** | `pandas`, `numpy` | Data manipulation | LINQ (limited) |
| **Database ORM** | `SQLAlchemy` | Object-relational mapping | Entity Framework |
| **Async** | `asyncio` | Asynchronous programming | async/await |
| **Serialization** | `json`, `pickle` | Data serialization | System.Text.Json, Newtonsoft.Json |
| **Date/Time** | `datetime` | Date/time handling | DateTime, DateTimeOffset |
| **Regular Expressions** | `re` | Pattern matching | System.Text.RegularExpressions |
| **Logging** | `logging` | Application logging | ILogger, Serilog |

### Installation Examples:

```bash
# Web requests
pip install requests

# Web frameworks
pip install flask
pip install django
pip install fastapi uvicorn

# Testing
pip install pytest

# Data science
pip install pandas numpy matplotlib

# Database
pip install sqlalchemy psycopg2-binary

# Async HTTP
pip install aiohttp
```

---

## Quick Reference Guide

### String Formatting

```python
name = "Alice"
age = 30

# f-strings (Python 3.6+) - RECOMMENDED
message = f"Hello, {name}! Age: {age}"
formatted = f"Price: ${99.99:.2f}"  # Price: $99.99

# .format() method
message = "Hello, {}! Age: {}".format(name, age)
message = "Hello, {name}! Age: {age}".format(name=name, age=age)

# % formatting (old style, avoid)
message = "Hello, %s! Age: %d" % (name, age)
```

---

### File I/O

```python
# Reading entire file
with open("file.txt", "r") as f:
    content = f.read()

# Reading lines
with open("file.txt", "r") as f:
    lines = f.readlines()  # List of lines
    # or
    for line in f:  # Iterate line by line (memory efficient)
        print(line.strip())

# Writing
with open("file.txt", "w") as f:
    f.write("Hello, World!\n")
    f.writelines(["Line 1\n", "Line 2\n"])

# Appending
with open("file.txt", "a") as f:
    f.write("New line\n")

# Reading/writing JSON
import json

with open("data.json", "r") as f:
    data = json.load(f)

with open("output.json", "w") as f:
    json.dump(data, f, indent=2)
```

---

### Lambda Functions

**C#:**
```csharp
var doubled = numbers.Select(x => x * 2);
var evens = numbers.Where(x => x % 2 == 0);
```

**Python:**
```python
# Using map/filter (functional style)
doubled = list(map(lambda x: x * 2, numbers))
evens = list(filter(lambda x: x % 2 == 0, numbers))

# Using list comprehension (more Pythonic)
doubled = [x * 2 for x in numbers]
evens = [x for x in numbers if x % 2 == 0]

# Lambda with sorted
people.sort(key=lambda p: p.age)
sorted_people = sorted(people, key=lambda p: p.name)
```

---

### Working with Dates

```python
from datetime import datetime, timedelta, date

# Current date/time
now = datetime.now()
today = date.today()

# Creating dates
birthday = datetime(1990, 5, 15)
specific_date = date(2025, 1, 1)

# Formatting
formatted = now.strftime("%Y-%m-%d %H:%M:%S")  # 2025-01-15 14:30:00
formatted = now.strftime("%B %d, %Y")  # January 15, 2025

# Parsing
date_obj = datetime.strptime("2025-01-15", "%Y-%m-%d")

# Date arithmetic
tomorrow = today + timedelta(days=1)
next_week = now + timedelta(weeks=1)
age = (today - birthday.date()).days // 365
```

---

### String Operations

```python
text = "Hello, World!"

# Common methods
text.upper()           # "HELLO, WORLD!"
text.lower()           # "hello, world!"
text.strip()           # Remove whitespace
text.replace("Hello", "Hi")
text.split(",")        # ["Hello", " World!"]
",".join(["a", "b"])   # "a,b"

# Checking
text.startswith("Hello")  # True
text.endswith("!")        # True
"World" in text           # True

# Multiline strings
multiline = """
This is a
multiline string
"""

# Raw strings (no escape sequences)
path = r"C:\Users\name\file.txt"
```

---

## Async/Await Pattern

**C#:**
```csharp
public async Task<string> FetchDataAsync(string url)
{
    using var client = new HttpClient();
    return await client.GetStringAsync(url);
}

public async Task ProcessMultipleAsync()
{
    var tasks = new List<Task<string>>
    {
        FetchDataAsync("url1"),
        FetchDataAsync("url2")
    };
    
    var results = await Task.WhenAll(tasks);
}
```

**Python:**
```python
import asyncio
import aiohttp

async def fetch_data(url: str) -> str:
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            return await response.text()

async def process_multiple():
    tasks = [
        fetch_data("url1"),
        fetch_data("url2")
    ]
    
    results = await asyncio.gather(*tasks)
    return results

# Running async code
asyncio.run(process_multiple())
```

---

## Recommended Learning Path

### Week 1-2: Fundamentals
- ✅ Master basic syntax and data types
- ✅ Understand indentation and code structure
- ✅ Practice with lists, dictionaries, and sets
- ✅ Write simple scripts and functions
- ✅ Learn about Python's dynamic typing

### Week 3-4: Intermediate
- ✅ Learn OOP in Python (classes, inheritance, properties)
- ✅ Explore list comprehensions and generators
- ✅ Work with modules and packages
- ✅ Practice file I/O and exception handling
- ✅ Understand decorators basics

### Week 5-6: Advanced
- ✅ Dive into decorators and context managers
- ✅ Learn async/await patterns
- ✅ Explore popular frameworks (Flask, Django, or FastAPI)
- ✅ Study testing with pytest
- ✅ Build a complete project

### Week 7-8: Specialization
- ✅ Choose a domain: Web (Flask/Django), Data Science (pandas/numpy), Automation, or DevOps
- ✅ Learn domain-specific libraries
- ✅ Contribute to open-source projects
- ✅ Build portfolio projects

---

## Common C# to Python Translations

| C# Concept | Python Equivalent |
|------------|-------------------|
| `namespace` | `module` (file) or `package` (directory) |
| `using` | `import` |
| `var` | No keyword needed (dynamic typing) |
| `null` | `None` |
| `bool` | `bool` |
| `string` | `str` |
| `int` | `int` |
| `double` | `float` |
| `List<T>` | `list` |
| `Dictionary<K,V>` | `dict` |
| `HashSet<T>` | `set` |
| `IEnumerable<T>` | `iterable` or `generator` |
| `readonly` | Property with only getter |
| `const` | `UPPERCASE_NAME` convention |
| `static` | `@staticmethod` or `@classmethod` |
| `interface` | Duck typing or `ABC` (Abstract Base Class) |
| `async/await` | `async/await` |
| `Task<T>` | `Coroutine[Any, Any, T]` or `asyncio.Task` |
| `LINQ` | List comprehensions, `map()`, `filter()` |
| `?.` (null conditional) | `and` operator or walrus with check |
| `??` (null coalescing) | `or` operator or ternary |

---

## Project Structure Comparison

### C# Project Structure:
```
MyProject/
├── MyProject.sln
├── MyProject/
│   ├── MyProject.csproj
│   ├── Program.cs
│   ├── Models/
│   │   └── User.cs
│   ├── Services/
│   │   └── UserService.cs
│   └── appsettings.json
└── MyProject.Tests/
    └── MyProject.Tests.csproj
```

### Python Project Structure:
```
my_project/
├── requirements.txt
├── setup.py (optional)
├── README.md
├── my_project/
│   ├── __init__.py
│   ├── main.py
│   ├── models/
│   │   ├── __init__.py
│   │   └── user.py
│   ├── services/
│   │   ├── __init__.py
│   │   └── user_service.py
│   └── config.py
├── tests/
│   ├── __init__.py
│   └── test_user_service.py
└── venv/ (virtual environment)
```

**Key differences:**
- `__init__.py` makes directories into packages
- `requirements.txt` instead of `.csproj`
- Virtual environment (`venv`) for dependencies
- No solution files

---

## Testing Comparison

**C# (xUnit):**
```csharp
public class CalculatorTests
{
    [Fact]
    public void Add_TwoNumbers_ReturnsSum()
    {
        // Arrange
        var calculator = new Calculator();
        
        // Act
        var result = calculator.Add(2, 3);
        
        // Assert
        Assert.Equal(5, result);
    }
    
    [Theory]
    [InlineData(1, 2, 3)]
    [InlineData(0, 0, 0)]
    public void Add_MultipleInputs_ReturnsSum(int a, int b, int expected)
    {
        var calculator = new Calculator();
        Assert.Equal(expected, calculator.Add(a, b));
    }
}
```

**Python (pytest):**
```python
import pytest
from calculator import Calculator

class TestCalculator:
    def test_add_two_numbers_returns_sum(self):
        # Arrange
        calculator = Calculator()
        
        # Act
        result = calculator.add(2, 3)
        
        # Assert
        assert result == 5
    
    @pytest.mark.parametrize("a,b,expected", [
        (1, 2, 3),
        (0, 0, 0),
        (-1, 1, 0),
    ])
    def test_add_multiple_inputs_returns_sum(self, a, b, expected):
        calculator = Calculator()
        assert calculator.add(a, b) == expected

# Running tests
# pytest test_calculator.py
# pytest -v  # Verbose
# pytest --cov=calculator  # With coverage
```

---

## Dependency Injection

**C# (built-in DI):**
```csharp
// Startup.cs
services.AddScoped<IUserRepository, UserRepository>();
services.AddScoped<IUserService, UserService>();

// UserService.cs
public class UserService
{
    private readonly IUserRepository _repository;
    
    public UserService(IUserRepository repository)
    {
        _repository = repository;
    }
}
```

**Python (dependency-injector or manual):**
```python
# Using dependency-injector library
from dependency_injector import containers, providers

class Container(containers.DeclarativeContainer):
    user_repository = providers.Singleton(UserRepository)
    user_service = providers.Factory(
        UserService,
        repository=user_repository
    )

# Or manual dependency injection
class UserService:
    def __init__(self, repository: UserRepository):
        self.repository = repository

# Usage
repository = UserRepository()
service = UserService(repository)
```

---

## Conclusion

Python's philosophy emphasizes readability and simplicity. As a C# developer, you'll find many familiar concepts, but embrace Python's more dynamic and flexible nature. The transition will feel natural as both languages share object-oriented principles, but Python will encourage you to write more concise, expressive code.

### Key Takeaways:

✅ **Embrace duck typing and dynamic features** - Don't fight Python's dynamic nature  
✅ **Follow PEP 8 style guidelines** - Consistency matters in Python  
✅ **Use type hints for better code quality** - They help IDEs and catch bugs  
✅ **Leverage Python's rich standard library** - "Batteries included"  
✅ **Practice writing Pythonic code** - Not C# in Python syntax  
✅ **Use list comprehensions** - More readable than loops for simple transformations  
✅ **Virtual environments are essential** - Always use them for projects  
✅ **Testing is easy with pytest** - Write tests early and often  

### Resources:
- **Official Python Tutorial**: https://docs.python.org/3/tutorial/
- **PEP 8 Style Guide**: https://pep8.org/
- **Real Python**: https://realpython.com/
- **Python Package Index (PyPI)**: https://pypi.org/

---

**Happy coding in Python! 🐍**
