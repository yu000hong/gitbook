# 安装Kubernetes的过程中，遇到报错：docker-ce-cli conflicts with 2:docker-1.13.1-205.git7d71120.el7.centos.x86_64

安装Kubernetes：

```
sudo yum install kubernetes
```

报错：

```
Error: docker-ce-cli conflicts with 2:docker-1.13.1-205.git7d71120.el7.centos.x86_64
Error: docker-ce conflicts with 2:docker-1.13.1-205.git7d71120.el7.centos.x86_64
```

原因：

在安装Kubernetes之前已经安装过Docker了。

解决：

先卸载Docker，再安装Kubernetes即可。

```
sudo yum list installed | grep docker
Repository base is listed more than once in the configuration
Repository updates is listed more than once in the configuration
Repository epel is listed more than once in the configuration
docker-ce.x86_64               3:19.03.6-3.el7                         @mweibo
docker-ce-cli.x86_64           1:19.03.6-3.el7                         @mweibo

sudo yum remove -y docker-ce.x86_64 docker-ce-cli.x86_64

sudo yum install -y kubernetes
```




