# Cannot retrieve metalink for repository: epel


> "CentOS EPEL Error: Cannot retrieve metalink for repository: epel"

If you see an error when attempting to install or update a package, such as:

```bash
# yum install mypackage
Loaded plugins: fastestmirror, security
Setting up Install Process
Loading mirror speeds from cached hostfile
Error: Cannot retrieve metalink for repository: epel. Please verify its path and try again
```

This is fairly common on CentOS 6 if you attempt to use the EPEL repository, and occurs when you attempt to retrieve files from the HTTPS server instead of the HTTP server.

There are two solutions.

### Bash command solution

Run the following command from your command line:

```bash
sudo sed -i 's/(mirrorlist=http)s/\1/' /etc/yum.repos.d/epel.repo
```

### Edit configuration file

While the previous method directly erases the 'S' in 'HTTPS', you can also do it by editing the repository configuration file.

```
vim /etc/yum.repos.d/epel.repo
```

Then, find any lines such as:

`mirrorlist=https://mirrors.fedoraproject.org/metalink?repo=epel-6&arch=$basearch`

And erase the S from any instances of https://

`mirrorlist=http://mirrors.fedoraproject.org/metalink?repo=epel-6&arch=$basearch`
