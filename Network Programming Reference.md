> **For GPYC/pyWars Preparation**

## Socket Programming

### TCP Sockets

#### TCP Client

```python
import socket

# Create socket
client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Connect to server
client.connect(('example.com', 80))

# Send data
client.send(b'GET / HTTP/1.1\r\nHost: example.com\r\n\r\n')

# Receive data
response = client.recv(4096)  # buffer size in bytes
print(response.decode())

# Close connection
client.close()
```

#### TCP Server

```python
import socket

# Create socket
server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Bind to address and port
server.bind(('0.0.0.0', 8000))

# Listen for connections (max 5 queued)
server.listen(5)
print("Server listening on port 8000")

# Accept connection
client_socket, client_address = server.accept()
print(f"Connection from {client_address}")

# Receive data
data = client_socket.recv(1024)
print(f"Received: {data.decode()}")

# Send response
client_socket.send(b'Hello from server!')

# Close connections
client_socket.close()
server.close()
```

#### Multi-threaded TCP Server

```python
import socket
import threading

def handle_client(client_socket, address):
    print(f"Handling client {address}")
    data = client_socket.recv(1024)
    client_socket.send(b'Response from server')
    client_socket.close()

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind(('0.0.0.0', 8000))
server.listen(5)

while True:
    client, address = server.accept()
    thread = threading.Thread(target=handle_client, args=(client, address))
    thread.start()
```

### UDP Sockets

#### UDP Client

```python
import socket

# Create UDP socket
client = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

# Send data (no connection needed)
message = b'Hello Server'
client.sendto(message, ('example.com', 9000))

# Receive response
data, server_address = client.recvfrom(4096)
print(f"Received: {data.decode()}")

client.close()
```

#### UDP Server

```python
import socket

server = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
server.bind(('0.0.0.0', 9000))

print("UDP Server listening on port 9000")

while True:
    data, client_address = server.recvfrom(4096)
    print(f"From {client_address}: {data.decode()}")
    
    # Send response
    server.sendto(b'ACK', client_address)
```

### Socket Options

```python
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Set timeout
s.settimeout(5.0)  # 5 seconds

# Reuse address (useful for servers)
s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

# Get socket info
s.getpeername()  # remote address
s.getsockname()  # local address

# Non-blocking mode
s.setblocking(False)
```

---

## HTTP Requests (requests library)

### Installation

```bash
pip install requests --break-system-packages
```

### Basic GET Request

```python
import requests

# Simple GET
response = requests.get('https://api.example.com/data')

# Check status
if response.status_code == 200:
    print("Success!")

# Get response content
text = response.text           # as string
data = response.json()         # parse JSON
raw = response.content         # as bytes

# Response properties
response.status_code   # 200, 404, etc.
response.headers       # response headers
response.url          # final URL (after redirects)
response.encoding     # character encoding
```

### GET with Parameters

```python
import requests

# URL parameters
params = {
    'search': 'python',
    'page': 1,
    'limit': 10
}
response = requests.get('https://api.example.com/search', params=params)
# Requests: https://api.example.com/search?search=python&page=1&limit=10
```

### POST Requests

```python
import requests

# POST with form data
data = {
    'username': 'alice',
    'password': 'secret123'
}
response = requests.post('https://api.example.com/login', data=data)

# POST with JSON
json_data = {
    'name': 'Alice',
    'email': 'alice@example.com'
}
response = requests.post('https://api.example.com/users', json=json_data)

# POST with files
files = {'file': open('document.pdf', 'rb')}
response = requests.post('https://api.example.com/upload', files=files)
```

### Headers and Authentication

```python
import requests

# Custom headers
headers = {
    'User-Agent': 'MyApp/1.0',
    'Authorization': 'Bearer YOUR_TOKEN',
    'Content-Type': 'application/json'
}
response = requests.get('https://api.example.com/data', headers=headers)

# Basic authentication
response = requests.get('https://api.example.com/data', 
                       auth=('username', 'password'))

# Bearer token authentication
headers = {'Authorization': f'Bearer {token}'}
response = requests.get('https://api.example.com/data', headers=headers)
```

