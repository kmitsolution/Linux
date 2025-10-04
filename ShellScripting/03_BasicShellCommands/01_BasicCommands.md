✅ **Basic Shell Commands**

These are the **most commonly used commands** in shell scripting — they help you interact with files, directories, and user input.

---

### 🗣️ **1. `echo` – Display Text or Variables**

Used to **print output** to the terminal.

#### **Syntax:**

```bash
echo "Hello, World!"
echo "My name is $USER"
```

#### **Example Output:**

```
Hello, World!
My name is linuxuser
```

💡 You can also use `-e` to enable escape sequences:

```bash
echo -e "Line1\nLine2"
```

Output:

```
Line1
Line2
```

---

### 📥 **2. `read` – Take User Input**

Used to **read input** from the keyboard.

#### **Syntax:**

```bash
read name
echo "Hello, $name"
```

#### **Example:**

```
Enter your name: Alex
Hello, Alex
```

💡 Use `-p` for inline prompts:

```bash
read -p "Enter your age: " age
echo "You are $age years old."
```

---

### 📂 **3. `pwd` – Print Working Directory**

Displays your **current directory path**.

```bash
pwd
```

**Output:**

```
/home/linuxuser/scripts
```

---

### 🚶 **4. `cd` – Change Directory**

Used to **move** between directories.

#### **Examples:**

```bash
cd /home/user/Documents
cd ..          # Go up one directory
cd ~           # Go to home directory
cd /           # Go to root directory
```

💡 Always use absolute paths for scripts to avoid confusion.

---

### 📋 **5. `ls` – List Files and Directories**

Lists contents of the current (or specified) directory.

#### **Examples:**

```bash
ls              # Basic listing
ls -l           # Detailed view (permissions, size, etc.)
ls -a           # Show hidden files
ls -lh          # Human-readable file sizes
```

---

### 📄 **6. `cp` – Copy Files and Directories**

Used to **copy** files or folders.

#### **Syntax:**

```bash
cp source.txt destination.txt
cp -r folder1/ folder2/
```

* `-r` → recursive (for directories)

#### **Example:**

```bash
cp hello.sh /home/user/backup/
```

---

### 🔀 **7. `mv` – Move or Rename Files**

Used to **move** or **rename** files and directories.

#### **Examples:**

```bash
mv oldname.sh newname.sh      # Rename
mv script.sh /home/user/bin/  # Move to another folder
```

💡 `mv` will overwrite without warning; use `-i` to prompt:

```bash
mv -i file.txt backup/
```

---

### 🗑️ **8. `rm` – Remove Files or Directories**

Deletes files or directories permanently ⚠️

#### **Examples:**

```bash
rm file.txt              # Remove file
rm -i file.txt           # Ask before removing
rm -r folder/            # Remove directory recursively
rm -rf folder/           # Force delete (dangerous!)
```

⚠️ Be **very careful** with `rm -rf /` — it can delete your system.

---

### 📁 **9. `mkdir` – Make Directories**

Creates one or more new directories.

#### **Examples:**

```bash
mkdir test
mkdir -p projects/shell/scripts
```

* `-p` → create parent directories if they don’t exist.

---

### 🧾 **Summary Table**

| Command | Purpose                  | Example          |
| ------- | ------------------------ | ---------------- |
| `echo`  | Print text or variables  | `echo "Hello"`   |
| `read`  | Get user input           | `read name`      |
| `pwd`   | Show current directory   | `pwd`            |
| `cd`    | Change directory         | `cd /home/user`  |
| `ls`    | List files               | `ls -l`          |
| `cp`    | Copy files/directories   | `cp file1 file2` |
| `mv`    | Move or rename           | `mv old new`     |
| `rm`    | Remove files/directories | `rm -r dir`      |
| `mkdir` | Create new directories   | `mkdir newdir`   |

---


