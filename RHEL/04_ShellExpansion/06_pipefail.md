---

# 🔗 What is `pipefail`?

👉 `pipefail` is a shell option that changes how **pipelines (`|`) report errors**.

---

## 🧠 Default Behavior (WITHOUT `pipefail`)

In a pipeline:

```bash
command1 | command2 | command3
```

👉 The **exit status of the pipeline = exit status of the LAST command only**

---

### ❌ Example (Problem)

```bash
#!/bin/bash
set -e

cat missing.txt | grep hello | echo "grep command performed"

echo "Still running"
```

### What happens:

* `cat missing.txt` → ❌ fails
* `grep hello` → runs (gets no input, exits normally)
* ` echo command exexecuted successfully`

👉 Final exit status =  (success)

### Output:

```
cat: missing.txt: No such file
Still running
```

👉 ❗ Script **does NOT stop**, even though `cat` failed

---

# ✅ With `pipefail`

```bash
#!/bin/bash
set -euo pipefail

cat missing.txt | grep hello | echo "grep command performed"

echo "This will NOT run"
```

### What changes?

👉 Now:

* If **ANY command fails**, the pipeline fails

### Output:

```
cat: missing.txt: No such file
```

👉 Script **stops immediately**

---

# 🔍 How `pipefail` Works

👉 It returns the **exit code of the FIRST failed command** in the pipeline.

---

## Example Breakdown

```bash
false | true
```

### Without `pipefail`:

* `false` → fails
* `true` → succeeds
  👉 Result = success ❌ (wrong!)

---

### With `pipefail`:

```bash
set -o pipefail
```

👉 Result = failure ✅

---

# 🧪 Another Example

```bash
#!/bin/bash
set -o pipefail

echo "hello" | grep "bye" | wc -l
```

* `grep "bye"` → fails (no match)

👉 With `pipefail` → whole pipeline fails

---

# ⚙️ Syntax

```bash
set -o pipefail
```

OR combined:

```bash
set -euo pipefail
```

---

# 🔥 Why It Matters (Real-World)

Without `pipefail`, bugs can silently pass:

```bash
curl -s http://example.com/data | jq '.name'
```

👉 If `curl` fails:

* `jq` still runs
* Script may continue with bad data 😱

---

## ✅ Safe version

```bash
set -euo pipefail

curl -s http://example.com/data | jq '.name'
```

👉 Now script fails properly

---

# 🧠 Interview One-Liner

> "`pipefail` ensures that a pipeline fails if any command in it fails, not just the last one."

---

# ✅ Best Practice

Always use:

```bash
set -euo pipefail
```

---

# ⚠️ Quick Summary

| Without pipefail          | With pipefail              |
| ------------------------- | -------------------------- |
| Only last command matters | Any failure stops pipeline |
| Hidden bugs               | Safer scripts              |
| Not reliable              | Production-safe            |

---

