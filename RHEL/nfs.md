# NFS (Network File System) Configuration on RHEL 9

NFS (Network File System) allows a system to share files and directories with other systems over a network. This guide provides instructions for setting up NFS server and NFS client on RHEL 9.

---

## **NFS Server Configuration**

### **Step 1: Install NFS Packages**
Start by installing the necessary NFS server packages on the system that will act as the NFS server.

```bash
dnf install -y nfs-utils
```

### **Step 2: Start and Enable NFS Services**
Enable and start the NFS server service and the NFS mountd service.

```bash
systemctl enable --now nfs-server
systemctl enable --now nfs-mountd
systemctl enable --now rpcbind
```

### **Step 3: Configure Shared Directory**
Create a directory that will be shared over NFS.

```bash
mkdir -p /mnt/nfs_share
```

Give the appropriate permissions for the directory:

```bash
chmod 755 /mnt/nfs_share
```

### **Step 4: Export the Directory**
Edit the `/etc/exports` file to specify the directory you want to share and the allowed clients. For example:

```bash
vi /etc/exports
```

Add the following line to share the directory with a specific client or network (e.g., `192.168.1.0/24`):

```bash
/mnt/nfs_share 192.168.1.0/24(rw,sync,no_root_squash)
```

- `rw`: Allow read and write access.
- `sync`: Ensure data is written to disk before responding to requests.
- `no_root_squash`: Allow the root user on the client to have root access on the NFS share.

### **Step 5: Export the NFS Shares**
Run the following command to export the NFS shares and make them available to clients:

```bash
exportfs -r
```

### **Step 6: Configure Firewall**
If the system has a firewall enabled, you need to allow NFS traffic through the firewall.

```bash
firewall-cmd --permanent --add-service=nfs
firewall-cmd --permanent --add-service=rpc-bind
firewall-cmd --permanent --add-service=mountd
firewall-cmd --reload
```

### **Step 7: Verify NFS Export**
To verify that the directory is shared, use the following command:

```bash
exportfs -v
```

This will display the list of directories being shared and the permissions.

---

## **NFS Client Configuration**

### **Step 1: Install NFS Client Packages**
On the client machine, install the necessary NFS client utilities:

```bash
dnf install -y nfs-utils
```

### **Step 2: Create Mount Point**
Create a directory on the client machine where the NFS share will be mounted.

```bash
mkdir -p /mnt/nfs_client
```

### **Step 3: Mount the NFS Share**
You can mount the NFS share from the server on the client machine using the following command:

```bash
mount -t nfs <NFS_SERVER_IP>:/mnt/nfs_share /mnt/nfs_client
```

For example, if the NFS server's IP address is `192.168.1.10`, the command would look like this:

```bash
mount -t nfs 192.168.1.10:/mnt/nfs_share /mnt/nfs_client
```

### **Step 4: Verify the NFS Mount**
Check if the NFS share is mounted correctly by using the `mount` command or by listing the contents of the mount point:

```bash
mount | grep nfs
```

or

```bash
ls /mnt/nfs_client
```

### **Step 5: Configure NFS to Mount at Boot**
To ensure that the NFS share is automatically mounted at boot, add an entry to the `/etc/fstab` file on the client machine.

```bash
vi /etc/fstab
```

Add the following line to the file:

```bash
<NFS_SERVER_IP>:/mnt/nfs_share /mnt/nfs_client nfs defaults 0 0
```

For example:

```bash
192.168.1.10:/mnt/nfs_share /mnt/nfs_client nfs defaults 0 0
```

### **Step 6: Verify Automatic Mount**
You can now test the automatic mount by unmounting the share and rebooting the system.

```bash
umount /mnt/nfs_client
reboot
```

After the system reboots, the NFS share should be automatically mounted.

---

