# 7. Essential Linux Networking Commands

Linux provides powerful networking commands for:

* Configuration
* Troubleshooting
* Monitoring
* Connectivity testing

These commands are essential for:

* System administrators
* DevOps engineers
* Network engineers
* Security professionals

---

# Most Important Networking Commands

| Command      | Purpose                |
| ------------ | ---------------------- |
| `ip`         | Network configuration  |
| `ping`       | Connectivity testing   |
| `ss`         | Socket statistics      |
| `netstat`    | Network information    |
| `traceroute` | Trace network path     |
| `dig`        | DNS lookup             |
| `nslookup`   | DNS queries            |
| `curl`       | Transfer data          |
| `wget`       | Download files         |
| `hostname`   | Show system name       |
| `ethtool`    | Interface details      |
| `nmcli`      | NetworkManager control |

---

# 7.1 `ip` Command

Most important modern Linux networking command.

Replaces:

* `ifconfig`
* `route`
* `arp`

---

# Show Interfaces

```bash id="jlwm4p"
ip link
```

---

# Show IP Addresses

```bash id="jlwm1b"
ip addr
```

Short form:

```bash id="jlwm9q"
ip a
```

---

# Show Routing Table

```bash id="jlwm7v"
ip route
```

---

# Bring Interface Up

```bash id="jlwm3d"
sudo ip link set ens33 up
```

---

# Bring Interface Down

```bash id="jlwm6v"
sudo ip link set ens33 down
```

---

# Add Temporary IP

```bash id="jlwm0n"
sudo ip addr add 192.168.1.100/24 dev ens33
```

---

# Remove IP

```bash id="jlwm2h"
sudo ip addr del 192.168.1.100/24 dev ens33
```

---

# Show ARP/Neighbor Table

```bash id="jlwm8h"
ip neigh
```

---

# Why `ip` is Important

Used for:

* IP management
* Routing
* Interface control
* Network troubleshooting

---

# 7.2 `ping`

Tests connectivity using ICMP.

---

# Basic Ping

```bash id="jlwm5a"
ping google.com
```

---

# Ping IP Address

```bash id="jlwm1f"
ping 8.8.8.8
```

---

# Send Limited Packets

```bash id="jlwm7f"
ping -c 4 google.com
```

---

# Important Information

Ping shows:

* Connectivity
* Packet loss
* Latency
* TTL

---

# Common Problems

| Error                        | Meaning                 |
| ---------------------------- | ----------------------- |
| Network unreachable          | Routing issue           |
| Destination host unreachable | Gateway/interface issue |
| Request timeout              | Firewall or packet loss |

---

# 7.3 `ss` Command

Displays:

* Sockets
* Connections
* Listening ports

Modern replacement for:

```text id="jlwm4c"
netstat
```

---

# Show All Connections

```bash id="jlwm9f"
ss
```

---

# Show Listening Ports

```bash id="jlwm0q"
ss -l
```

---

# Show TCP Ports

```bash id="jlwm2d"
ss -lt
```

---

# Show UDP Ports

```bash id="jlwm5c"
ss -lu
```

---

# Show Process Information

```bash id="jlwm8s"
ss -tulnp
```

---

# Example Output

```text id="jlwm3y"
tcp LISTEN 0 128 0.0.0.0:22
```

Means:

* TCP
* Listening
* Port 22
* SSH service active

---

# 7.4 `netstat`

Older networking tool.

Still commonly seen.

---

# Install (if missing)

```bash id="jlwm7h"
sudo apt install net-tools
```

---

# Show Listening Ports

```bash id="jlwm1y"
netstat -tulnp
```

---

# Show Routing Table

```bash id="jlwm6x"
netstat -rn
```

---

# Show Interface Statistics

```bash id="jlwm4r"
netstat -i
```

---

# Important Note

Modern Linux prefers:

```text id="jlwm9c"
ss
```

instead of `netstat`.

---

# 7.5 `traceroute`

Shows path packets take to destination.

---

# Install

Ubuntu/Debian:

```bash id="jlwm2c"
sudo apt install traceroute
```

---

# Run Traceroute

```bash id="jlwm5h"
traceroute google.com
```

---

# Purpose

Helps identify:

* Routing problems
* Slow network hops
* Packet drops

---

# Example Output

```text id="jlwm8m"
1 192.168.1.1
2 ISP-router
3 google-network
```

---

# 7.6 `dig`

Advanced DNS lookup tool.

---

# Basic Lookup

```bash id="jlwm3u"
dig google.com
```

---

# Query Specific DNS Server

```bash id="jlwm0x"
dig @8.8.8.8 google.com
```

---

