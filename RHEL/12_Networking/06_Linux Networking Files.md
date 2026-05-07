# 6. Linux Networking Files

Linux networking behavior is controlled by several important configuration files.

These files manage:

* Hostnames
* DNS
* IP resolution
* Network interfaces
* Service mappings

Understanding these files is essential for Linux networking and troubleshooting.

---

# Important Linux Networking Files

| File                 | Purpose                  |
| -------------------- | ------------------------ |
| `/etc/hosts`         | Local hostname mapping   |
| `/etc/resolv.conf`   | DNS configuration        |
| `/etc/hostname`      | System hostname          |
| `/etc/nsswitch.conf` | Name resolution order    |
| `/etc/services`      | Port-to-service mappings |
| `/etc/hosts.allow`   | TCP wrapper allow rules  |
| `/etc/hosts.deny`    | TCP wrapper deny rules   |

---

# 6.1 `/etc/hosts`

Used for:

```text id="jlwm9d"
Local hostname resolution
```

Maps:

```text id="jlwm1r"
IP Address → Hostname
```

---

# Why `/etc/hosts` is Important

Linux checks this file before DNS (depending on configuration).

Useful for:

* Local testing
* Small networks
* Temporary mappings
* Blocking domains

---

# View File

```bash id="jlwm5p"
cat /etc/hosts
```

---

# Example

```text id="jlwm0t"
127.0.0.1 localhost
192.168.1.10 webserver
192.168.1.20 database
```

---

# Meaning

| IP           | Hostname  |
| ------------ | --------- |
| 127.0.0.1    | localhost |
| 192.168.1.10 | webserver |

---

# Test Host Resolution

```bash id="jlwm8d"
ping webserver
```

Linux resolves:

```text id="jlwm3g"
webserver → 192.168.1.10
```

---

# Edit `/etc/hosts`

```bash id="jlwm7w"
sudo nano /etc/hosts
```

---

# Common Uses

| Use Case         | Example                    |
| ---------------- | -------------------------- |
| Testing          | Map domain to local server |
| Blocking         | Redirect unwanted domains  |
| Internal servers | Local hostname mapping     |

---

# Common Problems

| Problem                   | Cause                |
| ------------------------- | -------------------- |
| Wrong hostname resolution | Incorrect entry      |
| Duplicate entries         | Conflicting mappings |
| Unable to resolve host    | Missing entry        |

---

# 6.2 `/etc/resolv.conf`

Controls:

```text id="jlwm2x"
DNS configuration
```

Contains:

* DNS servers
* Search domains

---

# View DNS Configuration

```bash id="jlwm4n"
cat /etc/resolv.conf
```

---

# Example

```text id="jlwm6d"
nameserver 8.8.8.8
nameserver 1.1.1.1
```

---

# Meaning

Linux uses:

* 8.8.8.8
* 1.1.1.1

for DNS lookups.

---

# DNS Query Flow

```text id="jlwm9g"
Application
   ↓
Resolver
   ↓
/etc/resolv.conf
   ↓
DNS Server
```

---

# Test DNS

```bash id="jlwm5y"
dig google.com
```

or

```bash id="jlwm1x"
nslookup google.com
```

---

# Important Note

Modern Linux systems may auto-generate:

```text id="jlwm8n"
/etc/resolv.conf
```

Managed by:

* NetworkManager
* systemd-resolved
* DHCP client

---

# Common Problems

| Problem             | Cause                 |
| ------------------- | --------------------- |
| DNS failure         | Wrong nameserver      |
| Slow resolution     | Unreachable DNS       |
| Temporary overwrite | DHCP replacing config |

---

# 6.3 `/etc/hostname`

Stores:

```text id="jlwm4d"
System hostname
```

---

# View Hostname File

```bash id="jlwm6k"
cat /etc/hostname
```

Example:

```text id="jlwm0p"
server01
```

---

# View Current Hostname

```bash id="jlwm2k"
hostname
```

---

# Change Hostname Permanently

```bash id="jlwm9l"
sudo hostnamectl set-hostname webserver
```

---

# Verify

```bash id="jlwm1j"
hostnamectl
```

---

