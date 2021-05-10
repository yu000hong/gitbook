# 为什么不建议将Jenkins安装到容器中？

因为Jenkins需要宿主机的许多其他功能，这时候，我们使用Docker的话，就很难安装这些相应的软件了。

比如：我们要使用**Jenkins+Ansible**进行持续集成，除了需要在Jenkins里面安装Ansible插件外，还需要宿主机已经安装好了Ansible，路径下有可用的Ansible系列命令，如：ansible、ansible-playbook、ansible-galaxy等命令。


