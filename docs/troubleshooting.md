
!!! note
    If you are encountering problems not described here, please [open an issue](https://github.com/mjordan/islandora_workbench/issues) and help improve Islandora Workbench!

## Workbench is failing to ingest some nodes and is leaving messages in the log mentioning HTTP response code 422.

This is probably caused by unexpected data in your CSV file that Workbench's `--check` validation is not finding. If you encounter these messages, please [open an issue](https://github.com/mjordan/islandora_workbench/issues) and share any relevant entries in your Drupal log (as an admin user, go to Admin > Reports > Recent log messages) so we can track down the problem.

One of the most common causes of this error is that one or more of the vocabularies being populated in your CSV contain required fields other than the default term name, something which Islandora Workbench [cannot currently handle](https://github.com/mjordan/islandora_workbench/issues/111). While it is possible to have Workbench create these fields, the additional fields will need to be included in the input CSV. If you have an opinion on how that can be best done (while keeping the CSV relatively human-readable), please leave a comment on the linked Github issue.

## --check is telling me that one the rows in my CSV file has more columns than headers.

The most likely problem is that one of your CSV values contains a comma but is not wrapped in double quotes. 

## Workbench is slow.

True, it can be slow. However, users have found that the following strategies increase Workbench's speed substantially:

* Running Workbench on the same server that Drupal is running on (e.g. using "localhost" as the value of `host` in your config file). While doing this negates Workbench's most important design principle - that it does not require access to the Drupal server's command line - during long-running jobs such as those that are part of migrations, this is the best way to speed up Workbench.
* Using local instead of remote files. If you populate your `file` or "additional files" fields with filenames that start with "http", Workbench downloads each of those files before ingesting them. Providing local copies of those files in advance of running Workbench will eliminate the time it takes Workbench to download them.
* Avoid confirming taxonomy terms' existence during `--check`. If you add `validate_terms_exist: false` to your configuration file, Workbench will not query Drupal for each taxonomy term during `--check`. This option is suitable if you know that the terms don't exist in the target Drupal. Note that this option only speeds up `--check`; it does not have any effect when creating new nodes.

## The text in my CSV does not match how it looks when I view it in Drupal.

If a field is configured in Drupal to use [text filters](https://www.drupal.org/node/213156), the HTML that is displayed to the user may not be exactly the same as the content of the node add/edit form field. If you check the node add/edit form, the content of the field should match the content of the CSV field. If it does, it is likely that Drupal is apply a text filter. See [this issue](https://github.com/mjordan/islandora_workbench/issues/328) for more information.

## My Islandora uses a custom media type and I need to tell Workbench what file field to use.

If you need to create a media that is not one of the standard Islandora types (Image, File, Digital Document, Video, Audio, Extracted Text, or FITS Technical metadata), you will need to include the  `media_file_fields` setting in your config file, like this:


```yaml
media_file_fields:
 - mycustommedia_machine_name: field_custom_file
 - myothercustommedia_machine_name: field_other_custom_file
```

This configuration setting adds entries to the following default mapping of media types to file field names:

```yaml
'file': 'field_media_file',
'document': 'field_media_document',
'image': 'field_media_image',
'audio': 'field_media_audio_file',
'video': 'field_media_video_file',
'extracted_text': 'field_media_file',
'fits_technical_metadata': 'field_media_file'
```

## I've pulled in updates to Islandora Workbench from Github but when I run it, Python complains about not being able to find a library.

This is likely due to the addition of a new Python library to Workbench. Rerunning `setup.py` will install the missing library. Details are available in the "Updating Islandora Workbench" section of the [Requirements and Installation](https://mjordan.github.io/islandora_workbench_docs/installation/#updating-islandora-workbench) docs.

## EDTF 'interval' values are not rendering in Islandora properly.

Islandora can display EDTF interval values (e.g., `2004-06/2006-08`, `193X/196X`) properly, but by default, the configuration that allows this is disabled (see [this issue](https://github.com/Islandora/documentation/issues/1889) for more information). To enable it, for each field in your Islandora content types that use EDTF fields, visit the "Manage form display" configuration for the content type, and for each field that uses the "Default EDTF widget", within the widget configuration (click on the gear), check the "Permit date intervals" option and click "Update":

![EDTF form widget configuration](images/edtf_form_widget_config.png)

## My CSV file has a `url_alias` column, but the aliases are not being created.

First thing to check is whether you are using the [Pathauto](https://www.drupal.org/project/pathauto) module. It also creates URL aliases, and since by default Drupal only allows one URL alias, in most cases, the aliases it creates will take precedence over aliases created by Workbench.
