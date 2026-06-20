## Pandas in Cybersecurity

### What is Pandas?

Pandas is a Python library used for data manipulation and analysis. It provides two main data structures:

- **Series** – a one-dimensional labeled array
- **DataFrame** – a two-dimensional labeled table (like a spreadsheet or SQL table)

It's built on top of NumPy and is the go-to tool for handling structured/tabular data in Python.

### Why Pandas Matters in Cybersecurity

Security work generates huge volumes of structured data — logs, packet captures, alerts, threat intel feeds. Pandas helps because:

1. **Log Analysis** – Parse and analyze firewall logs, system logs (syslog), web server logs, and authentication logs to spot anomalies.
2. **Handling Large Datasets** – Efficiently process thousands/millions of log entries that would be impractical to review manually.
3. **Pattern & Anomaly Detection** – Group, filter, and aggregate data to find unusual login times, repeated failed attempts, traffic spikes, etc.
4. **Threat Intelligence Correlation** – Merge/join data from multiple sources (IP blacklists, malware hashes, indicators of compromise) to cross-reference events.
5. **Incident Response** – Quickly filter timelines of events during forensic investigation.
6. **Data Cleaning** – Normalize messy, inconsistent log formats from different devices/sources into a clean, analyzable structure.
7. **Visualization Prep** – Pandas integrates with Matplotlib/Seaborn to turn security data into charts (attack trends, geo-distribution of IPs, etc.).
8. **Automation** – Build repeatable scripts/pipelines for daily log review instead of manual checking.

### Common Pandas Functions Used in Security Analysis

### Reading Data

python

`pd.read_csv("logs.csv")          # Read CSV log file
pd.read_json("alerts.json")      # Read JSON formatted logs
pd.read_excel("report.xlsx")     # Read Excel report`

### Inspecting Data

python

`df.head()           # First 5 rows
df.tail()           # Last 5 rows
df.info()           # Column types, non-null counts
df.describe()       # Statistical summary
df.shape            # (rows, columns)
df.columns          # List column names`

### Filtering & Searching (key for threat hunting)

python

`df[df['status'] == 'failed']                       # Filter failed logins
df[df['src_ip'] == '192.168.1.10']                  # Filter by IP
df[df['bytes_sent'] > 1000000]                      # Find large data transfers
df[df['username'].str.contains('admin')]            # Pattern match`

### Aggregation & Grouping (detecting brute force, beaconing, etc.)

python

`df.groupby('src_ip').size()                         # Count events per IP
df.groupby('username')['status'].value_counts()     # Login attempts per user
df['src_ip'].value_counts().head(10)                 # Top 10 most active IPs
df.groupby('src_ip')['timestamp'].count()             # Frequency analysis`

### Sorting

python

`df.sort_values('timestamp', ascending=False)        # Most recent events first
df.sort_values('bytes_sent', ascending=False)        # Largest transfers first`

### Merging / Joining (correlating with threat intel)

python

`pd.merge(logs_df, blacklist_df, on='ip', how='inner')  # Match logs against IOC list
pd.concat([log1_df, log2_df])                            # Combine multiple log sources`

### Time-based Analysis (timeline reconstruction)

python

`df['timestamp'] = pd.to_datetime(df['timestamp'])
df.set_index('timestamp', inplace=True)
df.resample('1H').count()        # Events per hour
df.between_time('00:00', '05:00')  # Off-hour activity`

### Cleaning Data

python

`df.dropna()                       # Remove missing values
df.fillna('unknown')              # Fill missing values
df.drop_duplicates()              # Remove duplicate log entries
df.rename(columns={'src': 'source_ip'})  # Standardize column names`

### Exporting Results

python

`df.to_csv("suspicious_activity.csv", index=False)
df.to_excel("incident_report.xlsx")`

### Example: Detecting Brute Force Login Attempts

python

`import pandas as pd

df = pd.read_csv("auth.log.csv")
failed = df[df['status'] == 'failed']

# Find IPs with more than 10 failed attempts
suspicious = failed.groupby('src_ip').size()
suspicious = suspicious[suspicious > 10]

print(suspicious)`

Cleaning the datas using pandas:

```python
import pandas as pd

df = pd.read_csv("Z:/brocamp/week 16 python/Visualization/login_logs.csv")

print("\nOriginal Dataset Shape:")
print(df.shape)

df = df.replace(r'^\s*$', pd.NA, regex=True)

print("\nMissing Values Count in Each Column:")
print(df.isnull().sum())

print("\nDuplicate Records Count:")
print(df.duplicated().sum())

df = df.drop_duplicates()

print("\nDataset Shape After Removing Duplicates:")
print(df.shape)

df["username"] = df["username"].fillna("Unknown")
df["source_ip"] = df["source_ip"].fillna("0.0.0.0")
df["destination_ip"] = df["destination_ip"].fillna("0.0.0.0")
df["location"] = df["location"].fillna("Unknown")
df["status"] = df["status"].fillna("Unknown")

print("\nMissing Values After Cleaning:")
print(df.isnull().sum())

print("\nCleaned Security Logs:")
print(df.head(10))

df.to_csv("cleaned_security_logs.csv", index=False)

print("\nCleaned data has been saved as 'cleaned_security_logs.csv'")

```

output:

