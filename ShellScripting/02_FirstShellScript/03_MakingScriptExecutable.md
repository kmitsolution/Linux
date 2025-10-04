✅ **Making Shell Scripts Executable**

Once you’ve written your shell script (for example, `first_script.sh`), you must **give it execute permission** before running it directly.

---

### 🧩 **1. Check Current Permissions**

Use the `ls -l` command to see the file permissions:

```bash
ls -l first_script.sh
```

**Example Output:**

```
-rw-r--r--  1 user user 45 Oct 4 10:00 first_script.sh
```

Here:

* `rw-` → Read & write for the owner
* `r--` → Read-only for group and others
* ❌ No **execute (x)** permission yet

---

### ⚙️ **2. Add Execute Permission**

To make the script executable, use:

```bash
chmod +x first_script.sh
```

Now check again:

```bash
ls -l first_script.sh
```

**Output:**

```
-rwxr-xr-x  1 user user 45 Oct 4 10:00 first_script.sh
```

✅ The `x` indicates **execute permission** has been added.

---

### 🧠 **Explanation**

* `chmod` → Change file mode (permissions)
* `+x` → Add execute permission
* You can also specify users:

  * `chmod u+x file.sh` → only for the **owner**
  * `chmod a+x file.sh` → for **everyone**

---

### 🧾 **3. Run the Script**

Now you can execute it directly:

```bash
./first_script.sh
```

💡 **Note:** The `./` tells the shell to look for the script in the current directory.

---

### ⚠️ **If You Don’t Make It Executable**

You’ll see an error like:

```
bash: ./first_script.sh: Permission denied
```

You can *still* run it without making it executable using:

```bash
bash first_script.sh
```

…but giving execute permission is the proper and professional way.

---

### 🧩 **Summary**

| Command               | Purpose                             |
| --------------------- | ----------------------------------- |
| `chmod +x script.sh`  | Make script executable for everyone |
| `chmod u+x script.sh` | Make executable only for user       |
| `ls -l script.sh`     | Check permissions                   |
| `./script.sh`         | Run script directly                 |

---


