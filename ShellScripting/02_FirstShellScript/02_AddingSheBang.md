✅ **Adding the Shebang (`#!/bin/bash`)**

---

### 🧩 **What Is a Shebang?**

The **shebang** (pronounced *sha-bang*) is the very first line in a shell script that tells the system **which interpreter** should be used to run the script.

---

### 📘 **Syntax**

```bash
#!/path/to/interpreter
```

For Bash scripts, this is:

```bash
#!/bin/bash
```

---

### 🧠 **Explanation**

* `#!` → Special characters that tell the system this file should be executed using the program that follows.
* `/bin/bash` → The full path to the **Bash shell** interpreter.

You can check this path by running:

```bash
which bash
```

---

### 💡 **Why Is It Important?**

* Ensures the script always runs in the **intended shell**, even if a user’s default shell is different (e.g., `sh`, `zsh`, `dash`).
* Makes the script **portable** and **predictable** across systems.

---

### 🧾 **Example Script**

```bash
#!/bin/bash
echo "This script runs using the Bash interpreter."
```

---

### ⚙️ **Alternative Shebangs**

Depending on the shell you want to use:

| Shell            | Shebang Line         |
| ---------------- | -------------------- |
| Bash             | `#!/bin/bash`        |
| Sh (POSIX)       | `#!/bin/sh`          |
| Zsh              | `#!/bin/zsh`         |
| Ksh              | `#!/bin/ksh`         |
| Python (example) | `#!/usr/bin/python3` |

---

### ⚠️ **Common Mistake**

❌ Missing the shebang line can cause:

* The system to use the **default shell**, leading to unexpected errors.
* Scripts to behave differently on Ubuntu vs RHEL (Ubuntu often uses `dash` for `/bin/sh`).

---


