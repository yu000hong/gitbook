# Jenkins Plugin for Ansible 

官方网址：[https://plugins.jenkins.io/ansible/](https://plugins.jenkins.io/ansible/)

使用示例：

```
stage('Deploy') {
    steps {
        sh 'export ANSIBLE_HOST_KEY_CHECKING=False'
        ansiblePlaybook (
            colorized: true,
            become: true,
            playbook: 'playbook.yml',
            inventory: 'inventory/production',
            credentialsId: 'yuhong4',
            disableHostKeyChecking: true,
            extraVars: [
                owner: 'oasis',
                group: 'oasis',
            ],
        )
    }
}
```

### 传递环境变量

传递环境变量可以使用多种方式，包括：

- 使用**extraVars**参数
- 使用**sh**步骤

```
stage('Deploy') {
    steps {
        sh 'export ANSIBLE_HOST_KEY_CHECKING=False'
        ansiblePlaybook (
            colorized: true,
            become: true,
            playbook: 'playbook.yml',
            inventory: 'inventory/production',
            credentialsId: 'yuhong4',
            disableHostKeyChecking: true,
            extraVars: [
                owner: 'oasis',
                group: 'oasis',
            ],
        )
    }
}
```

### Ansible输出着色

要想使Ansible输出能够根据不同状态显示不同颜色，需要如下三步：

- 安装**AnsiColor**插件
- 设置**colorized**参数
- 设置**ansiColor**选项

```Jenkinsfile
pipeline {
    agent any
    options {
        ansiColor("xterm")
    }
    stages {
        stage('Deploy') {
            steps {
                ansiblePlaybook (
                    colorized: true,
                    become: true,
                    playbook: 'playbook.yml',
                    inventory: 'inventory/production',
                    credentialsId: 'ssh-yuhong4',
                    disableHostKeyChecking: true,
                    extraVars: [
                        owner: 'oasis',
                        group: 'oasis',
                    ],
                )
            }
        }
    }
}
```

[Jenkins Console – Xterm Terminal – Colorized Ansible playbook Output](https://www.unixarena.com/2019/04/jenkins-console-xterm-terminal-colorized-ansible-playbook-output.html/)

[https://github.com/jcsirot/ansible-plugin/issues/9](https://github.com/jcsirot/ansible-plugin/issues/9)







