To **add** or **remove** specific services like HTTP (port 80) or ICMP (used by the `ping` command) in the firewall configuration using **firewalld** in RHEL, you can use the `firewall-cmd` command.

### 1. **Adding HTTP or ICMP to Firewalld**

To **allow** HTTP (port 80) or ICMP (ping) traffic through the firewall, follow these steps.

#### 1.1 **Allow HTTP (port 80) through the firewall**

- To allow **HTTP** traffic (port 80), run the following command:
  ```bash
  sudo firewall-cmd --zone=public --add-service=http --permanent
  ```

  - `--zone=public`: Specifies the network zone to apply the rule to. You can change this to a different zone if necessary (like `home`, `internal`, etc.).
  - `--add-service=http`: This adds the HTTP service (which uses port 80) to the firewall rule.
  - `--permanent`: This makes the rule persistent across reboots.

- To apply the changes, reload the firewall:
  ```bash
  sudo firewall-cmd --reload
  ```

#### 1.2 **Allow ICMP (ping) traffic through the firewall**


## What ICMP Actually Is

ICMP = Internet Control Message Protocol

Used for:

* `ping`
* traceroute
* network error messages

It is a protocol, not a service.

---

## How firewalld Handles ICMP

Firewalld manages ICMP separately using:

```bash id="t50dxa"
firewall-cmd --get-icmptypes
```

Example output:

```text id="mbx1qo"
echo-request
echo-reply
destination-unreachable
time-exceeded
```

---

## Allow or Block Ping

### Block ping

```bash id="5on1nk"
sudo firewall-cmd --add-icmp-block=echo-request
```

Permanent:

```bash id="ozg88g"
sudo firewall-cmd --permanent --add-icmp-block=echo-request
sudo firewall-cmd --reload
```

---

### Remove block

```bash id="rzc79v"
sudo firewall-cmd --remove-icmp-block=echo-request
```

---

## Check ICMP Blocks

```bash id="5gk92p"
firewall-cmd --list-icmp-blocks
```

---



#### 2.1 **Remove HTTP (port 80) from the firewall**

- To remove **HTTP** (port 80), run:
  ```bash
  sudo firewall-cmd --zone=public --remove-service=http --permanent
  ```

- Reload the firewall to apply the changes:
  ```bash
  sudo firewall-cmd --reload
  ```

  ```

### 3. **Verifying Firewall Changes**

To check the current active firewall rules and confirm whether HTTP or ICMP is allowed or removed, you can use:

- To list all the services allowed in the `public` zone:
  ```bash
  sudo firewall-cmd --zone=public --list-services
  ```

- To list all open ports:
  ```bash
  sudo firewall-cmd --zone=public --list-ports
  ```

These commands will show you the current firewall configuration and confirm whether the desired changes (such as allowing or blocking HTTP or ICMP) have been applied.

### Summary:
- **Allow HTTP (port 80)**:
  ```bash
  sudo firewall-cmd --zone=public --add-service=http --permanent
  sudo firewall-cmd --reload
  ```

  ```

- **Remove HTTP (port 80)**:
  ```bash
  sudo firewall-cmd --zone=public --remove-service=http --permanent
  sudo firewall-cmd --reload
  ```


This provides a basic and flexible approach to controlling access to your system's services and managing firewall rules.
