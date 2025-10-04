✅ **Pipes (`|`) in Shell**

A **pipe (`|`)** in shell scripting is used to **connect multiple commands** — sending the **output of one command as the input to another.**
It’s one of the most powerful tools for building command chains.

---

### 🧩 **1. Basic Syntax**

```bash
command1 | command2
```

* `command1` → produces **output**
* `command2` → takes that output as **input**

---

### 🧠 **How It Works**

* The shell **doesn’t store** the output in a file; it **passes it directly** between commands.
* You can chain multiple commands together to process data step by step.

---

### 📘 **2. Simple Example**

```bash
ls | grep ".sh"
```

* `ls` lists files
* `grep ".sh"` filters only files ending with `.sh`

✅ **Output:**

```
first_script.sh
backup.sh
```

---

### 🧰 **3. Multiple Pipes**

You can chain multiple commands:

```bash
cat /etc/passwd | grep bash | wc -l
```

Explanation:

1. `cat /etc/passwd` → shows all users
2. `grep bash` → filters users using bash shell
3. `wc -l` → counts how many lines (users)

✅ **Output Example:**

```
7
```

---

### 🧩 **4. Another Common Example**

```bash
ps aux | grep nginx | awk '{print $2}'
```

Explanation:

* `ps aux` → shows all running processes
* `grep nginx` → filters lines containing "nginx"
* `awk '{print $2}'` → prints the process ID column

---

### 💡 **5. Using with Sorting and Counting**

```bash
cat access.log | sort | uniq -c | sort -nr | head
```

Explanation:

* `sort` → sorts lines
* `uniq -c` → removes duplicates & counts
* `sort -nr` → sorts by number (descending)
* `head` → shows top 10 results

✅ Useful for analyzing logs.

---

### ⚙️ **6. Combining with Redirection**

You can mix **pipes** and **redirection**:

```bash
ls /etc | grep conf > conf_files.txt
```

* `ls /etc | grep conf` → finds config files
* `> conf_files.txt` → saves result to a file

---

### 🧾 **Summary Table**

| Symbol | Purpose     | Example        | Description |           |                               |                        |
| ------ | ----------- | -------------- | ----------- | --------- | ----------------------------- | ---------------------- |
| `      | `           | Pipe           | `ls         | grep txt` | Send output of `ls` to `grep` |                        |
| `      | ` (chained) | Multiple pipes | `cat file   | grep word | wc -l`                        | Filter and count lines |

---

### ⚠️ **Common Mistake**

Using `>` instead of `|`
❌ Wrong:

```bash
ls > grep "sh"
```

✅ Correct:

```bash
ls | grep "sh"
```

---



---


