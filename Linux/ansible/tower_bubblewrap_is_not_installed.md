# Tower: bubblewrap is not installed

在使用[ybalt/ansible-tower](https://github.com/ybalt/ansible-tower)安装Tower之后，碰到一个问题：

> RESULTS TRACEBACK
Traceback (most recent call last): File "/tmp/buildd/ansible-tower-3.2.1/debian/ansible-tower/usr/lib/python2.7/dist-packages/awx/main/tasks.py", line 839, in run RuntimeError: bubblewrap is not installed

解决办法就在文档里面：

[Bubblewrap functionality and variables](https://docs.ansible.com/ansible-tower/latest/html/administration/proot_func_variables.html)

只需要进入`SETTINGS > CONFIGURE TOWER > JOBS`将`Enable Job Isolation`设置为**OFF**就行了。

