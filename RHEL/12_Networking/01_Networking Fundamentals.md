# 1. Introduction to Networking in Linux

## What is Networking?

Networking means connecting two or more devices so they can communicate and share data.

In Linux, networking allows systems to:

* Access the internet
* Communicate with other servers
* Transfer files
* Host websites
* Provide remote access
* Run distributed applications

Linux is heavily used in:

* Servers
* Cloud computing
* Data centers
* Cybersecurity
* DevOps
* Networking devices

---

# Why Networking is Important in Linux

Linux is designed with strong networking capabilities built into the kernel.

Almost every Linux server depends on networking:

* Web servers
* Database servers
* DNS servers
* SSH servers
* Mail servers
* Container platforms
* Kubernetes clusters

Without networking:

* No remote login
* No internet access
* No communication between systems
* No cloud connectivity

---

# Linux Networking Roles

A Linux machine can act as different networking devices.

---

## 1. Linux as a Client

A client requests services from another system.

Examples:

* Opening websites
* Downloading files
* Connecting to SSH servers

Common Linux client tools:

* `curl`
* `wget`
* `ssh`
* `ping`

Example:

```bash
curl https://example.com
```

---

## 2. Linux as a Server

A server provides services to clients.

Examples:

* Web server
* File server
* SSH server
* Database server

Popular Linux servers:

* Apache
* Nginx
* OpenSSH
* Samba

Example:

```bash
sudo systemctl start nginx
```

---

## 3. Linux as a Router

A router forwards packets between networks.

Linux can:

* Route traffic
* Share internet connections
* Perform NAT
* Control packet forwarding

Used in:

* Enterprise routers
* Firewalls
* Cloud gateways

Key concept:

```bash
net.ipv4.ip_forward=1
```

---

## 4. Linux as a Firewall

Linux can filter and secure network traffic.

Firewall capabilities:

* Block ports
* Allow trusted traffic
* NAT
* Packet filtering

Tools:

* `iptables`
* `nftables`
* `firewalld`
* `ufw`

Example:

```bash
sudo ufw allow 22
```

---

# Basic Networking Components in Linux

## 1. Network Interface Card (NIC)

Hardware/software interface used for communication.

Examples:

* Ethernet card
* Wireless adapter

Linux interface names:

```bash
eth0
ens33
wlan0
```

View interfaces:

```bash
ip link show
```

---

## 2. IP Address

Unique logical address assigned to a device.

Example:

```text
192.168.1.10
```

Check IP:

```bash
ip addr
```

---

## 3. MAC Address

Physical hardware address of NIC.

Example:

```text
00:1A:2B:3C:4D:5E
```

View MAC:

```bash
ip link
```

---

## 4. Hostname

Human-readable system name.

Check hostname:

```bash
hostname
```

---

## 5. Gateway

Device used to reach other networks.

View gateway:

```bash
ip route
```

---

## 6. DNS Server

Converts domain names into IP addresses.

Example:

```text
google.com → 142.250.x.x
```

Check DNS:

```bash
cat /etc/resolv.conf
```

---

# Linux Network Communication Flow

Example:

```text
Linux PC → Router → Internet → Web Server
```

Steps:

1. Application generates request
2. Linux networking stack processes packet
3. NIC sends packet
4. Router forwards packet
5. Destination server responds

---

# Important Linux Networking Concepts

## 1. Packet

Small unit of transmitted data.

---

## 2. Port

Logical communication endpoint.

Examples:

* 22 → SSH
* 80 → HTTP
* 443 → HTTPS

---

## 3. Protocol

Rules for communication.

Examples:

* TCP
* UDP
* HTTP
* SSH

---

## 4. Socket

Combination of:

```text
IP Address + Port
```

---

# Common Linux Networking Commands

| Command      | Purpose           |
| ------------ | ----------------- |
| `ip addr`    | Show IP addresses |
| `ip link`    | Show interfaces   |
| `ping`       | Test connectivity |
| `ss`         | View sockets      |
| `curl`       | Transfer data     |
| `wget`       | Download files    |
| `traceroute` | Trace packet path |
| `dig`        | DNS lookup        |

---

# Real-World Linux Networking Examples

## Example 1: Access Website

```bash
curl https://google.com
```

---

## Example 2: Check Connectivity

```bash
ping 8.8.8.8
```

---

## Example 3: View Network Interfaces

```bash
ip addr
```

---

## Example 4: Check Open Ports

```bash
ss -tulnp
```

---

# Networking Layers in Linux (Simplified)

| Layer       | Example     |
| ----------- | ----------- |
| Application | SSH, HTTP   |
| Transport   | TCP, UDP    |
| Internet    | IP          |
| Link        | Ethernet    |
| Physical    | Cable/Wi-Fi |

---

# Key Takeaways

* Linux has powerful built-in networking support
* Linux can act as:

  * Client
  * Server
  * Router
  * Firewall
* Networking is essential for Linux administration
* Most Linux services depend on networking
* Understanding Linux networking is foundational for:

  * DevOps
  * Cloud
  * Cybersecurity
  * System Administration
  * Networking Engineering
