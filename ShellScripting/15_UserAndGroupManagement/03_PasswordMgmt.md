
# 🔐 **Password Management in Shell Scripting (RHEL & Ubuntu)**

Password management is a core task for SysAdmins, especially when onboarding users, enforcing password policies, resetting credentials, or automating secure access setups.

We will cover:
1️⃣ Setting user passwords
2️⃣ Forcing password change
3️⃣ Password expiry policies
4️⃣ Generating random passwords
5️⃣ Bulk password management
6️⃣ RHEL vs Ubuntu differences

---

# 🧠 **1. Setting Password for a User**

### **Using `passwd`**

Interactive:

```bash
passwd username
```

Non-interactive (automation):

```bash
echo "username:NewPass123" | chpasswd
```

✔️ Works in both RHEL & Ubuntu
✔️ Ideal for automation

---

# 🧩 **Example 1 – Setting Password Automatically**

```bash
#!/bin/bash

user="developer"
password="Dev@12345"

echo "$user:$password" | chpasswd
echo "Password set for $user"
```

---

# 🧠 **2. Forcing Password Change on Next Login**

This is a **compliance requirement** in most organizations.

```bash
passwd -e username
```

✔️ For RHEL & Ubuntu
✔️ Forces user to reset password immediately after login

---

# 🧩 **Example 2 – Create User and Force Password Reset**

```bash
#!/bin/bash

user="newuser"
pass="TempPass@123"

useradd -m "$user"
echo "$user:$pass" | chpasswd
passwd -e "$user"

echo "User $user created with temporary password."
```

---

# 🧠 **3. Password Aging & Policies (chage)**

`chage` allows you to control password expiry.

| Option | Meaning                             |
| ------ | ----------------------------------- |
| `-M`   | Maximum days before password change |
| `-m`   | Minimum days                        |
| `-W`   | Warning before expiry               |
| `-E`   | Expiry date                         |

---

### 🧩 **Example 3 – Set Strong Password Policy**

```bash
chage -M 90 -m 7 -W 14 username
```

✔️ Max age = 90 days
✔️ Min age = 7 days
✔️ Warn user 14 days before expiry

---

# 🧠 **4. Generate Secure Random Passwords**

### **Method 1: Using `openssl`**

```bash
openssl rand -base64 12
```

### **Method 2: Using `/dev/urandom`**

```bash
tr -dc 'A-Za-z0-9@#$%' </dev/urandom | head -c 12
```

---

# 🧩 **Example 4 – Create User with Random Password**

```bash
#!/bin/bash

user="devuser"
password=$(openssl rand -base64 12)

useradd -m "$user"
echo "$user:$password" | chpasswd
passwd -e "$user"

echo "User: $user"
echo "Temp Password: $password"
```

📘 *Use Case:* Onboarding scripts for new developers or employees.

---

# 🧠 **5. Bulk Password Reset Script**

Useful for mass password resets (e.g., compliance or security events).

### **users.txt**

```
alice
bob
charlie
```

### **Script**

```bash
#!/bin/bash

while read user
do
    if id "$user" &>/dev/null; then
        pass=$(openssl rand -base64 12)
        echo "$user:$pass" | chpasswd
        passwd -e "$user"
        echo "Updated password for $user: $pass"
    else
        echo "User $user does not exist."
    fi
done < users.txt
```

📘 *Use Case:* Security audits or organization-wide password rotations.

---

# 🧠 **6. Check Password Status**

```bash
chage -l username
```

Example output:

```
Last password change                                    : Nov 08, 2025
Password expires                                         : Feb 06, 2026
Password inactive                                        : never
Account expires                                          : never
```

✔️ Helps SysAdmins audit password aging.

---

# 🧠 **7. Locking & Unlocking User Accounts**

### **Lock User**

```bash
usermod -L username
```

### **Unlock User**

```bash
usermod -U username
```

✔️ “L” means lock, “U” means unlock
✔️ Used for security incidents, staff leave, or expired contracts

---

# 🧩 **Example 5 – Lock Idle Users**

```bash
#!/bin/bash

inactive_days=30

for user in $(awk -F: '{print $1}' /etc/passwd)
do
    last_login=$(lastlog -u "$user" | awk 'NR==2 {print $4}')
    if [[ "$last_login" == "**Never logged in**" ]]; then
        continue
    fi
done
```

(This can be expanded depending on what you need.)

---

# 🧠 **8. RHEL vs Ubuntu Differences**

| Task                | RHEL                     | Ubuntu                            |
| ------------------- | ------------------------ | --------------------------------- |
| Add user command    | `useradd`                | `adduser` (friendly) or `useradd` |
| Password file       | `/etc/shadow`            | `/etc/shadow`                     |
| Password aging tool | `chage`                  | `chage`                           |
| Auth logs           | `/var/log/secure`        | `/var/log/auth.log`               |
| PAM config          | `/etc/pam.d/system-auth` | `/etc/pam.d/common-password`      |

### ✔️ Important difference

**Ubuntu’s `adduser`** is an interactive wrapper around `useradd`,
while **RHEL SysAdmins primarily use `useradd`** for automation.

---

# 🧾 **Quick Summary**

| Task                  | Command                       |           |
| --------------------- | ----------------------------- | --------- |
| Set user password     | `echo "user:pass"             | chpasswd` |
| Force password change | `passwd -e user`              |           |
| Set aging policy      | `chage -M 90 -m 7 -W 14 user` |           |
| Random password       | `openssl rand -base64 12`     |           |
| Lock user             | `usermod -L user`             |           |
| Unlock user           | `usermod -U user`             |           |
| Check aging status    | `chage -l user`               |           |

---

# 💡 SysAdmin Best Practices

✔️ Never hardcode passwords in scripts
✔️ Force password change for new users
✔️ Store temporary passwords securely
✔️ Log all password changes (`logger`, log files)
✔️ Rotate passwords regularly
✔️ Use `chage` to enforce policies
✔️ Use strong random passwords (`openssl`)
✔️ Use `getent passwd` to check user existence in enterprise environments

---


