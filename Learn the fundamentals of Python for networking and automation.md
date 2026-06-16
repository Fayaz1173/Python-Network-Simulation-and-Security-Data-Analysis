## 1. What is Network Automation?

Network automation is the process of using scripts and software to automatically configure, manage, monitor, and test network devices instead of doing tasks manually.

### Why Use Automation?

- Reduces human errors.
- Saves time.
- Improves consistency.
- Allows management of many devices at once.

### Example Tasks

- Configuring routers and switches.
- Backing up device configurations.
- Monitoring network status.
- Collecting logs from multiple devices.

---

## 2. Why Python for Network Automation?

Python is widely used because it:

- Has simple syntax.
- Has many networking libraries.
- Works on Windows, Linux, and macOS.
- Easily interacts with APIs and network devices.

### Example

```
devices= ["Router1","Router2","Switch1"]

fordeviceindevices:
print(f"Checking{device}")

```

# 3.Python in Cybersecurity

Cybersecurity professionals use Python for:

### Automation

- Log analysis
- Vulnerability scanning
- Threat detection

### Penetration Testing

- Network scanning
- Banner grabbing
- Exploit development

### Incident Response

- Malware analysis
- Forensics
- Automated alerts

# Common Python Modules for Cybersecurity

| Module | Purpose |
| --- | --- |
| socket | Network communication |
| requests | HTTP requests |
| paramiko | SSH automation |
| netmiko | Network device automation |
| scapy | Packet creation and analysis |
| nmap-python | Nmap integration |
| logging | Log management |
| hashlib | Hash generation |
| re | Pattern matching |

# Python Libraries Commonly Used in Networking

These libraries are frequently used in network automation, monitoring, cybersecurity, and data analysis.

---

# 1. Socket Library

## What is Socket?

The `socket` module is Python's built-in library for network communication.

A socket acts as an endpoint through which two devices communicate over a network.

### Uses

- Client-server communication
- Port scanning
- Network testing
- TCP/UDP communication

## Types of Sockets

### TCP Socket

- Connection-oriented
- Reliable
- Data arrives in order
- Used for web applications

Example:

```
Browser → Website
```

### UDP Socket

- Connectionless
- Faster
- No guarantee of delivery
- Used in gaming, streaming

Example:

```
Online games
Video streaming
```

## Socket Programming Workflow

### Server Side

1. Create socket
2. Bind IP and Port
3. Listen for connections
4. Accept client connection
5. Send/Receive data
6. Close connection

### Client Side

1. Create socket
2. Connect to server
3. Send/Receive data
4. Close connection

## TCP vs UDP

| Feature | TCP | UDP |
| --- | --- | --- |
| Connection | Required | Not Required |
| Reliability | High | Low |
| Speed | Slower | Faster |
| Ordering | Guaranteed | Not Guaranteed |
| Usage | Web, Email | Gaming, Streaming |

## Applications in Cybersecurity

Socket programming is used for:

### Port Scanners

Check open ports on a target system.

### Banner Grabbing

Identify services running on ports.

### Network Monitoring

Capture and analyze traffic.

### Chat Applications

Secure communication systems.

### Vulnerability Assessment

Test network services and configurations.

# 2. Scapy

`Scapy` is a powerful Python library used for **network packet creation, manipulation, sending, capturing, and analysis**. It is widely used in **network automation, cybersecurity, penetration testing, and packet analysis**.

## Features of Scapy

- Create custom network packets
- Send and receive packets
- Perform network scanning
- Sniff network traffic
- Analyze packets
- Conduct security testing
- Automate network tasks

## Common Scapy Functions

| Function | Purpose |
| --- | --- |
| `send()` | Send packets |
| `sr()` | Send and receive multiple packets |
| `sr1()` | Send and receive one packet |
| `sniff()` | Capture packets |
| `ARP()` | Create ARP packets |
| `IP()` | Create IP packets |
| `TCP()` | Create TCP packets |
| `UDP()` | Create UDP packets |
| `ICMP()` | Create ICMP packets |
| `packet.show()` | Display packet details |
| `packet.summary()` | Display packet summary |

## Applications in Cybersecurity

- Network Discovery
- Port Scanning
- Packet Analysis
- Intrusion Detection Testing
- Protocol Testing
- Security Research
- Network Troubleshooting

## Pandas in Python

**Pandas** is a powerful Python library used for **data analysis, manipulation, and processing**. It helps work with structured data such as tables, CSV files, Excel files, and databases.

### Why Pandas?

