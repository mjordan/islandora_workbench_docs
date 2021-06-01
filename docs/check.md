## Overview

You should always (always, [I can't stress that enough](https://www.youtube.com/watch?v=2ZgtEdFAg3s)) check your configuration and input prior to creating, updating, or deleting content. You can do this by running Workbench with the `--check` option, e.g.:

`./workbench --config config.yml --check`

!!! note
    If you're on Windows, you will likely need to run Workbench by explicitly invoking Python, e.g. `python workbench --config config.yml --check` instead of using `./workbench` as illustrated above.

If you do this, Workbench will check the following and report any errors that require your attention before proceding:

* Configuration file
    * Whether your configuration file is valid YAML (i.e., no YAML syntax errors).
    * Whether your configuration file contains all required values.
* Connection to Drupal
    * Whether your Drupal has the required [Workbench Integration](https://github.com/mjordan/islandora_workbench_integration) module enabled.
    * Whether the `host` you provided will accept the `username` and `password` you provided.
* CSV file
    * Whether the CSV file is encoded in either ASCII or UTF-8.  
    * Whether each row contains the same number of columns as there are column headers.
    * Whether there are any duplicate column headers.
    * Whether your CSV file contains required columns headers, including the field defined as the unique ID for each record (defaults to "id" if the `id_field` key is not in your config file)
    * Whether your CSV column headers correspond to existing Drupal field machine names.
    * Whether all Drupal fields that are configured to be required are present in the CSV file.
    * Whether the columns required to create paged content are present (see "Creating paged content" below).
* Media files
    * Whether the files named in the CSV file are present, or in the case of remote files, are accessible (but this check is skipped if `allow_missing_files: true` is present in your config file for "create" tasks). If `nodes_only` is true, this check is skipped.
    * Whether the media types configured for specific file extensions are configured on the target Drupal. Islandora Workbench will default to the 'file' media type if it can't find another more specific media type for a file, so the most likely cause for this check to fail is that the assigned media type does not exist on the target Drupal.
* Field values
    * Base fields
        * If the `langcode` field is present in your CSV, whether values in it are valid Drupal language codes.
        * Whether your CSV file contains a  `title` field (`create` task only)
        * Whether values in the `title` field exceed Drupal's maximum length for titles of 255 characters (but this check is skipped if `validate_title_length` is set to `False`).
        * If the `created` field is present in your CSV file, whether the values in it are formatted correctly (like "2020-11-15T23:49:22+00:00") and whether the date is in the past (both of which are Drupal requirements).
        * If the `uid` field is present in your CSV file, whether the user IDs in that field exist in the target Drupal. Note that this check does not inspect permissions or roles, only that the user ID exists.
    * Whether aliases in the `url_alias` field in your CSV already exist, and whether they start with a leading slash (`/`).
    * Taxonomy
        * Whether term ID and term URIs used in CSV fields correspond to existing terms.
        * Whether the length of new terms exceeds 255 characters, which is the maximum length for a term name.
        * Whether the term ID (or term URI) provided for `media_use_tid` is a member of the "Islandora Media Use" vocabulary.
        * Whether term names in your CSV require a vocabulary namespace.
    * Typed Relation fields
        * Whether values used in typed relation fields are in the required format
        * Whether values need to be namespaced
        * Whether the term IDs/term names/term URIs used in the values exist in the vocabularies configured for the field.
    * If using the pages from directories configuration (`paged_content_from_directories: true`):
        * Whether page filenames contain an occurance of the sequence separator.
        * Whether any page directories are empty.
    * Whether multivalued fields exceed their allowed number of values.
    * Whether values in text-type fields exceed their configured maximum length.
    * Whether the nodes refrenced in `field_member_of` (if that field is present in the CSV) exist.
    * Whether values used in geolocation fields are valid lat,long coordinates.
    * Whether values used in EDTF fields are valid EDTF date/time values (subset of date/time values only; see [documentation](https://mjordan.github.io/islandora_workbench_docs/fields/#field-types) for more detail). Also validates whether dates are valid Gregorian calendar dates.
* Hook scripts
  * Whether registered bootstrap scripts exist and are executable.
  * Whether registered preprocessor scripts and are executable.
  * Whether registered post-action scripts and are executable.

If Workbench detects a configuration or input data violation, it will either stop and tell you why it stopped, or (if the violation will not cause Workbench's interaction with Drupal to fail), tell you that it found an anomoly and to check the log file for more detail.

## Typical (and recommended) Islandora Workbench usage

You will probably need to run Workbench using `--check` a few times before you will be ready to run it without `--check` and commit your data to Islandora. For example, you may need to correct errors in taxonomy term IDs or names, fix errors in media filenames, or wrap values in your CSV files in quotation marks.

It's also a good idea to check the Workbench log file after running `--check`. All warnings and errors are printed to the console, but the log file may contain additional information or detail that will help you resolve issues.

Once you have used `--check` to detect all of the problems with your CSV data, committing it to Islandora will work very reliably.

Also, it is good practice to check your log after each time you run Islandora Workbench, since it may contain information that is not printed to the caonsole. 
