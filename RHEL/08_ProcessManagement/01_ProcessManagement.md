### **Process Management in RHEL**

In Red Hat Enterprise Linux (RHEL), processes are managed by the operating system through a variety of commands and utilities. Understanding processes, their IDs, parent-child relationships, and how to manage or terminate them is essential for system administration. Below is a detailed explanation of key terms and concepts related to process management in RHEL.

---
The `ps` command in **Red Hat Enterprise Linux (RHEL)** is used to **display information about currently running processes** on the system.

---

## 🔧 What `ps` does

It shows a snapshot of processes, including:

* Process ID (PID)
* User running the process
* CPU & memory usage
* Terminal (TTY)
* Command that started the process

---

## 🧾 Basic Syntax

```bash
ps [options]
```

---

## 📌 Commonly Used `ps` Commands

### 1. Show processes for current shell

```bash
ps
```

Displays processes associated with your current terminal session.

---

### 2. Show all processes (Linux style)

```bash
ps -e
```

or

```bash
ps -A
```

---

### 3. Detailed full-format listing

```bash
ps -ef
```

* `-e` → all processes
* `-f` → full details (UID, PID, PPID, start time, command)

That line is the **header row** from the `ps -ef` (or similar) command output. Each column tells you specific information about processes in Red Hat Enterprise Linux.

Let’s break it down clearly:

---

## 🧾 Column Explanation

```
UID   PID   PPID   C   STIME   TTY   TIME   CMD
```

### 🔹 UID (User ID)

* The **user who owns the process**
* Example: `root`, `oracle`, `user1`

---

### 🔹 PID (Process ID)

* Unique **identifier of the process**
* Every running process has its own PID

---

### 🔹 PPID (Parent Process ID)

* The **PID of the parent process**
* Shows which process started this one

---

### 🔹 C (CPU usage)

* Represents **CPU utilization (recent usage)**
* Usually a small number (0–99)

---

### 🔹 STIME (Start Time)

* When the process started
* Example:

  * `10:30` → started today
  * `Apr21` → started earlier date

---

### 🔹 TTY (Terminal)

* Terminal associated with the process
* Examples:

  * `pts/0` → SSH or terminal session
  * `tty1` → physical console
  * `?` → no terminal (background/daemon)

---

### 🔹 TIME (CPU Time Used)

* Total CPU time consumed by the process
* Format: `minutes:seconds`

---

### 🔹 CMD (Command)

* The **actual command/program** that started the process
* Example:

  * `bash`
  * `sshd`
  * `httpd`

---

## 📌 Example

```
root   1   0   0   10:00   ?   00:00:02   systemd
```

👉 Meaning:

* Run by **root**
* PID = 1 (first process)
* No terminal (`?`)
* Started at 10:00
* Command = `systemd`

---
### 4. Search for a specific process

```bash
ps -ef | grep nginx
```

---

### 5. Tree view (process hierarchy)

```bash
ps -ef --forest
```

---

## 📊 Example Output Explained

```bash
UID   PID  PPID  C STIME TTY   TIME CMD
root    1     0  0 10:00 ?     00:00:02 systemd
```

### 6. User-friendly format

```bash
ps aux
```

* `a` → processes for all users
* `u` → user-oriented format
* `x` → includes processes without a terminal

That header comes from commands like `ps aux` in Red Hat Enterprise Linux. It shows a **more detailed, user-friendly view of processes**.

Let’s decode each column:

---

## 🧾 Column Breakdown

```bash id="r8l6l2"
USER   PID  %CPU  %MEM   VSZ   RSS   TTY   STAT   START   TIME   COMMAND
```

---

### 🔹 USER

* The **owner of the process**
* Example: `root`, `oracle`, `ec2-user`

---

### 🔹 PID (Process ID)

* Unique ID of the process

---

### 🔹 %CPU

* **CPU usage percentage**
* Shows how much CPU the process is using right now

---

### 🔹 %MEM

* **Memory usage percentage**
* Percentage of RAM used by the process

---

### 🔹 VSZ (Virtual Size)

* Total **virtual memory** used (in KB)
* Includes all memory (used + swapped + allocated)

---

### 🔹 RSS (Resident Set Size)

* Actual **physical RAM used** (in KB)
* This is the real memory currently in RAM

---

### 🔹 TTY

* Terminal associated with the process
* `pts/0` → terminal
* `?` → no terminal (background process)

---

### 🔹 STAT (Process State)

Shows current status of the process:

Common values:

* `R` → Running
* `S` → Sleeping
* `D` → Uninterruptible sleep (I/O wait)
* `T` → Stopped
* `Z` → Zombie

Extra flags:

* `s` → session leader
* `l` → multithreaded
* `+` → foreground process

Example: `Ss`, `R+`

---

### 🔹 START

* Time or date when process started

---

### 🔹 TIME

* Total **CPU time used** since start

---

### 🔹 COMMAND

* Full command with arguments that started the process

---

## 📌 Example

```bash id="2avij0"
root   1023  0.0  1.2  225000  5000  ?  Ss  10:00  0:01  sshd
```

👉 Meaning:

* Owned by `root`
* Using 0.0% CPU, 1.2% memory
* Running as a background service (`?`)
* Status = sleeping (`Ss`)
* Command = `sshd`

---




### **1. Process ID (PID)**

