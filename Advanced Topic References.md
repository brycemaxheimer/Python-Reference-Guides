> **For GPYC/pyWars Preparation**

## Object-Oriented Programming (OOP)

### Classes and Objects

```python
# Basic class
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def greet(self):
        return f"Hello, I'm {self.name}"
    
    def __str__(self):
        return f"Person({self.name}, {self.age})"

# Create instance
person = Person("Alice", 25)
print(person.greet())  # "Hello, I'm Alice"
print(person)  # "Person(Alice, 25)"
```

### Class and Instance Attributes

```python
class Counter:
    # Class attribute (shared by all instances)
    total_count = 0
    
    def __init__(self, name):
        # Instance attribute (unique to each instance)
        self.name = name
        self.count = 0
        Counter.total_count += 1
    
    def increment(self):
        self.count += 1

c1 = Counter("A")
c2 = Counter("B")
Counter.total_count  # 2
c1.count  # 0
c2.count  # 0
```

### Methods

```python
class Calculator:
    def __init__(self, value=0):
        self.value = value
    
    # Instance method (needs self)
    def add(self, x):
        self.value += x
        return self
    
    # Class method (works with class, not instance)
    @classmethod
    def from_string(cls, string):
        value = int(string)
        return cls(value)
    
    # Static method (doesn't need class or instance)
    @staticmethod
    def is_even(n):
        return n % 2 == 0

# Usage
calc = Calculator(10)
calc.add(5)  # Instance method

calc2 = Calculator.from_string("20")  # Class method

Calculator.is_even(4)  # Static method - True
```

### Inheritance

```python
# Parent class
class Animal:
    def __init__(self, name):
        self.name = name
    
    def speak(self):
        return "Some sound"

# Child class
class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name)  # Call parent __init__
        self.breed = breed
    
    def speak(self):  # Override parent method
        return "Woof!"
    
    def fetch(self):  # New method
        return f"{self.name} is fetching"

dog = Dog("Buddy", "Golden Retriever")
dog.speak()  # "Woof!"
dog.fetch()  # "Buddy is fetching"
isinstance(dog, Dog)     # True
isinstance(dog, Animal)  # True
```

### Multiple Inheritance

```python
class Flyable:
    def fly(self):
        return "Flying!"

class Swimmable:
    def swim(self):
        return "Swimming!"

class Duck(Flyable, Swimmable):
    def quack(self):
        return "Quack!"

duck = Duck()
duck.fly()   # "Flying!"
duck.swim()  # "Swimming!"
duck.quack() # "Quack!"
```

### Special Methods (Magic Methods)

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    # String representation
    def __str__(self):
        return f"Vector({self.x}, {self.y})"
    
    def __repr__(self):
        return f"Vector({self.x}, {self.y})"
    
    # Addition
    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)
    
    # Equality
    def __eq__(self, other):
        return self.x == other.x and self.y == other.y
    
    # Length
    def __len__(self):
        return int((self.x**2 + self.y**2)**0.5)
    
    # Indexing
    def __getitem__(self, index):
        if index == 0:
            return self.x
        elif index == 1:
            return self.y
        raise IndexError("Index out of range")

v1 = Vector(1, 2)
v2 = Vector(3, 4)
v3 = v1 + v2  # Vector(4, 6)
v1 == v2      # False
len(v1)       # 2
v1[0]         # 1
```

### Properties and Encapsulation

```python
class Temperature:
    def __init__(self, celsius):
        self._celsius = celsius  # "Private" attribute (convention)
    
    @property
    def celsius(self):
        """Get temperature in Celsius"""
        return self._celsius
    
    @celsius.setter
    def celsius(self, value):
        """Set temperature in Celsius"""
        if value < -273.15:
            raise ValueError("Temperature below absolute zero!")
        self._celsius = value
    
    @property
    def fahrenheit(self):
        """Get temperature in Fahrenheit"""
        return self._celsius * 9/5 + 32
    
    @fahrenheit.setter
    def fahrenheit(self, value):
        """Set temperature in Fahrenheit"""
        self.celsius = (value - 32) * 5/9

