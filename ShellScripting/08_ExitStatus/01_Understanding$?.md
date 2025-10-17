f **`$?` (Exit Status)** — with **SysAdmin-level examples** and real use cases for RHEL/Ubuntu scripting 

---

## 💡 **What is `$?` in Shell Scripting?**

`$?` holds the **exit status** of the **last executed command**.
It tells whether the previous command **succeeded** or **failed**.

---

### 🧠 **Exit Status Codes**

| Exit Code | Meaning                         |
| --------- | ------------------------------- |
| **0**     | Success (command ran correctly) |
| **1–255** | Error (non-zero = failure)      |

---

## 🧩 **Basic Examples**

### Example 1️⃣ – Simple Success Check

```bash
ls /etc
echo $?
```

✅ Output: `0` → means the `ls` command succeeded.

---

### Example 2️⃣ – Failed Command

```bash
ls /no/such/directory
echo $?
```

❌ Output: `2` → means an error occurred (directory not found).

---

### Example 3️⃣ – Using in Conditional Statements

```bash
#!/bin/bash
ping -c1 google.com &>/dev/null
if [ $? -eq 0 ]; then
    echo "Internet connection is OK!"
else
    echo "No Internet connection!"
fi
```

📘 *Use Case:* Check internet connectivity before running updates or automation tasks.

---

## ⚙️ **SysAdmin-Level Examples**

### 🧩 Example 4: Check Service Status Before Restarting

```bash
#!/bin/bash
systemctl is-active --quiet sshd
if [ $? -eq 0 ]; then
    echo "sshd is already running."
else
    echo "sshd is not running. Starting service..."
    systemctl start sshd
    echo "Exit status of start command: $?"
fi
```

📘 *Use Case:* Monitor and restart essential services automatically.

---

### 🧩 Example 5: Backup Script Verification

```bash
#!/bin/bash
tar -czf /backup/etc_$(date +%F).tar.gz /etc
if [ $? -eq 0 ]; then
    echo "Backup created successfully!"
else
    echo "Backup failed! Check disk space or permissions."
fi
```

📘 *Use Case:* Validate critical backup or file copy operations.

---

### 🧩 Example 6: Combining with `&&` and `||`

```bash
#!/bin/bash
cp /etc/hosts /tmp/ && echo "Copy successful" || echo "Copy failed!"
```

📘 *Explanation:*

* `&&` runs next command **only if previous command succeeds** (`$? == 0`)
* `||` runs next command **only if previous command fails** (`$? != 0`)

Equivalent logic:

```bash
cp /etc/hosts /tmp/
if [ $? -eq 0 ]; then
    echo "Copy successful"
else
    echo "Copy failed!"
fi
```

---

### 🧩 Example 7: Logging Exit Status to File

```bash
#!/bin/bash
df -h / >> /var/log/disk_check.log 2>&1
echo "$(date): Exit status of df command = $?" >> /var/log/disk_check.log
```

📘 *Use Case:* Record command success/failure for troubleshooting automation logs.

---

## 🧠 **Pro Tip:**

You can store the status in a variable:

```bash
cmd="systemctl restart httpd"
$cmd
status=$?
echo "Command: $cmd exited with status $status"
```

---

## ✅ **Quick Summary**

| Command  | Meaning                      |   |                              |
| -------- | ---------------------------- | - | ---------------------------- |
| `$?`     | Exit status of last command  |   |                              |
| `0`      | Success                      |   |                              |
| Non-zero | Error/failure                |   |                              |
| `&&`     | Execute next only if success |   |                              |
| `        |                              | ` | Execute next only if failure |

---

