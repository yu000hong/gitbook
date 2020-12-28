# Supervisord: Restarting and Reloading

`Supervisord` is a great daemon for managing application processes. However it does not have a reload option, and restart works different than we get used to. These command makes the following effects.

### service supervisor restart

```bash
service supervisor restart
```

Restart supervisor service without making configuration changes available. It stops, and re-starts all managed applications.

### supervisorctl restart <name>

```bash
supervisorctl restart <name>
```

Restart application without making configuration changes available. It stops, and re-starts the application.

If you create a new configuration. None of the commands above will make it available. If you want to apply your configuration changes in both existing and new configurations, start applications in new configurations, and re-start all managed applications, you should run:

```bash
service supervisor stop
service supervisor start
```
### supervisorctl reread

If you do not want to re-start all managed applications, but make your configuration changes available, use this command:

```bash
supervisorctl reread
```

This command only updates the changes. It does not restart any of the managed applications, even if their configuration has changed. **New application configurations cannot be started, neither**. (See the “update” command below)

### supervisorctl update

```bash
supervisorctl update
```

Restarts the applications whose configuration has changed.

> **Note**: After the update command, new application configurations becomes available to start, but do not start automatically until the supervisor service restarts or system reboots (even if autostart option is not disabled). In order to start new application, e.g app2, simply use the following command: `supervisorctl start app2`
