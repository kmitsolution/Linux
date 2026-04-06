Absolutely—let’s turn everything you wrote into a **clean, structured mini documentation + practical guide**, combining:

* `set`, `unset`
* `PS1`
* `set -e`, `-u`, `-x`
* Real script examples

---

# 🐧 Linux Shell Variables & `set` Command — Complete Guide

---

# 1. 🔧 `set` Command

The `set` command is used to:

* Display all shell variables and functions
* Set shell variables
* Enable/disable shell options

---

## 📌 Display all variables

```bash
set
```

👉 Shows:

* Shell variables
* Environment variables
* Functions

---

## 📌 Set a variable

```bash
myvar="Hello"
```

⚠️ Note:
Using `set myvar=Hello` is **not standard in Bash scripting**. Preferred way is:

```bash
myvar="Hello"
```

---

## 📌 Export to environment (child processes)

```bash
export myvar="Hello"
```

---

## ⚙️ `set` Options (Important)

| Option | Meaning                 |
| ------ | ----------------------- |
| `-e`   | Exit on error           |
| `-u`   | Error on unset variable |
| `-x`   | Debug (print commands)  |

---

# 2. ⚠️ `set -e` (Exit on Error)

### Without `-e`

```bash
#!/bin/bash

echo "Start"

cat missing.txt

echo "Still running"
```

### Output:

```
Start
cat: missing.txt: No such file
Still running
```

---

### With `-e`

```bash
#!/bin/bash
set -e

echo "Start"

cat missing.txt

echo "Will NOT run"
```

### Output:

```
Start
cat: missing.txt: No such file
```

👉 Script **stops immediately**

---

# 3. ⚠️ `set -u` (Unset Variable Error)

### Without `-u`

```bash
#!/bin/bash

echo "Value: $name"
```

Output:

```
Value:
```

---

### With `-u`

```bash
#!/bin/bash
set -u

echo "Value: $name"
```

Output:

```
unbound variable
```

👉 Prevents silent bugs

---

# 4. 🐞 `set -x` (Debug Mode)

```bash
#!/bin/bash
set -x

a=5
b=10
echo $((a + b))
```

### Output:

```
+ a=5
+ b=10
+ echo 15
15
```

👉 Shows execution trace (very useful for debugging)

---

# 5. 🚀 Combined Safe Mode

```bash
set -euo pipefail
```

👉 Best practice for production scripts

---

# 6. ❌ `unset` Command

Used to remove variables or functions.

---

## 📌 Remove a variable

```bash
myvar="Hello"
unset myvar

echo $myvar   # empty
```

---

## 📌 Remove a function

```bash
myfunc() {
  echo "Hi"
}

unset -f myfunc
```

---

# 7. 💻 `PS1` Variable (Shell Prompt)

`PS1` controls how your terminal prompt looks.

---

## 📌 Default Example

```bash
PS1='\u@\h \w\$ '
```

### Meaning:

| Symbol | Description       |
| ------ | ----------------- |
| `\u`   | username          |
| `\h`   | hostname          |
| `\w`   | current directory |
| `\$`   | `$` or `#`        |

---

## Example Output

```
user@localhost /home/user$
```

---

## 🎨 Custom PS1 Examples

### 1. Simple custom prompt

```bash
PS1='[\u@\h \w]\$ '
```

```
[user@localhost /home/user]$
```

---

### 2. Colored prompt

```bash
PS1='\[\e[32m\]\u@\h \w\[\e[0m\]\$ '
```

👉 Green colored prompt

---

### 3. Add time

```bash
PS1='\t \u@\h \w\$ '
```

```
15:30:10 user@host /dir$
```

---

### 4. Add date

```bash
PS1='\d \u@\h \w\$ '
```

---

## 📌 Make PS1 Permanent

```bash
echo 'PS1="\u@\h \w\$ "' >> ~/.bashrc
source ~/.bashrc
```

---

# 8. 🧪 Final Combined Script Example

This script demonstrates everything:

```bash
#!/bin/bash
set -eux

echo "Starting script"

# Variables
name="Linux"
echo "Hello $name"

# Debug example (-x shows this)
a=5
b=3
echo "Sum: $((a + b))"

# Unset example
temp="data"
unset temp

# set -u will fail here
echo "Temp: $temp"

# set -e example (this will also stop script)
cat missing_file.txt

echo "This will never run"
```

---

# 🧠 Final Summary

| Feature  | Purpose                      |
| -------- | ---------------------------- |
| `set`    | Show/set variables & options |
| `set -e` | Stop on errors               |
| `set -u` | Prevent undefined variables  |
| `set -x` | Debug execution              |
| `unset`  | Remove variables/functions   |
| `PS1`    | Customize shell prompt       |

