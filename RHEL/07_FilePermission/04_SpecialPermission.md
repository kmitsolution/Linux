### **Understanding Special Permissions in Linux:**

In Linux, there are special permissions that can be set on files and directories to modify the behavior of those files in certain ways. The special permissions include the **sticky bit**, **setuid**, and **setgid**. Here's a detailed explanation of each special permission with examples:

---

### **1. Sticky Bit: `chmod +t`**

The **sticky bit** is a special permission that can be applied to a directory. When set, it allows only the owner of a file within that directory to delete or rename the file, even if other users have write permissions to the directory.

#### **Syntax**:
```bash
chmod +t directory_name
```

#### **Example 1: Setting the Sticky Bit**
Let's consider the `/tmp` directory, which is typically used for temporary file storage. We want to ensure that only the owner of a file can delete or rename their files in this directory.

1. **Set the sticky bit on `/tmp`**:
   ```bash
   sudo chmod +t /tmp
   ```

2. **Check the permissions**:
   ```bash
   ls -ld /tmp
   ```

   Output:
   ```
   drwxrwxrwt 18 root root 4096 Dec  7 12:00 /tmp
   ```

   The **`t`** at the end indicates that the sticky bit is set. This ensures that users can only delete or rename their own files in the `/tmp` directory.

---

### **2. `chmod 1777`**

The **`1777`** permission is a combination of:
- `1` (setuid, setgid, or sticky bit)
- `7` (rwx for owner, group, and others)

When applied to a directory, it ensures that:
- **Setuid** or **Setgid** are not needed for the directory but the sticky bit (`1`) is active.
- The directory has full read, write, and execute permissions for everyone, but only the file owner can delete or rename their files.

#### **Example 2: Set `1777` on `/tmp`**:
1. **Set the sticky bit and full permissions** on `/tmp`:
   ```bash
   sudo chmod 1777 /tmp
   ```

2. **Check the permissions**:
   ```bash
   ls -ld /tmp
   ```

   Output:
   ```
   drwxrwxrwt 18 root root 4096 Dec  7 12:00 /tmp
   ```

---


### **Sticky Bit Example**

The **sticky bit** is a special permission applied to a **directory**, which ensures that only the **owner** of a file within the directory can delete or rename that file, even if other users have write permissions on the directory.

This is often used on directories like `/tmp` to prevent one user from deleting or modifying another user's files.

---

### **Steps to Set and Test Sticky Bit in `/tmp` Directory**

#### **1. Set the Sticky Bit on `/tmp` Directory**

To begin, ensure the **sticky bit** is set on the `/tmp` directory.

```bash
sudo chmod +t /tmp
```

Verify the sticky bit is set by listing the directory details:

```bash
ls -ld /tmp
```

You should see an output like this, with a **`t`** at the end of the permissions:

```
drwxrwxrwt 18 root root 4096 Dec  7 12:00 /tmp
```

This means the sticky bit is set on `/tmp`, allowing users to only delete or rename their own files.

---

#### **2. Create a File with Full Permissions for All Users**

1. **Create a file `file1.txt` in `/tmp` by a user (e.g., `user1`)**:
   
   First, switch to a user (e.g., `user1`), and create a file inside `/tmp`:

   ```bash
   touch /tmp/file1.txt
   ```

2. **Give full permissions to the file for all users**:
   
   Now, set full permissions (`rwx`) for **user**, **group**, and **others** on the file:

   ```bash
   sudo chmod 777 /tmp/file1.txt
   ```

   Verify the permissions:

   ```bash
   ls -l /tmp/file1.txt
   ```

   The output should look like this:

   ```
   -rwxrwxrwx 1 user1 user1 0 Dec  7 12:00 /tmp/file1.txt
   ```

   Here, all users (including `user1` and others) have **read**, **write**, and **execute** permissions.

---

#### **3. Try Deleting the File as a Different User**

1. **Switch to another user** (e.g., `user2`):

   ```bash
   su - user2
   ```

2. **Try to delete the file**:

   Attempt to delete the file `file1.txt` that was created by `user1`:

   ```bash
   rm /tmp/file1.txt
   ```

   You will get a **permission denied** message:

   ```
   rm: cannot remove '/tmp/file1.txt': Operation not permitted
   ```

   This happens because, despite the file having full permissions for all users (`777`), the **sticky bit** on the `/tmp` directory restricts `user2` from deleting the file. Only the **owner** of the file (`user1`) has the permission to delete it, regardless of other users' permissions on the file.

---

#### **4. Owner Can Delete the File**

Now, switch back to the owner of the file (`user1`), and try to delete the file:

```bash
su - user1
rm /tmp/file1.txt
```

Since `user1` is the owner of the file, the file will be successfully deleted:

```
rm: remove regular empty file '/tmp/file1.txt'? y
```

