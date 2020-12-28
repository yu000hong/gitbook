# How to change hostname without reboot on CentOS 7

A computer hostname represents a unique name that gets assigned to a computer in a network in order to uniquely identify that computer in that specific network. A computer hostname can be set to any name you like, but you should keep in mind the following rules:

- hostnames can contain letters (from a to z).
- hostnames can contain digits (from 0 to 9).
- hostnames can contain only the hyphen character ( – ) as a special character.
- hostnames can contain the dot special character ( . ).
- hostnames can contain a combination of all three rules but must start and end with a letter or a number.
- hostnames letters are **case-insensitive**.
- hostnames must contain between 2 and 63 characters long.
- hostnames should be descriptive (to ease identifying the computer purpose, location, geographical area, etc on the network).
-
### Display hostname

**hostname**

In order to display a computer name in CentOS 7/8 and RHEL 7/8 systems via console, issue the following command: `hostname`. The `-s` flag displayed the computer short name (hostname only) and the `-f` flag displays the computer FQDN in the network (only if the computer is a part of a domain or realm and the FQDN is set).

```bash
[yu000hong@agent39:~] $ hostname
agent39.yu000hong.com
[yu000hong@agent39:~] $ hostname -s
agent39
[yu000hong@agent39:~] $ hostname -f
agent39.yu000hong.com
```

**/etc/hostname**

You can also display a Linux system hostname by inspecting the content of `/etc/hostname` file using the `cat` command.

```bash
[yu000hong@agent39:~] $ cat /etc/hostname
agent39.yu000hong.com
```

**hostnamectl**

Also, you can display hostname by the command: `hostnamectl`.

```bash
[yu000hong@agent39:~] $ hostnamectl status
   Static hostname: agent39.yu000hong.com
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 3a104133d5fdfe34956114de26a87a97
           Boot ID: 1d0baebe430c413d95a7bf34dcfa3fb3
    Virtualization: kvm
  Operating System: CentOS Linux 7 (Core)
       CPE OS Name: cpe:/o:centos:centos:7
            Kernel: Linux 4.13.3-1.el7.elrepo.x86_64
      Architecture: x86-64
```

### Change hostname

**hostname**

`hostname` command can change the system hostname, but the content in `/etc/hostname` cannot be changed. So, the modification is transient.

```bash
[yu000hong@agent39:~] $ sudo hostname agent39.weibo.com
[yu000hong@agent39:~] $ hostname
agent39.yu000hong.com
[yu000hong@agent39:~] $ cat /etc/hostname
agent39.yu00hong.com
```

**/etc/hostname**

A second method to set up a CentOS 7/8 machine hostname is to manually edit the `/etc/hostname` file and type your new hostname.

**hostnamectl**

In order to change or set a CentOS 7/8 machine hostname, use the `hostnamectl` command as shown in the below command excerpt.

```bash
[yu000hong@agent39:~] $ sudo hostnamectl set-hostname agent39.weibo.com
[yu000hong@agent39:~] $ hostname
agent39.weibo.com
[yu000hong@agent39:~] $ cat /etc/hostname
agent39.weibo.com
```




