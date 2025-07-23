Islandora Workbench requires user credentials that have administrator-level permissions in the target Drupal. Therefore you should exercise caution when managing those credentials. Workbench offers a few ways to help you do this.

## Password management

Workbench configuration files must contain a `username` setting (unless you use an external credentials file, as described below), but you can provide the corresponding password in three ways:

1. in the `password` setting in your YAML configuration file
1. in the `ISLANDORA_WORKBENCH_PASSWORD` environment variable
1. in response to a prompt when you run Workbench.

If the `password` setting is present in your configuration files, Workbench will use its value as the user password and will ignore the other two methods of providing a password. If the `password` setting is absent, Workbench will look for the `ISLANDORA_WORKBENCH_PASSWORD` environment variable and if it is present, use its value. If both the `password` setting and the `ISLANDORA_WORKBENCH_PASSWORD` environment variable are absent, Workbench will prompt the user for a password before proceeding.

!!! warning
    If you put the password in configuration files, you should not leave the files in directories that are widely readable, send them in emails or share them in Slack, commit the configuration files to public Git repositories, etc.

## The `credentials_file_path` configuration setting

As an alternative to including Drupal user credentials in your Workbench configuration files, you can store the `username` and `password` settings in their own simple YAML file and point to that file in the `credentials_file_path` config setting. The syntax within the main config file (`username` and `password` are omitted from the main config file) is:

```
credentials_file_path: /home/mark/credentials.yml
```

and within the referenced credentials file, it's:

```yaml
username: admin
password: islandora
```

The credentials file can be somewhere only visible to the computer user running Workbench, such as their home directory, or multiple Workbench users can point to the same credentials from their respective configuration files provided all users are able to read the file.

## The `include_password_in_rollback_config_file` configuration setting

By default, Workbench does not include the `password` value in rollback configuration files. If you want it to include that value, set `include_password_in_rollback_config_file` to `true` in the `create` task configuration file that generates the rollback.

## The `remove_password_from_config_file` configuration setting

If you include `remove_password_from_config_file: true` in your configuration file, Workbench will remove the `password` entry from the configuration file immediately after it reads it. All other configuration settings remain as they were in the original version of the file. Adding `remove_password_from_config_file: true` to your configuration files removes the security risk of having passwords exposed to anyone who can read the files, but will mean that, unless you add the `password` setting back into a configuration file prior to the next time you run Workbench using that configuration file, it will prompt you for the password as described above.

This setting will interfere with automated or scripted workflows since Workbench will not continue until the password is entered in response to the prompt. If you don't want to include `password` in your configuration files in these workflows, configure your `ISLANDORA_WORKBENCH_PASSWORD` environment variable to contain the password.