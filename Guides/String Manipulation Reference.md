## String Basics

### Creating Strings

```python
# Single quotes
s = 'Hello'

# Double quotes
s = "Hello"

# Triple quotes (multiline)
s = """This is
a multiline
string"""

s = '''Also
multiline'''

# Raw strings (ignore escape characters)
path = r'C:\new\folder'  # backslashes not escaped

# F-strings (formatted string literals)
name = "Alice"
age = 25
s = f"My name is {name} and I'm {age} years old"
```

### String Properties

```python
# Strings are immutable
s = "hello"
# s[0] = 'H'  # ERROR: can't modify

# Strings are sequences
len("hello")  # 5
"hello"[0]    # 'h'
"hello"[-1]   # 'o'
"hello"[1:4]  # 'ell'
```

---

## String Methods

### Case Conversion

```python
s = "Hello World"

s.upper()       # "HELLO WORLD"
s.lower()       # "hello world"
s.capitalize()  # "Hello world"
s.title()       # "Hello World"
s.swapcase()    # "hELLO wORLD"

# Case checking
s.isupper()     # False
s.islower()     # False
s.istitle()     # True
```

### Searching and Checking

```python
s = "Hello World"

# Find substring
s.find('World')     # 6 (index of first occurrence)
s.find('xyz')       # -1 (not found)
s.index('World')    # 6 (raises ValueError if not found)

# Check if starts/ends with
s.startswith('Hello')  # True
s.endswith('World')    # True

# Count occurrences
s.count('l')    # 3
s.count('ll')   # 1

# Membership (use 'in' operator)
'World' in s    # True
'xyz' in s      # False
```

### Splitting and Joining

```python
# Split on whitespace (default)
"hello world python".split()  # ['hello', 'world', 'python']

# Split on specific delimiter
"apple,banana,cherry".split(',')  # ['apple', 'banana', 'cherry']

# Split on newlines
text = "line1\nline2\nline3"
text.split('\n')     # ['line1', 'line2', 'line3']
text.splitlines()    # ['line1', 'line2', 'line3']

# Limit splits
"a-b-c-d".split('-', 2)  # ['a', 'b', 'c-d']

# Split from right
"a-b-c-d".rsplit('-', 1)  # ['a-b-c', 'd']

# Join strings
'-'.join(['a', 'b', 'c'])  # 'a-b-c'
' '.join(['Hello', 'World'])  # 'Hello World'
''.join(['H', 'i'])  # 'Hi'
```

### Trimming and Padding

```python
s = "  hello  "

# Remove whitespace
s.strip()   # "hello"
s.lstrip()  # "hello  " (left strip)
s.rstrip()  # "  hello" (right strip)

# Remove specific characters
"###hello###".strip('#')  # "hello"

# Padding
"hello".ljust(10)      # "hello     " (left justify)
"hello".rjust(10)      # "     hello" (right justify)
"hello".center(10)     # "  hello   " (center)
"hello".ljust(10, '-') # "hello-----"

# Zero padding (for numbers)
"42".zfill(5)  # "00042"
```

### Replacing

```python
s = "hello world"

# Replace all occurrences
s.replace('l', 'L')     # "heLLo worLd"

# Replace limited occurrences
s.replace('l', 'L', 2)  # "heLLo world"

# Remove substring (replace with empty)
s.replace('world', '')  # "hello "
```

### Type Checking

```python
# Alphanumeric
"hello123".isalnum()  # True
"hello-123".isalnum() # False

# Alphabetic
"hello".isalpha()     # True
"hello123".isalpha()  # False

# Digits
"123".isdigit()       # True
"123.45".isdigit()    # False

# Numeric (includes floats, fractions)
"123".isnumeric()     # True
"123.45".isnumeric()  # False

# Whitespace
"   ".isspace()       # True
" a ".isspace()       # False

# Printable
"hello".isprintable() # True
"hello\n".isprintable() # False
```

---

## String Formatting

### Old Style (% operator)

```python
# Basic
"Hello %s" % "World"  # "Hello World"

# Multiple values
"Name: %s, Age: %d" % ("Alice", 25)  # "Name: Alice, Age: 25"

# Format specifiers
"%.2f" % 3.14159  # "3.14"
"%5d" % 42        # "   42" (width 5)
```

