### **Access Control Lists (ACL) in RHEL**

Access Control Lists (ACLs) in Linux provide a more granular level of permission management than the traditional owner/group/other permission scheme. ACLs allow you to define permissions for individual users or groups beyond the basic owner/group/other permissions model. This helps in more complex scenarios where you need to give specific permissions to multiple users or groups on the same file.

---

### **Working with ACLs**

1. **Displaying ACL of a File:**

   To view the ACL of a file, use the `getfacl` command:

   ```bash
   getfacl filename
   ```

   Example output:
   ```
   # file: filename
   # owner: raman
   # group: alice
   user::rw-
   user:john:rw-
   group::r--
   mask::rw-
   other::r--
   ```

   - This shows the file's ACL, listing the permissions for the file owner, groups, users, and others.

2. **Setting ACL for a User (Add Permissions):**

   To set permissions for a user, use the `setfacl` command:

   ```bash
   setfacl -m u:username:permissions filename
   ```

   Example:
   ```bash
   setfacl -m u:john:rw filename
   ```

   This will grant **read** and **write** (`rw`) permissions to the user `john` for `filename`.

   - **Mask**: The mask defines the maximum allowed permissions for users and groups (excluding the owner). It acts as a limit for the effective ACL permissions for users/groups. 

3. **Setting ACL for a Group:**

   To set permissions for a group, use the `setfacl` command:

   ```bash
   setfacl -m g:groupname:permissions filename
   ```

   Example:
   ```bash
   setfacl -m g:admin:rwx filename
   ```

   This will grant **read**, **write**, and **execute** (`rwx`) permissions to the group `admin` for the `filename`.

4. **Checking ACL After Modifying Permissions:**

   After applying the `setfacl` command, you can verify the new ACL settings with:

   ```bash
   getfacl filename
   ```

   Example output:
   ```
   # file: filename
   # owner: raman
   # group: alice
   user::rw-
   user:john:rw-
   group::r--
   group:admin:rwx
   mask::rw-
   other::r--
   ```

5. **Removing ACL for a User:**

   To remove a specific ACL for a user, use the following command:

   ```bash
   setfacl -x u:username filename
   ```

   Example:
   ```bash
   setfacl -x u:john filename
   ```

   This command will remove the ACL entry for user `john` on `filename`.

6. **Removing All ACLs from a File:**

   To remove all ACL entries from a file, use the `setfacl -b` command:

   ```bash
   setfacl -b filename
   ```

   This command will clear all ACL entries on the specified file, effectively returning it to the default permission scheme.

---
## 🔐 What is **mask** in ACL (Access Control List)?

In ACL, the **mask** acts like a **maximum permission filter**.

👉 Simple definition:

> **Mask defines the maximum effective permissions for group and named users in ACL**

---

## 🧠 Why mask is needed?

Even if you give full permissions to a user in ACL, the **mask can restrict it**.

👉 So actual permission =
**ACL permission ∩ Mask permission**

---

## 📌 Example

### Step 1: Create file

```bash
touch file1
```

---

### Step 2: Give ACL to user `manoj`

```bash
setfacl -m u:manoj:rwx file1
```

---

### Step 3: Check ACL

```bash
getfacl file1
```

Output:

```
user::rw-
user:manoj:rwx
group::r--
mask::rwx
other::r--
```

👉 Here:

* manoj = `rwx`
* mask = `rwx`
  ✔ Effective = `rwx`

---

## 🔒 Now restrict using mask

```bash
setfacl -m m::r-- file1
```

Check again:

```
user::rw-
user:manoj:rwx   #effective:r--
group::r--       #effective:r--
mask::r--
other::r--
```

👉 Even though:

* manoj has `rwx`
* Mask allows only `r--`

✔ Final permission = **r-- only**

---

## 🔍 Key observations

* Mask affects:

  * Named users (like `manoj`)
  * Group permissions

* Mask does NOT affect:

  * Owner (`user::`)
  * Others (`other::`)

---

## 🧠 Important concept

👉 Think of mask like a **ceiling (limit)**
Even if you grant more, mask will **cap it**

---

## 📊 Quick Formula

Effective permission:

```
Actual = ACL permission AND Mask
```

---

## ⚙️ Set mask manually

```bash
setfacl -m m::rx file1
```

---

## 🔍 Remove ACL (reset)

```bash
setfacl -b file1
```

---

## 🎯 Real-world use case

* You want to give `rwx` to multiple users
* But restrict them temporarily → change mask instead of editing each user

---


### **Working with Mask and No-Mask Calculations**

1. **Set ACL Without Mask Calculation:**

   By default, when setting ACLs, the permissions are constrained by the mask, which is the **maximum** allowed permissions for users and groups. However, you can override this behavior and set ACLs without applying the mask using `--no-mask`:

   ```bash
   setfacl --no-mask -m u:raman:rw filename
   ```

   This will grant **read** and **write** permissions to `raman` without considering the mask. Normally, the mask would limit the permissions to the maximum allowed, but in this case, it is ignored.

2. **Check the File's ACL with `getfacl`:**

   After using the `--no-mask` option, check the file's ACL with:

   ```bash
   getfacl filename
   ```

   Example output:

   ```
   # file: filename
   # owner: raman
   # group: alice
   user::rw-
   user:raman:rw-
   group::r--
   mask::rw-
   other::r--
   ```

   - You can see that `raman` has `rw-` permissions, despite the mask (`rw-`).

---

### **Understanding ACL in `/etc/group`**

You can check which users are members of a group by looking at `/etc/group`. To view the groups and users associated with them, you can use:

```bash
cat /etc/group
```

Example output:

```
admin:x:1001:user1,user2
alice:x:1002:user3,user4
```

- In this case, `user1` and `user2` belong to the `admin` group, and `user3` and `user4` belong to the `alice` group.
- If a group has write or execute permission for a file, the members of that group will have those permissions applied through ACLs.

---

### **Example of ACL Commands**

1. **Add user `john` with `read` permission on a file:**

   ```bash
   setfacl -m u:john:r filename
   ```

2. **Add group `admin` with `read`, `write`, and `execute` permissions:**

   ```bash
   setfacl -m g:admin:rwx filename
   ```

3. **Remove `john`’s ACL:**

   ```bash
   setfacl -x u:john filename
   ```

4. **Remove all ACLs from the file:**

   ```bash
   setfacl -b filename
   ```

---

### **Conclusion**

- **ACL** allows more granular control over permissions for files and directories, extending the basic owner/group/other permissions model.
- You can use commands like `getfacl` and `setfacl` to view and modify ACLs for users and groups.
- The **mask** defines the maximum allowed permissions, but it can be overridden using the `--no-mask` option in `setfacl`.
