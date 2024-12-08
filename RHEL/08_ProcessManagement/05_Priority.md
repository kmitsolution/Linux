In Red Hat Enterprise Linux (RHEL) and other Linux-based systems, processes have **priorities** that determine how much CPU time they receive relative to other processes. The priority of a process in Linux is managed primarily through the **nice value** and **real-time scheduling policies**.

### 1. **Process Priority in Linux (RHEL)**

There are two main ways that Linux handles process priority:

#### **1.1 Nice Value**
- The **nice value** is a user-level mechanism that affects the priority of a process. It influences how the Linux scheduler allocates CPU time.
- **Range**: The nice value can range from **-20** (highest priority) to **+19** (lowest priority). The default nice value is **0**.

- A process with a lower nice value (e.g., -20) will get more CPU time than a process with a higher nice value (e.g., +19).
- The nice value can be adjusted for **non-real-time processes**.

#### **1.2 Real-Time Priority**
- Real-time processes are given the highest priority by the scheduler.
- **Priority Range**: Real-time tasks can have priorities from **1 to 99**, with **1** being the lowest and **99** being the highest.
- These processes are usually for time-sensitive tasks, such as audio or video streaming, system control, and more.

### 2. **Finding the Priority of a Process**

To find the priority of a process, we typically use the `ps` command or the `top` command.

#### **2.1 Using `ps` Command**
You can use the `ps` command to see the priority of a process by checking its **priority** (PRI) and **nice** (NI) values.

```bash
ps -eo pid,comm,pri,ni
```

- **PRI**: The actual priority of the process, which is influenced by the nice value and real-time priority.
- **NI**: The nice value of the process.

Example:

```bash
ps -eo pid,comm,pri,ni
```

Output:
```
PID  COMMAND       PRI NI
1234 bash          20  0
5678 vim           19  5
9101 sshd          10 -5
```

#### **2.2 Using `top` Command**
The `top` command provides real-time monitoring of processes, including their priorities.

1. Run `top`:
   ```bash
   top
   ```

2. The output will show various columns, including:
   - **PR**: The process priority.
   - **NI**: The nice value.

For example, in the `top` output:
```
PID  USER    PR  NI  VIRT  RES  SHR S %CPU %MEM     TIME+ COMMAND
1234 root    20   0  16876  1324  976 S  0.0  0.1   0:03.20 /sbin/init
5678 user    19   5  12345  2345 1245 S  0.0  1.2   0:01.40 vim
9101 root    10  -5  12345  6789 1234 S  0.0  0.1   0:00.15 sshd
```
- **PR**: The priority of the process (e.g., 20, 19, 10).
- **NI**: The nice value of the process (e.g., 0, 5, -5).

### 3. **Setting the Priority of a Process**

You can set the priority of a process using the **nice** and **renice** commands.

#### **3.1 Setting Priority for a New Process with `nice`**
To start a process with a specific nice value, use the `nice` command.

Syntax:
```bash
nice -n <nice_value> <command>
```
- The `<nice_value>` is the nice value you want to assign to the process.
- A **negative nice value** increases the priority, and a **positive nice value** decreases the priority.

Example: Start a process with a nice value of `-10` (higher priority):
```bash
nice -n -10 vi
```

#### **3.2 Changing Priority of an Existing Process with `renice`**
To change the priority of an already running process, you can use the `renice` command.

Syntax:
```bash
renice -n <nice_value> -p <pid>
```
- **<nice_value>**: The new nice value you want to set for the process.
- **<pid>**: The Process ID (PID) of the process.

Example: Change the nice value of a process with PID 1234 to `+10` (lower priority):
```bash
renice -n 10 -p 1234
```

If you want to change the priority of a process owned by another user (as a root user), you can specify the user with the `-u` option:
```bash
renice -n 5 -u username
```

#### **3.3 Setting Real-Time Priority**
The `chrt` command in Linux is used to manipulate the real-time scheduling attributes of a process. This command allows you to **set or get the real-time priority** and **scheduling policy** of a running process or a new process. Real-time processes are given higher priority than normal processes, which is useful for tasks that are time-sensitive (e.g., audio processing or system control tasks).

### **Syntax of `chrt` Command:**
```bash
chrt [options] <priority> <command> [args...]
```

- **`<priority>`**: The real-time priority value (between 1 and 99, where 99 is the highest priority).
- **`<command>`**: The command to run the process with the specified scheduling policy and priority.
- **`[args...]`**: Arguments for the command, if applicable.

