# Ansible Role for Backups with Samba Share

This ansible role configures autofs to mount a samba share
and configure a script to run regular backups.

## Role Variables

There are three required variables you need to set:

* `backup_smb_user`: the user that will access the backup cloud
* `backup_smb_password`: the password for this user
* `backup_script`: The shell commands to run for creating a backup

Have a look at the [defaults](defaults/main.yml) to see all variables and how to set them.


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
          mysqldump -u root --no-data dbname | gzip > "${FILENAME}"
```

## License

[BSD-3-Clause](LICENSE)
