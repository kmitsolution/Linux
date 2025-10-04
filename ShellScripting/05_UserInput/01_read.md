✅ **`read` Command – Taking User Input in Shell Scripts**

The `read` command allows your shell script to **interactively get input** from the user.

---

### 🧩 **1. Basic Syntax**

```bash
read variable_name
```

#### **Example:**

```bash
echo "Enter your name:"
read name
echo "Hello, $name!"
```

**Sample Run:**

```
Enter your name:
Alex
Hello, Alex!
```

---

### 🧠 **2. Using `-p` for Inline Prompt**

Instead of a separate `echo`, you can use `-p` to display a prompt inline:

```bash
read -p "Enter your age: " age
echo "You are $age years old."
```

**Output:**

```
Enter your age: 25
You are 25 years old.
```

---

### 🧩 **3. Reading Multiple Inputs**

You can read multiple variables in one line:

```bash
read first_name last_name
echo "First: $first_name, Last: $last_name"
```

**Input Example:**

```
John Doe
```

**Output:**

```
First: John, Last: Doe
```

---

### ⚙️ **4. Silent Input (`-s`)**

Use `-s` to hide input (useful for passwords):

```bash
read -sp "Enter password: " password
echo
echo "Password accepted."
```

**Behavior:**
The typed password is **not displayed** on the terminal.

---

### 🧾 **5. Timeout and Default Value**

* `-t` → specify a timeout in seconds
* `-i` → provide a default value (requires Bash 4+)

```bash
read -t 10 -p "Enter city (default: Delhi): " city
city=${city:-Delhi}
echo "City: $city"
```

* If the user does not enter anything in 10 seconds, it will default to `Delhi`.

---

### 💡 **6. Reading From a File**

You can also read input **line by line** from a file:

```bash
while read line; do
  echo "Line: $line"
done < data.txt
```

---

### 🧾 **Summary Table**

| Option             | Description     | Example                                           |
| ------------------ | --------------- | ------------------------------------------------- |
| `read var`         | Basic input     | `read name`                                       |
| `read -p "Prompt"` | Inline prompt   | `read -p "Age: " age`                             |
| `read -s`          | Silent input    | `read -sp "Password: " pass`                      |
| `read -t seconds`  | Timeout         | `read -t 5 var`                                   |
| `read var1 var2`   | Multiple inputs | `read first last`                                 |
| `read < file`      | Read from file  | `while read line; do echo $line; done < file.txt` |

---


