## RHEL / Linux Boot Process

The **boot process** is the sequence of steps that happen when a system starts from **power on → login prompt**.

In **RHEL**, the boot process mainly follows these stages:

```
BIOS/UEFI → GRUB2 → Kernel → initramfs → systemd → Login
```

---

# 1. BIOS / UEFI (Hardware Initialization)

When the system powers on:

* **BIOS (Basic Input Output System)** or **UEFI** starts.
* It performs **POST (Power-On Self Test)**.
* Checks hardware like:

  * CPU
  * RAM
  * Disk
  * Keyboard

Then it looks for a **bootable device** (disk).

Important boot location:

```
MBR (Master Boot Record)
```

or for modern systems:

```
EFI System Partition
```

---

# 2. Bootloader (GRUB2)

RHEL uses **GRUB2 (Grand Unified Bootloader)**.

GRUB2 loads the **Linux kernel** into memory.

Important files:

```
/boot/grub2/grub.cfg
/etc/default/grub
```

For UEFI systems:

```
/boot/efi/EFI/redhat/grub.cfg
```

Useful commands:

```bash
grubby --default-kernel
cat /boot/grub2/grub.cfg
```

---

# 3. Kernel Loading

GRUB loads the **Linux kernel**.

Kernel file location:

```
/boot/vmlinuz-<kernel-version>
```

Example:

```
/boot/vmlinuz-5.14.0-362.el9.x86_64
```

The kernel then:

* Initializes hardware
* Detects devices
* Mounts the root filesystem
* Starts the init system

---

# 4. initramfs (Initial RAM Filesystem)

Before mounting the real root filesystem, the kernel loads **initramfs**.

File location:

```
/boot/initramfs-<kernel-version>.img
```

Purpose:

* Contains **drivers and modules**
* Helps the kernel access the real root filesystem.

---

# 5. systemd (Init Process)

Once the root filesystem is mounted, the kernel starts:

```
/sbin/init
```

In modern Linux, this is **systemd**.

Check it:

```bash
ps -p 1
```

Output:

```
systemd
```

systemd then:

* Starts system services
* Mounts filesystems
* Starts networking
* Reaches login state

---

# 6. Targets (Runlevels)

Older Linux used **runlevels**.

RHEL now uses **systemd targets**.

Common targets:

| Target            | Meaning          |
| ----------------- | ---------------- |
| rescue.target     | Single-user mode |
| multi-user.target | CLI mode         |
| graphical.target  | GUI mode         |

Check default target:

```bash
systemctl get-default
```

Change target:

```bash
systemctl set-default multi-user.target
```

---

# 7. Login Prompt

Finally the system shows:

```
login:
```

or a graphical login screen.

Users can now log in.

---

# Boot Process Summary

```
1 Power ON
2 BIOS / UEFI
3 GRUB2 Bootloader
4 Kernel
5 initramfs
6 systemd
7 Services start
8 Login prompt
```

---

# Important Boot Files in RHEL

| File                 | Purpose               |
| -------------------- | --------------------- |
| /boot/vmlinuz        | Kernel                |
| /boot/initramfs      | Initial filesystem    |
| /boot/grub2/grub.cfg | GRUB configuration    |
| /etc/default/grub    | GRUB settings         |
| /etc/fstab           | Filesystem mounts     |
| /etc/systemd/system  | Service configuration |

---

# Boot Troubleshooting Commands

Check boot logs:

```bash
journalctl -b
```

Kernel messages:

```bash
dmesg
```

Boot performance:

```bash
systemd-analyze
```

Failed services:

```bash
systemctl --failed
```

---

# Interview Short Answer

**RHEL Boot Process:**

1. BIOS/UEFI initializes hardware
2. GRUB2 bootloader loads the kernel
3. Kernel initializes hardware and loads initramfs
4. initramfs mounts the root filesystem
5. systemd starts system services
6. System reaches login prompt

