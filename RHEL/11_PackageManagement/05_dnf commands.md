
# 🌐 1. What is a Remote Repository?

A **remote repository** is a server (on internet or network) that stores:

* RPM packages
* Metadata (package info, dependencies, versions)

👉 Tools like DNF / YUM connect to these repos to install/update software.

---

## 📂 Where it is configured in RHEL

Repository configs are stored in:

```bash
/etc/yum.repos.d/
```

Example file:

```bash
cat /etc/yum.repos.d/redhat.repo
```

---

## 🧾 Example repo configuration

```ini
[baseos]
name=RHEL BaseOS
baseurl=http://mirror.example.com/rhel8/BaseOS
enabled=1
gpgcheck=1
```

---

## 🔍 Check available repos

```bash
dnf repolist
```

---

# 📦 2. What is Metadata?

Metadata is **information about packages**, not the packages themselves.

It includes:

* Package names
* Versions
* Dependencies
* File lists

👉 Stored in:

```bash
/var/cache/dnf/
```

---

## 🔄 How to update metadata

### Refresh metadata only:

```bash
dnf makecache
```

### Clean and rebuild:

```bash
dnf clean all
dnf makecache
```

---

## 🧠 Important

* `dnf install` uses metadata to find packages
* `dnf update` refreshes metadata automatically

---

# 💻 3. What is a Local Repository?

A **local repository** is:

> A directory containing RPM files + metadata

👉 Used when:

* No internet
* Controlled environments
* Faster installs

---

# 🛠️ 4. Create a Local Repository (Step-by-step)

---

## Step 1: Create directory

```bash
sudo mkdir -p /myrepo
```

---

## Step 2: Download htop RPM

```bash
wget https://www.rpmfind.net/linux/epel/8/Everything/x86_64/Packages/h/htop-3.2.1-1.el8.x86_64.rpm
```

---

## Step 3: Copy RPM into repo

```bash
cp htop-*.rpm /myrepo/
```

---

## Step 4: Install createrepo tool

```bash
dnf install createrepo -y
```

---

## Step 5: Generate metadata

```bash
createrepo /myrepo
```

👉 This creates:

```
/myrepo/repodata/
```

---

# ⚙️ 5. Configure Local Repo

Create file:

```bash
sudo vi /etc/yum.repos.d/local.repo
```

Add:

```ini
[local-repo]
name=Local Repository
baseurl=file:///myrepo
enabled=1
gpgcheck=0
```

---

## 🔄 Refresh repo list

```bash
dnf clean all
dnf repolist
```

👉 You should see `local-repo`

---

# 📋 6. List Packages from Local Repo

```bash
dnf list available
```

Or specifically:

```bash
dnf list htop
```

---

# 📥 7. Install Package from Local Repo

```bash
dnf install htop
```

👉 DNF will:

* Read metadata
* Install from `/myrepo`

---

# 📦 8. Verify Installation

```bash
rpm -qa | grep htop
```

or

```bash
htop
```

---

# ❌ 9. Remove Package

```bash
dnf remove htop
```

👉 This removes package and updates RPM database

---

# 🧠 10. Important Concepts Summary

---

## 🔁 Flow of package installation

```text
dnf install
   ↓
Check repo (local/remote)
   ↓
Read metadata
   ↓
Resolve dependencies
   ↓
Download RPM (or use local)
   ↓
Install using RPM
```

---

## ⚖️ Tool comparison

| Tool    | Purpose                           |
| ------- | --------------------------------- |
| rpm     | Low-level install/query           |
| dnf/yum | High-level (repos + dependencies) |

---

## 📌 Key Points

* Remote repo → internet/server-based
* Local repo → directory + metadata
* Metadata → required for package discovery
* `createrepo` → creates metadata
* `dnf` → installs using repo
* `rpm -qa` → lists installed packages

---


