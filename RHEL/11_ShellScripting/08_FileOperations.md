### **8. File Operations in Shell Scripts**

File operations are fundamental in shell scripting, allowing you to work with files by reading from them, writing to them, appending data, and performing file tests. These operations are commonly used for automating tasks related to system logs, configuration management, or data processing.

---

### **8.1 Reading from Files**

You can read the contents of a file line by line using a `while` loop and the `read` command. This allows you to process each line of the file in a script.

#### **Example: Reading a File Line by Line**

```bash
#!/bin/bash

# Read each line from myfile.txt and print it
while read line; do
    echo $line
done < myfile.txt
```

- **Explanation**:
  - The `while read line; do ... done` loop reads the file `myfile.txt` line by line.
  - Each line is stored in the `line` variable, which is then echoed to the terminal.
- **Important**:
  - This method reads from the file until the end of the file (EOF).
  - You can also redirect input from a file using `< filename`, as shown in the example.

---

### **8.2 Writing to Files**

You can write data to a file using output redirection. The most common methods for writing are:

- **Overwrite the file**: Use `>` to write data to a file, overwriting its contents.
- **Append to the file**: Use `>>` to append data to an existing file.

#### **Example: Overwriting a File**

```bash
#!/bin/bash

# Write "Hello, World!" to output.txt, overwriting any existing content
echo "Hello, World!" > output.txt
```

- **Explanation**:
  - The `>` operator writes the string `"Hello, World!"` to `output.txt`, replacing any existing content.
  
#### **Example: Appending to a File**

```bash
#!/bin/bash

# Append a new line to output.txt
echo "New line" >> output.txt
```

- **Explanation**:
  - The `>>` operator appends `"New line"` to `output.txt` without overwriting its current content.

---

### **8.3 File Testing**

You often need to check whether a file exists, whether it is a regular file, a directory, or if you have permission to read/write to it. In shell scripts, you can perform these checks using conditional tests.

#### **Checking if a File Exists**

```bash
#!/bin/bash

filename="myfile.txt"

if [ -f "$filename" ]; then
    echo "File exists."
else
    echo "File does not exist."
fi
```

- **Explanation**:
  - The `-f` operator checks if the file exists and is a regular file (not a directory).
  - The script prints `"File exists."` if the file is found, or `"File does not exist."` if it is not found.

#### **Other File Test Operators**
- `-d`: Checks if the file is a directory.
- `-e`: Checks if the file exists (regardless of type).
- `-r`: Checks if the file is readable.
- `-w`: Checks if the file is writable.
- `-x`: Checks if the file is executable.

##### **Example: Check if a Directory Exists**
```bash
#!/bin/bash

dir="mydir"

if [ -d "$dir" ]; then
    echo "Directory exists."
else
    echo "Directory does not exist."
fi
```

- **Explanation**:
  - The `-d` operator checks if `mydir` exists and is a directory.

##### **Example: Checking Permissions**
```bash
#!/bin/bash

filename="myfile.txt"

if [ -r "$filename" ]; then
    echo "File is readable."
else
    echo "File is not readable."
fi

if [ -w "$filename" ]; then
    echo "File is writable."
else
    echo "File is not writable."
fi
```

- **Explanation**:
  - The `-r` operator checks if the file is readable.
  - The `-w` operator checks if the file is writable.

---

### **8.4 Combining File Operations with Control Structures**

You can combine file operations with control structures (like `if` statements and loops) to automate tasks such as processing log files, filtering data, or creating backups.

#### **Example: Processing Log Files with If-Else**

```bash
#!/bin/bash

logfile="system.log"
keyword="ERROR"

if grep -q "$keyword" "$logfile"; then
    echo "Errors found in log file."
else
    echo "No errors found."
fi
```

- **Explanation**:
  - The `grep` command searches for the word `ERROR` in `system.log`.
  - If `grep` finds the keyword, it returns a success exit status (0), and the script prints `"Errors found in log file."`
  - If not, it prints `"No errors found."`

---

### **8.5 Redirecting Output to Files**

Output redirection is commonly used to store results of commands, error logs, or script outputs.

#### **Example: Redirect Output and Errors to a File**

```bash
#!/bin/bash

# Redirect standard output (stdout) and error (stderr) to a file
ls /nonexistentdir > output.txt 2>&1
```

- **Explanation**:
  - The `>` operator writes standard output to `output.txt`.
  - `2>&1` ensures that both standard error (`2`) and standard output (`1`) are redirected to the same file.

---

### **Conclusion**

File operations are crucial for automating file management tasks in shell scripts. Key operations include:

- **Reading from files** using loops.
- **Writing to files** using output redirection (`>`, `>>`).
- **Checking file existence and permissions** with file test operators (`-f`, `-d`, `-r`, etc.).
- Combining these operations with control structures enables powerful automation tasks such as log processing, file management, and configuration updates.

By mastering file operations in shell scripts, you can handle a wide variety of administrative tasks in RHEL efficiently.
