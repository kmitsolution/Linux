## 🧠 **RHEL Shell Scripting – Defining and Calling Functions**

### 📝 **What is a Function in Shell?**

A **function** in shell scripting is a block of reusable code that performs a specific task.
Functions help make scripts **modular, organized, and easier to debug or maintain** — very useful for system administrators who often repeat the same tasks (like log checks, backups, or service restarts).

---

### ⚙️ **Basic Syntax of a Function**

#### **Method 1 (Common style)**

```bash
function function_name() {
    commands
}
```

#### **Method 2 (POSIX compatible)**

```bash
function_name() {
    commands
}
```

Both styles are valid in **bash** on RHEL.

---

### ▶️ **Example 1 – Basic Function**

```bash
#!/bin/bash

say_hello() {
    echo "Hello, Welcome to RHEL Shell Scripting!"
}

# Call the function
say_hello
```

🧩 **Explanation:**

* `say_hello()` is the function name.
* Code inside `{ }` runs when you call `say_hello`.
* You must call the function **after defining it**, otherwise it won’t execute.

---

### ⚙️ **Example 2 – Function with Parameters**

```bash
#!/bin/bash

greet_user() {
    echo "Hello $1, Welcome to the RHEL environment!"
}

# Call the function with an argument
greet_user admin
```

🧩 **Explanation:**

* `$1`, `$2`, … represent arguments passed to the function.
* Here, `$1` becomes `"admin"` when the function is called.

---

### 🧰 **Example 3 – Function Returning Exit Status**

```bash
#!/bin/bash

check_root_user() {
    if [ "$EUID" -ne 0 ]; then
        echo "You must run this script as root!"
        return 1
    else
        echo "Running as root user."
        return 0
    fi
}

check_root_user
echo "Function exit status: $?"
```

🧩 **Explanation:**

* Functions can use `return` to send back an **exit status (0 = success, non-zero = failure)**.
* `$?` captures that status after the function runs.

---

### 🧩 **SysAdmin Example – Restarting a Service**

```bash
#!/bin/bash

restart_service() {
    local svc=$1
    echo "Restarting service: $svc"
    systemctl restart "$svc"
    if [ $? -eq 0 ]; then
        echo "$svc restarted successfully."
    else
        echo "Failed to restart $svc!"
    fi
}

# Call function with a service name
restart_service sshd
```

✅ Useful for automating service management tasks in RHEL.

---

### 🔍 **Key Notes**

* Define functions **before** calling them in the script.
* Use `local` variables inside functions to avoid conflicts.
* You can define multiple functions and call them in sequence.

---


