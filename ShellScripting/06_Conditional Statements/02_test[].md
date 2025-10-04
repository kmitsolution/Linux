✅ **`test` and `[ ]` Syntax in Shell Scripting**

The `test` command and `[ ]` brackets are used to **evaluate conditions** in shell scripts, such as checking numbers, strings, or file attributes.

> **Note:** `[ ]` is essentially a synonym for `test`, but `[ ]` is more commonly used in scripts.

---

## **1. Basic Syntax**

```bash
test CONDITION
# or
[ CONDITION ]
```

✅ Must have **spaces around `[ ]`**:

```bash
[ $age -ge 18 ]
```

---

## **2. Numeric Comparisons**

| Operator | Meaning               | Example         |
| -------- | --------------------- | --------------- |
| `-eq`    | Equal                 | `[ $a -eq 5 ]`  |
| `-ne`    | Not equal             | `[ $a -ne 5 ]`  |
| `-gt`    | Greater than          | `[ $a -gt 10 ]` |
| `-ge`    | Greater than or equal | `[ $a -ge 10 ]` |
| `-lt`    | Less than             | `[ $a -lt 20 ]` |
| `-le`    | Less than or equal    | `[ $a -le 20 ]` |

**Example:**

```bash
num=15
if [ $num -gt 10 ]; then
    echo "Number is greater than 10"
fi
```

---

## **3. String Comparisons**

| Operator | Meaning      | Example                 |
| -------- | ------------ | ----------------------- |
| `=`      | Equal        | `[ "$name" = "Alex" ]`  |
| `!=`     | Not equal    | `[ "$name" != "Alex" ]` |
| `-z`     | Empty string | `[ -z "$var" ]`         |
| `-n`     | Not empty    | `[ -n "$var" ]`         |

**Example:**

```bash
name="Linux"
if [ "$name" = "Linux" ]; then
    echo "System uses Linux"
fi
```

---

## **4. File Tests**

| Test | Meaning          | Example              |
| ---- | ---------------- | -------------------- |
| `-e` | File exists      | `[ -e /etc/passwd ]` |
| `-f` | Regular file     | `[ -f /etc/passwd ]` |
| `-d` | Directory exists | `[ -d /home/user ]`  |
| `-r` | Readable         | `[ -r file.txt ]`    |
| `-w` | Writable         | `[ -w file.txt ]`    |
| `-x` | Executable       | `[ -x script.sh ]`   |

**Example:**

```bash
file="/etc/passwd"
if [ -f "$file" ]; then
    echo "$file exists and is a regular file."
fi
```

---

## **5. Combining Conditions**

### **AND (`-a`)**

```bash
if [ $num -gt 10 -a $num -lt 20 ]; then
    echo "Number is between 10 and 20"
fi
```

### **OR (`-o`)**

```bash
if [ $num -lt 5 -o $num -gt 15 ]; then
    echo "Number is less than 5 or greater than 15"
fi
```

### **Negation (`!`)**

```bash
if [ ! -f "$file" ]; then
    echo "$file does not exist."
fi
```

---

## **6. Using `test` Instead of `[ ]`**

```bash
num=10
if test $num -eq 10; then
    echo "Number is 10"
fi
```

> Functionally the same as `[ $num -eq 10 ]`

---

### 💡 **System Administrator Examples**

1. **Check if a log file exists**

```bash
log_file="/var/log/syslog"
if [ -e "$log_file" ]; then
    echo "Log file exists."
fi
```

2. **Check if a directory exists**

```bash
backup_dir="/backup"
if [ ! -d "$backup_dir" ]; then
    mkdir -p "$backup_dir"
    echo "Backup directory created."
fi
```

3. **Check if a script is executable**

```bash
script="/usr/local/bin/myscript.sh"
if [ -x "$script" ]; then
    echo "Script is executable."
else
    echo "Make script executable: chmod +x $script"
fi
```

---

