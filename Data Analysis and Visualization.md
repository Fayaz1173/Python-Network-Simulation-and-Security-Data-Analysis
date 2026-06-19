## Project Overview

This project is a **basic cybersecurity log analysis system** built using **Python, Pandas, and Matplotlib**. It analyzes login logs to detect suspicious activities such as repeated login attempts, multiple failed logins, and unusual login times.

Security analysts use similar techniques to monitor system logs and identify possible threats like **brute-force attacks** and unauthorized access attempts.

## Project Objective

The main objectives of this project are:

- Load security login data from a CSV file.
- Clean and preprocess the data.
- Remove duplicate log entries.
- Handle missing information.
- Filter important security-related fields.
- Identify failed login attempts.
- Analyze user activity based on IP addresses and time.
- Create visual graphs to understand security patterns.
- Generate a summary of possible security threats.

## Technologies Used

### Python

The main programming language used to process and analyze the security data.

### Pandas

Used for:

- Reading CSV log files.
- Cleaning data.
- Removing duplicate records.
- Handling missing values.
- Filtering and analyzing log information.

### Matplotlib

Used for creating graphical visualizations such as:

- Bar charts
- Pie charts
- Line graphs

These visualizations make it easier to identify suspicious behavior.

## Project Workflow

```
              Login Logs (CSV)
                      |
                      ↓
             Load Data using Pandas
                      |
                      ↓
             Data Cleaning
        (Remove duplicates & handle missing data)
                      |
                      ↓
              Filter Important Fields
                      |
                      ↓
              Security Analysis
        (Failed logs, IP activity, timing)
                      |
                      ↓
            Visualization with Matplotlib
                      |
                      ↓
          Generate Security Summary Report
```

## Data Visualization

The project creates three visualizations:

### 1. Bar Chart – Login Attempts Per IP

Shows which IP addresses generated the most login requests.

**Use:** Detect suspicious or highly active IP addresses.

---

### 2. Pie Chart – Success vs Failed Logins

Shows the percentage of successful and unsuccessful login attempts.

**Use:** Understand the overall security health of the system.

---

### 3. Line Chart – Login Activity by Hour

Shows how login activity changes throughout the day.

**Use:** Detect unusual spikes and attack timings.

```python
import pandas as pd
import matplotlib.pyplot as plt

print("=" * 60)
print("        SECURITY LOG ANALYSIS USING PANDAS")
print("=" * 60)

data = pd.read_csv(
    r"Z:\brocamp\week 16 python\Visualization\login_logs.csv"
)
print("\nOriginal Dataset:")
print(data)

print("\nTotal records before cleaning:", len(data))

data = data.drop_duplicates()

data["ip_address"] = data["ip_address"].fillna("Unknown")

print("\nTotal records after removing duplicates:", len(data))

print("\nMissing Values:")
print(data.isnull().sum())

security_data = data[
    ["timestamp", "username", "ip_address", "status"]
]

print("\nCleaned Security Logs:")
print(security_data)

failed_logins = security_data[
    security_data["status"] == "Failed"
]

print("\nFailed Login Attempts:")
print(failed_logins)

print("\nTotal Failed Attempts:", len(failed_logins))

ip_attempts = security_data["ip_address"].value_counts()

plt.figure(figsize=(10, 5))
ip_attempts.plot(
    kind="bar",
    color="skyblue"
)

plt.title("Login Attempts Per IP Address")
plt.xlabel("IP Address")
plt.ylabel("Number of Attempts")
plt.xticks(rotation=45)
plt.grid(axis="y")

plt.tight_layout()
plt.show()

login_status = security_data["status"].value_counts()

plt.figure(figsize=(6, 6))
login_status.plot(
    kind="pie",
    autopct="%1.1f%%",
    colors=["lightgreen", "salmon"]
)

plt.title("Login Success vs Failed Attempts")
plt.ylabel("")

plt.show()

security_data["timestamp"] = pd.to_datetime(
    security_data["timestamp"]
)

security_data["hour"] = (
    security_data["timestamp"].dt.hour
)

hourly_activity = (
    security_data["hour"]
    .value_counts()
    .sort_index()
)

plt.figure(figsize=(10, 5))
hourly_activity.plot(
    kind="line",
    marker="o",
    linewidth=2,
    color="red"
)

plt.title("Login Activity by Hour")
plt.xlabel("Hour of the Day")
plt.ylabel("Number of Login Attempts")
plt.grid(True)

plt.xticks(range(0, 24))

plt.tight_layout()
plt.show()

print("\n" + "=" * 60)
print("              SECURITY ANALYSIS SUMMARY")
print("=" * 60)

print("Most Active IP Address:")
print(
    ip_attempts.idxmax(),
    "with",
    ip_attempts.max(),
    "attempts"
)

print("\nMost Common Login Status:")
print(login_status.idxmax())

print("\nPeak Activity Hour:")
print(
    hourly_activity.idxmax(),
    ":00 with",
    hourly_activity.max(),
    "attempts"
)

print("\nPossible Security Findings:")

if ip_attempts.max() >= 3:
    print("- High number of attempts from one IP. Possible brute-force attack.")

if len(failed_logins) >= 5:
    print("- Multiple failed login attempts detected.")

if hourly_activity.idxmax() >= 22 or hourly_activity.idxmax() <= 5:
    print("- Suspicious late-night login activity detected.")

print("\nAnalysis Completed Successfully.")
```

