**NFS (Network File System)** is a distributed file system protocol that allows a system to share directories and files with other systems over a network. In Red Hat Enterprise Linux (RHEL), you can configure an **NFS server** to share files with clients and an **NFS client** to mount shared directories from an NFS server.

Here’s how you can set up and configure NFS on **RHEL**.

### 1. **Install NFS Packages**

To set up an NFS server and client on RHEL, you need to install the **nfs-utils** package. 

#### On the NFS Server:
To install the NFS server package:

```bash
sudo yum install -y nfs-utils
```

#### On the NFS Client:
To install the NFS client package:

```bash
sudo yum install -y nfs-utils
```

### 2. **Configure NFS Server**

Once the NFS packages are installed, you need to configure the **NFS server** to share a directory.

#### 2.1. **Start and Enable NFS Services**

To enable and start the NFS server, run:

```bash
sudo systemctl enable nfs-server
sudo systemctl start nfs-server
```

#### 2.2. **Create a Directory to Share**

Choose or create a directory that will be shared over NFS. For example:

```bash
sudo mkdir -p /srv/nfs/shared
```

Set the appropriate permissions for the directory you want to share:

```bash
sudo chown -R nfsnobody:nfsnobody /srv/nfs/shared
sudo chmod 755 /srv/nfs/shared
```

#### 2.3. **Configure `/etc/exports`**

Edit the `/etc/exports` file to specify which directories you want to share and with whom. For example, to share the `/srv/nfs/shared` directory with a specific client, add the following line:

```bash
sudo vi /etc/exports
```

Add the following line to share `/srv/nfs/shared` with a specific client or network:

```
/srv/nfs/shared 192.168.1.0/24(rw,sync,no_root_squash)
```

- `rw`: Read and write access.
- `sync`: Ensure changes are written to disk before the server responds.
- `no_root_squash`: Allow root users on the client to have root privileges on the shared directory (use with caution).
- `192.168.1.0/24`: IP range or specific IP address that can access the share. You can replace it with a single IP (e.g., `192.168.1.100`), or a network range.

#### 2.4. **Export the Shared Directory**

After configuring the `/etc/exports` file, apply the changes by running:

```bash
sudo exportfs -r
```

This will re-export the file systems as per the configuration in `/etc/exports`.

#### 2.5. **Allow NFS in the Firewall**

Ensure that the necessary ports for NFS are allowed in the firewall. By default, NFS uses ports 2049, as well as other ports for NFS-related services.

For **RHEL 7 and later**:

```bash
sudo firewall-cmd --permanent --add-service=nfs
sudo firewall-cmd --permanent --add-service=mountd
sudo firewall-cmd --permanent --add-service=rpc-bind
sudo firewall-cmd --reload
```

### 3. **Configure NFS Client**

Once the server is configured to share directories, you need to configure the NFS client to mount the shared directory.

#### 3.1. **Mount the Shared Directory on the Client**

On the NFS client, create a directory where the shared directory will be mounted:

```bash
sudo mkdir -p /mnt/nfs/shared
```

Mount the NFS share from the server:

```bash
sudo mount 192.168.1.100:/srv/nfs/shared /mnt/nfs/shared
```

Replace `192.168.1.100` with the IP address of your NFS server. You should now be able to access the shared directory at `/mnt/nfs/shared`.

#### 3.2. **Verify the NFS Mount**

Check if the NFS mount was successful:

```bash
df -h /mnt/nfs/shared
```

You should see output that shows the shared NFS directory is mounted.

#### 3.3. **Make the Mount Permanent**

To ensure the NFS mount is automatically mounted on boot, add an entry to the `/etc/fstab` file on the client.

Edit `/etc/fstab`:

```bash
sudo vi /etc/fstab
```

Add the following line:

```
192.168.1.100:/srv/nfs/shared /mnt/nfs/shared nfs defaults 0 0
```

This will automatically mount the NFS share at boot time.

### 4. **Test the NFS Share**

- **From the NFS server**, you can test if the directory is being shared by checking the NFS status:

  ```bash
  sudo exportfs -v
  ```

  This will list all the shared directories and their configurations.

- **From the NFS client**, you can test the shared directory by creating a file in the mounted directory:

  ```bash
  sudo touch /mnt/nfs/shared/testfile
  sudo ls -l /mnt/nfs/shared
  ```

  You should be able to see `testfile` in the shared directory.

### 5. **Unmount NFS Share (Optional)**

If you need to unmount the NFS share on the client, you can use the following command:

```bash
sudo umount /mnt/nfs/shared
```

If you made the mount permanent by adding it to `/etc/fstab`, you can remove the entry to prevent the mount from occurring at boot time.

### 6. **Troubleshooting NFS**

- If you encounter any issues, you can check the NFS logs for errors:

  On the server:
  ```bash
  sudo journalctl -xe | grep nfs
  ```

  On the client:
  ```bash
  sudo journalctl -xe | grep nfs
  ```

- Ensure both the server and client can reach each other by using `ping` to test connectivity.

### Conclusion

Setting up NFS on RHEL is straightforward once the necessary packages are installed and configured. By following the steps outlined above, you should be able to share directories between RHEL systems using NFS. Make sure to adjust the firewall and SELinux settings as needed for secure and functional NFS sharing.
