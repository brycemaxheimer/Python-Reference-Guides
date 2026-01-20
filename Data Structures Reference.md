> **For GPYC/pyWars Preparation**

## Lists

### Creating Lists

```python
# Empty list
my_list = []
my_list = list()

# With initial values
numbers = [1, 2, 3, 4, 5]
mixed = [1, "hello", 3.14, True]
nested = [[1, 2], [3, 4], [5, 6]]
```

### Accessing Elements

```python
numbers = [10, 20, 30, 40, 50]

# Indexing (0-based)
numbers[0]   # 10 (first element)
numbers[-1]  # 50 (last element)
numbers[-2]  # 40 (second from end)

# Slicing [start:stop:step]
numbers[1:4]    # [20, 30, 40] (index 1 to 3)
numbers[:3]     # [10, 20, 30] (first 3)
numbers[2:]     # [30, 40, 50] (from index 2 to end)
numbers[::2]    # [10, 30, 50] (every 2nd element)
numbers[::-1]   # [50, 40, 30, 20, 10] (reverse)
```

### Modifying Lists

```python
# Add single element
numbers.append(60)        # [10, 20, 30, 40, 50, 60]

# Add multiple elements
numbers.extend([70, 80])  # [10, 20, 30, 40, 50, 60, 70, 80]

# Insert at specific position
numbers.insert(0, 5)      # [5, 10, 20, 30, 40, 50, 60, 70, 80]

# Remove by value
numbers.remove(20)        # [5, 10, 30, 40, 50, 60, 70, 80]

# Remove by index
numbers.pop(0)            # removes and returns 5
numbers.pop()             # removes and returns last element (80)

# Clear all elements
numbers.clear()           # []

# Change element
numbers[0] = 100
```

### List Operations

```python
# Length
len([1, 2, 3])  # 3

# Concatenation
[1, 2] + [3, 4]  # [1, 2, 3, 4]

# Repetition
[1, 2] * 3  # [1, 2, 1, 2, 1, 2]

# Membership
3 in [1, 2, 3]      # True
5 not in [1, 2, 3]  # True

# Count occurrences
[1, 2, 2, 3].count(2)  # 2

# Find index
[1, 2, 3].index(2)  # 1 (first occurrence)
```

### Sorting Lists

```python
numbers = [3, 1, 4, 1, 5]

# Sort in place (modifies original)
numbers.sort()           # [1, 1, 3, 4, 5]
numbers.sort(reverse=True)  # [5, 4, 3, 1, 1]

# Return new sorted list (original unchanged)
sorted_nums = sorted(numbers)  # returns new list

# Sort with key function
words = ['banana', 'pie', 'Washington', 'book']
words.sort(key=len)  # sort by length
words.sort(key=str.lower)  # case-insensitive sort
```

### List Comprehensions

```python
# Basic
squares = [x**2 for x in range(5)]  # [0, 1, 4, 9, 16]

# With condition
evens = [x for x in range(10) if x % 2 == 0]  # [0, 2, 4, 6, 8]

# Transform and filter
words = ['hello', 'world', 'python']
caps = [w.upper() for w in words if len(w) > 4]  # ['HELLO', 'WORLD', 'PYTHON']

# Nested
matrix = [[i*j for j in range(3)] for i in range(3)]
```

### Common List Patterns

```python
# Reverse a list
numbers[::-1]
numbers.reverse()  # in-place

# Copy a list
new_list = old_list.copy()
new_list = old_list[:]
new_list = list(old_list)

# Flatten nested list
nested = [[1, 2], [3, 4], [5, 6]]
flat = [item for sublist in nested for item in sublist]  # [1, 2, 3, 4, 5, 6]

# Remove duplicates (loses order)
unique = list(set(my_list))

# Filter None values
filtered = [x for x in my_list if x is not None]
```

---

## Dictionaries

### Creating Dictionaries

```python
# Empty dictionary
my_dict = {}
my_dict = dict()

# With initial values
person = {
    "name": "Alice",
    "age": 25,
    "city": "NYC"
}

# From list of tuples
dict([("a", 1), ("b", 2)])  # {'a': 1, 'b': 2}

# Dict comprehension
squares = {x: x**2 for x in range(5)}  # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```

### Accessing Dictionary Elements

```python
person = {"name": "Alice", "age": 25}

# Access by key
person["name"]  # "Alice"

# Access with default (safer)
person.get("name")        # "Alice"
person.get("email")       # None
person.get("email", "N/A")  # "N/A" (default value)

# Check if key exists
"name" in person    # True
"email" in person   # False
```