### str.format() Method

```python
# Positional arguments
"Hello {}".format("World")  # "Hello World"
"{} {}".format("Hello", "World")  # "Hello World"
"{1} {0}".format("World", "Hello")  # "Hello World"

# Named arguments
"{name} is {age} years old".format(name="Alice", age=25)

# Format specifiers
"{:.2f}".format(3.14159)  # "3.14"
"{:5d}".format(42)        # "   42"
"{:>10}".format("test")   # "      test" (right align)
"{:<10}".format("test")   # "test      " (left align)
"{:^10}".format("test")   # "   test   " (center)
```

### F-Strings (Python 3.6+) - RECOMMENDED

```python
name = "Alice"
age = 25
pi = 3.14159

# Basic
f"Hello {name}"  # "Hello Alice"

# Expressions
f"{2 + 2}"  # "4"
f"{name.upper()}"  # "ALICE"

# Format specifiers
f"{pi:.2f}"       # "3.14"
f"{age:5d}"       # "   25"
f"{name:>10}"     # "     Alice"

# Multiple values
f"{name} is {age} years old"  # "Alice is 25 years old"

# Debugging (Python 3.8+)
f"{name=}"  # "name='Alice'"
```

### Common Format Specifiers

```python
# Numbers
f"{42:d}"      # "42" (decimal)
f"{42:b}"      # "101010" (binary)
f"{42:o}"      # "52" (octal)
f"{42:x}"      # "2a" (hex lowercase)
f"{42:X}"      # "2A" (hex uppercase)

# Floats
f"{3.14159:.2f}"   # "3.14" (2 decimal places)
f"{1234567:.2e}"   # "1.23e+06" (scientific)
f"{0.5:.2%}"       # "50.00%" (percentage)

# Width and alignment
f"{42:5d}"    # "   42" (width 5, right aligned)
f"{42:05d}"   # "00042" (zero padded)
f"{'hi':<5}"  # "hi   " (left aligned)
f"{'hi':>5}"  # "   hi" (right aligned)
f"{'hi':^5}"  # " hi  " (centered)

# Signs
f"{42:+d}"    # "+42" (always show sign)
f"{42: d}"    # " 42" (space for positive)
f"{-42:d}"    # "-42"

# Thousands separator
f"{1234567:,}"  # "1,234,567"
f"{1234567:_}"  # "1_234_567"
```

---

## Regular Expressions (regex)

### Import and Basic Usage

```python
import re

# Search for pattern
match = re.search(r'pattern', 'text to search')
if match:
    print("Found!")

# Find all matches
matches = re.findall(r'pattern', 'text to search')

# Replace pattern
new_text = re.sub(r'pattern', 'replacement', 'text')

# Split on pattern
parts = re.split(r'pattern', 'text')
```

### Common Patterns

```python
# Literal characters
re.search(r'hello', 'hello world')  # matches "hello"

# Special characters (need escaping)
re.search(r'\$', 'price is $5')     # matches "$"
re.search(r'\.', 'file.txt')        # matches "."

# Character classes
r'[abc]'      # matches 'a', 'b', or 'c'
r'[a-z]'      # any lowercase letter
r'[A-Z]'      # any uppercase letter
r'[0-9]'      # any digit
r'[^abc]'     # NOT 'a', 'b', or 'c'

# Predefined character classes
r'\d'         # digit [0-9]
r'\D'         # non-digit
r'\w'         # word character [a-zA-Z0-9_]
r'\W'         # non-word character
r'\s'         # whitespace [ \t\n\r\f\v]
r'\S'         # non-whitespace
r'.'          # any character except newline

# Anchors
r'^hello'     # starts with "hello"
r'world$'     # ends with "world"
r'\bword\b'   # word boundary

# Quantifiers
r'a*'         # 0 or more 'a'
r'a+'         # 1 or more 'a'
r'a?'         # 0 or 1 'a'
r'a{3}'       # exactly 3 'a's
r'a{2,5}'     # 2 to 5 'a's
r'a{2,}'      # 2 or more 'a's

# Groups
r'(abc)+'     # group, 1 or more
r'(ab|cd)'    # alternatives: "ab" or "cd"
```

