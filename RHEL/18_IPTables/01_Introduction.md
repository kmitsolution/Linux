## Introduction to **iptables** in RHEL (Red Hat Enterprise Linux)

### 1. What is iptables?

**iptables** is a **command-line firewall utility** used in Linux systems (including **RHEL**) to **control incoming and outgoing network traffic** based on predefined rules.

It works with the **Linux kernel netfilter framework** to filter packets.

In simple terms:

> **iptables = tool used to manage firewall rules in Linux.**

It helps administrators:

* Allow or block network traffic
* Secure servers
* Control ports and protocols
* Prevent unauthorized access

---

### 2. Why iptables is used

Common purposes:

* Block unwanted IP addresses
* Allow specific services (SSH, HTTP, FTP)
* Protect servers from attacks
* Network packet filtering
* Port forwarding and NAT

Example:

Allow only SSH access to a server while blocking all other ports.

---

### 3. Basic Architecture of iptables

iptables works with **tables**, **chains**, and **rules**.

#### 1. Tables

Tables define **what type of processing** is done.

Main tables in RHEL:

| Table  | Purpose                                   |
| ------ | ----------------------------------------- |
| filter | Default table for packet filtering        |
| nat    | Network Address Translation               |
| mangle | Packet modification                       |
| raw    | Packet exemption from connection tracking |

---

#### 2. Chains

Chains contain **rules that packets must follow**.

Common chains:

| Chain   | Function                           |
| ------- | ---------------------------------- |
| INPUT   | Incoming packets to the system     |
| OUTPUT  | Packets leaving the system         |
| FORWARD | Packets passing through the system |

Example flow:

```
Internet → INPUT → Local system
Local system → OUTPUT → Internet
Router → FORWARD → Destination
```

---

#### 3. Rules

Rules determine **what happens to packets**.

Typical actions:

* **ACCEPT** → allow packet
* **DROP** → discard packet silently
* **REJECT** → block packet and notify sender
* **LOG** → log packet details

---

### 4. Basic iptables Command Syntax

```
iptables [options] chain rule-specification target
```

Example:

Allow SSH port:

```
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

Explanation:

| Option     | Meaning          |
| ---------- | ---------------- |
| -A         | Append rule      |
| INPUT      | Chain            |
| -p tcp     | Protocol         |
| --dport 22 | Destination port |
| -j ACCEPT  | Action           |

---

### 5. Viewing Firewall Rules

List rules:

```
iptables -L
```

Detailed view:

```
iptables -L -n -v
```

---

### 6. Example Rules

#### Allow SSH

```
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

#### Allow HTTP

```
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

#### Block an IP

```
iptables -A INPUT -s 192.168.1.10 -j DROP
```

---

### 7. Save iptables Rules (RHEL)

To save rules permanently:

```
service iptables save
```

or

```
iptables-save
```

---

### 8. iptables vs firewalld in RHEL

| Feature         | iptables       | firewalld         |
| --------------- | -------------- | ----------------- |
| Configuration   | Static         | Dynamic           |
| Complexity      | Manual rules   | Easier management |
| Default in RHEL | Older versions | RHEL 7+ default   |

Note: **RHEL 7+ uses `firewalld` by default**, but it still uses **iptables internally**.

---

### 9. Summary

iptables is a powerful Linux firewall tool used to:

* Filter network packets
* Allow or block traffic
* Secure servers
* Manage network access

Structure:

```
Tables → Chains → Rules → Actions
```

---

