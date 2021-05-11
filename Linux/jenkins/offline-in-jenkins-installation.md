# Jenkins安装过程中因离线问题导致无法安装插件

Jenkins刚安装好后，登录出现如下图的离线的问题：

![](https://images1.tqwba.com/20200909/3omksllzqa1.png)

### 解决办法

- 第一步：打开配置页面
- 第二步：修改更新网址
- 第三步：添加网站证书
- 第四步：重启Jenkins

### 第一步：打开配置页面

[http://localhost:8080/pluginManager/advanced](http://localhost:8080/pluginManager/advanced)

### 第二步：修改更新网址

原官方网址为：**https://updates.jenkins.io/update-center.json**

修改后网址为：**http://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/current/update-center.json**

### 第三步：添加网站证书

先从浏览器上下载网站证书，上传到Jenkins服务器。

然后将证书添加到Java信任证书列表里：

```
sudo keytool -import -alias SITE_ALIAS -keystore $JAVA_HOME/jre/lib/security/cacerts -file SITE_CERT
```

这里我们需要添加如下证书：

- tuna.tsinghua.edu.cn.cer
- updates.jenkins.io.cer
- get.jenkins.io.cer

那么执行如下命令：

```
sudo keytool -import -alias tsinghuaMirror -keystore $JAVA_HOME/jre/lib/security/cacerts -file tuna.tsinghua.edu.cn.cer
sudo keytool -import -alias updatesJenkins -keystore $JAVA_HOME/jre/lib/security/cacerts -file updates.jenkins.io.cer
sudo keytool -import -alias getJenkins -keystore $JAVA_HOME/jre/lib/security/cacerts -file get.jenkins.io.cer
```

### 第四步：重启Jenkins

这样问题基本纠结了，可以安装插件了，一般安装插件不熟的情况下按照系统推荐的安装即可。

重启：`yum restart jenkins`。


### 参考资源

[Mac用chrome获取https的证书](https://blog.csdn.net/lllkey/article/details/17526323)

[How to Import Public Certificates into Java’s Truststore from a Browser](https://medium.com/expedia-group-tech/how-to-import-public-certificates-into-javas-truststore-from-a-browser-a35e49a806dc)

[Docker 安装 Jenkins 并解决初始安装插件失败问题](https://www.jb51.net/article/185542.htm)

[插件更新中心 - Jenkins中文社区](https://jenkins-zh.cn/tutorial/management/plugin/update-center/)
