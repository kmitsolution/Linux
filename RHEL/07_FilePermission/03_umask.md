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
### **2. Directory Execute Permission**

In Linux, **directories require the execute (`x`) permission** in order to allow users to **enter** or **navigate** into them.

- If a directory does not have **execute** permission, users cannot **cd** (change directory) into it, even if they have read (`r`) permission.

#### **Example 1: Without execute permission on directory**

1. **Create a directory**:
   ```bash
   mkdir example_dir
   ```

2. **Remove execute permission** for the directory:
   ```bash
   chmod -x example_dir
   ```

3. **Attempt to navigate into the directory**:
   ```bash
   cd example_dir
   ```
   Output:
   ```
   bash: cd: example_dir: Permission denied
   ```

#### **Example 2: With execute permission on directory**

1. **Add execute permission** to the directory:
   ```bash
   chmod +x example_dir
   ```

2. **Navigate into the directory**:
   ```bash
   cd example_dir
   ```
   Output:
   ```
   (No error, you are inside the directory now)
   ```

Without the **execute permission**, users cannot enter a directory, regardless of read permissions.

---

### **3. `mkdir -m` Command**

The **`mkdir -m`** command allows you to set specific permissions when creating a new directory.

#### **Syntax of `mkdir -m`**:
```bash
mkdir -m [permissions] directory_name
```

- **`[permissions]`**: The permissions you want to assign (in numeric or symbolic format).

#### **Example:**

Create a directory with specific permissions using `mkdir -m`:

1. **Create a directory** `new_dir` with **755** permissions:
   ```bash
   mkdir -m 755 new_dir
   ```

2. **Check the directory permissions**:
   ```bash
   ls -ld new_dir
   ```

   Output:
   ```
   drwxr-xr-x 2 user user 4096 Dec  7 12:00 new_dir
   ```

Explanation:
- The directory is created with `rwxr-xr-x` (755) permissions, where:
  - Owner has read, write, execute permissions.
  - Group and others have read and execute permissions.

---

### **4. `cp -p` Command**

The **`cp -p`** command is used to copy files or directories while preserving the original attributes, including the file permissions, timestamps, and ownerships.

#### **Syntax of `cp -p`**:
```bash
cp -p source_file destination_file
```

#### **Example 1: Copying a file with `cp -p`**

1. **Create a file**:
   ```bash
   echo "Hello, World!" > original_file.txt
   ```

2. **Check the permissions of the original file**:
   ```bash
   ls -l original_file.txt
   ```

   Output:
   ```
   -rw-r--r-- 1 user user 14 Dec  7 12:00 original_file.txt
   ```

3. **Copy the file using `cp -p`**:
   ```bash
   cp -p original_file.txt copied_file.txt
   ```

4. **Check the permissions of the copied file**:
   ```bash
   ls -l copied_file.txt
   ```

   Output:
   ```
   -rw-r--r-- 1 user user 14 Dec  7 12:00 copied_file.txt
   ```

Explanation:
- The `cp -p` command preserves the permissions and timestamps of the original file (`original_file.txt`) when copying it to `copied_file.txt`.

#### **Example 2: Copying a directory with `cp -p`**

1. **Create a directory**:
   ```bash
   mkdir original_dir
   ```

2. **Copy the directory** using `cp -p`:
   ```bash
   cp -p -r original_dir copied_dir
   ```

3. **Check the permissions of the copied directory**:
   ```bash
   ls -ld copied_dir
   ```

   Output:
   ```
   drwxr-xr-x 2 user user 4096 Dec  7 12:00 copied_dir
   ```

Explanation:
- The `-r` option is used for recursively copying directories, and `-p` ensures that all original attributes, including permissions, ownership, and timestamps, are preserved.

---

### **Summary**

- **`umask`**: A command that determines the default file and directory permissions when new files and directories are created.
- **Maximum Permissions**:
  - Files: **666** (`rw-rw-rw-`)
  - Directories: **777** (`rwxrwxrwx`)
- **Directory Execute Permission**: Without execute permission, users cannot navigate into a directory (`cd` command).
- **`mkdir -m`**: Creates directories with specific permissions set during the creation process.
- **`cp -p`**: Copies files and directories while preserving the original file permissions, timestamps, and ownership.
