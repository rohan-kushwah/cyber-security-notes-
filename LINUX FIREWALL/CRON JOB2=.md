## Cron Job Scheduling Syntax

A crontab entry follows this format:  
![Pasted image 20250305184611.png](app://713d0bc9a626dc627065168a7d196bd4d9c8/C:/linux%20basic%20commands/IMAGES/Pasted%20image%2020250305184611.png?1741180571277)

```
minute (0-59) called hourly
hour (0-23) called daily
day of month (1-31) called monthly
month (1-12 or jan-dec) called yearly
day of week (0-6 or sun-sat) called weekly
# * * * * * user command-to-execute
```

## Basic Crontab Syntax

The syntax of a cron job line in a crontab file must use the following format:

```
MIN HOUR DOM MON DOW CMD
```

The first five fields, each separated by a single space, represent time intervals: **MIN** for _minutes_, **HOUR** for _hours_, **DOM** for _day of the month_, **MON** for _month_, and **DOW** for _day of the week_. They tell Cron when to initiate the cron job.

These fields are followed by the command (**CMD**), which is usually a path to a script or a system command. For example, the following line prompts Cron to execute a script on January 1st at 9 AM:

```
0 9 1 1 * /path/to/your_script.sh
```

Each field has its own set of permissible values, which can be accompanied or swapped for a special character. The asterisk (**`*`**) operator in this example instructs Cron to execute the job regardless of which day of the week falls on January 1st.

### Cron Job Time Format

This table lists possible values for the time fields in a [cron expression](https://phoenixnap.com/kb/cron-expression):

|Field|Possible Values|Syntax|Description|
|---|---|---|---|
|**Minute Within the Hour (MIN)**|0 – 59|**7 * * * ***|The cron job is initiated every time the system clock shows 7 in the minute's position.|
|**Hour of the Day**  <br>**(HOUR)**|0 – 23|**0 7 * * ***|The cron job runs any time the system clock shows 7 AM (7 PM would be coded as 19).|
|**Day of the Month (DOM)**|1 – 31|**0 0 7 * ***|The day of the month is 7, which means that the job runs every 7th day of the month.|
|**Month of the Year (MON)**|1 - 12 or JAN - DEC|**0 0 0 7 ***|The numerical month is 7, which determines that the job runs only in July. The _Month_ field value can be a number or the month abbreviation.|
|**Day of the Week**  <br>**(DOW)**|0 - 7 or SUN - SAT|**0 0 * * 7**|7 in the current position means that the job would only run on Sundays. The _Day of the Week_ field can be a number or the day abbreviation.|

### Command to Execute

After defining the schedule, enter the absolute path to the script or executable command you want Cron to complete. For example, the following command tells Cron to execute the **`backup.sh`** script, located in the **`root`** directory, every day at midnight:

```
0 0 * * * /root/backup.sh
```

Users must ensure their scripts have appropriate execute permissions and that the Cron service can access and run scripts in the specified directories.

### Using Operators

Operators are special characters used instead of or in conjunction with allowed field values. They simplify and shorten cron expressions and are integral to almost all cron jobs. The most frequently used operators include:

- **An asterisk (*)** substitutes all possible values for a time field. For example, when used in the day field, it indicates that the task should be executed every day.
- **A comma (,)** is used to separate individual values within a field. For example, **`20,40`** in the minute field runs the task at 20 and 40 minutes past the hour.
- **A dash (-)** defines a range of values in a single field. Entering **`15-18`** in the minute field instructs Cron to run a task every minute between and including the 15th and 18th minute.
- **A forward slash (/)** divides a field value into increments. For example, **`*/30`** in the minute field means that the task is run every 30 minutes.

### Common Symbols in Crontab

|Symbol|Meaning|
|---|---|
|*|Any value|
|,|List separator (e.g., 1,5,10)|
|-|Range of values (e.g., 1-5)|
|/|Step values (e.g., */5 means every 5 units)|

### Example Cron Jobs

Run a script at 2:30 AM every day:

```sh
30 2 * * * /path/to/backup_script.sh
```

Clear the `/tmp/` folder every Sunday at 3:00 AM:

```sh
0 3 * * 0 rm -rf /tmp/*
```

Check disk usage every 10 minutes and log it:

```sh
*/10 * * * * df -h > /var/log/disk_usage.log
```

Run a PHP script on the 1st of every month at midnight:

```sh
0 0 1 * * php /path/to/script.php
```



```
for further example go on crontab guru
```


crontab me entry hogi sabki


list the current user crontab-
```
crontab -l
```

edit the crontab-
```
crontab -e
```

list another user crontab
```
crontab -l -u username
```

edit another user crontab -
```
cronatab -e  -u username
```

remove cron job of login user
```
crontab -r
```
 
  remove cronjob for another user
```
crontab -r -u username
```

