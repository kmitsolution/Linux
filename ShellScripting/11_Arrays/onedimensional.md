## 🧠 **What is an Array in Shell Scripting?**

An **array** is a **collection of elements (values)** stored under one variable name.
Each element has an **index number** starting from `0`.

Arrays are useful for storing **lists of items** like filenames, usernames, IP addresses, or services — things a **SysAdmin** often works with.

---

## ⚙️ **1. Defining a One-Dimensional Array**

### **Syntax:**

```bash
array_name=(value1 value2 value3 ...)
```

### **Example:**

```bash
servers=("server1" "server2" "server3")
```

✅ The array `servers` now contains 3 elements:

| Index | Value   |
| ----- | ------- |
| 0     | server1 |
| 1     | server2 |
| 2     | server3 |

---

## 🧩 **2. Accessing Array Elements**

### **Syntax:**

```bash
${array_name[index]}
```

### **Example:**

```bash
echo "First server: ${servers[0]}"
echo "Second server: ${servers[1]}"
```

✅ **Output:**

```
First server: server1
Second server: server2
```

---

## 🔁 **3. Looping Through Array Elements**

### **Example:**

```bash
for s in "${servers[@]}"
do
    echo "Connecting to $s..."
done
```

✅ **Output:**

```
Connecting to server1...
Connecting to server2...
Connecting to server3...
```

---

## 🧠 **4. Array Length (Number of Elements)**

### **Syntax:**

```bash
${#array_name[@]}
```

### **Example:**

```bash
echo "Total servers: ${#servers[@]}"
```

✅ **Output:**

```
Total servers: 3
```

---

## 🧩 **5. Updating and Adding Elements**

### **Example:**

```bash
servers=("server1" "server2")
servers[2]="server3"      # Add new element
servers[1]="db-server"    # Modify element
echo "${servers[@]}"
```

✅ **Output:**

```
server1 db-server server3
```

---

## ⚙️ **6. Deleting an Array Element**

### **Syntax:**

```bash
unset array_name[index]
```

### **Example:**

```bash
unset servers[1]
echo "${servers[@]}"
```

✅ **Output:**

```
server1 server3
```

---

## 🧰 **SysAdmin-Level Examples**

### 🧩 **Example 1 – Service Monitoring**

```bash
#!/bin/bash
services=("sshd" "crond" "firewalld")

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

📘 *Use Case:* Quickly monitor the status of multiple services.

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

📘 *Use Case:* Automate backups for important system directories.

---

### 🧩 **Example 3 – Pinging Multiple Servers**

```bash
#!/bin/bash
hosts=("8.8.8.8" "google.com" "localhost")

for h in "${hosts[@]}"
do
    ping -c1 "$h" &>/dev/null && echo "$h is reachable" || echo "$h is not reachable"
done
```

📘 *Use Case:* Quickly check connectivity to multiple servers.

---

### 🧩 **Example 4 – Package Verification**

```bash
#!/bin/bash
packages=("httpd" "wget" "curl")

for pkg in "${packages[@]}"
do
    rpm -q "$pkg" &>/dev/null
    if [ $? -eq 0 ]; then
        echo "✅ $pkg is installed"
    else
        echo "❌ $pkg is missing"
    fi
done
```

📘 *Use Case:* Verify if required packages are installed across systems.

---

## 🧾 **Quick Summary Table**

| Action         | Syntax               | Example                   |
| -------------- | -------------------- | ------------------------- |
| Define array   | `array=(a b c)`      | `servers=("srv1" "srv2")` |
| Access element | `${array[index]}`    | `${servers[0]}`           |
| All elements   | `${array[@]}`        | `echo ${servers[@]}`      |
| Count elements | `${#array[@]}`       | `echo ${#servers[@]}`     |
| Add element    | `array[index]=value` | `servers[2]="srv3"`       |
| Delete element | `unset array[index]` | `unset servers[1]`        |

---

### 💡 **Best Practices for SysAdmins**

1. Always quote `"${array[@]}"` to handle spaces in values correctly.
2. Use arrays for lists — servers, services, paths — not for single values.
3. Combine arrays with functions and loops for powerful automation.
4. Use descriptive array names like `SERVICES`, `HOSTS`, `BACKUP_DIRS`.

---


