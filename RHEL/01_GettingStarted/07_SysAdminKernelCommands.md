## Linux Kernel (RHEL) – Useful Commands 🐧

The **Linux kernel** is the core part of the operating system. It manages **CPU, memory, hardware devices, processes, and system calls** between applications and hardware.

In **Red Hat Enterprise Linux (RHEL)**, several commands help you view and manage kernel information.

---

### 1. Check Current Kernel Version

```bash
uname -r
```

Example output:

```
5.14.0-362.el9.x86_64
```

* `5.14.0` → Kernel version
* `362.el9` → RHEL build
* `x86_64` → Architecture

To see **complete kernel information**:

```bash
uname -a
```

---

### 2. Show Kernel Name

```bash
uname -s
```

Output:

```
Linux
```

---

### 3. Show System Architecture

```bash
uname -m
```

Example:

```
x86_64
```

---

### 4. View Kernel Parameters (Runtime)

Kernel parameters control system behavior.

```bash
sysctl -a
```

Example:

```
net.ipv4.ip_forward = 0
vm.swappiness = 60
```

To check a specific parameter:

```bash
sysctl net.ipv4.ip_forward
```

To change it temporarily:

```bash
sysctl -w net.ipv4.ip_forward=1
```

---

### 5. View Loaded Kernel Modules

Kernel modules are drivers loaded into the kernel.

```bash
lsmod
```

Example output:

```
Module          Size  Used by
vboxguest      49152  2
vboxsf         53248  0
```

---

### 6. Load a Kernel Module

```bash
modprobe module_name
```

Example:

```bash
modprobe dummy
```

---

### 7. Remove a Kernel Module

```bash
modprobe -r module_name
```

Example:

```bash
modprobe -r dummy
```

---

### 8. Show Kernel Ring Buffer (Hardware / Boot Messages)

```bash
dmesg
```

Example:

```
[    0.000000] Linux version 5.14.0
[    1.234567] CPU: Intel(R) Core(TM)
```

To view latest messages:

```bash
dmesg | tail
```

---

### 9. Show Installed Kernels in RHEL

```bash
rpm -qa | grep kernel
```

Example:

```
kernel-5.14.0-362.el9.x86_64
kernel-core-5.14.0-362.el9.x86_64
```

---

### 10. Show Kernel Boot Parameters

```bash
cat /proc/cmdline
```

Example:

```
BOOT_IMAGE=/vmlinuz-5.14.0 root=/dev/mapper/rhel-root
```

---

### 11. View Kernel Information from `/proc`

```bash
cat /proc/version
```

Shows:

* Kernel version
* Compiler
* Build info

---

### 12. Check Kernel Modules Directory

```bash
ls /lib/modules/$(uname -r)
```

Shows modules available for the current kernel.

---

## Quick Commands Summary

| Command             | Purpose                    |
| ------------------- | -------------------------- |
| `uname -r`          | Show kernel version        |
| `uname -a`          | Complete kernel info       |
| `sysctl -a`         | Show kernel parameters     |
| `lsmod`             | List loaded kernel modules |
| `modprobe`          | Load kernel module         |
| `modprobe -r`       | Remove module              |
| `dmesg`             | Kernel boot messages       |
| `cat /proc/version` | Kernel build info          |

---

💡 **RHEL Admin Tip:**
When troubleshooting **network, drivers, or hardware issues**, the most commonly used kernel commands are:

```
uname -r
lsmod
dmesg
sysctl
```

---