---

### **Explanation of the Sticky Bit Behavior:**

- **Sticky Bit** (`+t`): When set on a directory, it ensures that **only the file's owner** can delete or rename their files inside the directory. Even if the file has full permissions (`777`), other users cannot remove or modify it.
  
- In this case:
  - `user1` (the owner) has full permissions to **read**, **write**, and **execute** the file.
  - **Others** (e.g., `user2`) also have full permissions (`rwx`) on the file, but cannot delete it due to the **sticky bit**.
  - The **sticky bit** ensures that the owner of the file (`user1`) is the only one who can delete or modify it, even though `user2` has write permissions on the file.

---

### **3. Remove Sticky Bit: `chmod -t`**

To **remove the sticky bit**, you can use the `chmod -t` command. This will remove the special permission, allowing users with write access to a directory to delete or rename any file within the directory.

#### **Example 3: Remove the Sticky Bit on `/tmp`**:
```bash
sudo chmod -t /tmp
```

Check the permissions again:
```bash
ls -ld /tmp
```

The sticky bit has been removed, and the directory no longer restricts deletion/renaming based on ownership.

---

### **4. `chmod 2777` for `setgid` on Directories**

When applied to a directory, the **setgid** (`2` in the permission) ensures that any files created in the directory will inherit the group of the directory rather than the user's primary group.

#### **Example 4: Set `2777` on a directory**:
1. **Set the `setgid` on a directory**:
   ```bash
   sudo chmod 2777 /tmp
   ```

2. **Check the directory permissions**:
   ```bash
   ls -ld /tmp
   ```

   Output:
   ```
   drwxrwxrwt 18 root root 4096 Dec  7 12:00 /tmp
   ```

In this case, files created in `/tmp` would inherit the group `root` as the group ownership.

---
### **Understanding `setgid` Permission with Example**

The **`setgid` (Set Group ID)** permission is one of the special permissions in Linux that can be applied to directories and files. When set on a directory, it ensures that all new files created within that directory inherit the **group ownership** of the directory, rather than the user's primary group.

This is helpful when you want all files in a directory to have the same group ownership, regardless of the user who created them.

---

### **Steps to Set Up and Demonstrate `setgid` on a Directory**

#### **1. Create a Directory**

First, create a directory `/tmp/test`:

```bash
mkdir /tmp/test
```

This command will create a directory named `test` inside the `/tmp` directory.

---

#### **2. Change Ownership of the Directory**

Next, set the owner of the directory to `raman` and the group to `alice`:

```bash
chown raman:alice /tmp/test
```

Now, the owner of the directory `/tmp/test` is `raman` and the group ownership is `alice`.

---

#### **3. Verify the Ownership of the Directory**

Check the directory’s permissions and ownership:

```bash
ls -ld /tmp/test
```

Output:
```
drwxr-xr-x 2 raman alice 4096 Dec  7 12:00 /tmp/test
```

Here:
- The directory `/tmp/test` is owned by user `raman` and group `alice`.
- The permissions for the directory are `rwxr-xr-x` (read, write, execute for the owner; read, execute for the group and others).

---

#### **4. Set the `setgid` Permission on the Directory**

Now, apply the **setgid** permission on the directory `/tmp/test`:

```bash
chmod 2777 /tmp/test
```

The permission `2777` consists of:
- **2**: This is the setgid permission (set group ID).
- **777**: This grants full read, write, and execute permissions to the owner, group, and others.

After applying the setgid permission, verify the changes:

```bash
ls -ld /tmp/test
```

Output:
```
drwxrwxrwx 2 raman alice 4096 Dec  7 12:00 /tmp/test
```

- Notice that the directory now has the **`s`** in the group’s execute permission (rwx), which indicates that the **setgid** bit has been set.
- The setgid bit does not change the basic permissions of the directory but adds a special feature, which we will explore next.

---

#### **5. Create a New File in the Directory**

Now, create a new file `test.java` inside the `/tmp/test` directory:

```bash
touch /tmp/test/test.java
```

After running this command, the new file `test.java` is created inside `/tmp/test`.

---

#### **6. Verify the Ownership of the New File**

Check the ownership of the newly created file:

```bash
ls -l /tmp/test
```

Output:
```
-rw-r--r-- 1 raman alice 0 Dec  7 12:00 /tmp/test/test.java
```

Here’s what happens:
- **Owner**: The owner of the new file `test.java` is `raman`.
- **Group**: The group ownership of `test.java` is `alice`, **not the user's primary group**.

### **Explanation of What Happened:**

- The **setgid** permission was applied to the directory `/tmp/test`. As a result:
  - Any new file created inside `/tmp/test` automatically inherits the **group ownership** of the directory (`alice`), regardless of the user who creates it.
  - Even though `raman` is the user creating the file, the file's group ownership is set to `alice` (the group of the directory).
  
