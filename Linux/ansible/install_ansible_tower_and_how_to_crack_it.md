# Install Ansible Tower and How to crack it

### Ansible Tower 简介

- 公司中实现运维自动化的架构中主要用到Ansible，Ansible脚本在部署服务器指令行中显得不太直观。Ansible-= Tower是将Ansible的指令界面化，简明直观，简单易用。
- Ansibke Tower其实就是一个图形化的任务调度，复杂服务部署，IT自动化的一个管理平台，属于发布配置管理系统，支持Api及界面操作，Django编写。
- Ansible Tower可以通过界面从git拉取最新playbook实施服务部署，提高生产效率。当然它也提供一个RESET API和命令行CLI以供python脚本调用。

官方网站：https://www.ansible.com/products/tower

中文指南：http://www.ansible.com.cn/docs/tower.html

官方安装文档：http://docs.ansible.com/ansible-tower/latest/html/quickinstall/index.html

官方源地址：http://releases.ansible.com/ansible-tower/setup-bundle/

### 安装 Ansible Tower

有两种安装方式：

- 下载bundle包
- 使用Docker


#### 下载bundle包

```bash
$ wget https://releases.ansible.com/ansible-tower/setup-bundle/ansible-tower-setup-bundle-3.6.2-1.el7.tar.gz
$ tar xf ansible-tower-setup-bundle-3.6.2-1.el7.tar.gz
$ cd ansible-tower-setup-bundle-3.6.2-1/
$ ls
backup.yml  bundle  group_vars  install.yml  inventory  licenses  README.md  rekey.yml  restore.yml  roles  setup.sh
$ vim inventory
[tower]
localhost ansible_connection=local

[database]

[all:vars]
admin_password='tower'     # 必须设置

pg_host=''
pg_port=''

pg_database='awx'
pg_username='awx'
pg_password='tower'        # 必须设置
pg_sslmode='prefer'  # set to 'verify-full' for client-side enforced SSL

rabbitmq_username=tower
rabbitmq_password='tower'  # 必须设置
rabbitmq_cookie=cookiemonster

$ ./setup.sh
```

`admin_password`, `pg_password`, `rabbitmq_password`必须设置，否则无法正常启动Ansible Tower！`admin_password`是Ansible Tower的登录密码。

然后就可以访问Tower了：[https://localhost](https://localhost)

> Ansible Tower 3.6.2 要求CentOS版本最低为：7.4 ！

#### 使用Docker

[ybalt / ansible-tower](https://github.com/ybalt/ansible-tower)

```bash
$ sudo docker run -d -p 443:443 --name tower ybalt/ansible-tower
```

容器启动之后就可以访问Tower了：[https://localhost](https://localhost)

- 帐号：admin
- 密码：password

### 破解 Ansible Tower

> 无论是使用bundle包安装还是使用Docker，下面的步骤都是一样的，不一样的是一个是在主机环境中操作，一个是在Docker环境中操作。

```bash
# python版本不一样可能要稍微调整一下
$ cd /var/lib/awx/venv/awx/lib/python3.6/site-packages/tower_license
$ ls
__init__.pyc   __pycache__

# 安装pip，如果Docker中没有安装wget的话，那么从主机copy进来
$ wget https://bootstrap.pypa.io/get-pip.py
$ python get-pip.py
$ pip install uncompyle6

# 反汇编init.pyc
$ uncompyle6 __init__.pyc >__init__.py
$ ls
__init__.py  __init__.pyc  __pycache__
$ vi __init__.py
    def _check_cloudforms_subscription(self):
        return True    #添加这一行
        if os.path.exists('/var/lib/awx/i18n.db'):
            return True
        else:
            ...

# 修改后重新编译一下
$ python -m py_compile __init__.py
$ python -O -m py_compile __init__.py
$ ls
__init__.py  __init__.pyc  __init__.pyo  __pycache__

# 重启服务
$ ansible-tower-service restart
Restarting Tower
Redirecting to /bin/systemctl stop rh-postgresql10-postgresql.service
Redirecting to /bin/systemctl stop rabbitmq-server.service
Redirecting to /bin/systemctl stop nginx.service
Redirecting to /bin/systemctl stop supervisord.service
Redirecting to /bin/systemctl start rh-postgresql10-postgresql.service
Redirecting to /bin/systemctl start rabbitmq-server.service
Redirecting to /bin/systemctl start nginx.service
Redirecting to /bin/systemctl start supervisord.service
```

至此，破解成功！

### 问题解决

安装破解之后，使用仍然会碰到一个问题：

> RESULTS TRACEBACK Traceback (most recent call last): File "/tmp/buildd/ansible-tower-3.2.1/debian/ansible-tower/usr/lib/python2.7/dist-packages/awx/main/tasks.py", line 839, in run RuntimeError: bubblewrap is not installed

解决办法就在文档里面：

[Bubblewrap functionality and variables](https://docs.ansible.com/ansible-tower/latest/html/administration/proot_func_variables.html)

只需要进入`SETTINGS` > `CONFIGURE TOWER` > `JOBS`将 **Enable Job Isolation** 设置为 **OFF** 就行了。

### 参考资料

[Ansible-Tower--安装配置及破解](https://www.cnblogs.com/hujinzhong/p/12172903.html)


