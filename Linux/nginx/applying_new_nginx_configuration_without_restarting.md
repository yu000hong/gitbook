# Applying new nginx configuration without restarting

Test the configuration:

```bash
/usr/nginx/sbin/nginx -t -c conf/nginx.conf
```

Find the process ID of the master process:

```bash
ps auxww | grep 'nginx: master process'
```

> You can also just inspect `/var/run/nginx.pid`.

Send a HUP signal:

```bash
kill -s HUP 1234
```

Alternatively, you can instruct nginx to reload it’s configuration file using the `-s` switch.

```bash
/usr/nginx/sbin/nginx -s reload
```
