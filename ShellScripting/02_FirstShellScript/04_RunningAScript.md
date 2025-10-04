✅ **Running a Script with `./script.sh` or `bash script.sh`**

After you’ve created and made your shell script executable, you can run it in two main ways.

---

### 🧩 **1. Running with `./script.sh`**

#### **Command:**

```bash
./first_script.sh
```

#### **Explanation:**

* The `./` tells the shell to look for the file **in the current directory** (because by default, the current directory `.` is not in your PATH).
* The script will run **using the interpreter** specified in the **shebang line** (`#!/bin/bash`).

#### ✅ **Requirements:**

* The script **must have execute permission** (`chmod +x first_script.sh`).
* The **shebang line must point** to a valid interpreter.

#### **Example:**

```bash
#!/bin/bash
echo "Hello, World!"
```

Run:

```bash
./first_script.sh
```

**Output:**

```
Hello, World!
```

---

### 🧩 **2. Running with `bash script.sh`**

#### **Command:**

```bash
bash first_script.sh
```

#### **Explanation:**

* This explicitly runs the script using the **bash** interpreter, **ignoring the shebang line**.
* It doesn’t require execute permissions since you’re running Bash directly and passing the script as an argument.

#### ✅ **Useful When:**

* You want to test a script even if it’s not executable.
* You want to run it in a specific shell (e.g., force Bash even if shebang says `sh`).

---

### ⚖️ **Comparison Table**

| Method           | Needs Execute Permission? | Uses Shebang? | Typical Use Case               |
| ---------------- | ------------------------- | ------------- | ------------------------------ |
| `./script.sh`    | ✅ Yes                     | ✅ Yes         | Normal usage after setup       |
| `bash script.sh` | ❌ No                      | ❌ No          | Quick testing or debugging     |
| `sh script.sh`   | ❌ No                      | ❌ No          | Run in `sh` shell (POSIX mode) |

---

### ⚠️ **Common Errors**

1. **Permission Denied**

   ```
   bash: ./first_script.sh: Permission denied
   ```

   → Fix with `chmod +x first_script.sh`

2. **No Such File or Directory**

   ```
   bash: ./first_script.sh: No such file or directory
   ```

   → Make sure you’re in the correct directory (`cd path/to/script`)

3. **Bad Interpreter**

   ```
   bash: ./first_script.sh: /bin/bash^M: bad interpreter
   ```

   → Happens if script was created in Windows; fix by converting line endings:

   ```bash
   dos2unix first_script.sh
   ```

---

### 💡 **Pro Tip**

If you often run scripts from the same directory, add `.` (the current directory) to your PATH:

```bash
export PATH=$PATH:.
```

Then you can simply run:

```bash
first_script.sh
```

*(Not recommended on shared systems for security reasons.)*

---


