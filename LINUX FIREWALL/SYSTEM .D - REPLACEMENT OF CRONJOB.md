Systemd Timers in Linux  
Systemd timers are a modern replacement for traditional cron jobs in Linux. They are used to schedule tasks to run at specific times or intervals.

1. Overview of Systemd Timers

A systemd timer consists of two main components:

- Timer Unit (.timer) - Defines when the job should run.
- Service Unit (.service) - Defines what should be executed.

Timers provide more flexibility than cron, such as:

- Better logging and tracking via journalctl
- Ability to run jobs immediately after boot
- Randomized delays to avoid system overload

1. Creating a Systemd Timer

Step 1: Create a Service Unit  
A service unit defines the actual task that will be executed.

Example: `/etc/systemd/system/myjob.service`

Code

```
[Unit]Description=My Scheduled Job[Service]Type=oneshotExecStart=/usr/bin/bash -c 'echo "Hello, world!" >> /tmp/hello.txt'
```

`Type=oneshot` means the task runs once and exits.

Step 2: Create a Timer Unit  
A timer unit determines when the service should be executed.  
Example: `/etc/systemd/system/myjob.timer`

Code

```
[Unit]Description=Run My Scheduled Job Every 5 Minutes[Timer]OnBootSec=1minOnUnitActiveSec=5minUnit=myjob.service[Install]WantedBy=timers.target
```

- `OnBootSec=1min` Runs the job 1 minute after boot.
- `OnUnitActiveSec=5min` Runs every 5 minutes after that.
- `Unit=myjob.service` Specifies the service to run.


3 . Enabling and Managing the Timer

Enable and Start the Timer

Code

```
sudo systemctl daemon-reload 
sudo systemctl enable myjob.timer --now
```

- `daemon-reload` refreshes systemd after creating new units.
- `enable --now` enables and starts the timer immediately.

Check Timer Status

Code

```
systemctl list-timers --all 
systemctl status myjob.timer
```

Manually Trigger the Job

Code

```
sudo systemctl start myjob.service
```

Disable and Remove the Timer

Code

```
sudo systemctl stop myjob.timersudo systemctl disable myjob.timersudo rm /etc/systemd/system/myjob.{timer,service}sudo systemctl daemon-reload
```  

4 .  Alternative Scheduling Options

Instead of OnUnitActiveSec, one can use:

- `OnCalendar daily` Runs once a day.
- `OnCalendar Mon 12:00:00` Runs every Monday at 12:00 PM.
- `OnCalendar*-*-* 03:15:00` Runs daily at 3:15 AM.

Example Timer:

Code

```
[Timer]OnCalendar=*-*-* 03:15:00Persistent=true
```

`Persistent=true` ensures missed executions (e.g., during a reboot) are run immediately after the system starts.

1. Viewing Logs

Check logs for the service:  
`journalctl -u myjob.service --no-pager`

Systemd Timers vs. Cron Jobs:

|Feature|Cron Jobs|Systemd Timers|
|---|---|---|
|Logging|Limited|Full `journalctl` logs|
|Start at Boot|No|Yes|
|Flexibility|Fixed intervals|More options (randomized delays, boot timers)|
|Failure Handling|No built-in retry|Can retry failed jobs|

Systemd timers are more powerful and integrate better with system logging and startup processes.