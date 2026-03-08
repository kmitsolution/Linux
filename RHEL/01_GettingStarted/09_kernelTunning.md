Kernel tuning in **RHEL** means **adjusting kernel parameters to optimize system performance, networking, memory, and security**.
These parameters are controlled using **`sysctl`** and stored mainly in:

```
/etc/sysctl.conf
/etc/sysctl.d/*.conf
```

Temporary changes affect the **running kernel**, while permanent changes persist after reboot.

---

# 1. View Current Kernel Parameters

List all kernel parameters:

```bash
sysctl -a
```

Example output:

```
net.ipv4.ip_forward = 0
vm.swappiness = 60
kernel.pid_max = 4194304
```

Check a specific parameter:

```bash
sysctl vm.swappiness
```

---

# 2. Temporary Kernel Tuning (Runtime)

Example: Enable IP forwarding.

```bash
sysctl -w net.ipv4.ip_forward=1
```

Verify:

```bash
sysctl net.ipv4.ip_forward
```

⚠️ This change disappears after reboot.

---

# 3. Permanent Kernel Tuning

Edit:

```bash
vi /etc/sysctl.conf
```

Example:

```
net.ipv4.ip_forward = 1
vm.swappiness = 10
```

Apply changes:

```bash
sysctl -p
```

---

# 4. Important Kernel Tuning Examples (Real Admin Tasks)

## 1. Enable IP Forwarding (Router / NAT Servers)

Used when Linux acts as a **router, firewall, or NAT gateway**.

Check:

```bash
sysctl net.ipv4.ip_forward
```

Enable temporarily:

```bash
sysctl -w net.ipv4.ip_forward=1
```

Permanent:

```
net.ipv4.ip_forward = 1
```

---

# 2. Reduce Swappiness (Improve Memory Performance)

Swappiness controls **how aggressively Linux uses swap memory**.

Check:

```bash
cat /proc/sys/vm/swappiness
```

Default:

```
60
```

Better for servers:

```
vm.swappiness = 10
```

Apply:

```bash
sysctl -p
```

Less swap usage → **better performance for database/web servers**.

---

# 3. Increase Maximum Open Files

Important for **web servers, databases, and high-traffic systems**.

Check:

```bash
cat /proc/sys/fs/file-max
```

Increase:

```
fs.file-max = 1000000
```

Apply:

```bash
sysctl -p
```

---

# 4. Increase Maximum Process IDs

Controls how many processes the system can create.

Check:

```bash
cat /proc/sys/kernel/pid_max
```

Example tuning:

```
kernel.pid_max = 4194304
```

Useful in **large container or cloud systems**.

---

# 5. Protect Against SYN Flood Attacks

Important for **network security**.

Enable SYN cookies:

```
net.ipv4.tcp_syncookies = 1
```

Check:

```bash
sysctl net.ipv4.tcp_syncookies
```

Used to **prevent TCP SYN flood attacks**.

---

# 6. Increase Network Queue (High Traffic Servers)

Improve network performance.

```
net.core.somaxconn = 1024
```

Used for **high-load web servers** like **NGINX or Apache**.

---

# 7. Increase TCP Buffer Sizes

Better for **high-speed networks**.

Example:

```
net.core.rmem_max = 16777216
net.core.wmem_max = 16777216
```

---

# 8. Disable ICMP Redirects (Security Hardening)

```
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.all.send_redirects = 0
```

Prevents **man-in-the-middle attacks**.

---

# 5. Apply Changes Without Reboot

Reload sysctl configuration:

```bash
sysctl -p
```

Or reload all config files:

```bash
sysctl --system
```

---

# 6. Example Real Production Kernel Tuning File

Example `/etc/sysctl.conf`:

```
# Network tuning
net.ipv4.ip_forward = 1
net.core.somaxconn = 1024

# Security
net.ipv4.tcp_syncookies = 1
net.ipv4.conf.all.accept_redirects = 0

# Memory tuning
vm.swappiness = 10

# File limits
fs.file-max = 1000000
```

Apply:

```bash
sysctl -p
```

---

# 7. Kernel Parameter Locations

Kernel parameters exist in:

```
/proc/sys/
```

Example:

```
/proc/sys/net/ipv4/ip_forward
/proc/sys/vm/swappiness
/proc/sys/kernel/pid_max
```

You can modify directly:

```bash
echo 1 > /proc/sys/net/ipv4/ip_forward
```

But **sysctl is recommended**.

---

# 8. Most Important Kernel Tunings (Interview)

Linux admins should know:

| Parameter               | Purpose              |
| ----------------------- | -------------------- |
| net.ipv4.ip_forward     | Enable routing       |
| vm.swappiness           | Control swap usage   |
| fs.file-max             | Max open files       |
| kernel.pid_max          | Max processes        |
| net.ipv4.tcp_syncookies | SYN flood protection |
| net.core.somaxconn      | Network queue size   |

---

