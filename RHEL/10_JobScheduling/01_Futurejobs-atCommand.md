### **The `at` Command in Linux**

The `at` command is used to schedule a one-time job to run at a specific time in the future. It allows users to specify the exact time when a command or script should be executed. Once the job is completed, it will not run again.

The `at` command is typically used for scheduling tasks that need to run once at a specific time rather than recurring jobs (for which `cron` is usually used).

### **1. Installing and Setting Up `at`**

First, ensure that the `at` command is installed on your system:

```bash
yum install at
```

Check if `at` is installed:

```bash
yum list installed at
```

Verify that the `atd` service (which is responsible for running `at` jobs) is running:

```bash
systemctl status atd.service
```

If it's not running, start it:

```bash
systemctl start atd.service
```

You can enable `atd` to start on boot:

```bash
systemctl enable atd.service
```

### **2. Syntax of `at` Command**

```bash
at [time] [options]
```

Where `time` is the time you want the job to run, and `options` are additional flags for controlling job execution.

### **3. Example Usage of `at`**

- **Run a job at a specific time (e.g., 8:08 AM):**

```bash
at 8:08
```

This opens an interactive prompt where you can enter the command(s) you want to run at 8:08. After entering the command, press `Ctrl+D` to save and exit.

Example:

```bash
at 8:08
> echo "Hello, World!" > /tmp/hello.txt
> <Ctrl+D>
```

This will run the `echo` command at 8:08 AM and write "Hello, World!" to `/tmp/hello.txt`.

- **Run a job in the future (e.g., 2 minutes from now):**

```bash
at now + 2 minutes
```

This schedules a job to run 2 minutes from the current time.

- **Run a job at a specific time (e.g., tomorrow at 9:10 AM):**

```bash
at 9:10 AM tomorrow
```

This schedules a job to run at 9:10 AM on the next day.

- **Run a job at a specific time (e.g., 9:10 PM tomorrow):**

```bash
at 9:10 PM tomorrow
```

- **Use a specific date, time, and year:**

You can also specify a specific day of the month, month, and year.

```bash
at 10:00 AM 12/25/2024
```

This runs the job on December 25, 2024, at 10:00 AM.

---

### **4. Managing `at` Jobs**

- **List the pending `at` jobs:**

To view the list of pending jobs, use the `atq` command. This will show the jobs that are scheduled to run:

```bash
atq
```

Example output:

```
1   2024-12-08 08:08 a user
2   2024-12-08 09:10 a user
```

- **Remove a job from the queue:**

To remove a job, use the `atrm` command followed by the job ID (which you can get from `atq`):

```bash
atrm 1
```

This will remove job ID 1 from the queue.

- **List the scheduled jobs with `at -l`:**

```bash
at -l
```

This is similar to `atq` and shows a list of scheduled jobs.

---

### **5. Other Time Formats**

The `at` command supports various ways of specifying the time when the job should be executed.

- **Time (e.g., 9:10 PM tomorrow)**: Specifies the time for the job to run tomorrow at 9:10 PM.
  
  ```bash
  at 9:10 PM tomorrow
  ```

- **Specific time (e.g., 6:28 AM):**

  ```bash
  at 06:28
  ```

- **Relative time (e.g., 2 minutes from now):**

  ```bash
  at now + 2 minutes
  ```

- **Specific month, day, and year (e.g., 10:00 AM on December 25, 2024):**

  ```bash
  at 10:00 AM 12/25/2024
  ```

- **Relative time (e.g., 1 minute from now):**

  ```bash
  at now + 1 minute
  ```

---

### **6. Restricting Access to `at`**

You can control who is allowed to schedule `at` jobs by editing the `/etc/at.deny` file. This file lists users who are not allowed to use `at`. If a user is in this file, they will not be able to schedule jobs with `at`.

- **Edit `/etc/at.deny` to restrict access:**

```bash
vi /etc/at.deny
```

Add the username(s) of users you want to deny access to `at` commands. For example:

```
user1
user2
```

After saving and exiting, these users will not be able to create `at` jobs.

- **Alternatively, users allowed to use `at` can be listed in `/etc/at.allow`. If this file exists and the user is not listed, they are denied access.**

```bash
vi /etc/at.allow
```

This will allow only the listed users to run `at` commands.

### **7. Examples of `at` with More Advanced Scheduling**

- **Running a job in the future (e.g., 3:00 PM next Friday):**

```bash
at 3:00 PM Fri
```

- **Running a job 2 hours from now:**

```bash
at now + 2 hours
```

- **Running a job on the first day of the month (e.g., 1st of every month at 10:00 AM):**

```bash
at 10:00 AM 1
```

This will run the job at 10:00 AM on the 1st of the current month.

---

### **8. Viewing and Managing `at` Jobs in Logs**

You can also check logs for scheduled jobs, especially if you're troubleshooting or verifying that jobs are being executed properly.

```bash
tail /var/log/cron
```

This shows logs of cron jobs and scheduled `at` jobs.

---

### **Summary of Important `at` Commands**

- **Install `at`**: `yum install at`
- **Check if `atd` service is running**: `systemctl status atd.service`
- **Start `atd` service**: `systemctl start atd.service`
- **Schedule a job**: `at 9:10 PM tomorrow`
- **List scheduled jobs**: `atq`
- **Remove a job**: `atrm <job_id>`
- **Show all scheduled jobs**: `at -l`
- **Restrict user access**: Edit `/etc/at.deny` and `/etc/at.allow`
- **Examples of time formats**:
  - `at now + 5 minutes`
  - `at 10:00 PM 12/25/2024`
  - `at 9:00 AM tomorrow`

The `at` command is a powerful tool for scheduling one-time tasks in the future. Understanding how to use time formats and managing the `at` service allows you to automate tasks for a specific time, improving system management and task scheduling efficiency.
