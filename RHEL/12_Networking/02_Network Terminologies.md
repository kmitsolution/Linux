# 2. Basic Networking Concepts (Linux Point of View)

These are the core concepts you must understand before working with Linux networking.

---

# 2.1 IP Address

An IP address is a logical address assigned to a device on a network.

It identifies:

* Source device
* Destination device

Linux uses IP addresses for communication between systems.

---

## Types of IP Addresses

### IPv4

32-bit address.

Example:

```text id="8h6ktx"
192.168.1.10
```

---

### IPv6

128-bit address.

Example:

```text id="pqyw5u"
2001:db8::1
```

---

## Private IP Ranges

Used inside local networks.

| Range          | Purpose         |
| -------------- | --------------- |
| 10.0.0.0/8     | Large networks  |
| 172.16.0.0/12  | Medium networks |
| 192.168.0.0/16 | Home/office     |

---

## Public IP

Accessible over the internet.

Assigned by:

* ISP
* Cloud provider

---

## Check IP Address in Linux

```bash id="dyf58d"
ip addr
```

Short form:

```bash id="44aw0s"
ip a
```

---

## Assign Temporary IP Address

```bash id="xxy90m"
sudo ip addr add 192.168.1.100/24 dev eth0
```

---

# 2.2 MAC Address

MAC = Media Access Control Address

A MAC address is the physical hardware address of a network interface card (NIC).

---

## Characteristics

* Unique per NIC
* Layer 2 address
* Used inside local networks

Example:

```text id="ygt6yf"
00:1A:2B:3C:4D:5E
```

---

## View MAC Address

```bash id="8x4x0d"
ip link
```

or

```bash id="pm8bd0"
ip addr
```

---

## Important Note

| IP Address | MAC Address   |
| ---------- | ------------- |
| Logical    | Physical      |
| Can change | Usually fixed |
| Layer 3    | Layer 2       |

---

# 2.3 Hostname

Hostname is the human-readable name of a Linux system.

Example:

```text id="cf6mtw"
server01
db-server
web-node
```

---

## View Hostname

```bash id="m0i59l"
hostname
```

---

## Change Hostname

Temporary:

```bash id="2itc0j"
sudo hostname newname
```

Permanent:

```bash id="7n3m2o"
sudo hostnamectl set-hostname newname
```

---

## Hostname Resolution

Linux checks:

```text id="u6hll3"
/etc/hosts
```

Example:

```text id="40a0n6"
127.0.0.1 localhost
192.168.1.10 server01
```

---

# 2.4 Gateway

Gateway is the device that connects your network to other networks.

Usually:

```text id="g2jdr6"
Router
```

---

## Role of Gateway

If Linux wants to communicate outside its subnet:

* Packet goes to gateway
* Gateway forwards packet

---

## View Gateway

```bash id="3e6h1k"
ip route
```

Example output:

```text id="g6zmpn"
default via 192.168.1.1 dev eth0
```

---

## Add Gateway

```bash id="5e5h8w"
sudo ip route add default via 192.168.1.1
```

---

# 2.5 Subnet Mask / CIDR

Subnet mask defines:

* Network portion
* Host portion

---

## CIDR Notation

Example:

```text id="5g31zc"
192.168.1.10/24
```

Meaning:

* `/24` → first 24 bits are network bits

Equivalent subnet mask:

```text id="j99o91"
255.255.255.0
```

---

## Common CIDR Values

| CIDR | Subnet Mask   |
| ---- | ------------- |
| /8   | 255.0.0.0     |
| /16  | 255.255.0.0   |
| /24  | 255.255.255.0 |

---

# 2.6 DNS (Domain Name System)

DNS converts domain names into IP addresses.

Example:

```text id="n0ymw5"
google.com → 142.x.x.x
```

Without DNS:

* We would use IP addresses directly

---

## Linux DNS Configuration

File:

```text id="fyj6yc"
/etc/resolv.conf
```

Example:

```text id="jqmbmk"
nameserver 8.8.8.8
```

---

## Test DNS

Using `dig`:

```bash id="ghd2jq"
dig google.com
```

Using `nslookup`:

```bash id="bq4i5r"
nslookup google.com
```

---

# 2.7 Ports

Ports identify services running on a system.

Think of:

```text id="5q7w4j"
IP Address = Building
Port = Room Number
```

---

## Common Ports

| Port | Service |
| ---- | ------- |
| 22   | SSH     |
| 80   | HTTP    |
| 443  | HTTPS   |
| 53   | DNS     |
| 25   | SMTP    |

---

## View Listening Ports

```bash id="zj7o25"
ss -tulnp
```

Older command:

```bash id="k8i9bp"
netstat -tulnp
```

---

# 2.8 Protocols

Protocols define communication rules.

---

## Common Networking Protocols

| Protocol | Purpose                |
| -------- | ---------------------- |
| TCP      | Reliable communication |
| UDP      | Fast communication     |
| HTTP     | Web traffic            |
| HTTPS    | Secure web traffic     |
| SSH      | Remote login           |
| DNS      | Name resolution        |
| ICMP     | Ping                   |

---

# TCP vs UDP

| TCP                 | UDP               |
| ------------------- | ----------------- |
| Reliable            | Faster            |
| Connection-oriented | Connectionless    |
| Error checking      | Minimal checking  |
| Used in SSH/HTTP    | Used in streaming |

---

# 2.9 Socket

Socket =

```text id="jlwm4j"
IP Address + Port
```

Example:

```text id="uv4i9w"
192.168.1.10:22
```

Linux uses sockets for:

* Network communication
* Client-server communication

---

# 2.10 Network Interface

Network interface is the connection point between Linux and the network.

Examples:

```text id="m3c0oi"
eth0
ens33
wlan0
lo
```

---

## Types

| Interface       | Purpose                |
| --------------- | ---------------------- |
| Ethernet        | Wired                  |
| Wireless        | Wi-Fi                  |
| Loopback (`lo`) | Internal communication |
| Virtual         | Containers/VMs         |

---

## View Interfaces

```bash id="z3ovm7"
ip link
```

---

# 2.11 Loopback Interface

Loopback interface:

```text id="xjmfze"
lo
```

Used for:

* Internal communication
* Testing networking locally

IP:

```text id="i8k75m"
127.0.0.1
```

Test:

```bash id="yv8ye7"
ping 127.0.0.1
```

---

# Packet Flow Example in Linux

```text id="mzt98d"
Application
   ↓
Socket
   ↓
TCP/UDP
   ↓
IP Layer
   ↓
Network Interface
   ↓
Router/Gateway
   ↓
Destination
```

---

# Important Linux Files

| File               | Purpose                |
| ------------------ | ---------------------- |
| `/etc/hosts`       | Local hostname mapping |
| `/etc/resolv.conf` | DNS servers            |
| `/etc/hostname`    | System hostname        |

---

# Key Takeaways

* IP address identifies systems
* MAC address identifies hardware
* Gateway connects networks
* DNS resolves names to IPs
* Ports identify services
* Protocols define communication rules
* Interfaces connect Linux to networks
* Linux networking relies heavily on:

  * IP
  * Routing
  * DNS
  * TCP/UDP
  * Interfaces