- Read and write data from files (CSV, Excel, JSON, etc.)
- Clean and preprocess data
- Filter, sort, and analyze data
- Handle missing values
- Perform statistical operations
- Create data summaries

### 

## Networking/Cybersecurity Use Case

Analyze network logs:

```python
importpandasaspd

logs=pd.read_csv("network_logs.csv")

# Show top 5 rows
print(logs.head())

# Filter failed login attempts
failed=logs[logs["status"]=="failed"]

print(failed)
```

This is useful for:

- Log analysis
- Network traffic analysis
- Security event monitoring
- Reporting and visualization (with Matplotlib)

# Matplotlib in Cyber Security

**Matplotlib** is a Python data visualization library used in **cybersecurity** to analyze and display security-related data such as **network traffic, attack patterns, login attempts, malware activity, and system logs**.

## Why Matplotlib is Used in Cybersecurity

- Visualize network traffic trends
- Detect unusual spikes in activity
- Analyze failed and successful login attempts
- Monitor malware detection statistics
- Track security incidents over time
- Create reports and dashboards

## Common Matplotlib Functions in Cybersecurity

| Function | Cybersecurity Use |
| --- | --- |
| `plt.plot()` | Monitor network traffic trends |
| `plt.bar()` | Compare login failures or attack counts |
| `plt.pie()` | Show malware distribution |
| `plt.hist()` | Analyze port access frequency |
| `plt.scatter()` | Detect unusual security events |
| `plt.title()` | Add graph title |
| `plt.xlabel()` | Label data parameters |
| `plt.ylabel()` | Label measurements |
| `plt.grid()` | Improve readability |
| `plt.show()` | Display the visualization |

## Review basic Python concepts required: file handling, loops, exception handling,
and data structures.

## 1. File Handling

File handling is used to create, read, write, and modify files in Python.

### Opening a File

```
file=open("example.txt","r")# Read mode
```

### Common File Modes

- `"r"` – Read a file
- `"w"` – Write to a file (creates a new file or overwrites existing content)
- `"a"` – Append data to a file
- `"x"` – Create a new file
- `"b"` – Binary mode

### Reading from a File

```
file=open("example.txt","r")
content=file.read()
print(content)
file.close()
```

### Writing to a File

```
file=open("example.txt","w")
file.write("Hello, Python!")
file.close()
```

### Using `with` Statement (Recommended)

```
withopen("example.txt","r")asfile:
content=file.read()
print(content)
```

The `with` statement automatically closes the file after use.

---

## 2. Loops

Loops allow a block of code to execute repeatedly.

### `for` Loop

Used to iterate over a sequence.

```
foriinrange(5):
print(i)
```

**Output:**

```
0
1
2
3
4
```

### `while` Loop

Executes as long as a condition is true.

```
count=1

whilecount<=5:
print(count)
count+=1
```

### Loop Control Statements

- `break` – Terminates the loop.
- `continue` – Skips the current iteration.
- `pass` – Acts as a placeholder.

Example:

```
foriinrange(5):
ifi==3:
break
print(i)
```

---

## 3. Exception Handling

Exception handling prevents a program from crashing when errors occur.

### Basic Syntax

```
try:
num=int(input("Enter a number: "))
result=10/num

exceptValueError:
print("Invalid input. Enter a number.")

exceptZeroDivisionError:
print("Cannot divide by zero.")

finally:
print("Execution completed.")
```

### Keywords

- `try` – Contains code that may cause an error.
- `except` – Handles the error.
- `else` – Runs when no exception occurs.
- `finally` – Runs whether an exception occurs or not.

---

## 4. Data Structures

Python provides built-in data structures to store and organize data.

### List

Ordered, mutable collection.

```
fruits= ["apple","banana","orange"]
fruits.append("mango")
print(fruits)
```

### Tuple

Ordered, immutable collection.

```
coordinates= (10,20)
print(coordinates[0])
```

### Set

Unordered collection of unique elements.

```
numbers= {1,2,3,3,4}
print(numbers)
```

**Output:**

```
{1, 2, 3, 4}
```

### Dictionary

Stores data as key–value pairs.

```
student= {
"name":"Alex",
"age":20,
"grade":"A"
}

print(student["name"])
```

---

### Summary Table

| Concept | Purpose | Examples |
| --- | --- | --- |
| File Handling | Work with files | `open()`, `read()`, `write()` |
| Loops | Repeat tasks | `for`, `while`, `break`, `continue` |
| Exception Handling | Manage errors | `try`, `except`, `finally` |
| Data Structures | Store and organize data | List, Tuple, Set, Dictionary |
