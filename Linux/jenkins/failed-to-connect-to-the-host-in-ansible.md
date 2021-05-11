# Failed to connect to the host via ssh: Host key verification failed

我们在Jenkins中，使用Ansible插件进行自动化部署的时候，出现了如下报错：

```
Failed to connect to the host via ssh: Host key verification failed.
```

我多次检查了SSH Key的配置，都没发现问题，但就是不清楚为啥不能连接到主机。Google搜索了一下，发现你必须将`ANSIBLE_HOST_KEY_CHECKING`环境变量设置为**False**才行。

处理方式有三种：

- 直接设置`ANSIBLE_HOST_KEY_CHECKING`
- 在**ansible.cfg**文件中设置
- 在**ansiblePlaybook**步骤里设置

### 第一种：直接设置`ANSIBLE_HOST_KEY_CHECKING`

```
export ANSIBLE_HOST_KEY_CHECKING=False
```

在Jenkins下添加一个`sh`步骤来处理：

```
stage('Deploy') {
    steps {
        sh 'export ANSIBLE_HOST_KEY_CHECKING=False'
        ansiblePlaybook (
            colorized: true,
            become: true,
            playbook: 'playbook.yml',
            inventory: 'inventory/production',
            credentialsId: 'yuhong4'
        )
    }
}
```

### 第二种：在**ansible.cfg**文件中设置

在项目根目录新建文件**ansible.cfg**，添加如下配置：

```
[defaults]
host_key_checking = False
```

Jenkins在自动化操作的时候，ansible会读取当前目录下的**ansible.cfg**配置文件。

### 第三种：在**ansiblePlaybook**步骤里设置

```
stage('Deploy') {
    steps {
        ansiblePlaybook (
            colorized: true,
            become: true,
            playbook: 'playbook.yml',
            inventory: 'inventory/production',
            credentialsId: 'yuhong4',
            hostKeyChecking: false
        )
    }
}
```

将其中的参数**hostKeyChecking**设置为**false**。

但是，这里遇到一个问题就是**hostKeyChecking**参数的设置不起作用，最后发现这是一个BUG： [The hostKeyChecking parameter in a pipeline script does not work](https://issues.jenkins.io/browse/JENKINS-59182)。

这个BUG说明了**hostKeyChecking**参数不起作用，但是之前被废弃的**disableHostKeyChecking**参数却仍然生效，那我们直接使用以前被废弃的这个参数即可：

```
stage('Deploy') {
    steps {
        ansiblePlaybook (
            colorized: true,
            become: true,
            playbook: 'playbook.yml',
            inventory: 'inventory/production',
            credentialsId: 'yuhong4',
            disableHostKeyChecking: true
        )
    }
}
```

