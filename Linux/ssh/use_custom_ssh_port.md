# Use custom ssh port

You can change ssh port in two ways:

- use `-p` option
- use `.ssh/config` configuration

### use `-p` option

```
$ ssh -p 222 yu000hong@10.x.x.x
```

### use `.ssh/config` configuration

```
$ vim .ssh/config

Host hadoop01
    HostName 10.x.x.x
    Port 222

$ ssh yu000hong@hadoop01
```

