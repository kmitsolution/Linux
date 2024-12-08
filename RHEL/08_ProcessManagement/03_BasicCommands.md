Let's break down the commands you're asking about, starting with `pgrep` and `pstree`, and then going into `ps` with some examples. I'll explain each command in detail.

### 1. `pgrep` Command:
`pgrep` is used to search for processes by name and return their process IDs (PIDs). 

#### Example:
```bash
pgrep sshd
```
This command searches for processes with the name `sshd` (Secure Shell Daemon) and returns their PIDs. You might see output like this:
```
1234
5678
```
This means the `sshd` process is running with PIDs 1234 and 5678.

### 2. `pstree` Command:
`pstree` shows a tree of processes, displaying their hierarchical structure.

#### Example:
```bash
pstree
```
This will output something like this:
```
systemd─┬─sshd───bash───pstree
     └─sshd───bash───vim
```
This shows the parent-child relationship between processes. For instance, `bash` is a child of `sshd`, and `pstree` is a child of `bash`.

#### Example with `-p` flag:
```bash
pstree -p
```
This will show the process tree with the PIDs:
```
init(1)─┬─sshd(1234)───bash(2345)───pstree(3456)
        └─sshd(6789)───bash(7890)───vim(8901)
```
Here, each process has its PID shown next to it.

#### Example with `-p` and `-u` flags:
```bash
pstree -p -u username
```
This command shows the process tree with PIDs and the user `username` associated with the processes.

#### Example with `-p -s` flags:
```bash
pstree -p -s 2345
```
This will show the process tree for the process with PID `2345` and its ancestors (i.e., the processes leading up to the root).

#### Example with `man`:
```bash
man pstree
```
This opens the manual page for `pstree`, which explains all options in detail.

### 3. `sleep &` Command:
`sleep` is used to pause execution for a specified amount of time. Using `&` runs the command in the background.

#### Example:
```bash
sleep 60 &
```
This runs the `sleep` command in the background for 60 seconds. The terminal will immediately return the PID of the `sleep` process, so you can continue working.

### 4. `ps -C` Command:
`ps` is used to display information about processes running on the system. The `-C` option allows you to filter processes by name.

#### Example:
```bash
ps -C bash
```
This will show information about all running processes with the name `bash`:
```
  PID TTY          TIME CMD
  2345 pts/0    00:00:00 bash
  7890 pts/1    00:00:00 bash
```
This lists the PIDs of all `bash` processes along with their associated TTY (terminal), running time, and command name.

### 5. Example of Advanced `ps` Commands:
- **Displaying all processes with a custom format**:
  ```bash
  ps -eo pid,ppid,cmd,%cpu,%mem
  ```
  This command shows all processes with the following information:
  - `pid`: Process ID
  - `ppid`: Parent Process ID
  - `cmd`: Command name
  - `%cpu`: CPU usage
  - `%mem`: Memory usage

- **Displaying all processes of a user**:
  ```bash
  ps -u username
  ```
  This will display processes running for the user `username`.

- **Displaying processes with specific states**:
  ```bash
  ps -eo pid,stat,cmd | grep Z
  ```
  This filters and shows processes in the "Zombie" state (represented by `Z`).

The `ps` command is used to display information about processes running on a system. The `-f` and `-ef` options modify the output to display different levels of detail. Let's break down both of these options:

### 1. `ps -f` (Full format)
The `-f` option stands for **"full format"**. It provides more detailed information about each process compared to the default `ps` output.

#### Example:
```bash
ps -f
```

#### Output:
```
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 Dec08 ?        00:00:03 /sbin/init
user      1234   567  0 10:15 pts/0    00:00:01 bash
user      5678  1234  0 10:16 pts/0    00:00:00 ps -f
```

#### Explanation of Columns:
- **UID**: User ID of the process owner.
- **PID**: Process ID.
- **PPID**: Parent Process ID (the ID of the process that spawned this one).
- **C**: CPU utilization.
- **STIME**: Start time of the process.
- **TTY**: The terminal associated with the process (or `?` for processes not associated with a terminal).
- **TIME**: Total CPU time used by the process.
- **CMD**: The command that started the process.

This format provides more detailed process information compared to the default `ps` command.

---

### 2. `ps -ef` (Extended format)
The `-e` option means **"show all processes"**, and `-f` means **"full format"**. Together, `ps -ef` shows detailed information about all processes running on the system.

#### Example:
```bash
ps -ef
```

#### Output:
```
UID        PID  PPID  C STIME TTY      STAT   TIME    COMMAND
root         1     0  0 Dec08 ?        Ss     00:00:03 /sbin/init
user      1234   567  0 10:15 pts/0    S      00:00:01 bash
user      5678  1234  0 10:16 pts/0    S      00:00:00 ps -ef
```

#### Explanation of Columns:
- **UID**: User ID of the process owner.
- **PID**: Process ID.
- **PPID**: Parent Process ID.
- **C**: CPU utilization.
- **STIME**: Start time of the process.
- **TTY**: The terminal associated with the process.
- **STAT**: Process status (e.g., `S` for sleeping, `R` for running, `Z` for zombie).
- **TIME**: Total CPU time used by the process.
- **COMMAND**: The full command line that started the process.

### Key Differences Between `ps -f` and `ps -ef`:
- **`ps -f`**: Displays a **full-format** output, but only for the processes associated with the current user and the terminal session.
- **`ps -ef`**: Displays a **full-format** output for **all processes** running on the system, including those owned by other users and those not associated with any terminal.

### Common Use Cases:
- **`ps -f`**: Useful for getting a detailed view of processes running within your terminal or user session.
- **`ps -ef`**: Useful for getting a system-wide view of all processes running on the machine, often used for system monitoring or troubleshooting.

---

### Example Commands for Process Management:
- **To find a specific process**: You can combine `ps -ef` with `grep` to search for a specific process by name.
  ```bash
  ps -ef | grep sshd
  ```
  This will show all `sshd` processes running on the system.

- **To see processes running by a specific user**: Use the `-u` flag with `ps`.
  ```bash
  ps -ef -u username
  ```

In summary:
- **`ps -f`**: Displays detailed information about the processes for the current user and terminal.
- **`ps -ef`**: Displays detailed information about all processes running on the system, regardless of the user or terminal.

  

### Summary:
- `pgrep sshd`: Find PIDs of processes with the name `sshd`.
- `pstree`: View the process tree.
- `pstree -p`: View process tree with PIDs.
- `pstree -p -u username`: View process tree with PIDs and user.
- `sleep &`: Run the `sleep` command in the background.
- `ps -C bash`: List processes named `bash`.
- `ps -eo pid,ppid,cmd,%cpu,%mem`: Show a custom format with process details.
- `ps -u username`: Show processes for a specific user.

