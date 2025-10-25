
## 🧩 **Local vs Global Variables in RHEL Shell Functions**

### 🧠 **Concept Overview**

In Bash (on RHEL or any Linux system), **variables are global by default** — meaning if you define a variable inside a function, it can be accessed (and even changed) from anywhere in the script **unless** you specifically make it **local**.

Using **local variables** inside functions helps prevent unexpected changes to variables in other parts of the script — a best practice for **system administrators** working on large automation scripts.

---

### ⚙️ **Global Variables Example**

```bash
#!/bin/bash

server_name="prod-server"

print_server() {
    echo "Inside function: Server is $server_name"
    server_name="test-server"
}

print_server
echo "Outside function: Server is $server_name"
```

🧩 **Output:**

```
Inside function: Server is prod-server
Outside function: Server is test-server
```

✅ The change made inside the function affected the **global variable** — because by default, all variables in Bash are global.

---

### ⚙️ **Local Variables Example**

```bash
#!/bin/bash

server_name="prod-server"

print_server() {
    local server_name="test-server"
    echo "Inside function: Server is $server_name"
}

print_server
echo "Outside function: Server is $server_name"
```

🧩 **Output:**

```
Inside function: Server is test-server
Outside function: Server is prod-server
```

✅ Using `local` restricted the variable’s scope **only to the function**, keeping the global `server_name` safe.

---

### 🧰 **SysAdmin Example – Using Local Variables Safely**

```bash
#!/bin/bash

backup_logs() {
    local src_dir="/var/log"
    local dest_dir="/backup/logs_$(date +%F)"

    echo "Creating backup directory: $dest_dir"
    mkdir -p "$dest_dir"

    echo "Copying logs from $src_dir to $dest_dir"
    cp -r "$src_dir"/* "$dest_dir"
    echo "Backup completed successfully."
}

# Main script
backup_logs
echo "Log backup script finished on $(hostname)"
```

✅ **Why local helps here:**
Even if another part of the script later uses variables like `src_dir`, they won’t interfere with this function’s internal operations.

---

### ⚠️ **Best Practices for SysAdmins**

1. Always use `local` inside functions for temporary variables.
2. Use **meaningful names** for global variables (e.g., `GLOBAL_PATH`, `CONFIG_FILE`).
3. Avoid modifying global state unless necessary.
4. Keep functions modular — one task per function.

---


