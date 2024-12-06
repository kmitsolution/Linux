
1. **`whoami`**:
   - **Description**: Displays the username of the currently logged-in user.
   - **Example**: If you run `whoami` as the root user, it will output `root`.
   - **Usage**: 
     ```bash
     whoami
     ```

2. **`who`**:
   - **Description**: Displays a list of users currently logged into the system, along with their login time, terminal, and IP address (if applicable).
   - **Example**: 
     ```bash
     who
     ```
   - **Output**:
     ```
     user1   tty7         2024-12-06 08:00 (:0)
     user2   pts/1        2024-12-06 09:30 (:0)
     ```

3. **`w`**:
   - **Description**: Displays information about the users who are currently logged in and their activity. It includes the username, terminal, login time, idle time, and the command being executed.
   - **Example**:
     ```bash
     w
     ```
   - **Output**:
     ```
     10:15:40 up 10 days,  4:32,  2 users,  load average: 0.04, 0.05, 0.01
     USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU  WHAT
     user1    tty7     :0               08:00   1.00s  0.10s  0.00s  /usr/bin/gnome-shell
     user2    pts/1    :0               09:30   3.00s  0.20s  0.10s  w
     ```

4. **`id`**:
   - **Description**: Displays the user and group ID (UID and GID) of the current user, as well as the group memberships.
   - **Example**:
     ```bash
     id
     ```
   - **Output**:
     ```
     uid=1000(user1) gid=1000(user1) groups=1000(user1),10(wheel)
     ```

5. **`id username`**:
   - **Description**: Displays the user and group ID (UID and GID) for a specific user, as well as their group memberships.
   - **Example**:
     ```bash
     id user2
     ```
   - **Output**:
     ```
     uid=1001(user2) gid=1001(user2) groups=1001(user2),10(wheel)
     ```

6. **`id root`**:
   - **Description**: Displays the user and group ID (UID and GID) for the root user, as well as the root's group memberships.
   - **Example**:
     ```bash
     id root
     ```
   - **Output**:
     ```
     uid=0(root) gid=0(root) groups=0(root)
     ```

7. **`su username`**:
   - **Description**: Switches the user to another username (if you have the necessary permissions). By default, if no username is specified, `su` switches to the root user. To switch to another user, use `su` followed by the username.
   - **Example**:
     ```bash
     su user2
     ```
   - **Output**:
     ```
     Password: (enter user2's password)
     ```
   - After successful authentication, you'll be switched to that user's environment.

### Summary of Commands:
- `whoami`: Shows current logged-in username.
- `who`: Lists all logged-in users.
- `w`: Lists logged-in users and their activities.
- `id`: Shows UID, GID, and group memberships for the current user.
- `id username`: Shows UID, GID, and group memberships for a specific user.
- `id root`: Shows UID, GID, and group memberships for root.
- `su username`: Switches to another user, given you know their password.

These commands help in managing and monitoring users on a RHEL system.
