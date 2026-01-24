## Basic File Operations

### Opening Files

```python
# Basic syntax
file = open("filename.txt", "mode")

# Always close when done
file.close()

# Better: Use context manager (auto-closes)
with open("filename.txt", "mode") as file:
    # work with file
    pass
# File automatically closed here
```

### File Modes

|Mode|Description|Creates if Missing|Overwrites|
|---|---|---|---|
|`'r'`|Read (default)|No|N/A|
|`'w'`|Write|Yes|Yes|
|`'a'`|Append|Yes|No|
|`'x'`|Exclusive create|Yes|Error if exists|
|`'r+'`|Read and write|No|No|
|`'w+'`|Write and read|Yes|Yes|
|`'rb'`|Read binary|No|N/A|
|`'wb'`|Write binary|Yes|Yes|
|`'rt'`|Read text (default)|No|N/A|

### Reading Files

```python
# Read entire file as string
with open("file.txt", "r") as f:
    content = f.read()

# Read line by line (memory efficient)
with open("file.txt", "r") as f:
    for line in f:
        print(line.strip())  # strip() removes newline

# Read all lines into list
with open("file.txt", "r") as f:
    lines = f.readlines()  # ['line1\n', 'line2\n', ...]

# Read specific number of characters
with open("file.txt", "r") as f:
    chunk = f.read(100)  # read first 100 characters

# Read one line at a time
with open("file.txt", "r") as f:
    line1 = f.readline()
    line2 = f.readline()
```

### Writing Files

```python
# Write string to file (overwrites)
with open("output.txt", "w") as f:
    f.write("Hello, World!\n")
    f.write("Second line\n")

# Write list of strings
lines = ["Line 1\n", "Line 2\n", "Line 3\n"]
with open("output.txt", "w") as f:
    f.writelines(lines)

# Append to file
with open("output.txt", "a") as f:
    f.write("Appended line\n")

# Write multiple items with print
with open("output.txt", "w") as f:
    print("Hello", "World", file=f)
```

### Working with File Positions

```python
with open("file.txt", "r") as f:
    # Get current position
    pos = f.tell()
    
    # Move to specific position
    f.seek(0)  # beginning of file
    f.seek(0, 2)  # end of file
```

---

## Binary Files

### Reading Binary Files

```python
# Read binary file
with open("image.png", "rb") as f:
    data = f.read()  # bytes object

# Read specific number of bytes
with open("data.bin", "rb") as f:
    chunk = f.read(1024)  # read 1KB
```

### Writing Binary Files

```python
# Write bytes
data = b'\x00\x01\x02\x03'
with open("output.bin", "wb") as f:
    f.write(data)

# Convert string to bytes
text = "Hello"
with open("output.bin", "wb") as f:
    f.write(text.encode('utf-8'))
```

### Struct Module (Binary Data)

```python
import struct

# Pack data into binary
# Format: i=int, f=float, s=string
data = struct.pack('i f 5s', 42, 3.14, b'hello')

# Unpack binary data
value1, value2, value3 = struct.unpack('i f 5s', data)

# Common format characters:
# b = signed char (1 byte)
# B = unsigned char (1 byte)
# h = short (2 bytes)
# H = unsigned short (2 bytes)
# i = int (4 bytes)
# I = unsigned int (4 bytes)
# f = float (4 bytes)
# d = double (8 bytes)
# s = char[] (string)
```

---

## File System Operations (os module)

### Path Operations

```python
import os

# Current working directory
os.getcwd()  # '/home/user/project'

# Change directory
os.chdir('/path/to/directory')

# List directory contents
os.listdir('/path/to/directory')  # ['file1.txt', 'file2.txt', 'subdir']

# Check if path exists
os.path.exists('/path/to/file')  # True/False

# Check if it's a file
os.path.isfile('/path/to/file')  # True/False

# Check if it's a directory
os.path.isdir('/path/to/directory')  # True/False

# Get file size
os.path.getsize('/path/to/file')  # bytes

# Get absolute path
os.path.abspath('relative/path')  # '/home/user/relative/path'

# Join paths (OS-independent)
os.path.join('directory', 'subdirectory', 'file.txt')
# Unix: 'directory/subdirectory/file.txt'
# Windows: 'directory\\subdirectory\\file.txt'

# Split path into directory and filename
os.path.split('/path/to/file.txt')  # ('/path/to', 'file.txt')

# Get directory name
os.path.dirname('/path/to/file.txt')  # '/path/to'

# Get filename
os.path.basename('/path/to/file.txt')  # 'file.txt'

# Split filename and extension
os.path.splitext('file.txt')  # ('file', '.txt')
```

### Directory Operations

```python
import os

# Create directory
os.mkdir('new_directory')

# Create nested directories
os.makedirs('parent/child/grandchild', exist_ok=True)

# Remove directory (must be empty)
os.rmdir('directory')

# Remove directory and contents
import shutil
shutil.rmtree('directory')

# Walk directory tree
for dirpath, dirnames, filenames in os.walk('/path/to/directory'):
    print(f"Directory: {dirpath}")
    print(f"Subdirectories: {dirnames}")
    print(f"Files: {filenames}")
```

