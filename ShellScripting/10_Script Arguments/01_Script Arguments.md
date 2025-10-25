
## 🧠 **Understanding Script Arguments in RHEL Shell Scripting**

When you run a shell script, you can **pass arguments from the command line**.
These arguments make your scripts **dynamic** — you don’t need to hard-code values like filenames, usernames, or directories.

---

### ⚙️ **Basic Syntax**

| Variable         | Description                      |
| ---------------- | -------------------------------- |
| `$0`             | Name of the script               |
| `$1`, `$2`, `$3` | First, second, third arguments   |
| `$@`             | All arguments as separate words  |
| `$#`             | Number of arguments              |
| `$*`             | All arguments as a single string |

---

### 🧩 **Example 1 – Basic Argument Demo**

```bash
#!/bin/bash

echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "Total arguments: $#"
```

Run:

```bash
./demo.sh server1 /var/log
```

Output:

```
Script name: ./demo.sh
First argument: server1
Second argument: /var/log
Total arguments: 2
```

---

## ⚙️ **SysAdmin-Level Examples**

### 🧰 **Example 2 – Check Disk Usage for a Given Mount Point**

```bash
#!/bin/bash

mount_point=$1

if [ -z "$mount_point" ]; then
    echo "Usage: $0 <mount_point>"
    exit 1
fi

df -h "$mount_point"
```

✅ **Usage:**

```bash
./disk_check.sh /
```

✅ **SysAdmin Use Case:**
Monitor disk usage on specific mount points dynamically (like `/`, `/home`, or `/data`).

---

### 🧰 **Example 3 – Backup Script with Dynamic Source and Destination**

```bash
#!/bin/bash

src=$1
dest=$2

if [ $# -ne 2 ]; then
    echo "Usage: $0 <source_dir> <destination_dir>"
    exit 1
fi

echo "Backing up $src to $dest..."
rsync -av "$src" "$dest"
echo "Backup completed successfully."
```

✅ **Usage:**

```bash
./backup.sh /etc /backup/etc_backup
```

✅ **SysAdmin Use Case:**
Reuse the same script for different backup locations — flexible and automated.

---

### 🧰 **Example 4 – Restart a Service Dynamically**

```bash
#!/bin/bash

service=$1

if [ -z "$service" ]; then
    echo "Usage: $0 <service_name>"
    exit 1
fi

echo "Restarting $service service..."
systemctl restart "$service"

if [ $? -eq 0 ]; then
    echo "$service restarted successfully."
else
    echo "Failed to restart $service."
fi
```

✅ **Usage:**

```bash
./restart_service.sh sshd
```

✅ **SysAdmin Use Case:**
Quickly restart any service without editing the script.

---

### 🧰 **Example 5 – Loop Through Multiple Arguments**

```bash
#!/bin/bash

if [ $# -eq 0 ]; then
    echo "Usage: $0 <service1> <service2> ..."
    exit 1
fi

for svc in "$@"; do
    echo "Checking status of $svc..."
    systemctl is-active --quiet "$svc"
    if [ $? -eq 0 ]; then
        echo "$svc is running."
    else
        echo "$svc is NOT running!"
    fi
done
```

✅ **Usage:**

```bash
./check_services.sh sshd crond firewalld
```

✅ **SysAdmin Use Case:**
Check multiple service statuses in one go — great for automation and health checks.

---

### 🧰 **Example 6 – Log Search with Arguments**

```bash
#!/bin/bash

log_file=$1
keyword=$2

if [ $# -ne 2 ]; then
    echo "Usage: $0 <log_file> <keyword>"
    exit 1
fi

echo "Searching for '$keyword' in $log_file..."
grep -i "$keyword" "$log_file" || echo "No matches found."
```

✅ **Usage:**

```bash
./log_search.sh /var/log/secure failed
```

✅ **SysAdmin Use Case:**
Quickly search logs for errors, failed login attempts, or specific patterns.

---

### 🧩 **Example 7 – Script Summary with Argument Count**

```bash
#!/bin/bash

echo "Total Arguments Passed: $#"
echo "Arguments List: $@"
```

✅ **Usage:**

```bash
./args_summary.sh one two three
```

✅ **Output:**

```
Total Arguments Passed: 3
Arguments List: one two three
```

---

### ⚙️ **Best Practices for SysAdmins**

1. Always validate arguments before using them (`if [ -z "$1" ]` or `$# -ne expected`).
2. Provide clear **usage messages** to avoid errors.
3. Quote all variables (`"$1"`, `"$@"`) to handle spaces safely.
4. Use `$@` instead of `$*` to handle arguments with spaces properly.
5. Combine arguments with functions for modular and scalable automation.

---


