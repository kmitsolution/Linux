
### **File Permissions**

In Linux, file permissions determine who can read, write, or execute a file. The permissions are displayed when running `ls -l` and are divided into three groups: Owner, Group, and Others.

1. **Permissions:**
   - **`r`** – Read: The ability to read the contents of the file.
   - **`w`** – Write: The ability to modify the file's contents.
   - **`x`** – Execute: The ability to execute the file (for programs or scripts).
   - **`-`** – No permission.

2. **Example of Permissions (from `ls -l`):**
   ```
   -rwxr-xr--
   ```
   - **Owner**: Read (`r`), Write (`w`), Execute (`x`)
   - **Group**: Read (`r`), Execute (`x`)
   - **Others**: Read (`r`)

### **Numeric Representation of Permissions (Octal)**

File permissions can be represented by numbers, which is easier to manage when setting permissions with `chmod`. Here’s how they work:

| Permission | Numeric Value | Binary Value |
|------------|---------------|--------------|
| **Read**   | 4             | 100          |
| **Write**  | 2             | 010          |
| **Execute**| 1             | 001          |
| **No Permission** | 0        | 000          |

- **Owner permissions (first number)**, **Group permissions (second number)**, **Others permissions (third number)** are represented by these values.
  
For example, the permissions `rwxr-xr--` are converted to numbers as follows:
- Owner: `rwx` → `4 + 2 + 1 = 7`
- Group: `r-x` → `4 + 0 + 1 = 5`
- Others: `r--` → `4 + 0 + 0 = 4`

Thus, `rwxr-xr--` = **754** in numeric (octal) format.

### **Examples of Numeric and Binary Permissions**

1. **`777` (rwxrwxrwx)**  
   - **Owner**: rwx (7)  
   - **Group**: rwx (7)  
   - **Others**: rwx (7)
   - **Binary**: 111 111 111  
   - **Meaning**: All users can read, write, and execute the file.

2. **`755` (rwxr-xr-x)**  
   - **Owner**: rwx (7)  
   - **Group**: r-x (5)  
   - **Others**: r-x (5)
   - **Binary**: 111 101 101  
   - **Meaning**: The owner can read, write, and execute, while the group and others can only read and execute.

3. **`644` (rw-r--r--)**  
   - **Owner**: rw- (6)  
   - **Group**: r-- (4)  
   - **Others**: r-- (4)
   - **Binary**: 110 100 100  
   - **Meaning**: The owner can read and write, while the group and others can only read.

4. **`700` (rwx------)**  
   - **Owner**: rwx (7)  
   - **Group**: --- (0)  
   - **Others**: --- (0)
   - **Binary**: 111 000 000  
   - **Meaning**: Only the owner has full permissions; the group and others have no permissions.

5. **`444` (r--r--r--)**  
   - **Owner**: r-- (4)  
   - **Group**: r-- (4)  
   - **Others**: r-- (4)
   - **Binary**: 100 100 100  
   - **Meaning**: All users can read the file, but no one can write to or execute it.

### **How to Change Permissions with `chmod`**

You can use the `chmod` command to modify file permissions in either symbolic or numeric form.

1. **Using Symbolic Mode**:
   - Example: `chmod u+x file1.txt` (adds execute permission to the owner).
   - Example: `chmod g-w file1.txt` (removes write permission from the group).

2. **Using Numeric Mode**:
   - Example: `chmod 755 file1.txt` (sets the permissions to `rwxr-xr-x`).

---


---

# 🧩 Case Study: Secure Project Directory Access Using chmod

## 🎯 Scenario

You’re a system admin managing a project directory:

* Owner: `raman`
* Another user: `manoj` (normal user, no sudo)
* Goal: Control **who can enter the directory** and **who can execute a script**

---

# 🧱 Step 1: Create project directory

```bash
mkdir /home/raman/myproject
```

---

# 👤 Step 2: Set ownership to raman

```bash
chown raman:raman /home/raman/myproject
```

---

# 👥 Step 3: Create a normal user (manoj)

```bash
useradd manoj
passwd manoj
```

👉 `manoj` is a regular user (not in sudo/wheel)

---

# 📄 Step 4: Create script file