### File Operations

```python
import os
import shutil

# Copy file
shutil.copy('source.txt', 'destination.txt')
shutil.copy2('source.txt', 'dest.txt')  # preserves metadata

# Move/rename file
shutil.move('old_name.txt', 'new_name.txt')
os.rename('old.txt', 'new.txt')  # same as move

# Delete file
os.remove('file.txt')
os.unlink('file.txt')  # same as remove
```

---

## Pathlib (Modern Path Handling)

### Creating Path Objects

```python
from pathlib import Path

# Create Path object
p = Path('/path/to/directory')
p = Path('relative/path')
p = Path.home()  # user's home directory
p = Path.cwd()   # current working directory

# Join paths with / operator
full_path = Path('/home') / 'user' / 'documents' / 'file.txt'
```

### Path Properties

```python
from pathlib import Path

p = Path('/home/user/documents/report.pdf')

# Components
p.name        # 'report.pdf'
p.stem        # 'report'
p.suffix      # '.pdf'
p.suffixes    # ['.pdf'] (or ['.tar', '.gz'] for file.tar.gz)
p.parent      # Path('/home/user/documents')
p.parents[0]  # Path('/home/user/documents')
p.parents[1]  # Path('/home/user')
p.anchor      # '/' (or 'C:\' on Windows)

# Check properties
p.exists()     # True/False
p.is_file()    # True/False
p.is_dir()     # True/False
p.is_absolute() # True/False
```

### Path Operations

```python
from pathlib import Path

p = Path('/home/user/documents')

# List directory contents
list(p.iterdir())  # [Path('file1.txt'), Path('file2.txt')]

# Recursive glob pattern matching
list(p.glob('*.txt'))        # all .txt files
list(p.glob('**/*.txt'))     # all .txt files recursively
list(p.rglob('*.txt'))       # same as glob('**/*.txt')

# Create directory
p.mkdir(exist_ok=True)       # create if doesn't exist
p.mkdir(parents=True)        # create parent directories too

# Read/write files
content = p.read_text()      # read entire file as string
p.write_text('Hello World')  # write string to file

bytes_content = p.read_bytes()  # read as bytes
p.write_bytes(b'\x00\x01')     # write bytes

# File operations
p.rename('new_name.txt')     # rename/move
p.unlink()                   # delete file
```

### Common Pathlib Patterns

```python
from pathlib import Path

# Iterate over files in directory
directory = Path('/home/user/logs')
for file in directory.iterdir():
    if file.is_file():
        print(file.name)

# Find all Python files recursively
for py_file in directory.rglob('*.py'):
    print(py_file)

# Check file extension
if file.suffix == '.txt':
    process_text_file(file)

# Build path safely
config_file = Path.home() / '.config' / 'app' / 'settings.ini'

# Get absolute path
absolute = Path('relative/path').absolute()
```

---

## Compressed Files (gzip)

### Reading Gzip Files

```python
import gzip

# Read as text
with gzip.open('file.txt.gz', 'rt') as f:
    content = f.read()

# Read as bytes
with gzip.open('file.txt.gz', 'rb') as f:
    data = f.read()

# Read line by line
with gzip.open('file.txt.gz', 'rt') as f:
    for line in f:
        print(line.strip())
```

### Writing Gzip Files

```python
import gzip

# Write text
with gzip.open('output.txt.gz', 'wt') as f:
    f.write('Hello, World!\n')

# Write bytes
with gzip.open('output.bin.gz', 'wb') as f:
    f.write(b'\x00\x01\x02\x03')

# Compress existing file
with open('input.txt', 'rb') as f_in:
    with gzip.open('output.txt.gz', 'wb') as f_out:
        f_out.writelines(f_in)
```

### Other Compression Formats

```python
import zipfile
import tarfile

# Zip files
with zipfile.ZipFile('archive.zip', 'r') as zip_ref:
    zip_ref.extractall('destination_folder')
    names = zip_ref.namelist()

# Tar files
with tarfile.open('archive.tar.gz', 'r:gz') as tar:
    tar.extractall('destination_folder')
    names = tar.getnames()
```

---

## CSV Files

### Reading CSV

```python
import csv

# Basic reading
with open('data.csv', 'r') as f:
    reader = csv.reader(f)
    for row in reader:
        print(row)  # row is a list

# Reading with headers
with open('data.csv', 'r') as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(row['column_name'])  # row is a dict

# Custom delimiter
with open('data.tsv', 'r') as f:
    reader = csv.reader(f, delimiter='\t')
```

### Writing CSV