### Regex Functions

```python
import re

text = "Contact: alice@example.com and bob@test.org"

# search - find first match
match = re.search(r'\w+@\w+\.\w+', text)
if match:
    print(match.group())  # "alice@example.com"

# findall - find all matches
emails = re.findall(r'\w+@\w+\.\w+', text)  # ['alice@example.com', 'bob@test.org']

# finditer - iterator of match objects
for match in re.finditer(r'\w+@\w+\.\w+', text):
    print(match.group(), match.start(), match.end())

# match - match at beginning only
re.match(r'Contact', text)  # matches
re.match(r'alice', text)    # no match (not at start)

# sub - substitute
re.sub(r'\d+', 'X', 'Room 123, Floor 4')  # "Room X, Floor X"

# split - split on pattern
re.split(r'[,;]\s*', 'a,b; c,d')  # ['a', 'b', 'c', 'd']

# Compile for reuse (more efficient)
pattern = re.compile(r'\w+@\w+\.\w+')
pattern.findall(text)
pattern.search(text)
```

### Capturing Groups

```python
import re

# Basic capturing
match = re.search(r'(\w+)@(\w+)\.(\w+)', 'alice@example.com')
if match:
    print(match.group(0))  # "alice@example.com" (entire match)
    print(match.group(1))  # "alice" (first group)
    print(match.group(2))  # "example" (second group)
    print(match.group(3))  # "com" (third group)
    print(match.groups())  # ('alice', 'example', 'com')

# Named groups
match = re.search(r'(?P<user>\w+)@(?P<domain>\w+\.\w+)', 'alice@example.com')
if match:
    print(match.group('user'))    # "alice"
    print(match.group('domain'))  # "example.com"
    print(match.groupdict())      # {'user': 'alice', 'domain': 'example.com'}

# Non-capturing group
re.search(r'(?:abc)+', 'abcabc')  # groups but doesn't capture
```

### Regex Flags

```python
import re

# Case insensitive
re.search(r'hello', 'HELLO', re.IGNORECASE)  # matches
re.search(r'hello', 'HELLO', re.I)           # shorthand

# Multiline (^ and $ match line boundaries)
re.findall(r'^word', 'word1\nword2', re.MULTILINE)

# Dotall (. matches newline too)
re.search(r'a.b', 'a\nb', re.DOTALL)

# Verbose (allow comments and whitespace)
pattern = re.compile(r'''
    \d+      # one or more digits
    \s*      # optional whitespace
    [a-z]+   # one or more letters
''', re.VERBOSE)

# Combine flags
re.search(r'hello', 'HELLO\nWORLD', re.IGNORECASE | re.MULTILINE)
```

### Common Regex Patterns

```python
# Email (simple)
r'\w+@\w+\.\w+'
r'[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}'

# Phone number (US)
r'\d{3}-\d{3}-\d{4}'
r'\(\d{3}\)\s*\d{3}-\d{4}'

# IP address (simple)
r'\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}'

# URL (simple)
r'https?://[\w.-]+\.\w+'

# Hex color
r'#[0-9A-Fa-f]{6}'

# Date (YYYY-MM-DD)
r'\d{4}-\d{2}-\d{2}'

# Time (HH:MM)
r'\d{2}:\d{2}'

# Extract numbers
r'\d+'           # integers
r'\d+\.\d+'      # floats

# Extract words
r'\b\w+\b'
```

---

## String Encoding

### Encoding and Decoding

```python
# String to bytes (encoding)
text = "Hello, 世界"
bytes_utf8 = text.encode('utf-8')      # b'Hello, \xe4\xb8\x96\xe7\x95\x8c'
bytes_ascii = text.encode('ascii', errors='ignore')  # b'Hello, '

# Bytes to string (decoding)
text = bytes_utf8.decode('utf-8')      # "Hello, 世界"

# Common encodings
text.encode('utf-8')
text.encode('ascii')
text.encode('latin-1')
text.encode('cp1252')  # Windows

# Error handling
text.encode('ascii', errors='ignore')    # skip unencodable chars
text.encode('ascii', errors='replace')   # replace with '?'
text.encode('ascii', errors='xmlcharrefreplace')  # use XML entities
```

