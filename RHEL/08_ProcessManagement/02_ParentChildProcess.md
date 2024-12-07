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

### **5. Installing `ksh` and `zsh`**

- To install **Korn Shell (ksh)** and **Z Shell (zsh)**, use the following `yum` commands:
  
   ```bash
   sudo yum install ksh
   sudo yum install zsh
   ```

   These commands install `ksh` and `zsh` on your system if they are not already installed.

---

### **6. Fork and Execute Commands**

#### a) **Check PID and Start `ksh`**

- Start by checking your current **PID**:

   ```bash
   echo $$
   ```

   Output:
   ```
   5678
   ```

- Run `ksh` to start the Korn shell:

   ```bash
   ksh
   ```

- Now, check the **PID** and **PPID** of the `ksh` shell:

   ```bash
   echo $$   # PID of ksh
   echo $PPID  # PPID of ksh
   ```

   Output:
   ```
   6789  # PID of ksh
   5678  # PPID of ksh (which is the original bash shell)
   ```

   As you can see, the **PID** of `ksh` is different from the `bash` PID. Also, the **PPID** of `ksh` is the **PID** of the parent shell (`bash`), which was `5678`.

#### b) **Execute `bash` from `ksh`**

- Now, run the `exec` command to start a new `bash` shell within `ksh`:

   ```bash
   exec bash
   ```

   The `exec` command replaces the current shell (which is `ksh` in this case) with a new `bash` shell. Once you run this command, the `ksh` shell will be replaced by `bash`, and the **PID** and **PPID** will reflect the new `bash` shell process.

- Check the **PID** and **PPID** after executing `bash`:

   ```bash
   echo $$   # PID of bash
   echo $PPID  # PPID of bash
   ```

   Output:
   ```
   6789  # PID of bash (same as the previous ksh shell PID)
   5678  # PPID of bash (same as the previous ksh shell PPID)
   ```

   **Explanation:**
   - The **PID** of `bash` after using `exec` is the same as the **PID** of the previous `ksh` shell because `exec` replaces the `ksh` shell with `bash`.
   - The **PPID** remains the same (`5678`), as it’s still the parent shell that initiated the `ksh` process.

---

### **Key Concepts:**

1. **`$$`** – This gives the PID of the current shell.
2. **`$PPID`** – This gives the PPID (Parent Process ID) of the current shell.
3. **`ps -C <command>`** – This shows all running instances of a specific command (e.g., `bash`, `ksh`).
4. **`exec`** – Replaces the current shell with a new command, using the same PID but a different process.
5. **Forking a new shell** – When you run `bash` from an existing shell, the new shell will have a different PID, but the PPID of the new shell will be the PID of the original shell.

---

### **Summary**

- The `bash`, `ksh`, and `zsh` shells each have their own **PID**.
- The **PPID** of a new shell is always the PID of the shell from which it was spawned.
- **`exec`** is used to replace the current shell with another process (e.g., replacing `ksh` with `bash`), and the PID stays the same while the PPID remains the same as the parent process.
- By using the commands `$$` and `$PPID`, you can track the process and parent process relationships.
