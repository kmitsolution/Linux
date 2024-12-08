### **1. `jobs` Command to See Background Processes:**

The `jobs` command in Linux is used to display the processes that are running in the background, specifically those that were started in the current shell session.

- **Syntax**:
  ```bash
  jobs
  ```

- **Example**:
  Suppose you have started a process in the background:
  ```bash
  sleep 100 &
  ```

  You can check the status of this background process by running:
  ```bash
  jobs
  ```

  **Output**:
  ```
  [1]+  1234  Running                 sleep 100 &
  ```

- **Explanation**: The output shows the job number (e.g., `[1]+`), the **PID** of the process (e.g., `1234`), the status (`Running`), and the command (`sleep 100 &`).
  - You can refer to background jobs by their job number (e.g., `%1` for job 1).

- **Use Case**: To see all the processes you have run in the background within the current shell session.

---

### **2. `kill` Command to Terminate Processes:**

The `kill` command is used to send signals to processes, such as terminating or pausing them.

#### **Basic Syntax**:
```bash
kill <PID>
```

- **Example**: To terminate a process with PID 1234:
  ```bash
  kill 1234
  ```

This sends the default signal (`SIGTERM`), which gracefully terminates the process.

---

### **3. `kill` with Different Signals:**

- **`kill` with `-9` (SIGKILL)**:  
  The `SIGKILL` signal forcefully terminates a process and cannot be caught or ignored by the process.
  ```bash
  kill -9 <PID>
  ```

- **Example**: Forcefully terminate a process with PID `1234`:
  ```bash
  kill -9 1234
  ```
  - **Output**: The process is killed immediately without any cleanup.

- **`kill` with `-15` (SIGTERM, default)**:  
  The default signal is `SIGTERM`, which asks the process to terminate gracefully.
  ```bash
  kill 1234
  ```

#### **Other Signals:**
- **SIGCONT**: Resume a process that has been stopped.
  - **Example**: To resume a process:
    ```bash
    kill -CONT <PID>
    ```

- **SIGSTOP**: Stop a running process (this is like pausing the process).
  - **Example**: To stop a process:
    ```bash
    kill -STOP <PID>
    ```

#### **`kill -l`**:  
This option lists all available signals for the `kill` command.

- **Example**:
  ```bash
  kill -l
  ```

  **Output**:
  ```
  1) SIGHUP     2) SIGINT     3) SIGQUIT    4) SIGILL     5) SIGTRAP
  6) SIGABRT    7) SIGBUS     8) SIGFPE     9) SIGKILL    10) SIGUSR1
  11) SIGUSR2   12) SIGSEGV    13) SIGPIPE    14) SIGALRM    15) SIGTERM
  16) SIGSTKFLT 17) SIGCHLD    18) SIGCONT    19) SIGSTOP    20) SIGTSTP
  21) SIGTTIN    22) SIGTTOU    23) SIGURG     24) SIGXCPU    25) SIGXFSZ
  26) SIGVTALRM 27) SIGPROF    28) SIGWINCH   29) SIGIO      30) SIGPWR
  31) SIGSYS     32) SIGRTMIN   33) SIGRTMIN+1 ...
  ```

  This list shows all the available signals that can be sent to a process. Each signal has a number and a corresponding name.

---

### **4. `kill -19` and `kill -18`:**

- **`kill -19`**:  
  Sends the **SIGSTOP** signal (signal number 19), which pauses (stops) a process.
  ```bash
  kill -19 <PID>
  ```
  - **Use Case**: To pause a process.

- **`kill -18`**:  
  Sends the **SIGCONT** signal (signal number 18), which resumes a paused process.
  ```bash
  kill -18 <PID>
  ```
  - **Use Case**: To resume a process that was paused with `SIGSTOP`.

---

### **5. `killall` Command to Terminate Processes by Name:**

The `killall` command is used to terminate processes by their **name** instead of by their PID.

- **Syntax**:
  ```bash
  killall <process_name>
  ```

- **Example**: To kill all `sleep` processes:
  ```bash
  killall sleep
  ```

  This will terminate all processes named `sleep`, regardless of their PID.

#### **Options with `killall`:**
- **`killall -9 <process_name>`**: Forcefully terminate all processes by name using `SIGKILL`.
  ```bash
  killall -9 sleep
  ```

- **`killall -15 <process_name>`**: Gracefully terminate all processes by name using `SIGTERM`.
  ```bash
  killall -15 sleep
  ```

---

### **6. `pkill` Command to Terminate Processes by Name:**

`pkill` is similar to `killall` but offers more flexibility in pattern matching. It can send signals to processes based on their name, user, group, or other attributes.

- **Syntax**:
  ```bash
  pkill <process_name>
  ```

- **Example**: To kill all `sleep` processes:
  ```bash
  pkill sleep
  ```

  This will send the default signal (SIGTERM) to all processes with the name `sleep`.

#### **Options with `pkill`:**
- **`pkill -9 <process_name>`**: Forcefully kill processes using `SIGKILL`.
  ```bash
  pkill -9 sleep
  ```

- **`pkill -15 <process_name>`**: Gracefully terminate processes using `SIGTERM`.
  ```bash
  pkill -15 sleep
  ```

- **`pkill -u <username> <process_name>`**: Kill processes owned by a specific user.
  ```bash
  pkill -u john firefox
  ```

---

### **Summary of Key Commands:**

- **`jobs`**: List the background jobs in the current shell session.
- **`kill <PID>`**: Gracefully terminate a process by its PID using `SIGTERM` (default).
- **`kill -9 <PID>`**: Forcefully kill a process using `SIGKILL`.
- **`kill -l`**: List all available signals that can be sent to processes.
- **`kill -19 <PID>`**: Stop (pause) a process using `SIGSTOP`.
- **`kill -18 <PID>`**: Resume a stopped process using `SIGCONT`.
- **`killall <process_name>`**: Terminate all processes with the given name.
- **`pkill <process_name>`**: Terminate processes with the given name, with additional matching options.

These commands help in managing processes, whether in the background or foreground, by sending signals to control their behavior.

