# 🕒 **Using systemd Timers in RHEL 8+ (Modern Alternative to cron)**

---

## 🧠 **What is a systemd timer?**

A **systemd timer** is a unit that triggers a **systemd service** on a schedule — similar to how cron triggers a script.

**Advantages over cron:**
✅ Centralized logging via `journalctl`
✅ Handles missed runs (if system was off)
✅ Runs at precise intervals (`OnCalendar`, `OnBootSec`, etc.)
✅ Integrates with system startup and service dependencies
✅ Can be monitored like any other systemd service

---

## ⚙️ **1. Components of a systemd Timer**

Each timer requires **two files** (unit files):

1. A **Service file** → defines *what to do*
2. A **Timer file** → defines *when to do it*

---

## 🧩 **Example 1 – Create a Simple Backup Timer**

### 🧱 Step 1: Create a service file

Create `/etc/systemd/system/backup.service`

```ini
[Unit]
Description=Daily Backup Script

[Service]
Type=simple
ExecStart=/usr/local/bin/backup.sh
```

✅ *Explanation:*

* `ExecStart` runs the actual script.
* The service handles **what** action should occur.

---

### 🕒 Step 2: Create the corresponding timer file

Create `/etc/systemd/system/backup.timer`

```ini
[Unit]
Description=Run Daily Backup Script

[Timer]
OnCalendar=daily
Persistent=true

[Install]
WantedBy=timers.target
```

✅ *Explanation:*

* `OnCalendar=daily` → runs every day at midnight.
* `Persistent=true` → ensures missed runs (due to downtime) are executed next time system boots.
* `WantedBy=timers.target` → enables the timer at startup.

---

### 🧰 Step 3: Enable and Start the Timer

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now backup.timer
```

✅ **Check status:**

```bash
systemctl list-timers
```

You’ll see:

```
NEXT                         LEFT        LAST                         PASSED       UNIT          ACTIVATES
Fri 2025-11-09 00:00:00 IST  6h left     Fri 2025-11-08 00:00:01 IST  18h ago      backup.timer  backup.service
```

---

### 🧰 Step 4: Verify Logs

```bash
journalctl -u backup.service
```

✅ **Output:**

```
Nov 08 00:00:00 server systemd[1]: Started Daily Backup Script.
Nov 08 00:00:02 server backup.sh[2487]: Backup completed successfully.
```

---

## 🧩 **Example 2 – Run a Script Every 15 Minutes**

Create `/etc/systemd/system/healthcheck.service`

```ini
[Unit]
Description=System Health Check

[Service]
Type=simple
ExecStart=/usr/local/bin/health_check.sh
```

Create `/etc/systemd/system/healthcheck.timer`

```ini
[Unit]
Description=Run health check every 15 minutes

[Timer]
OnCalendar=*:0/15
Persistent=true

[Install]
WantedBy=timers.target
```

✅ *Explanation:*

* `OnCalendar=*:0/15` → runs every 15 minutes.
* `Persistent=true` → catches up on missed intervals.

---

## 🧩 **Example 3 – Run a Task at Boot and Then Every Hour**

Create `/etc/systemd/system/logrotate.timer`

```ini
[Unit]
Description=Rotate logs hourly and at boot

[Timer]
OnBootSec=5min
OnUnitActiveSec=1h
Persistent=true

[Install]
WantedBy=timers.target
```

✅ *Explanation:*

* `OnBootSec=5min` → start 5 minutes after boot.
* `OnUnitActiveSec=1h` → repeat every hour.

---

## ⚙️ **4. Common `OnCalendar` Expressions**

| Expression                       | Description                 |
| -------------------------------- | --------------------------- |
| `OnCalendar=daily`               | Every day at midnight       |
| `OnCalendar=hourly`              | Every hour                  |
| `OnCalendar=weekly`              | Every week                  |
| `OnCalendar=Mon *-*-* 09:00:00`  | Every Monday at 9 AM        |
| `OnCalendar=*-*-01 00:00:00`     | 1st of every month          |
| `OnCalendar=*:0/10`              | Every 10 minutes            |
| `OnCalendar=2025-11-10 15:00:00` | One-time specific date/time |

---

## 🧰 **5. Useful Commands**

| Command                                 | Description               |
| --------------------------------------- | ------------------------- |
| `systemctl list-timers`                 | View all active timers    |
| `systemctl status myscript.timer`       | Check specific timer      |
| `journalctl -u myscript.service`        | View logs for the service |
| `sudo systemctl start myscript.timer`   | Start timer immediately   |
| `sudo systemctl stop myscript.timer`    | Stop timer                |
| `sudo systemctl disable myscript.timer` | Disable timer permanently |

---

## 🧩 **Example 4 – System Health Script (SysAdmin Use Case)**

Create a system health check that runs every hour and logs CPU/memory usage.

**File:** `/usr/local/bin/sys_health.sh`

```bash
#!/bin/bash
LOGFILE="/var/log/sys_health.log"

{
    echo "===== $(date) ====="
    echo "Hostname: $(hostname)"
    echo "Uptime: $(uptime -p)"
    echo "Disk Usage:"
    df -h /
    echo "Memory Usage:"
    free -h
    echo "===================="
} >> "$LOGFILE"
```

Make it executable:

```bash
chmod +x /usr/local/bin/sys_health.sh
```

**Create service:** `/etc/systemd/system/sys_health.service`

```ini
[Unit]
Description=System Health Logger

[Service]
ExecStart=/usr/local/bin/sys_health.sh
```

**Create timer:** `/etc/systemd/system/sys_health.timer`

```ini
[Unit]
Description=Run System Health Logger every hour

[Timer]
OnCalendar=hourly
Persistent=true

[Install]
WantedBy=timers.target
```

Enable it:

```bash
systemctl daemon-reload
systemctl enable --now sys_health.timer
```

✅ Check timer status:

```bash
systemctl list-timers | grep sys_health
```

✅ View logs:

```bash
tail -f /var/log/sys_health.log
```

---

## 🧾 **6. Quick Comparison: Cron vs Systemd Timer**

| Feature                     | Cron                     | systemd Timer                    |
| --------------------------- | ------------------------ | -------------------------------- |
| Precision                   | Good                     | High                             |
| Missed runs after reboot    | Skipped                  | **Executed (Persistent=true)**   |
| Logging                     | Manual (via redirection) | Integrated (`journalctl`)        |
| Dependencies                | None                     | **Can depend on other services** |
| Monitoring                  | Minimal                  | **`systemctl status`**           |
| Ease of debugging           | Basic                    | **Structured and logged**        |
| Recommended for new systems | ⚠️ Legacy                | ✅ **Modern & Preferred**         |

---

## 💡 **SysAdmin Best Practices**

1. Store scripts in `/usr/local/bin/` and make them executable.
2. Use `Persistent=true` for critical automation (backups, health checks).
3. Always use **absolute paths** in service files.
4. Use `journalctl -u <service>` to review execution logs.
5. Combine with `set -e` and `trap` for safer script execution.
6. Keep your `.service` and `.timer` files documented with `[Description]` fields.

---


