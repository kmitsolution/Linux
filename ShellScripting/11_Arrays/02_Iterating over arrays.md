
## 🧠 **Iterating Over Arrays in Shell Scripting**

Once you have an array defined, you can **iterate** (loop) over its elements to perform actions on each item — for example, checking services, pinging servers, backing up directories, or verifying packages.

---

### ⚙️ **1. Basic Syntax**

#### **Using a `for` loop**

```bash
for element in "${array[@]}"
do
    commands using "$element"
done
```

✅ Always use quotes (`"${array[@]}"`) to correctly handle items containing spaces.

---

### 🧩 **Example 1 – Print Array Elements**

```bash
#!/bin/bash
servers=("web1" "web2" "db1")

for s in "${servers[@]}"
do
    echo "Server: $s"
done
```

✅ **Output:**

```
Server: web1
Server: web2
Server: db1
```

---

### ⚙️ **2. Iterating with Array Indexes**

If you need both **index and value**, you can loop using a numeric index.

```bash
for i in "${!servers[@]}"
do
    echo "Index: $i | Value: ${servers[$i]}"
done
```

✅ **Output:**

```
Index: 0 | Value: web1
Index: 1 | Value: web2
Index: 2 | Value: db1
```

---

## 🧰 **SysAdmin-Level Practical Examples**

---

### 🧩 **Example 1 – Checking Multiple Services**

```bash
#!/bin/bash
services=("sshd" "crond" "firewalld" "nginx")

for svc in "${services[@]}"
do
    systemctl is-active --quiet "$svc"
    if [ $? -eq 0 ]; then
        echo "✅ $svc is running"
    else
        echo "❌ $svc is NOT running"
    fi
done
```

📘 **Use Case:**
Monitor key system services on RHEL servers.

---

### 🧩 **Example 2 – Backing Up Multiple Directories**

```bash
#!/bin/bash
dirs=("/etc" "/var/log" "/home")

backup_dir="/backup/$(date +%F)"
mkdir -p "$backup_dir"

for d in "${dirs[@]}"
do
    tar -czf "$backup_dir/$(basename $d).tar.gz" "$d"
    echo "Backed up $d to $backup_dir"
done
```

📘 **Use Case:**
Automate backups for multiple system directories (logs, configs, home data).

---

### 🧩 **Example 3 – Pinging Multiple Hosts**

```bash
#!/bin/bash
hosts=("8.8.8.8" "google.com" "localhost")

for h in "${hosts[@]}"
do
    if ping -c1 "$h" &>/dev/null; then
        echo "✅ $h is reachable"
    else
        echo "❌ $h is not reachable"
    fi
done
```

📘 **Use Case:**
Test connectivity across internal and external systems.

---

### 🧩 **Example 4 – Verifying Installed Packages**

```bash
#!/bin/bash
packages=("httpd" "wget" "curl" "vim")

for pkg in "${packages[@]}"
do
    if rpm -q "$pkg" &>/dev/null; then
        echo "✅ Package $pkg is installed"
    else
        echo "⚠️  Package $pkg is NOT installed"
    fi
done
```

📘 **Use Case:**
Check if essential software packages exist on the system.

---

### 🧩 **Example 5 – Managing Multiple Users**

```bash
#!/bin/bash
users=("devops" "developer" "qa")

for user in "${users[@]}"
do
    if id "$user" &>/dev/null; then
        echo "User $user already exists."
    else
        useradd "$user"
        echo "User $user created successfully."
    fi
done
```

📘 **Use Case:**
Automatically create or verify multiple service or project accounts.

---

### 🧩 **Example 6 – Mixed Use: Directory + Service Monitoring**

```bash
#!/bin/bash
dirs=("/var/log" "/etc" "/home")
services=("sshd" "crond")

echo "=== Checking Directories ==="
for dir in "${dirs[@]}"
do
    [ -d "$dir" ] && echo "✅ $dir exists" || echo "❌ $dir missing!"
done

echo
echo "=== Checking Services ==="
for svc in "${services[@]}"
do
    systemctl is-active --quiet "$svc" && echo "✅ $svc running" || echo "❌ $svc stopped"
done
```

📘 **Use Case:**
Perform both file system and service validation in a single automated check.

---

## 🔁 **Bonus: Looping Over Arrays with Command Output**

You can create arrays dynamically from commands:

```bash
users=($(cut -d: -f1 /etc/passwd))

for u in "${users[@]}"
do
    echo "User: $u"
done
```

📘 **Use Case:**
Process system users, log entries, or command results dynamically.

---

## 🧾 **Quick Summary**

| Concept               | Example                               |
| --------------------- | ------------------------------------- |
| Define array          | `arr=(one two three)`                 |
| Access single element | `${arr[1]}`                           |
| All elements          | `${arr[@]}`                           |
| Indexes               | `${!arr[@]}`                          |
| Loop elements         | `for i in "${arr[@]}"; do ...; done`  |
| Loop indexes          | `for i in "${!arr[@]}"; do ...; done` |

---

## 💡 **SysAdmin Best Practices**

1. Use `local` arrays inside functions to prevent variable conflicts.
2. Always quote `"${array[@]}"` when looping (to handle spaces safely).
3. Use arrays for grouping servers, users, packages, or file paths.
4. Combine arrays + loops for robust automation scripts.
5. Log actions inside loops for auditing (`>> /var/log/script.log`).

---


