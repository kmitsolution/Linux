### **1. `/etc/shadow` File**
The `/etc/shadow` file stores encrypted password information for user accounts and additional account-related data such as password expiration and account locking.

**Structure of `/etc/shadow`:**

```bash
username:$1$xyz$abc...:18035:0:99999:7:::
```

- `username`: The username of the account.
- `$1$xyz$abc...`: The hashed password (it could also be `*` or `!` if the account is locked).
- `18035`: The last password change (as the number of days since January 1, 1970).
- `0`: The minimum number of days between password changes.
- `99999`: The maximum number of days a password is valid.
- `7`: The number of days before the password expires and the user is warned.

### **2. `/etc/login.defs` File**
The `/etc/login.defs` file defines configuration parameters for user account management. It contains various settings, such as default values for password expiration, UID ranges, and the size of the user home directory.

**Important fields in `/etc/login.defs`:**

- **`PASS_MAX_DAYS`**: The maximum number of days a password can be used before it must be changed.
- **`PASS_MIN_DAYS`**: The minimum number of days required between password changes.
- **`PASS_WARN_AGE`**: The number of days before a password expires when the user is warned.

**Example of `/etc/login.defs`:**
```bash
PASS_MAX_DAYS   99999
PASS_MIN_DAYS   0
PASS_WARN_AGE   7
```

This means:
- Passwords can last up to 99999 days.
- There is no minimum password age.
- Users will be warned 7 days before their password expires.

### **3. `chage -l <username>`**
The `chage` command is used to change or display the user's password expiration information. The `-l` option lists the user's current password expiry information.

**Syntax:**
```bash
chage -l username
```

**Example:**
```bash
chage -l alice
```

**Output:**
```bash
Last password change                                    : Dec 06, 2024
Password expires                                        : Dec 20, 2024
Password inactive                                       : never
Account expires                                         : never
Minimum number of days between password change          : 0
Maximum number of days between password change          : 14
Number of days of warning before password expires       : 7
```

### **4. `passwd -l <username>`**
The `passwd` command with the `-l` option locks a user account by disabling the password. This effectively prevents the user from logging in.

**Syntax:**
```bash
sudo passwd -l username
```

**Example:**
```bash
sudo passwd -l alice
```

This will lock the `alice` account by setting an invalid password hash in `/etc/shadow`, preventing login.

### **5. `passwd -u <username>`**
The `passwd` command with the `-u` option unlocks a user account that was previously locked using `passwd -l`.

**Syntax:**
```bash
sudo passwd -u username
```

**Example:**
```bash
sudo passwd -u alice
```

This will unlock the `alice` account by re-enabling the password hash in `/etc/shadow`, allowing the user to log in again.

### **6. `usermod -L <username>`**
The `usermod` command with the `-L` option locks a user account. This option adds an exclamation mark (`!`) at the beginning of the encrypted password in the `/etc/shadow` file, effectively disabling the account.

**Syntax:**
```bash
sudo usermod -L username
```

**Example:**
```bash
sudo usermod -L alice
```

This locks the `alice` user account by disabling their password, preventing login.

### **7. `usermod -U <username>`**
The `usermod` command with the `-U` option unlocks a user account that was previously locked with `usermod -L`. It removes the exclamation mark (`!`) from the encrypted password in `/etc/shadow`.

**Syntax:**
```bash
sudo usermod -U username
```

**Example:**
```bash
sudo usermod -U alice
```

This unlocks the `alice` account, re-enabling their password and allowing login again.

### **8. `vipw`**
The `vipw` command is used to safely edit the `/etc/passwd` and `/etc/shadow` files. It opens the `/etc/passwd` file (or `/etc/shadow` if specified) in the `vi` editor, ensuring the file is locked to prevent concurrent editing.

**Syntax:**
```bash
sudo vipw
```

**Example:**
```bash
sudo vipw
```

This opens `/etc/passwd` in the `vi` editor. After making changes, you can save and exit by typing `:wq`.

`vipw` ensures that only one user edits the file at a time, preventing file corruption.

### **9. `/etc/passwd` and `nologin`**
The `/etc/passwd` file contains user account information. If you want to prevent a user from logging in, you can set their shell to `/sbin/nologin`. This is commonly done for system accounts or when a user is temporarily disabled.

**Example:**
```bash
sudo usermod -s /sbin/nologin username
```

This sets the shell for `username` to `/sbin/nologin`, preventing them from logging into the system.

You can also manually edit the `/etc/passwd` file using `vipw` or `vigr` to change the user's shell to `/sbin/nologin`.

### **10. `vigr`**
The `vigr` command is used to safely edit the `/etc/group` and `/etc/sudoers` files. It works similarly to `vipw`, but it is specifically for editing group files and the sudoers file, and it ensures that the file is locked during editing.

**Syntax:**
```bash
sudo vigr
```

**Example:**
```bash
sudo vigr
```

This opens the `/etc/sudoers` file (or `/etc/group` if specified) in `vi` for editing. It checks the syntax when you save and exit, preventing errors that could lock you out of sudo access.

---

### **Summary of Commands with Examples:**

1. **`/etc/shadow`**: Stores encrypted passwords and account expiration details.
2. **`/etc/login.defs`**: Contains default settings for user account creation (password aging, UID/GID ranges).
3. **`chage -l <username>`**: Displays password expiration information for a user.
   ```bash
   chage -l alice
   ```
4. **`passwd -l <username>`**: Locks the user account by disabling the password.
   ```bash
   sudo passwd -l alice
   ```
5. **`passwd -u <username>`**: Unlocks a user account, re-enabling the password.
   ```bash
   sudo passwd -u alice
   ```
6. **`usermod -L <username>`**: Locks the user account by adding `!` to the password field in `/etc/shadow`.
   ```bash
   sudo usermod -L alice
   ```
7. **`usermod -U <username>`**: Unlocks a user account by removing `!` from the password field in `/etc/shadow`.
   ```bash
   sudo usermod -U alice
   ```
8. **`vipw`**: Safely edits the `/etc/passwd` or `/etc/shadow` files using `vi`.
   ```bash
   sudo vipw
   ```
9. **`/etc/passwd` with `nologin`**: Setting a user's shell to `/sbin/nologin` disables their login.
   ```bash
   sudo usermod -s /sbin/nologin alice
   ```
10. **`vigr`**: Safely edits the `/etc/group` or `/etc/sudoers` files using `vi`.
   ```bash
   sudo vigr
   ```

These commands are essential for managing user accounts and permissions in Linux, and they offer fine-grained control over user access and account security.
