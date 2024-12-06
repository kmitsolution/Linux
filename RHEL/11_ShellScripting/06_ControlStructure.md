### **6. Control Structures in Shell Scripting**

Control structures, such as `if-else` statements, allow you to implement conditional logic in your shell scripts. They enable the script to make decisions based on certain conditions, improving the flexibility and functionality of the script. This section focuses on the `if-else` statement, and we will also look at an example where administrative privileges are checked before performing a sensitive action.

---

#### **If-Else Statements**

An `if-else` statement allows the script to execute certain commands depending on whether a given condition is true or false. 

- **Syntax**:

  ```bash
  if [ condition ]; then
      # code to run if the condition is true
  elif [ condition ]; then
      # code to run if the second condition is true
  else
      # code to run if no condition is true
  fi
  ```

- **Explanation**:
  - **`if [ condition ]; then`**: If the condition evaluates to true, the commands inside the `if` block are executed.
  - **`elif [ condition ]; then`**: Optionally, if the previous `if` or `elif` conditions are false, this condition is checked.
  - **`else`**: If none of the above conditions are true, the commands inside the `else` block are executed.
  - **`fi`**: The end of the `if-else` block.

---

### **Admin-Level Example: Checking for Sudo Access**

Let's create an example where the script checks if the user running the script has `sudo` (administrator) privileges. If the user has `sudo` privileges, they will be allowed to add a new user. If not, the script will print an error message and exit.

#### **Example Script: Check for Sudo Privileges**

```bash
#!/bin/bash

# Check if the user has sudo privileges
if sudo -l > /dev/null 2>&1; then
    echo "You have sudo privileges. Proceeding to add a new user."

    # Ask for the new user's name
    echo "Enter the username for the new user:"
    read new_user

    # Add the new user
    sudo useradd $new_user

    # Check if the user was created successfully
    if [ $? -eq 0 ]; then
        echo "User $new_user added successfully."
    else
        echo "Failed to add user $new_user."
    fi
else
    echo "You do not have sudo privileges. Exiting script."
    exit 1
fi
```

---

#### **Explanation of the Example:**

1. **Check for sudo privileges**: 
   - The script checks if the user running the script has `sudo` privileges by using `sudo -l`, which lists the allowed commands for the user. If the command succeeds (i.e., the user has sudo access), the script proceeds. Otherwise, it prints an error message and exits.

2. **Adding a new user**:
   - If the user has `sudo` privileges, the script prompts them to enter a username for the new user.
   - The `useradd` command is then used to create the new user. The script checks the exit status (`$?`) of `useradd` to confirm if the user was created successfully.

3. **Exit if no sudo privileges**:
   - If the user does not have `sudo` privileges, the script immediately exits with a status code of `1`.

---

#### **Breakdown of the `if-else` logic**:

1. **`if sudo -l > /dev/null 2>&1; then`**:
   - This command checks if the user has sudo privileges. The `> /dev/null 2>&1` part suppresses the output and error messages, ensuring only the exit status is used. If the command runs successfully (indicating that the user has `sudo` access), the script proceeds to add the user.

2. **`echo "You have sudo privileges. Proceeding to add a new user."`**:
   - If the user has `sudo` privileges, this message is displayed.

3. **`read new_user`**:
   - The script prompts the user for the new username.

4. **`sudo useradd $new_user`**:
   - The script attempts to add the user using the `useradd` command, with `sudo` for administrative access.

5. **`if [ $? -eq 0 ]; then`**:
   - This checks if the `useradd` command was successful by testing the exit status (`$?`). If the exit status is `0`, it means the user was created successfully.

6. **`else`**:
   - If the user does not have `sudo` privileges, the script prints an error message and exits with status code `1`.

---

### **Output Examples**

1. **Scenario 1**: User has sudo privileges:
   
   ```
   $ ./create_user.sh
   You have sudo privileges. Proceeding to add a new user.
   Enter the username for the new user:
   bob
   User bob added successfully.
   ```

2. **Scenario 2**: User does not have sudo privileges:
   
   ```
   $ ./create_user.sh
   You do not have sudo privileges. Exiting script.
   ```

---

### **Conclusion**

In this section, we've learned how to use `if-else` statements to make decisions based on certain conditions, such as checking for administrative privileges (`sudo`). This logic allows your scripts to be more dynamic and adaptive, enabling them to perform different actions depending on the context, such as checking if a user has sufficient privileges before attempting an administrative action.


### **7. Case Statements and Loops in Shell Scripting**

In shell scripting, **case statements** and **loops** are control structures that allow you to implement decision-making and repetitive tasks within a script. These structures make your scripts more powerful and flexible by enabling them to handle multiple conditions and execute tasks repeatedly.

---

### **7.1 Case Statements**

A `case` statement is used when you have multiple conditions to check, and each condition has a different set of actions to perform. It is more efficient than using multiple `if-else` statements when you have many possible options.

