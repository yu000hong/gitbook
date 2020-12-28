# Creating a Linux service with Systemd

`Systemd` is used for a lot of things in Linux; each object it manages is called a **Unit**, and have a corresponding `Unit File` defining what they are. These can be simple **services** like nginx or MySQL, but they can also be things like **mount points**, **devices**, **sockets**, and lots of other under-the-hood stuff, all managed by systemd. Units can also be targets, used to control when other services will run (i.e., after network is initialized).

### Create Unit File

For this use case though, you probably just want to configure your application as a basic service. To do this, you’ll have to **create a new unit file**, which you’ll want to place in `/etc/systemd/system/` and name with a `.service` extension:

```bash
touch /etc/systemd/system/myapp.service
```

```
[Unit]
Description=Example Service
After=network.target
StartLimitIntervalSec=0

[Service]
Type=simple
Restart=always
RestartSec=1
User=serviceuser
ExecStartPre=
ExecStart=/path/to/executable [options]
ExecStartPost=
ExecStop=
ExecReload=

[Install]
WantedBy=multi-user.target
```

**[Unit]**

First up, the `[Unit]` section, which defines a bunch of metadata about the unit. The **After=** directive can be used to delay unit activation until another unit is started, such as network, or another service, like mysql.service This doesn’t make it a hard dependant of that service, though you can do that with the **Requires=** or **Wants=** directives. This section also configures the maximum number of times the unit will attempt to be started before systemd gives up entirely; since you probably want it to keep trying, you can set this to 0 to disable this behavior.

**[Service]**

Next is the `[Service]` section, specific to service unit files. This is where you’ll configure the **Exec options**. User will run the service as a certain user. You can set this to your personal user account, root, or a custom-made service account. Just make sure the user has enough permissions to do its job.

There are a few different directives here for specifying programs to run. **ExecStartPre=** will run first, allowing you to do any setup necessary before the service really starts. **ExecStart=** is the main executable. **ExecStartPost=** runs afterward, and **ExecStop=** is run when the service shuts down. **ExecReload=** is a special directive, and is used when you call “reload” instead of restart. This allows you to perform run-time reloading of configuration, provided your application has the capability.

**[Install]**

Lastly, the `[Install]` section, which defines some more behaviour related to how systemd handles the unit. This is most commonly used to specify the **WantedBy=** directive, which is used to tell systemd when to start your service, and creates symlinks between targets and their dependant units. If you’re unsure which target to use, `multi-user.target` will run services at startup after most everything is loaded.

### Reload Systemd Daemon

Overall, the configuration is fairly easy, and all you really have to do is put in your executable as an argument in the [Service] section. Once your service is created, you’ll have to reload the systemctl daemon to update with your changes:

```bash
sudo systemctl daemon-reload
```

### Enable the Service

And enable it (which will run it at boot, per the unit config):

```bash
sudo systemctl enable myapp
```

### Start the Service

And then start the service:

```bash
sudo service myapp start
```

The service should now run in the background, and you can check journalctl to watch its output. If it exits, systemd will automatically start it again, and it should run at boot alongside the other services on your system.

If you need to restart your service, you can use:

```bash
sudo service myapp restart
```

Which will execute the **ExecStop=** directive, shutdown the unit, and then start it up again.

