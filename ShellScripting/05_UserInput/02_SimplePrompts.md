✅ **Simple Prompts in Shell Scripts**

Simple prompts allow your script to **ask the user for input** in a clean, interactive way. They usually combine `echo` and `read` or just `read -p`.

---

### 🧩 **1. Using `echo` + `read`**

The most basic way to prompt the user:

```bash
echo "Enter your name:"
read name
echo "Hello, $name!"
```

**Sample Run:**

```
Enter your name:
Alex
Hello, Alex!
```

---

### 🧠 **2. Using `read -p` (Inline Prompt)**

`-p` allows displaying the prompt **on the same line** as input:

```bash
read -p "Enter your favorite color: " color
echo "Your favorite color is $color."
```

**Output:**

```
Enter your favorite color: Blue
Your favorite color is Blue.
```

---

### 🧩 **3. Silent Prompts (for sensitive input)**

```bash
read -sp "Enter password: " password
echo
echo "Password accepted."
```

* `-s` → hides typed input
* The `echo` after ensures a newline for readability

---

### ⚙️ **4. Prompting with Default Values**

You can provide a **default value** if the user presses Enter without typing anything:

```bash
read -p "Enter your city (default: Delhi): " city
city=${city:-Delhi}
echo "City: $city"
```

**Behavior:**

* If user types `Mumbai` → output: `City: Mumbai`
* If user presses Enter → output: `City: Delhi`

---

### 🧾 **5. Combining Multiple Inputs in One Prompt**

```bash
read -p "Enter first and last name: " first last
echo "First: $first, Last: $last"
```

**Input Example:**

```
John Doe
```

**Output:**

```
First: John, Last: Doe
```

---

### 💡 **Best Practices for Simple Prompts**

1. Use **`read -p`** for clean, inline prompts.
2. Quote variables when printing (`"$name"`) to handle spaces.
3. Use **default values** to make scripts user-friendly.
4. For passwords or sensitive data, always use `-s`.

---