### Cookies and Sessions

```python
import requests

# Send cookies
cookies = {'session_id': 'abc123'}
response = requests.get('https://example.com', cookies=cookies)

# Access response cookies
response.cookies['session_id']

# Use session (maintains cookies)
session = requests.Session()
session.get('https://example.com/login')
session.post('https://example.com/data')  # cookies maintained
```

### Timeouts and Error Handling

```python
import requests
from requests.exceptions import Timeout, ConnectionError

try:
    # Set timeout (seconds)
    response = requests.get('https://example.com', timeout=5)
    response.raise_for_status()  # raise exception for 4xx/5xx
    
except Timeout:
    print("Request timed out")
except ConnectionError:
    print("Connection failed")
except requests.exceptions.HTTPError as e:
    print(f"HTTP error: {e}")
except requests.exceptions.RequestException as e:
    print(f"Error: {e}")
```

### Download Files

```python
import requests

# Download and save file
response = requests.get('https://example.com/file.pdf')
with open('downloaded.pdf', 'wb') as f:
    f.write(response.content)

# Stream large files
response = requests.get('https://example.com/large_file.zip', stream=True)
with open('large_file.zip', 'wb') as f:
    for chunk in response.iter_content(chunk_size=8192):
        f.write(chunk)
```

---

## Web Scraping (BeautifulSoup)

### Installation

```bash
pip install beautifulsoup4 --break-system-packages
```

### Basic Scraping

```python
import requests
from bs4 import BeautifulSoup

# Fetch page
response = requests.get('https://example.com')
html = response.text

# Parse HTML
soup = BeautifulSoup(html, 'html.parser')

# Find elements
title = soup.title.string           # <title> text
h1 = soup.find('h1')               # first <h1>
all_links = soup.find_all('a')     # all <a> tags
divs = soup.find_all('div', class_='content')  # divs with class="content"
```

### Finding Elements

```python
from bs4 import BeautifulSoup

# By tag name
soup.find('div')              # first div
soup.find_all('p')            # all paragraphs

# By class
soup.find(class_='header')    # first element with class="header"
soup.find_all(class_='item')  # all elements with class="item"

# By ID
soup.find(id='main')          # element with id="main"

# By attributes
soup.find('a', href='#')      # <a href="#">
soup.find('div', {'data-id': '123'})  # <div data-id="123">

# CSS selectors
soup.select('.class-name')    # class selector
soup.select('#id-name')       # id selector
soup.select('div > p')        # child selector
soup.select('a[href]')        # has attribute
```

### Extracting Data

```python
from bs4 import BeautifulSoup

# Get text
element = soup.find('h1')
text = element.text           # or element.get_text()
text = element.string         # direct text only

# Get attributes
link = soup.find('a')
href = link['href']           # or link.get('href')
classes = link['class']       # list of classes

# Get all links
links = []
for a in soup.find_all('a'):
    href = a.get('href')
    if href:
        links.append(href)
```

### Navigating the Tree

```python
# Parent
element.parent

# Children
for child in element.children:
    print(child)

# Siblings
element.next_sibling
element.previous_sibling

# Find parent with tag
element.find_parent('div')

# Find next element
element.find_next('p')
```

### Common Scraping Patterns

```python
import requests
from bs4 import BeautifulSoup

# Extract article titles and links
url = 'https://news.example.com'
response = requests.get(url)
soup = BeautifulSoup(response.text, 'html.parser')

articles = []
for article in soup.find_all('article'):
    title = article.find('h2').text.strip()
    link = article.find('a')['href']
    articles.append({'title': title, 'link': link})

# Extract table data
table = soup.find('table')
rows = []
for tr in table.find_all('tr')[1:]:  # skip header
    cells = [td.text.strip() for td in tr.find_all('td')]
    rows.append(cells)

# Extract metadata
meta_description = soup.find('meta', {'name': 'description'})
if meta_description:
    description = meta_description.get('content')
```

---

## URL Handling (urllib.parse)

### Parse URLs

