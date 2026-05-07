# 5. Network Interfaces in Linux

A network interface is the connection point between Linux and the network.

Linux uses interfaces to:

* Send packets
* Receive packets
* Communicate with other systems
* Access the internet

Every network communication in Linux happens through an interface.

---

# What is a Network Interface?

A network interface can be:

* Physical
* Virtual
* Software-based

Examples:

```text id="k3mx8w"
eth0
ens33
wlan0
lo
docker0
```

---

# Types of Network Interfaces in Linux

| Interface Type | Purpose                  |
| -------------- | ------------------------ |
| Ethernet       | Wired networking         |
| Wireless       | Wi-Fi                    |
| Loopback       | Internal communication   |
| Virtual        | Containers/VMs           |
| Bridge         | Connect virtual networks |
| Bonded         | Combine interfaces       |

---

# 5.1 Ethernet Interfaces

Used for wired network communication.

---

# Traditional Names

Older Linux systems used:

```text id="jlwm7a"
eth0
eth1
```

---

# Modern Predictable Names

Modern Linux uses:

```text id="jlwm3n"
ens33
enp0s3
eno1
```

Purpose:

```text id="jlwm0w"
Consistent interface naming
```

---

# View Interfaces

```bash id="jlwm5m"
ip link show
```

or

```bash id="jlwm2y"
ip addr
```

---

# Example Output

```text id="jlwm8u"
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP>
```

---

# Interface States

| State      | Meaning                  |
| ---------- | ------------------------ |
| UP         | Interface enabled        |
| DOWN       | Interface disabled       |
| LOWER_UP   | Physical cable connected |
| NO-CARRIER | No physical connection   |

---

# Bring Interface Up

```bash id="jlwm1q"
sudo ip link set ens33 up
```

---

# Bring Interface Down

```bash id="jlwm6e"
sudo ip link set ens33 down
```

---

# 5.2 Wireless Interfaces

Wireless interfaces are used for Wi-Fi communication.

Common names:

```text id="jlwm4j"
wlan0
wlp2s0
```

---

# View Wireless Interfaces

```bash id="jlwm9r"
iwconfig
```

or

```bash id="jlwm7p"
ip link
```

---

# Scan Wi-Fi Networks

```bash id="jlwm2p"
nmcli dev wifi list
```

---

# Connect to Wi-Fi

```bash id="jlwm5v"
nmcli dev wifi connect WIFI_NAME password PASSWORD
```

---

# Wireless Problems

| Problem               | Cause                 |
| --------------------- | --------------------- |
| Wi-Fi not detected    | Missing driver        |
| Weak signal           | Distance/interference |
| Authentication failed | Wrong password        |

---

# 5.3 Loopback Interface (`lo`)

Special internal interface.

Name:

```text id="jlwm1z"
lo
```

IP:

```text id="jlwm8k"
127.0.0.1
```

---

# Purpose of Loopback

Used for:

* Internal communication
* Local testing
* Service communication on same machine

---

# Test Loopback

```bash id="jlwm3s"
ping 127.0.0.1
```

---

# View Loopback Interface

```bash id="jlwm6g"
ip addr show lo
```

---

# Why Loopback is Important

Many Linux services depend on:

```text id="jlwm0m"
localhost
```

If loopback fails:

* Services may not start
* Internal communication breaks

---

# 5.4 Virtual Interfaces

Software-created interfaces.

Used in:

* Virtual machines
* Containers
* Bridges
* VPNs

---

# Examples

| Interface | Purpose              |
| --------- | -------------------- |
| docker0   | Docker bridge        |
| virbr0    | KVM virtualization   |
| tun0      | VPN tunnel           |
| vethXXX   | Container networking |

---

# View Virtual Interfaces

```bash id="jlwm5l"
ip link
```

---

# Docker Example

```text id="jlwm7x"
docker0
```

Allows containers to communicate.

---

# VPN Example

```text id="jlwm2n"
tun0
```

Created when VPN connects.

---

# 5.5 Interface Naming in Linux

Modern Linux uses predictable naming.

---

# Common Naming Patterns

| Prefix | Meaning        |
| ------ | -------------- |
| en     | Ethernet       |
| wl     | Wireless       |
| lo     | Loopback       |
| virbr  | Virtual bridge |
| docker | Docker bridge  |