### Modifying Dictionaries

```python
# Add or update
person["email"] = "alice@example.com"
person["age"] = 26  # updates existing

# Update multiple values
person.update({"age": 27, "city": "LA"})

# Remove by key
del person["city"]

# Remove and return value
age = person.pop("age")  # returns 27
person.pop("phone", None)  # safe removal with default

# Remove and return last inserted item (Python 3.7+)
person.popitem()

# Clear all items
person.clear()
```

### Dictionary Methods

```python
person = {"name": "Alice", "age": 25, "city": "NYC"}

# Get all keys
person.keys()    # dict_keys(['name', 'age', 'city'])
list(person.keys())  # ['name', 'age', 'city']

# Get all values
person.values()  # dict_values(['Alice', 25, 'NYC'])
list(person.values())  # ['Alice', 25, 'NYC']

# Get all key-value pairs
person.items()   # dict_items([('name', 'Alice'), ('age', 25), ('city', 'NYC')])

# Iterate over dictionary
for key in person:
    print(key, person[key])

for key, value in person.items():
    print(f"{key}: {value}")
```

### Dictionary Comprehensions

```python
# Create from lists
keys = ['a', 'b', 'c']
values = [1, 2, 3]
my_dict = {k: v for k, v in zip(keys, values)}  # {'a': 1, 'b': 2, 'c': 3}

# Transform dictionary
original = {'a': 1, 'b': 2, 'c': 3}
doubled = {k: v*2 for k, v in original.items()}  # {'a': 2, 'b': 4, 'c': 6}

# Filter dictionary
filtered = {k: v for k, v in original.items() if v > 1}  # {'b': 2, 'c': 3}
```

### Common Dictionary Patterns

```python
# Get value or set default
count = counts.setdefault(key, 0)
counts[key] = counts.setdefault(key, 0) + 1

# Merge dictionaries (Python 3.9+)
dict1 = {'a': 1, 'b': 2}
dict2 = {'c': 3, 'd': 4}
merged = dict1 | dict2  # {'a': 1, 'b': 2, 'c': 3, 'd': 4}

# Merge (Python 3.5+)
merged = {**dict1, **dict2}

# Invert dictionary (swap keys and values)
inverted = {v: k for k, v in my_dict.items()}

# Group by key
from collections import defaultdict
grouped = defaultdict(list)
for item in items:
    grouped[item.category].append(item)

# Find key with max value
max_key = max(my_dict, key=my_dict.get)
```

---

## Sets

### Creating Sets

```python
# Empty set (note: {} creates empty dict, not set)
my_set = set()

# With initial values
numbers = {1, 2, 3, 4, 5}
mixed = {1, "hello", 3.14}  # can contain different types

# From list (removes duplicates)
unique = set([1, 2, 2, 3, 3, 3])  # {1, 2, 3}
```

### Set Operations

```python
# Add element
numbers.add(6)

# Add multiple elements
numbers.update([7, 8, 9])

# Remove element (raises error if not found)
numbers.remove(5)

# Remove element (no error if not found)
numbers.discard(100)  # does nothing

# Remove and return arbitrary element
numbers.pop()

# Clear all elements
numbers.clear()

# Membership
3 in numbers  # True
```

### Mathematical Set Operations

```python
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}

# Union (all elements from both)
a | b           # {1, 2, 3, 4, 5, 6}
a.union(b)      # {1, 2, 3, 4, 5, 6}

# Intersection (common elements)
a & b                # {3, 4}
a.intersection(b)    # {3, 4}

# Difference (in a but not in b)
a - b             # {1, 2}
a.difference(b)   # {1, 2}

# Symmetric difference (in either but not both)
a ^ b                        # {1, 2, 5, 6}
a.symmetric_difference(b)    # {1, 2, 5, 6}

# Subset
{1, 2}.issubset({1, 2, 3})     # True

# Superset
{1, 2, 3}.issuperset({1, 2})   # True
```

### Set Comprehensions

```python
# Create set of squares
squares = {x**2 for x in range(5)}  # {0, 1, 4, 9, 16}

# Filter
evens = {x for x in range(10) if x % 2 == 0}  # {0, 2, 4, 6, 8}
```

### Common Set Patterns

```python
# Remove duplicates from list
unique_list = list(set(my_list))

# Check if lists have common elements
bool(set(list1) & set(list2))

# Get unique elements across multiple lists
unique = set(list1) | set(list2) | set(list3)
```

---

## Tuples

### Creating Tuples

