Below is a **structured documentation-style explanation** for your lab setup with **3 systems and iptables traffic restriction**.

---

# RHEL iptables Lab Documentation

### Restricting and Allowing Traffic Between Ubuntu Input and Ubuntu Output

<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/4df82b35-4449-4a6b-be2a-fbdd4e483ffd" />



## 1. Lab Objective

The purpose of this lab is to:

* Configure a **RHEL machine as a router/firewall**
* Use **iptables** to control network traffic
* Restrict direct communication with the RHEL firewall
* Allow communication **between Ubuntu Input and Ubuntu Output systems**
* Demonstrate how to **add and remove iptables restrictions**

---

# 2. Network Topology

```
                192.168.0.0/24 (Input Network)
                        
                 Ubuntu Input
                 192.168.0.10
                       |
                       |
                enp0s3 192.168.0.3
                   RHEL Firewall
                enp0s8 172.24.0.3
                       |
                       |
                 Ubuntu Output
                 172.24.0.10

             172.24.0.0/24 (Output Network)
```

---

# 3. System Network Configuration

## RHEL Firewall Machine

| Interface | IP Address     | Network        |
| --------- | -------------- | -------------- |
| enp0s3    | 192.168.0.3/24 | Input Network  |
| enp0s8    | 172.24.0.3/24  | Output Network |

---

## Ubuntu Input Machine

| Interface | IP Address      | Network             |
| --------- | --------------- | ------------------- |
| enp0s8    | 192.168.0.10/24 | 192.168.0.0 Network |

Gateway should be:

```
192.168.0.3
```

---

## Ubuntu Output Machine

| Interface | IP Address     | Network            |
| --------- | -------------- | ------------------ |
| enp0s8    | 172.24.0.10/24 | 172.24.0.0 Network |

Gateway should be:

```
172.24.0.3
```

---

# 4. Enable Packet Forwarding on RHEL

RHEL must act as a **router**.

Check:

```bash
cat /proc/sys/net/ipv4/ip_forward
```

Enable temporarily:

```bash
echo 1 > /proc/sys/net/ipv4/ip_forward
```

Permanent configuration:

```
vi /etc/sysctl.conf
```

Add:

```
net.ipv4.ip_forward = 1
```

Apply:

```bash
sysctl -p
```

---

# 5. Verify Connectivity (Before iptables Rules)

From **Ubuntu Input**

```
ping 192.168.0.3
```

```
ping 172.24.0.10
```

From **Ubuntu Output**

```
ping 172.24.0.3
```

```
ping 192.168.0.10
```

All should work before applying firewall rules.

---

# 6. Flush Existing iptables Rules

Before starting the lab clear existing rules.

```bash
iptables -F
iptables -X
iptables -t nat -F
```

---

# 7. Restrict All Incoming Traffic

Set default policy to **DROP**

```bash
iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT ACCEPT
```

This blocks all incoming and forwarded traffic.

---

# 8. Allow Ubuntu Input → Ubuntu Output Communication

###  Verify Default Gateway on Ubuntu Machines

### Ubuntu Input

Check:

```bash
ip route
```

You should see:

```
default via 192.168.0.3
```

If not:

```bash
sudo ip route add default via 192.168.0.3
```

---

### Ubuntu Output

Check:

```bash
ip route
```

You should see:

```
default via 172.24.0.3
```

If not:

```bash
sudo ip route add default via 172.24.0.3
```

Without gateways **packets never reach RHEL for forwarding**.

---

Allow forwarding between both networks.

```bash
iptables -A FORWARD -s 192.168.0.10 -d 172.24.0.10 -j ACCEPT
iptables -A FORWARD -s 172.24.0.10 -d 192.168.0.10 -j ACCEPT
```

This allows both Ubuntu machines to communicate **through the RHEL firewall**.

---

# 9. Restrict Ubuntu Input From Accessing RHEL

Block access to the firewall itself.

```bash
iptables -A INPUT -s 192.168.0.10 -j DROP
```

Result:

Ubuntu Input:

❌ Cannot ping RHEL
✅ Can communicate with Ubuntu Output

---

# 10. Allow Established Connections

Add rule for return traffic.

```bash
iptables -A FORWARD -m state --state ESTABLISHED,RELATED -j ACCEPT
```

---

# 11. Verify Rules

List rules:

```bash
iptables -L -n -v
```

Example output:

```
Chain INPUT (policy DROP)
target     prot opt source            destination

Chain FORWARD (policy DROP)
ACCEPT     all  -- 192.168.0.10       172.24.0.10
ACCEPT     all  -- 172.24.0.10        192.168.0.10
```

---

# 12. Test Connectivity

From **Ubuntu Input**

```
ping 192.168.0.3
```

Result:

❌ blocked

```
ping 172.24.0.10
```

Result:

✅ allowed

---

# 13. Remove iptables Restrictions

To remove all firewall rules:

```bash
iptables -F
```

Reset policies:

```bash
iptables -P INPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -P OUTPUT ACCEPT
```

Now all traffic will be allowed again.

---

# 14. Save Rules (Optional)

On RHEL:

```
service iptables save
```

or

```
iptables-save > /etc/sysconfig/iptables
```

---

# 15. Summary

| Machine       | IP                       |
| ------------- | ------------------------ |
| Ubuntu Input  | 192.168.0.10             |
| RHEL Firewall | 192.168.0.3 / 172.24.0.3 |
| Ubuntu Output | 172.24.0.10              |

Firewall behaviour:

| Traffic                      | Result  |
| ---------------------------- | ------- |
| Ubuntu Input → RHEL          | Blocked |
| Ubuntu Input → Ubuntu Output | Allowed |
| Ubuntu Output → RHEL         | Allowed |
| Ubuntu Output → Ubuntu Input | Allowed |

---