---

# Examples

| Interface | Meaning       |
| --------- | ------------- |
| ens33     | Ethernet      |
| wlp2s0    | Wireless      |
| lo        | Loopback      |
| docker0   | Docker bridge |

---

# 5.6 IP Address Assignment

Interfaces need IP addresses.

---

# Dynamic IP (DHCP)

Automatically assigned.

Common in:

* Home networks
* Cloud systems

---

# Renew DHCP Lease

```bash id="jlwm8z"
sudo dhclient ens33
```

---

# Static IP

Manually configured.

Used in:

* Servers
* Databases
* Infrastructure systems

---

# Assign Temporary Static IP

```bash id="jlwm4u"
sudo ip addr add 192.168.1.100/24 dev ens33
```

---

# Remove IP Address

```bash id="jlwm1d"
sudo ip addr del 192.168.1.100/24 dev ens33
```

---

# 5.7 Interface Statistics

Linux tracks:

* Packets
* Errors
* Drops
* Traffic

---

# View Statistics

```bash id="jlwm0c"
ip -s link
```

---

# Example Metrics

| Metric  | Meaning             |
| ------- | ------------------- |
| RX      | Received packets    |
| TX      | Transmitted packets |
| Errors  | Failed packets      |
| Dropped | Discarded packets   |

---

# 5.8 MTU (Maximum Transmission Unit)

Defines maximum packet size.

Default Ethernet MTU:

```text id="jlwm4k"
1500 bytes
```

---

# View MTU

```bash id="jlwm6y"
ip link
```

---

# Change MTU

```bash id="jlwm9e"
sudo ip link set dev ens33 mtu 9000
```

---

# Jumbo Frames

MTU larger than 1500.

Used in:

* High-speed networks
* Storage networks

---

# 5.9 MAC Address Management

Each interface has a MAC address.

---

# View MAC Address

```bash id="jlwm8c"
ip link
```

---

# Change MAC Address Temporarily

```bash id="jlwm7k"
sudo ip link set dev ens33 address 00:11:22:33:44:55
```

---

# 5.10 Interface Configuration Files

Different Linux distributions use different configuration systems.

---

# Common Locations

| Distribution | File/System               |
| ------------ | ------------------------- |
| Ubuntu       | Netplan                   |
| RHEL/CentOS  | ifcfg files               |
| Debian       | `/etc/network/interfaces` |

---

# Ubuntu Netplan Example

```yaml id="jlwm1v"
network:
  version: 2
  ethernets:
    ens33:
      dhcp4: true
```

---

# Apply Netplan

```bash id="jlwm5b"
sudo netplan apply
```

---

# 5.11 Important Commands for Interfaces

| Command      | Purpose                |
| ------------ | ---------------------- |
| `ip link`    | Show interfaces        |
| `ip addr`    | Show IP addresses      |
| `ip -s link` | Show statistics        |
| `ethtool`    | Interface details      |
| `nmcli`      | NetworkManager control |
| `iwconfig`   | Wireless configuration |

---

# Useful Troubleshooting

---

# Check Link Status

```bash id="jlwm0b"
ethtool ens33
```

---

# Check Driver Information

```bash id="jlwm9u"
ethtool -i ens33
```

---

# Restart Interface

```bash id="jlwm6q"
sudo systemctl restart NetworkManager
```

---

# Common Network Interface Problems

| Problem        | Cause                 |
| -------------- | --------------------- |
| Interface DOWN | Disabled interface    |
| NO-CARRIER     | Cable unplugged       |
| No IP address  | DHCP failure          |
| Slow network   | Duplex mismatch       |
| Packet drops   | MTU or hardware issue |

---

# Real Linux Networking Flow

```text id="jlwm7j"
Application
   ↓
Socket
   ↓
TCP/UDP
   ↓
IP
   ↓
Network Interface
   ↓
Physical Network
```

Interface is the final Linux component before packets leave the machine.

---

# Key Takeaways

* Network interfaces connect Linux to networks
* Interfaces may be:

  * Physical
  * Wireless
  * Virtual
  * Loopback
* Linux uses interfaces for all communication
* `ip` command is the primary networking tool
* Understanding interfaces is critical for:

  * Server administration
  * Troubleshooting
  * Cloud networking
  * Containers
  * DevOps