This behavior ensures that all files created within the directory have the same group ownership, which can be useful in collaborative environments where users need to share files within a specific group.

---



### **5. `chmod g+s` and `chmod g-s` (setgid and remove setgid)**

- **`chmod g+s`**: Set the **setgid** bit on a directory or file.
- **`chmod g-s`**: Remove the **setgid** bit from a directory or file.

#### **Example 5: Set and Remove the `setgid` bit**

1. **Set the `setgid` bit on a directory**:
   ```bash
   chmod g+s mydir
   ```

2. **Remove the `setgid` bit**:
   ```bash
   chmod g-s mydir
   ```

---

### **6. `chmod u+s` and `chmod u+x` (setuid and add execute)**

## 🔐 What is **setuid (Set User ID)**?

**setuid** is a special permission in Linux that allows a user to **execute a file with the privileges of the file owner**, instead of their own.

👉 In simple words:

> When a file has setuid, whoever runs it gets **owner’s permissions (usually root)** during execution.

---

## 🧠 Why do we need setuid?

Some tasks require higher privileges. Example:

* Changing passwords
* System-level operations

A normal user cannot do these directly, so setuid allows controlled access.

---

## 📌 Classic Example: `passwd` command

```bash
ls -l /usr/bin/passwd
```

Output:

```bash
-rwsr-xr-x 1 root root ...
```

👉 Notice the **`s` instead of `x`**:

* `rws` → setuid is enabled
* Owner = root

📍 This means:
When any user runs `passwd`, it runs with **root privileges**.

That’s how a normal user can change their password.

---

## ⚙️ How to set setuid

### ✅ Using symbolic method

```bash
chmod u+s filename
```

### ✅ Using numeric method

```bash
chmod 4755 filename
```

👉 Breakdown:

* `4` → setuid
* `755` → normal permissions

---

## 🧪 Practical Example

Great — that explains it. Without `chmod 4755`, setuid simply won’t activate. Let’s rebuild the **complete, clean case study** with a proper C example so you can reuse it for learning or teaching.

---

# 🔐 Complete Case Study: setuid with C Program (Working Demo)

## 🎯 Objective

Demonstrate how **setuid allows a normal user to execute a program with root privileges**.

---

# 🧱 Step 1: Create C program

```bash
vi test.c
```

Add this code:

```c
#include <stdio.h>
#include <unistd.h>

int main() {
    printf("Real UID: %d\n", getuid());
    printf("Effective UID: %d\n", geteuid());
    return 0;
}
```

---

# ⚙️ Step 2: Compile the program

```bash
gcc test.c -o test
```

---

# 🔍 Step 3: Check default permissions

```bash
ls -l test
```

Example:

```
-rwxr-xr-x 1 raman raman ...
```

👉 Right now, it runs as the normal user.

---

# 🧪 Step 4: Run as normal user

```bash
./test
```

Output:

```
Real UID: 1001
Effective UID: 1001
```

---

# 👑 Step 5: Change owner to root

```bash
sudo chown root:root test
```

---

# 🔐 Step 6: Apply setuid permission

```bash
sudo chmod 4755 test
```

---

# 🔍 Step 7: Verify permissions

```bash
ls -l test
```

Output:

```
-rwsr-xr-x 1 root root ...
```

👉 `s` = setuid enabled

---

# 🚀 Step 8: Run as normal user again

```bash
./test
```

Output:

```
Real UID: 1001
Effective UID: 0
```

---

# 🧠 Understanding the Output

| Term          | Meaning                          |
| ------------- | -------------------------------- |
| Real UID      | Actual logged-in user            |
| Effective UID | Privileges used during execution |

👉 `0` = root → setuid is working

---

# ⚠️ Important Notes

## ❌ setuid does NOT work on:

* Shell scripts (`.sh`)
* Filesystems with `nosuid`

---

---

# 🔐 Real-world Example

```bash
ls -l /usr/bin/passwd
```

Output:

```
-rwsr-xr-x 1 root root ...
```

👉 That’s how users can change passwords securely

---

# 🧠 Key Takeaways

* `chmod 4755` → enables setuid
* Program runs with **owner privileges**
* Works only with **compiled binaries**
* Must have:

  * correct ownership (root)
  * correct permissions
  * no `nosuid` restriction

---

### **Summary**

- The **sticky bit** (`+t`) restricts file deletion to the file's owner in shared directories.
- **Setuid** and **setgid** bits (`u+s` and `g+s`) can elevate the privileges of programs and directories, allowing users to run executables with the file owner's privileges or ensure files inherit the group of the directory.
- The **`s`** permission indicates special permissions like **setuid**, **setgid**, or **sticky bit**, and you can modify them using `chmod`.
