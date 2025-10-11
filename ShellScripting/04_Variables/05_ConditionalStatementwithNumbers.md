
## 🧮 1. Integer Math Recap

Arithmetic expansion:

```bash
a=10
b=3

sum=$((a + b))
diff=$((a - b))
mul=$((a * b))
div=$((a / b))   # Integer division only

echo "Sum: $sum, Diff: $diff, Mul: $mul, Div: $div"
```

Output:

```
Sum: 13, Diff: 7, Mul: 30, Div: 3
```

---

## 🔢 2. Floating-Point (Decimal) Math with `bc`

Bash itself **cannot handle decimals**, but the `bc` command (installed by default on RHEL) can.

```bash
a=10.5
b=3.2

sum=$(echo "$a + $b" | bc)
diff=$(echo "$a - $b" | bc)
mul=$(echo "$a * $b" | bc)
div=$(echo "scale=2; $a / $b" | bc)   # scale=2 → 2 decimal places

echo "Sum: $sum"
echo "Difference: $diff"
echo "Product: $mul"
echo "Quotient: $div"
```

Output:

```
Sum: 13.7
Difference: 7.3
Product: 33.6
Quotient: 3.28
```

---

## ⚖️ 3. Integer Comparisons in Bash

You can compare integers directly inside `[[ ... ]]` or `(( ... ))`.

### Using `[[ ]]` syntax:

```bash
a=5
b=10

if [[ $a -lt $b ]]; then
  echo "$a is less than $b"
fi
```

### Comparison operators for integers:

| Operator | Meaning            |
| -------- | ------------------ |
| `-eq`    | equal              |
| `-ne`    | not equal          |
| `-lt`    | less than          |
| `-le`    | less than or equal |
| `-gt`    | greater than       |
| `-ge`    | greater or equal   |

---

## 🧠 4. Floating-Point Comparisons Using `bc`

Bash cannot compare floats directly — use `bc` again:

```bash
x=4.5
y=4.50

if (( $(echo "$x == $y" | bc -l) )); then
  echo "Equal"
else
  echo "Not equal"
fi

if (( $(echo "$x < 5.0" | bc -l) )); then
  echo "$x is less than 5.0"
fi
```

---

## 🧰 5. Full Example Script (RHEL-Compatible)

```bash
#!/bin/bash
# math_compare.sh

read -p "Enter first number: " a
read -p "Enter second number: " b

# Basic math
sum=$(echo "$a + $b" | bc)
diff=$(echo "$a - $b" | bc)
mul=$(echo "$a * $b" | bc)
div=$(echo "scale=2; $a / $b" | bc)

echo "Sum: $sum"
echo "Difference: $diff"
echo "Product: $mul"
echo "Quotient: $div"

# Comparison
if (( $(echo "$a == $b" | bc -l) )); then
  echo "Both numbers are equal."
elif (( $(echo "$a > $b" | bc -l) )); then
  echo "$a is greater than $b."
else
  echo "$a is less than $b."
fi
```

Run it:

```bash
chmod +x math_compare.sh
./math_compare.sh
```

---

