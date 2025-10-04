✅ **Creating a `.sh` File**

A shell script is simply a text file that contains a series of Linux commands. These commands are executed in order by the shell interpreter (usually **Bash**).

---

### 🧩 **Steps to Create Your First Shell Script**

#### **1. Create a new file**

You can use any text editor like `nano`, `vim`, or even `touch` to create a shell script file.

```bash
touch first_script.sh
```

This creates an empty file named **first_script.sh**.

---

#### **2. Open the file in an editor**

For example, using `nano`:

```bash
nano first_script.sh
```

or using `vim`:

```bash
vim first_script.sh
```

---

#### **3. Add the “shebang” line**

At the top of the script, always add this line:

```bash
#!/bin/bash
```

🧠 **Why?**
It tells the system to use **/bin/bash** as the interpreter to run your script.
Without it, your script might run in a different shell (like `sh`), which could cause compatibility issues.

---

#### **4. Write your commands**

Example:

```bash
#!/bin/bash
echo "Hello, World!"
echo "This is my first shell script."
```

---

#### **5. Save and exit**

If using `nano`, press:

```
Ctrl + O   → Save
Ctrl + X   → Exit
```

---

#### **6. Make the script executable**

Before running, you need to give it execute permission:

```bash
chmod +x first_script.sh
```

---

#### **7. Run the script**

You can run it in two ways:

**Method 1:** Run directly (if it has execute permission)

```bash
./first_script.sh
```

**Method 2:** Run with bash command

```bash
bash first_script.sh
```

---

### 🧾 Output Example

```bash
Hello, World!
This is my first shell script.
```

---


