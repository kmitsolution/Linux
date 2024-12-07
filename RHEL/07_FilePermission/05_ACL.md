### **Access Control Lists (ACL) in RHEL**

Access Control Lists (ACLs) in Linux provide a more granular level of permission management than the traditional owner/group/other permission scheme. ACLs allow you to define permissions for individual users or groups beyond the basic owner/group/other permissions model. This helps in more complex scenarios where you need to give specific permissions to multiple users or groups on the same file.

---

### **How to Check if ACL is Supported by the File System**

To check whether ACLs are supported by the file system, you can follow these steps:

1. **Check `/etc/fstab`**:
   - In the `/etc/fstab` file, if the file system is mounted with the `acl` option, ACL support is enabled. The `defaults` option typically includes ACL support by default on modern systems.
   
   Example line from `/etc/fstab`:
   ```
   /dev/sda1  /  ext4  defaults,acl  1  1
   ```

   - **`acl`** indicates that ACL is enabled for the file system.

2. **Check the file system with `df -Th`**:
   - Run `df -Th` to view the type of file systems mounted and whether they support ACL:
   ```bash
   df -Th
   ```

   - The output will show the file system types, and you can confirm if it supports ACL.

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

### **Working with Mask and No-Mask Calculations**

1. **Set ACL Without Mask Calculation:**

   By default, when setting ACLs, the permissions are constrained by the mask, which is the **maximum** allowed permissions for users and groups. However, you can override this behavior and set ACLs without applying the mask using `--no-mask`:

   ```bash
   setfacl --no-mask -m u:raman:rw filename
   ```

   This will grant **read** and **write** permissions to `nehra` without considering the mask. Normally, the mask would limit the permissions to the maximum allowed, but in this case, it is ignored.

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
