> **For GPYC/pyWars Preparation**

## Variables and Assignment

### Variable Naming Rules

- Must start with letter or underscore
- Can contain letters, numbers, underscores
- Case-sensitive (`myVar` ≠ `myvar`)
- Cannot use Python keywords

```python
# Valid variables
name = "Alice"
age = 25
_private = "hidden"
user_count = 100

# Invalid variables
# 2fast = "invalid"  # starts with number
# my-var = "invalid"  # contains hyphen
```

## Data Types

### Basic Types

**Integers (int)**

```python
count = 42
negative = -10
large_num = 1_000_000  # underscores for readability
```

**Floats (float)**

```python
price = 19.99
pi = 3.14159
scientific = 1.5e3  # 1500.0
```

**Strings (str)**

```python
name = "Alice"
message = 'Hello'
multiline = """This is
a multiline
string"""
```

**Booleans (bool)**

```python
is_active = True
is_admin = False
```

**None**

```python
result = None  # represents absence of value
```

### Type Checking and Conversion

```python
# Check type
type(42)  # <class 'int'>
isinstance(42, int)  # True

# Convert between types
str(42)       # "42"
int("42")     # 42
float("3.14") # 3.14
int(3.14)     # 3 (truncates)

# Convert with base (for hex, binary, etc)
int("41", 16)  # 65 (hex to decimal)
int("1010", 2) # 10 (binary to decimal)

# Convert to character
chr(65)  # 'A'
ord('A') # 65
```

## Operators

### Arithmetic Operators

```python
10 + 5   # 15 (addition)
10 - 5   # 5 (subtraction)
10 * 5   # 50 (multiplication)
10 / 5   # 2.0 (division, always returns float)
10 // 3  # 3 (floor division, integer result)
10 % 3   # 1 (modulo, remainder)
10 ** 2  # 100 (exponentiation)
```

### Comparison Operators

```python
5 == 5   # True (equal)
5 != 3   # True (not equal)
5 > 3    # True (greater than)
5 < 3    # False (less than)
5 >= 5   # True (greater or equal)
5 <= 4   # False (less or equal)
```

### Logical Operators

```python
True and False  # False
True or False   # True
not True        # False

# Short-circuit evaluation
x = 5
(x > 0) and (x < 10)  # True
```

### Membership Operators

```python
'a' in 'apple'     # True
'x' not in 'apple' # True
5 in [1, 2, 3]     # False
```

## Input and Output

### Print Function

```python
print("Hello")
print("Name:", name)
print(f"Age: {age}")  # f-string (Python 3.6+)

# Multiple values
print("x =", x, "y =", y)

# Custom separator and end
print("a", "b", "c", sep="-")  # a-b-c
print("Hello", end="!")        # Hello! (no newline)
```

### User Input

```python
name = input("Enter your name: ")
age = int(input("Enter your age: "))  # convert to int

# Note: input() always returns a string
```

## Control Structures

### If/Elif/Else

```python
if x > 0:
    print("Positive")
elif x < 0:
    print("Negative")
else:
    print("Zero")

# Ternary operator (one-liner)
result = "Even" if num % 2 == 0 else "Odd"
```

### For Loops

```python
# Iterate over list
for item in [1, 2, 3]:
    print(item)

# Iterate with range
for i in range(5):        # 0, 1, 2, 3, 4
    print(i)

for i in range(1, 5):     # 1, 2, 3, 4
    print(i)

for i in range(0, 10, 2): # 0, 2, 4, 6, 8
    print(i)

# Enumerate (get index and value)
for index, value in enumerate(['a', 'b', 'c']):
    print(f"{index}: {value}")
# Output: 0: a, 1: b, 2: c

# Enumerate starting from 1
for num, value in enumerate(['a', 'b', 'c'], start=1):
    print(f"{num}: {value}")
# Output: 1: a, 2: b, 3: c
```

### While Loops

```python
count = 0
while count < 5:
    print(count)
    count += 1

# Infinite loop with break
while True:
    user_input = input("Enter 'quit' to exit: ")
    if user_input == 'quit':
        break
    print(f"You entered: {user_input}")
```

### Loop Control

```python
# break - exit loop completely
for i in range(10):
    if i == 5:
        break
    print(i)  # prints 0, 1, 2, 3, 4

# continue - skip to next iteration
for i in range(5):
    if i == 2:
        continue
    print(i)  # prints 0, 1, 3, 4

# pass - do nothing (placeholder)
for i in range(5):
    pass  # TODO: implement later
```

## Functions

### Basic Function Definition

```python
def greet(name):
    return f"Hello, {name}!"

result = greet("Alice")  # "Hello, Alice!"
```

### Default Arguments

```python
def greet(name="Guest"):
    return f"Hello, {name}!"

greet()         # "Hello, Guest!"
greet("Alice")  # "Hello, Alice!"
```