- Each process running in a Linux system is assigned a unique **Process ID (PID)**. This is a number used to identify the process in the system.
- To view the PID of a running process, you can use the `ps` command:

   ```bash
   ps aux
   ```

   This will list all running processes along with their PIDs.

---

### **2. Parent Process ID (PPID)**

- The **Parent Process ID (PPID)** refers to the ID of the process that started or "parented" another process.
- A process can have a PPID, which identifies the parent process from which it was spawned. You can see the PPID in the `ps` command output.

   Example:
   ```bash
   ps -ef
   ```

   In the output, the second column typically shows the PPID (Parent Process ID).

---

### **3. Parent Process ID is Less Than Child Process ID**

- The **Parent Process ID (PPID)** is usually smaller than the **Child Process ID (PID)**. This is because processes are typically created sequentially. The first process, which is `init` (or `systemd`), has the PID 1, and any new processes it spawns will have higher PIDs.
- The relationship between a parent process and its child process is maintained by their respective PIDs, and the parent process is responsible for managing the child processes.

---

### **4. `init` or `systemd` - The First Process Started**

- On most modern Linux systems, **`systemd`** is the first process that is started by the kernel during the boot process, and it has a PID of 1.
- **`init`** was traditionally the first process in older Linux distributions. `systemd` replaced `init` in many distributions, but the concept remains the same: **PID 1** is responsible for starting and managing the system's processes.
- The `init` or `systemd` process has no parent because it is the first process started by the kernel.

   Example:
   ```bash
   ps -ef | grep systemd
   ```

   This will show the `systemd` process and its PID.

---

### **5. Process Kill Meaning**

- To **kill a process** means to terminate it, which can be done using the `kill` command followed by the PID of the process.
  
   Example:
   ```bash
   kill 1234
   ```

   - The number `1234` is the PID of the process you want to terminate. This sends a **SIGTERM** signal to the process, asking it to gracefully terminate.

   If a process does not terminate with the SIGTERM signal, you can use the **SIGKILL** signal:

   ```bash
   kill -9 1234
   ```

   The **SIGKILL (9)** signal immediately terminates the process without allowing it to clean up.

---

### **6. Daemon Process**

- A **daemon process** is a background process that starts automatically when the system boots up and usually runs continuously without interaction from users. Daemons typically provide system services or handle background tasks.
- Examples of daemon processes include web servers (e.g., `httpd`), database servers (e.g., `mysqld`), and system logging daemons (e.g., `syslog`).
- Daemons are designed to run independently of users, and they are often managed by **`systemd`** or **`init`** (in older systems).
- Daemons cannot be killed easily since they are configured to restart automatically if terminated. They often include mechanisms for restarting when needed.

   Example:
   - To view daemon processes running on your system:
     ```bash
     ps aux | grep -i daemon
     ```

   - You can check if a daemon is managed by `systemd` using:
     ```bash
     systemctl status daemon_name
     ```

---

### **7. Zombie Process**

- A **zombie process** is a process that has completed execution, but still has an entry in the process table. It is in the **zombie state** because its parent process has not read its exit status (it has not yet "reaped" it).
- Even though the process has terminated, the system keeps some information about it until the parent process reads its status using the **`wait()`** system call.
- Zombie processes don't consume CPU resources, but they still occupy an entry in the process table. If too many zombie processes accumulate, it could lead to resource exhaustion.

   Example:
   - To check for zombie processes, you can use the `ps` command:
     ```bash
     ps aux | grep 'Z'
     ```
   
   - To remove a zombie process, you need to kill its parent process (PPID). Once the parent process reads the exit status of its child, the zombie will be removed from the process table.

---

### **Summary of Key Terms**

| **Term**               | **Description**                                                                                                                                                   |
|------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Process ID (PID)**   | A unique identifier assigned to every running process in the system.                                                                                             |
| **Parent Process ID (PPID)** | The PID of the process that started or "parented" the current process.                                                                                           |
| **`init` or `systemd`** | The first process started by the kernel at boot (PID=1), which is responsible for launching and managing other processes.                                          |
| **Kill Process**       | The action of terminating a process, typically using the `kill` command. A process can be terminated with signals like **SIGTERM (15)** or **SIGKILL (9)**.         |
| **Daemon Process**     | A background process that starts automatically at boot and runs continuously, often without user interaction.                                                     |
| **Zombie Process**     | A process that has finished executing but still exists in the process table because its parent has not yet read its exit status. It does not consume resources but occupies an entry in the process table. |

---

### **Example Commands for Process Management**

- **View All Processes:**
  ```bash
  ps aux
  ```

- **Find a Process by Name (e.g., `httpd`):**
  ```bash
  ps aux | grep httpd
  ```

- **Kill a Process (Graceful Termination):**
  ```bash
  kill <PID>
  ```

- **Kill a Process (Forceful Termination):**
  ```bash
  kill -9 <PID>
  ```

- **View the Parent Process of a Given Process:**
  ```bash
  ps -o ppid= -p <PID>
  ```

- **View Processes and Their Parent-Child Relationship:**
  ```bash
  pstree
  ```

By understanding process management and concepts like **PID**, **PPID**, **zombie processes**, **daemon processes**, and how to kill or terminate processes, you can efficiently manage processes in a Red Hat Enterprise Linux environment.
