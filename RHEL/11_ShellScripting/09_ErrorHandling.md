### **9. Error Handling and Exit Status**

Error handling is a crucial aspect of writing reliable and robust shell scripts. It helps you detect issues during execution and ensures your script reacts appropriately. Understanding how to handle exit statuses and using the `exit` command effectively can help you manage errors in your scripts.

---

### **9.1 Exit Status in Shell Scripting**

In shell scripts, every command executed returns an **exit status** (also known as return code or exit code). The exit status is an integer value, where:

- **0** indicates **success**.
- **Non-zero** values indicate **failure** or an error.

The exit status of the last executed command can be accessed using the special variable `$?`. This value helps in determining whether the previous command was successful or encountered an error.

#### **Example: Checking Exit Status**

```bash
#!/bin/bash

# Trying to list a nonexistent directory
ls /nonexistentdir

# Checking the exit status of the 'ls' command
echo $?
```

- **Explanation**:
  - The `ls /nonexistentdir` command will fail because the directory doesn't exist.
  - `$?` will output a non-zero value, typically `2` (the exit status indicating an error in the case of `ls`).

**Output:**
```
ls: cannot access '/nonexistentdir': No such file or directory
2
```

In this case, the exit status `2` indicates the error encountered by the `ls` command.

---

### **9.2 Using `exit` in Scripts**

The `exit` command in a shell script is used to terminate the script immediately and optionally return an exit status. You can use it to stop execution if certain conditions are met (such as invalid user input or failure in a command).

#### **Example: Using `exit` to Terminate Script on Failure**

```bash
#!/bin/bash

# Read user input
echo "Please enter your age:"
read age

# Check if the age is less than 18
if [ $age -lt 18 ]; then
    echo "You must be 18 or older."
    exit 1  # Exit the script with a failure status
fi

echo "You are eligible."
```

- **Explanation**:
  - If the user enters an age less than 18, the script will print the message "You must be 18 or older." and then exit with status `1`, which signifies an error.
  - If the user enters a valid age (18 or older), the script will print "You are eligible."

**Output for age = 16:**
```
Please enter your age:
16
You must be 18 or older.
```

**Exit Status:**
- If the script terminates early (with the `exit 1`), you can check the exit status after running the script.

---

### **9.3 Conditional Execution Based on Exit Status**

You can conditionally execute commands based on the exit status of the previous command using `if` statements, `&&` (AND), and `||` (OR) operators.

#### **Example: Using `if` with Exit Status**

```bash
#!/bin/bash

# Try to change to a directory
cd /some/directory

# Check the exit status and take action
if [ $? -eq 0 ]; then
    echo "Successfully changed directory."
else
    echo "Failed to change directory."
fi
```

- **Explanation**:
  - After attempting to change the directory using `cd /some/directory`, the exit status is checked.
  - If the `cd` command is successful, the script prints "Successfully changed directory."
  - If the `cd` command fails (e.g., the directory doesn't exist), it prints "Failed to change directory."

---

### **9.4 Logical Operators for Error Handling**

You can use the logical operators `&&` and `||` to chain commands and execute them based on the success or failure of the previous command.

- **`&&` (AND operator)**: The next command will run only if the previous command succeeded (exit status 0).
- **`||` (OR operator)**: The next command will run only if the previous command failed (non-zero exit status).

#### **Example: Using `&&` for Conditional Success**

```bash
#!/bin/bash

# Try to create a directory, only if it does not exist
mkdir /tmp/newdir && echo "Directory created successfully."
```

- **Explanation**:
  - The `mkdir` command creates the directory `/tmp/newdir` if it doesn't exist.
  - If `mkdir` succeeds (exit status 0), the `echo` command will execute and print "Directory created successfully."

#### **Example: Using `||` for Conditional Failure**

```bash
#!/bin/bash

# Try to remove a file, and handle failure if the file doesn't exist
rm /tmp/nonexistentfile || echo "File does not exist."
```

- **Explanation**:
  - The `rm` command attempts to delete `/tmp/nonexistentfile`.
  - If the file does not exist (i.e., `rm` fails), the `echo` command will execute and print "File does not exist."

---

### **9.5 Exit Status and `trap`**

Sometimes, you want to handle errors globally or when your script exits unexpectedly. You can use the `trap` command to catch the exit status and perform actions like cleaning up resources.

#### **Example: Using `trap` to Handle Exit**

```bash
#!/bin/bash

# Define a function to clean up on exit
cleanup() {
    echo "Script is exiting, performing cleanup..."
}

# Trap any exit signal and call the cleanup function
trap cleanup EXIT

# Simulate script work
echo "Script is running..."
exit 0
```

- **Explanation**:
  - The `trap` command catches the `EXIT` signal, which is triggered when the script finishes, and runs the `cleanup` function.
  - This is useful for ensuring that resources (such as temporary files or locks) are cleaned up when the script terminates.

**Output:**
```
Script is running...
Script is exiting, performing cleanup...
```

---

### **Conclusion**

Error handling and understanding exit statuses are vital for creating robust shell scripts. Key points include:

- **Exit Status**: Use `$?` to get the exit status of the last command and determine whether it succeeded or failed.
- **`exit` Command**: Use `exit` to terminate a script with a specific exit status. You can use it to stop execution when a condition fails.
- **Conditional Execution**: Use `&&` and `||` operators for conditional execution based on the success or failure of commands.
- **Error Handling with `trap`**: Use `trap` to handle script termination and perform cleanup tasks.

Mastering these concepts will help you create more reliable and predictable shell scripts.
