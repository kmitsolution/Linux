# RHEL 9 NTP Server and Client Configuration Documentation

This document provides a step-by-step guide to setting up and configuring an NTP server and client using Chrony on RHEL 9.

---

## **NTP Server Configuration (Chrony)**

The NTP (Network Time Protocol) server configuration allows your system to synchronize its time with external sources and provide time synchronization to client systems.

### **Step 1: Install Chrony**
First, we need to install the Chrony package, which is the default NTP service on RHEL 9.

```bash
dnf install chrony* -y
```

### **Step 2: Start Chrony Service**
Start the Chrony service to enable time synchronization on your server.

```bash
systemctl start chronyd
```

### **Step 3: Verify the Chrony Service Status**
Check the status of the Chrony service to ensure it is running properly.

```bash
systemctl status chronyd
```

### **Step 4: Configure the Chrony Configuration File**
Edit the `chrony.conf` file to allow client connections.

```bash
vi /etc/chrony.conf
```

- Uncomment the line `# allow` and add the IP address of the clients that will be allowed to connect to the server. For example:

```bash
allow 192.168.1.0/24
```

This allows all clients within the 192.168.1.0/24 network to synchronize time with the NTP server.

### **Step 5: Restart the Chrony Service**
After making changes to the configuration file, restart the Chrony service to apply the changes.

```bash
systemctl restart chronyd
```

### **Step 6: Open NTP Ports in the Firewall**
To allow NTP traffic through the firewall, run the following command to add the NTP service to the firewall configuration.

```bash
firewall-cmd --permanent --add-service=ntp
```

### **Step 7: Install Firewall Tools (if not installed)**
If firewall tools are not installed, use the following commands to install them.

```bash
yum install firewall
yum install firewall-cmd
yum install firewalld
```

### **Step 8: Restart the Firewall**
After configuring the firewall, restart it to apply the changes.

```bash
systemctl restart firewalld
```

### **Step 9: Verify the Firewall Configuration**
To verify the NTP service is allowed, use the following commands to check the firewall settings:

```bash
firewall-cmd --permanent --add-service=ntp
firewall-cmd reload
```

### **Step 10: Check Clients**
You can check the connected clients by using the following command:

```bash
chronyc clients
```

### **Step 11: Verify the Date**
Finally, verify the current date and time of the NTP server.

```bash
date
```

---

## **NTP Client Configuration (Chrony)**

The NTP client configuration allows the system to synchronize its time with an NTP server.

### **Step 1: Check Network Configuration**
On the client machine, check the network interface settings to ensure the system is properly connected to the network.

```bash
ip a
```

### **Step 2: Verify Chrony Service Status**
Check the status of the Chrony service on the client to ensure it's running.

```bash
systemctl status chronyd
```

### **Step 3: Configure the Chrony Client File**
Edit the `chrony.conf` file to specify the NTP server IP address.

```bash
vi /etc/chrony.conf
```

- Add the following line to the configuration file:

```bash
server 172.31.1.90
```

Replace `172.31.1.90` with the IP address of your NTP server.

### **Step 4: Restart the Chrony Service**
After modifying the configuration, restart the Chrony service to apply the changes.

```bash
systemctl restart chronyd
```

### **Step 5: Verify Time Synchronization Sources**
Check the synchronization sources to confirm that the client is connected to the NTP server.

```bash
chronyc sources
```

### **Step 6: Verify the Date**
You can check the current date and time on the client machine.

```bash
date
```

### **Step 7: Set Date and Time Manually (Optional)**
If needed, you can manually set the system time using the `timedatectl` command.

```bash
timedatectl set-time 10:08:08
```

### **Step 8: Stop the Chrony Service (Optional)**
If you need to stop the Chrony service for any reason, use the following command:

```bash
systemctl stop chronyd
```

### **Step 9: Check Chrony Service Status**
Check the status of the Chrony service to confirm it has stopped.

```bash
systemctl status chronyd
```

### **Step 10: Restart the Chrony Service**
To restart the Chrony service, run:

```bash
systemctl start chronyd
```

### **Step 11: Check Date Again**
Finally, verify the date again after restarting the Chrony service.

```bash
date
```

---

## **Conclusion**

This guide covers the setup of an NTP server and client using Chrony on RHEL 9. The NTP server is configured to provide time synchronization to clients, and the firewall is configured to allow NTP traffic. The NTP client is then configured to synchronize time with the NTP server.

For ongoing time synchronization management, you can check the Chrony service status, verify clients, and ensure that the system time is accurate.

