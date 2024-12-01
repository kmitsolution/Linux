The **`history`** command in Linux (including Red Hat, CentOS, and other Unix-like systems) is used to display a list of previously executed commands. This feature is useful for recalling commands you've run in the past and rerunning them without having to retype them completely.

### **Using the `history` Command**

1. **Display the Command History:**
   - When you type `history` at the command prompt, it will display a list of commands that were previously entered, along with their line numbers. The default number of commands displayed is typically 1000, but this can be adjusted.
   - **Example:**
     ```bash
     history
     ```
     Output:
     ```
     1  ls -l
     2  cd /home/user
     3  mkdir new_directory
     4  echo "Hello, World!"
     ```

2. **Display a Specific Number of Previous Commands:**
   - You can specify a number to display the last **n** commands from your history. For example, to display the last 10 commands:
   - **Example:**
     ```bash
     history 10
     ```
   - This will show the last 10 commands you executed.

### **Running a Specific Command Using `!` (Event Designators)**

The `!` character is used to reference and execute commands from your history based on their event number (the number displayed next to each command in the `history` output).

#### **Examples of Using `!` for Running Specific Commands:**

1. **Run a Command by its Event Number:**
   - If you want to run the 3rd command from your history:
     ```bash
     !3
     ```
   - This will execute the command with event number 3 (e.g., `mkdir new_directory` in the previous example).

2. **Run the Last Command:**
   - To rerun the last command executed (similar to pressing the up arrow):
     ```bash
     !!
     ```
   - This executes the last command from the history. For example, if your last command was `echo "Hello, World!"`, it will execute `echo "Hello, World!"` again.

3. **Run a Command Starting with a Specific String:**
   - You can also run the last command that started with a specific string:
     ```bash
     !echo
     ```
   - This will run the most recent command starting with `echo`. In the previous example, it would rerun `echo "Hello, World!"`.

4. **Run a Command by Searching with `!<string>`:**
   - For example, if you previously ran a command like `ls -l` and want to run it again, you can use:
     ```bash
     !ls
     ```
   - This will execute the most recent `ls` command from your history.

5. **Run the Command Before a Specific Event Number:**
   - You can also use `!` to run the command before a certain event number.
     ```bash
     !-2
     ```
   - This will run the second-to-last command in your history (e.g., `cd /home/user` if it was the second-to-last command).

### **Modifying the History Size with `$HISTSIZE`**

The **`$HISTSIZE`** environment variable defines the number of commands that the shell keeps in the history list. By default, this value is typically set to 1000, but you can change it to retain more or fewer commands.

#### **Examples of Using `$HISTSIZE`:**

1. **Check the Current Value of `$HISTSIZE`:**
   - To view the current history size limit, you can check the value of the `$HISTSIZE` variable:
     ```bash
     echo $HISTSIZE
     ```
   - This will display the current limit, e.g., `1000`.

2. **Change the History Size Temporarily (for the Current Session):**
   - To increase or decrease the number of commands saved in your history for the current session, use:
     ```bash
     export HISTSIZE=5000
     ```
   - This changes the history size to 5000 commands for the current session only.

3. **Change the History Size Permanently:**
   - To permanently change the history size, add the `export HISTSIZE` line to your shell configuration file (e.g., `~/.bashrc` for the Bash shell):
     ```bash
     echo "export HISTSIZE=5000" >> ~/.bashrc
     ```
   - After modifying the `.bashrc` file, apply the changes by running:
     ```bash
     source ~/.bashrc
     ```

4. **Clear History (Optional):**
   - If you wish to clear the history list completely:
     ```bash
     history -c
     ```
   - This command will clear the history list for the current session, but it won't affect the history file unless you manually delete it (e.g., `~/.bash_history`).

---

### **Summary of Key Points:**

- **`history`** shows the list of previous commands.
- **`!<event_number>`** runs a specific command from history based on its event number (e.g., `!3` runs the command at event number 3).
- **`!!`** reruns the last command.
- **`!<string>`** reruns the most recent command starting with a specific string (e.g., `!echo`).
- **`$HISTSIZE`** sets the number of commands to be saved in the history list.
- Use **`history -c`** to clear the history for the current session.

By using the `history` command effectively, you can quickly repeat past commands and streamline your workflow in the Linux shell.