```python
import csv

# Basic writing
with open('output.csv', 'w', newline='') as f:
    writer = csv.writer(f)
    writer.writerow(['Name', 'Age', 'City'])
    writer.writerow(['Alice', 25, 'NYC'])
    writer.writerows([
        ['Bob', 30, 'LA'],
        ['Charlie', 35, 'Chicago']
    ])

# Writing with headers
with open('output.csv', 'w', newline='') as f:
    fieldnames = ['name', 'age', 'city']
    writer = csv.DictWriter(f, fieldnames=fieldnames)
    writer.writeheader()
    writer.writerow({'name': 'Alice', 'age': 25, 'city': 'NYC'})
```

---

## JSON Files

### Reading JSON

```python
import json

# Read from file
with open('data.json', 'r') as f:
    data = json.load(f)  # returns dict or list

# Parse JSON string
json_string = '{"name": "Alice", "age": 25}'
data = json.loads(json_string)
```

### Writing JSON

```python
import json

data = {
    "name": "Alice",
    "age": 25,
    "languages": ["Python", "JavaScript"]
}

# Write to file
with open('output.json', 'w') as f:
    json.dump(data, f)

# Write with formatting
with open('output.json', 'w') as f:
    json.dump(data, f, indent=4, sort_keys=True)

# Convert to JSON string
json_string = json.dumps(data, indent=2)
```

---

## Error Handling

### Common File Errors

```python
# File not found
try:
    with open('nonexistent.txt', 'r') as f:
        content = f.read()
except FileNotFoundError:
    print("File not found!")

# Permission denied
try:
    with open('/root/file.txt', 'r') as f:
        content = f.read()
except PermissionError:
    print("Permission denied!")

# Unicode decode error
try:
    with open('file.txt', 'r') as f:
        content = f.read()
except UnicodeDecodeError:
    print("Cannot decode file - might be binary")
    # Try reading as binary or with different encoding
    with open('file.txt', 'rb') as f:
        content = f.read()

# Generic error handling
try:
    with open('file.txt', 'r') as f:
        content = f.read()
except Exception as e:
    print(f"Error: {e}")
```

### Safe File Operations

```python
from pathlib import Path

# Check before opening
filepath = Path('file.txt')
if filepath.exists() and filepath.is_file():
    content = filepath.read_text()

# Use exist_ok for directories
Path('new_dir').mkdir(exist_ok=True)

# Skip files that can't be decoded
for filepath in Path('.').rglob('*.txt'):
    try:
        content = filepath.read_text()
        process(content)
    except UnicodeDecodeError:
        continue  # Skip binary files
```

---

## Common File I/O Patterns

### Process Large Files Line by Line

```python
# Memory efficient - doesn't load entire file
with open('large_file.txt', 'r') as f:
    for line in f:
        process(line.strip())
```

### Filter Files by Extension

```python
from pathlib import Path

# Using pathlib
txt_files = list(Path('.').glob('*.txt'))

# Recursive search
all_py_files = list(Path('.').rglob('*.py'))

# Filter manually
for file in Path('.').iterdir():
    if file.suffix == '.txt':
        print(file)
```

### Find Files Containing String

```python
from pathlib import Path

search_text = "error"
for filepath in Path('/var/log').rglob('*.log'):
    try:
        content = filepath.read_text()
        if search_text in content:
            print(f"Found in: {filepath}")
    except (UnicodeDecodeError, PermissionError):
        continue  # Skip files we can't read
```

### Copy Directory Structure

```python
import shutil

# Copy entire directory tree
shutil.copytree('source_dir', 'dest_dir')

# Copy only files matching pattern
shutil.copytree('source', 'dest', 
                ignore=shutil.ignore_patterns('*.pyc', 'tmp*'))
```

### Temporary Files

```python
import tempfile

# Create temporary file
with tempfile.NamedTemporaryFile(mode='w', delete=False) as f:
    f.write('temporary data')
    temp_path = f.name

# Create temporary directory
with tempfile.TemporaryDirectory() as temp_dir:
    # Use temp_dir for temporary files
    temp_file = Path(temp_dir) / 'temp.txt'
    temp_file.write_text('data')
# Directory and contents automatically deleted
```

---

## Performance Tips

1. **Use context managers** (`with` statement) - ensures files are closed
2. **Read line by line** for large files - don't load entire file into memory
3. **Use pathlib** for modern, OS-independent path handling
4. **Buffer writes** when writing many small pieces
5. **Use binary mode** when encoding doesn't matter
6. **Skip unreadable files** with try/except when processing many files

---

## Quick Reference Card

```python
# Read entire file
content = Path('file.txt').read_text()

# Write to file
Path('output.txt').write_text('Hello, World!')

# Read lines into list
lines = Path('file.txt').read_text().splitlines()

# Process file line by line (memory efficient)
with open('file.txt', 'r') as f:
    for line in f:
        process(line.strip())

# List all files in directory
files = list(Path('.').iterdir())

# Find files recursively
txt_files = list(Path('.').rglob('*.txt'))

# Check if file exists
if Path('file.txt').exists():
    # ...

# Get file extension
ext = Path('file.txt').suffix  # '.txt'

# Read gzipped file
with gzip.open('file.txt.gz', 'rt') as f:
    content = f.read()
```

---