# Reverse Lookup

```bash id="jlwm6m"
dig -x 8.8.8.8
```

---

# Why `dig` is Important

Used for:

* DNS troubleshooting
* Record verification
* Mail server debugging

---

# 7.7 `nslookup`

Simpler DNS lookup command.

---

# Example

```bash id="jlwm1h"
nslookup google.com
```

---

# Query Specific DNS Server

```bash id="jlwm4h"
nslookup google.com 8.8.8.8
```

---

# Difference Between `dig` and `nslookup`

| dig                  | nslookup              |
| -------------------- | --------------------- |
| Advanced             | Simple                |
| Detailed output      | Basic output          |
| Preferred for admins | Good for quick checks |

---

# 7.8 `curl`

Transfers data from servers.

Very important in Linux.

---

# Fetch Web Page

```bash id="jlwm7m"
curl https://example.com
```

---

# View Headers

```bash id="jlwm2w"
curl -I https://example.com
```

---

# Download File

```bash id="jlwm5s"
curl -O https://example.com/file.txt
```

---

# Common Uses

* API testing
* Web troubleshooting
* Health checks
* Automation

---

# 7.9 `wget`

Downloads files from internet.

---

# Download File

```bash id="jlwm8q"
wget https://example.com/file.iso
```

---

# Continue Interrupted Download

```bash id="jlwm3p"
wget -c https://example.com/file.iso
```

---

# Difference Between `curl` and `wget`

| curl          | wget                |
| ------------- | ------------------- |
| Data transfer | File downloading    |
| APIs          | Recursive downloads |
| Flexible      | Simpler downloads   |

---

# 7.10 `hostname`

Displays or changes hostname.

---

# Show Hostname

```bash id="jlwm0j"
hostname
```

---

# Show Detailed Info

```bash id="jlwm6p"
hostnamectl
```

---

# Change Hostname

```bash id="jlwm1s"
sudo hostnamectl set-hostname server01
```

---

# 7.11 `ethtool`

Displays Ethernet interface details.

---

# View Interface Details

```bash id="jlwm4m"
ethtool ens33
```

---

# Show Driver Info

```bash id="jlwm9s"
ethtool -i ens33
```

---

# Important Information

Shows:

* Speed
* Duplex
* Link status
* Driver

---

# Example

```text id="jlwm2z"
Speed: 1000Mb/s
Duplex: Full
Link detected: yes
```

---

# 7.12 `nmcli`

Command-line tool for NetworkManager.

---

# Show Connections

```bash id="jlwm5j"
nmcli connection show
```

---

# Show Devices

```bash id="jlwm8v"
nmcli device status
```

---

# Connect Wi-Fi

```bash id="’wini1"
nmcli dev wifi connect WIFI_NAME password PASSWORD
```

---

# Restart Networking

```bash id="’wini4"
sudo systemctl restart NetworkManager
```

---

# 7.13 `arp` Command

Shows ARP table.

---

# View ARP Cache

```bash id="’wini7"
arp -a
```

Modern alternative:

```bash id="្មីន0"
ip neigh
```

---

# 7.14 `route` Command

Shows routing table.

Older command:

```bash id="្មីន3"
route -n
```

Modern replacement:

```bash id="្មីន6"
ip route
```

---

# Useful Command Combination

---

# Full Network Diagnostics

```bash id="្មីន9"
ip addr
ip route
ping 8.8.8.8
dig google.com
ss -tulnp
```

---

# Real Linux Troubleshooting Workflow

---

## Step 1: Check Interface

```bash id="្មីន2"
ip link
```

---

## Step 2: Check IP

```bash id="្មីន5"
ip addr
```

---

## Step 3: Check Gateway

```bash id="្មីន8"
ip route
```

---

## Step 4: Test Connectivity

```bash id="្មីន1"
ping 8.8.8.8
```

---

## Step 5: Test DNS

```bash id="្មីន4"
dig google.com
```

---

## Step 6: Check Listening Services

```bash id="្មីន7"
ss -tulnp
```

---

# Important Modern Linux Networking Tools

| Modern Tool | Old Tool            |
| ----------- | ------------------- |
| `ip`        | `ifconfig`, `route` |
| `ss`        | `netstat`           |
| `ip neigh`  | `arp`               |

---

# Key Takeaways

* Linux provides powerful networking commands
* `ip` is the most important networking command
* `ping` tests connectivity
* `ss` checks ports and sockets
* `dig` and `nslookup` troubleshoot DNS
* `curl` and `wget` work with web/network data
* These commands are essential for:

  * Linux administration
  * Networking
  * Cloud
  * DevOps
  * Security troubleshooting
