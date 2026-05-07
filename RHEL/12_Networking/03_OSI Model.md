# 3. OSI Model (Practical Linux View)

The OSI Model (Open Systems Interconnection Model) is a conceptual framework used to understand how networking works.

It divides networking into layers.

---

# Why OSI Model is Important in Linux

In Linux administration, the OSI model helps you:

* Troubleshoot network issues
* Understand packet flow
* Diagnose connectivity problems
* Understand protocols and services

Example:

* Ping not working?
* SSH connection failing?
* DNS issue?
* Firewall blocking traffic?

OSI helps identify where the problem exists.

---

# OSI Layers Overview

| Layer Number | Layer Name   | Linux Examples |
| ------------ | ------------ | -------------- |
| 7            | Application  | SSH, HTTP, DNS |
| 6            | Presentation | SSL/TLS        |
| 5            | Session      | RPC, NetBIOS   |
| 4            | Transport    | TCP, UDP       |
| 3            | Network      | IP, Routing    |
| 2            | Data Link    | Ethernet, MAC  |
| 1            | Physical     | Cable, Wi-Fi   |

---

# Simplified Linux Networking Stack

In real Linux networking, we commonly focus on:

| Simplified Layer | Examples  |
| ---------------- | --------- |
| Application      | SSH, HTTP |
| Transport        | TCP/UDP   |
| Internet         | IP        |
| Link             | Ethernet  |
| Physical         | NIC/Cable |

---

# Layer 1 — Physical Layer

## Purpose

Responsible for physical transmission of data.

---

## Examples

* Ethernet cable
* Fiber cable
* Wi-Fi signals
* Switch ports
* NIC hardware

---

## Linux Perspective

Linux interacts with:

* NIC drivers
* Link detection
* Hardware communication

---

## Common Problems

* Cable unplugged
* Wi-Fi disabled
* NIC failure
* Link down

---

## Check Interface Status

```bash id="m0hsk4"
ip link
```

Example:

```text id="ccqqs4"
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP>
```

Important:

* `UP` → interface enabled
* `LOWER_UP` → physical connection exists

---

## Bring Interface Up

```bash id="8yrz7z"
sudo ip link set ens33 up
```

---

# Layer 2 — Data Link Layer

## Purpose

Handles communication inside local network.

Uses:

* MAC addresses
* Ethernet frames

---

## Protocols

* Ethernet
* ARP
* VLAN

---

## Linux Perspective

Linux uses:

* MAC addresses
* ARP table
* Ethernet interfaces

---

## View MAC Address

```bash id="m8xjlwm"
ip link
```

Example:

```text id="7n1xgf"
link/ether 00:0c:29:ab:cd:ef
```

---

## ARP Table

ARP maps:

```text id="uhv3di"
IP Address → MAC Address
```

View ARP table:

```bash id="0s9fyi"
ip neigh
```

---

## Common Problems

* Duplicate MAC
* ARP failure
* VLAN mismatch
* Switch issues

---

# Layer 3 — Network Layer

## Purpose

Responsible for routing packets between networks.

Uses:

* IP addresses
* Routing

---

## Protocols

* IPv4
* IPv6
* ICMP

---

## Linux Perspective

Linux handles:

* IP addressing
* Routing tables
* Gateways
* Packet forwarding

---

## View IP Address

```bash id="i9wlci"
ip addr
```

---

## View Routing Table

```bash id="8v4wbl"
ip route
```

Example:

```text id="hkj2e5"
default via 192.168.1.1 dev ens33
```

---

## Test Connectivity

```bash id="t9ocd8"
ping 8.8.8.8
```

---

## Common Problems

* Wrong IP
* Incorrect subnet
* Missing gateway
* Routing issues

---

# Layer 4 — Transport Layer

## Purpose

Responsible for end-to-end communication.

Uses:

* TCP
* UDP
* Ports

---

# TCP

Reliable communication.

Features:

* Connection-oriented
* Error checking
* Retransmission

