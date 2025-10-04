✅ **Redirection in Shell (`>`, `>>`, `<`)**

In shell scripting, **redirection** allows you to **control where input and output go** — instead of displaying everything on the screen, you can send it to files or take input from files.

---

### 📤 **1. Output Redirection (`>`)**

Used to **redirect (send)** the **output** of a command into a file — replacing any existing content.

#### **Syntax:**

```bash
command > filename
```

#### **Example:**

```bash
echo "Hello, World!" > output.txt
```

✅ Creates (or overwrites) `output.txt` with:

```
Hello, World!
```

#### ⚠️ Important:

If `output.txt` already exists, it will be **overwritten** without warning.

---

### 📥 **2. Append Redirection (`>>`)**

Used to **append** command output to the **end of a file** — without deleting the existing content.

#### **Syntax:**

```bash
command >> filename
```

#### **Example:**

```bash
echo "This is line 1" > log.txt
echo "This is line 2" >> log.txt
```

✅ **Result in `log.txt`:**

```
This is line 1
This is line 2
```

💡 **Use `>>` for logs** or scripts that run repeatedly — it preserves previous data.

---

### 📚 **3. Input Redirection (`<`)**

Used to **take input** for a command **from a file** instead of the keyboard.

#### **Syntax:**

```bash
command < filename
```

#### **Example:**

If you have a file `names.txt`:

```
Alice
Bob
Charlie
```

You can use:

```bash
while read name; do
  echo "Hello, $name"
done < names.txt
```

✅ **Output:**

```
Hello, Alice
Hello, Bob
Hello, Charlie
```

---

### ⚙️ **4. Combine Input and Output Redirection**

You can use both input (`<`) and output (`>`, `>>`) in the same command.

#### **Example:**

```bash
sort < unsorted.txt > sorted.txt
```

This reads data from `unsorted.txt` and writes the sorted output into `sorted.txt`.

---

### 🧾 **Summary Table**

| Symbol | Type            | Description              | Overwrites? |
| ------ | --------------- | ------------------------ | ----------- |
| `>`    | Output          | Send output to a file    | ✅ Yes       |
| `>>`   | Output (append) | Add output to file’s end | ❌ No        |
| `<`    | Input           | Take input from file     | N/A         |

---

### 💡 **Pro Tip: Redirecting Errors**

Use `2>` to redirect **error messages**:

```bash
ls /no/such/dir 2> errors.txt
```

✅ Saves error output in `errors.txt`.

Or to send **both output and errors**:

```bash
command > all_output.txt 2>&1
```

---


