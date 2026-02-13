# Python-Reference-Guides

## Table of Contents

### 01. Python Fundamentals

**Core Python concepts and syntax**

- Variables & Data Types
- Operators (Arithmetic, Comparison, Logical)
- Control Structures (if/elif/else, for, while)
- Functions (basic, default args, *args, **kwargs)
- Built-in Functions (len, max, min, sorted, map, filter, zip)
- List Comprehensions
- Lambda Functions
- Variable Scope

### 02. Data Structures

**Lists, Dictionaries, Sets, and Tuples**

- **Lists**: Creation, indexing, slicing, methods, sorting, comprehensions
- **Dictionaries**: Key-value operations, methods, comprehensions, patterns
- **Sets**: Mathematical operations, uniqueness, set operations
- **Tuples**: Immutability, unpacking, named tuples
- **Advanced Collections**: Counter, defaultdict, deque
- Performance comparison and choosing the right structure

### 03. File I/O

**Working with files and directories**

- Basic file operations (open, read, write)
- File modes (r, w, a, rb, wb, rt)
- Binary files and struct module
- os module (path operations, directory operations)
- **pathlib** (modern path handling - RECOMMENDED)
- Compressed files (gzip, zip, tar)
- CSV and JSON files
- Error handling for file operations

### 04. String Manipulation

**String processing and text operations**

- String methods (split, join, strip, replace, find, etc.)
- String formatting (%, .format(), **f-strings**)
- **Regular Expressions** (patterns, groups, flags, functions)
- Encoding/Decoding (UTF-8, Base64, hex)
- Text processing patterns
- Performance tips

### 05. Network Programming

**Network communications and web interaction**

- **Socket Programming** (TCP/UDP clients and servers)
- **HTTP Requests** (requests library - GET, POST, headers, auth)
- **Web Scraping** (BeautifulSoup - parsing HTML, extracting data)
- URL handling (urllib.parse)
- **Network Traffic Analysis** (Scapy - reading pcaps, creating packets)
- DNS operations
- Common patterns (port scanning, downloading files)

### 06. Security & Pentesting

**Security concepts and pentesting techniques**

- **Cryptography** (hashing with hashlib, encryption with cryptography)
- **Password Security** (bcrypt, secure password generation)
- Network security (SSL/TLS, port scanning, banner grabbing)
- **Web Application Security** (SQL injection, XSS, directory traversal)
- Data exfiltration (extracting emails, IPs, URLs, passwords)
- **Log Analysis** (parsing logs, detecting attacks)
- Steganography basics
- Reverse engineering (decoding, extracting strings)
- Creating executables (PyInstaller)

### 07. Advanced Topics

**Object-oriented programming and advanced concepts**

- **Object-Oriented Programming** (classes, inheritance, properties)
- **Exception Handling** (try/except, custom exceptions, context managers)
- **Decorators** (function decorators, common patterns)
- **Generators** (yield, memory efficiency, iterators)
- Advanced comprehensions
- **Concurrency** (threading, multiprocessing, process pools)
- JSON and REST APIs
- Command-line arguments (argparse)
- Performance optimization

---
### Key Topics Covered

1. Python fundamentals and control structures
2. Functions, classes, and OOP
3. Data structures (lists, dicts, sets, tuples)
4. File handling and I/O operations
5. Network programming (sockets, HTTP)
6. Web scraping and interaction
7. Regular expressions
8. Data analysis and log parsing
9. Security concepts and pentesting basics
10. Creating tools and executables

## Quick Reference Patterns

### Most Common Operations

```python
# File I/O
with open('file.txt', 'r') as f:
    content = f.read()

# HTTP Request
import requests
response = requests.get('https://api.example.com/data')
data = response.json()

# Web Scraping
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'html.parser')
links = soup.find_all('a')

# Regex
import re
matches = re.findall(r'\d+', text)
clean = re.sub(r'[^\w\s]', '', text)

# Hashing
import hashlib
hash = hashlib.sha256(data.encode()).hexdigest()

# JSON
import json
data = json.loads(json_string)
json.dump(data, file, indent=2)

# List files
from pathlib import Path
files = list(Path('.').rglob('*.txt'))
```

---

## Essential Libraries to Know

### Standard Library (Built-in)

- `os`, `sys` - Operating system interface
- `pathlib` - Object-oriented file paths
- `re` - Regular expressions
- `json` - JSON encoding/decoding
- `csv` - CSV file handling
- `hashlib` - Cryptographic hashing
- `socket` - Network programming
- `argparse` - Command-line arguments
- `collections` - Specialized containers
- `itertools` - Iterator functions

### External Libraries (pip install)

- `requests` - HTTP library
- `beautifulsoup4` - HTML/XML parsing
- `cryptography` - Encryption and cryptographic recipes
- `scapy` - Packet manipulation and analysis
- `bcrypt` - Password hashing
- `pyinstaller` - Create executables

---

### Practice Platforms

- [HackerRank Python](https://www.hackerrank.com/domains/python)
- [LeetCode](https://leetcode.com/)
- [Project Euler](https://projecteuler.net/)

### Books

- "Automating the Boring Stuff with Python" - Al Sweigart
- "Python Crash Course" - Eric Matthes
- "Fluent Python" - Luciano Ramalho
- "Black Hat Python" - Justin Seitz

---