temp = Temperature(25)
temp.celsius      # 25
temp.fahrenheit   # 77.0
temp.celsius = 30
temp.fahrenheit   # 86.0
```

---

## Exception Handling

### Try/Except Basics

```python
# Basic exception handling
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero!")

# Multiple exceptions
try:
    value = int("abc")
except (ValueError, TypeError) as e:
    print(f"Error: {e}")

# Catch all exceptions (use sparingly)
try:
    risky_operation()
except Exception as e:
    print(f"Error occurred: {e}")

# Else clause (runs if no exception)
try:
    result = 10 / 2
except ZeroDivisionError:
    print("Error!")
else:
    print(f"Result: {result}")

# Finally clause (always runs)
try:
    file = open("file.txt")
    # process file
except FileNotFoundError:
    print("File not found")
finally:
    print("Cleanup code runs always")
```

### Raising Exceptions

```python
# Raise exception
def divide(a, b):
    if b == 0:
        raise ValueError("Division by zero!")
    return a / b

# Re-raise exception
try:
    risky_operation()
except Exception as e:
    log_error(e)
    raise  # Re-raise the same exception

# Raise from another exception
try:
    result = parse_data(data)
except ValueError as e:
    raise RuntimeError("Failed to parse data") from e
```

### Custom Exceptions

```python
# Create custom exception
class InvalidPasswordError(Exception):
    def __init__(self, message, min_length):
        super().__init__(message)
        self.min_length = min_length

def validate_password(password):
    if len(password) < 8:
        raise InvalidPasswordError("Password too short", min_length=8)

# Usage
try:
    validate_password("abc")
except InvalidPasswordError as e:
    print(f"{e} (minimum: {e.min_length})")
```

### Context Managers

```python
# Using with statement (context manager)
with open("file.txt", "r") as f:
    content = f.read()
# File automatically closed

# Custom context manager
class Timer:
    def __enter__(self):
        import time
        self.start = time.time()
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        import time
        self.end = time.time()
        print(f"Elapsed: {self.end - self.start:.2f}s")
        return False  # Don't suppress exceptions

# Usage
with Timer():
    # Code to time
    time.sleep(1)

# Using contextlib
from contextlib import contextmanager

@contextmanager
def file_manager(filename, mode):
    f = open(filename, mode)
    try:
        yield f
    finally:
        f.close()

with file_manager("file.txt", "r") as f:
    content = f.read()
```

---

## Decorators

### Function Decorators

```python
# Simple decorator
def my_decorator(func):
    def wrapper(*args, **kwargs):
        print("Before function")
        result = func(*args, **kwargs)
        print("After function")
        return result
    return wrapper

@my_decorator
def greet(name):
    print(f"Hello, {name}!")

greet("Alice")
# Output:
# Before function
# Hello, Alice!
# After function
```

### Decorator with Arguments

```python
def repeat(times):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for _ in range(times):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator

@repeat(3)
def greet(name):
    print(f"Hello, {name}!")

greet("Alice")
# Output:
# Hello, Alice!
# Hello, Alice!
# Hello, Alice!
```

### Common Decorators

```python
import time
from functools import wraps

# Timer decorator
def timer(func):
    @wraps(func)  # Preserves function metadata
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"{func.__name__} took {end - start:.4f}s")
        return result
    return wrapper

# Memoization (caching)
def memoize(func):
    cache = {}
    @wraps(func)
    def wrapper(*args):
        if args not in cache:
            cache[args] = func(*args)
        return cache[args]
    return wrapper

# Built-in lru_cache
from functools import lru_cache

@lru_cache(maxsize=128)
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)
```

---

## Generators and Iterators

### Generators

```python
# Generator function (uses yield)
def countdown(n):
    while n > 0:
        yield n
        n -= 1

