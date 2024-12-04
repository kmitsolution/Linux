In Linux and Unix-like systems, shell variables, environment variables, and profile files are used to configure the behavior of the shell and set the environment for programs. Here's an explanation of shell variables, how to set them permanently, the `env` command with the `-i` option, the `SLVL` command, and the `export` command with examples:

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

### 2. **Setting Shell Variables Permanently in Profile Files**
To set shell variables permanently, you need to add them to profile configuration files that are executed every time a new shell session starts. Common files used for this purpose are `~/.bashrc`, `~/.bash_profile`, `~/.profile` (for interactive shells), and `~/.bash_profile` (for login shells).

#### Steps:
1. Open the appropriate profile file for editing. For example, for a Bash shell, use `~/.bashrc`:
   ```bash
   nano ~/.bashrc
   ```

2. Add the variable definition at the end of the file:
   ```bash
   export MYVAR="Hello"
   ```

3. Save the file and exit (`Ctrl + O`, `Enter`, then `Ctrl + X` in `nano`).

4. Apply the changes:
   ```bash
   source ~/.bashrc
   ```

Now, `MYVAR` will be available in all future shell sessions.

### 3. **`env` Command with the `-i` Option**
The `env` command is used to display or modify the environment for a command. The `-i` option allows you to run a command with a clean environment (i.e., without any inherited environment variables).

#### Example:
```bash
env -i myvar="Hello" bash
```
- This command runs a new `bash` shell with the variable `myvar` set to `"Hello"`, but without any of the environment variables from the parent shell.

You can verify the environment inside the new shell by running:
```bash
echo $myvar
```
Output:
```
Hello
```
- In this case, only the variable `myvar` is available in the environment, and the other environment variables are cleared because of the `-i` option.

### 4. **`SLVL` Command**
The `SLVL` (Shell Level) command shows the current shell's level. It is a built-in variable that tracks how many nested shells you have. A value of `1` indicates that you're in the first (outermost) shell, while higher numbers indicate deeper nested shells.

#### Example:
```bash
echo $SLVL
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

The `set` and `unset` commands in Linux are used to manage shell variables and environment variables. The `PS1` variable is an important environment variable that controls the appearance of the shell prompt. Let's explore these commands and the `PS1` variable in more detail.

### 1. **`set` Command**
The `set` command is used to display or set shell variables, environment variables, and shell options.

#### Usage:
- **Display all variables**:
  ```bash
  set
  ```
  This will display all shell variables (including environment variables) and shell functions for the current shell session.

- **Set a shell variable**:
  ```bash
  set variable_name=value
  ```
  Example:
  ```bash
  set myvar=Hello
  ```

  After this, the variable `myvar` will be set to "Hello". However, `set` alone does not export the variable to child processes, so it remains local to the shell session.

#### `set` for Shell Options:
The `set` command can also be used to set shell options. For example:
- **Enable debugging**:
  ```bash
  set -x
  ```
  This enables debugging mode and prints each command as it is executed.

- **Disable debugging**:
  ```bash
  set +x
  ```

### 2. **`unset` Command**
The `unset` command is used to remove shell variables, environment variables, or shell functions.

#### Usage:
- **Unset a variable**:
  ```bash
  unset variable_name
  ```
  Example:
  ```bash
  unset myvar
  ```

  This removes the variable `myvar` from the current shell session.

- **Unset a function**:
  ```bash
  unset -f function_name
  ```

  For example, if you created a function `myfunc`, you can unset it using `unset -f myfunc`.

### 3. **The `PS1` Variable**
The `PS1` variable is used to define the appearance of the shell prompt in Bash. It controls what the prompt looks like and can include dynamic information, such as the current user, hostname, directory, and more.

#### Default `PS1` Example:
The default value of `PS1` in many Linux distributions looks like this:
```bash
PS1='\u@\h \w\$ '
```
This prompt includes:
- `\u`: the current username.
- `\h`: the hostname (up to the first `.`).
- `\w`: the current working directory.
- `\$`: displays `#` for root or `$` for non-root users.

For example, if the username is `user`, the hostname is `localhost`, and the current directory is `/home/user`, the prompt will look like:
```
user@localhost /home/user$
```

#### Customizing the `PS1` Prompt:
You can modify `PS1` to change the prompt to suit your needs. Here are a few examples of customizing the `PS1` variable:

- **Show current directory with color**:
  ```bash
  PS1='\[\e[32m\]\w\[\e[m\]$ '
  ```
  This will show the current directory in green.

- **Show the current time in the prompt**:
  ```bash
  PS1='\t \u@\h \w\$ '
  ```
  This will include the current time (e.g., `15:24:12`) in the prompt.

- **Add the date to the prompt**:
  ```bash
  PS1='\d \u@\h \w\$ '
  ```
  This will show the date (e.g., `Tue Dec  4`) in the prompt.

- **Add the Git branch (if in a Git repo)**:
  ```bash
  PS1='\u@\h \w$(__git_ps1) \$ '
  ```
  This will append the current Git branch name to the prompt (you'll need `git` for this to work).

#### Example of Modifying `PS1`:
```bash
PS1='[\u@\h \w]\$ '
```
This will display a prompt like:
```
[user@localhost /home/user]$
```

To make these changes permanent, you can add the desired `PS1` setting to the `~/.bashrc` file. For example, to set a custom prompt, you can add this line to `~/.bashrc`:
```bash
echo 'PS1="[\u@\h \w]\$ "' >> ~/.bashrc
source ~/.bashrc
```

### Summary:

- **`set` Command**:
  - Displays all shell variables and functions.
  - Used to set variables, including shell options (`set -x` for debugging).
  
- **`unset` Command**:
  - Removes shell variables, environment variables, or functions.
  
- **`PS1` Variable**:
  - Defines the appearance of the shell prompt.
  - Can be customized to show information like username, hostname, directory, time, and more.
  - Changes to `PS1` can be made permanent by adding them to the `~/.bashrc` file.

These commands and the `PS1` variable allow you to configure the shell environment and customize the shell prompt to your liking.
