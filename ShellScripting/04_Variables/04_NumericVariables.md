
### 🧮 1. Assign and Use Numeric Variables in Bash (RHEL/Linux)

#### Example: Addition and Subtraction

```bash
#!/bin/bash

a=10
b=5

# Method 1: Using arithmetic expansion
sum=$((a + b))
diff=$((a - b))

echo "Sum: $sum"
echo "Difference: $diff"
```

**Output:**

```
Sum: 15
Difference: 5
```

---

### 🧩 2. Multiplication and Division

```bash
x=8
y=4

mul=$((x * y))
div=$((x / y))

echo "Multiply: $mul"
echo "Divide: $div"
```

---

### 💡 3. Using `expr` (older style, still works)

```bash
a=7
b=3

sum=$(expr $a + $b)
diff=$(expr $a - $b)

echo "Sum = $sum"
echo "Difference = $diff"
```

> ⚠️ Note: In `expr`, you need spaces between operators and variables.

---

### 🧠 4. Using `bc` for Floating-Point Math

Bash only supports **integers** in `$(( ))` — for decimals, use `bc`.

```bash
a=5.5
b=2.2

sum=$(echo "$a + $b" | bc)
diff=$(echo "$a - $b" | bc)
echo "Sum = $sum"
echo "Difference = $diff"
```

Output:

```
Sum = 7.7
Difference = 3.3
```

---

### 🏗️ 5. Example Script (RHEL-compatible)

Save as `math.sh`:

```bash
#!/bin/bash
read -p "Enter first number: " a
read -p "Enter second number: " b

echo "Sum: $((a + b))"
echo "Difference: $((a - b))"
echo "Product: $((a * b))"
echo "Quotient: $((a / b))"
```

Then run:

```bash
chmod +x math.sh
./math.sh
```

---