# Usage
for i in countdown(5):
    print(i)  # 5, 4, 3, 2, 1

# Generator expression
squares = (x**2 for x in range(10))
list(squares)  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

### Generator Advantages

```python
# Memory efficient - doesn't create entire list
def read_large_file(file_path):
    """Read file line by line (memory efficient)"""
    with open(file_path, 'r') as f:
        for line in f:
            yield line.strip()

# Process large file without loading all into memory
for line in read_large_file('huge_file.txt'):
    process(line)

# Infinite generator
def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

# Get first 10 Fibonacci numbers
fib = fibonacci()
first_10 = [next(fib) for _ in range(10)]
```

### Custom Iterators

```python
class Countdown:
    def __init__(self, start):
        self.current = start
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.current <= 0:
            raise StopIteration
        self.current -= 1
        return self.current + 1

# Usage
for i in Countdown(5):
    print(i)  # 5, 4, 3, 2, 1
```

---

## Comprehensions (Advanced)

### Nested Comprehensions

```python
# Flatten nested list
nested = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flat = [item for sublist in nested for item in sublist]
# [1, 2, 3, 4, 5, 6, 7, 8, 9]

# Matrix transpose
matrix = [[1, 2, 3], [4, 5, 6]]
transposed = [[row[i] for row in matrix] for i in range(len(matrix[0]))]
# [[1, 4], [2, 5], [3, 6]]

# Cartesian product
colors = ['red', 'blue']
sizes = ['S', 'M', 'L']
products = [(color, size) for color in colors for size in sizes]
# [('red', 'S'), ('red', 'M'), ('red', 'L'), ('blue', 'S'), ('blue', 'M'), ('blue', 'L')]
```

### Dictionary and Set Comprehensions

```python
# Dictionary comprehension
squares = {x: x**2 for x in range(5)}
# {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

# Invert dictionary
original = {'a': 1, 'b': 2, 'c': 3}
inverted = {v: k for k, v in original.items()}
# {1: 'a', 2: 'b', 3: 'c'}

# Filter dictionary
scores = {'Alice': 85, 'Bob': 92, 'Charlie': 78}
high_scores = {k: v for k, v in scores.items() if v >= 80}
# {'Alice': 85, 'Bob': 92}

# Set comprehension
unique_lengths = {len(word) for word in ['hello', 'world', 'hi']}
# {2, 5}
```

---

## Lambda Functions and Functional Programming

### Lambda Functions

```python
# Lambda basics
square = lambda x: x**2
add = lambda x, y: x + y

# With map
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x**2, numbers))

# With filter
evens = list(filter(lambda x: x % 2 == 0, numbers))

# With sorted
students = [('Alice', 85), ('Bob', 92), ('Charlie', 78)]
sorted_by_score = sorted(students, key=lambda x: x[1], reverse=True)
```

### Functional Tools

```python
from functools import reduce

# reduce - apply function cumulatively
numbers = [1, 2, 3, 4, 5]
total = reduce(lambda x, y: x + y, numbers)  # 15
product = reduce(lambda x, y: x * y, numbers)  # 120

# map - apply function to each item
squared = list(map(lambda x: x**2, numbers))

# filter - keep items that pass test
evens = list(filter(lambda x: x % 2 == 0, numbers))

# any/all
has_even = any(x % 2 == 0 for x in numbers)  # True
all_positive = all(x > 0 for x in numbers)   # True
```

---

## Concurrency and Parallelism

### Threading

```python
import threading
import time

def worker(name, delay):
    print(f"{name} starting")
    time.sleep(delay)
    print(f"{name} finished")

# Create threads
threads = []
for i in range(3):
    t = threading.Thread(target=worker, args=(f"Thread-{i}", i+1))
    threads.append(t)
    t.start()

# Wait for all threads to complete
for t in threads:
    t.join()

print("All threads completed")
```

