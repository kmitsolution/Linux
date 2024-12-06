### **5. User Input in Shell Scripts**

User input is an essential feature in many shell scripts. It allows the script to interact with the user, making it more flexible and dynamic. In this section, we will explore how to read user input using the `read` command, and how to handle numeric input and perform arithmetic operations using `expr`.

---

#### **Reading User Input with `read`**

The `read` command in shell scripting is used to accept user input from the terminal. The input is then stored in a variable that can be used within the script.

- **Syntax**:
  ```bash
  read variable_name
  ```

- **Example**: Reading a user’s name and storing it in the variable `name`:
  ```bash
  echo "Enter your name:"
  read name
  echo "Hello, $name!"
  ```

  **Explanation**:
  - The script prompts the user with `Enter your name:`.
  - It waits for user input and stores the input in the `name` variable.
  - The script then greets the user with the message: `Hello, $name!`.

---

#### **Using Input in Scripts**

Once you've captured the user input, you can use it in your script as required, for example, in strings, conditions, or calculations.

**Example**: Using user input to display a personalized message.
```bash
#!/bin/bash
echo "Enter your name:"
read name
echo "Hello, $name! Welcome to the script."
```

---

#### **Reading Numeric Input and Performing Arithmetic with `expr`**

To read numeric input and perform arithmetic operations, you can use the `expr` command. This command allows you to perform basic arithmetic operations such as addition, subtraction, multiplication, and division.

- **Syntax for reading a number**:
  ```bash
  read variable_name
  ```

- **Example**: Reading two numbers from the user and calculating their sum:
  
  ```bash
  #!/bin/bash
  echo "Enter the first number:"
  read num1
  
  echo "Enter the second number:"
  read num2
  
  sum=$(expr $num1 + $num2)
  echo "The sum of $num1 and $num2 is: $sum"
  ```

  **Explanation**:
  - The script prompts the user to enter two numbers.
  - It reads the numbers and stores them in variables `num1` and `num2`.
  - `expr` is used to calculate the sum of `num1` and `num2`.
  - The result is stored in the `sum` variable and printed.

  **Output Example**:
  ```
  Enter the first number:
  5
  Enter the second number:
  3
  The sum of 5 and 3 is: 8
  ```

---

#### **Other Arithmetic Operations with `expr`**

Besides addition, `expr` supports other arithmetic operations:

- **Subtraction**:
  ```bash
  difference=$(expr $num1 - $num2)
  ```

- **Multiplication** (Note: Use `\*` to escape the `*` symbol):
  ```bash
  product=$(expr $num1 \* $num2)
  ```

- **Division**:
  ```bash
  quotient=$(expr $num1 / $num2)
  ```

- **Modulo (remainder)**:
  ```bash
  remainder=$(expr $num1 % $num2)
  ```

- **Example**: Perform multiple arithmetic operations:
  ```bash
  #!/bin/bash
  echo "Enter the first number:"
  read num1
  
  echo "Enter the second number:"
  read num2
  
  sum=$(expr $num1 + $num2)
  difference=$(expr $num1 - $num2)
  product=$(expr $num1 \* $num2)
  quotient=$(expr $num1 / $num2)
  remainder=$(expr $num1 % $num2)

  echo "Sum: $sum"
  echo "Difference: $difference"
  echo "Product: $product"
  echo "Quotient: $quotient"
  echo "Remainder: $remainder"
  ```

  **Output Example**:
  ```
  Enter the first number:
  10
  Enter the second number:
  3
  Sum: 13
  Difference: 7
  Product: 30
  Quotient: 3
  Remainder: 1
  ```

---

#### **Using Double Parentheses for Arithmetic**

In modern shell scripting, you can also use **double parentheses `(( ))`** for arithmetic operations, which makes the syntax cleaner and easier to understand than `expr`.

- **Example**: Using `(( ))` for arithmetic operations:
  
  ```bash
  #!/bin/bash
  echo "Enter the first number:"
  read num1
  
  echo "Enter the second number:"
  read num2
  
  sum=$((num1 + num2))
  difference=$((num1 - num2))
  product=$((num1 * num2))
  quotient=$((num1 / num2))
  remainder=$((num1 % num2))

  echo "Sum: $sum"
  echo "Difference: $difference"
  echo "Product: $product"
  echo "Quotient: $quotient"
  echo "Remainder: $remainder"
  ```

  **Explanation**:
  - The `(( ))` syntax is used for arithmetic operations. It's more intuitive and doesn’t require escaping operators like `*` for multiplication.
  - The result of the operation is directly stored in the variables (e.g., `sum`, `difference`).

---

### **Conclusion**

Handling user input in shell scripts is a key part of making them interactive and dynamic. Using the `read` command allows you to capture input from users, which can then be processed and utilized in various ways, including performing arithmetic operations. By combining user input with commands like `expr` or the more modern `(( ))` for arithmetic, you can create scripts that perform complex calculations or logic based on user input.
