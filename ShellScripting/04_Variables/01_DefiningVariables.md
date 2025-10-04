✅ **Defining and Using Variables in Shell Scripting**

Variables are used to **store and reuse data** such as text, numbers, filenames, or command outputs within your script.

---

### 🧩 **1. Defining a Variable**

#### **Syntax:**

```bash
variable_name=value
```

⚠️ **Important Rules:**

* ❌ **No spaces** around `=`
  (✅ `name=Linux` → ❌ `name = Linux`)
* Variable names can contain **letters, numbers, and underscores**, but **cannot start with a number**.
* Variables are **case-sensitive** (`NAME` and `name` are different).

---

### 📘 **2. Using (Accessing) a Variable**

To use a variable, prefix it with `$`:

```bash
name="Linux"
echo "Hello, $name!"
```

✅ **Output:**

```
Hello, Linux!
```

---

### 💡 **3. Display Without Quotes**

If you use `echo` without quotes:

```bash
greeting=Hello
name=World
echo $greeting $name
```

✅ Output:

```
Hello World
```

If you omit the `$`, it will print the literal name:

```bash
echo greeting
```

❌ Output:

```
greeting
```

---

### 🧠 **4. Double vs Single Quotes**

| Type              | Example         | Expands Variables? | Description            |
| ----------------- | --------------- | ------------------ | ---------------------- |
| Double quotes `"` | `"Hello $USER"` | ✅ Yes              | Variables are expanded |
| Single quotes `'` | `'Hello $USER'` | ❌ No               | Treated as plain text  |

#### Example:

```bash
USER="Alex"
echo "User: $USER"   # Output: User: Alex
echo 'User: $USER'   # Output: User: $USER
```

---

### ⚙️ **5. Combining Text and Variables**

You can combine variables with text directly:

```bash
file="report"
echo "The file is ${file}.txt"
```

✅ Output:

```
The file is report.txt
```

💡 The curly braces `{}` are useful when appending characters to variable names.

---

### 🔁 **6. Assigning Command Output to a Variable**

You can store command output using **backticks (` `)** or **$( )**:

```bash
current_dir=$(pwd)
echo "You are in: $current_dir"
```

✅ Output:

```
You are in: /home/user/scripts
```

🧠 Preferred modern syntax: `$( )` (more readable and nestable than backticks).

---

### 🔒 **7. Read-Only Variables**

To make a variable **immutable**:

```bash
readonly version="1.0"
version="2.0"  # ❌ Error
```

---

### 🧹 **8. Unsetting Variables**

To delete or clear a variable:

```bash
unset name
```

After this, `$name` will return nothing.

---

### 🧾 **Summary Table**

| Action             | Syntax               | Example            |
| ------------------ | -------------------- | ------------------ |
| Define variable    | `var=value`          | `name="Linux"`     |
| Use variable       | `$var`               | `echo $name`       |
| Use in text        | `"Hello $var"`       | `echo "Hi $USER"`  |
| Prevent expansion  | `'text $var'`        | `echo '$USER'`     |
| Command output     | `$(command)`         | `date=$(date)`     |
| Read-only variable | `readonly var=value` | `readonly OS=RHEL` |
| Remove variable    | `unset var`          | `unset name`       |

---


