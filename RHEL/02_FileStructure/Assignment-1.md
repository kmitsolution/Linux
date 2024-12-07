Here’s the **Linux Command Lab Assignment** with the answers at the bottom:

---

### **Linux Command Lab Assignment**

#### **Part 1: Basic Commands & Navigation**

1. **Display the Files:**
   - Use the `ls` command to list all the files and directories in your home directory. Include both visible and hidden files. What command did you use? Write the output format you see.
   
2. **Change Directories:**
   - Navigate to the `/var/log` directory. Once you're there, run `pwd` to confirm the current directory. 

3. **Create a Directory:**
   - Create a directory called `test_lab` in your home directory. Verify the creation of the directory using the `ls` command.

4. **Create Files:**
   - Inside the `test_lab` directory, create three empty files named `file1.txt`, `file2.txt`, and `file3.txt`. You can use any method to create the files. List the files in the directory after creation.

#### **Part 2: File Operations**

5. **Copy Files:**
   - Copy `file1.txt` from the `test_lab` directory to `/tmp`. Use the `cp` command and confirm the file was copied by listing the contents of `/tmp`.

6. **Move Files:**
   - Move `file2.txt` from the `test_lab` directory to `/tmp/backup`. If the `backup` directory doesn't exist, create it first. Verify the move operation by listing the files in both directories.

7. **Rename a File:**
   - Rename `file3.txt` to `renamed_file.txt` in the `test_lab` directory. Use the `mv` command and verify the change with the `ls` command.

8. **Using Wildcards:**
   - List all `.txt` files in the `test_lab` directory using a wildcard. Explain how the wildcard works in your command.

#### **Part 3: Redirection and File Information**

9. **Output Redirection:**
   - Redirect the output of `ls` (listing files in the current directory) to a file called `file_list.txt` in the `test_lab` directory. Verify the contents of the `file_list.txt` file using the `cat` command.

10. **Append Output to a File:**
    - Append the output of the `df -h` command (disk usage) to `file_list.txt`. Verify that the contents were appended successfully.

11. **Check File Type:**
    - Use the `file` command on `file1.txt` to determine the file type. Explain the output you receive.

#### **Part 4: Compression and Archiving**

12. **Create a Tar Archive:**
    - Create a `.tar` archive of the `test_lab` directory and name the archive `test_lab.tar`. Verify the creation of the archive by listing the contents in the current directory.

13. **Extract a Tar Archive:**
    - Extract the contents of `test_lab.tar` to a new directory called `extracted_test_lab`. Use the appropriate command for extraction and verify that the files were extracted successfully.

14. **Compress with Gzip:**
    - Compress the `file_list.txt` file using `gzip` and verify the creation of `file_list.txt.gz`. What is the file size change after compression?

15. **Extract Gzip File:**
    - Decompress `file_list.txt.gz` using `gunzip`. Verify the file has been restored to its original form and check the file contents using `cat`.

#### **Part 5: Cleanup and Final Verification**

16. **Clean Up:**
    - Remove the `test_lab` directory and all its contents (files, subdirectories, etc.) using the `rm` command. Ensure you use the proper flags to delete the directory and its contents recursively.

17. **Verify Deletion:**
    - Verify that `test_lab` has been deleted by running the `ls` command in your home directory. If the directory still exists, explain how you would remove it properly.

---

### **Answers:**

#### **Part 1: Basic Commands & Navigation**

1. **Display the Files:**
   - Command: `ls -ah`
   - Explanation: The `-a` option lists hidden files (those starting with a dot), and the `-h` option shows file sizes in a human-readable format.
   - Example Output:
     ```
     drwxr-xr-x  2 user user 4.0K Dec  7 12:00 test_lab
     -rw-r--r--  1 user user    0 Dec  7 12:01 .hidden_file
     -rw-r--r--  1 user user    0 Dec  7 12:01 file1.txt
     ```