---

# ✅ Pro Tip

For most scripts, always start with:

```bash
#!/bin/bash
set -euo pipefail
```

👉 Makes your script:

* safer
* predictable
* easier to debug

Great question—this is exactly where many people (even experienced engineers) get tripped up.
`set -e` looks simple, but its behavior has **non-obvious exceptions**.

Let’s go through the most important **interview edge cases** 👇

---

# ⚠️ 1. `set -e` DOES NOT trigger in `if` conditions

```bash
#!/bin/bash
set -e

if cat missing.txt; then
  echo "File exists"
fi

echo "Script continues"
```

### Output:

```
cat: missing.txt: No such file
Script continues
```

👉 ❗ Why?

* Commands inside `if` are **expected to fail**
* Bash uses exit status for condition checking

---

# ⚠️ 2. `set -e` is ignored in `||` (OR lists)

```bash
#!/bin/bash
set -e

cat missing.txt || echo "Handled error"

echo "Still running"
```

### Output:

```
cat: missing.txt: No such file
Handled error
Still running
```

👉 ❗ Why?

* Bash assumes you are **handling the failure explicitly**

---

# ⚠️ 3. `set -e` is ignored in `&&` chains (partially)

```bash
#!/bin/bash
set -e

false && echo "Won't run"

echo "Script continues"
```

👉 ❗ Why?

* `false` failing is part of logical evaluation
* Not treated as a fatal error

---

# ⚠️ 4. Pipelines ignore failures by default

```bash
#!/bin/bash
set -e

cat missing.txt | grep hello

echo "Still running"
```

### Output:

```
cat: missing.txt: No such file
Still running
```

👉 ❗ Why?

* Only the **last command (`grep`) exit status** is checked

---

## ✅ Fix with `pipefail`

```bash
set -euo pipefail
```

Now:

```bash
cat missing.txt | grep hello
```

👉 Script will **exit immediately**

---

# ⚠️ 5. Subshells can hide failures

```bash
#!/bin/bash
set -e

( cat missing.txt )

echo "Still running"
```

👉 In some cases, failures inside subshells don’t propagate as expected.

---

# ⚠️ 6. Command substitution `$()` ignores `set -e`

```bash
#!/bin/bash
set -e

output=$(cat missing.txt)

echo "Still running"
```

### Output:

```
cat: missing.txt: No such file
Still running
```

👉 ❗ Why?

* Failure happens in a **subshell**
* `set -e` does not propagate

---

## ✅ Safer alternative

```bash
set -euo pipefail

output=$(cat missing.txt) || exit 1
```

---

# ⚠️ 7. Functions + `set -e` surprises

```bash
#!/bin/bash
set -e

myfunc() {
  cat missing.txt
  echo "Inside function"
}

myfunc

echo "After function"
```

👉 Sometimes behaves unexpectedly depending on context (especially with conditionals).

---

# ⚠️ 8. Negation (`!`) disables exit

```bash
#!/bin/bash
set -e

! cat missing.txt

echo "Still running"
```

👉 ❗ Why?

* `!` flips exit status → failure becomes success

---

# ⚠️ 9. Loops may ignore failures

```bash
#!/bin/bash
set -e

while cat missing.txt; do
  echo "Loop"
done

echo "Still running"
```

👉 Again, treated like a conditional context.

---

# 🔥 Most Important Interview Summary

👉 `set -e` **does NOT exit when failure is expected or handled**

---

## ✅ It DOES exit in:

* Simple commands:

  ```bash
  cat file
  ```

---

## ❌ It DOES NOT exit in:

* `if`, `while`, `until`
* `&&`, `||`
* pipelines (unless `pipefail`)
* subshells `(...)`
* command substitution `$()`
* negation `!`

---

# 🧠 Golden Rule (Interview Answer)

👉 If they ask:

**"Is `set -e` reliable?"**

✔️ Best answer:

> "`set -e` is useful but not fully reliable because it is ignored in many contexts like conditionals, pipelines, and command substitutions. For robust scripts, it should be combined with `set -o pipefail` and explicit error handling."

---

# ✅ Best Practice (Production)

```bash
#!/bin/bash
set -euo pipefail
```

AND explicitly handle critical commands:

```bash
cat file || {
  echo "Error reading file"
  exit 1
}
```

---

# 🚀 Pro Tip (Senior-level answer)

Many teams avoid relying only on `set -e` and instead:

* use explicit error checks
* or trap errors:

```bash
trap 'echo "Error occurred"; exit 1' ERR
```

---

---

