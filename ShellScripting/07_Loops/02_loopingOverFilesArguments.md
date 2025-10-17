
🔁 *Looping over files, arguments, and numbers* — with **real-world SysAdmin examples** for **RHEL/Ubuntu shell scripting**.

---

## 🌀 **1. Looping Over Files**

### 📘 **Syntax**

```bash
for file in /path/to/directory/*
do
    commands
done
```

### 💻 **Examples**

#### 🧩 Example 1: List All `.log` Files

```bash
#!/bin/bash
for file in /var/log/*.log
do
    echo "Log file: $file"
done
```

📘 *Use Case:* Useful to verify or collect logs for review.

---

#### 🧩 Example 2: Compress All `.log` Files

```bash
#!/bin/bash
for file in /var/log/*.log
do
    gzip "$file"
    echo "Compressed: $file"
done
```

📘 *Use Case:* Automate log rotation or archiving.

---

#### 🧩 Example 3: Check File Permissions

```bash
#!/bin/bash
for file in /etc/*.conf
do
    ls -l "$file"
done
```

📘 *Use Case:* Check all configuration file permissions quickly.

---

## 💡 **2. Looping Over Script Arguments**

### 📘 **Syntax**

```bash
for arg in "$@"
do
    commands
done
```

### 💻 **Examples**

#### 🧩 Example 1: Print All Arguments

```bash
#!/bin/bash
echo "You passed $# arguments"
for arg in "$@"
do
    echo "Argument: $arg"
done
```

Run:

```bash
./myscript.sh file1.txt file2.txt dir1
```

Output:

```
You passed 3 arguments
Argument: file1.txt
Argument: file2.txt
Argument: dir1
```

📘 *Use Case:* Validate or process files passed to your script.

---

#### 🧩 Example 2 (SysAdmin): Check Status of Given Services

```bash
#!/bin/bash
for svc in "$@"
do
    systemctl is-active --quiet "$svc" && echo "$svc is running" || echo "$svc is stopped"
done
```

Run:

```bash
./check_services.sh sshd crond nginx
```

📘 *Use Case:* Quickly check multiple services passed as script arguments.

---

## 🔢 **3. Looping Over Numbers**

### 📘 **Syntax**

```bash
for i in {start..end}
do
    commands
done
```

or (C-style loop):

```bash
for ((i=start; i<=end; i++))
do
    commands
done
```

### 💻 **Examples**

#### 🧩 Example 1: Print Numbers 1 to 5

```bash
for i in {1..5}
do
    echo "Number: $i"
done
```

---

#### 🧩 Example 2: Create Multiple Backup Directories

```bash
#!/bin/bash
for i in {1..3}
do
    mkdir -p "/backup/day$i"
    echo "Created /backup/day$i"
done
```

📘 *Use Case:* Automate directory creation (daily/weekly backups).

---

#### 🧩 Example 3: Ping a Range of Hosts

```bash
#!/bin/bash
for i in {1..5}
do
    ping -c1 192.168.1.$i &>/dev/null && echo "Host $i is up" || echo "Host $i is down"
done
```

📘 *Use Case:* Network admins can use it for quick subnet availability checks.

---

## 🧠 **Quick Summary Table**

| Loop Type     | Iteration Target    | Example Use Case                    |
| ------------- | ------------------- | ----------------------------------- |
| **Files**     | `/path/*.ext`       | Scan or process multiple files      |
| **Arguments** | `$@`                | Handle input parameters dynamically |
| **Numbers**   | `{1..N}` or `(( ))` | Repeat a task multiple times        |

---

