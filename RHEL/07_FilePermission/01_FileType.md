### **File Types in Linux**

In Linux, file types are denoted by the first character in the file listing when using the `ls -l` command. Here's what each character means:

1. **`d`** – **Directory**  
   - Indicates that the file is a directory.
   - Example:  
     ```
     drwxr-xr-x 2 user user 4096 Dec  7 10:00 test_directory
     ```

2. **`l`** – **Symbolic Link**  
   - Indicates that the file is a symbolic link (a reference to another file or directory).
   - Example:
     ```
     lrwxrwxrwx 1 user user 14 Dec  7 10:00 link_to_file -> /home/user/file.txt
     ```

3. **`p`** – **Named Pipe (FIFO)**  
   - Indicates that the file is a named pipe (FIFO), a special type of file used for inter-process communication.
   - Example:
     ```
     prw-r--r-- 1 user user 0 Dec  7 10:00 my_pipe
     ```

4. **`b`** – **Block Device**  
   - Indicates that the file is a block device, such as a hard drive or USB device.
   - Example:
     ```
     brw-rw---- 1 root disk 8, 1 Dec  7 10:00 /dev/sda1
     ```

5. **`c`** – **Character Device**  
   - Indicates that the file is a character device, such as a terminal or a serial port.
   - Example:
     ```
     crw-rw-rw- 1 root tty 5, 1 Dec  7 10:00 /dev/tty1
     ```

6. **`s`** – **Socket**  
   - Indicates that the file is a socket, a special file used for inter-process communication.
   - Example:
     ```
     srwxrwxrwx 1 user user 0 Dec  7 10:00 /tmp/socketfile
     ```

### **File Ownership Commands**

1. **`chown` Command (Change Ownership)**
   - Used to change the ownership of a file or directory.
   - Syntax:  
     ```
     chown [OPTION] OWNER[:GROUP] FILE(s)
     ```
   - Example:  
     To change the owner of `file1.txt` to `user2` and the group to `staff`:
     ```
     chown user2:staff file1.txt
     ```
     To change only the owner:
     ```
     chown user2 file1.txt
     ```
