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

Excellent question ✅ — this is a **core Bash concept** that every RHEL/Linux admin or scripter should know clearly.

Let’s go through it **step-by-step**, with **when to use `[[ ... ]]` vs `(( ... ))`** inside an `if` condition.

---

## 🧩 1. `[[ ... ]]` — **for string and integer comparisons**

This is a **test command** (an improved version of `[ ... ]`).

### ✅ You use `[[ ... ]]` when:

* You’re comparing **strings**
* You’re comparing **integers** using `-eq`, `-lt`, `-gt`, etc.
* You’re checking **file conditions** (`-f`, `-d`, `-e`, etc.)

---

### 🧠 Examples with `[[ ... ]]`

#### ➤ String comparisons

```bash
name="RHEL"

if [[ $name == "RHEL" ]]; then
  echo "Name matches RHEL"
fi
```

#### ➤ Integer comparisons

```bash
a=5
b=10

if [[ $a -lt $b ]]; then
  echo "$a is less than $b"
fi
```

#### ➤ File test

```bash
if [[ -f /etc/passwd ]]; then
  echo "File exists"
fi
```

---

### 🧩 2. `(( ... ))` — **for arithmetic (numeric) evaluation**

This is the **arithmetic evaluation** syntax — it works with **integers only**.

### ✅ You use `(( ... ))` when:

* You’re doing **numeric** comparisons or calculations
* You want **C-like syntax** (e.g., `a < b`, `a++`, etc.)
* You don’t want to quote or use `$` before variables inside

---

### 🧠 Examples with `(( ... ))`

#### ➤ Numeric comparison

```bash
a=10
b=5

if (( a > b )); then
  echo "$a is greater than $b"
fi
```

#### ➤ Arithmetic operation in loop

```bash
for ((i=1; i<=5; i++)); do
  echo "Count: $i"
done
```

#### ➤ Incrementing variables

```bash
x=1
((x++))
echo $x   # Output: 2
```

---

## ⚖️ 3. Summary Table — When to Use Each

| Feature / Use case                                | `[[ ... ]]` | `(( ... ))`                 |       |       |
| ------------------------------------------------- | ----------- | --------------------------- | ----- | ----- |
| **String comparison** (`==`, `!=`, pattern match) | ✅ Yes       | ❌ No                        |       |       |
| **Integer comparison** (`-lt`, `-gt`, etc.)       | ✅ Yes       | ✅ Yes (with normal symbols) |       |       |
| **Arithmetic math** (`+`, `-`, `*`, `/`)          | ❌ No        | ✅ Yes                       |       |       |
| **File checks** (`-f`, `-d`, `-e`)                | ✅ Yes       | ❌ No                        |       |       |
| **Logical operators** (`&&`, `                    |             | `)                          | ✅ Yes | ✅ Yes |
| **Float comparisons**                             | ❌ No        | ❌ No (use `bc`)             |       |       |

---

## 💡 4. Example Mixing Both Correctly

```bash
a=5
b=10
name="RHEL"

if (( a < b )); then
  echo "$a is less than $b"
fi

if [[ $name == "RHEL" ]]; then
  echo "Running on RHEL"
fi
```

---

## ⚠️ 5. Common Mistakes

🚫 **Wrong:**

```bash
if [[ $a < $b ]]; then   # "<" in [[ ]] does STRING comparison, not numeric
  echo "This may be wrong for numbers"
fi
```

✅ **Correct:**

```bash
if (( a < b )); then
  echo "Numeric comparison done correctly"
fi
```

---


Excellent question ✅ — this shows you’re thinking **deeply about how Bash `if` conditions actually work**.

Let’s break it down clearly 👇

---

## 🧠 The command:

```bash
if systemctl is-active --quiet $service_name; then
    echo "$service_name is running."
else
    echo "$service_name is not running. Starting service..."
    systemctl start $service_name
fi
```

---

## 🔍 Why are there **no `[ ]` or `[[ ]]` brackets here?

Because:

> In Bash, the `if` statement doesn’t require `[ ]` if you’re testing the **exit status of a command** directly.

---

### 🧩 1️⃣ How `if` really works in Bash

The syntax:

```bash
if COMMAND; then
   ...
fi
```

means:

> Run **COMMAND**, and if its **exit status is 0 (success)**, execute the “then” block; otherwise, execute the “else” block.

So you can put **any command** there — not just tests.

---

### 🧩 2️⃣ What `systemctl is-active --quiet` does

This command checks whether a systemd service is **active (running)**.

* If the service **is running**, it exits with **status 0** (success).
* If the service **is not running**, it exits with **non-zero** (failure).

You can see this yourself:

```bash
systemctl is-active sshd
echo $?
```

If `sshd` is active, `$?` (exit code) will be `0`.
If it’s not, `$?` will be something else (like `3`).

---

### 🧩 3️⃣ How the `if` statement interprets it

Bash uses the **exit code** of the command:

* **0** → true → run the `then` block
* **non-zero** → false → run the `else` block

So:

```bash
if systemctl is-active --quiet $service_name
```

is equivalent to:

```bash
systemctl is-active --quiet $service_name
if [[ $? -eq 0 ]]; then
```

---

### ✅ 4️⃣ Why `[ ]` or `[[ ]]` are **not needed**

Because `[ ... ]` and `[[ ... ]]` are **test commands** themselves — they’re only needed when you want to **evaluate conditions manually**, like:

```bash
if [[ $var == "value" ]]
```

But here, `systemctl` is already a **real command** whose success/failure status tells you what you need — no extra test brackets required.

---

### 🧰 5️⃣ Full Logic Summary

| Step                                        | Command                     | What it does                  | Exit Code |
| ------------------------------------------- | --------------------------- | ----------------------------- | --------- |
| `systemctl is-active --quiet $service_name` | Checks if service is active | 0 if running, non-zero if not |           |
| `if` evaluates exit code                    | True if 0                   | Runs “then” block             |           |
| Otherwise                                   | False                       | Runs “else” block             |           |

---

### 🧠 Example in Action

```bash
service_name=sshd

if systemctl is-active --quiet $service_name; then
  echo "$service_name is running."
else
  echo "$service_name is not running. Starting service..."
  systemctl start $service_name
fi
```

**Output when running:**

```
sshd is running.
```

**Output when stopped:**

```
sshd is not running. Starting service...
```

---

### 🧩 Bonus Tip:

This style works with any command that gives a meaningful exit code — for example:

```bash
if ping -c1 google.com &>/dev/null; then
  echo "Internet is up"
else
  echo "Internet is down"
fi
```

✅ No `[ ]` needed because the command itself returns success/failure.

---

# Examples

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



