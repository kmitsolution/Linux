# 4. TCP/IP Fundamentals (Linux Point of View)

TCP/IP is the core networking model used by Linux and the internet.

Linux networking is built around the TCP/IP protocol suite.

Without TCP/IP:

* No internet
* No SSH
* No web browsing
* No server communication

---

# What is TCP/IP?

TCP/IP stands for:

| Term | Full Form                     |
| ---- | ----------------------------- |
| TCP  | Transmission Control Protocol |
| IP   | Internet Protocol             |

---

# Role of TCP/IP

| Protocol | Responsibility                   |
| -------- | -------------------------------- |
| IP       | Delivers packets between systems |
| TCP      | Ensures reliable communication   |

---

# TCP/IP Model

Linux primarily uses the TCP/IP model instead of the full OSI model.

---

## TCP/IP Layers

| TCP/IP Layer   | Purpose                      | Linux Examples  |
| -------------- | ---------------------------- | --------------- |
| Application    | User services                | SSH, HTTP, DNS  |
| Transport      | End-to-end communication     | TCP, UDP        |
| Internet       | Routing and IP addressing    | IPv4, IPv6      |
| Network Access | Physical/local communication | Ethernet, Wi-Fi |

---

# TCP/IP Packet Flow in Linux

Example:

```text id="ehw18h"
Application
   ↓
TCP/UDP
   ↓
IP
   ↓
Ethernet
   ↓
NIC
   ↓
Network
```

---

# 4.1 Internet Protocol (IP)

IP is responsible for:

* Addressing
* Routing
* Packet delivery

---

# Characteristics of IP

* Connectionless
* Best effort delivery
* No guarantee of delivery
* No error recovery

IP only tries to deliver packets.

---

# IPv4

Most widely used protocol.

Example:

```text id="lm2qz8"
192.168.1.10
```

---

## Check IPv4 Address

```bash id="kll14u"
ip addr
```

---

# IPv6

Newer protocol designed to replace IPv4.

Example:

```text id="hjlwm9"
2001:db8::1
```

---

## Advantages of IPv6

* Larger address space
* Better routing
* Built-in security improvements

---

## Check IPv6

```bash id="m49duw"
ip -6 addr
```

---

# IP Packet Structure

An IP packet contains:

```text id="u6q4n5"
IP Header + Data
```

IP Header includes:

* Source IP
* Destination IP
* TTL
* Protocol type

---

# TTL (Time To Live)

Limits packet lifetime.

Each router decreases TTL by 1.

Prevents:

```text id="0djlwm"
Infinite routing loops
```

---

# Test TTL

```bash id="6u58dq"
ping 8.8.8.8
```

Example:

```text id="6b4d2d"
ttl=117
```

---

# Routing in Linux

Linux uses routing tables to determine:

```text id="8pnl0w"
Where packets should go
```

---

## View Routing Table

```bash id="7fjlwm"
ip route
```

Example:

```text id="k8l8e3"
default via 192.168.1.1 dev ens33
```

---

# 4.2 TCP (Transmission Control Protocol)

TCP provides:

* Reliable communication
* Ordered delivery
* Error checking

---

# TCP Features

| Feature             | Purpose              |
| ------------------- | -------------------- |
| Reliable            | Ensures delivery     |
| Ordered             | Maintains sequence   |
| Connection-oriented | Establishes session  |
| Error checking      | Detects corruption   |
| Retransmission      | Resends lost packets |

---

# TCP 3-Way Handshake

TCP creates connection before communication.

---

## Steps

### 1. SYN

Client sends:

```text id="yr39vw"
SYN
```

---

### 2. SYN-ACK

Server replies:

```text id="4iy1ba"
SYN + ACK
```

---

### 3. ACK

Client responds:

```text id="8q6b2s"
ACK
```

Connection established.

---

# TCP Connection Flow

```text id="v2z6gm"
Client → SYN
Server → SYN-ACK
Client → ACK
```

---

# TCP in Linux

Common TCP services:

* SSH
* HTTP
* HTTPS
* FTP

---

## View TCP Connections

```bash id="vjxjlwm"
ss -t
```

---

## View Listening TCP Ports

