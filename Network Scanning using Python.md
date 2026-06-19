## Network Scanning using Python
## Script to scan a range of IP addresses and identify active hosts

```python
from scapy.all import ARP, Ether, srp, IP, ICMP, sr1
import time

network = "192.168.56.0/24"

print("\nStarting Network Scan...")
print("-" * 80)

arp = ARP(pdst=network)
ether = Ether(dst="ff:ff:ff:ff:ff:ff")

packet = ether / arp

result = srp(packet, timeout=2, verbose=0)[0]

print("IP Address\t\tMAC Address\t\tResponse Time")
print("-" * 80)

active_hosts = 0

for sent, received in result:
    ip = received.psrc
    mac = received.hwsrc

    start_time = time.time()

    reply = sr1(
        IP(dst=ip) / ICMP(),
        timeout=1,
        verbose=0
    )

    end_time = time.time()

    if reply:
        response_time = (end_time - start_time) * 1000

        print(
            f"{ip}\t{mac}\t{response_time:.2f} ms"
        )

        active_hosts += 1

print("-" * 80)
print(f"Total Reachable Hosts: {active_hosts}")
print("Network Scan Completed")
```

## output

```python
Starting Network Scan...
--------------------------------------------------------------------------------
IP Address              MAC Address             Response Time
--------------------------------------------------------------------------------
192.168.56.1    ac:71:2e:d3:0e:69       30.72 ms
192.168.56.84   cc:47:40:e9:56:0a       8.15 ms
192.168.56.89   9e:99:db:ee:2a:5b       357.44 ms
192.168.56.100  da:e5:04:18:55:25       359.29 ms
192.168.56.98   2e:8f:21:c3:ae:15       626.06 ms
192.168.56.73   74:0e:a4:97:8f:b3       132.08 ms
192.168.56.65   62:53:2d:9d:33:b8       566.70 ms
192.168.56.114  be:41:06:85:50:51       214.84 ms
192.168.56.124  c6:3c:e4:eb:68:55       329.59 ms
192.168.56.82   22:54:de:c3:67:66       505.44 ms
--------------------------------------------------------------------------------
Total Reachable Hosts: 10
Network Scan Completed
```

### Step 1: ARP Scan

It discovers devices that reply to an ARP request.

### Step 2: ICMP Ping Verification

It then sends an ICMP Echo Request (ping) and only displays devices that send an ICMP Echo Reply.

So your output:

```
Total Reachable Hosts: 10
```

means **10 devices responded to both ARP and ICMP**.

# How Network Scanning Helps in Cybersecurity Monitoring

Network scanning helps cybersecurity professionals identify and monitor devices connected to a network. It provides information such as active IP addresses, available services, open ports, and device status.

By performing regular network scans, security teams can detect unauthorized devices, identify potential vulnerabilities, and ensure that all network assets are accounted for. It also helps in detecting misconfigurations, monitoring network changes, and improving overall network security.

In summary, network scanning plays an important role in **network visibility, threat detection, vulnerability assessment, and maintaining a secure network environment**.
