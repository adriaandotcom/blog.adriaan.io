---
title: Log output of your cronjobs to syslog in Ubuntu
---

When I setup crontab for a script I want to have all logging in syslog. This way I can see all live output of the script with a single command `tail -n 100 -f /var/log/syslog`.

If I have this crontab `*/10 * * * * /home/user/do-something.sh` (`crontab -l`) I would see this in the logs:

```
Dec 29 12:00:00 server CRON[1011]: (user) CMD (/home/user/do-something.sh)
Dec 29 12:00:00 server CRON[1009]: (CRON) info (No MTA installed, discarding output
```

_No MTA installed, discarding output_ is basically saying that it can't log because there is no email client. Default logs of cronjobs are send to an email client. This is not something I want so I change my crontab `crontab -e` to this:

```
*/10 * * * * /home/user/do-something.sh 2>&1 | /usr/bin/logger -t CRON
```

Explanation

- `*/10 * * * *` Runs the script every 10 minutes
- `/home/user/do-something.sh` is the script being exucted
- `2>&1` forwards the error to standard output
- `| /usr/bin/logger -t CRON` forwards (pipes) stardard output to syslog with the name `CRON`

If you use the name `CRON` you can search your logs like this `cat /var/log/syslog | grep CRON` and you have all output of the `/home/user/do-something.sh` script.