OUTPUT:

```python
============================================================
        SECURITY LOG ANALYSIS USING PANDAS
============================================================

Original Dataset:
           timestamp username    ip_address   status location
0   2026-06-19 08:30    admin  192.168.1.10  Success    India
1   2026-06-19 08:32    admin  192.168.1.10   Failed    India
2   2026-06-19 08:33    admin  192.168.1.10   Failed    India
3   2026-06-19 08:34    admin  192.168.1.10   Failed    India
4   2026-06-19 09:00    user1  192.168.1.20  Success    India
5   2026-06-19 09:10    user2  192.168.1.25   Failed      USA
6   2026-06-19 09:15    user3  192.168.1.30  Success       UK
7   2026-06-19 10:00    user4  192.168.1.50   Failed    India
8   2026-06-19 10:05    user4  192.168.1.50   Failed    India
9   2026-06-19 10:10    user4  192.168.1.50   Failed    India
10  2026-06-19 11:00    user5           NaN  Success   Canada
11  2026-06-19 23:30  unknown      10.0.0.5   Failed   Russia
12  2026-06-19 23:35  unknown      10.0.0.5   Failed   Russia
13  2026-06-19 23:40  unknown      10.0.0.5   Failed   Russia
14  2026-06-19 23:45  unknown      10.0.0.5   Failed   Russia
15  2026-06-19 23:45  unknown      10.0.0.5   Failed   Russia

Total records before cleaning: 16

Total records after removing duplicates: 15

Missing Values:
timestamp     0
username      0
ip_address    0
status        0
location      0
dtype: int64

Cleaned Security Logs:
           timestamp username    ip_address   status
0   2026-06-19 08:30    admin  192.168.1.10  Success
1   2026-06-19 08:32    admin  192.168.1.10   Failed
2   2026-06-19 08:33    admin  192.168.1.10   Failed
3   2026-06-19 08:34    admin  192.168.1.10   Failed
4   2026-06-19 09:00    user1  192.168.1.20  Success
5   2026-06-19 09:10    user2  192.168.1.25   Failed
6   2026-06-19 09:15    user3  192.168.1.30  Success
7   2026-06-19 10:00    user4  192.168.1.50   Failed
8   2026-06-19 10:05    user4  192.168.1.50   Failed
9   2026-06-19 10:10    user4  192.168.1.50   Failed
10  2026-06-19 11:00    user5       Unknown  Success
11  2026-06-19 23:30  unknown      10.0.0.5   Failed
12  2026-06-19 23:35  unknown      10.0.0.5   Failed
13  2026-06-19 23:40  unknown      10.0.0.5   Failed
14  2026-06-19 23:45  unknown      10.0.0.5   Failed

Failed Login Attempts:
           timestamp username    ip_address  status
1   2026-06-19 08:32    admin  192.168.1.10  Failed
2   2026-06-19 08:33    admin  192.168.1.10  Failed
3   2026-06-19 08:34    admin  192.168.1.10  Failed
5   2026-06-19 09:10    user2  192.168.1.25  Failed
7   2026-06-19 10:00    user4  192.168.1.50  Failed
8   2026-06-19 10:05    user4  192.168.1.50  Failed
9   2026-06-19 10:10    user4  192.168.1.50  Failed
11  2026-06-19 23:30  unknown      10.0.0.5  Failed
12  2026-06-19 23:35  unknown      10.0.0.5  Failed
13  2026-06-19 23:40  unknown      10.0.0.5  Failed
14  2026-06-19 23:45  unknown      10.0.0.5  Failed

Total Failed Attempts: 11

============================================================
              SECURITY ANALYSIS SUMMARY
============================================================
Most Active IP Address:
192.168.1.10 with 4 attempts

Most Common Login Status:
Failed

Peak Activity Hour:
8 :00 with 4 attempts

Possible Security Findings:
- High number of attempts from one IP. Possible brute-force attack.
- Multiple failed login attempts detected.

Analysis Completed Successfully.
```

[]()

<img width="1250" height="700" alt="Screenshot 2026-06-19 190330" src="https://github.com/user-attachments/assets/2f4aa871-2343-4fef-991c-64604f11641d" />

<img width="747" height="830" alt="Screenshot 2026-06-19 190341" src="https://github.com/user-attachments/assets/e368e102-b86c-4557-9e2d-a039f5a346cc" />

<img width="1242" height="705" alt="Screenshot 2026-06-19 190354" src="https://github.com/user-attachments/assets/498465d7-2442-4731-8c14-a8b18c88d862" />

