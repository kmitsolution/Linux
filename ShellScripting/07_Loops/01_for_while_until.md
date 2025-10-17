Here’s a complete and easy-to-follow explanation of **`for`, `while`, and `until` loops in RHEL (and other Bash shells)** — including syntax and real-world admin examples 👇

---

##  **1. `for` Loop**

###  **Basic Syntax**

```bash
for var in list
do
    command1
    command2
done
```

###  **Explanation**

* Iterates through a list of values.
* The variable (`var`) takes each value one by one.

###  **Examples**

#### Example 1: Print Numbers

```bash
for i in 1 2 3 4 5
do
    echo "Number: $i"
done
```

#### Example 2: Using Brace Expansion

```bash
for i in {1..5}
do
    echo "Loop count: $i"
done
```

#### Example 3 (Sysadmin Use Case): Check Disk Usage of `/home` Directories

```bash
for dir in /home/*
do
    du -sh "$dir"
done
```

#### Example 4: Loop through a File List

```bash
for file in *.log
do
    echo "Processing file: $file"
done
```

---

##  **2. `while` Loop**

###  **Basic Syntax**

```bash
while [ condition ]
do
    commands
done
```

###  **Explanation**

* Runs **while** the condition is **true**.
* Used when you don’t know how many iterations are needed.

###  **Examples**

#### Example 1: Count from 1 to 5

```bash
count=1
while [ $count -le 5 ]
do
    echo "Count is: $count"
    ((count++))
done
```

#### Example 2 (Sysadmin Use Case): Check if a Service Is Running

```bash
while systemctl is-active --quiet httpd
do
    echo "Apache is running..."
    sleep 5
done

echo "Apache stopped!"
```

#### Example 3: Read File Line by Line

```bash
while read line
do
    echo "Line: $line"
done < /etc/passwd
```

---

##  **3. `until` Loop**

###  **Basic Syntax**

```bash
until [ condition ]
do
    commands
done
```

###  **Explanation**

* Similar to `while`, but it runs **until** the condition becomes **true**.
* In other words, runs **while condition is false**.

###  **Examples**

#### Example 1: Run Until Variable Reaches 5

```bash
count=1
until [ $count -gt 5 ]
do
    echo "Count: $count"
    ((count++))
done
```

#### Example 2 (Sysadmin Use Case): Wait for Network to Come Up

```bash
until ping -c1 google.com &>/dev/null
do
    echo "Waiting for network..."
    sleep 3
done

echo "Network is up!"
```

#### Example 3: Wait for File to Exist

```bash
until [ -f /tmp/backup.done ]
do
    echo "Waiting for backup to complete..."
    sleep 5
done

echo "Backup completed!"
```

---

## ⚙️ **Quick Comparison Table**

| Loop Type | Runs While            | Best For                                    |
| --------- | --------------------- | ------------------------------------------- |
| `for`     | Iterating over a list | Known list of items (files, users, etc.)    |
| `while`   | Condition is true     | Continuous monitoring or unknown iterations |
| `until`   | Condition is false    | Waiting for an event to occur               |

---

 **SysAdmin-level real-world examples** for each type of loop (`for`, `while`, and `until`) in **RHEL/Ubuntu Bash scripting** 

---

## ⚙️ **1. `for` Loop – Real-World SysAdmin Examples**

### 🧩 Example 1: Check Disk Usage of All Mounted Filesystems

```bash
#!/bin/bash
for mount in $(df -h --output=target | tail -n +2)
do
    echo "Checking usage on $mount:"
    df -h "$mount" | awk 'NR==2{print "Used:", $3, "Available:", $4, "Usage:", $5}'
    echo "------------------------------------"
done
```

📘 **Use Case:** Quickly report disk usage for all mounted filesystems.

---

### 🧩 Example 2: Restart Multiple Services

```bash
#!/bin/bash
for svc in sshd httpd crond
do
    echo "Restarting $svc..."
    systemctl restart "$svc"
    systemctl is-active --quiet "$svc" && echo "$svc restarted successfully" || echo "$svc failed!"
done
```

📘 **Use Case:** Restart and verify status of multiple critical services.

---

### 🧩 Example 3: Backup Home Directories

```bash
#!/bin/bash
backup_dir="/backup"
mkdir -p "$backup_dir"

for userdir in /home/*
do
    user=$(basename "$userdir")
    echo "Creating backup for $user..."
    tar -czf "$backup_dir/${user}_$(date +%F).tar.gz" "$userdir"
done
```

📘 **Use Case:** Automated user home directory backups.

---

## 🔁 **2. `while` Loop – Real-World SysAdmin Examples**

### 🧩 Example 1: Monitor a Log File in Real Time

```bash
#!/bin/bash
logfile="/var/log/secure"

while read line
do
    echo "$(date +%T) -> $line"
done < <(tail -f "$logfile")
```

📘 **Use Case:** Continuous monitoring of security logs (failed SSH attempts, etc.).

---

### 🧩 Example 2: Check Server Uptime Continuously

```bash
#!/bin/bash
while true
do
    uptime
    sleep 10
done
```

📘 **Use Case:** Periodically monitor system load and uptime.

---

### 🧩 Example 3: Wait for a Service to Start

```bash
#!/bin/bash
service="nginx"

while ! systemctl is-active --quiet "$service"
do
    echo "Waiting for $service to start..."
    sleep 3
done

echo "$service is now running!"
```

📘 **Use Case:** Useful during boot scripts or automated deployments.

---

## 🔂 **3. `until` Loop – Real-World SysAdmin Examples**

### 🧩 Example 1: Wait Until a Host Becomes Reachable

```bash
#!/bin/bash
host="192.168.1.10"

until ping -c1 "$host" &>/dev/null
do
    echo "Waiting for $host to respond..."
    sleep 5
done

echo "$host is reachable!"
```

📘 **Use Case:** Check network connectivity before running dependent scripts.

---

### 🧩 Example 2: Wait Until Backup File Appears

```bash
#!/bin/bash
until [ -f /backup/daily_backup.done ]
do
    echo "Backup not finished yet... waiting..."
    sleep 10
done

echo "Backup completed successfully!"
```

📘 **Use Case:** Synchronize tasks that depend on completion of another process.

---

### 🧩 Example 3: Wait Until CPU Load Is Low Before Running Updates

```bash
#!/bin/bash
until [ $(awk '{print int($1)}' /proc/loadavg) -lt 2 ]
do
    echo "CPU load high, waiting to apply updates..."
    sleep 15
done

echo "CPU load normal. Proceeding with system updates..."
yum -y update
```

📘 **Use Case:** Automate safe update scheduling when the system is idle.

---

## 🧠 **Summary Table for Admins**

| Loop      | Use Case Example                    | Real-World Usage                          |
| --------- | ----------------------------------- | ----------------------------------------- |
| **for**   | Iterate over services, files, users | Batch operations like restarts or backups |
| **while** | Monitor conditions                  | Continuous monitoring or polling          |
| **until** | Wait for event                      | Dependency handling and delayed actions   |

---

Would you like me to create a **combined practice script** (e.g., a single shell script that uses all three loops for monitoring + automation) for your **notes or video demo** next?



