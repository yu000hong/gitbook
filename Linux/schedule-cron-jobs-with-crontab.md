# Scheduling Cron Jobs with Crontab

Cron is a scheduling daemon that executes tasks at specified intervals. These tasks are called cron jobs and are mostly used to automate system maintenance or administration.

### What is Crontab File

Crontab (cron table) is a text file that specifies the schedule of cron jobs. There are two types of crontab files:
- system crontab files
- user crontab files

**system crontab files**

The `/etc/crontab` file and the scripts inside the `/etc/cron.d` directory are system crontab files that can be edited only by the system administrators.

In most Linux distributions you can also put scripts inside the `/etc/cron.hourly`,`/etc/cron.daily`,`/etc/cron.weekly`,`/etc/cron.monthly` directories, and the scripts will be executed every hour/day/week/month.

**user crontab files**

Users' crontab files are named according to the user’s name, and their location varies by operating systems. In Red Hat based distributions such as CentOS, crontab files are stored in the `/var/spool/cron` directory, while on Debian and Ubuntu files are stored in the `/var/spool/cron/crontabs` directory.

### Crontab Syntax and Operators

```
* * * * * command(s)
- - - - -
| | | | |
| | | | ----- Day of week (0 - 7) (Sunday=0 or 7)
| | | ------- Month (1 - 12)
| | --------- Day of month (1 - 31)
| ----------- Hour (0 - 23)
------------- Minute (0 - 59)
```

The first five fields may contain one or more values, separated by a comma or a range of values separated by a hyphen.

- `*`: The asterisk operator means any value or always. If you have the asterisk symbol in the Hour field, it means the task will be performed each hour.
- `,`: The comma operator allows you to specify a list of values for repetition. For example, if you have 1,3,5 in the Hour field, the task will run at 1 am, 3 am and 5 am.
- `-`: The hyphen operator allows you to specify a range of values. If you have 1-5 in the Day of week field, the task will run every weekday (From Monday to Friday).
- `/`: The slash operator allows you to specify values that will be repeated over a certain interval between them. For example, if you have */4 in the Hour field, it means the action will be performed every four hours. It is same as specifying 0,4,8,12,16,20. Instead of asterisk before the slash operator, you can also use a range of values, 1-30/10 means the same as 1,11,21.

> The syntax of system crontab files is slightly different than user crontabs. It contains an additional mandatory user field that specifies which user will run the cron job.
>
> `* * * * * <username> command(s)`

### Predefined Macros

There are several special Cron schedule macros used to specify common intervals. You can use these shortcuts in place of the five-column date specification.

- `@yearly` (or `@annually`): Run the specified task once a year at midnight (12:00 am) of the 1st of January. Equivalent to 0 0 1 1 *.
- `@monthly`: Run the specified task once a month at midnight on the first day of the month. Equivalent to 0 0 1 * *.
- `@weekly`: Run the specified task once a week at midnight on Sunday. Equivalent to 0 0 * * 0.
- `@daily`: Run the specified task once a day at midnight. Equivalent to 0 0 * * *.
- `@hourly`: Run the specified task once an hour at the beginning of the hour. Equivalent to 0 * * * *.
- `@reboot`: Run the specified task at the system startup (boot-time).

### Linux Crontab Command

The crontab command allows you to install, view , or open a crontab file for editing:

- `crontab -e`: Edit crontab file, or create one if it doesn’t already exist.
- `crontab -l`: Display crontab file contents.
- `crontab -r`: Remove your current crontab file.
- `crontab -i`: Remove your current crontab file with a prompt before removal.
- `crontab -u <username>`: Edit other user crontab file. This option requires system administrator privileges.

The crontab command opens the crontab file using the editor specified by the `VISUAL` or `EDITOR` environment variables.

### Crontab Variables

The cron daemon automatically sets several environment variables .

- `PATH`: The default path is set to **PATH=/usr/bin:/bin**. If the command you are executing is not present in the cron specified path, you can either use the absolute path to the command or change the cron $PATH variable. You can’t implicitly append :$PATH as you would do with a regular script.
- `SHELL`: The default shell is set to **/bin/sh**. To change the different shell, use the SHELL variable.
- `HOME`: Cron invokes the command from the user’s home directory. The HOME variable can be set in the crontab.
- `EMAILTO`: The email notification is sent to the owner of the crontab. To overwrite the default behavior, you can use the MAILTO environment variable with a list (comma separated) of all the email addresses you want to receive the email notifications. When MAILTO is defined but empty (MAILTO=""), no mail is sent.

### Crontab Restrictions

The `/etc/cron.deny` and `/etc/cron.allow` files allows you to control which users have access to the crontab command. The files consist of a list of usernames, one user name per line.

By default, only the `/etc/cron.deny` file exists and is empty, which means that all users can use the crontab command. If you want to deny access to the crontab commands to a specific user, add the username to this file.

If the `/etc/cron.allow` file exists only the users who are listed in this file can use the crontab command.

If neither of the files exists, only the users with administrative privileges can use the crontab command.

### Cron Jobs Examples

Run a command at 15:00 on every day from Monday through Friday:

```
0 15 * * 1-5 command
```

Set custom HOME, PATH, SHELL and MAILTO variables and run a command every minute.

```
HOME=/opt
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
SHELL=/usr/bin/zsh
MAILTO=email@example.com

*/1 * * * * command
```

List all crontab related files:

```
[yu000hong$] tree /etc/cron*
/etc/crontab      [not dir]
/etc/cron.deny    [not dir]
/etc/cron.d
    ├── 0hourly
    ├── pcp-pmie
    ├── pcp-pmlogger
    ├── raid-check
    ├── sinabasesyn
    ├── sinalldpexch
    └── sysstat
/etc/cron.daily
    ├── logrotate
    ├── man-db.cron
    └── mlocate
/etc/cron.hourly
    ├── 0anacron
    └── mcelog.cron
/etc/cron.weekly
/etc/cron.monthly
```