```python
from urllib.parse import urlparse, parse_qs

url = 'https://example.com:8080/path/page?search=python&page=1#section'

# Parse URL
parsed = urlparse(url)
parsed.scheme    # 'https'
parsed.netloc    # 'example.com:8080'
parsed.hostname  # 'example.com'
parsed.port      # 8080
parsed.path      # '/path/page'
parsed.query     # 'search=python&page=1'
parsed.fragment  # 'section'

# Parse query string
params = parse_qs(parsed.query)
# {'search': ['python'], 'page': ['1']}
```

### Build URLs

```python
from urllib.parse import urlencode, urljoin

# Build query string
params = {'search': 'python', 'page': 1}
query = urlencode(params)  # 'search=python&page=1'

# Join URLs
base = 'https://example.com/path/'
relative = '../other/page.html'
full = urljoin(base, relative)  # 'https://example.com/other/page.html'

# URL encode/decode
from urllib.parse import quote, unquote
encoded = quote('hello world')  # 'hello%20world'
decoded = unquote('hello%20world')  # 'hello world'
```

---

## Network Traffic Analysis (Scapy)

### Installation

```bash
pip install scapy --break-system-packages
```

### Basic Packet Creation

```python
from scapy.all import *

# Create IP packet
ip = IP(dst="8.8.8.8")

# Create ICMP packet (ping)
icmp = ICMP()

# Combine layers
packet = ip/icmp

# Send packet
send(packet)

# Send and receive
response = sr1(packet, timeout=2)
if response:
    response.show()
```

### Reading PCAP Files

```python
from scapy.all import *

# Read pcap file
packets = rdpcap('capture.pcap')

# Iterate through packets
for packet in packets:
    if packet.haslayer(IP):
        src = packet[IP].src
        dst = packet[IP].dst
        print(f"{src} -> {dst}")
    
    if packet.haslayer(TCP):
        sport = packet[TCP].sport
        dport = packet[TCP].dport
        print(f"TCP {sport} -> {dport}")
```

### Packet Filtering

```python
from scapy.all import *

packets = rdpcap('capture.pcap')

# Filter HTTP packets (port 80)
http_packets = [p for p in packets if TCP in p and p[TCP].dport == 80]

# Filter by IP
target_packets = [p for p in packets if IP in p and p[IP].src == '192.168.1.100']

# Filter by protocol
tcp_packets = [p for p in packets if TCP in p]
udp_packets = [p for p in packets if UDP in p]
```

### Creating Packets

```python
from scapy.all import *

# TCP SYN packet
ip = IP(dst="192.168.1.1")
tcp = TCP(dport=80, flags="S")
packet = ip/tcp

# UDP packet with payload
ip = IP(dst="192.168.1.1")
udp = UDP(dport=53)
dns = DNS(rd=1, qd=DNSQR(qname="example.com"))
packet = ip/udp/dns

# Custom payload
payload = Raw(load="Hello World")
packet = IP(dst="192.168.1.1")/TCP(dport=1234)/payload
```

### Sniffing Traffic

```python
from scapy.all import *

# Sniff packets
def packet_callback(packet):
    if packet.haslayer(IP):
        print(f"{packet[IP].src} -> {packet[IP].dst}")

# Sniff 10 packets
sniff(count=10, prn=packet_callback)

# Sniff with filter
sniff(filter="tcp port 80", count=10, prn=packet_callback)

# Sniff on specific interface
sniff(iface="eth0", count=10, prn=packet_callback)
```

---

## DNS Operations

### Using socket

```python
import socket

# Hostname to IP
ip = socket.gethostbyname('example.com')  # '93.184.216.34'

# IP to hostname (reverse DNS)
hostname = socket.gethostbyaddr('93.184.216.34')
# ('example.com', [], ['93.184.216.34'])

# Get all addresses
info = socket.getaddrinfo('example.com', 80)
for item in info:
    print(item[4][0])  # IP addresses
```

### Using dnspython (advanced)

```python
# pip install dnspython --break-system-packages
import dns.resolver

# A record (IPv4)
answers = dns.resolver.resolve('example.com', 'A')
for rdata in answers:
    print(rdata.address)

# MX record (mail servers)
answers = dns.resolver.resolve('example.com', 'MX')
for rdata in answers:
    print(rdata.exchange, rdata.preference)

# TXT records
answers = dns.resolver.resolve('example.com', 'TXT')
for rdata in answers:
    print(rdata.strings)
```

