### **fdisk Command Overview**

`fdisk` is a command-line utility in Linux used for partitioning hard drives and other block devices. It can be used to create, delete, modify, and view partitions on a disk. It supports only **MBR (Master Boot Record)** partition tables, not **GPT (GUID Partition Table)**.

Here’s a breakdown of the common options and functionality of `fdisk`:

#### **Key fdisk Options:**

1. **`fdisk -l`**:
   - Lists all partitions on all connected block devices.
   - Example:
     ```bash
     fdisk -l
     ```

2. **`fdisk /dev/sda`**:
   - This starts the `fdisk` interactive interface for the disk `/dev/sda`, where you can create, delete, and modify partitions.
   - Example:
     ```bash
     fdisk /dev/sda
     ```

3. **Interactive Commands in fdisk**:
   - `m` – Displays a help screen with available options.
   - `p` – Prints the partition table of the disk.
   - `n` – Creates a new partition (primary or extended).
   - `d` – Deletes a partition.
   - `t` – Changes a partition’s system type (e.g., ext4, swap).
   - `w` – Writes changes to disk and exits.
   - `q` – Exits without saving changes.

### **Creating Partitions Using fdisk:**

#### **1. Creating a Primary Partition:**

A primary partition is one of the main partitions on a disk and is directly addressable by the operating system. You can have up to **4 primary partitions** on an MBR partition table.

**Steps to create a primary partition**:
1. Start `fdisk` on `/dev/sda`:
   ```bash
   fdisk /dev/sda
   ```
2. Type `n` to create a new partition.
3. Type `p` for a primary partition.
4. Choose the partition number (1, 2, 3, or 4).
5. Enter the starting and ending sectors (you can press Enter to select the defaults, which will use the available space).
6. Type `w` to write the changes and exit.

Example:
```bash
fdisk /dev/sda
# Follow the prompts
# Type: n (new partition)
# Type: p (primary partition)
# Choose partition number: 1
# Set starting and ending sectors (or press Enter for defaults)
# Type: w (write and exit)
```

#### **2. Creating an Extended Partition:**

An extended partition is a special type of partition that allows you to create logical partitions within it. You can only have **one extended partition** on a disk.

**Steps to create an extended partition**:
1. Start `fdisk` on `/dev/sda`:
   ```bash
   fdisk /dev/sda
   ```
2. Type `n` to create a new partition.
3. Type `e` for an extended partition.
4. Choose the partition number.
5. Set the starting and ending sectors as prompted.
6. Type `w` to write changes and exit.

Example:
```bash
fdisk /dev/sda
# Type: n (new partition)
# Type: e (extended partition)
# Choose partition number
# Set starting and ending sectors (or press Enter for defaults)
# Type: w (write and exit)
```

### **Changing the File System of a Partition:**

To change or format a partition with a file system, you use a command like `mkfs`. For example, to create an **XFS file system** on `/dev/sda1`:

```bash
mkfs.xfs /dev/sda1
```

You can also create other file systems such as **ext4** by using:

```bash
mkfs.ext4 /dev/sda1
```

### **Mounting a Directory to a Partition:**

To mount a partition (e.g., `/dev/sda1`) to a directory (e.g., `/mydata`), you can use the `mount` command:

```bash
mkdir /mydata        # Create the mount point
mount /dev/sda1 /mydata  # Mount the partition
```

After mounting, any files created in `/mydata` will be stored on the partition `/dev/sda1`.

### **Case Study Example:**

Let’s walk through a scenario based on the following commands:

1. **List Block Devices:**
   ```bash
   lsblk
   ```

   This command lists the block devices and their partitions.

2. **Scan for New Disks:**
   ```bash
   ls /sys/class/scsi_host/ | while read host ; do echo "- - -" > /sys/class/scsi_host/$host/scan ; done
   ```

   This command scans for any newly added disk devices without rebooting.

3. **List Partitions:**
   ```bash
   cat /proc/partitions
   ```

   This lists the current partitions on all devices.

4. **List Details of `/dev/sda`:**
   ```bash
   fdisk -l /dev/sda
   ```

   This command shows the detailed partitioning information for `/dev/sda`.

5. **Create Partitions:**
   ```bash
   fdisk /dev/sda
   ```

   Use `fdisk` to create partitions on `/dev/sda` (primary and extended). Example:
   - Type `n` to create a new partition.
   - Choose `p` for primary and set sizes.
   - Choose `e` for extended if you need more partitions.

6. **Mount a Data Directory:**
   ```bash
   mkdir /mydata
   mount /dev/sda1 /mydata
   ```

   Mount the first partition (`/dev/sda1`) to `/mydata`. If no file system is created on the partition, you might encounter an error.

7. **Create an XFS Filesystem:**
   ```bash
   mkfs.xfs /dev/sda1
   mount /dev/sda1 /mydata
   ```

   Format `/dev/sda1` with XFS and mount it to `/mydata`.

8. **Check Disk Space and Filesystem Info:**
   ```bash
   df -h
   lsblk -f
   blkid /dev/sda1
   ```

   These commands display disk space usage, filesystem details, and specific information about `/dev/sda1`.

9. **Create a File and Verify Storage Location:**
   ```bash
   touch /mydata/file_sda1.java
   ```

   Create a file named `file_sda1.java` inside `/mydata`. This file is stored on the partition `/dev/sda1`.

10. **Unmount and Verify Data:**
    ```bash
    umount /mydata
    ls /mydata
    ```

    After unmounting `/mydata`, the directory is empty because the data is stored on the partition, not the mount point itself.

11. **Mount to a New Directory:**
    ```bash
    mkdir /testdata
    mount /dev/sda1 /testdata
    ls /testdata
    ```

    Mount `/dev/sda1` to `/testdata` and verify that `file_sda1.java` is still there. This proves that the file is stored on the partition (`/dev/sda1`) rather than the mount point.

### **Conclusion:**

The **`fdisk`** utility is crucial for partitioning disks in Linux. You can use it to create both primary and extended partitions, and assign file systems to them using commands like `mkfs.xfs` or `mkfs.ext4`. Mounting partitions to directories like `/mydata` allows you to interact with the data stored on these partitions. The case study demonstrates how a partition (e.g., `/dev/sda1`) stores data, which is confirmed by mounting it to different directories (`/mydata` and `/testdata`).
