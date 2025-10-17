#  **Conditional Execution: `&&` and `||` in Bash**

These operators let you chain commands together so that **the next command runs only if** the previous one succeeds or fails.

---

## 🧩 **1️⃣ Using `&&` (AND Operator)**

### 📘 **Meaning:**

Run the next command **only if the previous one succeeds** (`exit status == 0`).

### 💻 **Syntax:**

```bash
command1 && command2
```

➡️ `command2` runs **only if** `command1` is successful.

---

### ✅ **Example 1:**

```bash
mkdir /tmp/testdir && echo "Directory created successfully!"
```

If `/tmp/testdir` is created → prints success message.
If not → no message is shown.

---

### ✅ **Example 2 (SysAdmin Use Case):**

```bash
systemctl restart sshd && echo "sshd restarted successfully"
```

📘 *Use Case:* Log or print confirmation **only if** service restart works.

---

### ✅ **Example 3: Run Multiple Successive Commands**

```bash
yum -y update && yum -y upgrade && echo "System updated successfully!"
```

All commands will execute **sequentially only if** each one succeeds.

---

## 🧩 **2️⃣ Using `||` (OR Operator)**

### 📘 **Meaning:**

Run the next command **only if the previous one fails** (`exit status != 0`).

### 💻 **Syntax:**

```bash
command1 || command2
```

➡️ `command2` runs **only if** `command1` fails.

---

### ❌ **Example 1:**

```bash
ls /nonexistentdir || echo "Directory not found!"
```

Output:

```
ls: cannot access '/nonexistentdir': No such file or directory
Directory not found!
```

---

### ✅ **Example 2 (SysAdmin Use Case):**

```bash
systemctl is-active --quiet nginx || systemctl start nginx
```

📘 *Use Case:*
If **nginx is not running**, start it automatically.

---

### ✅ **Example 3: Backup Error Handling**

```bash
tar -czf /backup/etc_backup.tar.gz /etc || echo "Backup failed — check disk space!"
```

---

## 🧠 **3️⃣ Combining `&&` and `||` (Ternary Style)**

This is like a mini “if-else” in one line.

### 💻 **Syntax:**

```bash
command1 && echo "Success" || echo "Failure"
```

📘 *Explanation:*

* If `command1` succeeds → prints “Success”
* If it fails → prints “Failure”

### ✅ **Example:**

```bash
ping -c1 google.com &>/dev/null && echo "Internet OK" || echo "No Internet"
```

Output depends on network status.

---

## 🧩 **4️⃣ Combining Multiple Conditions**

### 💻 **Example:**

```bash
systemctl restart httpd && systemctl is-active --quiet httpd && echo "Apache is running fine" || echo "Apache failed to start!"
```

📘 *Use Case:*
Restart Apache and confirm it’s active — otherwise show error message.

---

## 🧠 **Summary Table**

| Operator | Meaning | Runs When                     | Example                          |                                        |                        |   |                    |
| -------- | ------- | ----------------------------- | -------------------------------- | -------------------------------------- | ---------------------- | - | ------------------ |
| `&&`     | AND     | Previous command **succeeds** | `mkdir /test && echo "Created!"` |                                        |                        |   |                    |
| `        |         | `                             | OR                               | Previous command **fails**             | `ls /abc               |   | echo "Not found!"` |
| `&& ...  |         | ...`                          | Combined (if-else)               | Success → 1st echo, Failure → 2nd echo | `ping ... && echo "OK" |   | echo "Down"`       |

---

## 🧰 **SysAdmin Quick Examples**

| Task                                  | Command                                                      |   |                                      |
| ------------------------------------- | ------------------------------------------------------------ | - | ------------------------------------ |
| Restart a service only if it’s active | `systemctl is-active --quiet sshd && systemctl restart sshd` |   |                                      |
| Start service if not running          | `systemctl is-active --quiet nginx                           |   | systemctl start nginx`               |
| Test connection before update         | `ping -c1 google.com &>/dev/null && yum update -y            |   | echo "No Internet, skipping update"` |
| Verify backup success                 | `tar -czf /backup/etc.tar.gz /etc && echo "Backup OK"        |   | echo "Backup Failed!"`               |

---


