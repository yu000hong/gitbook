# Checking nginx config file syntax

Assuming an nginx installation under `/usr/nginx`:

```bash
/usr/nginx/sbin/nginx -t -c /usr/nginx/conf/nginx.conf
```

Note that the path to the config file must be an absolute one, or one relative to the prefix directory where nginx was installed (in this case, /usr/nginx), so this will also work:

```bash
/usr/nginx/sbin/nginx -t -c conf/nginx.conf
```

To test the default discovered config run:

```bash
/usr/nginx/sbin/nginx -t
```
