### **Understanding `umask`, Maximum Permissions, and Directory Permissions**

In Linux, the `umask` command is used to set the default file creation permissions for users. It determines which permissions are **masked out** or **subtracted** from the maximum possible permissions for files and directories.

---

### **1. `umask` Command**

The **`umask`** command sets default permissions for newly created files and directories by subtracting from the maximum permissions.

- **Maximum file permissions**: 666 (rw-rw-rw-)
- **Maximum directory permissions**: 777 (rwxrwxrwx)

The **`umask`** value specifies which permissions will **not** be set. The mask works by subtracting permissions from the maximum value. For example, a `umask` of `022` would subtract `0` from the owner’s permissions, `2` from the group’s permissions, and `2` from others’ permissions.

#### **Syntax of `umask`:**
```bash
umask [value]
```

#### **Default Permissions and Example:**
- **Maximum permissions**:
  - Files: **666** (`rw-rw-rw-`)
  - Directories: **777** (`rwxrwxrwx`)
  
- **umask Example 1 (umask 022)**:
  - For **files**: `666 - 022 = 644` (rw-r--r--)
  - For **directories**: `777 - 022 = 755` (rwxr-xr-x)
  
  In this example:
  - Files are created with permissions `rw-r--r--`.
  - Directories are created with permissions `rwxr-xr-x`.
  
- **umask Example 2 (umask 0777)**:
  - For **files**: `666 - 0777 = 000` (no permissions at all).
  - For **directories**: `777 - 0777 = 000` (no permissions at all).

---
### **`umask -S` Command**

The **`umask -S`** command is used to display the current **user file creation mask** in symbolic format, rather than the default numeric (octal) format.

### **Syntax**:
```bash
umask -S
```

### **What does `umask -S` do?**
- The **`umask`** command, when used without any options, shows the current **umask value** in **octal** format (e.g., `022`).
- The **`umask -S`** option shows the **umask** in **symbolic format** (e.g., `u=rwx,g=rx,o=rx`), which is easier to understand because it shows the permissions that are being **masked out**.

### **Understanding Symbolic Format**:
In symbolic format, the **umask** will display which permissions are being **removed** from the default maximum permissions:
- **`r`**: Read
- **`w`**: Write
- **`x`**: Execute
- **`u`**: User (owner)
- **`g`**: Group
- **`o`**: Others

### **Example of `umask -S`**:

#### Example 1: Default umask (022)

1. **Check the umask**:
   ```bash
   umask -S
   ```

   Output:
   ```
   u=rwx,g=rx,o=rx
   ```

   This means:
   - The **user** (owner) has **read**, **write**, and **execute** permissions.
   - The **group** and **others** have **read** and **execute** permissions.
   - This is equivalent to a numeric umask value of `022`, which subtracts **write** permission from group and others.

#### Example 2: Custom umask (027)

1. **Set a custom umask**:
   ```bash
   umask 027
   ```

2. **Check the umask**:
   ```bash
   umask -S
   ```

   Output:
   ```
   u=rwx,g=rx,o=
   ```

   This means:
   - The **user** (owner) has **read**, **write**, and **execute** permissions.
   - The **group** has **read** and **execute** permissions.
   - **Others** have **no** permissions at all.
   - The numeric equivalent of this `umask` is `027`, which removes **write** permission from the group and **read**, **write**, and **execute** permissions from others.

### **Key Points**:
- The **symbolic format** (`umask -S`) shows the permissions that will **not be set** when a new file or directory is created.
- By default, **files** are created with `666` permissions (`rw-rw-rw-`), and **directories** with `777` permissions (`rwxrwxrwx`).
- The **umask** value reduces the permissions by "masking out" certain bits.
  
### **Summary of Example Outputs**:
- **`umask -S` with `umask 022`**:
  - Output: `u=rwx,g=rx,o=rx`
  - Numeric Equivalent: `022`
- **`umask -S` with `umask 027`**:
  - Output: `u=rwx,g=rx,o=`
  - Numeric Equivalent: `027`