### Hex and Base64

```python
# Hex encoding
data = b'\x00\x01\x02\x03'
hex_str = data.hex()  # '00010203'
data = bytes.fromhex(hex_str)  # b'\x00\x01\x02\x03'

# Base64
import base64
text = "Hello World"
encoded = base64.b64encode(text.encode())  # b'SGVsbG8gV29ybGQ='
decoded = base64.b64decode(encoded).decode()  # "Hello World"
```

---

## Text Processing Patterns

### Remove Whitespace

```python
# Remove all whitespace
text = "  hello   world  "
''.join(text.split())  # "helloworld"

# Normalize whitespace
' '.join(text.split())  # "hello world"
```

### Extract Numbers

```python
import re

text = "Price: $123.45, Quantity: 10"

# All numbers
numbers = re.findall(r'\d+', text)  # ['123', '45', '10']

# Floats
floats = re.findall(r'\d+\.\d+', text)  # ['123.45']
```

### Clean Text

```python
import re

text = "Hello!!!  World???"

# Remove punctuation
clean = re.sub(r'[^\w\s]', '', text)  # "Hello  World"

# Remove extra spaces
clean = ' '.join(clean.split())  # "Hello World"

# Lowercase
clean = clean.lower()  # "hello world"
```

### Parse CSV Line (manual)

```python
line = "John,Doe,25,NYC"
fields = line.split(',')  # ['John', 'Doe', '25', 'NYC']

# With quoted fields
line = '"Smith, John","Engineer","San Francisco, CA"'
# Use csv module instead for proper parsing
```

### Word Frequency

```python
from collections import Counter

text = "hello world hello python world"
words = text.lower().split()
freq = Counter(words)
freq.most_common(2)  # [('hello', 2), ('world', 2)]
```

### Reverse String

```python
s = "hello"
s[::-1]  # "olleh"

# Reverse words
" ".join("hello world".split()[::-1])  # "world hello"
```

---

## Common String Operations

### Check for Substring

```python
# Simple check
if 'world' in 'hello world':
    print("Found")

# Case insensitive
if 'WORLD' in 'hello world'.upper():
    print("Found")

# Multiple substrings (any)
if any(word in text for word in ['hello', 'hi', 'hey']):
    print("Found one")

# Multiple substrings (all)
if all(word in text for word in ['hello', 'world']):
    print("Found all")
```

### Remove Prefix/Suffix (Python 3.9+)

```python
# Remove prefix
"HelloWorld".removeprefix("Hello")  # "World"

# Remove suffix
"test.txt".removesuffix(".txt")  # "test"

# Pre-3.9 alternative
s = "HelloWorld"
if s.startswith("Hello"):
    s = s[len("Hello"):]  # "World"
```

### String Comparison

```python
# Case sensitive
"hello" == "Hello"  # False

# Case insensitive
"hello".lower() == "Hello".lower()  # True

# Alphabetical order
"apple" < "banana"  # True
```

---

## Performance Tips

1. **Use f-strings** - fastest and most readable
2. **Use join for concatenation** - `''.join(list)` instead of `+=` in loop
3. **Compile regex** if using multiple times
4. **Use str methods** over regex when possible (faster)
5. **Use in operator** for simple substring search

---

## Quick Reference Card

```python
# Common methods
s.upper() / s.lower()
s.strip() / s.lstrip() / s.rstrip()
s.split(delimiter)
s.replace(old, new)
s.startswith(prefix) / s.endswith(suffix)
s.find(substring) / s.index(substring)
s.count(substring)

# Formatting
f"{var}"
f"{var:.2f}"
f"{var:>10}"

# Regex
import re
re.search(r'pattern', text)
re.findall(r'pattern', text)
re.sub(r'pattern', 'replacement', text)

# Common patterns
r'\d+'       # digits
r'\w+'       # words
r'\s+'       # whitespace
r'[a-z]+'    # lowercase letters
r'^start'    # line start
r'end$'      # line end

# Encoding
text.encode('utf-8')
bytes.decode('utf-8')
```
