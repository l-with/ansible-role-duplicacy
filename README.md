# Ansible Role Duplicacy

install and configure [duplicacy](https://github.com/gilbertchen/duplicacy)

The role installs duplicacy and three scripts for `duplicacy init`, `duplicacy backup` and `duplicacy prune`.
The pre and post scripts by default are placed in the duplicacy working directory at `.duplicacy/scripts` to be used by the builtin mechanism from duplicacy for pre and post commands.
Then can be placed in `duplicacy_scriptfile_path` and then will be executed by the `duplicacy backup` or `duplicacy prune` script.

The output of `duplicacy backup` is placed in `backup.log` and the output of `duplicacy prune` in `prune.log` in the working directory of duplicacy.

## Role Variables

<!-- markdownlint-disable MD033 -->
| group | variable | default | description |
| --- | --- | ---| --- |
| install | duplicacy_version | `2.7.2` | the duplicacy version to install |
| install | duplicacy_path | `/opt/duplicacy` | the path to install duplicacy |
| backup | duplicacy_snapshot_id | | the `<snapshot id>` for `duplicacy init` |
| backup | duplicacy_working_directory | | the working directory for duplicacy which is the default path for the repository to backup |
| backup | duplicacy_password | | the value for `DUPLICACY_PASSWORD`, e.g. the passphrase to encrypt the backups with before they are stored remotely |
| backup | duplicacy_storage_url | | the `<storage url>` for ´duplicacy init`, e.g. the [Duplicacy URI](https://github.com/gilbertchen/duplicacy/wiki/Storage-Backends) of where to store the backups |
| backup | duplicacy_storage_backend | | the storage backend, possible values are  <br /><ol><li>`Local disk`</li><li>`Backblaze B2`</li><li>`SSH/SFTP Password`</li><li>`SSH/SFTP Keyfile`</li><li>`Onedrive`</li></ol> |
| backup | duplicacy_init_options | `''` | the options for `duplicacy init` |
| backup | duplicacy_repository_path | `"{{ duplicacy_working_directory }}"` | the `<path>` for `duplicacy ini -repository <path>` |
| backup | duplicacy_secrets_file_path | `"{{ duplicacy_path }}/secret"` | the path where the token and the ssh-key files are created |
| backup | duplicacy_secret_file_name | it depends on `duplicacy_autobackup_storage_backend` | the filename for the secret file, the default is <br /><ol><li>`Local disk`<br />irrelevant</li><li>`Backblaze B2`<br />irrelevant</li><li>`SSH/SFTP Password`<br />irrelevant</li><li>`SSH/SFTP Keyfile`<br />`"{{ duplicacy_ssh_key_file_name }}"`</li><li>`Onedrive`<br />`{{ duplicacy_onedrive_token_file_name }}`</li></ol> |
| backup | duplicacy_secret_file_content | | the content for `duplicacy_secret_file_name` |
| backup | duplicacy_secret_file_force | `false` | if the templating of the secret file will be forced, even if the secret file exists |
| backup | duplicacy_onedrive_token_file_name | `one-token.json`| the filename for `DUPLICACY_ONE_TOKEN` |
| backup | duplicacy_ssh_key_file_name | `id` | the filename for `DUPLICACY_SSH_KEY_FILE` |
| backup | duplicacy_b2_id | | the value for `DUPLICACY_B2_ID` |
| backup | duplicacy_b2_key | | the value for `DUPLICACY_B2_KEY` |
| backup | duplicacy_backup_immediately | `false` | if a backup should be performed immediately after the container is started immediately |
| backup | duplicacy_backup_schedule_user | `root` | the cron schedule user for duplicacy backups |
| backup | duplicacy_backup_schedule_hour | `1` | the cron schedule hour for duplicacy backups |
| backup | duplicacy_backup_schedule_minute | `0` | the cron schedule minute for duplicacy backups |
| backup | duplicacy_backup_schedule_day | `*` | the cron schedule day for duplicacy backups |
| backup | duplicacy_backup_schedule_weekday | `*` | the cron schedule weekday for duplicacy backups |
| backup | duplicacy_backup_schedule_month | `*` | the cron schedule month for duplicacy backups |
| backup | duplicacy_scriptfile_path | `"{{ duplicacy_path }}/scripts"` | the path where the scripts for `duplicacy init`, `duplicacy backup` and `duplicacy prune` are created |
| backup | duplicacy_pre_backup_script_file_name | `'pre-backup'` | the file name of the pre backup script |
| backup | duplicacy_pre_backup_script_file | `"{{ duplicacy_working_directory }}/.duplicacy/scripts/{{ duplicacy_pre_backup_script_file_name }}"` | the pre backup script file |
| backup | duplicacy_post_backup_script_file_name | `'post-backup'` | the file name of the post backup script |
| backup | duplicacy_post_backup_script_file | `"{{ duplicacy_working_directory }}/.duplicacy/scripts/{{ duplicacy_post_backup_script_file_name }}"` | the post backup script file |
| backup | duplicacy_pre_prune_script_file_name | `'pre-prune'` | the file name of the pre prune script |
| backup | duplicacy_pre_prune_script_file | `"{{ duplicacy_working_directory }}/.duplicacy/scripts/{{ duplicacy_pre_prune_script_file_name }}"` | the pre prune script file |
| backup | duplicacy_post_prune_script_file_name | `'post-prune'` | the file name of the post backup script |
| backup | duplicacy_post_prune_script_file | `"{{ duplicacy_working_directory }}/.duplicacy/scripts/{{ duplicacy_post_backup_script_file_name }}"` | the post backup script file |
| backup | duplicacy_pre_backup_script_file_content |  | the content for the pre backup script |
| backup | duplicacy_post_backup_script_file_content |  | the content for the post backup script |
| backup | duplicacy_pre_prune_script_file_content |  | the content for the pre prune script |
| backup | duplicacy_post_prune_script_file_content |  | the content for the post prune script |
| prune | duplicacy_prune_options | `-keep 365:3650 -keep 30:365 -keep 7:30 -keep 1:7 -a` | the options for `duplicacy prune` |
| prune | duplicacy_prune_schedule_user | `root` | the cron schedule user for duplicacy prunes |
| prune | duplicacy_prune_schedule_hour | `4` | the cron schedule hour for duplicacy prunes |
| prune | duplicacy_prune_schedule_minute | `0` | the cron schedule minute for duplicacy prunes |
| prune | duplicacy_prune_schedule_day | `*` | the cron schedule day for duplicacy prunes |
| prune | duplicacy_prune_schedule_weekday | `*` | the cron schedule weekday for duplicacy prunes |
| prune | duplicacy_prune_schedule_month | `*` | the cron schedule month for duplicacy prunes |
| prune | duplicacy_pre_prune_script_file_name | `pre-prune.sh` | the filename for the pre prune script |
| prune | duplicacy_pre_prune_script_file_content |  | the content for the pre prune script |
| prune | duplicacy_post_prune_script_file_name | `post-prune.sh` | the filename for the post prune script |
| prune | duplicacy_post_prune_script_file_content |  | the content for the post prune script |
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
