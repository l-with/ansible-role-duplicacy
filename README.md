# Ansible Role Duplicacy

install and configure [duplicacy](https://github.com/gilbertchen/duplicacy)

The role installs duplicacy and three scripts for `duplicacy init`, `duplicacy backup` and `duplicacy prune`.
The pre and post scripts by default are placed in the duplicacy working directory at `.duplicacy/scripts` to be used by the builtin mechanism from duplicacy for pre and post commands.
They can be placed in `duplicacy_scriptfile_path` and then will be executed by the `duplicacy backup` or `duplicacy prune` script.

The output of `duplicacy backup` is placed in `backup.log` and the output of `duplicacy prune` in `prune.log` in the working directory of duplicacy.

Supported storage backends are defined in `duplicacy_storage_backends`:

- 'Local disk'
- 'Blackblaze B2'
- 'SSH/SFTP Password'
- 'SSH/SFTP Keyfile'
- 'Onedrive'

## Role Variables

<!-- markdownlint-disable MD033 -->
| group | variable | default | description |
| --- | --- | ---| --- |
| install | duplicacy_version | `3.2.3` | the duplicacy version to install |
| install | duplicacy_path | `/opt/duplicacy` | the path to install duplicacy |
| install | duplicacy_scriptfile_path | `"{{ duplicacy_path }}/scripts"` | the path where the scripts for `duplicacy init`, `duplicacy backup`, `duplicacy restore` and `duplicacy prune` are created |
| configure | duplicacy_snapshot_id | | the `<snapshot id>` for `duplicacy init` |
| configure | duplicacy_working_directory | | the working directory for duplicacy which is the default path for the repository to backup |
| configure | duplicacy_password | | the value for `DUPLICACY_PASSWORD`, e.g. the passphrase to encrypt the backups with before they are stored remotely |
| configure | duplicacy_storage_url | | the `<storage url>` for ´duplicacy init`, e.g. the [Duplicacy URI](https://github.com/gilbertchen/duplicacy/wiki/Storage-Backends) of where to store the backups |
| configure | duplicacy_storage_backend | | the storage backend, possible values are  <br /><ol><li>`Local disk`</li><li>`Backblaze B2`</li><li>`SSH/SFTP Password`</li><li>`SSH/SFTP Keyfile`</li><li>`Onedrive`</li></ol> |
| configure | duplicacy_repository | `"{{ duplicacy_working_directory }}"` | the `<path>` for `duplicacy init -repository <path>` |
| configure | duplicacy_secret_file_path | `"{{ duplicacy_path }}/secret"` | the path where the token and the ssh-key files are created |
| configure | _duplicacy_secret_file_name | ATTENTION: internal variable! The value depends on `duplicacy_storage_backend` | the filename for the secret file, the default is <br /><ol><li>`Local disk`<br />irrelevant</li><li>`Backblaze B2`<br />irrelevant</li><li>`SSH/SFTP Password`<br />irrelevant</li><li>`SSH/SFTP Keyfile`<br />`"{{ duplicacy_ssh_key_file_name }}"`</li><li>`Onedrive`<br />`{{ duplicacy_onedrive_token_file_name }}`</li></ol> |
| configure | duplicacy_secret_file_content | | the content for `_duplicacy_secret_file_name` |
| configure | duplicacy_secret_file_force | `false` | if the templating of the secret file will be forced, even if the secret file exists |
| configure | duplicacy_onedrive_token_file_name | `one-token.json`| the filename for `DUPLICACY_ONE_TOKEN` |
| configure | duplicacy_ssh_key_file_name | `id` | the filename for `DUPLICACY_SSH_KEY_FILE` |
| configure | duplicacy_ssh_password | | the value for `DUPLICACY_SSH_PASSWORD` |
| configure | duplicacy_ssh_passphrase | | the value for `DUPLICACY_SSH_PASSPHRASE` |
| configure | duplicacy_b2_id | | the value for `DUPLICACY_B2_ID` |
| configure | duplicacy_b2_key | | the value for `DUPLICACY_B2_KEY` |
| configure | duplicacy_schedule | `true` | if duplicacy should be scheduled with cron |
| init | duplicacy_init_options | `'-encrypt'` | the options for `duplicacy init` |
| init | duplicacy_init_script_file | `"{{ duplicacy_script_file_path }}/init"` | the duplicacy init script file |
| backup | duplicacy_backup_options | `` | the options for `duplicacy backup` |
| backup | duplicacy_backup_immediately | `false` | if a backup should be performed immediately |
| backup | duplicacy_backup_script_file | `"{{ duplicacy_script_file_path }}/backup"` | the duplicacy backup script file |
| backup | duplicacy_pre_backup_script_file_name | `'pre-backup'` | the file name of the pre backup script |
| backup | duplicacy_pre_backup_script_file | `"{{ duplicacy_working_directory }}/.duplicacy/scripts/{{ duplicacy_pre_backup_script_file_name }}"` | the pre backup script file |
| backup | duplicacy_post_backup_script_file_name | `'post-backup'` | the file name of the post backup script |
| backup | duplicacy_post_backup_script_file | `"{{ duplicacy_working_directory }}/.duplicacy/scripts/{{ duplicacy_post_backup_script_file_name }}"` | the post backup script file |
| backup | duplicacy_pre_backup_script_file_content |  | the content for the pre backup script |
| backup | duplicacy_post_backup_script_file_content |  | the content for the post backup script |
| backup | duplicacy_backup_schedule | `"{{ duplicacy_schedule }}"` | if duplicacy backup should be scheduled with cron |
| backup | duplicacy_backup_schedule_user | `root` | the cron schedule user for duplicacy backups |
| backup | duplicacy_backup_schedule_hour | `1` | the cron schedule hour for duplicacy backups |
| backup | duplicacy_backup_schedule_minute | `0` | the cron schedule minute for duplicacy backups |
| backup | duplicacy_backup_schedule_day | `*` | the cron schedule day for duplicacy backups |
| backup | duplicacy_backup_schedule_weekday | `*` | the cron schedule weekday for duplicacy backups |
| backup | duplicacy_backup_schedule_month | `*` | the cron schedule month for duplicacy backups |
| prune | duplicacy_prune_script_file | `"{{ duplicacy_script_file_path }}/prune"` | the duplicacy prune script file |
| prune | duplicacy_pre_prune_script_file_name | `'pre-prune'` | the file name of the pre prune script |
| prune | duplicacy_pre_prune_script_file | `"{{ duplicacy_working_directory }}/.duplicacy/scripts/{{ duplicacy_pre_prune_script_file_name }}"` | the pre prune script file |
| prune | duplicacy_post_prune_script_file_name | `'post-prune'` | the file name of the post backup script |
| prune | duplicacy_post_prune_script_file | `"{{ duplicacy_working_directory }}/.duplicacy/scripts/{{ duplicacy_post_backup_script_file_name }}"` | the post backup script file |
| prune | duplicacy_pre_prune_script_file_content |  | the content for the pre prune script |
| prune | duplicacy_post_prune_script_file_content |  | the content for the post prune script |
| prune | duplicacy_prune_options | `-keep 365:3650 -keep 30:365 -keep 7:30 -keep 1:7 -a` | the options for `duplicacy prune` |
| prune | duplicacy_prune_schedule | `"{{ duplicacy_schedule }}"` | if duplicacy prune should be scheduled with cron |
| prune | duplicacy_prune_schedule_user | `root` | the cron schedule user for duplicacy prunes |
| prune | duplicacy_prune_schedule_hour | `4` | the cron schedule hour for duplicacy prunes |
| prune | duplicacy_prune_schedule_minute | `0` | the cron schedule minute for duplicacy prunes |
| prune | duplicacy_prune_schedule_day | `*` | the cron schedule day for duplicacy prunes |
| prune | duplicacy_prune_schedule_weekday | `*` | the cron schedule weekday for duplicacy prunes |
| prune | duplicacy_prune_schedule_month | `*` | the cron schedule month for duplicacy prunes |
| restore | duplicacy_restore_options | `'-overwrite'` | the options for `duplicacy restore` |
| restore | duplicacy_restore_script_file | `"{{ duplicacy_script_file_path }}/restore"`| the duplicacy restore script file |
<!-- markdownlint-enable MD033 -->

## Test

This role can be tested by

```bash
ansible-playbook test/playbook.yml -e "test_backend='Not implemented'"
```

```bash
ansible-playbook test/playbook.yml -e "test_backend='Local disk'"
```

```bash
ansible-playbook test/playbook.yml -e "test_backend='Blackblaze B2'" -e@test/.blackblaze_b2.yml
```

```bash
ansible-playbook test/playbook.yml -e "test_backend='SSH/SFTP Password'" -e@test/.ssh_sftp_password.yml
```

```bash
ansible-playbook test/playbook.yml -e "test_backend='SSH/SFTP Keyfile'" -e@test/.ssh_sftp_key.yml
```

```bash
ansible-playbook test/playbook.yml -e "test_backend='Onedrive'" -e@test/.onedrive.yml
```

The files `test/.<storage>.yml` should contain and set the variables of `test/<storage>.yml`.
