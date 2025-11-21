# 📦 **Package Management in Ubuntu (APT & dpkg)**

Ubuntu uses **APT (Advanced Package Tool)** as its main package manager.
It also uses **dpkg** for low-level package operations.

As a SysAdmin, you'll manage:
✔️ Installing packages
✔️ Updating the system
✔️ Removing packages
✔️ Checking installed software
✔️ Automating package installation via scripts

---

# 🧠 **1. APT – The High-Level Package Manager**

APT resolves dependencies automatically and is the recommended tool for everyday use.

---

# ⚙️ **Basic APT Commands**

### **1. Update Package Index**

```bash
sudo apt update
```

Updates the list of available packages.

---

### **2. Upgrade Installed Packages**

```bash
sudo apt upgrade -y
```

Installs newer versions of installed packages.

---

### **3. Install a Package**

```bash
sudo apt install package_name -y
```

✔️ `-y` auto-confirms
✔️ Installs dependencies automatically

Example:

```bash
sudo apt install nginx -y
```

---

### **4. Remove a Package (Keep Config Files)**

```bash
sudo apt remove package_name -y
```

---

### **5. Remove Package + Config Files**

```bash
sudo apt purge package_name -y
```

---

### **6. Search for Packages**

```bash
apt search package_name
```

---

### **7. Show Package Details**

```bash
apt show package_name
```

---

### **8. List Installed Packages**

```bash
dpkg -l
```

---

### **9. Auto-remove Unused Dependencies**

```bash
sudo apt autoremove -y
```

---

# 📦 **2. dpkg – Low-Level Package Manager**

`dpkg` handles `.deb` files but **does NOT resolve dependencies**.

---

## ⚙️ **Install a .deb File**

```bash
sudo dpkg -i file.deb
```

If dependency errors occur:

```bash
sudo apt --fix-broken install -y
```

---

## ⚙️ **List Installed Packages**

```bash
dpkg -l | grep package_name
```

---

## ⚙️ **Remove Installed .deb**

```bash
sudo dpkg -r package_name
```

---

## ⚙️ **Get the Install Location**

```bash
dpkg -L package_name
```

---

# 🧩 **3. SysAdmin-Level Examples**

---

## 🧰 **Example 1 – Install Multiple Packages at Once**

```bash
sudo apt install -y nginx git curl wget vim
```

---

## 🧰 **Example 2 – Check if a Package Is Installed**

```bash
dpkg -l | grep nginx >/dev/null 2>&1
if [ $? -eq 0 ]; then
    echo "nginx is installed."
else
    echo "nginx is NOT installed."
fi
```

---

## 🧰 **Example 3 – Installing Packages from a List**

### **packages.txt**

```
nginx
curl
git
htop
```

### **install_packages.sh**

```bash
#!/bin/bash

while read pkg
do
    if dpkg -l | grep -w "$pkg" >/dev/null 2>&1; then
        echo "$pkg already installed."
    else
        echo "Installing $pkg..."
        apt install -y "$pkg"
    fi
done < packages.txt
```

✔️ *Ideal for new server provisioning*

---

## 🧰 **Example 4 – Remove Unwanted Software**

```bash
to_remove=("apache2" "snapd" "games")

for pkg in "${to_remove[@]}"
do
    sudo apt purge -y "$pkg"
done

sudo apt autoremove -y
```

---

## 🧰 **Example 5 – System Update Script**

```bash
#!/bin/bash

LOGFILE="/var/log/ubuntu_update.log"

echo "===== Update Started at $(date) =====" >> "$LOGFILE"

apt update >> "$LOGFILE" 2>&1
apt upgrade -y >> "$LOGFILE" 2>&1
apt autoremove -y >> "$LOGFILE" 2>&1

echo "===== Update Completed at $(date) =====" >> "$LOGFILE"
```

✔️ You can schedule this using cron for weekly updates.

---

# 🧠 **4. APT Repository Management**

### **Add a PPA Repository**

```bash
sudo add-apt-repository ppa:repo/name -y
sudo apt update
```

---

### **Remove a PPA**

```bash
sudo add-apt-repository -r ppa:repo/name -y
```

---

### **Add .deb Repo (Advanced)**

Edit:

```
/etc/apt/sources.list
```

Or create a file in:

```
/etc/apt/sources.list.d/
```

Then run:

```bash
sudo apt update
```

---

# 🧠 **5. Cleaning Package Cache**

### **Clear APT Cache**

```bash
sudo apt clean
```

### **Clear Partial Downloads**

```bash
sudo apt autoclean
```

---

# 🧾 **6. Quick Reference Table**

| Task                  | Command                    |
| --------------------- | -------------------------- |
| Update index          | `apt update`               |
| Install               | `apt install package`      |
| Remove                | `apt remove package`       |
| Purge (remove config) | `apt purge package`        |
| Upgrade packages      | `apt upgrade`              |
| List installed        | `dpkg -l`                  |
| Install .deb local    | `dpkg -i file.deb`         |
| Fix dependencies      | `apt --fix-broken install` |
| Search                | `apt search package`       |
| Show package info     | `apt show package`         |

---

# 💡 SysAdmin Best Practices (Ubuntu)

✔️ Always run: `apt update` before installing anything
✔️ Use `apt install -y` in automation
✔️ Use log files for update & installation scripts
✔️ Remove unused packages with `autoremove`
✔️ Never install `.deb` files without checking dependencies
✔️ For multiple packages, use **script + package list**
✔️ Use cron or systemd timers for automatic updates

---


