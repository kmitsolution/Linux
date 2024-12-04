### 1. **`type` Command**
The `type` command in Linux is used to determine whether a command is a built-in shell command, an external command (program or executable), or an alias. It provides information about how a command is implemented.

#### Basic Syntax:
```bash
type <command>
```

#### Examples:

- **Check if a command is built-in or external**:
   ```bash
   type ls
   ```
   Output:
   ```
   ls is /bin/ls
   ```
   This tells you that `ls` is an external command located in `/bin/ls`.

- **Check if a command is an alias**:
   ```bash
   type ll
   ```
   If you have an alias `ll` for `ls -la`, it might output:
   ```
   ll is aliased to `ls -la`
   ```

- **Check if a command is a shell built-in**:
   ```bash
   type cd
   ```
   Output:
   ```
   cd is a shell builtin
   ```
   This indicates that `cd` is a built-in command of the shell.

- **For a command that is not found**:
   ```bash
   type unknowncommand
   ```
   Output:
   ```
   unknowncommand not found
   ```

### 2. **The `$SHELL` Variable**
The `$SHELL` environment variable holds the path to the current shell being used by the user. It indicates which shell is running in the current session (e.g., Bash, Zsh, etc.).

#### To check the current shell:
```bash
echo $SHELL
```

#### Example Output:
- If you are using Bash:
  ```bash
  /bin/bash
  ```
- If you are using Zsh:
  ```bash
  /bin/zsh
  ```

### Summary:
- The `type` command helps you determine if a command is a shell built-in, an external program, or an alias.
- The `$SHELL` variable tells you the path of the current shell being used in your session.
