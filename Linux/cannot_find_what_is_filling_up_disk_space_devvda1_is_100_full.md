# Cannot find what is filling up disk space /dev/vda1 is 100% full

Here is what I have:

```bash
$ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/vda1        12G   12G     0 100% /usr
```

However, I cannot find what is using the space up with `du -sh *`

```bash
$ du -sh /usr
5.4G	/usr
```

## Answer

You can try `ncdu` to find out what is using up space. If your os is Ubuntu, you can install it with:

```
$ sudo apt-get install ncdu
```

Or, CentOs:

```
$ sudo yum install ncdu
```

**But**, ncdu did not help!

So, I use:

```bash
$ lsof /usr | grep deleted
entrypoin 198069 yuhong  cwd    DIR    8,1          0 426760 /usr/home/yuhong/oasis-service-test (deleted)
entrypoin 198069 yuhong    1w   REG    8,1 3555684352 426763 /usr/home/yuhong/oasis-service-test/nohup.out (deleted)
entrypoin 198069 yuhong    2w   REG    8,1 3555684352 426763 /usr/home/yuhong/oasis-service-test/nohup.out (deleted)
entrypoin 198069 yuhong  255r   REG    8,1        366 426821 /usr/home/yuhong/oasis-service-test/entrypoint.sh (deleted)
java      198070 yuhong  cwd    DIR    8,1          0 426760 /usr/home/yuhong/oasis-service-test (deleted)
java      198070 yuhong    1w   REG    8,1 3555684352 426763 /usr/home/yuhong/oasis-service-test/nohup.out (deleted)
java      198070 yuhong    2w   REG    8,1 3555684352 426763 /usr/home/yuhong/oasis-service-test/nohup.out (deleted)
java      198070 yuhong    3w   REG    8,1     671744 426843 /usr/home/yuhong/oasis-service-test/logs/gc.log.0.current (deleted)
java      198070 yuhong    5r   REG    8,1  177334160 426762 /usr/home/yuhong/oasis-service-test/oasis-service-1.0.jar (deleted)
java      198070 yuhong    6r   REG    8,1  177334160 426762 /usr/home/yuhong/oasis-service-test/oasis-service-1.0.jar (deleted)
java      198070 yuhong   10w   REG    8,1  284184342 426828 /usr/home/yuhong/oasis-service-test/logs/oasis-service-info-class.log (deleted)
java      198070 yuhong   14w   REG    8,1          0 426832 /usr/home/yuhong/oasis-service-test/logs/cashout-service-info.log (deleted)
java      198070 yuhong   15w   REG    8,1          0 426833 /usr/home/yuhong/oasis-service-test/logs/cashout-service-error.log (deleted)
java      198070 yuhong   17w   REG    8,1          0 426835 /usr/home/yuhong/oasis-service-test/logs/oasis-service-appreciate.log (deleted)
java      198070 yuhong   27r   REG    8,1        122 426846 /usr/home/yuhong/oasis-service-test/config/env.conf (deleted)
java      198070 yuhong   61w   REG    8,1 2054794241 426842 /usr/home/yuhong/logs/service_access_log.log (deleted)
java      198070 yuhong  256r   REG    8,1        122 426846 /usr/home/yuhong/oasis-service-test/config/env.conf (deleted)
```

This did work!

> Sometimes if a file is deleted while it is opened by a process, it will not actually free up the disk space until the process is ended. This command will show if there are any files in that state.

Then, I just need kill the processes which are still opening the deleted files.
