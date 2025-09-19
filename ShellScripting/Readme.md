# Shell Scripting Outline
### 1. Introduction to the Shell

* What is a shell?
* Bash vs other shells (sh, zsh, ksh)
* Shell in Ubuntu vs RHEL
* Terminal usage basics

### 2. Your First Shell Script

* Creating a `.sh` file
* Adding the shebang: `#!/bin/bash`
* Making scripts executable
* Running a script with `./script.sh` or `bash script.sh`

### 3. Basic Commands in Shell

* `echo`, `read`, `pwd`, `cd`, `ls`, `cp`, `mv`, `rm`, `mkdir`
* Redirection: `>`, `>>`, `<`
* Pipes: `|`

### 4. Variables and Quotes

* Defining and using variables
* Using `"$var"` vs `$var` vs `'`
* Environment vs user-defined variables

### 5. User Input

* `read` for input
* Simple prompts

### 6. Conditional Statements

* `if`, `else`, `elif`
* `test` and `[ ]` syntax
* Comparing strings, numbers, and files

### 7. Loops

* `for`, `while`, `until` loops
* Looping over files, arguments, numbers

### 8. Exit Status and `&&` / `||`

* Understanding `$?`
* Conditional execution

---

##  **Level 2: Intermediate – Real-World Scripting**

### 9. Functions

* Defining and calling functions
* Local vs global variables

### 10. Script Arguments

* `$1`, `$2`, `$@`, `$#`
* Argument validation and usage messages

### 11. Arrays

* One-dimensional arrays
* Iterating over arrays

### 12. File Operations

* Reading files line-by-line
* File tests: `-f`, `-d`, `-e`, etc.

### 13. Logging and Debugging

* Using `logger`
* Writing logs to files
* Using `set -x`, `set -e`, `trap`

### 14. Scheduling Scripts

* Using `cron` in Ubuntu
* Using `systemd timers` in RHEL 8+ (modern alternative)
* Creating cron jobs and understanding crontab format

### 15. User and Group Management

* Creating users and groups via script
* Checking for existing users
* Password management (Ubuntu vs RHEL differences)

### 16. Package Management via Script

* Ubuntu: `apt`, `dpkg`
* RHEL: `yum`, `dnf`, `rpm`
* Installing, removing, checking installed packages

---

## 🚀 **Level 3: Advanced – Power Scripting & Automation**

### 17. Regular Expressions and `grep`, `awk`, `sed`

* `grep` with regex
* `awk` for column processing
* `sed` for stream editing and in-place file edits

### 18. Advanced File Parsing and Manipulation

* Reading CSV/TSV files
* JSON parsing with `jq`
* XML parsing with `xmllint`

### 19. Networking and API Calls

* Using `curl` and `wget`
* Making REST API calls (GET/POST)
* Parsing API JSON responses

### 20. Automation Scripts

* Backup scripts (tar, rsync)
* Log rotation (manual and via `logrotate`)
* System monitoring scripts

### 21. Secure Scripting Practices

* Avoiding command injection
* Input sanitization
* File permission management
* Using `sudo` safely in scripts

### 22. System Administration via Scripts

* Service management: `systemctl` vs `service`
* Checking disk/memory/CPU usage
* Automating security updates

### 23. Advanced Error Handling

* Using `trap` for cleanup
* Retry logic and timeouts
* Logging failures with timestamps



---

## 🧪 **Level 4: Expert – Modular, Portable, and Production-Ready Scripts**

### 25. Modular Scripting

* Organizing large scripts into functions and files
* Sourcing other scripts (`source`, `.`)

### 26. Unit Testing Shell Scripts

* Using `bats-core` for testing
* Creating testable functions
* CI/CD compatibility

### 27. Portable Scripting

* Writing scripts that run on both Ubuntu and RHEL
* Checking OS type with `uname` and `/etc/os-release`
* Conditional logic for different OS/package managers

### 28. Using Git with Scripts

* Version controlling your scripts
* Best practices for collaboration

### 29. Real-world Projects

* Build a log analysis tool
* Build a system health dashboard
* Build a deployment automation script

