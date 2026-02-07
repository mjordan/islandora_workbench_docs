By default, Islandora Workbench requires user credentials that have administrator-level permissions in the target Drupal. However, it is possible to avoid giving the user the administrator role; the steps required to do this are described below. In either case, you should exercise caution when managing the Workbench user's credentials. Workbench offers a few ways to help you do this.

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

## Using a non-administrator user

If you prefer that the user indicated in Workbench's `username` config setting not have full administrator permissions, you will need to do the following:

1. Create a new Drupal role for you user named, for example, "Workbench user".
2. Make sure Drupal is running version 1.2.0 or higher of the Islandora Workbench Integration module.
3. Include `use_workbench_permissions: true` in your Workbench config file. If you do not include this setting, or set it to `false` (the default), Workbench will expect the user defined in `username` to have the administrator role.

!!! warning
    The specific permissions required to use a non-administrator user depend on which modules are enabled on your Drupal site. In additiona, local or custom content types, media types, and vocabularies will also need to be included in the role configuration. The configuration file provided below only includes permissions required by core Islandora modules, content types, media types, and vocabularies.

    After you import the role configuration into your Drupal, you will need to test whether Workbench runs as expected and if not, add any required permissions to the role that are required.

To create the "Workbench user" role using the sample configuration file below, do the following:

1. Go to your Drupal's Administration > Configuration > Development page (`/admin/config/development/configuration`).
2. Click on the "Import" tab.
3. Click on the "Single item" subtab.
4. In the "Configuration type" list, choose "Role".
5. Copy and paste the YAML configuration data below into the specified text box.
6. Click the "Import" button.

```yaml
langcode: en
status: true
dependencies: {  }
id: workbench_user
label: Workbench user
weight: 10
is_admin: null
permissions:
 - 'access administration pages'
 - 'access content'
 - 'access content overview'
 - 'access contextual links'
 - 'access files overview'
 - 'access help pages'
 - 'access toolbar'
 - 'administer url aliases'
 - 'create article content'
 - 'create audio media'
 - 'create document media'
 - 'create extracted_text media'
 - 'create file media'
 - 'create fits_technical_metadata media'
 - 'create image media'
 - 'create islandora_object content'
 - 'create page content'
 - 'create remote_video media'
 - 'create terms in corporate_body'
 - 'create terms in country'
 - 'create terms in family'
 - 'create terms in frequencies'
 - 'create terms in genre'
 - 'create terms in geo_location'
 - 'create terms in issuance_modes'
 - 'create terms in language'
 - 'create terms in person'
 - 'create terms in physical_form'
 - 'create terms in resource_types'
 - 'create terms in resource_types_dcmi'
 - 'create terms in subject'
 - 'create terms in tags'
 - 'create terms in temporal_subjects'
 - 'create url aliases'
 - 'create video media'
 - 'delete article revisions'
 - 'delete media'
 - 'delete own article content'
 - 'delete own audio media'
 - 'delete own document media'
 - 'delete own extracted_text media'
 - 'delete own file media'
 - 'delete own files'
 - 'delete own fits_technical_metadata media'
 - 'delete own image media'
 - 'delete own islandora_object content'
 - 'delete own page content'
 - 'delete own remote_video media'
 - 'delete own video media'
 - 'delete page revisions'
 - 'delete terms in corporate_body'
 - 'delete terms in country'
 - 'delete terms in family'
 - 'delete terms in frequencies'
 - 'delete terms in genre'
 - 'delete terms in geo_location'
 - 'delete terms in issuance_modes'
 - 'delete terms in language'
 - 'delete terms in person'
 - 'delete terms in physical_form'
 - 'delete terms in resource_types'
 - 'delete terms in resource_types_dcmi'
 - 'delete terms in subject'
 - 'delete terms in tags'
 - 'delete terms in temporal_subjects'
 - 'edit own article content'
 - 'edit own document media'
 - 'edit own extracted_text media'
 - 'edit own file media'
 - 'edit own fits_technical_metadata media'
 - 'edit own image media'
 - 'edit own islandora_object content'
 - 'edit own page content'
 - 'edit own remote_video media'
 - 'edit own video media'
 - 'edit terms in corporate_body'
 - 'edit terms in country'
 - 'edit terms in family'
 - 'edit terms in frequencies'
 - 'edit terms in genre'
 - 'edit terms in geo_location'
 - 'edit terms in islandora_display'
 - 'edit terms in islandora_media_use'
 - 'edit terms in islandora_models'
 - 'edit terms in issuance_modes'
 - 'edit terms in language'
 - 'edit terms in person'
 - 'edit terms in physical_form'
 - 'edit terms in resource_types'
 - 'edit terms in resource_types_dcmi'
 - 'edit terms in subject'
 - 'edit terms in tags'
 - 'edit terms in temporal_subjects'
 - 'manage media'
 - 'manage members'
 - 'replace files'
 - 'revert all revisions'
 - 'update media'
 - 'use islandora workbench'
 - 'use text format basic_html'
 - 'view all revisions'
 - 'view checksums'
 - 'view media'
 - 'view own unpublished content'
 - 'view own unpublished media'
 - 'view the administration theme'
```

Once you have imported the role configuration, confirm that the role is showing up in Drupal by visiting Administration > People > Roles (`/admin/people/roles`). If "Workbench user" is in the list of available roles, it is ready to assign to your Workbench user. Unless the role is lacking any additional permissions required by your Drupal, you can now specify the user with this new role in your configuration file instead of an admin-role user. Don't forget to include `use_workbench_permissions: true` in your Workbench config file.