```python
# Empty tuple
empty = ()
empty = tuple()

# Single element (note the comma!)
single = (5,)  # tuple
not_tuple = (5)  # just int 5

# Multiple elements
coordinates = (10, 20)
person = ("Alice", 25, "NYC")

# Without parentheses (tuple packing)
point = 10, 20  # (10, 20)
```

### Accessing Tuple Elements

```python
point = (10, 20, 30)

# Indexing
point[0]   # 10
point[-1]  # 30

# Slicing
point[1:]  # (20, 30)

# Unpacking
x, y, z = point  # x=10, y=20, z=30

# Unpacking with *
first, *rest = (1, 2, 3, 4)  # first=1, rest=[2, 3, 4]
*start, last = (1, 2, 3, 4)  # start=[1, 2, 3], last=4
```

### Tuple Operations

```python
# Length
len((1, 2, 3))  # 3

# Concatenation
(1, 2) + (3, 4)  # (1, 2, 3, 4)

# Repetition
(1, 2) * 3  # (1, 2, 1, 2, 1, 2)

# Membership
2 in (1, 2, 3)  # True

# Count occurrences
(1, 2, 2, 3).count(2)  # 2

# Find index
(1, 2, 3).index(2)  # 1
```

### Why Use Tuples?

```python
# Immutable - can't be changed after creation
point = (10, 20)
# point[0] = 15  # ERROR: tuples are immutable

# Use as dictionary keys (lists can't)
locations = {
    (0, 0): "origin",
    (1, 0): "east",
    (0, 1): "north"
}

# Return multiple values from function
def get_coordinates():
    return 10, 20  # returns tuple

x, y = get_coordinates()

# Named tuples (more readable)
from collections import namedtuple
Point = namedtuple('Point', ['x', 'y'])
p = Point(10, 20)
print(p.x, p.y)  # 10 20
```

---

## Choosing the Right Data Structure

|Data Structure|Ordered|Mutable|Duplicates|Use Case|
|---|---|---|---|---|
|**List**|Yes|Yes|Yes|Ordered collection, frequent changes|
|**Tuple**|Yes|No|Yes|Immutable data, dictionary keys, multiple return values|
|**Set**|No|Yes|No|Unique values, membership testing, math operations|
|**Dictionary**|Yes*|Yes|Keys: No, Values: Yes|Key-value pairs, lookups|

*Dictionaries maintain insertion order since Python 3.7

---

## Common Patterns Across Data Structures

### Iteration

```python
# Lists
for item in my_list:
    print(item)

# Dictionaries
for key, value in my_dict.items():
    print(key, value)

# Sets
for item in my_set:
    print(item)

# Tuples
for item in my_tuple:
    print(item)
```

### Membership Testing

```python
# Fast for sets and dictionaries
item in my_set        # O(1) average
key in my_dict        # O(1) average

# Slower for lists and tuples
item in my_list       # O(n)
item in my_tuple      # O(n)
```

### Converting Between Types

```python
# List to set (remove duplicates)
unique_items = set(my_list)

# Set to list (for sorting)
sorted_items = sorted(my_set)

# Dictionary keys to list
keys_list = list(my_dict.keys())

# Tuple to list (to make mutable)
mutable = list(my_tuple)

# List to tuple (to make immutable)
immutable = tuple(my_list)
```

---

## Advanced Collections (stdlib)

### Counter (counting hashable objects)

```python
from collections import Counter

# Count elements
counts = Counter([1, 2, 2, 3, 3, 3])  # Counter({3: 3, 2: 2, 1: 1})

# Most common elements
counts.most_common(2)  # [(3, 3), (2, 2)]

# Update counts
counts.update([1, 1, 1])
```

### defaultdict (dictionary with default values)

```python
from collections import defaultdict

# Group items
groups = defaultdict(list)
for item in items:
    groups[item.category].append(item)

# Count items
counts = defaultdict(int)
for item in items:
    counts[item] += 1
```

### deque (double-ended queue)

```python
from collections import deque

# Efficient appending/popping from both ends
queue = deque([1, 2, 3])
queue.append(4)        # append right
queue.appendleft(0)    # append left
queue.pop()            # pop right
queue.popleft()        # pop left
```

---

## Performance Comparison

### Time Complexity (Big O)

|Operation|List|Set|Dict|
|---|---|---|---|
|Access by index/key|O(1)|N/A|O(1)|
|Search|O(n)|O(1)|O(1)|
|Insert|O(1)*|O(1)|O(1)|
|Delete|O(n)|O(1)|O(1)|
|Iteration|O(n)|O(n)|O(n)|

*Appending to end of list is O(1), inserting elsewhere is O(n)

---
