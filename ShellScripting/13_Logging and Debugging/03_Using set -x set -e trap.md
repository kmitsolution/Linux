
# 🧠 **Debugging and Error Handling in Shell Scripting (RHEL)**

These tools help SysAdmins make scripts **reliable, safe, and easier to troubleshoot** — especially in production environments.

---

## ⚙️ **1. Using `set -x` – Debug Mode**

### 🧩 **What it does:**

`set -x` prints **each command before it’s executed**, along with its arguments.
It’s very useful for **debugging** complex scripts.

---

### ✅ **Example 1 – Basic Debugging**

```bash
#!/bin/bash
set -x

echo "Starting script..."
mkdir /tmp/testdir
cp /etc/hosts /tmp/testdir/
echo "Script completed."

set +x
```

📘 **Explanation:**

* `set -x` enables debug tracing.
* Each command will be displayed as it runs.
* `set +x` disables debug mode.

✅ **Sample Output:**

```
+ echo 'Starting script...'
Starting script...
+ mkdir /tmp/testdir
+ cp /etc/hosts /tmp/testdir/
+ echo 'Script completed.'
Script completed.
```

📘 *Use Case:* Debug file operations, variables, or loops in scripts.

---

### ✅ **Example 2 – Partial Debugging**

You can enable debug mode only for a section:

```bash
echo "Start script"
set -x
cp /etc/passwd /tmp/
ls -l /tmp/passwd
set +x
echo "End script"
```

📘 *Use Case:* Focus on a specific block of commands during debugging.

---

## ⚙️ **2. Using `set -e` – Exit on Error**

### 🧩 **What it does:**

`set -e` tells the shell to **exit immediately if any command fails** (returns non-zero exit code).
This ensures scripts **stop safely** instead of continuing with potential issues.

---

### ✅ **Example 3 – Exit on Failure**

```bash
#!/bin/bash
set -e

echo "Starting script..."
cp /nonexistent/file /tmp/
echo "This line will NOT execute if above fails"
```

📘 **Output:**

```
Starting script...
cp: cannot stat '/nonexistent/file': No such file or directory
```

✅ **Script exits immediately** after the failed `cp`.

---

### ✅ **Example 4 – Using with Backups**

```bash
#!/bin/bash
set -e
echo "Starting backup..."
tar -czf /backup/etc_$(date +%F).tar.gz /etc
cp /backup/etc_$(date +%F).tar.gz /mnt/backup/
echo "Backup completed successfully!"
```

📘 *Use Case:* Stop the backup process immediately if any step fails — prevents corrupted backups.

---

### ⚠️ **Important Tip:**

If you need to **ignore specific failures**, use:

```bash
command || true
```

Example:

```bash
rm /tmp/nonexistentfile || true
```

---

## ⚙️ **3. Using `trap` – Handle Signals & Cleanup**

### 🧩 **What it does:**

`trap` lets you define a **custom action** when your script receives signals or errors.

Typical uses:

* Clean temporary files
* Log errors
* Stop services safely
* Handle `Ctrl+C` interruptions

---

### ✅ **Example 5 – Simple Trap Example**

```bash
#!/bin/bash

trap 'echo "Script interrupted! Exiting..."; exit 1' INT

echo "Running script. Press Ctrl+C to stop."
sleep 10
echo "Completed successfully!"
```

📘 *Use Case:* Handle user interruption (`Ctrl+C`) gracefully.

---

### ✅ **Example 6 – Cleanup Temporary Files**

```bash
#!/bin/bash

temp_file="/tmp/tempfile_$$"

trap 'echo "Cleaning up..."; rm -f "$temp_file"; exit' EXIT

echo "Creating temp file: $temp_file"
touch "$temp_file"
echo "Working..."
sleep 5
echo "Done!"
```

📘 *Use Case:* Always remove temporary files, even if the script exits early.

---

### ✅ **Example 7 – Logging Errors on Failure**

```bash
#!/bin/bash

LOGFILE="/var/log/script_error.log"

trap 'echo "$(date): Script failed at line $LINENO" >> $LOGFILE' ERR
set -e

echo "Starting script..."
cp /nonexistent/file /tmp/
echo "This will not execute."
```

📘 *Use Case:* Automatically record the **line number and timestamp** of any failure.

---

### ✅ **Example 8 – Combine `trap`, `set -e`, and Logging**

```bash
#!/bin/bash
set -e
LOGFILE="/var/log/deploy.log"
trap 'echo "$(date): Error at line $LINENO during deployment" >> $LOGFILE' ERR

echo "$(date): Deployment started" >> "$LOGFILE"

systemctl stop nginx
cp -r /tmp/new_site/* /var/www/html/
systemctl start nginx

echo "$(date): Deployment completed successfully" >> "$LOGFILE"
```

📘 *Use Case:*
A **production-grade deployment script** that logs both success and failure details — ideal for CI/CD or sysadmin maintenance.

---

### ✅ **Example 9 – Trap on Script Exit**

```bash
#!/bin/bash

trap 'echo "Script finished at $(date)" >> /var/log/script_status.log' EXIT

echo "Running script..."
sleep 2
echo "Work done."
```

📘 *Use Case:* Track script completion in automation logs.

---

## 🧩 **Common Trap Signals**

| Signal | Name      | Description             |
| ------ | --------- | ----------------------- |
| `INT`  | Interrupt | Sent by `Ctrl+C`        |
| `TERM` | Terminate | Kill signal             |
| `EXIT` | Exit      | Runs on script exit     |
| `ERR`  | Error     | Runs on command failure |
| `HUP`  | Hangup    | Terminal disconnect     |
| `QUIT` | Quit      | Stop and dump core      |

---

## 🧾 **Quick Summary Table**

| Feature        | Purpose                   | Example                   |
| -------------- | ------------------------- | ------------------------- |
| `set -x`       | Debug — show each command | `set -x; command; set +x` |
| `set -e`       | Exit on first error       | `set -e`                  |
| `trap`         | Handle cleanup or errors  | `trap 'cleanup' EXIT`     |
| `trap ... ERR` | Log on error              | `trap 'echo "Error"' ERR` |
| `trap ... INT` | Handle Ctrl+C             | `trap 'exit 1' INT`       |

---

## 💡 **SysAdmin Best Practices**

1. **Always enable `set -e`** in production scripts to prevent silent failures.
2. Use **`trap ERR`** to capture and log unexpected issues.
3. Include **`$LINENO`** and **timestamps** in error logs.
4. Use **`set -x`** only during debugging — disable it in production runs.
5. Combine `trap` + `logger` + file logs for complete audit trails.
6. Test your trap handlers thoroughly before deploying automation.

---