#### **Syntax of Case Statement:**
```bash
case $variable in
    pattern1)
        # Commands to execute if pattern1 matches
        ;;
    pattern2)
        # Commands to execute if pattern2 matches
        ;;
    *)
        # Default command if no pattern matches
        ;;
esac
```

- **Explanation**:
  - The `case` statement evaluates the value of the variable.
  - It checks for the first matching pattern.
  - The `;;` marks the end of a block of commands for a given pattern.
  - The `*` wildcard is used for the default case if no other pattern matches.
  - The statement is terminated with `esac`, which is `case` spelled backward.

#### **Example: Case Statement for Menu Selection**
Here's an example that uses a `case` statement to create a simple menu system for user input:

```bash
#!/bin/bash

echo "Select an option:"
echo "1) Create a new user"
echo "2) Show system info"
echo "3) Exit"
read choice

case $choice in
    1)
        echo "Enter the username:"
        read username
        sudo useradd $username
        echo "User $username created."
        ;;
    2)
        echo "System Info:"
        uname -a
        ;;
    3)
        echo "Exiting script."
        exit 0
        ;;
    *)
        echo "Invalid option. Please select 1, 2, or 3."
        ;;
esac
```

**Explanation**:
- The user is prompted to choose an option.
- Based on their selection, the `case` statement runs the corresponding block:
  - Option 1 creates a new user.
  - Option 2 displays system information.
  - Option 3 exits the script.
  - If the user enters an invalid choice, the default `*` case displays an error message.

---

### **7.2 Loops in Shell Scripting**

Loops are used to repeat a block of code multiple times. There are several types of loops in shell scripting: `for`, `while`, and `until`.

#### **For Loop**

A `for` loop is used when you know the number of iterations in advance, or when iterating over a list of items (such as files or users).

##### **Syntax of For Loop:**
```bash
for variable in list
do
    # Commands to execute for each item in the list
done
```

##### **Example: For Loop to Iterate Over a List of Users**
```bash
#!/bin/bash

for user in user1 user2 user3
do
    echo "Checking user: $user"
    id $user
done
```

**Explanation**:
- The `for` loop iterates over the list `user1`, `user2`, and `user3`.
- For each user, it executes the `id` command to display their user ID and group information.

#### **While Loop**

A `while` loop runs as long as a specific condition is true.

##### **Syntax of While Loop:**
```bash
while [ condition ]
do
    # Commands to execute as long as the condition is true
done
```

##### **Example: While Loop for Counting**
```bash
#!/bin/bash

count=1
while [ $count -le 5 ]
do
    echo "Count is: $count"
    ((count++))  # Increment the counter
done
```

**Explanation**:
- The `while` loop continues as long as the value of `count` is less than or equal to 5.
- The counter is incremented each time the loop runs.
- The output will be:
  ```
  Count is: 1
  Count is: 2
  Count is: 3
  Count is: 4
  Count is: 5
  ```

#### **Until Loop**

An `until` loop runs as long as the specified condition is false. It’s the opposite of a `while` loop.

##### **Syntax of Until Loop:**
```bash
until [ condition ]
do
    # Commands to execute until the condition becomes true
done
```

##### **Example: Until Loop to Count Down**
```bash
#!/bin/bash

count=5
until [ $count -lt 1 ]
do
    echo "Count is: $count"
    ((count--))  # Decrement the counter
done
```

**Explanation**:
- The `until` loop will execute until the condition `$count -lt 1` becomes true.
- The output will be:
  ```
  Count is: 5
  Count is: 4
  Count is: 3
  Count is: 2
  Count is: 1
  ```

---

### **7.3 Using Loops with Case Statements**

You can combine loops and `case` statements for more complex control flow. Here’s an example where we use a `for` loop to process multiple files and a `case` statement to check the file extension.

#### **Example: Process Files Based on Extension**

```bash
#!/bin/bash

for file in *.txt *.jpg
do
    case $file in
        *.txt)
            echo "Processing text file: $file"
            ;;
        *.jpg)
            echo "Processing image file: $file"
            ;;
        *)
            echo "Unknown file type: $file"
            ;;
    esac
done
```

**Explanation**:
- The `for` loop iterates over all `.txt` and `.jpg` files in the current directory.
- The `case` statement processes each file based on its extension, printing different messages for text files, image files, and unknown file types.

---

### **Conclusion**

Control structures such as **`case` statements** and **loops** (`for`, `while`, `until`) are fundamental in shell scripting, allowing for decision-making and repetitive tasks. 
- The **`case` statement** is useful for handling multiple options, providing a cleaner and more readable way to manage different conditions.
- **Loops** enable you to perform repetitive tasks, whether iterating over a list of items (`for` loop) or running commands as long as a condition holds true (`while` loop) or until a condition becomes true (`until` loop).
By combining these constructs, you can write more dynamic and efficient shell scripts in RHEL.