Used in:

* SSH
* HTTP/HTTPS
* FTP

---

# UDP

Fast communication.

Features:

* Connectionless
* Minimal overhead

Used in:

* Streaming
* DNS
* VoIP

---

# Linux Perspective

Linux manages:

* Sockets
* Connections
* Listening ports

---

## View Listening Ports

```bash id="k14vdv"
ss -tulnp
```

---

## Example Output

```text id="tjlwm3"
tcp LISTEN 0 128 0.0.0.0:22
```

Meaning:

* TCP
* Listening on port 22
* SSH service active

---

## Common Problems

* Port blocked
* Firewall issue
* Service not running
* TCP connection timeout

---

# Layer 5 — Session Layer

## Purpose

Manages sessions between applications.

---

## Examples

* SSH session
* Remote desktop session
* Authentication sessions

---

## Linux Perspective

Usually handled by:

* Applications
* Libraries

Linux admins rarely configure this layer directly.

---

# Layer 6 — Presentation Layer

## Purpose

Handles:

* Encryption
* Compression
* Data formatting

---

## Examples

* SSL/TLS
* UTF-8 encoding

---

## Linux Perspective

Common in:

* HTTPS
* SSH encryption
* Secure APIs

---

## Check SSL Connection

```bash id="hthfj4"
openssl s_client -connect google.com:443
```

---

## Common Problems

* SSL certificate expired
* TLS mismatch
* Encryption failure

---

# Layer 7 — Application Layer

## Purpose

Provides services directly to users/applications.

---

## Protocols

* HTTP/HTTPS
* SSH
* DNS
* FTP
* SMTP

---

## Linux Perspective

Most Linux network services operate here.

Examples:

* Apache/Nginx
* OpenSSH
* Bind DNS
* Postfix

---

## Test HTTP

```bash id="jlwmw2"
curl http://example.com
```

---

## Test DNS

```bash id="jlwmz9"
dig google.com
```

---

## Common Problems

* Service down
* DNS failure
* Authentication issue
* Application misconfiguration

---

# Packet Flow Through OSI Layers

Example:

```text id="cnmd1m"
Browser Request
    ↓
HTTP
    ↓
TCP
    ↓
IP
    ↓
Ethernet
    ↓
NIC
    ↓
Network Cable
```

At destination:

```text id="7x9hcu"
Cable
 ↓
NIC
 ↓
Ethernet
 ↓
IP
 ↓
TCP
 ↓
HTTP
 ↓
Application
```

---

# Encapsulation in Linux

Each layer adds headers.

Example:

```text id="t4zyxw"
HTTP Data
   ↓
TCP Header
   ↓
IP Header
   ↓
Ethernet Header
```

This process is called:

```text id="h9muh3"
Encapsulation
```

Removing headers:

```text id="7wkp3z"
Decapsulation
```

---

# Practical Linux Troubleshooting Using OSI

| Problem         | OSI Layer |
| --------------- | --------- |
| Cable unplugged | Layer 1   |
| ARP failure     | Layer 2   |
| Wrong IP        | Layer 3   |
| Port blocked    | Layer 4   |
| SSL error       | Layer 6   |
| Web server down | Layer 7   |

---

# Important Linux Commands by Layer

| Layer   | Commands                      |
| ------- | ----------------------------- |
| Layer 1 | `ip link`                     |
| Layer 2 | `ip neigh`                    |
| Layer 3 | `ip addr`, `ip route`, `ping` |
| Layer 4 | `ss`, `netstat`               |
| Layer 7 | `curl`, `dig`, `ssh`          |

---

# Key Takeaways

* OSI model helps understand Linux networking
* Linux networking mainly focuses on:

  * Ethernet
  * IP
  * TCP/UDP
  * Applications
* Most troubleshooting follows OSI layers
* Linux tools map directly to networking layers
* Understanding packet flow is critical for:

  * System administration
  * DevOps
  * Cloud
  * Security
  * Networking jobs
