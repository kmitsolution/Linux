
# 📦 **Package Management in RHEL (YUM, DNF & RPM)**

RHEL uses **DNF** (Dandified YUM) as the modern package manager, but **YUM** commands still work (backward compatibility).
**RPM** is the low-level package manager used underneath.

We'll cover:
✔️ Installing packages
✔️ Removing packages
✔️ Checking installed software
✔️ Repository management
✔️ Installing `.rpm` files
✔️ Automation scripts for SysAdmins

---

# 🧠 **1. DNF – Modern Package Manager (RHEL 8/9)**

---

## ⚙️ **Basic DNF Commands**

### **1. Update the system**

```bash
sudo dnf update -y
```

### **2. Install a package**

```bash
sudo dnf install httpd -y
```

---

### **3. Remove a package**

```bash
sudo dnf remove httpd -y
```

---

### **4. Check package info**

```bash
dnf info httpd
```

---

### **5. Search packages**

```bash
dnf search nginx
```

---

### **6. List installed packages**

```bash
dnf list installed
```

---

### **7. Clean cache**

```bash
dnf clean all
```

---

### **8. List package groups**

```bash
dnf group list
```

### **Install a group**

```bash
dnf groupinstall "Development Tools" -y
```

---

# 🧠 **2. YUM – For Older Systems (RHEL 6/7)**

### **Install**

```bash
sudo yum install httpd -y
```

### **Update**

```bash
sudo yum update -y
```

### **Remove**

```bash
sudo yum remove httpd -y
```

### **Search**

```bash
yum search nginx
```

➡️ **Note:** On RHEL 8+, `yum` redirects to `dnf` automatically.

---

# 🧠 **3. RPM – Low-Level Package Tool**

RPM does *not* resolve dependencies.
Used for installing `.rpm` files manually.

---

### **Install `.rpm` file**

```bash
sudo rpm -ivh file.rpm
```

---

### **Upgrade `.rpm` file**

```bash
sudo rpm -Uvh file.rpm
```

---

### **Remove RPM**

```bash
sudo rpm -e package_name
```

---

### **Check installed package**

```bash
rpm -q httpd
```

---

### **List all files installed by a package**

```bash
rpm -ql httpd
```

---

### **Verify package integrity**

```bash
rpm -V httpd
```

---

# 🧩 **4. SysAdmin-Level Automation Examples**

---

## 🧰 **Example 1 – Install Multiple Packages**

```bash
sudo dnf install -y httpd git wget curl vim
```

---

## 🧰 **Example 2 – Script to Install Packages from File**

### packages.txt

```
httpd
vim
curl
tree
```

### script:

```bash
#!/bin/bash

while read pkg
do
    if rpm -q "$pkg" &>/dev/null; then
        echo "$pkg already installed."
    else
        echo "Installing $pkg..."
        dnf install -y "$pkg"
    fi
done < packages.txt
```

✔️ Perfect for server provisioning automation.

---

## 🧰 **Example 3 – Check if Package Is Installed**

```bash
if rpm -q httpd &>/dev/null; then
    echo "httpd is installed."
else
    echo "httpd is NOT installed."
fi
```

---

## 🧰 **Example 4 – Remove Unwanted Packages**

```bash
remove_list=("firefox" "libreoffice" "games")

for pkg in "${remove_list[@]}"
do
    dnf remove -y "$pkg"
done
```

---

## 🧰 **Example 5 – System Update with Logging**

```bash
#!/bin/bash

LOGFILE="/var/log/system_update.log"

echo "===== Update Started: $(date) =====" >> "$LOGFILE"

dnf update -y >> "$LOGFILE" 2>&1

echo "===== Update Completed: $(date) =====" >> "$LOGFILE"
```

✔️ Run this via **cron** or **systemd timer**.

---

# 🧠 **5. Repository Management (Important for SysAdmins)**

### **List enabled repos**

```bash
dnf repolist
```

---

### **List disabled repos**

```bash
dnf repolist all
```

---

### **Disable a repository**

```bash
sudo dnf config-manager --set-disabled repository_name
```

---

### **Enable a repository**

```bash
sudo dnf config-manager --set-enabled repository_name
```

---

### **Add a repository (.repo file)**

```bash
sudo vi /etc/yum.repos.d/custom.repo
```

Example:

```
[customrepo]
name=Custom Repo
baseurl=http://repo.example.com/packages/
enabled=1
gpgcheck=0
```

---

# 🧠 **6. Installing Local `.rpm` Packages**

### **Download:**

```bash
wget https://example.com/file.rpm
```

### **Install using rpm:**

```bash
rpm -ivh file.rpm
```

If dependency error occurs:

```bash
dnf install -y file.rpm
```

✔️ DNF handles dependencies automatically.

---

# 🧾 **7. Quick Comparison Table**

| Feature                 | DNF | YUM            | RPM        |
| ----------------------- | --- | -------------- | ---------- |
| Resolves dependencies   | ✔️  | ✔️             | ❌          |
| Installs `.rpm`         | ✔️  | ✔️             | ✔️         |
| Modern (RHEL 8/9)       | ✔️  | Redirect → dnf | ✔️         |
| Installs package groups | ✔️  | ✔️             | ❌          |
| Useful for scripting    | ✔️  | ✔️             | ⚠️ Limited |
| Repo management         | ✔️  | ✔️             | ❌          |

---

# 💡 SysAdmin Best Practices

✔️ Use **DNF** for all package installs on RHEL 8/9
✔️ Use **RPM** only when needed (manual `.rpm` installs)
✔️ Always run `dnf update -y` before installing packages
✔️ Log package installation in automation scripts
✔️ Use **package lists** for server provisioning
✔️ Clean cache periodically (`dnf clean all`)
✔️ Always check package availability with `dnf info`

---