```bash id="jlwm8t"
ss -lt
```

---

# Common TCP Problems

| Problem            | Cause                 |
| ------------------ | --------------------- |
| Connection timeout | Firewall/routing      |
| Connection refused | Service down          |
| Slow connection    | Packet retransmission |
| Reset connection   | Server issue          |

---

# 4.3 UDP (User Datagram Protocol)

UDP is:

* Fast
* Lightweight
* Connectionless

---

# UDP Features

| Feature        | Description        |
| -------------- | ------------------ |
| No handshake   | Faster             |
| No reliability | Packets may drop   |
| No ordering    | Unordered delivery |
| Low overhead   | Efficient          |

---

# UDP Used In

| Service         | Reason             |
| --------------- | ------------------ |
| DNS             | Speed              |
| Video streaming | Real-time          |
| VoIP            | Low latency        |
| Gaming          | Fast communication |

---

# View UDP Connections

```bash id="qjlwm1"
ss -u
```

---

# TCP vs UDP

| TCP                 | UDP            |
| ------------------- | -------------- |
| Reliable            | Faster         |
| Connection-oriented | Connectionless |
| Ordered             | Unordered      |
| Error recovery      | No recovery    |
| Higher overhead     | Lightweight    |

---

# 4.4 Ports in TCP/IP

Ports identify applications/services.

---

# Port Format

```text id="3cjlwm"
IP:PORT
```

Example:

```text id="e4av6g"
192.168.1.10:22
```

---

# Common Ports

| Port | Service |
| ---- | ------- |
| 22   | SSH     |
| 80   | HTTP    |
| 443  | HTTPS   |
| 53   | DNS     |
| 25   | SMTP    |

---

# View Listening Ports

```bash id="jlwm2s"
ss -tulnp
```

---

# 4.5 Sockets in Linux

Socket =

```text id="jlwm1o"
IP Address + Port + Protocol
```

Linux applications communicate using sockets.

---

# Example Socket

```text id="jlwm7c"
192.168.1.10:80/TCP
```

---

# View Active Sockets

```bash id="jlwm5h"
ss
```

---

# 4.6 ICMP (Internet Control Message Protocol)

Used for:

* Diagnostics
* Error reporting

---

# Common ICMP Tools

## Ping

```bash id="jlwm7y"
ping google.com
```

---

## Traceroute

```bash id="jlwm0s"
traceroute google.com
```

---

# Common ICMP Problems

* Ping blocked by firewall
* Network unreachable
* TTL exceeded

---

# DNS in TCP/IP

DNS resolves:

```text id="jlwm9x"
Domain → IP
```

---

# DNS Query Example

```bash id="jlwm4v"
dig google.com
```

---

# Packet Encapsulation in TCP/IP

Example:

```text id="jlwm2g"
HTTP Data
   ↓
TCP Header
   ↓
IP Header
   ↓
Ethernet Frame
```

---

# Linux TCP/IP Configuration Files

| File               | Purpose                |
| ------------------ | ---------------------- |
| `/etc/hosts`       | Local hostname mapping |
| `/etc/resolv.conf` | DNS configuration      |
| `/etc/services`    | Port/service mappings  |

---

# Important Linux Networking Commands

| Command      | Purpose            |
| ------------ | ------------------ |
| `ip addr`    | Show IP addresses  |
| `ip route`   | Show routing table |
| `ss`         | View sockets       |
| `ping`       | Test connectivity  |
| `traceroute` | Trace route        |
| `dig`        | DNS lookup         |

---

# Real Linux TCP/IP Example

## Accessing a Website

```text id="jlwm6f"
Browser
 ↓
DNS resolves domain
 ↓
TCP handshake occurs
 ↓
HTTP request sent
 ↓
Server responds
 ↓
Browser displays page
```

---

# Key Takeaways

* Linux networking is built on TCP/IP
* IP handles addressing and routing
* TCP provides reliable communication
* UDP provides fast communication
* Ports identify services
* Sockets enable applications to communicate
* Linux provides powerful TCP/IP troubleshooting tools
* Understanding TCP/IP is essential for:

  * Linux administration
  * DevOps
  * Cloud computing
  * Security
  * Networking careers