### Thread Safety

```python
import threading

# Thread-safe counter
class Counter:
    def __init__(self):
        self.count = 0
        self.lock = threading.Lock()
    
    def increment(self):
        with self.lock:
            self.count += 1
    
    def get_count(self):
        with self.lock:
            return self.count

counter = Counter()

def worker():
    for _ in range(1000):
        counter.increment()

threads = [threading.Thread(target=worker) for _ in range(10)]
for t in threads:
    t.start()
for t in threads:
    t.join()

print(counter.get_count())  # 10000
```

### Multiprocessing

```python
from multiprocessing import Process, Pool
import os

def worker(name):
    print(f"{name} running in process {os.getpid()}")

# Create processes
if __name__ == '__main__':
    processes = []
    for i in range(3):
        p = Process(target=worker, args=(f"Process-{i}",))
        processes.append(p)
        p.start()
    
    for p in processes:
        p.join()
```

### Process Pool

```python
from multiprocessing import Pool

def square(x):
    return x * x

if __name__ == '__main__':
    # Create pool of workers
    with Pool(processes=4) as pool:
        # Map function to data
        numbers = [1, 2, 3, 4, 5]
        results = pool.map(square, numbers)
        print(results)  # [1, 4, 9, 16, 25]
```

---

## Working with JSON and APIs

### JSON Operations

```python
import json

# Python to JSON
data = {
    'name': 'Alice',
    'age': 25,
    'languages': ['Python', 'JavaScript']
}
json_string = json.dumps(data, indent=2)

# JSON to Python
parsed = json.loads(json_string)

# Read from file
with open('data.json', 'r') as f:
    data = json.load(f)

# Write to file
with open('output.json', 'w') as f:
    json.dump(data, f, indent=2)
```

### REST API Patterns

```python
import requests

# GET request
response = requests.get('https://api.example.com/users')
users = response.json()

# POST request
new_user = {'name': 'Alice', 'email': 'alice@example.com'}
response = requests.post('https://api.example.com/users', json=new_user)

# PUT request (update)
updated_user = {'name': 'Alice Smith'}
response = requests.put('https://api.example.com/users/1', json=updated_user)

# DELETE request
response = requests.delete('https://api.example.com/users/1')

# Handle pagination
def get_all_pages(base_url):
    results = []
    page = 1
    while True:
        response = requests.get(f"{base_url}?page={page}")
        data = response.json()
        if not data:
            break
        results.extend(data)
        page += 1
    return results
```

---

## Command Line Arguments

### argparse

```python
import argparse

# Create parser
parser = argparse.ArgumentParser(description='Process some data')

# Add arguments
parser.add_argument('input', help='Input file path')
parser.add_argument('-o', '--output', help='Output file path')
parser.add_argument('-v', '--verbose', action='store_true', help='Verbose output')
parser.add_argument('--count', type=int, default=10, help='Number of items')

# Parse arguments
args = parser.parse_args()

print(f"Input: {args.input}")
print(f"Output: {args.output}")
print(f"Verbose: {args.verbose}")
print(f"Count: {args.count}")

# Run with: python script.py input.txt -o output.txt -v --count 5
```

### sys.argv (Simple)

```python
import sys

# Get command line arguments
if len(sys.argv) < 2:
    print("Usage: python script.py <filename>")
    sys.exit(1)

filename = sys.argv[1]
print(f"Processing {filename}")
```

---

## Regular Expression Advanced

### Named Groups and Backreferences

```python
import re

# Named groups
pattern = r'(?P<year>\d{4})-(?P<month>\d{2})-(?P<day>\d{2})'
match = re.search(pattern, '2024-01-15')
if match:
    print(match.group('year'))   # '2024'
    print(match.group('month'))  # '01'
    print(match.groupdict())     # {'year': '2024', 'month': '01', 'day': '15'}

# Backreferences
pattern = r'(\w+)\s+\1'  # Match repeated word
text = "hello hello world"
match = re.search(pattern, text)
if match:
    print(match.group())  # 'hello hello'
```