2. **Change Directories:**
   - Command: `cd /var/log`
   - Command to confirm: `pwd`
   - Example Output:
     ```
     /var/log
     ```

3. **Create a Directory:**
   - Command: `mkdir test_lab`
   - Command to verify: `ls`
   - Example Output:
     ```
     test_lab
     ```

4. **Create Files:**
   - Command to create files: 
     ```
     touch test_lab/file1.txt test_lab/file2.txt test_lab/file3.txt
     ```
   - Command to list files: `ls test_lab`
   - Example Output:
     ```
     file1.txt  file2.txt  file3.txt
     ```

#### **Part 2: File Operations**

5. **Copy Files:**
   - Command: `cp test_lab/file1.txt /tmp/`
   - Verify by listing: `ls /tmp`
   - Example Output:
     ```
     file1.txt
     ```

6. **Move Files:**
   - Command to create backup directory: `mkdir /tmp/backup`
   - Command to move file: `mv test_lab/file2.txt /tmp/backup/`
   - Verify by listing: `ls /tmp/backup/` and `ls test_lab/`
   - Example Output:
     ```
     /tmp/backup/file2.txt
     ```

7. **Rename a File:**
   - Command: `mv test_lab/file3.txt test_lab/renamed_file.txt`
   - Verify with `ls test_lab/`
   - Example Output:
     ```
     file1.txt  file2.txt  renamed_file.txt
     ```

8. **Using Wildcards:**
   - Command: `ls test_lab/*.txt`
   - Explanation: The `*` wildcard matches any sequence of characters, so it lists all `.txt` files.
   - Example Output:
     ```
     file1.txt  file2.txt  renamed_file.txt
     ```

#### **Part 3: Redirection and File Information**

9. **Output Redirection:**
   - Command: `ls > test_lab/file_list.txt`
   - Verify: `cat test_lab/file_list.txt`
   - Example Output:
     ```
     file1.txt
     file2.txt
     renamed_file.txt
     ```

10. **Append Output to a File:**
    - Command: `df -h >> test_lab/file_list.txt`
    - Verify with: `cat test_lab/file_list.txt`
    - Example Output (appended):
      ```
      file1.txt
      file2.txt
      renamed_file.txt
      Filesystem      Size  Used Avail Use% Mounted on
      /dev/sda1       50G   20G   28G  43% /
      ```

11. **Check File Type:**
    - Command: `file test_lab/file1.txt`
    - Example Output:
      ```
      test_lab/file1.txt: ASCII text
      ```

#### **Part 4: Compression and Archiving**

12. **Create a Tar Archive:**
    - Command: `tar -cvf test_lab.tar test_lab/`
    - Verify: `ls`
    - Example Output:
      ```
      test_lab.tar
      ```

13. **Extract a Tar Archive:**
    - Command: `tar -xvf test_lab.tar -C extracted_test_lab/`
    - Verify with `ls extracted_test_lab/`
    - Example Output:
      ```
      file1.txt  file2.txt  renamed_file.txt
      ```

14. **Compress with Gzip:**
    - Command: `gzip test_lab/file_list.txt`
    - Verify: `ls`
    - Example Output:
      ```
      file_list.txt.gz
      ```
    - The file size will be smaller after compression.

15. **Extract Gzip File:**
    - Command: `gunzip test_lab/file_list.txt.gz`
    - Verify: `cat test_lab/file_list.txt`
    - Example Output:
      ```
      file1.txt
      file2.txt
      renamed_file.txt
      ```

#### **Part 5: Cleanup and Final Verification**

16. **Clean Up:**
    - Command: `rm -r test_lab/`
    - Verify with: `ls`
    - Example Output:
      ```
      (No output, the directory is removed)
      ```

17. **Verify Deletion:**
    - Command: `ls` (verify no `test_lab` directory)
    - If it still exists, use `rm -r test_lab/` to delete it properly.

---

This assignment includes a practical application of various Linux commands and tools for file manipulation, redirection, and archiving. Completing it will help reinforce fundamental Linux skills.
