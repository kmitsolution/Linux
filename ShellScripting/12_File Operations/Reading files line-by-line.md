
# 🗂️ **File Operations in Shell Scripting (RHEL Focus)**

As a Linux administrator, you’ll frequently work with files: reading configuration files, checking logs, validating backups, and monitoring system status.
In shell scripting, file operations help automate these repetitive tasks efficiently.

---

## 🧠 **1. File Test Operators**

Bash provides special **file test operators** that let you check for **existence, permissions, and type** of a file or directory.

| Operator | Description    | Example                    |
| -------- | -------------- | -------------------------- |
| `-e`     | File exists    | `[ -e /etc/passwd ]`       |
| `-f`     | Regular file   | `[ -f /etc/passwd ]`       |
| `-d`     | Directory      | `[ -d /home ]`             |
| `-r`     | Readable       | `[ -r /etc/passwd ]`       |
| `-w`     | Writable       | `[ -w /tmp/test.txt ]`     |
| `-x`     | Executable     | `[ -x /usr/bin/bash ]`     |
| `-s`     | File not empty | `[ -s /var/log/messages ]` |

---

## 🧩 **Example 1 – Check if a File Exists**

```bash
#!/bin/bash

file="/etc/passwd"

if [ -e "$file" ]; then
    echo "✅ File $file exists."
else
    echo "❌ File $file not found!"
fi
```

📘 *Use Case:* Validate critical configuration files before modifying them.

---

## 🧩 **Example 2 – Check Directory Before Backup**

```bash
#!/bin/bash

backup_dir="/backup"

if [ ! -d "$backup_dir" ]; then
    echo "Backup directory missing. Creating now..."
    mkdir -p "$backup_dir"
fi

echo "Backup directory ready: $backup_dir"
```

📘 *Use Case:* Ensure backup paths exist before copying files.

---

## 🧩 **Example 3 – Check Read/Write Permissions**

```bash
#!/bin/bash

file="/var/log/secure"

if [ -r "$file" ]; then
    echo "File is readable."
else
    echo "File is NOT readable."
fi

if [ -w "$file" ]; then
    echo "File is writable."
else
    echo "File is NOT writable."
fi
```

📘 *Use Case:* Check file accessibility before reading or editing sensitive logs.

---

## 🧠 **2. Reading Files Line by Line**

The `while read` loop is the most efficient way to process files line-by-line.

### **Example 4 – Display File Line by Line**

```bash
#!/bin/bash

file="/etc/passwd"

while read line
do
    echo "$line"
done < "$file"
```

✅ *Output:* Each line of `/etc/passwd` will be printed.

📘 *Use Case:* Parse user data, logs, or config files safely.

---

### **Example 5 – Count Users from /etc/passwd**

```bash
#!/bin/bash

count=0
while read line
do
    ((count++))
done < /etc/passwd

echo "Total users: $count"
```

📘 *Use Case:* Quickly get user count from `/etc/passwd`.

---

### **Example 6 – Extract Specific Field from File**

```bash
#!/bin/bash

echo "List of users with /bin/bash shell:"
while IFS=":" read user pass uid gid desc home shell
do
    if [[ "$shell" == "/bin/bash" ]]; then
        echo "$user"
    fi
done < /etc/passwd
```

📘 *Use Case:* Find all users with login access (`/bin/bash` shell).

---

## 🧰 **3. Automating File Maintenance**

### **Example 7 – Clean Old Log Files**

```bash
#!/bin/bash

log_dir="/var/log"
days=7

echo "Cleaning logs older than $days days in $log_dir..."
find "$log_dir" -type f -mtime +$days -exec rm -f {} \;
echo "Cleanup complete."
```

📘 *Use Case:* Automate log rotation to free disk space.

---

### **Example 8 – Archive Large Files**

```bash
#!/bin/bash

target_dir="/var/log"
backup_dir="/backup/large_files"

mkdir -p "$backup_dir"

find "$target_dir" -type f -size +50M -exec tar -czf "$backup_dir/$(basename {}).tar.gz" {} \;
echo "Large files archived to $backup_dir"
```

📘 *Use Case:* Identify and compress large files automatically.

---

### **Example 9 – Compare Two Files**

```bash
#!/bin/bash

file1="/etc/passwd"
file2="/tmp/passwd_backup"

if cmp -s "$file1" "$file2"; then
    echo "✅ Files are identical."
else
    echo "⚠️ Files differ!"
fi
```

📘 *Use Case:* Verify configuration backups or integrity checks.

---

## 🧠 **4. Using Redirection with File Operations**

| Operator | Purpose           | Example                       |
| -------- | ----------------- | ----------------------------- |
| `>`      | Overwrite file    | `echo "text" > file.txt`      |
| `>>`     | Append to file    | `echo "new line" >> file.txt` |
| `<`      | Input redirection | `command < file.txt`          |

### **Example 10 – Log Command Output**

```bash
#!/bin/bash
df -h > /tmp/disk_report.txt
echo "Disk report saved to /tmp/disk_report.txt"
```

---

### **Example 11 – Append Log with Timestamp**

```bash
#!/bin/bash
echo "$(date): Backup completed successfully" >> /var/log/backup.log
```

📘 *Use Case:* Create simple time-stamped logs for automation scripts.

---

## 🧾 **Summary Table**

| Task                 | Command or Concept                     |
| -------------------- | -------------------------------------- |
| Check if file exists | `[ -e file ]`                          |
| Check directory      | `[ -d dir ]`                           |
| Read line-by-line    | `while read line; do ...; done < file` |
| Delete old files     | `find /path -mtime +N -exec rm {} \;`  |
| Compare files        | `cmp -s file1 file2`                   |
| Write to file        | `echo "data" > file`                   |
| Append to file       | `echo "data" >> file`                  |

---

## 💡 **SysAdmin Best Practices**

1. Always **validate file existence** before reading or modifying.
2. Use **absolute paths** (e.g., `/var/log/secure`) to avoid ambiguity.
3. Log script actions (e.g., `>> /var/log/script.log`).
4. Use `set -e` in production scripts to stop on errors.
5. Schedule file maintenance tasks via `cron` for daily or weekly automation.

---


