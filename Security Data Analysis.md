### Security Log Analysis Project — Report

### 1. Project Overview

This project implements an end-to-end Python pipeline for analyzing security login logs, using **Pandas** for cleaning/transformation and **Matplotlib** for interactive, menu-driven visualization. The script reads a CSV of login events (timestamps, usernames, source/destination IPs, locations, event types, status), cleans it, and then offers an interactive console menu to visualize failed login activity from different angles — a workflow typical of a Security Operations analyst turning raw logs into actionable insight.

### 2. Objectives

1. Clean and standardize raw security log data (blanks, duplicates, missing fields, data types).
2. Persist a cleaned dataset (`cleaned_security_logs.csv`) for reuse.
3. Provide an interactive menu to explore failed login attempts.
4. Surface patterns — peak attack hours, top attacking IPs, most targeted servers, attack types, geographic origin.

### 3. Methodology

**3.1 Data Cleaning (`clean_data`)**

- Loads CSV via `pandas.read_csv()`.
- Replaces empty/whitespace strings with `pd.NA` so missing data is correctly detected.
- Reports shape, missing-value counts, and duplicate count for transparency.
- Drops exact duplicate rows.
- Fills missing values: `"Unknown"` for username/location/status, `"0.0.0.0"` for IPs.
- Converts `timestamp` to proper datetime.
- Exports cleaned data to `cleaned_security_logs.csv`.

**3.2 Visualization (`visualize_logs`)**

Filters to `status == "Failed"`, then loops through a menu:

| # | Visualization | Chart Type | Purpose |
| --- | --- | --- | --- |
| 1 | Failed Attempts by Hour | Line | Time-of-day attack patterns |
| 2 | Top Source IPs | Bar (top 10) | Likely brute-force/attacker IPs |
| 3 | Most Targeted Destination IPs | Bar (top 10) | Systems under most pressure |
| 4 | Failed Attacks by Event Type | Bar | Attack/event category breakdown |
| 5 | Failed Attempts by Country | Bar | Geographic origin of attacks |

Option 6 exits; invalid input is rejected gracefully.

### 4. Key Insights & Analysis

**Strengths**

- Filling missing values (rather than dropping rows) preserves sample size — important since every security event could matter.
- Defaulting missing IPs/usernames to traceable placeholders (`"0.0.0.0"`, `"Unknown"`) keeps logging gaps visible instead of silently discarding records.
- Scoping all charts to `status == "Failed"` keeps analysis tightly focused on the actual security signal.
- The hourly trend chart is especially valuable — spikes at unusual hours (e.g. 2–4 AM) are a classic sign of automated/scripted attacks rather than genuine user error.
- Top Source IPs + Most Targeted Destinations together support triage: one informs IP blocklisting, the other prioritizes which servers need hardening.
- An interactive, repeatable menu supports real exploratory analysis instead of a one-shot script.

### 5.Code snipet

```python
import pandas as pd
import matplotlib.pyplot as plt

def clean_data(file_path):

    df = pd.read_csv(file_path)

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

    df["timestamp"] = pd.to_datetime(df["timestamp"])

    print("\nMissing Values After Cleaning:")
    print(df.isnull().sum())

    print("\nCleaned Security Logs:")
    print(df.head(10))

    df.to_csv("cleaned_security_logs.csv", index=False)

    print("\nCleaned data has been saved as 'cleaned_security_logs.csv'")

    return df

def visualize_logs(df):

    failed_logs = df[df["status"] == "Failed"].copy()

    while True:

        print("\n====== Security Visualization Menu ======")
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

            print("\nExiting Security Visualization Tool...")
            break

        else:

            print("\nInvalid choice! Please enter a number from 1 to 6.")

file_path = "Z:/brocamp/week 16 python/Visualization/login_logs.csv"

security_logs = clean_data(file_path)

visualize_logs(security_logs)
```

Output:

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
0 2026-06-20 08:00:00    admin  192.168.1.10      10.0.0.10  Success        Login    India
1 2026-06-20 08:02:00    admin  192.168.1.10      10.0.0.10   Failed        Login    India
2 2026-06-20 08:03:00    admin  192.168.1.10      10.0.0.10   Failed        Login    India
3 2026-06-20 08:04:00    admin  192.168.1.10      10.0.0.10   Failed        Login    India
4 2026-06-20 08:10:00    user1  192.168.1.20      10.0.0.11  Success          VPN    India
5 2026-06-20 08:15:00    user2  192.168.1.21      10.0.0.12  Success  File Access      USA
6 2026-06-20 08:18:00    user3  192.168.1.22      10.0.0.13   Failed          SSH       UK
7 2026-06-20 08:20:00    user3  192.168.1.22      10.0.0.13   Failed          SSH       UK
8 2026-06-20 08:21:00    user3  192.168.1.22      10.0.0.13   Failed          SSH       UK
9 2026-06-20 08:25:00    user4  192.168.1.23      10.0.0.14  Success          RDP  Germany

Cleaned data has been saved as 'cleaned_security_logs.csv'

====== Security Visualization Menu ======
1. Failed Login Attempts by Hour
2. Top Source IPs with Failed Attempts
3. Most Targeted Destination IPs
4. Failed Attacks by Event Type
5. Failed Attempts by Country
6. Exit

Enter your choice: 
```
choice 1:

<img width="1245" height="700" alt="Screenshot 2026-06-20 103432" src="https://github.com/user-attachments/assets/008f1160-25ba-4289-bd99-4af9f68e8686" />

choice 2:

<img width="1241" height="700" alt="Screenshot 2026-06-20 103558" src="https://github.com/user-attachments/assets/9cf4dd23-44ff-4f04-96f7-b7bde4f9f738" />

choice 3:

<img width="1243" height="702" alt="Screenshot 2026-06-20 103641" src="https://github.com/user-attachments/assets/fcc98390-6f4f-4839-a5fc-35183e9e5fce" />

choice 4:

<img width="992" height="707" alt="Screenshot 2026-06-20 103752" src="https://github.com/user-attachments/assets/b5c9fd01-8b71-42a1-b728-ddd356d0787a" />

choice 5:

<img width="996" height="700" alt="Screenshot 2026-06-20 103857" src="https://github.com/user-attachments/assets/32a50a5d-3125-4398-9aaa-35870f8e7338" />



