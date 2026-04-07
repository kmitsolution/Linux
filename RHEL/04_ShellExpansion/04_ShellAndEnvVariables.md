In Linux and Unix-like systems, shell variables, environment variables, and profile files are used to configure the behavior of the shell and set the environment for programs. Here's an explanation of shell variables, how to set them permanently, the `env` command , the `SHLVL` command, and the `export` command with examples:

### 1. **Shell Variables**
A shell variable is a temporary variable used within a shell session. Shell variables are typically defined and used by the shell itself, and their scope is limited to the session in which they are defined.

#### Example:
```bash
myvar="Hello"
echo $myvar
```
Output:
```
Hello
```
- In this example, `myvar` is a shell variable, and its value (`Hello`) is echoed to the terminal.
#### env varaibles

### 2. **Setting Shell or env Variables Permanently in Profile Files**
To set shell variables permanently, you need to add them to profile configuration files that are executed every time a new shell session starts. Common files used for this purpose are `~/.bashrc`, `~/.bash_profile`, `~/.profile` (for interactive shells), and `~/.bash_profile` (for login shells).

#### Steps:
1. Open the appropriate profile file for editing. For example, for a Bash shell, use `~/.bashrc`:
   ```bash
   nano ~/.bashrc
   ```

2. Add the variable definition at the end of the file- it will create the env variable:
   ```bash
   export MYVAR="Hello"
   ```

3. Save the file and exit (`Ctrl + O`, `Enter`, then `Ctrl + X` in `nano`).

4. Apply the changes:
   ```bash
   source ~/.bashrc
   ```

Now, `MYVAR` will be available in all future shell sessions.

### 3. **`SHLVL` Command**
The `SLVL` (Shell Level) command shows the current shell's level. It is a built-in variable that tracks how many nested shells you have. A value of `1` indicates that you're in the first (outermost) shell, while higher numbers indicate deeper nested shells.

#### Example:
```bash
echo $SHLVL
```
Output:
```
1
```
- If you're running the first instance of the shell, `$SLVL` will be `1`. If you run another shell within the first one, the value will increment to `2`, and so on.

### 5. **`export` Command**
The `export` command is used to set environment variables that are inherited by child processes. When you use `export`, the variable is made available to any program or command that is executed after the variable is set.

#### Example:
```bash
MYVAR="Hello"
export MYVAR
echo $MYVAR
```
Output:
```
Hello
```
- The `export` command makes the variable `MYVAR` available to any subsequent commands, including scripts or programs.

You can also combine the `export` command with variable assignment in a single line:
```bash
export MYVAR="Hello"
```

### Example of Setting Variables Permanently and Using `export`:
1. **Define an environment variable permanently** in `~/.bashrc`:
   ```bash
   export PATH=$PATH:/new/directory
   ```

2. **Reload the shell configuration**:
   ```bash
   source ~/.bashrc
   ```

3. **Verify the change**:
   ```bash
   echo $PATH
   ```

Now, the new directory (`/new/directory`) will be permanently added to your `PATH` variable.

### Summary:
- **Shell Variables**: Temporary variables, used only within the current shell session.
- **Permanent Variables**: To make shell variables permanent, define them in shell configuration files (e.g., `~/.bashrc`) and use `export` to ensure they are available to child processes.
- **`env -i`**: Run commands with a clean environment, removing all inherited environment variables.
- **`SLVL`**: A built-in variable that shows the current shell nesting level.
- **`export`**: Makes shell variables available to child processes, effectively turning them into environment variables.

