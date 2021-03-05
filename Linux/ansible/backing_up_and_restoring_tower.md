# Backing Up and Restoring Tower

The ability to backup and restore your system(s) has been integrated into the Tower setup playbook.

The Tower setup playbook is invoked as `setup.sh` from the path where you unpacked the Tower installer tarball. It uses the same inventory file used by the install playbook. The setup script takes the following arguments for backing up and restoring:

- `-b`: Perform a database backup rather than an installation.

- `-r`: Perform a database restore rather than an installation.

As the root user, call setup.sh with the appropriate parameters and Tower backup or restored as configured.

```bash
root@localhost:~# ./setup.sh -b
```

```bash
root@localhost:~# ./setup.sh -r
```

Backup files will be created on the same path that setup.sh script exists. It can be changed by specifying the following EXTRA_VARS: `backup_dest`.

```bash
root@localhost:~# ./setup.sh -e 'backup_dest=/path/to/backup_dir/' -b
```

A default restore path is used unless EXTRA_VARS `restore_backup_file` are provided with a non-default path, as shown in the example below:

```
root@localhost:~# ./setup.sh -e 'restore_backup_file=/path/to/nondefault/backup.tar.gz' -r
```

### my backup playbook

```yml
---
# tasks file for backup

- name: backup tower settings
  shell: |
    docker exec {{ tower_container_id }} bash -c "cd {{ tower_workdir }} && ./setup.sh -e 'backup_dest=tower-backup.tar.gz' -b"
    docker cp {{ tower_container_id }}:{{ tower_workdir }}/tower-backup.tar.gz /tmp/tower-backup-{{ ansible_date_time.date }}.tar.gz
    rsync -avz /tmp/tower-backup-{{ ansible_date_time.date }}.tar.gz {{ tower_backup_target }}
```
