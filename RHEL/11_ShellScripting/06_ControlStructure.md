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
