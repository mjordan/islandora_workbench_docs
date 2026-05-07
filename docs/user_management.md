!!! warning
    By default, Islandora Workbench requires user credentials that have administrator-level permissions in the target Drupal. However, it is possible to avoid giving the user the administrator role; the steps required to do this are described in the "Using a non-administrator user" section below.

    In any case, you must exercise caution when storing the Workbench user's credentials. As described below, Workbench provides a variety of options for managing the Drupal credentials it requires.

    The choice of what role/level of privilegies to give the Workbench user, and how that user's credentials are managed, needs to be informed by your institution's cybersecurity policies and practices. Always consult with your local systems administrators before deciding which options to choose.

## Password management

Workbench configuration files must contain a `username` setting (unless you use an external credentials file, as described below), but you can provide the corresponding password in five ways, depending on your security policies:

1. in the `password` setting in your Workbench configuration file (least secure)
1. in the `ISLANDORA_WORKBENCH_PASSWORD` environment variable (more secure)
1. in response to a prompt when the user runs Workbench (more secure)
1. in a credentials file separate from the user's Workbench configuration file (more secure)
1. by encrypting the credentials file (most secure).

If the `password` setting is present in your configuration files, Workbench will use its value as the user password and will ignore the other two methods of providing a password. If the `password` setting is absent, Workbench will look for the `ISLANDORA_WORKBENCH_PASSWORD` environment variable and if it is present, use its value. If both the `password` setting and the `ISLANDORA_WORKBENCH_PASSWORD` environment variable are absent, Workbench will prompt the user for a password before proceeding.

## The `credentials_file_path` configuration setting

An alternative to including Drupal user credentials in the main Workbench configuration file is to store the `username` and `password` settings in a separate YAML file. To do this, replace the `username` and `password` config settings with the `credentials_file_path` setting, which specifies the location of the separate file:

For example, a Workbench config file that contains the `username` and `password` config settings looks like this:

```
host: https://myislanorahost.org
username: admin
password: islandora
input_dir: /some/directory/path
```

The `credentials_file_path` setting replaces `username` and `password`, pointing to the file containing the credentials:

```
host: https://myislanorahost.org
credentials_file_path: /opt/workbench/credentials.yml
# Optional, see below for details.
credentials_key_file_path: /home/mark/key.yml
input_dir: /some/directory/path
```

If `credentials_file_path` is present in the configuration file, Workbench reads the referenced file for the `username` and `password` settings:

```yaml
username: admin
password: islandora
```

The credentials file needs to be located where it is readable by the user running Workbench, such as their home directory. Or, the credentials file can be shared by multiple Workbench users as long as it is located where it is readable by those users. The only requiement is that all Workbench users who reference the credentials file need read permmissions on the file.

## Encrypting the credentials file

The credentials file can be encrypted, requiring the Workbench user to enter a decryption key to proceed. To do this:

1. Create a credentials file and configure its path as described in the previous section.
2. Encrypt the file using the `encrypt_credentials_file.py` script in Workbench's `scripts` directory: `python encrypt_credentials_file.py /path/to/the/creditials/file.yml`. Encrypting the credentials file overwrites the original version with the encrypted version.
3. The script will tell you what the encryption/decryption key for that file is. This key will be necessary for Workbench to read the file.
4. Workbench automatically detects if the credentials file registered in the `credentials_file_path` config setting is encrypted and if it is, will prompt the user to enter the key.

There is no option to store the key in the Workbench configuration file, but there are a couple of alternatives to having Workbench prompt the user for the key:

- Have your systems administrator add the key to the `ISLANDORA_WORKBENCH_ENCRYPTION_KEY` environment variable in your computer. If that environment variable is present, Workbench will not prompt for the key.
- If you systems administrator prefers storing the decryption key in a securely-located file instead of having Workbench prompt for the key or storing the key in an environment variable, they can put the key in a plain text file and register the path to that file in the `credentials_key_file_path` setting. Note that it is important that this key file be placed in a directory readable only by the current Workbench user, such as their home directory. If it is in a directory that multiple people can read, the security benefits of encrypting the credentials are reduced.

## The `include_password_in_rollback_config_file` configuration setting

By default, Workbench does not include the `password` value in rollback configuration files. If you want it to include that value, set `include_password_in_rollback_config_file` to `true` in the `create` task configuration file that generates the rollback.

## The `remove_password_from_config_file` configuration setting

If you include `remove_password_from_config_file: true` in your configuration file, Workbench will remove the `password` entry from the configuration file immediately after it reads it. All other configuration settings remain as they were in the original version of the file. Adding `remove_password_from_config_file: true` to your configuration files removes the security risk of having passwords exposed to anyone who can read the files, but will mean that, unless you add the `password` setting back into a configuration file prior to the next time you run Workbench using that configuration file, it will prompt you for the password as described above.

This setting will interfere with automated or scripted workflows since Workbench will not continue until the password is entered in response to the prompt. If you don't want to include `password` in your configuration files in these workflows, configure your `ISLANDORA_WORKBENCH_PASSWORD` environment variable to contain the password.

## Using a non-administrator user

If you prefer that the Drupal user indicated in Workbench's `username` config setting not have full administrator-role permissions, you will need to do the following:

1. Create a new Drupal role named, for example, "Workbench user", for non-admin users whose credentials you want to use in your config file.
2. Include `use_workbench_permissions: true` in your Workbench config file. If you do not include this setting, or set it to `false` (the default), Workbench will expect the user defined in `username` to have the administrator role.
3. Make sure Drupal is running version 1.2.0 or higher of the Islandora Workbench Integration module. If you put `use_workbench_permissions: true` in your config file, and run `--check`, Workbench will tell you if you need to update the Integration module.


Below is a sample "Workbench user" role configuration that you can use.

!!! warning
    The specific permissions required to use a non-administrator user depend on which modules are enabled on your Drupal site. The configuration file provided below only includes permissions required by core Islandora modules, content types, media types, and vocabularies. Custom content types, media types, and vocabularies will need to be added to the role separately. For example, if you have the Paragraphs module enabled, or you are using some vocabularies specific to your Drupal, you will need to add the relevant permissions to this role using Drupal's GUI tools.

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
 - 'administer taxonomy'
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

Once you have imported the role configuration, confirm that the role is showing up in Drupal by visiting Administration > People > Roles (`/admin/people/roles`). If "Workbench user" is in the list of available roles, it is ready to assign to your Workbench user. Don't forget to include `use_workbench_permissions: true` in your Workbench config file.