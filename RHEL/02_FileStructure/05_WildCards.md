In Linux and RHEL, **wildcard patterns** are used in the command line to match filenames and paths that meet certain criteria. They allow you to perform operations on multiple files at once without needing to specify each file name individually. Wildcards are commonly used with commands like `ls`, `cp`, `mv`, `rm`, and others.

### **Types of Wildcards:**

1. **Asterisk `*` (Matches Any Number of Characters)**
   - The `*` wildcard matches any sequence of characters (including no characters at all).
   
   **Examples:**
   - `*.txt` - Matches all files that end with `.txt`.
     ```bash
     ls *.txt
     ```
     This will list all files with the `.txt` extension in the current directory.
   
   - `file*` - Matches all files starting with `file` (e.g., `file1.txt`, `file_example`, etc.).
     ```bash
     ls file*
     ```
   
   - `*example*` - Matches any file that contains the string `example` anywhere in its name.
     ```bash
     ls *example*
     ```

2. **Question Mark `?` (Matches Exactly One Character)**
   - The `?` wildcard matches exactly one character.
   
   **Examples:**
   - `file?.txt` - Matches files like `file1.txt`, `fileA.txt`, but not `file10.txt` or `file.txt`.
     ```bash
     ls file?.txt
     ```

   - `?.txt` - Matches any single character followed by `.txt`, such as `a.txt`, `b.txt`, etc.
     ```bash
     ls ?.txt
     ```

3. **Square Brackets `[]` (Matches Any One Character Inside the Brackets)**
   - The `[]` wildcard matches any one character from a set or range of characters.

   **Examples:**
   - `file[1-5].txt` - Matches files like `file1.txt`, `file2.txt`, ..., `file5.txt`.
     ```bash
     ls file[1-5].txt
     ```
   
   - `file[a-d].txt` - Matches files like `filea.txt`, `fileb.txt`, `filec.txt`, `filed.txt`.
     ```bash
     ls file[a-d].txt
     ```

   - `file[!1-5].txt` - Matches files like `filea.txt`, `filez.txt`, but not `file1.txt`, `file2.txt`, ..., `file5.txt` (negation with `!`).
     ```bash
     ls file[!1-5].txt
     ```

4. **Curly Braces `{}` (Matches Any One of the Strings in the Braces)**
   - The `{}` wildcard allows you to specify multiple alternatives to match.

   **Examples:**
   - `file{1,2,3}.txt` - Matches `file1.txt`, `file2.txt`, and `file3.txt`.
     ```bash
     ls file{1,2,3}.txt
     ```

   - `file{A,B,C}.txt` - Matches `fileA.txt`, `fileB.txt`, and `fileC.txt`.
     ```bash
     ls file{A,B,C}.txt
     ```

5. **Hyphen `-` (Range of Characters inside Square Brackets)**
   - The `-` character is used to define a range of characters inside square brackets.

   **Examples:**
   - `[a-z]` - Matches any lowercase letter (a to z).
     ```bash
     ls file[a-z].txt
     ```
     This will list `filea.txt`, `fileb.txt`, ..., `filez.txt`.

   - `[0-9]` - Matches any digit (0 through 9).
     ```bash
     ls file[0-9].txt
     ```
     This will list `file1.txt`, `file2.txt`, ..., `file9.txt`.

---

### **Examples of Wildcard Patterns in Use:**

1. **List all `.txt` files in the current directory**:
   ```bash
   ls *.txt
   ```

2. **List all files that start with `data` and end with `.csv`**:
   ```bash
   ls data*.csv
   ```

3. **List files that have exactly 3 characters followed by `.txt`**:
   ```bash
   ls ???.txt
   ```

4. **List files starting with `log` and having a single character after it, followed by `.txt`**:
   ```bash
   ls log?.txt
   ```

5. **List all files that have a digit between `1` and `5` in their name**:
   ```bash
   ls file[1-5].txt
   ```

6. **List files that either start with `file1`, `file2`, or `file3`**:
   ```bash
   ls file{1,2,3}.txt
   ```

7. **List all files that start with `report`, followed by any character, and then have `.pdf` as the extension**:
   ```bash
   ls report?.pdf
   ```

---

### **Advanced Usage of Wildcards:**

1. **Using wildcards with `cp`, `mv`, and `rm`**:
   - **Copy all `.txt` files to a different directory**:
     ```bash
     cp *.txt /path/to/destination/
     ```

   - **Move all files that start with `img` to a subdirectory**:
     ```bash
     mv img* /path/to/subdirectory/
     ```

   - **Remove all `.log` files**:
     ```bash
     rm *.log
     ```

   - **Remove all files starting with `temp` and having any two characters after it**:
     ```bash
     rm temp??*
     ```

---

### **Why Use Wildcards?**

Wildcards make it easier to:
- Perform bulk operations on multiple files, such as copying, renaming, or deleting them.
- Save time and effort, especially when working with a large number of files that follow certain naming conventions.
- Automate tasks in scripts or on the command line.

---

### **Important Notes:**

- Wildcards only work when the pattern matches filenames in the current directory or path.
- Be careful when using wildcards with commands like `rm`, as they can delete many files at once.

### **Summary of Wildcard Patterns:**

| Wildcard | Matches                                                   | Example Command              |
|----------|-----------------------------------------------------------|------------------------------|
| `*`      | Matches any number of characters                          | `ls *.txt`                   |
| `?`      | Matches exactly one character                             | `ls file?.txt`               |
| `[]`     | Matches any one character from a set or range of characters | `ls file[a-d].txt`           |
| `{}`     | Matches any one of the strings inside the braces          | `ls file{1,2,3}.txt`         |
| `-`      | Denotes a range of characters inside `[]`                 | `ls file[a-z].txt`           |

By using these wildcard patterns effectively, you can perform complex file operations quickly and efficiently.
