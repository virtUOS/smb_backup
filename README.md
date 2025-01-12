# Ansible Role for Backups with Samba Share

This ansible role configures autofs to mount a samba share
and configure a script to run regular backups.

## Role Variables

There are three required variables you need to set:

* `backup_smb_user`: the user that will access the backup cloud
* `backup_smb_password`: the password for this user
* `backup_script`: The shell commands to run for creating a backup
* `backup_metrics_path`: Write Prometheus metrics about updates to this file

Have a look at the [defaults](defaults/main.yml) to see all variables and how to set them.


## Prometheus Metrics

The backup automation can create metrics for Prometheus.
The metrics are written to file which can then be exposed via a web server like Apache or Nginx.
The metrics provide the time of the last successful backup.

```openmetrics
# HELP backup_time Time stamp of backup
# TYPE backup_time counter
backup_time 1735876976
```

## Example Playbook

Your playbook, could look like this:

```yaml
- hosts: all
  become: true
    - role: uos.smb_backup
      vars:
        backup_smb_share: //smb.example.com/backup
        backup_smb_user: samba_user
        backup_smb_password: samba_user_password
        backup_script: |
          FILENAME="{{ backup_mountpoint }}/$(date '+%Y%m%d-%H%M%S').sql.gz"
          mysqldump -u root dbname | gzip > "${FILENAME}"
        backup_metrics_path: /var/lib/nginx/backup/metrics
```

## License

[BSD-3-Clause](LICENSE)
