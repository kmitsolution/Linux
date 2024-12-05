### **`awk` Command in Linux**

The `awk` command is a powerful text-processing tool in Linux and Unix-like systems. It is used for pattern scanning and processing, and it allows you to manipulate, transform, and extract text from files or streams based on patterns or conditions.

### **Basic Syntax:**
```bash
awk 'pattern { action }' input_file
```
- **`pattern`**: Specifies the pattern to search for.
- **`action`**: Defines what to do when the pattern is found. If no action is specified, `awk` prints the entire line.
- **`input_file`**: The file to read. If no file is provided, `awk` processes the input from standard input (stdin).

### **Commonly Used Features in `awk`:**

1. **Fields and Records**:
   - `awk` works with **records** and **fields**:
     - **Record**: By default, a record is a line of text.
     - **Field**: By default, fields are space or tab-separated words in a record.
     - `$1`, `$2`, `$3`, ... : Refer to the first, second, third, etc., field in a record.
     - `$0`: Represents the entire line (all fields).
   
2. **Patterns**:
   - `awk` applies the `action` to lines that match the `pattern`. If no `pattern` is specified, `awk` applies the `action` to all lines.

3. **Built-in Variables**:
   - **`$0`**: The entire line.
   - **`$1, $2, ...`**: Fields (columns) in the current line.
   - **`NF`**: The number of fields in the current record.
   - **`NR`**: The current record (line) number.
   - **`FS`**: Field separator (default is whitespace or tab).
   - **`OFS`**: Output field separator (default is a space).
   - **`RS`**: Record separator (default is newline).

### **Basic Examples of Using `awk`:**

#### 1. **Print Entire File:**
Print the entire content of the file using `awk`:
```bash
awk '{ print }' file.txt
```
This command prints every line in the file.

#### 2. **Print Specific Field(s):**
Print the first field (`$1`) from a file:
```bash
awk '{ print $1 }' file.txt
```
For a file with content like:
```
apple 3
banana 2
cherry 5
```
The output will be:
```
apple
banana
cherry
```

#### 3. **Print Multiple Fields:**
Print the first and second fields (`$1` and `$2`) from a file:
```bash
awk '{ print $1, $2 }' file.txt
```
Output:
```
apple 3
banana 2
cherry 5
```

#### 4. **Print Entire Line:**
To print the entire line, you can use `$0`:
```bash
awk '{ print $0 }' file.txt
```
This will output each entire line in `file.txt`.

#### 5. **Using Field Separator (FS) with Custom Delimiters:**
By default, `awk` uses **whitespace** (spaces or tabs) as the field separator, but you can specify other delimiters with the `-F` option.

**Example 1: CSV (Comma Separated) File:**
To print the first column from a CSV file (`file.csv`):
```bash
awk -F, '{ print $1 }' file.csv
```
If `file.csv` contains:
```
John,Doe,30
Jane,Smith,25
Alice,Brown,28
```
The output will be:
```
John
Jane
Alice
```

**Example 2: Tab-Separated File:**
For a tab-separated file, you can specify `\t` as the field separator:
```bash
awk -F'\t' '{ print $1 }' file.txt
```

#### 6. **Using `NR` and `NF`:**
- `NR`: Number of records (lines processed so far).
- `NF`: Number of fields in the current record.

**Example 1: Print Line Numbers and Fields:**
```bash
awk '{ print NR, $0 }' file.txt
```
This will print each line with its line number.

**Example 2: Print Lines with More Than Two Fields:**
```bash
awk 'NF > 2' file.txt
```
This will print lines that have more than two fields.

#### 7. **Using Conditions:**
You can use `awk` with conditions to match specific lines.

**Example: Print Lines Where a Field Matches a Value:**
```bash
awk '$3 > 30' file.txt
```
This will print lines where the third field is greater than 30.

For a file:
```
John 35 Engineer
Jane 25 Teacher
Alice 28 Doctor
```
The output will be:
```
John 35 Engineer
```

#### 8. **Perform Calculations:**
You can use `awk` to perform arithmetic calculations.

**Example: Sum of Numbers:**
```bash
awk '{ sum += $2 } END { print sum }' file.txt
```
This will sum all the values in the second field and print the result.

If `file.txt` contains:
```
apple 3
banana 2
cherry 5
```
The output will be:
```
10
```

#### 9. **Printing Specific Fields Based on Conditions:**
You can use conditions to print specific fields.

**Example: Print Names Where Age is Greater Than 30:**
```bash
awk '$2 > 30 { print $1 }' file.txt
```
For the file:
```
John 35
Jane 25
Alice 28
```
The output will be:
```
John
```

#### 10. **Formatted Output:**
You can control the formatting of the output using `printf`, just like in C.

**Example: Formatted Output of Fields:**
```bash
awk '{ printf "Name: %-10s Age: %-3s\n", $1, $2 }' file.txt
```
For `file.txt`:
```
John 35
Jane 25
```
The output will be:
```
Name: John       Age: 35
Name: Jane       Age: 25
```

### **Advanced `awk` Features:**

1. **Multiple Actions with `;`**:
   You can specify multiple actions in the `awk` command by separating them with a semicolon `;`.

   Example: Print first and second fields, and also print line numbers:
   ```bash
   awk '{ print $1, $2; print NR }' file.txt
   ```

2. **BEGIN and END Blocks**:
   - **BEGIN**: Executes actions before processing any input.
   - **END**: Executes actions after all input has been processed.

   Example: Print a header and footer:
   ```bash
   awk 'BEGIN { print "Header" } { print $1 } END { print "Footer" }' file.txt
   ```

   Output:
   ```
   Header
   apple
   banana
   cherry
   Footer
   ```

3. **Using `awk` in a Pipeline**:
   You can combine `awk` with other commands using pipes.

   Example: Use `ps` and `awk` to show processes in a specific user’s name:
   ```bash
   ps aux | awk '$1 == "user" { print $1, $3, $11 }'
   ```

### **Conclusion:**

The `awk` command is an incredibly versatile and powerful tool for processing text files. It can:
- Handle complex patterns.
- Work with fields and records.
- Perform calculations and text manipulations.
- Format and print data with ease.

By combining `awk` with other Linux utilities like `grep`, `sed`, and `cut`, you can create sophisticated text-processing workflows. 

Let me know if you need more detailed examples or clarifications!