```bash
touch /home/raman/myproject/backup.sh
chown raman:raman /home/raman/myproject/backup.sh
```

---

# ✍️ Step 5: Add content to script

```bash
vi /home/raman/myproject/backup.sh
```

Add:

```bash
echo "Started Backup"
sleep 5
echo "Backup is completed"
```

---

# 🚫 Step 6: Remove execute permission on directory

```bash
chmod 666 /home/raman/myproject
```

👉 Permissions now:

* No **execute (x)** → cannot enter directory

---

# 🔍 Step 7: Test as manoj

```bash
su - manoj
cd /home/raman/myproject
```

❌ Output:

```bash
Permission denied
```

👉 **Reason:**
Without `x` (execute) on a directory → user cannot `cd` into it

---

# ✅ Step 8: Allow directory access (execute for others)

Switch back to root or raman:

```bash
chmod o+x /home/raman/myproject
```

---

# 🔍 Step 9: Test again as manoj

```bash
su - manoj
cd /home/raman/myproject
```

✅ Now it works

👉 **Why?**

* `x` on directory = permission to enter/traverse

---

# 🔐 Step 10: Restrict script execution

Give execute only to owner & group:

```bash
chmod 750 /home/raman/myproject/backup.sh
```

👉 Permissions:

* Owner (raman) → full
* Group → read + execute
* Others → no access

---

# 🚫 Step 11: Test execution as manoj (before group access)

```bash
su - manoj
/home/raman/myproject/backup.sh
```

❌ Output:

```bash
Permission denied
```

👉 Because:

* `manoj` is **not in raman group**
* Others have **no execute permission**

---

# 👥 Step 12: Add manoj to raman group

```bash
usermod -aG raman manoj
```

---

# 🔄 Step 13: Refresh session

```bash
su - manoj
```

---

# ▶️ Step 14: Run script again

```bash
/home/raman/myproject/backup.sh
```

✅ Output:

```bash
Started Backup
(wait 5 seconds)
Backup is completed
```

---

# 🧠 Key Learnings

## 🔑 Directory permissions

* `r` → list files
* `x` → enter directory (**most important**)

👉 Without `x`, even if `r` exists → ❌ cannot access

---

## 🔑 File permissions

* `x` → required to run script
* Controlled via:

```bash
chmod 750 backup.sh
```

---

## 🔑 Group-based access control

Instead of giving access to everyone:

* Add users to group
* Control via group permissions

---

## 🔑 Real-world use case

This setup is commonly used for:

* Dev teams sharing scripts
* Controlled access environments
* Secure automation systems

---


### **Case Study: Managing File Permissions in a Linux System**

In this case study, we will explore file permissions, ownership, and how to manage them using the `chown`, `chgrp`, and `chmod` commands. We will create files, change their permissions, modify ownership, and analyze the effects of these changes in a simulated environment.

---

### **Scenario Overview**

You are working as a system administrator for a project where multiple users collaborate. You need to ensure that files are accessible only to the intended users or groups while securing sensitive data from unauthorized access. Below are the steps involved in managing file ownership and permissions.

---

### **Step 1: Creating Files and Directories**

You are tasked with creating a directory structure for the project.

1. Create a directory called `project_data`:
   ```bash
   mkdir project_data
   ```

2. Inside `project_data`, create three files for different purposes:
   ```bash
   touch project_data/report.txt
   touch project_data/code.py
   touch project_data/backup.sh
   ```

---

### **Step 2: Viewing Default Permissions**

Now, list the files in `project_data` and their default permissions.

```bash
ls -l project_data/
```

Example output:
```
drwxr-xr-x 2 root root 4096 Dec  7 12:00 .
-rw-r--r-- 1 root root    0 Dec  7 12:00 report.txt
-rw-r--r-- 1 root root    0 Dec  7 12:00 code.py
-rw-r--r-- 1 root root    0 Dec  7 12:00 backup.sh
```

Explanation:
- **`rwxr-xr-x`** on the directory (`project_data`) means:
  - Owner (root): Read, write, execute
  - Group (root): Read, execute
  - Others: Read, execute

- **`rw-r--r--`** on the files means:
  - Owner (root): Read, write
  - Group (root): Read
  - Others: Read

