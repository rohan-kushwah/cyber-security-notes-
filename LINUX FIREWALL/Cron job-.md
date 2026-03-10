Cron-Jobs-in-Linux

# Cron Jobs in Linux

A cron job is a scheduled task in Linux or Unix-like systems that automates processes to run at specified intervals or times. It is managed by the `cron daemon`, which continuously runs in the background and checks for tasks defined in the crontab (cron table).

## Checking if Cron is Installed

Before using cron, ensure that it is installed on the system. Run the following commands to check for its installation:

```sh
rpm -qa | grep cron
```

```sh
rpm -qa | grep crontabs
```

If cron is not installed, install it using:

```sh
yum install crontabs
```

Verify the installation and list the installed files:

```sh
rpm -ql crontabs
```

For package information:

```sh
rpm -qi crontabs
```

## Understanding Cron Directories

Cron jobs are managed in specific system directories:

```sh
ls -l /etc/cron*
```

The output will list files and directories such as:

```sh
/etc/cron.deny
/etc/crontab
/etc/cron.d:
    0hourly
/etc/cron.daily:
    google-chrome
    google-chrome-beta
    google-chrome-unstable
/etc/cron.hourly:
    0anacron
/etc/cron.monthly:
/etc/cron.weekly:
```

To view or edit specific cron jobs, navigate to these directories:

- `cd /etc/cron.hourly`
- `cd /etc/cron.daily`
- `cd /etc/cron.weekly`
- `cd /etc/cron.monthly`

## Viewing and Editing Crontab

To list the configuration files for crontabs:

```sh
rpm -qc crontabs
```

To edit the system crontab file:

```sh
vim /etc/crontab
```

For manual reference:

```sh
man crontab
```

## System-Wide Cron Directories

Linux systems provide predefined directories for scheduling jobs at specific intervals without manually editing the crontab file. These directories are managed by `run-parts`, a command that executes all scripts inside a directory.

### 1. `/etc/cron.hourly`

- Contains scripts that run once every hour.
- Execution time is controlled by `/etc/crontab` or system cron settings.
- Example: A script to sync logs every hour.

```sh
#!/bin/bash
rsync -av /var/log/ /backup/logs/
```

Save this script as `/etc/cron.hourly/log_sync` and make it executable:

```sh
chmod +x /etc/cron.hourly/log_sync
```

### 2. `/etc/cron.daily`

- Contains scripts that run once per day.
- Default execution time is usually 3:00 AM but can vary by system.
- Example: A script to clean temporary files daily.

```sh
vim /etc/cron.daily/clean_tmp
```

```sh
#!/bin/bash
find /tmp -type f -mtime +7 -delete
```

Save this script as `/etc/cron.daily/clean_tmp` and make it executable:

```sh
chmod +x /etc/cron.daily/clean_tmp
```

### 3. `/etc/cron.weekly`

- Scripts in this directory run once per week.
- Default execution time is usually Sunday at 4:00 AM.
- Weekly tasks may include full system backups, package updates, or maintenance scripts.

### 4.   `/etc/cron.monthly`-
  1. scripts in this directory run once per a month
  2. default execution time is usually at month end at 5:00 AM
  ```
  vim /etc/cron.monthly/task
```

```
cat /etc/var/log/messages | grep -E "(root|rohan)"
```

```
chmod +x /etc/cron.monthly/task
```
## How to Manually Run These Scripts

To manually execute the scripts in these directories, use:

```sh
run-parts /etc/cron.daily
run-parts /etc/cron.weekly
run-parts /etc/cron.monthly
```

## Key Points to Remember

- The system runs scripts from these directories at predefined times.
- Scripts must be executable (`chmod +x script_name`).
- Scripts should not have file extensions (e.g., `.sh`).
- Check execution logs using:

```sh
cat /var/log/cron
```

- Modify the default execution times in `/etc/crontab` if needed.

## Working with Date and Time in Cron Jobs

To fetch the current date and time:

```sh
date
```

Formatting date output:

```sh
date "+%F"
date "+%F-%H-%M"
date "+%d-%m-%Y-%H-%M"
date "+%H-%M-%d-%m-%Y"
```

Creating files with timestamped names:

```sh
touch "$(date +%d-%m-%Y-%H-%M)"
```

```sh
tar -cvf /tmp/"$(date '+%d-%m-%Y-%H-%M')".tar /home/armour
```