### Multiple Return Values

```python
def get_stats(numbers):
    return min(numbers), max(numbers), sum(numbers)

minimum, maximum, total = get_stats([1, 2, 3, 4, 5])
```

### Variable Arguments

```python
# *args - variable number of positional arguments
def add_all(*numbers):
    return sum(numbers)

add_all(1, 2, 3, 4)  # 10

# **kwargs - variable number of keyword arguments
def print_info(**info):
    for key, value in info.items():
        print(f"{key}: {value}")

print_info(name="Alice", age=25, city="NYC")
```

## Common Built-in Functions

```python
# Length
len("hello")    # 5
len([1, 2, 3])  # 3

# Min/Max
max(1, 5, 3)    # 5
min([1, 5, 3])  # 1

# Sum
sum([1, 2, 3])  # 6

# Absolute value
abs(-5)         # 5

# Round
round(3.14159, 2)  # 3.14

# Range (creates sequence of numbers)
range(5)        # 0, 1, 2, 3, 4
range(1, 5)     # 1, 2, 3, 4
range(0, 10, 2) # 0, 2, 4, 6, 8

# Sorted (returns new sorted list)
sorted([3, 1, 2])  # [1, 2, 3]
sorted("cba")      # ['a', 'b', 'c']

# Map (apply function to each item)
list(map(str, [1, 2, 3]))  # ['1', '2', '3']

# Filter (keep items that pass test)
list(filter(lambda x: x > 2, [1, 2, 3, 4]))  # [3, 4]

# Zip (combine multiple iterables)
list(zip([1, 2], ['a', 'b']))  # [(1, 'a'), (2, 'b')]
```

## List Comprehensions

```python
# Basic list comprehension
squares = [x**2 for x in range(5)]  # [0, 1, 4, 9, 16]

# With condition
evens = [x for x in range(10) if x % 2 == 0]  # [0, 2, 4, 6, 8]

# Transform items
upper = [word.upper() for word in ['hello', 'world']]  # ['HELLO', 'WORLD']

# Nested comprehension
matrix = [[i*j for j in range(3)] for i in range(3)]
# [[0, 0, 0], [0, 1, 2], [0, 2, 4]]
```

## Variable Scope

```python
# Global scope
global_var = "accessible everywhere"

def my_function():
    # Local scope
    local_var = "only in function"
    print(global_var)  # can read global
    
# print(local_var)  # ERROR: not accessible outside function

# Modify global from function
count = 0
def increment():
    global count  # declare intent to modify global
    count += 1
```

## Lambda Functions (Anonymous Functions)

```python
# Regular function
def square(x):
    return x ** 2

# Lambda equivalent
square = lambda x: x ** 2

# Common use with map, filter, sorted
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x**2, numbers))

# Sort by custom key
students = [('Alice', 25), ('Bob', 20), ('Charlie', 30)]
sorted(students, key=lambda x: x[1])  # sort by age
```

## Common Mistakes to Avoid

### Integer Division

```python
# Wrong (if you want integer)
5 / 2      # 2.5 (float division)

# Right
5 // 2     # 2 (floor division)
```

### String to Number Conversion

```python
# Wrong
age = input("Age: ")  # age is string "25"
age + 1               # ERROR

# Right
age = int(input("Age: "))
age + 1               # works
```

### Comparing with None

```python
# Wrong
if x == None:
    pass

# Right (more Pythonic)
if x is None:
    pass
```

### Mutable Default Arguments

```python
# Wrong - list is created once and shared
def add_item(item, my_list=[]):
    my_list.append(item)
    return my_list

# Right
def add_item(item, my_list=None):
    if my_list is None:
        my_list = []
    my_list.append(item)
    return my_list
```

## Quick Tips

1. **Use meaningful variable names**: `user_count` not `uc`
2. **Follow naming conventions**: `snake_case` for variables/functions
3. **Comments**: Use `#` for single line, `"""` for docstrings
4. **Indentation**: Use 4 spaces (not tabs)
5. **Assignment vs Comparison**: `=` assigns, `==` compares
6. **String formatting**: Prefer f-strings for readability

## Practice Examples

### Check if number is even

```python
num = 42
if num % 2 == 0:
    print("Even")
else:
    print("Odd")
```

### Swap two variables

```python
a, b = 5, 10
a, b = b, a  # Pythonic swap
```

### Convert temperature

```python
celsius = 25
fahrenheit = (celsius * 9/5) + 32
```

### Find max in list without max()

```python
numbers = [3, 7, 2, 9, 1]
max_num = numbers[0]
for num in numbers:
    if num > max_num:
        max_num = num
```

### Count occurrences

```python
text = "hello world"
count = 0
for char in text:
    if char == 'l':
        count += 1
```

---

**Next**: [Data Structures](https://claude.ai/chat/02_data_structures.md)
