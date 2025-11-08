# 🧾 **Writing Logs to Files in Shell Scripting (RHEL)**

Logging to a file allows system administrators to:
✅ Keep track of script activities
✅ Troubleshoot automation issues
✅ Audit system changes
✅ Review history of cron jobs or scheduled tasks

---

## 🧠 **1. Basic Output Redirection**

You can redirect command output into a log file using:

| Operator | Meaning                         | Example                           |
| -------- | ------------------------------- | --------------------------------- |
| `>`      | Overwrite file                  | `echo "Start" > logfile.txt`      |
| `>>`     | Append to file                  | `echo "Next line" >> logfile.txt` |
| `2>`     | Redirect errors only            | `command 2> errors.log`           |
| `&>`     | Redirect both stdout and stderr | `command &> logfile.txt`          |

---

### ✅ **Example 1 – Basic Logging**

```bash
#!/bin/bash

echo "Script started at $(date)" > /var/log/myscript.log
echo "Performing system update..." >> /var/log/myscript.log
yum update -y &>> /var/log/myscript.log
echo "Update completed successfully at $(date)" >> /var/log/myscript.log
```

📘 *Use Case:* Keep detailed logs of all system updates or patch jobs.

---

### ✅ **Example 2 – Appending Logs**

```bash
#!/bin/bash

LOGFILE="/var/log/cleanup.log"

echo "----------------------------------" >> "$LOGFILE"
echo "Cleanup run at $(date)" >> "$LOGFILE"
find /tmp -type f -mtime +7 -delete >> "$LOGFILE" 2>&1
echo "Cleanup completed." >> "$LOGFILE"
```

📘 *Use Case:* Automate temp file cleanup and maintain execution history.

---

## ⚙️ **2. Logging Both Output and Errors**

You can log **both standard output (stdout)** and **errors (stderr)** together for complete visibility.

```bash
#!/bin/bash

LOGFILE="/var/log/disk_check.log"

{
    echo "===== Disk Usage Report $(date) ====="
    df -h
    echo
    echo "===== End of Report ====="
} &>> "$LOGFILE"
```

📘 *Use Case:* Scheduled monitoring scripts for disk usage, CPU, or memory.

---

### ✅ **Example 3 – Logging Errors Only**

```bash
#!/bin/bash

LOGFILE="/var/log/error_test.log"

echo "Running command that may fail..."
cp /nonexistent/file /tmp/ 2>> "$LOGFILE"
echo "Error logged to $LOGFILE"
```

📘 *Use Case:* Capture only error messages while suppressing normal output.

---

## ⚙️ **3. Using Variables for Dynamic Log Files**

### ✅ **Example 4 – Daily Log Rotation**

```bash
#!/bin/bash

LOGFILE="/var/log/backup_$(date +%F).log"

echo "Backup started at $(date)" > "$LOGFILE"
tar -czf /backup/etc_$(date +%F).tar.gz /etc &>> "$LOGFILE"
echo "Backup completed successfully at $(date)" >> "$LOGFILE"
```

📘 *Use Case:* Create a **new log per day** for backups or scheduled jobs.

---

## 🧰 **4. SysAdmin-Level Examples**

---

### 🧩 **Example 5 – Service Monitoring Script**

```bash
#!/bin/bash
LOGFILE="/var/log/service_monitor.log"

services=("sshd" "crond" "firewalld")

echo "===== Service Check Started: $(date) =====" >> "$LOGFILE"

for svc in "${services[@]}"
do
    systemctl is-active --quiet "$svc"
    if [ $? -eq 0 ]; then
        echo "$(date): $svc is running." >> "$LOGFILE"
    else
        echo "$(date): WARNING - $svc is NOT running!" >> "$LOGFILE"
    fi
done

echo "===== Service Check Completed =====" >> "$LOGFILE"
```

📘 *Use Case:* Run via `cron` to log service health automatically.

---

### 🧩 **Example 6 – Log Script Execution with PID**

```bash
#!/bin/bash
LOGFILE="/var/log/script_activity.log"
PID=$$

echo "$(date): Script started (PID: $PID)" >> "$LOGFILE"
sleep 3
echo "$(date): Script completed successfully." >> "$LOGFILE"
```

📘 *Use Case:* Track script runs by process ID — useful in debugging cron or parallel scripts.

---

### 🧩 **Example 7 – Network Test Logger**

```bash
#!/bin/bash
LOGFILE="/var/log/network_test.log"
hosts=("8.8.8.8" "google.com" "localhost")

echo "Network check started at $(date)" >> "$LOGFILE"

for h in "${hosts[@]}"
do
    if ping -c1 "$h" &>/dev/null; then
        echo "$(date): $h is reachable" >> "$LOGFILE"
    else
        echo "$(date): $h is unreachable" >> "$LOGFILE"
    fi
done

echo "Network check completed." >> "$LOGFILE"
```

📘 *Use Case:* Log connectivity status daily for remote or critical servers.

---

## ⚙️ **5. Using `exec` to Redirect All Script Output**

You can redirect **all output** (including errors) from a script to a file automatically.

```bash
#!/bin/bash
LOGFILE="/var/log/full_output.log"

exec &> >(tee -a "$LOGFILE")

echo "Script started at $(date)"
ls /etc
ls /nonexistentfile
echo "Script completed."
```

✅ **Explanation:**

* `exec` redirects **stdout and stderr** for the entire script.
* `tee` allows output to show on screen *and* be saved to a file.
* All messages will be logged to `/var/log/full_output.log`.

📘 *Use Case:* Useful for **production scripts** where every command should be recorded.

---

## 🧾 **6. Quick Summary**

| Task                       | Command / Example                    |
| -------------------------- | ------------------------------------ |
| Overwrite log file         | `>`                                  |
| Append to log              | `>>`                                 |
| Redirect errors only       | `2>> logfile`                        |
| Redirect all output        | `&>> logfile`                        |
| Log everything from script | `exec &> >(tee -a logfile)`          |
| Include timestamps         | `echo "$(date): message" >> logfile` |

---

## 💡 **SysAdmin Best Practices**

1. Always store logs in `/var/log/` with **unique names**.
2. Use timestamps in each log entry for easier troubleshooting.
3. Rotate logs periodically (e.g., using `logrotate` or date-based naming).
4. Use `tee` for dual logging (screen + file).
5. Log both output and errors for full visibility.
6. Combine `logger` + file logs for complete traceability.

---


