# Create Zombie Process using C language in RHEL
To ensure we specifically create a **zombie process**, we need to do the following:

1. **Create a child process**.
2. **Exit the child process**.
3. **Ensure the parent does not wait for the child** so the child process will remain in the zombie state until the parent collects the exit status.

Here's the corrected approach to create a zombie process in Linux:

### Step-by-step Instructions:

1. **Write a C program** that creates a child process but does not immediately wait for the child process to finish.

```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <unistd.h>

int main() {
    pid_t pid = fork();  // Create a child process

    if (pid < 0) {
        // Error in fork()
        perror("Fork failed");
        exit(1);
    }

    if (pid == 0) {
        // Child process
        printf("Child process (PID: %d) is terminating...\n", getpid());
        exit(0);  // Child process exits
    } else {
        // Parent process
        printf("Parent process (PID: %d), child PID: %d\n", getpid(), pid);
        sleep(30);  // Parent process sleeps for 30 seconds
        printf("Parent process (PID: %d) is done.\n", getpid());
    }

    return 0;
}
```

### Explanation of the Code:
- The parent process creates a child process using `fork()`.
- The **child process** immediately terminates (`exit(0)`), but the **parent process** doesn't wait for it (`wait()` is not called), so the child remains in the **zombie state** until the parent finishes sleeping.
- After the sleep, the parent process terminates, and the zombie process is cleaned up by the operating system.

### How to Compile and Run:

1. **Compile the program**:

   ```bash
   gcc -o zombie_process zombie_process.c
   ```

2. **Run the program**:

   ```bash
   ./zombie_process
   ```

3. **Check the zombie process**:

   While the parent process is sleeping, check for the zombie process by running:

   ```bash
   ps aux | grep Z
   ```

   You should see something like:

   ```bash
   user      12345  0.0  0.1  12345  6789 ?        Z    00:00   0:00 [zombie_process] <defunct>
   ```

   This `<defunct>` indicates that the child process is a **zombie**.

### Why This Works:
- When the child process terminates, it becomes a zombie until its exit status is collected by the parent.
- The parent process is sleeping for 30 seconds and does not collect the exit status, so the child remains in the zombie state during that time.
- Once the parent terminates, the zombie process is cleaned up automatically.

