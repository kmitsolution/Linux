✅ **Environment Variables vs User-Defined Variables**

In shell scripting, variables can either be **predefined by the system** (environment variables) or **created by the user** (user-defined variables). Understanding the difference is essential for writing robust scripts.

---

### 🧩 **1. Environment Variables**

#### **Definition:**

* Variables **predefined by the system or shell**.
* They are **available to the shell and all child processes**.

#### **Examples:**

```bash
echo $HOME       # Current user’s home directory
echo $USER       # Logged-in username
echo $PATH       # Directories searched for commands
echo $SHELL      # Default shell path
```

#### **Use Cases:**

* Access system info in scripts
* Configure shell behavior (e.g., `$PATH`, `$EDITOR`)

#### **Exporting Variables:**

To make a variable **available to child processes**, use `export`:

```bash
export MY_VAR="Hello"
bash -c 'echo $MY_VAR'   # Output: Hello
```

---

### 🧩 **2. User-Defined Variables**

#### **Definition:**

* Variables **created by the user** in a shell or script.
* By default, they are **only available in the current shell session**.

#### **Example:**

```bash
name="Alex"
echo "Hello, $name"
```

#### **Scope:**

* Local to the script or shell unless **exported**.
* Child scripts **cannot see** them unless exported.

#### **Export Example:**

```bash
greeting="Hello"
export greeting
./child_script.sh   # child_script.sh can access $greeting
```

---

### 🧠 **3. Key Differences**

| Feature        | Environment Variable                  | User-Defined Variable                |
| -------------- | ------------------------------------- | ------------------------------------ |
| Predefined     | ✅ Often by OS/shell                   | ❌ Only user-created                  |
| Scope          | Global (available to child processes) | Local (current shell only)           |
| Persistence    | Usually persistent in session         | Exists until shell/script ends       |
| Export needed? | Already exported                      | ✅ Use `export` to make global        |
| Examples       | `$HOME`, `$USER`, `$PATH`             | `myvar="Hello"`, `file="report.txt"` |

---

### 💡 **4. Practical Example**

```bash
# User-defined
file="data.txt"
echo "File: $file"  # Output: File: data.txt

# Environment
echo "Home directory: $HOME"  # Output: /home/username

# Exported user-defined variable
export greeting="Hi"
bash -c 'echo $greeting'      # Output: Hi
```

---

### ⚙️ **5. Best Practices**

1. Use **uppercase letters** for environment variables (`PATH`, `HOME`) to distinguish them from user-defined variables (`file`, `name`).
2. Export only variables that need to be accessed by **child scripts or processes**.
3. Avoid overwriting system environment variables unless intentional.

---


