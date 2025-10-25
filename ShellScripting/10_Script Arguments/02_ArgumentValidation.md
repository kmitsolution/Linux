
---

## 🧠 **Argument Validation & Usage Messages in Shell Scripting**

When building automation or administrative scripts, it’s important to ensure the user **passes the correct number of arguments** and **in the right format**.
If not, your script should display a **clear usage message** explaining how to use it.

This makes your scripts:
✅ User-friendly
✅ Safer (prevents wrong commands)
✅ Easier to debug

---

### ⚙️ **Basic Syntax**

```bash
if [ $# -ne <expected_number> ]; then
    echo "Usage: $0 <arg1> <arg2> ..."
    exit 1
fi
```

* `$#` → Number of arguments passed
* `$0` → Script name (helps in dynamic usage messages)

---

## 🧩 **Example 1 – Simple Validation**

```bash
#!/bin/bash

if [ $# -ne 2 ]; then
    echo "Usage: $0 <source_dir> <destination_dir>"
    exit 1
fi

echo "Copying from $1 to $2 ..."
cp -r "$1" "$2"
```

✅ **Run Example:**

```bash
./copy_script.sh /etc /backup/etc_copy
```

❌ **If run incorrectly:**

```bash
./copy_script.sh
```

Output:

```
Usage: ./copy_script.sh <source_dir> <destination_dir>
```

---

## 🧰 **SysAdmin Example 1 – Restarting a Service**

```bash
#!/bin/bash

if [ $# -ne 1 ]; then
    echo "Usage: $0 <service_name>"
    echo "Example: $0 sshd"
    exit 1
fi

service=$1
echo "Restarting $service service..."
systemctl restart "$service" && echo "$service restarted successfully!"
```

✅ **Why it’s useful:**
Prevents accidental restarts when no argument is given. Keeps automation clean and controlled.

---

## 🧰 **SysAdmin Example 2 – User Account Validation**

```bash
#!/bin/bash

if [ $# -lt 1 ]; then
    echo "Usage: $0 <username1> [username2] ..."
    echo "Example: $0 root devops testuser"
    exit 1
fi

for user in "$@"; do
    if id "$user" &>/dev/null; then
        echo "✅ User $user exists."
    else
        echo "❌ User $user does not exist!"
    fi
done
```

✅ **Sysadmin Use Case:**
You can verify multiple accounts at once — great for daily audits.

---

## 🧰 **SysAdmin Example 3 – Checking a File Before Reading**

```bash
#!/bin/bash

file=$1

if [ $# -ne 1 ]; then
    echo "Usage: $0 <file_path>"
    exit 1
fi

if [ ! -f "$file" ]; then
    echo "Error: File '$file' does not exist!"
    exit 2
fi

echo "Displaying content of $file..."
cat "$file"
```

✅ **Why it’s professional:**
It validates both the **argument count** and **file existence** — preventing runtime errors.

---

## 🧰 **SysAdmin Example 4 – Dynamic Backup Script**

```bash
#!/bin/bash

if [ $# -lt 2 ]; then
    echo "Usage: $0 <source_dir> <destination_dir> [compression_type]"
    echo "Example: $0 /etc /backup gzip"
    exit 1
fi

src=$1
dest=$2
compression=${3:-gzip}   # Default to gzip if not specified

tar_cmd="tar -czf"
[ "$compression" == "bzip2" ] && tar_cmd="tar -cjf"

mkdir -p "$dest"
$tar_cmd "$dest/backup_$(date +%F).tar.gz" "$src"
echo "Backup completed successfully with $compression compression."
```

✅ **Sysadmin Use Case:**
Flexible backup script that works even if the user omits the compression type.

---

## 🧩 **Example 5 – Using a Help Flag (-h or --help)**

```bash
#!/bin/bash

show_usage() {
    echo "Usage: $0 -f <filename> -w <word>"
    echo "Example: $0 -f /var/log/secure -w failed"
}

while getopts "f:w:h" opt; do
    case $opt in
        f) file=$OPTARG ;;
        w) word=$OPTARG ;;
        h) show_usage; exit 0 ;;
        *) show_usage; exit 1 ;;
    esac
done

if [ -z "$file" ] || [ -z "$word" ]; then
    show_usage
    exit 1
fi

grep -i "$word" "$file"
```

✅ **Why it’s sysadmin-level:**
This uses **getopts** for flexible flag-based arguments — commonly used in production scripts.

---

### 💡 **Best Practices**

1. **Always validate input** before using it.
2. **Exit with non-zero status** on invalid input (`exit 1`).
3. Use `$0` in usage messages for script reusability.
4. Consider adding a `--help` or `-h` flag.
5. Group related checks in functions for better readability.

---


