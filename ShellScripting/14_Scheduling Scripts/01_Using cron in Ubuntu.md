
# 🕒 **Using `cron` in Ubuntu (and RHEL)**

---

## 🧠 **What is `cron`?**

`cron` is a **time-based job scheduler** built into Linux.
It runs background tasks at specified times or intervals, like:

* Running backup scripts
* Rotating logs
* Monitoring system health
* Syncing data or performing cleanups

The cron service runs automatically in Ubuntu as:

```
systemctl status cron
```

---

## ⚙️ **1. Cron Structure (Crontab Syntax)**

Each cron job entry has **five time fields** followed by the **command to execute**:

```
*  *  *  *  *  command_to_run
│  │  │  │  │
│  │  │  │  └── Day of week (0–7) (Sun=0 or 7)
│  │  │  └───── Month (1–12)
│  │  └───────── Day of month (1–31)
│  └──────────── Hour (0–23)
└─────────────── Minute (0–59)
```

---

### 🧩 **Example Schedule Patterns**

| Time Expression | Meaning                                  |
| --------------- | ---------------------------------------- |
| `* * * * *`     | Every minute                             |
| `0 * * * *`     | Every hour                               |
| `0 0 * * *`     | Every day at midnight                    |
| `0 2 * * 1`     | Every Monday at 2:00 AM                  |
| `*/10 * * * *`  | Every 10 minutes                         |
| `30 6 1 * *`    | On the 1st day of every month at 6:30 AM |

---

## ⚙️ **2. Managing Cron Jobs**

### ✅ **View Current User’s Crontab**

```bash
crontab -l
```

### ✅ **Edit Current User’s Crontab**

```bash
crontab -e
```

When you run this command for the first time, Ubuntu will ask you to choose an editor (like nano or vim).

---

### ✅ **Remove All Cron Jobs**

```bash
crontab -r
```

⚠️ *Be careful!* This deletes all your user’s scheduled cron jobs.

---

## 🧩 **3. Example 1 – Running a Script Daily**

Let’s say you have a backup script:

```bash
/home/admin/scripts/backup.sh
```

Make sure it’s executable:

```bash
chmod +x /home/admin/scripts/backup.sh
```

Then schedule it in cron:

```bash
crontab -e
```

Add the line:

```
0 2 * * * /home/admin/scripts/backup.sh
```

✅ *Meaning:* Run every day at **2:00 AM**.

---

## 🧩 **4. Example 2 – Log Rotation Every Sunday**

```
0 3 * * 0 /home/admin/scripts/log_cleanup.sh >> /var/log/log_cleanup.log 2>&1
```

✅ *Meaning:*

* Runs every **Sunday at 3:00 AM**
* Logs both **output and errors** to `/var/log/log_cleanup.log`

---

## 🧩 **5. Example 3 – System Health Monitoring Every Hour**

```
0 * * * * /home/admin/scripts/health_check.sh
```

✅ *Meaning:* Runs your system health script hourly (great for uptime or resource tracking).

---

## ⚙️ **6. Example 4 – Using Environment Variables**

Cron runs with a **limited environment**, so you often need to specify paths manually.

```bash
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
MAILTO=admin@example.com

0 1 * * * /home/admin/scripts/backup.sh
```

✅ *Use Case:*

* Adds necessary PATH variables
* Sends job output to your email (if mail is configured)

---

## 🧩 **7. Example 5 – Every 5 Minutes (Test Job)**

```bash
*/5 * * * * echo "Cron test at $(date)" >> /tmp/cron_test.log
```

✅ *Meaning:* Every 5 minutes, a timestamp will be appended to `/tmp/cron_test.log`.

---

## 🧩 **8. Example 6 – Run Script on Reboot**

If you want a script to run automatically **when the system starts**, use the special keyword:

```
@reboot /home/admin/scripts/startup.sh
```

✅ *Use Case:* Start a custom service or initialize a monitoring agent on boot.

---

## ⚙️ **9. Cron Job Ownership**

You can define cron jobs:

* For **your user** → via `crontab -e`
* For **system-wide jobs** → `/etc/crontab` or `/etc/cron.d/`
* For **specific schedules**:

  * `/etc/cron.hourly/`
  * `/etc/cron.daily/`
  * `/etc/cron.weekly/`
  * `/etc/cron.monthly/`

Example system cron entry:

```
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
0 0 * * * root /usr/local/bin/system_backup.sh
```

📘 *Use Case:* Root-level cron tasks for system-wide maintenance.

---

## 🧩 **10. Checking Cron Logs**

### ✅ **View Cron Execution Logs**

```bash
grep CRON /var/log/syslog
```

or

```bash
journalctl -u cron
```

✅ **Tip:** Always redirect output and errors to a file for debugging:

```
*/30 * * * * /path/to/script.sh >> /var/log/script.log 2>&1
```

---

## 🧾 **Quick Summary**

| Task              | Command / Location                      |
| ----------------- | --------------------------------------- |
| List user crons   | `crontab -l`                            |
| Edit user crons   | `crontab -e`                            |
| Delete user crons | `crontab -r`                            |
| System-wide crons | `/etc/crontab`, `/etc/cron.d/`          |
| Hourly/daily jobs | `/etc/cron.hourly/`, `/etc/cron.daily/` |
| Run on reboot     | `@reboot /path/to/script`               |
| Check logs        | `grep CRON /var/log/syslog`             |

---

## 💡 **SysAdmin Best Practices**

1. Always use **absolute paths** in cron commands (e.g., `/usr/bin/df`, `/home/admin/script.sh`).
2. Redirect all script output to **log files** (`>> /var/log/your_script.log 2>&1`).
3. Test your script manually before scheduling.
4. Keep cron jobs under **root** only when necessary.
5. For complex scheduling, prefer **systemd timers** (RHEL 8+/Ubuntu 20+).
6. Use `MAILTO` in cron for email notifications on failures.

---


