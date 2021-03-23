# How to use Logrotate to manage log files

Logrotate is a software pre-installed in most Linux distributions, which allows you to manage long-term saving and organization of log files.

Logrotate is the most common solution for periodically checking log files and automatically managing their rotation, compression and elimination when a certain size and/or age is exceeded.

### Exploring the Logrotate Configuration

Logrotate’s configuration information can generally be found in two places:
- `/etc/logrotate.conf`: this file contains some default settings and sets up rotation for a few logs that are not owned by any system packages. It also uses an include statement to pull in configuration from any file in the /etc/logrotate.d directory.
- `/etc/logrotate.d/`: this is where any packages you install that need help with log rotation will place their Logrotate configuration. On a standard install you should already have files here for basic system tools like apt, dpkg, rsyslog and so on.

### Basic Configurations

Logrotate allows to define some basic parameters that will be used by all the subsequent configurations of the file in the path `/etc/logrotate.conf`:

```
# Set a weekly rotation
weekly

# Set the number of rotations to save
rotate 4
              
# make new log file
create     

# Include all configurations files
include /etc/logrotate.d
```

In this extract of `logrotate.conf`, besides applying all the parameters described above, reading all the configurations present in the `/etc/logrotate.d` directory is also recommended. By doing so, you can divide each configuration by application and/or context.

Logrotate configurations define a rule set for 1 or more files:

```
[FILE] [FILE?] [FILE?] {

    [SETTINGS]

}
```

A classic example is that of the configuration of `/etc/logrotate.d/syslog`:

```
/var/log/cron
/var/log/maillog
/var/log/messages
/var/log/secure
/var/log/spooler
{
    missingok
    sharedscripts
    create 0644
    postrotate
	    /bin/kill -HUP `cat /var/run/syslogd.pid 2> /dev/null` 2> /dev/null || true
    endscript
}
```

### More Configurations

You can append multiple paths, by separating them with a comma:

```
/var/log/custom_log, /var/log/alternative_log_file, /var/log/another_log {
    ...
}
```

or with a newline:

```
/var/log/custom_log
/var/log/alternative_log_file
/var/log/another_log
{
    ...
}
```

You can use file masks such as:

```
/var/custom/logs/*, /var/log/custom_log {
      # This configuration will rotate all the files in the directory. 
      # /var/custom/logs/* e il file /var/log/custom_log
      ...
}
```

### Important Settings

- `daily`, `monthly`, `weekly`, `yearly`: indicates how often to rotate the log file
- `compress`, `nocompress`: whether or not to compress old file rotations
- `maxage DAYS`: After how many days the old rotations are eliminated
- `size SIZE`: Sets the rotation only if the indicated files are larger than the specified size. Suffixes can be used to indicate the size format (eg "k" for kilobytes, "M" for megabytes)
- `rotate COUNT`: Number of rotated logs that have to be saved before being permanently removed. If set to 0, the log files will be cleared without being rotated.
- `create MODE OWNER GROUP`: Immediately after rotation (before the postrotate script is run) the log file is created (with the same name as the log file just rotated).
- `dateext`: Archive old versions of log files adding a daily extension like YYYYMMDD instead of simply adding a number. The extension may be configured using the `dateformat` option.
- `dateformat FORMAT`: Specify the extension for dateext using the notation similar to strftime function. Only **%Y** **%m** **%d** and **%s** specifiers are allowed.
- `ifempty`: Rotate the log file even if it is empty.
- `missingok`: If the log file is missing, go on to the next one without issuing an error message. 
- `copy`: Make a copy of the log file, but don't change the original at all. This option can be used, for instance, to make a snapshot of the current log file, or when some other utility needs to truncate or parse the file. When this option is used, the create option will have no effect, as the old log file stays in place.
- `copytruncate`: Truncate the original log file in place after creating a copy, instead of moving the old log file and optionally creating a new one. It can be used when some program cannot be told to close its logfile and thus might continue writing (appending) to the previous log file forever. **Note that there is a very small time slice between copying the file and truncating it, so some logging data might be lost**. When this option is used, the create option will have no effect, as the old log file stays in place.
- `olddir DIR`: Logs are moved into directory for rotation. The directory must be on the same physical device as the log file being rotated, and is assumed to be relative to the directory holding the log file unless an absolute path name is specified. When this option is used all old versions of the log end up in directory. This option may be overridden by the noolddir option.
- `sharedscripts`: Normally, prerotate and postrotate scripts are run for each log which is rotated and the absolute path to the log file is passed as first argument to the script. That means a single script may be run multiple times for log file entries which match multiple files (such as the /var/log/news/* example). If sharedscripts is specified, the scripts are only run once, no matter how many logs match the wildcarded pattern, and whole pattern is passed to them. However, if none of the logs in the pattern require rotating, the scripts will not be run at all. If the scripts exit with error, the remaining actions will not be executed for any logs. This option overrides the nosharedscripts option and implies create option.
- `prerotate/endscript`: The lines between prerotate and endscript (both of which must appear on lines by themselves) are executed (using /bin/sh) before the log file is rotated and only if the log will actually be rotated. These directives may only appear inside a log file definition. Normally, the absolute path to the log file is passed as first argument to the script. If sharedscripts is specified, whole pattern is passed to the script. 
- `postrotate/endscript`: The lines between postrotate and endscript (both of which must appear on lines by themselves) are executed (using /bin/sh) after the log file is rotated. These directives may only appear inside a log file definition. Normally, the absolute path to the log file is passed as first argument to the script. If sharedscripts is specified, whole pattern is passed to the script. See also prerotate. See sharedscripts and nosharedscripts for error handling.

- `nocreate`: New log files are not created (this overrides the create option).
- `nocopy`: Do not copy the original log file and leave it in place. (this overrides the copy option).
- `nocopytruncate`: Do not truncate the original log file in place after creating a copy (this overrides the copytruncate option).
- `nomissingok`: If a log file does not exist, issue an error. This is the default. 
- `nocompress`: Old versions of log files are not compressed.
- `nodateext`: Do not archive old versions of log files with date extension (this overrides the dateext option).
- `noolddir`: Logs are rotated in the same directory the log normally resides in (this overrides the olddir option).
- `notifempty`: Do not rotate the log if it is empty.

### Setting Crontab file

Logrotate is not a daemon application, so you need crontab to trigger the rotation action periodically. In my CentOS, there is a crontab file for logrotate:

```
[yu000hong@centos] cat /etc/cron.daily/logrotate
#!/bin/sh

/usr/sbin/logrotate -s /var/lib/logrotate/logrotate.status /etc/logrotate.conf
EXITVALUE=$?
if [ $EXITVALUE != 0 ]; then
    /usr/bin/logger -t logrotate "ALERT exited abnormally with [$EXITVALUE]"
fi
exit 0
```

As you see, the crontab file is in `/etc/cron.daily` directory, so, the logrotate application will be executed everyday.

### Logrotate State File

The default state file is `/var/lib/logrotate.status`. You can use an alternate state file by `--state STATEFILE` argument. This is useful if logrotate is being run as a different user for various sets of log files.  To prevent parallel execution logrotate by default acquires a lock on the state file, if it cannot be acquired logrotate will exit with value 3. 

Show the content of state file:

```
[yu000hong@centos] cat /var/lib/logrotate.status
logrotate state -- version 2
"/var/log/spooler" 2021-3-23-10:37:33
"/var/log/maillog" 2021-3-23-10:37:33
"/var/log/secure" 2021-3-23-10:37:33
"/var/log/messages" 2021-3-23-10:37:33
"/var/log/cron" 2021-3-23-10:37:33
```

