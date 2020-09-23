# useradd: cannot open /etc/passwd

### 问题描述

今天在一个新的Linux环境添加用户的时候，发现不能添加，遇到了以下错误：

```
useradd: cannot open /etc/passwd
```

### 解决方法

用`lsattr`命令查看**/etc/passwd**的隐藏权限：

```
[~]# lsattr /etc/passwd
----i--------e- /etc/passwd
```

权限 `i` 说明设定文件不能被删除、改名、设定链接关系，同时不能写入或新增内容。

用`chattr`命令对**/etc/group**去除`i`权限位：

```
[ ~]# chattr -i /etc/passwd
[ ~]# useradd -d /home/test -m test
useradd: cannot open /etc/shadow
```

用同样的方式去除/etc/shadow 权限：

```
[ ~]# chattr -i /etc/shadow
[ ~]# useradd -d /home/test -m test
```

