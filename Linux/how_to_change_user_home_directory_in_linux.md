# How to change user home directory in linux

You can use the `usermod` command to change the default home directory for a user.

```bash
usermod -m -d /path/to/dir username
```

>
> - `-d`: path to new home direcotry
> - `-m`: The contents of the current home directory will be moved to the new home directory, which is created if it does not already exist.

What this command does is edit the file `/etc/passwd`. Opening `/etc/passwd` you will find there is a line for every user, including system users (mysql, posftix, etc), with seven fields per line denoted by colons.

The first field is our user and the last two fields are the starting directory and the shell. For example, the following line will login user **gitlab-runner** to **/data0/home/gitlab-runner**.

```
gitlab-runner:x:987:984:GitLab Runner:/data0/home/gitlab-runner:/bin/bash
```