---

## Common Network Patterns

### Port Scanner

```python
import socket

def scan_port(host, port):
    try:
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        sock.settimeout(1)
        result = sock.connect_ex((host, port))
        sock.close()
        return result == 0
    except:
        return False

# Scan common ports
host = '192.168.1.1'
for port in [20, 21, 22, 23, 80, 443, 3306, 8080]:
    if scan_port(host, port):
        print(f"Port {port} is open")
```

### HTTP Server (Simple)

```python
from http.server import HTTPServer, SimpleHTTPRequestHandler

# Serve current directory on port 8000
server = HTTPServer(('0.0.0.0', 8000), SimpleHTTPRequestHandler)
print("Server running on port 8000")
server.serve_forever()
```

### Download with Progress

```python
import requests

url = 'https://example.com/large_file.zip'
response = requests.get(url, stream=True)
total_size = int(response.headers.get('content-length', 0))

with open('file.zip', 'wb') as f:
    downloaded = 0
    for chunk in response.iter_content(chunk_size=8192):
        f.write(chunk)
        downloaded += len(chunk)
        percent = (downloaded / total_size) * 100
        print(f"Downloaded: {percent:.1f}%", end='\r')
```

### Check if Website is Up

```python
import requests

def is_site_up(url, timeout=5):
    try:
        response = requests.get(url, timeout=timeout)
        return response.status_code < 500
    except:
        return False

if is_site_up('https://example.com'):
    print("Site is up")
else:
    print("Site is down")
```

### Extract All Links from Page

```python
import requests
from bs4 import BeautifulSoup
from urllib.parse import urljoin

def get_all_links(url):
    response = requests.get(url)
    soup = BeautifulSoup(response.text, 'html.parser')
    
    links = []
    for a in soup.find_all('a', href=True):
        href = a['href']
        full_url = urljoin(url, href)  # handle relative URLs
        links.append(full_url)
    
    return links

links = get_all_links('https://example.com')
```

---

## Best Practices

### Error Handling

```python
import requests
from requests.exceptions import RequestException

try:
    response = requests.get(url, timeout=10)
    response.raise_for_status()
    data = response.json()
except requests.exceptions.Timeout:
    print("Request timed out")
except requests.exceptions.ConnectionError:
    print("Connection failed")
except requests.exceptions.HTTPError as e:
    print(f"HTTP error {response.status_code}")
except RequestException as e:
    print(f"Request failed: {e}")
```

### Respect robots.txt

```python
from urllib.robotparser import RobotFileParser

rp = RobotFileParser()
rp.set_url("https://example.com/robots.txt")
rp.read()

if rp.can_fetch("*", "https://example.com/page"):
    # OK to scrape
    pass
```

### Rate Limiting

```python
import time
import requests

# Add delay between requests
urls = ['https://example.com/page1', 'https://example.com/page2']
for url in urls:
    response = requests.get(url)
    process(response)
    time.sleep(1)  # 1 second delay
```

### User Agent

```python
import requests

# Always set a proper User-Agent
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36'
}
response = requests.get('https://example.com', headers=headers)
```

---

## Quick Reference Card

```python
# Socket basics
import socket
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)  # TCP
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)   # UDP
s.connect((host, port))
s.send(data)
s.recv(bufsize)

# HTTP requests
import requests
response = requests.get(url)
response = requests.post(url, data=data)
response = requests.post(url, json=json_data)
response.status_code
response.text / response.json() / response.content

# Web scraping
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'html.parser')
soup.find('tag')
soup.find_all('tag', class_='name')
element.text / element['attribute']

# Scapy
from scapy.all import *
packets = rdpcap('file.pcap')
packet = IP(dst="1.1.1.1")/ICMP()
send(packet)
```

---

**Previous**: [String Manipulation](https://claude.ai/chat/04_string_manipulation.md) | **Next**: [Security & Pentesting](https://claude.ai/chat/06_security_pentesting.md)
