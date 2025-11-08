
# 🧠 **File Tests in Shell Scripting (RHEL)**

File test operators in Bash allow you to **check file types, permissions, and properties** before performing operations — ensuring your scripts don’t fail or damage important files.

These are typically used inside **`if` conditions** or **loops**.

---

## ⚙️ **1. Common File Test Operators**

| Operator | Description                           | Example                                   |
| -------- | ------------------------------------- | ----------------------------------------- |
| `-e`     | File exists                           | `[ -e /etc/passwd ]`                      |
| `-f`     | File exists **and** is a regular file | `[ -f /etc/passwd ]`                      |
| `-d`     | File exists **and** is a directory    | `[ -d /home ]`                            |
| `-r`     | File is readable                      | `[ -r /etc/passwd ]`                      |
| `-w`     | File is writable                      | `[ -w /tmp/test.txt ]`                    |
| `-x`     | File is executable                    | `[ -x /usr/bin/bash ]`                    |
| `-s`     | File is not empty                     | `[ -s /var/log/messages ]`                |
| `!`      | Negation (NOT)                        | `[ ! -e /file.txt ]` → file doesn’t exist |

---

## 🧩 **2. Basic Examples**

### ✅ Example 1: Check If File Exists

```bash
#!/bin/bash

file="/etc/passwd"

if [ -e "$file" ]; then
    echo "✅ File $file exists."
else
    echo "❌ File $file not found."
fi
```

📘 *Use Case:* Always check for file existence before reading or editing.

---

### ✅ Example 2: Check If Regular File

```bash
#!/bin/bash

file="/etc/passwd"

if [ -f "$file" ]; then
    echo "$file is a regular file."
else
    echo "$file is not a regular file."
fi
```

📘 *Use Case:* Validate config files before applying changes.

---

### ✅ Example 3: Check If Directory

```bash
#!/bin/bash

dir="/var/log"

if [ -d "$dir" ]; then
    echo "$dir is a directory."
else
    echo "$dir is NOT a directory."
fi
```

📘 *Use Case:* Ensure directories exist before backup or cleanup.

---

### ✅ Example 4: Check Read, Write, Execute Permissions

```bash
#!/bin/bash

file="/var/log/secure"

[ -r "$file" ] && echo "File is readable."
[ -w "$file" ] && echo "File is writable."
[ -x "$file" ] && echo "File is executable."
```

📘 *Use Case:* Verify permissions before attempting file access.

---

### ✅ Example 5: Check If File Is Empty or Not

```bash
#!/bin/bash

log_file="/var/log/secure"

if [ -s "$log_file" ]; then
    echo "$log_file has content."
else
    echo "$log_file is empty."
fi
```

📘 *Use Case:* Detect empty log files or configuration files.

---

## 🧰 **3. SysAdmin-Level Practical Examples**

---

### 🧩 Example 6: Verify and Backup Configuration File

```bash
#!/bin/bash

conf="/etc/ssh/sshd_config"
backup="/backup/ssh_$(date +%F).conf"

if [ -f "$conf" ]; then
    echo "Backing up $conf..."
    cp "$conf" "$backup"
    echo "Backup saved to $backup"
else
    echo "Error: $conf not found!"
fi
```

📘 *Use Case:* Automate daily config file backups safely.

---

### 🧩 Example 7: Directory Check Before Writing Logs

```bash
#!/bin/bash

log_dir="/var/log/custom"

if [ ! -d "$log_dir" ]; then
    echo "Creating log directory: $log_dir"
    mkdir -p "$log_dir"
fi

echo "$(date): System check started." >> "$log_dir/script.log"
```

📘 *Use Case:* Ensure directories exist before creating or appending to logs.

---

### 🧩 Example 8: Check Executable Before Running

```bash
#!/bin/bash

script="/usr/local/bin/cleanup.sh"

if [ -x "$script" ]; then
    echo "Running cleanup script..."
    "$script"
else
    echo "Error: $script not executable!"
fi
```

📘 *Use Case:* Validate script permissions before execution (avoids runtime errors).

---

### 🧩 Example 9: Identify Large, Non-Empty Logs

```bash
#!/bin/bash

for log in /var/log/*.log
do
    if [ -s "$log" ]; then
        echo "✅ $log has data."
    else
        echo "⚠️ $log is empty."
    fi
done
```

📘 *Use Case:* Find active log files vs empty ones.

---

### 🧩 Example 10: Check Backup Directory and Files

```bash
#!/bin/bash

backup_dir="/backup"
file_list=("$backup_dir/etc.tar.gz" "$backup_dir/home.tar.gz")

if [ -d "$backup_dir" ]; then
    echo "Backup directory exists."
    for f in "${file_list[@]}"
    do
        [ -f "$f" ] && echo "✅ $f exists." || echo "❌ Missing file: $f"
    done
else
    echo "Backup directory not found!"
fi
```

📘 *Use Case:* Confirm successful backups and detect missing files.

---

## 🧩 **11. Combining File Tests**

You can combine multiple conditions using:

* `-a` → AND
* `-o` → OR
* `!` → NOT

```bash
#!/bin/bash

file="/etc/passwd"

if [ -f "$file" -a -r "$file" ]; then
    echo "$file exists and is readable."
else
    echo "File either missing or not readable."
fi
```

✅ *Use Case:* Prevent reading inaccessible or non-existent files.

---

## 🧾 **Quick Summary Table**

| Test | Checks           | Typical SysAdmin Usage               |
| ---- | ---------------- | ------------------------------------ |
| `-e` | Existence        | Verify before file operations        |
| `-f` | Regular file     | Validate configuration or text files |
| `-d` | Directory        | Backup or cleanup automation         |
| `-r` | Read permission  | Ensure readable logs/configs         |
| `-w` | Write permission | Ensure writable paths for scripts    |
| `-x` | Executable       | Confirm before running scripts       |
| `-s` | Not empty        | Detect empty logs/configs            |
| `!`  | Negation         | Used in conditional logic            |

---

## 💡 **SysAdmin Best Practices**

1. **Always validate** file/directory existence before using them.
2. Combine tests logically (`-f` + `-r`) for safety.
3. Use **absolute paths** in scripts (`/etc/passwd` not `passwd`).
4. Log missing or unreadable files to `/var/log/script_errors.log`.
5. For scheduled scripts (via `cron`), include error checks for file availability.

---

