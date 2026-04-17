A **Named Pipe (FIFO)** is a special file that allows **communication between processes**.

---

## 🧠 What is a Named Pipe?

* It’s a file type (shown as `p`)
* Used for **one-way communication**
* Created using `mkfifo`

---

## 🔧 Example: Create a Named Pipe

```bash
mkfifo mypipe
```

Check it:

```bash
ls -l mypipe
```

Output:

```bash
prw-r--r-- 1 user user 0 Apr 17 10:00 mypipe
```

👉 `p` = named pipe

---

## 🔄 Example: Communication using Named Pipe

### 🟢 Terminal 1 (Reader)

```bash
cat < mypipe
```

---

### 🔵 Terminal 2 (Writer)

```bash
echo "Hello from pipe" > mypipe
```

👉 Output in Terminal 1:

```
Hello from pipe
```

---

## 🎯 Real-time Example

### Terminal 1:

```bash
cat mypipe
```

### Terminal 2:

```bash
echo "Linux IPC" > mypipe
```

👉 Message flows through the pipe

---

## ⚠️ Important behavior

* If no reader → writer waits
* If no writer → reader waits

---

## ❌ Remove the pipe

```bash
rm mypipe
```

---

## 📌 Summary

* Create → `mkfifo filename`
* Read → `cat filename`
* Write → `echo "msg" > filename`
* Used for IPC (Inter Process Communication)

