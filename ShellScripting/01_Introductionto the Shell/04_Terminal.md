Sure! Here's a simple guide to **Terminal usage basics** for Linux systems like **Ubuntu**, **RHEL**, or any other UNIX-like OS.

---

## 🖥️ What Is the Terminal?

The **Terminal** is a text-based interface that allows you to interact with the system by typing commands. It uses a **shell** (commonly Bash) to interpret those commands.

---

## 🛠️ Basic Terminal Commands

Here are some **fundamental commands** every Linux user should know:

### 📁 File & Directory Navigation

| Command             | Description                         |
| ------------------- | ----------------------------------- |
| `pwd`               | Print the current directory         |
| `ls`                | List files in the current directory |
| `ls -l`             | List files with details             |
| `ls -a`             | Show hidden files                   |
| `cd <dir>`          | Change directory                    |
| `cd ..`             | Go up one directory                 |
| `cd ~` or just `cd` | Go to home directory                |

---

### 📄 File & Directory Management

| Command            | Description                         |
| ------------------ | ----------------------------------- |
| `touch <filename>` | Create a new empty file             |
| `mkdir <dirname>`  | Create a directory                  |
| `rm <filename>`    | Delete a file                       |
| `rm -r <dirname>`  | Delete a directory and its contents |
| `cp <src> <dest>`  | Copy a file or directory            |
| `mv <src> <dest>`  | Move or rename a file or directory  |

---

### 📂 Viewing Files

| Command          | Description                              |
| ---------------- | ---------------------------------------- |
| `cat <file>`     | Show the contents of a file              |
| `less <file>`    | Scroll through a file (use `q` to quit)  |
| `head <file>`    | Show the first 10 lines of a file        |
| `tail <file>`    | Show the last 10 lines of a file         |
| `tail -f <file>` | View file in real-time (useful for logs) |

---

### ⚙️ System Info & Processes

| Command         | Description                                  |
| --------------- | -------------------------------------------- |
| `whoami`        | Show your username                           |
| `hostname`      | Show the system hostname                     |
| `top` or `htop` | Real-time system monitor (press `q` to quit) |
| `ps`            | View running processes                       |
| `ps aux`        | Detailed view of all running processes       |
| `kill <PID>`    | Kill a process by ID                         |

---

### 📦 Installing Software

* **Ubuntu (APT)**:

  ```bash
  sudo apt update
  sudo apt install <package-name>
  ```

* **RHEL (DNF/YUM)**:

  ```bash
  sudo dnf install <package-name>
  ```

---

### 🔍 Searching & Finding Files

| Command                        | Description              |
| ------------------------------ | ------------------------ |
| `find <path> -name <filename>` | Find files by name       |
| `grep <pattern> <file>`        | Search for text in files |
| `grep -r <pattern> <dir>`      | Search recursively       |

---

### 🔐 Permissions & Ownership

| Command                       | Description                      |
| ----------------------------- | -------------------------------- |
| `chmod +x <file>`             | Make a script or file executable |
| `chmod 755 <file>`            | Set specific permissions         |
| `chown <user>:<group> <file>` | Change file ownership            |

---

### 🧰 Useful Shortcuts

| Shortcut   | Description                         |
| ---------- | ----------------------------------- |
| `Tab`      | Autocomplete file or command        |
| `Ctrl + C` | Cancel a running command            |
| `Ctrl + L` | Clear the screen                    |
| `Ctrl + D` | Logout from shell or close terminal |
| `!!`       | Repeat the last command             |
| `history`  | Show previously run commands        |
| `!n`       | Run command number `n` from history |

---

### 🗃️ Redirects & Pipes

| Symbol | Description                           |                                         |
| ------ | ------------------------------------- | --------------------------------------- |
| `>`    | Redirect output to a file (overwrite) |                                         |
| `>>`   | Redirect output to a file (append)    |                                         |
| `<`    | Take input from a file                |                                         |
| \`     | \`                                    | Pipe output from one command to another |

**Example**:

```bash
ls -l | grep ".txt"
```

→ List all `.txt` files in current directory.

---

### 🔄 Environment Variables

| Command            | Description                    |
| ------------------ | ------------------------------ |
| `echo $HOME`       | Show value of HOME variable    |
| `export VAR=value` | Set an environment variable    |
| `printenv`         | Show all environment variables |

---

## 🧪 Try This Mini Practice!

```bash
mkdir test
cd test
touch file1.txt file2.txt
ls
echo "Hello world" > file1.txt
cat file1.txt
cd ..
rm -r test
```

---

## 🧠 Tip for Beginners

* Don't be afraid of the terminal. Most Linux power users rely heavily on it.
* Use `man <command>` to read manual pages for any command (e.g., `man ls`).
* Practice regularly to build confidence.

---