# Hostname Resolution Flow

Linux hostname often maps through:

```text id="jlwm5t"
/etc/hosts
```

---

# 6.4 `/etc/nsswitch.conf`

Controls:

```text id="jlwm3w"
Name Service Switch order
```

Defines:

* Where Linux looks for information
* Resolution priority

---

# View File

```bash id="jlwm7s"
cat /etc/nsswitch.conf
```

---

# Important Line

```text id="jlwm8w"
hosts: files dns
```

Meaning:

1. Check `/etc/hosts`
2. Then check DNS

---

# Possible Sources

| Source | Meaning                     |
| ------ | --------------------------- |
| files  | Local files                 |
| dns    | DNS server                  |
| nis    | Network Information Service |
| ldap   | LDAP directory              |

---

# Why This File Matters

Controls hostname resolution behavior.

---

# Example Resolution Process

```text id="jlwm6l"
ping google.com
    ↓
Check /etc/hosts
    ↓
If not found:
Query DNS
```

---

# Common Problems

| Problem              | Cause                |
| -------------------- | -------------------- |
| DNS ignored          | Wrong order          |
| Slow hostname lookup | Misconfigured source |

---

# 6.5 `/etc/services`

Maps:

```text id="jlwm0k"
Port Number ↔ Service Name
```

---

# View File

```bash id="jlwm2r"
cat /etc/services
```

---

# Example

```text id="jlwm4z"
ssh     22/tcp
http    80/tcp
https   443/tcp
```

---

# Purpose

Allows Linux tools to display:

```text id="jlwm9m"
ssh
```

instead of:

```text id="jlwm1n"
22
```

---

# Search for Port

```bash id="jlwm7n"
grep 22/tcp /etc/services
```

---

# Important Note

This file:

* Does NOT open ports
* Does NOT start services

Only maps names to ports.

---

# 6.6 `/etc/hosts.allow` and `/etc/hosts.deny`

Used by:

```text id="jlwm5n"
TCP Wrappers
```

Provides basic access control.

---

# Example Allow Rule

```text id="jlwm3x"
sshd: 192.168.1.
```

Allows SSH from subnet.

---

# Example Deny Rule

```text id="jlwm8x"
ALL: ALL
```

Blocks everything.

---

# Important Note

Modern Linux systems mainly use:

* Firewall rules
* SELinux
* AppArmor

instead of TCP wrappers.

---

# Networking File Relationships

```text id="jlwm6r"
/etc/hostname
        ↓
Hostname

/etc/hosts
        ↓
Local hostname mapping

/etc/resolv.conf
        ↓
DNS servers

/etc/nsswitch.conf
        ↓
Resolution order
```

---

# Real Linux Name Resolution Example

Suppose:

```bash id="jlwm1m"
ping webserver
```

Linux process:

```text id="jlwm4w"
1. Check /etc/nsswitch.conf
2. Check /etc/hosts
3. If not found:
4. Query DNS using /etc/resolv.conf
```

---

# Important Troubleshooting Commands

| Command                | Purpose             |
| ---------------------- | ------------------- |
| `cat /etc/hosts`       | View local mappings |
| `cat /etc/resolv.conf` | View DNS servers    |
| `hostnamectl`          | Show hostname       |
| `dig`                  | DNS testing         |
| `getent hosts`         | Test resolution     |

---

# Test Resolution

```bash id="jlwm0v"
getent hosts google.com
```

---

# Common Linux Networking Problems

| Issue               | Likely File          |
| ------------------- | -------------------- |
| DNS not working     | `/etc/resolv.conf`   |
| Wrong hostname      | `/etc/hostname`      |
| Wrong local mapping | `/etc/hosts`         |
| Slow resolution     | `/etc/nsswitch.conf` |

---

# Key Takeaways

* Linux networking relies heavily on configuration files
* `/etc/hosts` provides local hostname mapping
* `/etc/resolv.conf` defines DNS servers
* `/etc/hostname` stores system hostname
* `/etc/nsswitch.conf` controls lookup order
* Understanding these files is critical for:

  * Troubleshooting
  * Server administration
  * DevOps
  * Cloud environments
  * Network debugging
