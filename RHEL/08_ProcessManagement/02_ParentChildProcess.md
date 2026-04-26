### **Understanding Process IDs and Parent Process IDs with Examples**

In this guide, we will cover how to check the process IDs (`PID`) and Parent Process IDs (`PPID`) using the `echo $$`, `ps -C`, and `echo $PPID` commands, and explore what happens when you run new shells (`bash`, `ksh`, `zsh`), execute them, and check their process and parent process IDs.

---

### **1. Display Current Process ID (PID) Using `$$`**

- The command `$$` will return the **PID** of the current shell.
  
   Example:
   ```bash
   echo $$
   ```

   Output:
   ```
   1234
   ```

   This shows the PID of the current shell, which is `1234` in this case.

---

### **2. Check Processes Using `ps -C`**

- The `ps -C` command allows you to view processes by their name.
  
   Example:
   ```bash
   ps -C bash
   ```

   Output:
   ```
     PID TTY      TIME     CMD
    1234 pts/0    00:00:00 bash
   ```

   In this output, the `ps -C bash` command shows all instances of `bash` running on the system, along with their **PID**.

---

### **3. Display Parent Process ID (PPID)**

- The `$PPID` environment variable holds the **PID** of the parent process of the current shell.
  
   Example:
   ```bash
   echo $PPID
   ```

   Output:
   ```
   1123
   ```

   This shows the **PPID** of the current shell, which is `1123` in this case (this would typically be the PID of the terminal or the shell that started this shell).

---

### **4. Creating Another Bash Shell and Checking PIDs**

- If you run a new `bash` shell from the current shell, the new shell will have a **different PID**.
  
   Example:
   ```bash
   bash
   ```

   After running the `bash` command, you can check the new shell's **PID** by running:

   ```bash
   echo $$
   ```

   Output:
   ```
   5678
   ```

   Now, if you run `ps -C bash` again, you'll see two `bash` processes, each with its own **PID**:

   Example:
   ```bash
   ps -C bash
   ```

   Output:
   ```
     PID TTY      TIME     CMD
    1234 pts/0    00:00:00 bash
    5678 pts/0    00:00:00 bash
   ```

   - The **first shell** (`PID=1234`) is the original shell.
   - The **second shell** (`PID=5678`) is the new shell you just launched.

---

