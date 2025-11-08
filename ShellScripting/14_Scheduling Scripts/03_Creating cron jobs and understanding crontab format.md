 **Creating cron jobs and understanding the `crontab` format**

This is one of the **most used and essential skills** for Linux SysAdmins — especially when automating backups, monitoring, or cleanup scripts in RHEL and Ubuntu.

---

# 🕒 **Creating Cron Jobs and Understanding Crontab Format**

---

## 🧠 **What is a Crontab?**

A **crontab (cron table)** is a configuration file that contains a list of commands or scripts to be run periodically by the **cron daemon**.

Each user (including root) can have their **own crontab**, which defines their scheduled tasks.

---

## ⚙️ **1. Crontab File Location and Management**

| Command                       | Description                        |
| ----------------------------- | ---------------------------------- |
| `crontab -l`                  | List current user’s cron jobs      |
| `crontab -e`                  | Edit current user’s crontab        |
| `crontab -r`                  | Remove current user’s crontab      |
| `sudo crontab -e -u username` | Edit another user’s cron (as root) |
| `cat /etc/crontab`            | View system-wide cron jobs         |

---

## ⚙️ **2. Crontab Syntax (Format)**

Each line in the crontab file follows this **5-field time format**, followed by the **command**:

```
*  *  *  *  *  command_to_execute
│  │  │  │  │
│  │  │  │  └── Day of week (0–7) [Sun=0 or 7]
│  │  │  └───── Month (1–12)
│  │  └───────── Day of month (1–31)
│  └──────────── Hour (0–23)
└─────────────── Minute (0–59)
```

✅ **Example:**

```
30 2 * * * /usr/local/bin/backup.sh
```

This means → **Run at 2:30 AM every day**

---

## 🧩 **3. Time Field Examples**

| Schedule      | Meaning                       |
| ------------- | ----------------------------- |
| `* * * * *`   | Every minute                  |
| `*/5 * * * *` | Every 5 minutes               |
| `0 * * * *`   | Every hour                    |
| `0 0 * * *`   | Every day at midnight         |
| `0 2 * * 1`   | Every Monday at 2 AM          |
| `30 6 1 * *`  | 1st of every month at 6:30 AM |

---

## ⚙️ **4. Example – Editing the User Crontab**

Run:

```bash
crontab -e
```

This opens your user’s cron editor (usually **nano** or **vim**).
You can add job entries like:

```
# Run backup every day at 2 AM
0 2 * * * /home/admin/scripts/backup.sh >> /var/log/backup.log 2>&1

# Clean temp files every Sunday at 3 AM
0 3 * * 0 /home/admin/scripts/cleanup.sh >> /var/log/cleanup.log 2>&1
```

✅ **Pro Tip:**
Always redirect output (`>> logfile 2>&1`) to avoid mail clutter and to keep logs for troubleshooting.

---

## 🧩 **5. Example – System-Wide Crontab Format**

System-wide crontabs (like `/etc/crontab` and `/etc/cron.d/*`) include an **extra “user” field** to specify which user the job runs as.

```
# ┌───────────── minute (0 - 59)
# │ ┌───────────── hour (0 - 23)
# │ │ ┌───────────── day of month (1 - 31)
# │ │ │ ┌───────────── month (1 - 12)
# │ │ │ │ ┌───────────── day of week (0 - 7)
# │ │ │ │ │
# │ │ │ │ │
# * * * * *  user  command
```

✅ **Example (`/etc/crontab`):**

```
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root

# System backup
30 2 * * *  root  /usr/local/bin/system_backup.sh

# Weekly log cleanup
0 3 * * 0  root  /usr/local/bin/log_cleanup.sh
```

📘 *Use Case:*
System-wide jobs are ideal for root-managed maintenance and automation tasks.

---

## ⚙️ **6. Special Cron Keywords**

Instead of using numbers, you can use **shortcuts**:

| Keyword                  | Meaning                      |
| ------------------------ | ---------------------------- |
| `@reboot`                | Run once at startup          |
| `@hourly`                | Every hour                   |
| `@daily`                 | Every day at midnight        |
| `@weekly`                | Every week (Sunday midnight) |
| `@monthly`               | First day of month           |
| `@yearly` or `@annually` | Once a year                  |

✅ **Example:**

```
@reboot /usr/local/bin/startup_check.sh
@daily /usr/local/bin/backup.sh
```

---

## 🧩 **7. Environment Variables in Crontab**

You can define variables like `PATH`, `MAILTO`, or custom variables.

```
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
MAILTO=admin@example.com

0 1 * * * /usr/local/bin/health_check.sh
```

✅ *If mail is configured*, cron will send output to `admin@example.com`.

---

## 🧰 **8. SysAdmin-Level Practical Examples**

---

### 🧩 Example 1 – Disk Usage Monitor

```
*/10 * * * * /usr/local/bin/check_disk.sh >> /var/log/disk_usage.log 2>&1
```

✅ Run every 10 minutes and log disk usage stats.

---

### 🧩 Example 2 – Weekly Database Backup

```
0 1 * * 0 /usr/local/bin/db_backup.sh >> /var/log/db_backup.log 2>&1
```

✅ Every Sunday at 1 AM.

---

### 🧩 Example 3 – Reboot Job

```
@reboot /usr/local/bin/initialize_services.sh >> /var/log/boot.log 2>&1
```

✅ Run after every reboot (useful for restoring services).

---

### 🧩 Example 4 – Environment Check and Logging

```
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
MAILTO=root
0 * * * * /usr/local/bin/sys_health.sh
```

✅ Run health check every hour and email the results to root.

---

## ⚙️ **9. Viewing and Debugging Cron Jobs**

| Task                     | Command                                                |
| ------------------------ | ------------------------------------------------------ |
| List current user’s jobs | `crontab -l`                                           |
| Edit user jobs           | `crontab -e`                                           |
| Remove user jobs         | `crontab -r`                                           |
| Check cron service       | `systemctl status cron` *(Ubuntu)* or `crond` *(RHEL)* |
| View cron logs           | `grep CRON /var/log/syslog` *(Ubuntu)*                 |
| RHEL equivalent          | `grep CROND /var/log/cron`                             |

✅ **Example:**

```bash
grep CRON /var/log/syslog | tail -10
```

---

## 🧾 **10. Combining with Scripts**

Example `backup.sh`:

```bash
#!/bin/bash
set -e
LOGFILE="/var/log/backup_$(date +%F).log"

echo "Backup started at $(date)" > "$LOGFILE"
tar -czf /backup/etc_$(date +%F).tar.gz /etc &>> "$LOGFILE"
echo "Backup completed successfully at $(date)" >> "$LOGFILE"
```

Then add to crontab:

```
0 2 * * * /usr/local/bin/backup.sh
```

✅ Result: A reliable, automated backup running every day at 2 AM with logs.

---

## 💡 **SysAdmin Best Practices**

1. Always use **absolute paths** (no shortcuts like `~` or `$HOME`).
2. Redirect both stdout and stderr → `>> /path/to/log 2>&1`.
3. Include environment variables (`PATH`, `MAILTO`).
4. Use **`@reboot`** for startup tasks.
5. Always test your script manually before scheduling.
6. Combine cron + `logger` for centralized logging.
7. Keep documentation for all active cron jobs (`crontab -l > /root/cron_backup.txt`).
8. For frequent jobs (<1 min), use **systemd timers** instead.

---


