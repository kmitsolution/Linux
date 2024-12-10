**`firewalld`** is a dynamic firewall management tool available in RHEL (Red Hat Enterprise Linux) and other Linux distributions. It provides an easier and more flexible way to configure and manage firewall rules compared to traditional tools like `iptables`. `firewalld` uses zones to manage network traffic, making it easier to define rules for different network interfaces and services.

### Installing `firewalld` on RHEL

By default, `firewalld` is usually installed and enabled in RHEL 7 and later versions. However, if it is not installed, you can install it manually.

1. **Install `firewalld`**:

   Run the following command to install `firewalld` on a RHEL system:
   ```bash
   sudo yum install firewalld
   ```

2. **Check if `firewalld` is installed**:
   After installation, check if `firewalld` is installed by running:
   ```bash
   rpm -q firewalld
   ```

### Managing `firewalld` Service

Once `firewalld` is installed, you can manage it with `systemctl`, the system and service manager in RHEL.

#### 1. **Start `firewalld` Service**
   To start the `firewalld` service:
   ```bash
   sudo systemctl start firewalld
   ```

#### 2. **Enable `firewalld` to Start on Boot**
   To ensure `firewalld` starts automatically at boot time, use the following command:
   ```bash
   sudo systemctl enable firewalld
   ```

#### 3. **Check Status of `firewalld`**
   To check the status of `firewalld` (whether it's running or inactive):
   ```bash
   sudo systemctl status firewalld
   ```

   This will show whether the service is active, inactive, or failed.

#### 4. **Stop `firewalld` Service**
   To stop the `firewalld` service:
   ```bash
   sudo systemctl stop firewalld
   ```

#### 5. **Disable `firewalld` on Boot**
   To disable `firewalld` from starting automatically at boot:
   ```bash
   sudo systemctl disable firewalld
   ```

#### 6. **Restart `firewalld`**
   If you make changes to the firewall settings, you may need to restart the service:
   ```bash
   sudo systemctl restart firewalld
   ```

### Verify `firewalld` Status

After starting the service, you can verify that `firewalld` is working by running:
```bash
sudo firewall-cmd --state
```
This will return either `running` or `not running`, depending on the state of the `firewalld` service.

