✅ **Conditional Statements: `if`, `else`, `elif` in Shell Scripting**

Conditional statements allow your script to **make decisions** based on certain conditions.

---

### 🧩 **1. Basic `if` Statement**

#### **Syntax:**

```bash
if [ condition ]; then
    # commands to execute if condition is true
fi
```

#### **Example:**

```bash
age=18
if [ $age -ge 18 ]; then
    echo "You are an adult."
fi
```

**Output:**

```
You are an adult.
```

---

### 🧠 **2. `if-else` Statement**

#### **Syntax:**

```bash
if [ condition ]; then
    # commands if true
else
    # commands if false
fi
```

#### **Example:**

```bash
age=16
if [ $age -ge 18 ]; then
    echo "You are an adult."
else
    echo "You are a minor."
fi
```

**Output:**

```
You are a minor.
```

---

### 🧩 **3. `if-elif-else` Statement**

#### **Syntax:**

```bash
if [ condition1 ]; then
    # commands if condition1 is true
elif [ condition2 ]; then
    # commands if condition2 is true
else
    # commands if all conditions are false
fi
```

#### **Example:**

```bash
score=75

if [ $score -ge 90 ]; then
    echo "Grade: A"
elif [ $score -ge 75 ]; then
    echo "Grade: B"
else
    echo "Grade: C"
fi
```

**Output:**

```
Grade: B
```

---

### ⚙️ **4. Notes on Conditions**

1. Always put **spaces around `[ ]`**
   ❌ Wrong: `[ $age-gt 18 ]`
   ✅ Correct: `[ $age -gt 18 ]`
2. Use `-eq, -ne, -lt, -le, -gt, -ge` for **numbers**
3. Use `=` or `!=` for **strings**
4. Combine conditions using `-a` (AND) or `-o` (OR)

#### **Example: Numeric Comparison**

```bash
num=10
if [ $num -gt 5 -a $num -lt 15 ]; then
    echo "Number is between 5 and 15"
fi
```

#### **Example: String Comparison**

```bash
name="Alex"
if [ "$name" = "Alex" ]; then
    echo "Hello, Alex!"
fi
```

---

### 🧾 **5. Summary Table**

| Statement                       | Purpose                                 | Example                                                                                         |
| ------------------------------- | --------------------------------------- | ----------------------------------------------------------------------------------------------- |
| `if [ condition ]; then ... fi` | Execute commands if true                | `if [ $age -ge 18 ]; then echo "Adult"; fi`                                                     |
| `if-else`                       | True → do one thing, False → do another | `if [ $age -ge 18 ]; then echo "Adult"; else echo "Minor"; fi`                                  |
| `if-elif-else`                  | Multiple conditions                     | `if [ $score -ge 90 ]; then echo "A"; elif [ $score -ge 75 ]; then echo "B"; else echo "C"; fi` |

---

Absolutely! Let’s revisit **`if`, `else`, `elif`** with **practical system administration examples**. These are very common in scripts that automate tasks for servers and system monitoring.

---

## **1. Checking if a User Exists**

```bash
#!/bin/bash

read -p "Enter username to check: " user

if id "$user" &>/dev/null; then
    echo "User $user exists."
else
    echo "User $user does not exist."
fi
```

✅ **Use Case:** Automating user management or validating input before creating a new user.

---

## **2. Checking Disk Usage**

```bash
#!/bin/bash

disk_usage=$(df / | tail -1 | awk '{print $5}' | sed 's/%//')

if [ $disk_usage -ge 90 ]; then
    echo "Warning: Disk usage is critically high: $disk_usage%"
elif [ $disk_usage -ge 75 ]; then
    echo "Disk usage is high: $disk_usage%"
else
    echo "Disk usage is normal: $disk_usage%"
fi
```

✅ **Use Case:** Monitor disk usage and trigger alerts automatically.

---

## **3. Checking if a Service is Running**

```bash
#!/bin/bash

service_name="nginx"

if systemctl is-active --quiet $service_name; then
    echo "$service_name is running."
else
    echo "$service_name is not running. Starting service..."
    systemctl start $service_name
fi
```

✅ **Use Case:** Automated service monitoring and recovery.

---

## **4. Backup Script Example**

```bash
#!/bin/bash

backup_dir="/backup"
today=$(date +%F)

if [ -d "$backup_dir" ]; then
    tar -czf "$backup_dir/backup-$today.tar.gz" /etc
    echo "Backup completed successfully."
else
    echo "Backup directory $backup_dir does not exist. Creating now..."
    mkdir -p "$backup_dir"
    tar -czf "$backup_dir/backup-$today.tar.gz" /etc
    echo "Backup completed successfully."
fi
```

✅ **Use Case:** Automated configuration backups with folder existence check.

---

## **5. Checking Network Connectivity**

```bash
#!/bin/bash

host="8.8.8.8"

if ping -c 2 $host &>/dev/null; then
    echo "Network is up."
else
    echo "Network is down. Please check the connection."
fi
```

✅ **Use Case:** Monitoring server connectivity or external services.

---

### 💡 **System Admin Tips**

1. Combine `if` statements with logging to track automated decisions:

```bash
echo "$(date): Service nginx was not running. Started it." >> /var/log/service_check.log
```

2. Use numeric comparisons for resources (CPU %, disk, memory).
3. Always quote variables (`"$var"`) to handle spaces in filenames or paths.

---



