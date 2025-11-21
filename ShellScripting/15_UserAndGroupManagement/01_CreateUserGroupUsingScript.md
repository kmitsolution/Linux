# 👤 **User and Group Management via Script (RHEL & Ubuntu)**

---

## 🧠 **Why Automate User Management?**

✅ To save time when adding multiple users (e.g., developers or project accounts)
✅ To maintain consistency across servers
✅ To enforce security and password policies automatically
✅ To integrate with deployment pipelines (DevOps onboarding automation)

---

## ⚙️ **1. Basic Commands for User and Group Management**

| Task              | Command                          | Example                         |
| ----------------- | -------------------------------- | ------------------------------- |
| Create user       | `useradd username`               | `useradd devops`                |
| Create group      | `groupadd groupname`             | `groupadd developers`           |
| Add user to group | `usermod -aG groupname username` | `usermod -aG developers devops` |
| Delete user       | `userdel username`               | `userdel devops`                |
| Delete group      | `groupdel groupname`             | `groupdel developers`           |
| Set password      | `passwd username`                | `passwd devops`                 |
| Check user info   | `id username`                    | `id devops`                     |
| List all users    | `cat /etc/passwd`                |                                 |
| List all groups   | `cat /etc/group`                 |                                 |

---

## 🧩 **2. Example – Creating a Single User**

```bash
#!/bin/bash

read -p "Enter username to create: " username
useradd -m "$username"
passwd "$username"
echo "✅ User '$username' created successfully!"
```

📘 *Use Case:* Interactive script for creating one user with a home directory.

---

## 🧩 **3. Example – Creating a Group and Assigning a User**

```bash
#!/bin/bash

group="devteam"
user="developer"

groupadd "$group"
useradd -m -G "$group" "$user"

echo "User '$user' created and added to group '$group'."
```

📘 *Use Case:* Automate onboarding for new project teams.

---

## ⚙️ **4. Example – Bulk User Creation (From a File)**

You can maintain a simple file (e.g., `users.txt`) like:

```
alice
bob
charlie
```

Then automate user creation:

```bash
#!/bin/bash
input="users.txt"

while read username
do
    if id "$username" &>/dev/null; then
        echo "⚠️  User $username already exists."
    else
        useradd -m "$username"
        echo "User $username created successfully."
    fi
done < "$input"
```

📘 *Use Case:* Create multiple users at once — ideal for training environments or new departments.

---

## ⚙️ **5. Example – Creating Users with Default Passwords**

```bash
#!/bin/bash
input="users.txt"
default_pass="ChangeMe@123"

while read user
do
    if id "$user" &>/dev/null; then
        echo "User $user already exists."
    else
        useradd -m "$user"
        echo "$user:$default_pass" | chpasswd
        passwd -e "$user"  # Force password change on next login
        echo "✅ User $user created with default password."
    fi
done < "$input"
```

📘 *Use Case:* Enforce initial password reset for compliance and security.

---

## ⚙️ **6. Example – Creating Groups Dynamically**

```bash
#!/bin/bash

groups=("developers" "admins" "qa")

for grp in "${groups[@]}"
do
    if getent group "$grp" >/dev/null; then
        echo "Group $grp already exists."
    else
        groupadd "$grp"
        echo "Group $grp created."
    fi
done
```

📘 *Use Case:* Automate environment setup with predefined groups.

---

## 🧩 **7. Example – Assign Multiple Users to a Group**

```bash
#!/bin/bash

group="developers"
users=("alice" "bob" "charlie")

for user in "${users[@]}"
do
    if id "$user" &>/dev/null; then
        usermod -aG "$group" "$user"
        echo "Added $user to group $group."
    else
        echo "User $user does not exist!"
    fi
done
```

📘 *Use Case:* Quickly assign multiple users to a new project group.

---

## ⚙️ **8. Example – Delete Users Safely**

```bash
#!/bin/bash

read -p "Enter username to delete: " user

if id "$user" &>/dev/null; then
    userdel -r "$user"
    echo "User $user deleted successfully."
else
    echo "User $user not found."
fi
```

✅ `-r` → removes the user’s home directory too.
📘 *Use Case:* Cleanup user accounts when employees leave or access expires.

---

## ⚙️ **9. Example – Check for Existing User or Group**

```bash
#!/bin/bash

user="admin"
group="admins"

if id "$user" &>/dev/null; then
    echo "✅ User $user exists."
else
    echo "❌ User $user not found."
fi

if getent group "$group" >/dev/null; then
    echo "✅ Group $group exists."
else
    echo "❌ Group $group not found."
fi
```

📘 *Use Case:* Verification step in deployment scripts.

---

## ⚙️ **10. Example – Combining Everything (Full Automation Script)**

```bash
#!/bin/bash
# user_management.sh

group="devops"
users=("rheladmin" "ubadmin" "qaengineer")

# Ensure group exists
if ! getent group "$group" >/dev/null; then
    groupadd "$group"
    echo "Group $group created."
fi

for user in "${users[@]}"
do
    if id "$user" &>/dev/null; then
        echo "User $user already exists."
    else
        useradd -m -G "$group" "$user"
        echo "$user:TempPass@123" | chpasswd
        passwd -e "$user"
        echo "✅ User $user created and added to $group."
    fi
done

echo "All users and group setup completed successfully."
```

📘 *Use Case:* Full automation of user creation for new projects or server onboarding.

---

## ⚙️ **11. Ubuntu vs RHEL Differences**

| Task                  | Ubuntu                   | RHEL                        |
| --------------------- | ------------------------ | --------------------------- |
| Add user              | `adduser` (interactive)  | `useradd` (non-interactive) |
| Default home creation | Automatic with `adduser` | Use `-m` with `useradd`     |
| Group management      | `addgroup`, `usermod`    | `groupadd`, `usermod`       |
| Password change       | `passwd username`        | `passwd username`           |
| Logs                  | `/var/log/auth.log`      | `/var/log/secure`           |

✅ *Tip:* In Ubuntu, prefer `adduser` for simplicity.
In RHEL, use `useradd` for scripting flexibility.

---

## 🧾 **12. Quick Reference Table**

| Command                     | Description                     |
| --------------------------- | ------------------------------- |
| `useradd -m username`       | Create user with home directory |
| `useradd -G group username` | Add user to a group             |
| `groupadd groupname`        | Create a new group              |
| `passwd username`           | Set or change password          |
| `usermod -aG group user`    | Add existing user to a group    |
| `userdel -r username`       | Delete user with home directory |
| `getent group groupname`    | Check if group exists           |
| `id username`               | Display user’s UID/GID info     |

---

## 💡 **SysAdmin Best Practices**

1. Always validate if a user/group exists before creating or deleting.
2. Use **default passwords + force change** (`passwd -e`) for security.
3. Keep user creation logs (`>> /var/log/user_management.log`).
4. Use arrays or input files for bulk user creation.
5. Follow naming conventions (e.g., lowercase usernames).
6. Never hardcode sensitive passwords; use environment variables or secure vaults.
7. Automate cleanup (deactivation + removal after X days of inactivity).

---


