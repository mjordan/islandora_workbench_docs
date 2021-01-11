Islandora Workbench writes a log file for all tasks to `workbench.log` in the workbench directory, unless you specify an alternative log file location using the `log_file_path` configuration option, e.g.:

`log_file_path: /tmp/mylogfilepath.log`

The log contains information that is similar to what you see when you run Workbench, but with time stamps:

```text
24-Dec-20 15:05:06 - INFO - Starting configuration check for "create" task using config file create.yml.
24-Dec-20 15:05:07 - INFO - OK, configuration file has all required values (did not check for optional values).
24-Dec-20 15:05:07 - INFO - OK, CSV file input_data/metadata.csv found.
24-Dec-20 15:05:07 - INFO - OK, all 5 rows in the CSV file have the same number of columns as there are headers (5).
24-Dec-20 15:05:21 - INFO - OK, CSV column headers match Drupal field names.
24-Dec-20 15:05:21 - INFO - OK, required Drupal fields are present in the CSV file.
24-Dec-20 15:05:23 - INFO - OK, term IDs/names in CSV file exist in their respective taxonomies.
24-Dec-20 15:05:23 - INFO - OK, term IDs/names used in typed relation fields in the CSV file exist in their respective taxonomies.
24-Dec-20 15:05:23 - INFO - OK, files named in the CSV "file" column are all present.
24-Dec-20 15:05:23 - INFO - Configuration checked for "create" task using config file create.yml, no problems found.
```

It may also contain additional detail that would clutter up the console output, for example which term is being added to a vocabulary.

 By default, new entries are appended to this log, unless you indicate that the log file should be overwritten each time Workbench is run by providing the `log_file_mode` configuration option with a value of "w":

 `log_file_mode: w`

!!! note
    The only time that the default log file name is used instead of one defined in `log_file_path` is when Workbench detects that the specified configuration file is not valid YAML. In that case, Workbench can't parse the value of `log_file_path` so defaults to writing the details of the failed syntax check in `workbench.log`.