### **Real-Time Scheduling Policies in `chrt`:**
The `chrt` command allows you to select between two main real-time scheduling policies:
1. **`SCHED_FIFO` (First-In-First-Out)**: Processes are scheduled in a simple first-come, first-served manner, and once a process starts running, it runs to completion unless it voluntarily gives up the CPU.
2. **`SCHED_RR` (Round-Robin)**: Similar to `SCHED_FIFO`, but it allows processes to get a time slice to run. After a time slice is over, the process is re-queued, and another process gets a chance to run.

By default, most processes are scheduled using the **SCHED_OTHER** policy, which is the default time-sharing policy for non-real-time tasks.

### **Options for the `chrt` Command:**
- **`-f`**: Specifies **FIFO scheduling policy**.
- **`-r`**: Specifies **Round-Robin scheduling policy**.
- **`-p`**: Specifies that you want to **set the priority of an existing process** by providing its **PID** (Process ID).
- **`-v`**: Enables verbose mode, showing more detailed output.

### **1. Set the Real-Time Priority of a New Process:**
```bash
chrt -m # to list all Real Time Priority
```
To start a new process with a specific real-time priority, use `chrt` with the `-f` or `-r` option, depending on the scheduling policy you want to use.

#### Example: Set the **FIFO** priority to 50 for a new process:
```bash
chrt -f 50 myprocess
```
- **`-f`**: Sets the real-time FIFO scheduling policy.
- **`50`**: Sets the priority (between 1 and 99, with 99 being the highest).
- **`myprocess`**: The command or program you want to run.

#### Example: Set the **Round-Robin** priority to 80 for a new process:
```bash
chrt -r 80 myprocess
```
- **`-r`**: Sets the real-time Round-Robin scheduling policy.

### **2. Change the Scheduling Policy and Priority of a Running Process:**

You can also change the priority and scheduling policy of an existing process by providing its **PID**.

#### Example: Change the real-time FIFO priority of a running process (with PID 1234) to 60:
```bash
chrt -f 60 -p 1234
```
- **`-f`**: Specifies FIFO scheduling policy.
- **`-p`**: Indicates that you're modifying the priority of an existing process.
- **`1234`**: The PID of the process you want to modify.
- **`60`**: The new priority.

#### Example: Change the real-time Round-Robin priority of a running process (with PID 5678) to 90:
```bash
chrt -r 90 -p 5678
```
- **`-r`**: Specifies Round-Robin scheduling policy.
- **`-p`**: Modifies the priority of the running process.

### **3. View the Current Scheduling Policy and Priority of a Process:**

To view the scheduling policy and priority of a running process, you can use the `chrt -p` option.

#### Example: Check the scheduling policy and priority of a running process with PID 1234:
```bash
chrt -p 1234
```

Output:
```
pid 1234's current scheduling policy: SCHED_FIFO
pid 1234's current priority: 50
```
- This will show the current scheduling policy (e.g., **SCHED_FIFO** or **SCHED_RR**) and the current priority of the process with the given **PID**.

### **4. Viewing Available Real-Time Priorities:**

Real-time priorities range from **1 to 99**, with **99** being the highest priority. To see the available range for your system, you can check the system limits:


The real-time priority range is usually defined by the system settings (typically 1-99 for real-time scheduling).

### **5. Example: Start a High-Priority Real-Time Process:**

#### Example: Start a real-time process with the highest priority (99) using the FIFO scheduling policy:
```bash
chrt -f 99 myprocess
```

This will run `myprocess` with a real-time FIFO scheduling policy and the highest priority.

---

### **Summary of `chrt` Command:**

| **Option** | **Description** |
|------------|-----------------|
| `-f`       | Set **FIFO (First-In-First-Out)** scheduling policy. |
| `-r`       | Set **Round-Robin** scheduling policy. |
| `-p`       | Change the priority of an existing process by **PID**. |
| `-v`       | Enable **verbose mode** to show detailed output. |

- **Real-Time Priority Range**: 1 to 99 (with **99** being the highest priority).
- **Scheduling Policies**:
  - **SCHED_FIFO**: First-In, First-Out.
  - **SCHED_RR**: Round-Robin.
  
You can use the `chrt` command to set high-priority scheduling for critical tasks, ensuring they get more CPU time compared to regular processes.

Let me know if you need more details on any specific part of the `chrt` command!
### Summary:

- **Priority** in Linux is determined by the **nice value** and, for real-time tasks, **real-time priorities**.
- **Nice values** range from `-20` (highest priority) to `+19` (lowest priority).
- Use `ps -eo pid,comm,pri,ni` or `top` to find the priority of processes.
- Use **`nice`** to set the priority of a new process and **`renice`** to change the priority of a running process.
- For real-time processes, use the **`chrt`** command to set real-time priorities.

Let me know if you need any more information on this topic!
