
## 🧰 **SysAdmin-Level Function Examples (Local vs Global Variables)**

### 🧩 **Example 1 – Service Health Check**

A script to check multiple services and log their status.

```bash
#!/bin/bash

LOG_FILE="/var/log/service_check.log"

check_service() {
    local svc=$1
    local status

    systemctl is-active --quiet "$svc"
    status=$?

    if [ $status -eq 0 ]; then
        echo "$(date): $svc is running." >> "$LOG_FILE"
    else
        echo "$(date): $svc is NOT running!" >> "$LOG_FILE"
    fi
}

for service in sshd firewalld crond; do
    check_service "$service"
done

echo "Service check completed. Log saved to $LOG_FILE"
```

✅ **Key Points**

* `svc` and `status` are **local** — no conflict if other functions use similar variable names.
* Useful for daily system health checks and automation scripts.

---

### 🧩 **Example 2 – Backup with Function Isolation**

Backing up configuration files with unique timestamps.

```bash
#!/bin/bash

BACKUP_ROOT="/backup/configs"

create_backup() {
    local src_dir=$1
    local timestamp=$(date +%F_%H-%M-%S)
    local dest_dir="$BACKUP_ROOT/$(basename $src_dir)_$timestamp"

    mkdir -p "$dest_dir"
    cp -r "$src_dir"/* "$dest_dir"/
    echo "Backup created for $src_dir at $dest_dir"
}

create_backup "/etc"
create_backup "/var/www"
```

✅ **Why local helps:**
Each backup call uses its own `src_dir` and `dest_dir` without overwriting global variables or affecting other backups.

---

### 🧩 **Example 3 – Disk Usage Monitoring**

A function to monitor disk usage and alert if it exceeds a threshold.

```bash
#!/bin/bash

THRESHOLD=80

check_disk_usage() {
    local mount_point=$1
    local usage=$(df -h "$mount_point" | awk 'NR==2 {gsub("%",""); print $5}')

    if [ "$usage" -gt "$THRESHOLD" ]; then
        echo "ALERT: Disk usage on $mount_point is ${usage}% (above ${THRESHOLD}%)"
    else
        echo "OK: Disk usage on $mount_point is ${usage}%"
    fi
}

check_disk_usage "/"
check_disk_usage "/home"
```

✅ **Why it’s sysadmin-level:**
This can be run via `cron` to automatically alert admins when disks are nearly full — `local` keeps variables scoped within the function.

---

### 🧩 **Example 4 – Network Connectivity Check**

Test connectivity to multiple servers.

```bash
#!/bin/bash

check_connectivity() {
    local host=$1
    ping -c 2 "$host" &> /dev/null

    if [ $? -eq 0 ]; then
        echo "$(date): $host is reachable"
    else
        echo "$(date): $host is unreachable"
    fi
}

for host in 8.8.8.8 example.com localhost; do
    check_connectivity "$host"
done
```

✅ **Sysadmin Use Case:**
Ideal for quick network diagnostics or embedding into a daily system status script.

---

### 🧩 **Example 5 – Log Cleanup Automation**

Rotate and compress old logs safely using local variables.

```bash
#!/bin/bash

cleanup_logs() {
    local log_dir=$1
    local days=$2
    local backup_dir="/backup/old_logs"

    mkdir -p "$backup_dir"
    find "$log_dir" -type f -mtime +"$days" -exec gzip {} \; -exec mv {}.gz "$backup_dir" \;

    echo "Logs older than $days days moved to $backup_dir"
}

cleanup_logs "/var/log" 7
```

✅ **Sysadmin Use Case:**
Automates log rotation and cleanup — prevents `/var/log` from filling up.
`local` ensures no interference with global path or backup variables.

---

### 🧩 **Example 6 – User Account Checker**

Verify if certain users exist on the system.

```bash
#!/bin/bash

check_user() {
    local user=$1
    if id "$user" &>/dev/null; then
        echo "User $user exists"
    else
        echo "User $user not found!"
    fi
}

for user in root apache mysql devops; do
    check_user "$user"
done
```

✅ **Sysadmin Use Case:**
Quickly validates critical service accounts during setup or audits.

---