### Lookahead and Lookbehind

```python
import re

# Positive lookahead (?=...)
# Match 'foo' only if followed by 'bar'
pattern = r'foo(?=bar)'
re.findall(pattern, 'foobar foobaz')  # ['foo']

# Negative lookahead (?!...)
# Match 'foo' only if NOT followed by 'bar'
pattern = r'foo(?!bar)'
re.findall(pattern, 'foobar foobaz')  # ['foo']

# Positive lookbehind (?<=...)
# Match digits preceded by '$'
pattern = r'(?<=\$)\d+'
re.findall(pattern, 'Price: $100')  # ['100']

# Negative lookbehind (?<!...)
# Match digits NOT preceded by '$'
pattern = r'(?<!\$)\d+'
re.findall(pattern, 'Price: $100 and 50 items')  # ['00', '50']
```

---

## Performance Optimization

### Timing Code

```python
import time

# Simple timing
start = time.time()
# Code to time
result = expensive_operation()
end = time.time()
print(f"Took {end - start:.4f} seconds")

# timeit module (more accurate)
import timeit

# Time a statement
time_taken = timeit.timeit('x = sum(range(100))', number=10000)
print(f"Average: {time_taken/10000:.6f} seconds")

# Time a function
def my_function():
    return sum(range(100))

time_taken = timeit.timeit(my_function, number=10000)
```

### Profiling

```python
import cProfile
import pstats

# Profile code
cProfile.run('my_function()')

# Save profile to file
cProfile.run('my_function()', 'profile_stats')

# Analyze profile
stats = pstats.Stats('profile_stats')
stats.sort_stats('cumulative')
stats.print_stats(10)  # Top 10 functions
```

---

## Useful Standard Library Modules

### collections

```python
from collections import Counter, defaultdict, namedtuple, deque

# Counter - count hashable objects
words = ['apple', 'banana', 'apple', 'cherry', 'banana', 'apple']
counts = Counter(words)
counts.most_common(2)  # [('apple', 3), ('banana', 2)]

# defaultdict - dictionary with default values
groups = defaultdict(list)
groups['a'].append(1)  # No KeyError!

# namedtuple - tuple with named fields
Point = namedtuple('Point', ['x', 'y'])
p = Point(10, 20)
p.x  # 10

# deque - double-ended queue
queue = deque([1, 2, 3])
queue.append(4)       # Add to right
queue.appendleft(0)   # Add to left
queue.pop()           # Remove from right
queue.popleft()       # Remove from left
```

### itertools

```python
from itertools import combinations, permutations, product, chain

# Combinations
list(combinations([1, 2, 3], 2))  # [(1, 2), (1, 3), (2, 3)]

# Permutations
list(permutations([1, 2, 3], 2))  # [(1, 2), (1, 3), (2, 1), (2, 3), (3, 1), (3, 2)]

# Cartesian product
list(product([1, 2], ['a', 'b']))  # [(1, 'a'), (1, 'b'), (2, 'a'), (2, 'b')]

# Chain - flatten iterables
list(chain([1, 2], [3, 4], [5, 6]))  # [1, 2, 3, 4, 5, 6]
```

---

## Quick Reference Card

```python
# OOP
class MyClass:
    def __init__(self, value):
        self.value = value
    
    @property
    def computed(self):
        return self.value * 2

# Exception handling
try:
    risky_code()
except SpecificError as e:
    handle_error(e)
finally:
    cleanup()

# Decorators
@decorator
def function():
    pass

# Generators
def gen():
    yield value

# Lambda
lambda x: x * 2

# Threading
import threading
t = threading.Thread(target=func, args=(arg,))
t.start()
t.join()

# JSON
import json
data = json.loads(json_string)
json.dump(data, file)
```

---
