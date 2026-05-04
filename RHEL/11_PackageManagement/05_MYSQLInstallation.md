---

# 🧠 1. What is a Module in DNF?

In DNF, a **module** is:

> A way to provide **multiple versions of the same software** and let you choose which version you want.

---

## 📦 Example concept

For databases, you may have:

* MySQL 8.0
* MySQL 8.4
* MariaDB 10.5

👉 All are available, but only **one stream is active at a time**

---

## 🔍 Check modules

```bash id="a4z7qz"
dnf module list
```

---

## 📋 Example output

```text id="s4k5wq"
mysql        8.0   [d]   default
mysql        8.4         available
```

---

## 📌 Terms you must know

| Term    | Meaning                      |
| ------- | ---------------------------- |
| Module  | Group of packages            |
| Stream  | Version (8.0, 8.4)           |
| Profile | Package set (server, client) |

---

## ❓ Why we need modules

👉 Because:

* Different apps need different versions
* RHEL keeps system stable
* Avoids conflicts between versions

---

# ⚙️ 2. Why enable a module?

Before installing:

👉 You must **choose which version (stream)** you want

---

## Example:

```bash id="l3ny5j"
dnf module enable mysql:8.4
```

👉 Now system knows:

> “Use MySQL 8.4 packages”

---

# 🧹 Reset module (important sometimes)

```bash id="8q7g91"
dnf module reset mysql
```

---

# 🧰 3. Step-by-Step: Install MySQL on RHEL

---

## Step 1: Check available modules

```bash id="r4g7wy"
dnf module list mysql
```

---

## Step 2: Enable required version

```bash id="x7n8pz"
dnf module enable mysql:8.4 -y
```

---

## Step 3: Install MySQL Server

```bash id="j4j3tz"
dnf install mysql-server -y
```

👉 This installs:

* Server
* Client
* Dependencies

---

# 🗄️ About MySQL

* Popular database server
* Used in web apps, backend systems

---

## Step 4: Start MySQL service

```bash id="n1t94p"
systemctl start mysqld
```

---

## Step 5: Enable at boot

```bash id="2d6lkl"
systemctl enable mysqld
```

---

## Step 6: Check status

```bash id="dzw7rq"
systemctl status mysqld
```

---

## Step 7: Secure installation

```bash id="9r33lm"
mysql_secure_installation
```

👉 Set:

* Root password
* Remove anonymous users
* Disable remote root login

---

## Step 8: Login to MySQL

```bash id="9mwzru"
mysql -u root -p
```

---

# 🔍 4. Verify installation

```bash id="m2mj3r"
rpm -qa | grep mysql
```

---

# ⚠️ Important Notes

---

## ❗ MySQL vs MariaDB

* RHEL default often uses **MariaDB module**
* MySQL may require:

  * Module selection
  * Or external repo (Oracle)

---

## ❗ If module not enabled

```bash id="p1x6tf"
dnf install mysql-server
```

👉 May fail or install wrong version

---

# 🔁 Full flow

```text id="i8b6rb"
dnf module enable mysql:8.4
        ↓
dnf install mysql-server
        ↓
start mysqld
        ↓
secure installation
```

---

# 👍 Final understanding

👉 Module = version control system for packages
👉 Enable module = choose version
👉 Install = install packages from that stream

---

# 🔥 Simple analogy

* Module = “category of software”
* Stream = “version you select”
* DNF = “installer using your selection”

---