---

### **Step 3: Changing Group Ownership Using `chgrp`**

You decide to assign a new group (`developers`) to the `project_data` directory and its files. The `developers` group should have write access to all the files in the project.

1. Change the group of the `project_data` directory:
   ```bash
   chgrp developers project_data
   ```

2. Change the group ownership of all files inside `project_data`:
   ```bash
   chgrp developers project_data/*
   ```

3. Check the group ownership again:
   ```bash
   ls -l project_data/
   ```

Example output:
```
drwxr-xr-x 2 root developers 4096 Dec  7 12:00 .
-rw-r--r-- 1 root developers    0 Dec  7 12:00 report.txt
-rw-r--r-- 1 root developers    0 Dec  7 12:00 code.py
-rw-r--r-- 1 root developers    0 Dec  7 12:00 backup.sh
```

Explanation: 
- The group ownership of all files is now `developers`.

---

### **Step 4: Modifying Permissions Using `chmod`**

You now need to modify the permissions to allow the `developers` group to modify the files while ensuring that others cannot edit them.

1. Change the permissions of the files so that the owner can read/write, the group can read/write, and others can only read:
   ```bash
   chmod 664 project_data/*
   ```

2. Verify the permissions:
   ```bash
   ls -l project_data/
   ```

Example output:
```
drwxr-xr-x 2 root developers 4096 Dec  7 12:00 .
-rw-rw-r-- 1 root developers    0 Dec  7 12:00 report.txt
-rw-rw-r-- 1 root developers    0 Dec  7 12:00 code.py
-rw-rw-r-- 1 root developers    0 Dec  7 12:00 backup.sh
```

Explanation:
- **`rw-rw-r--`** means:
  - Owner (root): Read, write
  - Group (developers): Read, write
  - Others: Read

---

### **Step 5: Using `chmod` to Set Special Permissions**

Next, you decide that the `backup.sh` file should be executable, but you want to make sure that no one except the owner can modify it.

1. Make `backup.sh` executable:
   ```bash
   chmod u+x project_data/backup.sh
   ```

2. Remove the write permission for others:
   ```bash
   chmod o-w project_data/backup.sh
   ```

3. Check the permissions again:
   ```bash
   ls -l project_data/
   ```

Example output:
```
drwxr-xr-x 2 root developers 4096 Dec  7 12:00 .
-rw-rw-r-- 1 root developers    0 Dec  7 12:00 report.txt
-rw-rw-r-- 1 root developers    0 Dec  7 12:00 code.py
-rwxr-xr-- 1 root developers    0 Dec  7 12:00 backup.sh
```

Explanation:
- **`rwxr-xr--`** on `backup.sh` means:
  - Owner (root): Read, write, execute
  - Group (developers): Read, execute
  - Others: Read

---

### **Step 6: Numeric Representation of Permissions**

Let’s verify and apply numeric (octal) permissions. We want to change the permissions of `report.txt` to allow the owner full access (read, write, execute), the group to read and execute, and others to have no permissions.

1. Change the permissions numerically:
   ```bash
   chmod 750 project_data/report.txt
   ```

2. Verify the permissions:
   ```bash
   ls -l project_data/
   ```

Example output:
```
drwxr-xr-x 2 root developers 4096 Dec  7 12:00 .
-rwxr-x--- 1 root developers    0 Dec  7 12:00 report.txt
-rw-rw-r-- 1 root developers    0 Dec  7 12:00 code.py
-rwxr-xr-- 1 root developers    0 Dec  7 12:00 backup.sh
```

Explanation:
- **`rwxr-x---`** means:
  - Owner (root): Read, write, execute
  - Group (developers): Read, execute
  - Others: No permissions

---

### **Step 7: Applying Recursive Changes**

Suppose you have a large directory structure and want to change the group and permissions for all files and directories within `project_data` recursively.

1. Change the group recursively:
   ```bash
   chgrp -R developers project_data/
   ```

2. Change the permissions recursively to give read/write permissions to the owner and group, and read-only permissions to others:
   ```bash
   chmod -R 775 project_data/
   ```

3. Verify the changes:
   ```bash
   ls -l project_data/
   ```

---

###  Directory Execute Permission

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

