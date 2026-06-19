Network Logs

```python
[2026-06-19 08:15:23] INFO Login successful from IP 192.168.1.10 Status Code: 200

[2026-06-19 08:17:45] WARNING Failed login attempt from IP 192.168.1.25 Status Code: 401

[2026-06-19 08:20:11] INFO Connection established from IP 192.168.1.30 to Server 10.0.0.5 Port 443 Status Code: 200

[2026-06-19 08:25:39] WARNING Failed login attempt from IP 192.168.1.25 Status Code: 401

[2026-06-19 08:30:05] ALERT Multiple failed login attempts from IP 192.168.1.25 Status Code: 403

[2026-06-19 08:35:22] INFO User accessed file server from IP 192.168.1.45 Status Code: 200

[2026-06-19 08:40:18] WARNING Unauthorized access attempt from IP 192.168.1.60 Status Code: 403

[2026-06-19 08:50:27] WARNING Failed SSH login attempt from IP 192.168.1.25 Status Code: 401

[2026-06-19 08:55:10] ALERT Possible brute-force attack detected from IP 192.168.1.25 Status Code: 403
```

Python script to parse log files and extract key information

```python
import re
from collections import Counter

suspicious_ips = []

pattern = r"\[(.*?)\]\s+(\w+).*?IP\s+(\d+\.\d+\.\d+\.\d+).*?Status Code:\s+(\d+)"

with open(r"Z:\brocamp\week 16 python\log Analysis\network_log.txt", "r") as file:
    logs = file.readlines()

print("\n========== NETWORK LOG ANALYSIS ==========\n")

for log in logs:
    match = re.search(pattern, log)

    if match:
        timestamp = match.group(1)
        status_level = match.group(2)
        ip_address = match.group(3)
        status_code = match.group(4)

        print(f"Timestamp    : {timestamp}")
        print(f"Status Level : {status_level}")
        print(f"IP Address   : {ip_address}")
        print(f"Status Code  : {status_code}")
        print("-" * 45)

        if status_code in ["401", "403"]:
            suspicious_ips.append(ip_address)

ip_count = Counter(suspicious_ips)

print("\n========== SECURITY REPORT ==========\n")

if ip_count:
    print("Suspicious IP Activity:\n")

    for ip, count in ip_count.items():
        print(f"{ip} --> {count} suspicious attempts")

        if count >= 3:
            print(f"ALERT: Possible brute-force attack detected from {ip}")

else:
    print("No suspicious activity found.")
```

Output:

```python

========== NETWORK LOG ANALYSIS ==========

Timestamp    : 2026-06-19 08:15:23
Status Level : INFO
IP Address   : 192.168.1.10
Status Code  : 200
---------------------------------------------
Timestamp    : 2026-06-19 08:17:45
Status Level : WARNING
IP Address   : 192.168.1.25
Status Code  : 401
---------------------------------------------
Timestamp    : 2026-06-19 08:20:11
Status Level : INFO
IP Address   : 192.168.1.30
Status Code  : 200
---------------------------------------------
Timestamp    : 2026-06-19 08:25:39
Status Level : WARNING
IP Address   : 192.168.1.25
Status Code  : 401
---------------------------------------------
Timestamp    : 2026-06-19 08:30:05
Status Level : ALERT
IP Address   : 192.168.1.25
Status Code  : 403
---------------------------------------------
Timestamp    : 2026-06-19 08:35:22
Status Level : INFO
IP Address   : 192.168.1.45
Status Code  : 200
---------------------------------------------
Timestamp    : 2026-06-19 08:40:18
Status Level : WARNING
IP Address   : 192.168.1.60
Status Code  : 403
---------------------------------------------
Timestamp    : 2026-06-19 08:50:27
Status Level : WARNING
IP Address   : 192.168.1.25
Status Code  : 401
---------------------------------------------
Timestamp    : 2026-06-19 08:55:10
Status Level : ALERT
IP Address   : 192.168.1.25
Status Code  : 403
---------------------------------------------

========== SECURITY REPORT ==========

Suspicious IP Activity:

192.168.1.25 --> 5 suspicious attempts
ALERT: Possible brute-force attack detected from 192.168.1.25
192.168.1.60 --> 1 suspicious attempts
```

## Findings and Analysis

After analyzing the logs, the following observations were made:

- The script successfully extracted timestamps, IP addresses, status levels, and status codes from all log entries.
- Successful activities were identified with **HTTP Status Code 200 (OK)**.
- Failed authentication attempts were identified with **HTTP Status Code 401 (Unauthorized)**.
- Unauthorized access attempts were identified with **HTTP Status Code 403 (Forbidden)**.
- IP address **192.168.1.25** generated multiple failed login attempts, indicating suspicious repetitive behavior.
- Since the same IP address performed more than three suspicious activities, it was flagged as a possible brute-force attack.
- IP address **192.168.1.60** attempted unauthorized access and was recorded as a suspicious source.
- The `Counter` module was used to calculate the number of suspicious activities from each IP address.

## Conclusion

This project demonstrates how Python can be used for basic cybersecurity log analysis. By parsing network logs and identifying repeated failed attempts, security analysts can detect suspicious behavior and possible brute-force attacks. The project simulates a simplified Security Information and Event Management (SIEM) process, where logs are collected, analyzed, and used to generate security alerts.
