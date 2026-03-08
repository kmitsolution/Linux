In Linux (including RHEL), **`/proc`** and **`/sys`** are **virtual filesystems** created by the kernel. They do not store real files on disk. Instead, they **expose kernel and hardware information in real time**.
Linux administrators use them frequently for **troubleshooting, performance tuning, and system inspection**.

---

# 1. Important Kernel Files in `/proc`

The **`/proc` filesystem** mainly provides **process and kernel runtime information**.

### 1. Check CPU Information

```bash
cat /proc/cpuinfo
```

Shows:

* CPU model
* cores
* cache size
* CPU speed

Example:

```
processor : 0
model name : Intel(R) Core(TM) i7
cpu cores : 4
```

---

### 2. Check Memory Information

```bash
cat /proc/meminfo
```

Shows detailed RAM usage.

Example:

```
MemTotal:       8040000 kB
MemFree:        1200000 kB
SwapTotal:      2097148 kB
```

Useful for **memory troubleshooting**.

---

### 3. Kernel Version

```bash
cat /proc/version
```

Shows:

* Kernel version
* compiler
* build info

---

### 4. System Uptime

```bash
cat /proc/uptime
```

Example:

```
3600.23 2400.10
```

Meaning:

* System running for **3600 seconds**
* CPU idle time **2400 seconds**

---

### 5. Mounted Filesystems

```bash
cat /proc/mounts
```

Shows all mounted filesystems.

Example:

```
/dev/sda1 / ext4 rw 0 0
```

---

### 6. Kernel Boot Parameters

```bash
cat /proc/cmdline
```

Example:

```
BOOT_IMAGE=/vmlinuz root=/dev/sda1 ro quiet
```

Shows **kernel boot arguments from GRUB**.

---

### 7. Running Processes

Each process has a directory.

Example:

```
/proc/1
/proc/200
/proc/3500
```

Check process info:

```bash
cat /proc/1/status
```

Shows:

* process state
* memory usage
* user ID

---

### 8. Network Information

```bash
cat /proc/net/dev
```

Shows network interface statistics.

Example:

```
eth0: bytes packets errors
```

---

### 9. Load Average

```bash
cat /proc/loadavg
```

Example:

```
0.30 0.25 0.10
```

Shows system load for:

* 1 minute
* 5 minutes
* 15 minutes

---

# 2. Important Kernel Files in `/sys`

The **`/sys` filesystem (sysfs)** shows **hardware devices and kernel objects**.

Location:

```
/sys
```

Used mainly for **hardware, drivers, and kernel tuning**.

---

### 1. CPU Information

```bash
ls /sys/devices/system/cpu/
```

Shows CPUs:

```
cpu0 cpu1 cpu2 cpu3
```

Example:

```bash
cat /sys/devices/system/cpu/cpu0/online
```

Shows if CPU is active.

---

### 2. CPU Frequency

```bash
cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_cur_freq
```

Shows current CPU frequency.

---

### 3. Block Devices

```bash
ls /sys/block
```

Example:

```
sda
sdb
sr0
```

Check disk info:

```bash
cat /sys/block/sda/size
```

Shows disk size.

---

### 4. Network Interfaces

```bash
ls /sys/class/net
```

Example:

```
eth0
lo
```

Check interface state:

```bash
cat /sys/class/net/eth0/operstate
```

Output:

```
up
```

---

### 5. Device Information

```bash
ls /sys/devices
```

Shows hardware connected to the system.

Example:

```
pci0000:00
platform
system
```

---

# 3. Difference Between `/proc` and `/sys`

| Feature  | `/proc`                        | `/sys`                          |
| -------- | ------------------------------ | ------------------------------- |
| Purpose  | Process and kernel information | Hardware and device information |
| Type     | Virtual filesystem             | Virtual filesystem              |
| Used for | System status, process data    | Device and driver control       |
| Example  | `/proc/cpuinfo`                | `/sys/class/net`                |

---

# 4. Real Linux Admin Use Cases

### Check RAM quickly

```bash
cat /proc/meminfo | grep MemTotal
```

---

### Check CPU cores

```bash
grep -c processor /proc/cpuinfo
```

---

### Check network interface status

```bash
cat /sys/class/net/eth0/operstate
```

---

### Check disk devices

```bash
ls /sys/block
```

---

✅ **Most Important Files Linux Admins Use**

```
/proc/cpuinfo
/proc/meminfo
/proc/loadavg
/proc/cmdline
/proc/mounts
/sys/class/net/
/sys/block/
/sys/devices/system/cpu/
```

---

