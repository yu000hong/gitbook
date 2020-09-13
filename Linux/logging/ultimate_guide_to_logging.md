# Ultimate Guide to Logging

### Linux Logging Basics

Operating system logs provide a wealth of diagnostic information about your computer, and Linux is no exception. Everything from kernel events to user actions are logged by Linux, allowing you to see almost any action performed on your servers. In this section, we’ll explain what Linux logs are, where you can find them, and how to interpret them.

### Linux System Logs

Linux has a special directory for storing logs called `/var/log`. This directory contains logs from the OS itself, services, and various applications running on the system.

Some of the most important Linux system logs include:

- `/var/log/syslog` and `/var/log/messages` store all global system activity data, including startup messages. Debian-based systems like Ubuntu store this in **/var/log/syslog**, while Red Hat-based systems like RHEL or CentOS use **/var/log/messages**.
- `/var/log/auth.log` and `/var/log/secure` store all security-related events such as logins, root user actions, and output from `pluggable authentication modules (PAM)`. Ubuntu and Debian use **/var/log/auth.log**, while Red Hat and CentOS use **/var/log/secure**.
- `/var/log/kern.log` stores kernel events, errors, and warning logs, which are particularly helpful for troubleshooting custom kernels.
- `/var/log/cron` stores information about scheduled tasks (cron jobs). Use this data to verify that your cron jobs are running successfully.

Some applications also write log files in this directory. For example, the Apache web server writes logs to the `/var/log/apache2` directory (on Debian), while MySQL writes logs to the `/var/log/mysql` directory. Some applications also log via syslog, which we’ll explain in the next section.

### What’s Syslog?

Syslog is a standard for creating and transmitting logs. The word “syslog” can refer to any of the following.

- **The syslog service**, which receives and processes syslog messages. It listens for events by creating a socket located at `/dev/log`, which applications can write to. It can write messages to a local file or forward messages to a remote server. There are different syslog implementations including `rsyslogd` and `syslog-ng`.

- **The syslog protocol (RFC 5424)**, which is a transport protocol that specifies how to transmit logs over a network. It is also a data format defining how messages are structured. By default, it uses *port `514` for plaintext messages* and *port `6514` for encrypted messages*.

- **A syslog message**, which is any log formatted in the syslog message format. A syslog message consists of a standardized header and message containing the log’s contents.

Since syslog can forward messages to remote servers, it’s often used to forward system logs to log management solutions.

### Syslog Format and Fields

> Syslog messages contain a standardized header with several fields. These include:
- the timestamp,
- the name of the application that generated the event,
- the location in the system where the message originated,
- and its priority.

You can change this format in your syslog implementation’s configuration file, **but using the standard format makes it easier to parse, analyze, and route syslog events**.

Here is an example log message using the default format. It’s from the sshd daemon, which controls remote logins to the system. This message describes a failed login attempt:

```
Jun 4 22:14:15 server1 sshd[41458] : Failed password for root from 10.0.2.2 port 22 ssh2
```

You can also add additional fields to your syslog messages. Let’s repeat the last event after adding a few new fields. We’ll use the following rsyslog template, which adds the priority (`<%pri%>`), protocol version (`%protocol-version%`), and the date formatted using RFC 3339 (`%timestamp:::date-rfc3339%`):

```
<%pri%>%protocol-version% %timestamp:::date-rfc3339% %HOSTNAME% %app-name% %procid% %msgid% %msg%n
```

This generates the following log:

```
<34>1 2019-06-05T22:14:15.003Z server1 sshd - - pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=10.0.2.2
```

Below, you’ll find descriptions of some of the most commonly used syslog fields when searching or troubleshooting issues.

**Timestamp**

The [timestamp](https://tools.ietf.org/html/rfc5424#section-6.2.3) field indicates the time and date the message was generated on the system sending the message. The example timestamp breaks down like this:

- `2019-06-05` is the year, month, and day.
- `T` is a required element of the timestamp field, separating the date and the time.
- `22:14:15.003` is the 24-hour format of the time, including the number of milliseconds (003).
- `Z` indicates UTC time. Instead of z, the example could have included an offset, such as -08:00, which indicates that the time is offset from UTC by eight hours.

**Hostname**

The [hostname](https://tools.ietf.org/html/rfc5424#section-6.2.4) field (`server1` in the example above) indicates the name of the host or system that originally sent the message.

**App-Name**

The [app-name](https://tools.ietf.org/html/rfc5424#section-6.2.5) field (`sshd:auth` in the example) indicates the name of the application that sent the message.

**Priority**

The [priority](https://tools.ietf.org/html/rfc5424#section-6.2.1) field or pri for short (`<34>` in the example above) tells you how urgent or severe the event is. It’s a combination of two numerical fields: **the facility** and **the severity**. The facility specifies the type of process that created the event, ranging from 0 for kernel messages to 23 for local applications. The severity ranges from 0–7, with 0 indicating an emergency and 7 indicating a debug event.

Pri can be output in two ways. The first is as a single number, prival, which is calculated as the facility field value multiplied by eight, then the result is added to the severity field value: `(facility)(8) + (severity)`. The second is pri-text, which will output in the string format `facility.severity`. The latter format can often be easier to read and search, but takes up more storage space.

