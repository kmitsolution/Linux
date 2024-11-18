Here are some useful Ubuntu commands across various categories:

### **System Information**
1. **Check system information**:
   - `uname -a` — Displays system information (kernel, architecture, etc.).
   - `lsb_release -a` — Displays Ubuntu version information.
   - `hostname` — Displays the system’s hostname.
   - `top` — Task manager, shows CPU and memory usage.
   - `htop` — Enhanced version of `top` (requires installation: `sudo apt install htop`).
   - `df -h` — Displays disk space usage.
   - `free -h` — Displays memory usage.

### **Package Management**
1. **Update package list**:
   - `sudo apt update` — Updates the list of available packages.
2. **Upgrade packages**:
   - `sudo apt upgrade` — Installs available updates for installed packages.
   - `sudo apt full-upgrade` — Performs a full upgrade, including removing packages if necessary.
3. **Install a package**:
   - `sudo apt install <package_name>` — Installs a package.
4. **Remove a package**:
   - `sudo apt remove <package_name>` — Removes a package (keeps config files).
   - `sudo apt purge <package_name>` — Removes a package along with its configuration files.
5. **Search for a package**:
   - `apt search <package_name>` — Searches for a package in repositories.
6. **Show package details**:
   - `apt show <package_name>` — Displays details about a package.

### **File and Directory Operations**
1. **List files**:
   - `ls` — Lists files in the current directory.
   - `ls -l` — Lists files with detailed information.
   - `ls -a` — Lists all files, including hidden files.
2. **Change directory**:
   - `cd <directory>` — Changes to the specified directory.
   - `cd ..` — Moves one directory up.
   - `cd ~` — Moves to the home directory.
3. **Create a directory**:
   - `mkdir <directory_name>` — Creates a new directory.
4. **Remove a file**:
   - `rm <file_name>` — Removes a file.
   - `rm -rf <directory_name>` — Removes a directory and its contents recursively.
5. **Copy a file**:
   - `cp <source> <destination>` — Copies a file or directory.
6. **Move/Rename a file**:
   - `mv <source> <destination>` — Moves or renames a file.
7. **View file contents**:
   - `cat <file_name>` — Displays the content of a file.
   - `less <file_name>` — View file content with scroll support.
   - `tail -f <file_name>` — View the end of a file in real-time (great for logs).

### **File Permissions and Ownership**
1. **Change file permissions**:
   - `chmod <permissions> <file_name>` — Changes file permissions (e.g., `chmod 755 <file_name>`).
2. **Change file ownership**:
   - `chown <user>:<group> <file_name>` — Changes ownership of a file.

### **Process Management**
1. **List running processes**:
   - `ps aux` — Lists all running processes.
   - `top` — Interactive process viewer.
2. **Kill a process**:
   - `kill <pid>` — Terminates a process by its process ID (PID).
   - `kill -9 <pid>` — Forcefully kills a process.
3. **Find a process**:
   - `pgrep <process_name>` — Finds the PID of a process.
4. **Show system resource usage**:
   - `uptime` — Displays how long the system has been running, current time, load averages.
   - `vmstat` — Displays system performance (memory, CPU, disk).

### **Networking**
1. **Check IP address**:
   - `ip a` or `ifconfig` — Displays network interface information.
2. **Ping a host**:
   - `ping <hostname_or_ip>` — Pings a host to check connectivity.
3. **Check open ports**:
   - `sudo netstat -tuln` — Lists open ports and associated services.
   - `sudo ss -tuln` — Alternative to `netstat` for listing open ports.
4. **Trace route**:
   - `traceroute <hostname_or_ip>` — Traces the route packets take to a network host.
   - (Requires installation: `sudo apt install traceroute`).

### **User and Group Management**
1. **Add a user**:
   - `sudo adduser <username>` — Adds a new user.
2. **Add a group**:
   - `sudo addgroup <groupname>` — Adds a new group.
3. **Add user to group**:
   - `sudo usermod -aG <groupname> <username>` — Adds a user to a group.
4. **Delete a user**:
   - `sudo deluser <username>` — Deletes a user.
5. **Switch user**:
   - `su <username>` — Switches to another user.
   - `sudo -u <username> <command>` — Runs a command as another user.

### **Disk Management**
1. **Check disk usage**:
   - `df -h` — Displays disk space usage in human-readable format.
2. **Check disk partitions**:
   - `sudo fdisk -l` — Lists all disk partitions.
3. **Create a new partition**:
   - `sudo fdisk /dev/sda` — Create partitions on `/dev/sda` disk.
4. **Mount a disk**:
   - `sudo mount <device> <mount_point>` — Mounts a filesystem.
5. **Unmount a disk**:
   - `sudo umount <mount_point>` — Unmounts a filesystem.

### **System Services**
1. **Check status of a service**:
   - `sudo systemctl status <service_name>` — Displays the status of a service.
2. **Start/Stop/Restart a service**:
   - `sudo systemctl start <service_name>` — Starts a service.
   - `sudo systemctl stop <service_name>` — Stops a service.
   - `sudo systemctl restart <service_name>` — Restarts a service.
3. **Enable/Disable a service at boot**:
   - `sudo systemctl enable <service_name>` — Enables a service to start on boot.
   - `sudo systemctl disable <service_name>` — Disables a service from starting on boot.

### **System Shutdown/Restart**
1. **Shutdown system**:
   - `sudo shutdown now` — Immediately shuts down the system.
   - `sudo shutdown -h +10` — Shuts down the system in 10 minutes.
2. **Restart system**:
   - `sudo reboot` — Restarts the system.
3. **Power off**:
   - `sudo poweroff` — Powers off the system.

### **Miscellaneous**
1. **Search for a file**:
   - `find /path/to/search -name <filename>` — Searches for a file by name.
2. **Search command history**:
   - `history | grep <command>` — Searches through the command history.
3. **Show disk usage by directory**:
   - `du -sh <directory>` — Shows the total disk usage of a directory.
4. **Clear terminal screen**:
   - `clear` — Clears the terminal screen.
5. **Check last login**:
   - `last` — Shows last logins of all users.

These commands will help you navigate, manage, and troubleshoot your Ubuntu system. Let me know if you'd like to dive deeper into any specific topic!