```python
Original Dataset Shape:
(200, 7)

Missing Values Count in Each Column:
timestamp         0
username          6
source_ip         5
destination_ip    5
status            1
event_type        0
location          4
dtype: int64

Duplicate Records Count:
5

Dataset Shape After Removing Duplicates:
(195, 7)

Missing Values After Cleaning:
timestamp         0
username          0
source_ip         0
destination_ip    0
status            0
event_type        0
location          0
dtype: int64

Cleaned Security Logs:
          timestamp username     source_ip destination_ip   status   event_type location
0  2026-06-20 08:00    admin  192.168.1.10      10.0.0.10  Success        Login    India
1  2026-06-20 08:02    admin  192.168.1.10      10.0.0.10   Failed        Login    India
2  2026-06-20 08:03    admin  192.168.1.10      10.0.0.10   Failed        Login    India
3  2026-06-20 08:04    admin  192.168.1.10      10.0.0.10   Failed        Login    India
4  2026-06-20 08:10    user1  192.168.1.20      10.0.0.11  Success          VPN    India
5  2026-06-20 08:15    user2  192.168.1.21      10.0.0.12  Success  File Access      USA
6  2026-06-20 08:18    user3  192.168.1.22      10.0.0.13   Failed          SSH       UK
7  2026-06-20 08:20    user3  192.168.1.22      10.0.0.13   Failed          SSH       UK
8  2026-06-20 08:21    user3  192.168.1.22      10.0.0.13   Failed          SSH       UK
9  2026-06-20 08:25    user4  192.168.1.23      10.0.0.14  Success          RDP  Germany

Cleaned data has been saved as 'cleaned_security_logs.csv'
```

Visualising the datas using Matplotlib:

```python
import pandas as pd
import matplotlib.pyplot as plt

df = pd.read_csv("cleaned_security_logs.csv")

df["timestamp"] = pd.to_datetime(df["timestamp"])

failed_logs = df[df["status"] == "Failed"]

while True:

    print("\n select an option:")
    print("1. Failed Login Attempts by Hour")
    print("2. Top Source IPs with Failed Attempts")
    print("3. Most Targeted Destination IPs")
    print("4. Failed Attacks by Event Type")
    print("5. Failed Attempts by Country")
    print("6. Exit")

    choice = input("\nEnter your choice: ")

    if choice == "1":

        failed_logs["hour"] = failed_logs["timestamp"].dt.hour

        hourly_attacks = (
            failed_logs["hour"]
            .value_counts()
            .sort_index()
        )

        plt.figure(figsize=(10, 5))
        hourly_attacks.plot(
            kind="line",
            marker="o"
        )

        plt.title("Failed Login Attempts by Hour")
        plt.xlabel("Hour of the Day")
        plt.ylabel("Number of Failed Attempts")
        plt.grid(True)
        plt.show()

    elif choice == "2":

        top_attackers = (
            failed_logs["source_ip"]
            .value_counts()
            .head(10)
        )

        plt.figure(figsize=(10, 5))
        top_attackers.plot(
            kind="bar"
        )

        plt.title("Top Source IPs with Failed Attempts")
        plt.xlabel("Source IP Address")
        plt.ylabel("Number of Failed Attempts")
        plt.xticks(rotation=45)
        plt.grid(axis="y")
        plt.show()

    elif choice == "3":

        targeted_servers = (
            failed_logs["destination_ip"]
            .value_counts()
            .head(10)
        )

        plt.figure(figsize=(10, 5))
        targeted_servers.plot(
            kind="bar"
        )

        plt.title("Most Targeted Destination IPs")
        plt.xlabel("Destination IP Address")
        plt.ylabel("Number of Attack Attempts")
        plt.xticks(rotation=45)
        plt.grid(axis="y")
        plt.show()

    elif choice == "4":

        attack_types = (
            failed_logs["event_type"]
            .value_counts()
        )

        plt.figure(figsize=(8, 5))
        attack_types.plot(
            kind="bar"
        )

        plt.title("Failed Attack Types")
        plt.xlabel("Event Type")
        plt.ylabel("Number of Failed Events")
        plt.xticks(rotation=45)
        plt.grid(axis="y")
        plt.show()

    elif choice == "5":

        attack_locations = (
            failed_logs["location"]
            .value_counts()
        )

        plt.figure(figsize=(8, 5))
        attack_locations.plot(
            kind="bar"
        )

        plt.title("Failed Attempts by Country")
        plt.xlabel("Country")
        plt.ylabel("Number of Failed Attempts")
        plt.xticks(rotation=45)
        plt.grid(axis="y")
        plt.show()

    elif choice == "6":

        print("Exiting Security Visualization Tool...")
        break

    else:

        print("Invalid choice! Please enter a number from 1 to 6.")
```

output:

```python
 select an option:
1. Failed Login Attempts by Hour
2. Top Source IPs with Failed Attempts
3. Most Targeted Destination IPs
4. Failed Attacks by Event Type
5. Failed Attempts by Country
6. Exit

Enter your choice: 1
```
choice 1:

<img width="1245" height="700" alt="Screenshot 2026-06-20 103432" src="https://github.com/user-attachments/assets/af16c735-ff41-4464-8c8b-0d658cbcd22a" />


choice 2:

<img width="1241" height="700" alt="Screenshot 2026-06-20 103558" src="https://github.com/user-attachments/assets/776cd10c-c9d8-41fc-966a-7e05b3fa64a6" />

choice 3:

<img width="1243" height="702" alt="Screenshot 2026-06-20 103641" src="https://github.com/user-attachments/assets/17c01faf-4ac4-4083-b20c-52be790060fa" />


choice 4:

<img width="992" height="707" alt="Screenshot 2026-06-20 103752" src="https://github.com/user-attachments/assets/91b7693a-b017-4c50-b3ce-d2e2b6409f0f" />

choice 5:

<img width="996" height="700" alt="Screenshot 2026-06-20 103857" src="https://github.com/user-attachments/assets/e521e1da-edf3-4f1f-9eb8-ebecad33bd3e" />
