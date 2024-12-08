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
```bash
   echo test > /dev/pts/1 | at now + 1 min
   at 09:15PM -f /tmp/script.sh > /dev/pts/1
```

### **8. Viewing and Managing `at` Jobs in Logs**

You can also check logs for scheduled jobs, especially if you're troubleshooting or verifying that jobs are being executed properly.

```bash
tail /var/log/cron
```

This shows logs of cron jobs and scheduled `at` jobs.

---
### **Additional Options for the `at` Command**

In addition to the basic usage of the `at` command, there are a few other important options that provide more control over job management, job contents, and job queues. Let's explore these options:

---

### **1. `at -q` Command**
The `at -q` option allows you to specify the **queue** in which the job will be placed. In Linux, `at` uses different queues to manage the execution of scheduled jobs. Each queue can have a different priority level, and jobs are executed in the order they are scheduled within each queue.

By default, `at` jobs are scheduled in the "a" queue, but you can specify a different queue if necessary.

#### **Syntax of `at -q`**:
```bash
at -q [queue] [time]
```

- **[queue]**: The queue to which you want to assign the job. The queue name can be any lowercase letter (a-z), and each letter represents a priority level.
- **[time]**: The time at which the job is to be executed.

#### **Example:**
To schedule a job at 5:00 PM today, but place it in queue `b`:

```bash
at -q b 5:00 PM
```

This job will be placed in the "b" queue and executed at 5:00 PM. If the "a" queue is already busy, the "b" queue may be processed next, depending on its priority configuration.

To see the current list of job queues, you can use the `atq` command.

---

### **2. `at -c` Command**

The `at -c` option allows you to **view the contents of a scheduled `at` job** by specifying the job ID. This command shows the commands that are scheduled to run when the job is executed.

#### **Syntax of `at -c`**:
```bash
at -c [job_id]
```

- **[job_id]**: The ID of the job that you want to view. You can get the job ID from the `atq` command.

#### **Example:**
To view the contents of job ID 1:

```bash
at -c 1
```

This will display the contents of the job with ID 1, showing the command(s) that are set to be executed.

---

### **3. `/var/spool/at` Directory**

The `/var/spool/at` directory is where the system stores the **queued jobs** that are scheduled with the `at` command. The jobs are stored in files inside this directory and are executed by the `atd` daemon. Each file in this directory corresponds to a pending job.

#### **Important Files in `/var/spool/at`**:

- **Pending jobs**: The jobs are stored as files with filenames corresponding to job IDs.
- **Job files**: These files contain the actual job (command) to be executed, along with scheduling information.

You can view the files in the `/var/spool/at` directory using the `ls` command:

```bash
ls /var/spool/at
```

To inspect the contents of a specific job file, you can open it using a text editor or the `cat` command:

```bash
cat /var/spool/at/job_id
```

Each file in `/var/spool/at` represents a scheduled job. However, it’s worth noting that you should avoid manually editing these files unless you know what you are doing, as it may disrupt the job execution.

---

### **4. Changing Commands in `at` Jobs**

Once an `at` job is scheduled, it is not designed for editing directly through the `at` command. If you need to change a scheduled job, the best approach is to **remove the current job** and **recreate it** with the new command or schedule.

To modify an existing job:

1. **List all scheduled jobs** using the `atq` command.
   
   ```bash
   atq
   ```

2. **Remove the existing job** using `atrm` and the job ID:
   
   ```bash
   atrm [job_id]
   ```

3. **Recreate the job** with the new command or schedule using `at`.

   ```bash
   at [new_time]
   ```

### **Examples of Using `at -q` and `at -c`**

#### **1. Using `at -q` to specify a queue**

To schedule a job to run at 10:00 PM today and place it in queue `c`, run:

```bash
at -q c 10:00 PM
```

This job will be added to queue `c` and will execute at 10:00 PM, depending on the priority of the queues.

#### **2. Using `at -c` to view the contents of a job**

Let's say you have a job with ID `3` that you want to inspect. To view its contents, run:

```bash
at -c 3
```

This will display the commands associated with job ID `3`, showing you exactly what is scheduled to run.

---

### **Summary of Commands**

1. **`at -q`**: Specify the queue for scheduling a job.
   - Example: `at -q b 5:00 PM`
   
2. **`at -c`**: View the contents of a scheduled job.
   - Example: `at -c 3`

3. **`/var/spool/at`**: Directory where `at` jobs are stored.
   - You can inspect jobs in this directory by listing or viewing specific job files.
   
4. **Changing a scheduled `at` job**: Remove the old job with `atrm` and reschedule it with the new command.

This should give you a deeper understanding of how to use `at` for scheduling tasks, specifying job queues, viewing job contents, and managing scheduled tasks in Linux.

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
