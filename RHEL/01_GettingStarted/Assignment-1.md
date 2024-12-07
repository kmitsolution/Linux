## Assignment-1

### RHEL Assignment: Shell Commands and Environment Variables

1. **Environment Variables:**
   - What does the `$SHELL` variable represent in a Linux system? How can you display the current shell being used in your terminal?

2. **Home Directory:**
   - What is the purpose of the `$HOME` environment variable in Linux? How can you navigate to your home directory using a command, and what command will display the path to your home directory?

3. **Locating Commands:**
   - Use the `whereis` command to find the location of the `ls` command binary, source, and manual pages. What is the full path of the `ls` command executable?

4. **User Information:**
   - What does the `whoami` command do? Use it to display your current username in the terminal.

5. **System Release Information:**
   - How can you check the version of Red Hat Enterprise Linux (RHEL) that you are using? Which command would you use to display the system's release information?

6. **Command History:**
   - List the last 10 commands you have executed using the `history` command. What command would you use to display the last 20 commands in your history?

7. **Running a Command from History:**
   - Use the `history` command and then run a specific command from your history using its corresponding number. For example, if command number 15 in your history is `ls -l`, how would you execute it again?

8. **Clearing the History:**
   - How can you clear your command history in Linux? What is the command to remove all commands from the history list?

9. **Changing the History Size:**
   - How would you change the number of commands stored in the history file? Write the command to change the `HISTSIZE` to 500.

10. **Using the `man` and `help` Commands:**
    - How can you access the manual page for the `ls` command? Additionally, use the `help` command to display the available options for the `cd` command. What is the difference between using `man` and `help` for finding information about commands?

---

### Expected Answers:

1. `$SHELL` is an environment variable that stores the path of the current shell. You can display it using the command `echo $SHELL`.
   
2. `$HOME` represents the user's home directory. Use `cd ~` to navigate to the home directory or `echo $HOME` to display it.
   
3. Run the command `whereis ls` to find the location of `ls` command binaries and other related files.
   
4. `whoami` shows the username of the currently logged-in user.
   
5. The command `cat /etc/redhat-release` or `hostnamectl` displays the RHEL version.
   
6. `history` lists the recent commands executed. Use `history 20` to display the last 20 commands.
   
7. To run a command from history, use `!<number>`, such as `!15` to run the command with history number 15.
   
8. Clear history with `history -c` to remove all commands from the current session’s history.
   
9. To change the history size, use `export HISTSIZE=500` or add it to the `.bashrc` file for persistence.
   
10. To view the manual page for `ls`, use `man ls`. The `help` command provides a simpler, shell-specific help, while `man` provides detailed documentation for most commands.

---

These questions cover fundamental shell command usage, environment variables, and history management within a Red Hat Enterprise Linux system.
