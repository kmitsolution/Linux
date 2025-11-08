
# 🧠 **Using `logger` in Shell Scripting (RHEL)**

### 🧩 **What is `logger`?**

`logger` is a **command-line utility** that allows your scripts to **send log messages directly to the system log** (`/var/log/messages` on RHEL or `/var/log/syslog` on Ubuntu).

It uses the **syslog service**, which handles all system logging.

---

## ⚙️ **1. Basic Syntax**

```bash
logger "Your message here"
```

✅ This sends a log message to the system log (by default, `/var/log/messages`).

---

### **Example 1 – Simple Log Entry**

```bash
#!/bin/bash
logger "System update started by user $(whoami)"
sleep 2
logger "System update completed successfully"
```

📘 *Check Logs:*
After running, view the entries:

```bash
tail -n 5 /var/log/messages
```

✅ **Sample Output:**

```
Nov 8 12:10:23 server1 bash[4123]: System update started by user root
Nov 8 12:10:25 server1 bash[4123]: System update completed successfully
```

---

## 🧰 **2. Adding a Tag or Script Name**

You can specify a tag (identifier) so log entries are easy to recognize.

```bash
logger -t BACKUP_SCRIPT "Backup process started"
logger -t BACKUP_SCRIPT "Backup completed successfully"
```

✅ **Output:**

```
Nov 8 13:05:12 server1 BACKUP_SCRIPT: Backup process started
Nov 8 13:05:15 server1 BACKUP_SCRIPT: Backup completed successfully
```

📘 *Use Case:* Identify logs from specific scripts easily.

---

## ⚙️ **3. Logging with Priority Levels**

Syslog supports **logging levels** like `info`, `warning`, `err`, etc.

```bash
logger -p user.info "System check started"
logger -p user.err "Disk usage exceeded 90%!"
```

✅ **Output Example:**

```
Nov 8 13:20:05 server1 user: System check started
Nov 8 13:21:10 server1 user: Disk usage exceeded 90%!
```

📘 *Use Case:* Differentiate between info, warning, and error logs.

---

## 🧩 **4. Using Variables in Logs**

```bash
#!/bin/bash
user=$(whoami)
hostname=$(hostname)
logger -t USER_CHECK "Script run by $user on $hostname"
```

📘 *Use Case:* Record which user executed automation scripts.

---

## 🧰 **5. Logging Command Results**

```bash
#!/bin/bash
if ping -c1 google.com &>/dev/null; then
    logger -t NETWORK "Internet connection is OK"
else
    logger -p user.err -t NETWORK "Internet connection failed"
fi
```

📘 *Use Case:* Network monitoring and status logging.

---

## ⚙️ **6. Writing Logs to Custom Files**

You can redirect `logger` messages to specific log files by configuring syslog (advanced)
or simply **append messages to your own log file**.

```bash
#!/bin/bash
LOGFILE="/var/log/backup_script.log"
logger -t BACKUP "Backup started"
echo "$(date): Backup completed successfully" >> "$LOGFILE"
```

📘 *Use Case:* Maintain both **system-level** and **local** log history.

---

## 🧩 **7. Logging Errors from Commands**

You can use `logger` in combination with error checks (`$?`) to log failures automatically.

```bash
#!/bin/bash
tar -czf /backup/etc_$(date +%F).tar.gz /etc
if [ $? -eq 0 ]; then
    logger -t BACKUP "Backup completed successfully"
else
    logger -p user.err -t BACKUP "Backup failed!"
fi
```

📘 *Use Case:* Automate backup success/failure reporting to `/var/log/messages`.

---

## 🧰 **8. SysAdmin-Level Example – Automated System Health Logger**

```bash
#!/bin/bash
THRESHOLD=80
logger -t HEALTHCHECK "System health check started"

disk_usage=$(df / | awk 'NR==2 {print $5}' | sed 's/%//')
if [ "$disk_usage" -ge "$THRESHOLD" ]; then
    logger -p user.warn -t HEALTHCHECK "Disk usage high: ${disk_usage}%"
else
    logger -t HEALTHCHECK "Disk usage normal: ${disk_usage}%"
fi

mem_usage=$(free | awk '/Mem/{printf("%.0f"), $3/$2 * 100}')
if [ "$mem_usage" -ge "$THRESHOLD" ]; then
    logger -p user.err -t HEALTHCHECK "Memory usage high: ${mem_usage}%"
else
    logger -t HEALTHCHECK "Memory usage normal: ${mem_usage}%"
fi

logger -t HEALTHCHECK "System health check completed"
```

📘 *Use Case:*
Create a **daily cron job** to record system health logs for troubleshooting.

---

## 🧩 **9. Viewing Logger Output**

System logs are typically stored at:

* **RHEL / CentOS / Fedora:** `/var/log/messages`
* **Ubuntu / Debian:** `/var/log/syslog`

View with:

```bash
tail -f /var/log/messages | grep BACKUP
```

or search by tag:

```bash
grep HEALTHCHECK /var/log/messages
```

---

## 🧾 **10. Quick Summary**

| Option              | Meaning                      | Example                               |
| ------------------- | ---------------------------- | ------------------------------------- |
| `logger "message"`  | Log to syslog                | `logger "System update done"`         |
| `-t TAG`            | Add tag (script name)        | `logger -t BACKUP "Starting backup"`  |
| `-p FACILITY.LEVEL` | Set priority                 | `logger -p user.err "Error occurred"` |
| `-f file`           | Log from file                | `logger -f /tmp/logfile`              |
| `$?` check          | Log based on success/failure | `logger -p user.err "Command failed"` |

---

## 💡 **SysAdmin Best Practices**

1. Use a **unique tag** (`-t`) for each script to easily filter logs.
2. Combine `logger` with `$?` to capture success/failure automatically.
3. Review `/var/log/messages` or `/var/log/syslog` regularly.
4. For production systems, use `logger -p user.warn` or `user.err` to prioritize alerts.
5. Add `logger` to all critical automation scripts for traceability.

---


