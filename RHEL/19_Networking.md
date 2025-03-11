# Network Interfaces
Here's an overview of how to work with network interfaces, manage services, and troubleshoot network issues on RHEL using some common tools like `nmcli`, `ip`, `systemd`, `tcpdump`, `netstat`, and `ss`.

### 1. **Configuring Network Interfaces with `nmcli` and `ip`**

#### **Using `nmcli` (NetworkManager CLI)**

- **View the status of network interfaces:**
  ```bash
  nmcli device status
  ```
  This will show the status of all network devices (e.g., ethernet, Wi-Fi, etc.).

- **Bring up a network interface:**
  ```bash
  nmcli device connect eth0
  ```
  This brings the `eth0` interface up and connects it.

- **Bring down a network interface:**
  ```bash
  nmcli device disconnect eth0
  ```
  This brings the `eth0` interface down.

- **Assign a static IP address:**
  ```bash
  nmcli con mod "System eth0" ipv4.addresses 192.168.1.100/24
  nmcli con mod "System eth0" ipv4.gateway 192.168.1.1
  nmcli con mod "System eth0" ipv4.dns "8.8.8.8"
  nmcli con mod "System eth0" ipv4.method manual
  nmcli con up "System eth0"
  ```
  This sets a static IP (`192.168.1.100`) to the interface `eth0`.

- **Set DHCP on an interface:**
  ```bash
  nmcli con mod "System eth0" ipv4.method auto
  nmcli con up "System eth0"
  ```

#### **Using `ip` Command**

- **View current network interfaces:**
  ```bash
  ip addr show
  ```

- **Bring up an interface:**
  ```bash
  ip link set eth0 up
  ```

- **Bring down an interface:**
  ```bash
  ip link set eth0 down
  ```

- **Assign a static IP:**
  ```bash
  ip addr add 192.168.1.100/24 dev eth0
  ```

- **Delete an IP address from an interface:**
  ```bash
  ip addr del 192.168.1.100/24 dev eth0
  ```

### 2. **Managing Services with `systemd`**

- **Start a service:**
  ```bash
  sudo systemctl start <service_name>
  ```
  Example:
  ```bash
  sudo systemctl start httpd
  ```

- **Stop a service:**
  ```bash
  sudo systemctl stop <service_name>
  ```

- **Enable a service to start on boot:**
  ```bash
  sudo systemctl enable <service_name>
  ```

- **Disable a service from starting on boot:**
  ```bash
  sudo systemctl disable <service_name>
  ```

- **Check the status of a service:**
  ```bash
  sudo systemctl status <service_name>
  ```

- **Restart a service:**
  ```bash
  sudo systemctl restart <service_name>
  ```

- **View the logs of a service (via `journalctl`):**
  ```bash
  sudo journalctl -u <service_name>
  ```
  Example:
  ```bash
  sudo journalctl -u httpd
  ```

### 3. **Troubleshooting Network Issues**

#### **Using `tcpdump`**

`tcpdump` is a packet analyzer that allows you to capture and display network traffic.

- **Capture packets on a specific interface (e.g., eth0):**
  ```bash
  sudo tcpdump -i eth0
  ```

- **Capture packets from a specific host:**
  ```bash
  sudo tcpdump -i eth0 host 192.168.1.1
  ```

- **Capture only a certain number of packets:**
  ```bash
  sudo tcpdump -i eth0 -c 10
  ```

- **Capture traffic on a specific port (e.g., HTTP port 80):**
  ```bash
  sudo tcpdump -i eth0 port 80
  ```

#### **Using `netstat`**

`netstat` is a network utility that displays network connections, routing tables, interface statistics, and more.

- **Show all network connections (both listening and established):**
  ```bash
  netstat -tuln
  ```
  The `-t` flag shows TCP connections, `-u` shows UDP connections, `-l` shows only listening sockets, and `-n` shows numerical addresses.

- **Show network routing table:**
  ```bash
  netstat -rn
  ```

- **Show active network connections with PID:**
  ```bash
  netstat -tulnp
  ```

#### **Using `ss`**

`ss` is similar to `netstat`, but it is faster and more efficient.

- **Show all TCP and UDP sockets:**
  ```bash
  ss -tuln
  ```

- **Show listening sockets with associated process:**
  ```bash
  ss -tulnp
  ```

- **Check for established connections:**
  ```bash
  ss -s
  ```

- **Show connections to a specific port:**
  ```bash
  ss -tuln sport = :80
  ```

### Conclusion

By using tools like `nmcli` and `ip` for managing network interfaces, `systemd` for service management, and `tcpdump`, `netstat`, and `ss` for troubleshooting, you can efficiently configure and monitor your RHEL system's network setup and diagnose any issues.
