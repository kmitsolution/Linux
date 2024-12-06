### **7. Functions in Shell Scripts**

Functions in shell scripting allow you to group a set of commands into a reusable block of code. By defining functions, you can simplify your script, avoid repetition, and improve code organization.

---

### **7.1 Defining Functions**

A function is defined using the `function` keyword (optional in some shells, like Bash) or directly with the function name followed by parentheses. The code inside the function is executed whenever the function is called.

#### **Syntax:**

```bash
function function_name {
    # code
}
```

or

```bash
function_name() {
    # code
}
```

- The function is executed by simply calling its name.
- Functions can take arguments (parameters) that are passed when the function is called.

---

### **7.2 Example: Simple Function**

Let's define a simple function `greet` that accepts a name as a parameter and prints a greeting message.

```bash
#!/bin/bash

# Defining the function
greet() {
    echo "Hello, $1!"
}

# Calling the function with the argument "Alice"
greet "Alice"
```

#### **Explanation:**
- The `greet` function takes a single parameter `$1`, which represents the first argument passed to the function.
- The function prints "Hello, Alice!" when called with the argument "Alice".

**Output:**
```
Hello, Alice!
```

- Here, `$1` refers to the first argument passed to the function. You can pass different values to the function when calling it to get different outputs.

---

### **7.3 Functions with Multiple Arguments**

You can pass multiple arguments to a function. Each argument can be accessed inside the function using `$1`, `$2`, etc., and `$@` to refer to all arguments.

#### **Example: Function with Multiple Arguments**

```bash
#!/bin/bash

# Defining the function with multiple parameters
add_numbers() {
    sum=$(($1 + $2))
    echo "The sum of $1 and $2 is $sum."
}

# Calling the function with two arguments
add_numbers 5 10
```

**Explanation:**
- The function `add_numbers` takes two arguments (`$1` and `$2`), adds them, and prints the result.
- When called with `5` and `10`, it outputs:
  ```
  The sum of 5 and 10 is 15.
  ```

---

### **7.4 Using Return Values in Functions**

In shell scripting, functions can return a value using `return` for status codes or by printing the result. The value returned using `return` is an exit status (an integer between 0 and 255).

- **Return Value**: The return status can be captured using `$?`.
- **Output Value**: You can print the result directly from the function and capture it when calling the function.

#### **Example: Returning Status Code**

```bash
#!/bin/bash

# Function that returns a status code
check_file_exists() {
    if [ -f "$1" ]; then
        return 0  # success
    else
        return 1  # failure
    fi
}

# Calling the function and capturing the return value
check_file_exists "/path/to/file"
status=$?

if [ $status -eq 0 ]; then
    echo "File exists!"
else
    echo "File does not exist!"
fi
```

**Explanation:**
- The function `check_file_exists` checks whether the file exists.
- It returns `0` if the file exists (indicating success) or `1` if the file does not exist (failure).
- The return status is captured using `$?`, and the script prints a message based on whether the file exists.

**Output:**
```
File does not exist!
```

#### **Example: Returning a Value Using Output**

```bash
#!/bin/bash

# Function that adds two numbers and returns the result
add_numbers() {
    echo $(($1 + $2))
}

# Calling the function and capturing the result
result=$(add_numbers 7 8)
echo "The sum is $result."
```

**Explanation:**
- The function `add_numbers` calculates the sum of two numbers and uses `echo` to print the result.
- The result is captured using command substitution `$(...)` and stored in the `result` variable.
- The script then prints the sum of the two numbers.

**Output:**
```
The sum is 15.
```

---

### **7.5 Function Scope**

Variables inside a function are local to that function by default. If you want to modify a global variable within a function, you can declare it outside the function or pass it by reference.

#### **Example: Local Variable in a Function**

```bash
#!/bin/bash

# Global variable
var="Hello"

# Function that modifies the local variable
change_variable() {
    local var="Goodbye"
    echo "Inside function: $var"
}

change_variable

# Global variable remains unchanged
echo "Outside function: $var"
```

**Explanation:**
- The variable `var` is declared outside the function and is accessible globally.
- Inside the function `change_variable`, a local variable with the same name `var` is created using the `local` keyword, which prevents it from modifying the global `var`.
- The output shows that the global variable remains unchanged after the function executes.

**Output:**
```
Inside function: Goodbye
Outside function: Hello
```

---

### **7.6 Recursion in Shell Scripts**

A function can call itself, which is known as **recursion**. This is useful for tasks that require repetitive operations, such as calculating factorials or traversing directories.

#### **Example: Recursion to Calculate Factorial**

```bash
#!/bin/bash

# Recursive function to calculate factorial
factorial() {
    if [ $1 -le 1 ]; then
        echo 1
    else
        local result=$(($1 * $(factorial $(($1 - 1)))))
        echo $result
    fi
}

# Calling the recursive function
echo "Factorial of 5 is: $(factorial 5)"
```

**Explanation:**
- The `factorial` function calls itself until the base condition (`$1 -le 1`) is met.
- The function computes the factorial of the input number by multiplying it with the factorial of the number minus one.

**Output:**
```
Factorial of 5 is: 120
```

---

### **Conclusion**

Functions in shell scripts help you organize your code, avoid repetition, and make scripts more maintainable. By defining functions, you can:

- Group related commands together.
- Pass arguments to the function.
- Return values or status codes.
- Use recursion for tasks like calculating factorials.

Mastering functions in shell scripts is essential for writing efficient, modular, and scalable scripts.
