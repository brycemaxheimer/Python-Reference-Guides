---

## Table of Contents

1. [Python Fundamentals](#python-fundamentals)
2. [Data Structures](#data-structures)
3. [File I/O Operations](#file-io-operations)
4. [String Manipulation & Regex](#string-manipulation--regex)
5. [Network Programming](#network-programming)
6. [Registry Operations](#registry-operations)
7. [Cryptography & XOR](#cryptography--xor)
8. [Image Processing](#image-processing)
9. [Common Patterns & Gotchas](#common-patterns--gotchas)

---

## Python Fundamentals

### List Append Return Value
**Concept:** Understanding that `list.append()` returns `None`, not the modified list.

**Common Mistake:**
```python
def lab225(data):
    print(data.append("Pywars rocks"))  # Prints None!
    pass
```

**Correct Approach:**
```python
def lab225(data):
    data.append("Pywars rocks")
    print(data)  # or return data
```

**Key Takeaway:** Many list methods (`append()`, `sort()`, `reverse()`, `extend()`) modify in-place and return `None`. Don't wrap them in print or return statements.

---

### Basic Divisibility Check
**Concept:** Finding numbers divisible by a given value using modulo operator.

**Initial Attempt (Missing Return):**
```python
def lab229(data):
    nums = range(1, 1000)
    evendiv = []
    for num in nums:
        if (num % data) == 0:
            evendiv.append(num)
    # Missing return!
```

**Correct Solution:**
```python
def lab229(data):
    nums = range(1, 1000)
    evendiv = []
    for num in nums:
        if (num % data) == 0:
            evendiv.append(num)
    return evendiv  # Must return the list
```

**Key Learning:**
- Use modulo `%` to check divisibility
- Remember to `return` your result
- When debugging, print BEFORE return statements (not after)

**Best Practice with List Comprehension:**
```python
def lab229(data):
    return [num for num in range(1, 1000) if num % data == 0]
```

---

## Data Structures

### Working with Lists of Lists
**Concept:** Flattening and removing duplicates from nested lists.

**Problem:** Given `[[3,2,1],[3,4,5]]`, return sorted unique values.

**Solution:**
```python
def flatten_and_unique(the_data):
    newlist = []
    for data in the_data:
        for d in data:
            newlist.append(d)
    
    # Remove duplicates with set, then sort
    newlist = sorted(set(newlist))
    return newlist
```

**Key Concepts:**
- **Sets automatically remove duplicates**
- Can convert: list → set → list
- `sorted()` returns a new sorted list
- `.sort()` sorts in-place and returns None

**One-Liner Alternative:**
```python
def flatten_and_unique(the_data):
    return sorted(set([item for sublist in the_data for item in sublist]))
```

---

### Working with Dictionaries - Lab 43x Series
**Concept:** Analyzing OS vulnerability data by month.

**Problem:** Sum vulnerabilities for specific OS versions per month, find highest.

**Solution Structure:**
```python
def calculate_answer(data):
    vulnerable_totals = {}  # MUST be outside loop!
    
    for month_key in data:
        month_num = int(month_key.split("-")[0])
        os_dict = data[month_key]
        
        # Convert strings to float before adding
        total = (float(os_dict.get("WinXP", 0)) + 
                 float(os_dict.get("NT*", 0)) + 
                 float(os_dict.get("Vista", 0)))
        
        vulnerable_totals[month_num] = total
    
    # Find month with highest value
    max_month = max(vulnerable_totals, key=vulnerable_totals.get)
    return max_month
```

**Key Lessons:**
1. **Initialize collections OUTSIDE loops** - or they reset each iteration
2. **Use `.get(key, default)` to safely access dict values** - avoids KeyError
3. **Convert string numbers to float/int** before arithmetic operations
4. **`max(dict, key=dict.get)`** finds key with maximum value

**Common Errors:**
- String concatenation instead of addition: `'0.07' + '0.07' = '0.070.07'`
- Resetting dict inside loop: `for x in y: dict = {}` only keeps last iteration

---

### Hex to ASCII Conversion
**Concept:** Converting hex values to their ASCII character representation.

**Problem:** Given list of hex strings like `['41', '42', '43']`, convert to ASCII.

**Solution Approaches:**

**Method 1: Explicit Loop**
```python
def hex_to_ascii(the_data):
    int2chr = []
    for num in the_data:
        hex2int = int(num, 16)  # Convert hex string to int
        int2chr.append(chr(hex2int))  # Convert int to character
    return ''.join(int2chr)
```

**Method 2: Map Function (Elegant)**
```python
def hex_to_ascii(the_data):
    # Can't directly do: map(chr, map(int, the_data, 16))
    # Need lambda to pass base parameter
    return ''.join(map(chr, [int(x, 16) for x in the_data]))
```

**Method 3: List Comprehension (Best)**
```python
def hex_to_ascii(the_data):
    return ''.join([chr(int(num, 16)) for num in the_data])
```

**Key Functions:**
- `int(string, base)` - Convert string to int with specified base (16 for hex)
- `chr(int)` - Convert integer to its ASCII character
- `ord(char)` - Opposite: character to integer
- `hex(int)` - Integer to hex string (includes '0x' prefix)

**Important:** `hex()` returns strings like `'0x41'`, need to slice off `'0x'` if needed.

---

### String Chunking Problem
**Concept:** Split strings into fixed-size chunks.

**Problem:** Given list of strings and list of chunk sizes, split each string by its corresponding chunk size.

**Example:**
- Input: `strings = ["abcdefgh"]`, `sizes = [2]`
- Output: `[["ab", "cd", "ef", "gh"]]`

**Solution:**
```python
def chunk_string(string, chunk_size):
    chunks = []
    for i in range(0, len(string), chunk_size):
        chunks.append(string[i:i+chunk_size])
    return chunks

def lab_solution(the_data):
    # Extract strings and chunk sizes from data
    strnglst = [extract strings from the_data]
    splitpnt = [extract chunk sizes from the_data]
    
    result = []
    # Pair up strings with their chunk sizes
    for string, size in zip(strnglst, splitpnt):
        result.append(chunk_string(string, size))
    
    return result
```

**Key Concepts:**
- `range(start, end, step)` for jumping by chunk size
- `zip()` to pair up two lists element-by-element
- String slicing: `string[start:end]`
- List comprehension alternative: `[string[i:i+size] for i in range(0, len(string), size)]`

---

## File I/O Operations

### Basic File Reading
**Concept:** Opening, reading, and measuring file content length.

**Evolution of Solution:**

**Initial Attempt:**
```python
def lab310(the_data):
    file = open(the_data)
    return len(file.read())
```

**Better with Context Manager:**
```python
def lab310(the_data):
    with open(the_data) as file:
        return len(file.read())
```

**Why `with` is Better:**
- Automatically closes file even if errors occur
- Best practice in Python
- Prevents resource leaks

**Key Methods:**
- `open(path)` - Returns file object
- `.read()` - Reads entire file as string
- `.readlines()` - Returns list of lines
- `.readline()` - Reads one line at a time
- File modes: `'r'` (read), `'w'` (write), `'a'` (append), `'rb'` (read binary)

---

## String Manipulation & Regex

### Password Extraction Lab
**Concept:** Parsing formatted strings and extracting specific parts.

**Problem:** Given string like `"Aiden : 123456 , Jayden : porsche , Max : firebird"`, extract all passwords.

**Common Issue:**
```python
# Testing with pre-split list works:
list = ['Aiden : 123456', 'Jayden : porsche', ...]
for l in list:
    password = l.split(" : ")[1]
    print(password)  # Works!

# But actual data is ONE STRING, not a list!
```

**Correct Solution:**
```python
def calculate_answer(data):
    pairs = data.split(",")  # Split by comma FIRST
    passwords = []
    for pair in pairs:
        password = pair.split(" : ")[1].strip()  # .strip() removes whitespace
        passwords.append(password)
    return passwords
```

**List Comprehension:**
```python
def calculate_answer(data):
    return [pair.split(" : ")[1].strip() for pair in data.split(",")]
```

**Key Lessons:**
- Test with actual data format, not ideal format
- `.split()` returns a list
- `.strip()` removes leading/trailing whitespace
- Chain operations: `data.split(",")` then split each result again

---

### Finding Weakest Password
**Concept:** Identifying common weak passwords from a list.

**Problem:** Return the weakest password from the list.

**Common Weak Passwords:**
- `"password"` - literal word
- `"123456"` - most common numeric
- `"12345678"` - longer numeric
- `"1234"` - short numeric
- `"7777777"` - repeated digits

**Solution:**
```python
def calculate_answer(data):
    pairs = data.split(",")
    weak_passwords = ["password", "123456", "12345678", "1234"]
    
    for pair in pairs:
        pword = pair.split(" : ")[1].strip()
        if pword in weak_passwords:
            return pword
    
    return "password"  # Most likely answer
```

---

### Regex IP Address Extraction
**Problem:** Extracting IP addresses using regex, but getting tuples instead of full IPs.

**Incorrect Regex with Capturing Groups:**
```python
clientip = re.findall(r'\b(\d+\.){3}(\d+)', the_data)
# Returns: [('173.', '202'), ('138.', '90'), ...]
```

**Issue:** Parentheses `()` create capturing groups, so `findall()` returns the groups as tuples.

**Solution 1: Non-Capturing Groups**
```python
clientip = re.findall(r'\b(?:\d+\.){3}\d+', the_data)
# (?:...) means group but don't capture
# Returns: ['173.202', '138.90', ...]
```

**Solution 2: Simpler Pattern**
```python
clientip = re.findall(r'\b\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}\b', the_data)
# More explicit, no groups needed
```

**Key Regex Concepts:**
- `(...)` - Capturing group (saved for backreference)
- `(?:...)` - Non-capturing group (grouping only)
- `\b` - Word boundary
- `\d` - Digit (0-9)
- `\d{1,3}` - 1 to 3 digits
- `\.` - Literal dot (escaped)

---

## Network Programming

### Socket Basics
**Concepts covered in pyWars:**

**TCP Client:**
```python
import socket

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect(('example.com', 80))
client.send(b'GET / HTTP/1.1\r\nHost: example.com\r\n\r\n')
response = client.recv(4096)
client.close()
```

**Key Parameters:**
- `socket.AF_INET` - IPv4
- `socket.SOCK_STREAM` - TCP
- `socket.SOCK_DGRAM` - UDP
- `.send()` requires bytes, not strings
- `.recv(bufsize)` - buffer size in bytes

---

## Registry Operations

### Basic Registry Value Retrieval
**Concept:** Opening Windows Registry hive and retrieving specific values.

**Solution:**
```python
from Registry.Registry import Registry

def lab430(the_data):
    reg_hive = Registry(r"/home/student/Public/registry/SOFTWARE")
    key = reg_hive.open('Microsoft\\Windows NT\\CurrentVersion\\Winlogon')
    
    # Direct access by value name
    value = key.value(the_data)
    return value.value()  # Get the actual data
```

**Key Methods:**
- `Registry(path)` - Open registry hive file
- `.open(key_path)` - Open specific key
- `.value(name)` - Get value object by name
- `.value()` on value object - Get actual data
- `.name()` - Get value name

**Important:** Use `\\` for path separators in registry keys (Windows style).

---

### Sorted Registry Keys
**Concept:** List and sort registry subkeys, return key at specific position.

**Solution:**
```python
def lab431(the_data):
    reg_hive = Registry(r"/home/student/Public/registry/SOFTWARE")
    reg_root = reg_hive.root()
    sub_keys = reg_root.subkeys()
    
    # Sort by key name
    sorted_subkeys = sorted(sub_keys, key=lambda k: k.name())
    
    # the_data is an integer position
    answer = sorted_subkeys[the_data]
    return answer.name()  # Return the name, not the object
```

**Key Concepts:**
- `.root()` - Get root key
- `.subkeys()` - List of subkey objects
- `sorted(list, key=lambda x: x.name())` - Sort by name
- Registry key objects have `.name()` method
- Return the name as string, not the object

---

### Filtering Wireless Networks
**Concept:** Iterate through registry subkeys, filter values by starting letter.

**Problem:** Find all wireless networks starting with specific letter (case-insensitive).

**Common Mistake - Wrong Indentation:**
```python
def lab432(the_data):
    networks = []
    reg_key = reg_hive.open('Microsoft\\Windows NT\\CurrentVersion\\NetworkList\\Signatures\\Unmanaged')
    for key in reg_key.subkeys():
        network_name = key.value("FirstNetwork").value()
    if network_name.lower().startswith(the_data.lower()):  # WRONG - outside loop!
        networks.append(network_name)
```

**Correct Solution:**
```python
def lab432(the_data):
    networks = []
    reg_hive = Registry(r"/home/student/Public/registry/SOFTWARE")
    reg_key = reg_hive.open('Microsoft\\Windows NT\\CurrentVersion\\NetworkList\\Signatures\\Unmanaged')
    
    for key in reg_key.subkeys():  # Each subkey is a network
        network_name = key.value("FirstNetwork").value()
        if network_name.lower().startswith(the_data.lower()):  # Inside loop!
            networks.append(network_name)
    
    return sorted(networks)
```

**Key Lessons:**
- Each wireless network is a **subkey**, not a value
- Use `.subkeys()` not `.values()`
- Get value from each subkey: `subkey.value("FirstNetwork")`
- `.startswith()` for prefix matching
- `.lower()` for case-insensitive comparison
- Watch your indentation!

---

### Registry Key Path Issues
**Problem:** Opening registry keys that include "ROOT\\" prefix.

**Error:**
```
Registry.Registry.RegistryKeyNotFoundException: Registry key not found: ROOT\ROOT
```

**Cause:** Data contains `"ROOT\\SOFTWARE\\REGLAB\\Run\\Service"`, but Registry library already assumes ROOT as starting point.

**Solution:**
```python
def lab433(the_data):
    reg_hive = Registry(r"/home/student/Public/registry/NTUSER.DAT")
    
    # Remove "ROOT\\" prefix if present
    key_path = the_data.replace("ROOT\\", "")
    
    reg_keys = reg_hive.open(key_path)
    
    total = 0
    for val in reg_keys.values():
        value_data = val.value()
        total += int(value_data)  # May need conversion
    
    return total
```

**String Prefix Removal Options:**
- `.replace("ROOT\\", "")` - Replace all occurrences
- `.removeprefix("ROOT\\")` - Python 3.9+ (removes only from start)
- `[5:]` - String slicing if you know exact length

---

## Cryptography & XOR

### Basic XOR Encryption Lab
**Concept:** XOR encryption with repeating key.

**Problem:** Decrypt data using XOR with a single-byte key.

**Common Error:**
```python
TypeError: unsupported operand type(s) for ^: 'str' and 'int'
```

**Cause:** Trying to XOR strings instead of bytes.

**Working Solution:**
```python
import pywars
from itertools import cycle

def xor_crypt(data_bytes, key_bytes):
    return bytes(a ^ b for a, b in zip(data_bytes, cycle(key_bytes)))

def calculate_answer(data):
    # Convert string to bytes if needed
    if isinstance(data, str):
        data_bytes = data.encode()
    else:
        data_bytes = data
    
    # Key as bytes (not b'0x61' which is literal string!)
    key = b'a'  # This is byte 0x61
    # OR: key = bytes.fromhex('61')
    
    decrypt = xor_crypt(data_bytes, key)
    return decrypt.decode()  # Convert back to string
```

**Key Points:**
- **XOR only works on bytes**, not strings
- Use `.encode()` to convert string → bytes
- Use `.decode()` to convert bytes → string
- `b'0x61'` is the literal bytes for characters "0x61", NOT the hex value
- `b'a'` is the byte with value 0x61 (ASCII 'a')
- `bytes.fromhex('61')` converts hex string to byte
- `cycle(key_bytes)` repeats key infinitely for longer data

---

### XOR Property Lab - Double Encryption
**Concept:** Understanding XOR properties: `(Data1 XOR Data2) XOR Data1 = Data2`

**Problem:** Given:
- Encrypted = `Data1 XOR Data2` (two messages XORed together)
- Known = `Data1` (one of the original messages)
- Find: `Data2` (the other message)

**Mathematical Property:**
```
A XOR B XOR B = A
Why? Because XOR is self-inverse:
- B XOR B = 0
- A XOR 0 = A
```

**Example:**
```python
>>> d.data(64)
['\x00\x00\x00...', 'this parrot is dead']
# First: encrypted data (Data1 XOR Data2)
# Second: known plaintext (Data1 or Data2)
```

**Solution:**
```python
def calculate_answer(data):
    encrypted, known = data  # Unpack the two strings
    
    # XOR each character
    result = []
    for e, k in zip(encrypted, known):
        decrypted_byte = ord(e) ^ ord(k)
        result.append(chr(decrypted_byte))
    
    return ''.join(result)
```

**One-Liner:**
```python
def calculate_answer(data):
    encrypted, known = data
    return ''.join(chr(ord(e) ^ ord(k)) for e, k in zip(encrypted, known))
```

**Key Concepts:**
- XOR is **commutative**: `A XOR B = B XOR A`
- XOR is **associative**: `(A XOR B) XOR C = A XOR (B XOR C)`
- XOR is **self-inverse**: `A XOR B XOR B = A`
- `ord(char)` - character to integer
- `chr(int)` - integer to character
- `zip(a, b)` - pairs up elements from two sequences

---

## Image Processing

### Lab: Display and Resize Images
**Concept:** Using PIL/Pillow to manipulate and display images.

**Problem:** Load an image, resize to 200x200, and display.

**Solution:**
```python
from PIL import Image

def display_resized_image(image_path):
    # Load image
    imageobject = Image.open(image_path)
    
    # Resize (returns NEW image, doesn't modify original)
    resized_image = imageobject.resize((200, 200))  # Tuple, not list!
    
    # Display
    resized_image.show()
```

**Key Points:**
- `.resize(size)` takes a **tuple** `(width, height)`, not a list
- `.resize()` returns a NEW image - doesn't modify original
- `.show()` opens image in default viewer
- PIL works with various formats: JPEG, PNG, GIF, BMP, etc.

**Common Mistakes:**
- Using list instead of tuple: `[200, 200]` vs `(200, 200)`
- Forgetting to assign result: `imageobject.resize(...)` without `resized_image =`
- Using `size=` parameter name (not needed)

**Additional PIL Operations:**
```python
# Save to file
resized_image.save('output.jpg')

# Get dimensions
width, height = imageobject.size

# Convert format
imageobject.convert('L')  # Grayscale

# Rotate
rotated = imageobject.rotate(45)

# Crop
cropped = imageobject.crop((x1, y1, x2, y2))
```

---

## Common Patterns & Gotchas

### 1. Return None Issue
**Problem:** Functions that modify in-place return None.

**Affected Methods:**
- `list.append()`
- `list.sort()`
- `list.reverse()`
- `list.extend()`
- `dict.update()`

**Wrong:**
```python
return data.sort()  # Returns None!
print(data.append(x))  # Prints None!
```

**Right:**
```python
data.sort()
return data

data.append(x)
print(data)
```

---

### 2. Loop Variable Scope
**Problem:** Accidentally using loop variable after loop ends.

**Wrong:**
```python
for item in items:
    value = item.get_value()
if value > 10:  # 'value' only has last item's value!
    do_something()
```

**Right:**
```python
for item in items:
    value = item.get_value()
    if value > 10:  # Check inside loop
        do_something()
```

---

### 3. String vs Bytes
**Problem:** Mixing strings and bytes in operations that require one or the other.

**Common Errors:**
- XOR requires bytes
- Socket send/recv uses bytes
- File open in binary mode ('rb') gives bytes
- File open in text mode ('r') gives strings

**Conversions:**
```python
# String to bytes
text = "hello"
bytes_data = text.encode('utf-8')

# Bytes to string
bytes_data = b'hello'
text = bytes_data.decode('utf-8')
```

---

### 4. Integer Division vs Float Division
**Difference:**
```python
5 / 2   # = 2.5 (float division)
5 // 2  # = 2 (integer division, floor)
```

---

### 5. Mutable Default Arguments
**Dangerous Pattern:**
```python
def add_item(item, list=[]):  # BAD!
    list.append(item)
    return list

add_item(1)  # [1]
add_item(2)  # [1, 2] - NOT [2]!
```

**Why:** Default list is created once and shared across all calls.

**Solution:**
```python
def add_item(item, list=None):
    if list is None:
        list = []
    list.append(item)
    return list
```

---

### 6. Range and Index Confusion
**Remember:**
- Python uses **0-based indexing**
- `range(5)` gives `[0, 1, 2, 3, 4]` (5 elements, but max is 4)
- Negative indices count from end: `list[-1]` is last element

---

### 7. Dict KeyError vs .get()
**Unsafe:**
```python
value = dict[key]  # Raises KeyError if key doesn't exist
```

**Safe:**
```python
value = dict.get(key, default_value)  # Returns default if key missing
```

---

### 8. Regex Capturing Groups
**Problem:** `findall()` with groups returns tuples, not matches.

**With Groups:**
```python
re.findall(r'(\d+)\.(\d+)', '1.2.3.4')
# [('1', '2'), ('3', '4')]
```

**Without Groups (non-capturing):**
```python
re.findall(r'\d+\.\d+', '1.2.3.4')
# ['1.2', '3.4']
```

**Use `(?:...)` for non-capturing groups when you need grouping but not capture.**

---

## Quick Reference Cheat Sheet

### Essential Functions
```python
# Type Conversions
int(x), float(x), str(x), bool(x)
chr(65)  # → 'A'
ord('A')  # → 65
list(x), tuple(x), set(x), dict(x)

# String Methods
s.upper(), s.lower()
s.strip(), s.lstrip(), s.rstrip()
s.split(delim)
s.replace(old, new)
s.startswith(prefix), s.endswith(suffix)
s.find(sub), s.index(sub)
s.count(sub)
''.join(list)

# List Methods
list.append(x)
list.extend(iterable)
list.insert(i, x)
list.remove(x)
list.pop([i])
list.sort(), sorted(list)
list.reverse(), reversed(list)

# Dict Methods
dict.get(key, default)
dict.keys(), dict.values(), dict.items()
dict.update(other_dict)

# File Operations
with open(path, mode) as f:
    content = f.read()
    lines = f.readlines()

# Built-in Functions
len(x), max(x), min(x), sum(x)
range(start, stop, step)
enumerate(iterable)
zip(iter1, iter2)
map(function, iterable)
filter(function, iterable)
any(iterable), all(iterable)

# Regex
import re
re.search(pattern, text)
re.findall(pattern, text)
re.sub(pattern, replacement, text)
re.split(pattern, text)
```

### Common Patterns
```python
# List Comprehension
[x*2 for x in range(10) if x % 2 == 0]

# Dict Comprehension
{k: v*2 for k, v in dict.items() if v > 0}

# Lambda Functions
lambda x: x*2
sorted(list, key=lambda x: x.name)

# Unpacking
a, b = (1, 2)
first, *rest = [1, 2, 3, 4]

# Ternary Operator
value = x if condition else y

# Multiple Assignment
x = y = z = 0
```

---
## Additional Resources

### Must-Know Libraries
- **Standard Library:** `os`, `sys`, `pathlib`, `re`, `json`, `csv`, `hashlib`, `socket`, `collections`
- **External:** `requests`, `beautifulsoup4`, `cryptography`, `PIL/Pillow`
