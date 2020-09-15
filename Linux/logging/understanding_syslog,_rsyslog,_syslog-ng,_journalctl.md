# Linux logging guide: Understanding syslog, rsyslog, syslog-ng, journalctl

[https://medium.com/@ikshitijsingh/linux-logging-guide-understanding-syslog-rsyslog-syslog-ng-journalctl-8b4b22af4ad5](https://medium.com/@ikshitijsingh/linux-logging-guide-understanding-syslog-rsyslog-syslog-ng-journalctl-8b4b22af4ad5)

Let’s understand some of the basic linux logging terms such as syslog, journald, rsyslog , syslog-ng and systemd journald.

### Basics of linux logging

We’ll go ahead and start with very basics. So why is there a need to handle it ? Operating systems provide some very insightful information about your system with help of logs.

If dealt with these logs properly, they can yield you a wealth of diagnostic information about system. This information can help prevent your system from some unwanted situations such as downtime of you system, sudden crashes, freezing up etc.

Understanding it more from Linux system point of view, starting from Kernal events till the user interaction with the system is logged by OS.

So where exactly these logs are stored ? Linux has a special directory which is used to store all logs : `/var/log` . This is what a typical log directory looks like. To view these logs, you can use any of the command below :

```shell
# less /var/log/messages
# more -f /var/log/messages
# cat /var/log/messages
# tail -f /var/log/messages # grep -i error /var/log/messages
```

Linux has a graphical log viewer named, System log viewer that can be used to monitor system logs. To start System log viewer , follow the instruction below :

```
Click on System menu > Choose Administration > System Log
```

### What is Syslog ?

> Syslog is Linux system’s standard service to create, collect, store and transmit logs.

All the logs listed above are generated using `rsyslogd` service , which nothing other than syslog’s service. Support of both internet and unix domain sockets enables this utility to support both local and remote logging.
Syslog protocol has gained lot of support from many operating systems, including Linux, Unix and Mac Os. For windows as well it is supported , but through many open sources and third party libraries.

You can check what all syslog has to offer in terms of configuration from [Syslog official documentation](https://linux.die.net/man/3/syslog) . However if you want to change config files, please find commands below :

```
# vi /etc/syslog.conf
# ls /etc/syslog.d/
```

Syslog has many other implementations such as `rsyslog` , `syslog-ng` and some others.

### How does syslog work?

When a system is running the syslog daemon, device messages are generated during normal and abnormal operation such as error or exceptions based on what the application developers have specified in app logs.

These messages can then be viewed in various way. First is to monitor the messages in real time on the originating device’s console itself. Another method could be to view the local log files that contain historical log information.

[Stackify](https://stackify.com/syslog-101/) has written a well documented post on syslog.

The Syslog project was the very first project. It started in 1980. It is the root project to Syslog protocol. At this time, Syslog is a very simple protocol. **At the beginning it only supported UDP for transport, so it does not guarantee the delivery of the messages. Messages may or may not be delivered.**

### What is syslog-ng ?

Next came syslog-ng in 1998. "ng" here basically means "Next-Gen" and as the name suggests, it extends basic syslog protocol with new features like:

- content-based filtering
- Logging directly into a database
- TCP for transport
- TLS encryption

Edit syslog-ng config file with the command :

```
vi /etc/syslog-ng/syslog-ng.conf
```

Read more about rsyslog on their official website here : [www.syslog-ng.com](https://www.syslog-ng.com/)

### What is rsyslog ?

Next came Rsyslog in 2004. Rsyslog works with same config file as for syslog, but the file gets renamed as `rsyslog.conf` instead of `syslog.conf`. To change config for syslog you can use these commands :

```
# vi /etc/rsyslog.conf
# ls /etc/rsyslog.d/
```

rsyslog extends syslog protocol with new features like:

- RELP Protocol support
- Buffered operation support
- You can listen to TCP/UDP connections
- It can load a lot of modules
- You can discard message after one or more rules

Read more about rsyslog on their official website here : [www.rsyslog.com](www.rsyslog.com)

### What is journalctl or systemd’s journald ?

Journal is a systemd core component so it’s automatically installed on any operating system using systemd. Journal provides structured and indexed logging, while providing a certain degree of compatibility with classic syslog implementations.

**The major difference journal has from other syslog based management tools is that , it stores logs or messages in binary format, which is not human readable.** journal data logs are usually processed by an application called `journalctl`.

However, one more unique flexibility journal provides is of making it optional to store data permanently. Yes, you can actually disable permanent storage of logs. When permanent storage is not enabled, journal uses the directory `/run/log/journal` to store loges , whereas /var/log/journal in case of permanent storage enabled. Only in case of permanent storage, data logs remain after rebooting the computer.

Run the journalctl command without any arguments to view all the logs in your journal:

```
journalctl
```

So this was quick guide on linux logging wherein i tried explaining most famous ways to deal with the logs in your system. Any suggestions and comments are totally welcomed !
