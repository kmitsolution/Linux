Sure! Let's walk through how to create two types of users in RHEL:

1. **A normal user without any sudo privileges**.
2. **A user with sudo privileges** that can then create other users.

We'll first show that the normal user can't add a new user, and then after giving the user sudo privileges, they will be able to add users.

### **1. Create a Normal User Without Sudo Privileges**

A normal user without any special permissions is a user that can log in and use the system, but can't run administrative tasks like creating other users. We'll create a user called `alice`.

#### Step 1: Create a User `alice`
Use the `useradd` command to create the user `alice`, and set up a home directory and default shell.

```bash
sudo useradd -m -s /bin/bash alice
```

- `-m`: Creates the home directory `/home/alice`.
- `-s /bin/bash`: Specifies the default shell for the user as Bash.

#### Step 2: Set a Password for `alice`
Set a password for the user `alice` using the `passwd` command.

```bash
sudo passwd alice
```

You will be prompted to enter and confirm the password for `alice`.

#### Step 3: Verify Normal User `alice`
Switch to the user `alice` and verify that `alice` cannot create other users because they don't have sudo privileges.

```bash
su - alice
```

Now, try running a command that requires administrative privileges, such as adding a new user:

```bash
sudo useradd -m bob
```

**Expected Output**:
```
[sudo] password for alice: 
alice is not in the sudoers file.  This incident will be reported.
```

As you can see, the user `alice` does not have the permissions to use `sudo` to add a user because they are not in the `sudoers` file and are not part of the `wheel` group.

---

### **2. Create a User with Sudo Privileges**

Now, let's create a user called `bob` and give them sudo privileges. This user will be able to run administrative tasks like adding other users.

#### Step 1: Create the User `bob` Without Sudo Privileges (Initially)
First, create the user `bob` as a normal user, just like we did for `alice`.

```bash
sudo useradd -m -s /bin/bash bob
```

#### Step 2: Set a Password for `bob`
Set a password for `bob`.

```bash
sudo passwd bob
```

#### Step 3: Add `bob` to the `wheel` Group (Grant Sudo Privileges)
To give `bob` the ability to run `sudo` commands, add them to the `wheel` group, which has sudo privileges by default.

```bash
sudo usermod -aG wheel bob
```

- `-aG wheel`: Appends `bob` to the `wheel` group, which is configured to allow sudo access in the `/etc/sudoers` file.

---

### **3. Verify That `bob` Can Add a User After Gaining Sudo Privileges**

Now, let's switch to the user `bob` and verify that they can create new users by running commands with `sudo`.

#### Step 1: Switch to the User `bob`

```bash
su - bob
```

#### Step 2: Try Adding a User (Before and After Sudo Privileges)

**Before adding to the `wheel` group**:
- `bob` doesn't have sudo access yet. So, trying to create a new user should fail.

Try adding a new user `charlie` (without sudo privileges):

```bash
sudo useradd -m charlie
```

**Expected Output**:
```
[sudo] password for bob:
bob is not in the sudoers file.  This incident will be reported.
```

Since `bob` is not yet in the `wheel` group, this will fail.

---

#### Step 3: Add `bob` to the `wheel` Group (if not done earlier)
If you missed adding `bob` to the `wheel` group, you can do that now (make sure you are logged in as a user with sufficient privileges, like `root`):

```bash
sudo usermod -aG wheel bob
```

---

#### Step 4: Verify `bob` Can Now Add a New User

Now that `bob` has been added to the `wheel` group, they can use `sudo` to perform administrative tasks like adding users.

Switch back to the user `bob`:

```bash
su - bob
```

Try adding a new user `charlie` again:

```bash
sudo useradd -m charlie
```

**Expected Output**:
```
[sudo] password for bob:
```

After entering the password for `bob`, the user `charlie` will be created successfully. To confirm this, you can check if the user `charlie` exists:

```bash
id charlie
```

**Expected Output**:
```
uid=1003(charlie) gid=1003(charlie) groups=1003(charlie)
```

This confirms that `bob` was able to create the user `charlie` after being granted sudo privileges.

---

### **Complete Example Commands and Sequence**:

1. **Create a normal user `alice`** (without sudo privileges):

   ```bash
   sudo useradd -m -s /bin/bash alice
   sudo passwd alice
   ```

2. **Switch to `alice` and verify that they can't add a new user**:

   ```bash
   su - alice
   sudo useradd -m bob  # This will fail because 'alice' is not in the sudoers file
   ```

3. **Create a user `bob`** without sudo privileges:

   ```bash
   sudo useradd -m -s /bin/bash bob
   sudo passwd bob
   ```

4. **Add `bob` to the `wheel` group (to grant sudo privileges)**:

   ```bash
   sudo usermod -aG wheel bob
   ```

5. **Switch to `bob` and verify they can now add a new user**:

   ```bash
   su - bob
   sudo useradd -m charlie  # This will succeed because 'bob' now has sudo privileges
   ```

---

### **Conclusion**:

- **Normal User (`alice`)**: Before adding a user to the `wheel` group, the user can’t run commands with `sudo`. In our case, `alice` can't create new users because they don’t have admin privileges.
- **User with Sudo (`bob`)**: After adding `bob` to the `wheel` group, `bob` can run commands with `sudo`, including administrative tasks like creating new users.
