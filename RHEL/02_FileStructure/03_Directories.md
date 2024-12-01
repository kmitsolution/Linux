In RHEL (Red Hat Enterprise Linux) and other Linux distributions, directory and file management are essential tasks that can be performed using several commands. Below is a list of **basic and advanced directory commands** in Linux, including `mkdir`, `cd`, `rmdir`, and `rm`.

### **1. `mkdir` (Make Directory)**

The `mkdir` command is used to create new directories.

#### **Basic Usage:**
```bash
mkdir directory_name
```
- **Example:**
```bash
mkdir myfolder
```
This creates a directory named `myfolder` in the current directory.

#### **Advanced Usage:**

- **Create multiple directories at once:**
```bash
mkdir dir1 dir2 dir3
```
This creates three directories (`dir1`, `dir2`, `dir3`) in the current directory.

- **Create parent directories if they do not exist (use `-p` option):**
```bash
mkdir -p /home/user/docs/projects
```
This creates the directory `/home/user/docs/projects` and its parent directories (`/home/user/docs` and `/home/user`) if they don't already exist.

- **Create directories with specific permissions:**
```bash
mkdir -m 755 newdir
```
This creates the directory `newdir` with `755` permissions (read/write/execute for owner, and read/execute for others).

---

### **2. `cd` (Change Directory)**

The `cd` command is used to navigate between directories.

#### **Basic Usage:**
```bash
cd directory_name
```
- **Example:**
```bash
cd myfolder
```
This changes the current directory to `myfolder`.

#### **Advanced Usage:**

- **Navigate to the home directory:**
```bash
cd ~
```
This takes you to your home directory.

- **Navigate to the parent directory:**
```bash
cd ..
```
This moves you one level up from the current directory.

- **Navigate to the previous directory:**
```bash
cd -
```
This takes you to the directory you were in immediately before your current directory.

- **Absolute path navigation:**
```bash
cd /home/user/docs
```
This moves you directly to `/home/user/docs` from anywhere in the filesystem.

---

### **3. `rmdir` (Remove Directory)**

The `rmdir` command is used to remove empty directories.

#### **Basic Usage:**
```bash
rmdir directory_name
```
- **Example:**
```bash
rmdir myfolder
```
This removes the empty directory `myfolder`.

#### **Advanced Usage:**

- **Remove multiple directories at once:**
```bash
rmdir dir1 dir2 dir3
```
This removes the directories `dir1`, `dir2`, and `dir3` (if they are empty).

- **Remove a directory and its parent directories (if they are empty) using `-p`:**
```bash
rmdir -p /home/user/docs/myfolder
```
This removes `myfolder` as well as any parent directories that are empty.

---

### **4. `rm` (Remove File or Directory)**

The `rm` command is used to remove files and directories. **Caution**: `rm` is a powerful command and can permanently delete data.

#### **Basic Usage:**
```bash
rm filename
```
- **Example:**
```bash
rm file.txt
```
This deletes the file `file.txt`.

#### **Advanced Usage:**

- **Remove a directory and its contents using `-r` (recursive):**
```bash
rm -r myfolder
```
This removes the directory `myfolder` and all of its contents (files and subdirectories).

- **Remove write-protected files without prompting (use `-f` for force):**
```bash
rm -rf myfolder
```
This deletes the directory `myfolder` and all its contents without asking for confirmation, even if files are write-protected.

- **Prompt before deleting each file:**
```bash
rm -i myfolder/*
```
This prompts you to confirm the deletion of each file in `myfolder`.

- **Remove all files with a specific pattern (e.g., all `.txt` files):**
```bash
rm *.txt
```
This removes all `.txt` files in the current directory.

---

### **Other Directory-Related Commands:**

#### **5. `ls` (List Directory Contents)**
   - The `ls` command is used to list the contents of a directory.
   - **Basic Usage:**
     ```bash
     ls
     ```
   - **Advanced Usage:**
     ```bash
     ls -l        # List in long format (shows permissions, owner, etc.)
     ls -a        # List all files, including hidden files
     ls -lh       # List in long format with human-readable file sizes
     ls -R        # List directories recursively
     ```

#### **6. `pwd` (Print Working Directory)**
   - The `pwd` command shows the current working directory.
   - **Example:**
     ```bash
     pwd
     ```

---

### **Summary Table of Commands:**

| Command        | Description                                                               | Example Usage                                        |
|----------------|---------------------------------------------------------------------------|------------------------------------------------------|
| `mkdir`        | Creates a new directory.                                                  | `mkdir newfolder`                                    |
| `cd`           | Changes the current working directory.                                    | `cd /home/user/`                                     |
| `rmdir`        | Removes an empty directory.                                               | `rmdir oldfolder`                                    |
| `rm`           | Removes files or directories (with optional `-r` for directories).        | `rm file.txt` / `rm -r folder`                       |
| `ls`           | Lists directory contents.                                                 | `ls` / `ls -l`                                       |
| `pwd`          | Prints the current working directory.                                     | `pwd`                                                |

---

### **Important Notes:**

- **Be Cautious with `rm -rf`**: The `rm -rf` command is very powerful and can delete a large number of files or directories without asking for confirmation. Always double-check the path before executing this command.
  
- **Use `man` to Learn More**: To get more details on each of these commands, use the `man` command (manual) for each command. For example:
  ```bash
  man mkdir
  man cd
  man rm
  ```

By mastering these basic and advanced directory commands, you'll be able to efficiently manage files and directories in RHEL and other Linux-based systems.
