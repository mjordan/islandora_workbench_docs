In the `create` and `create_from_files` tasks, Workbench generates a configuration file and accompanying input CSV file in the format described in "Deleting nodes" documentation. These files allow you to easily roll back (i.e., delete) all the nodes and accompanying media you just created. Specifically, this configuration file defines a `delete` task. See the "[Deleting nodes](/islandora_workbench_docs/deleting_nodes/)" section for more information.

!!! warning
    The rollback configuration file contains the username and password used in the `create` or `create_from_files` task that generates it.

By default, the configuration file is named "rollback.yml" and is written into the Workbench directory. The input CSV file is named "rollback.csv" and is written into the directory defined in your `input_dir` configuration setting. If either of these files exist, they are overwritten during the next `create` or `create_from_files` task.

To roll back all the nodes and media you just created, run `./workbench --config rollback.yml`.

There are several configuration settings that let you control the names of these two files, and there is also an option to include comments in the files.

!!! note
    When secondary tasks are configured, each task will get its own rollback file. Each secondary task's rollback file will have a normalized version of the path to the task's configuration file appended to the rollback filename, e.g., `rollback.csv.home_mark_hacking_islandora_workbench_example_secondary_task`. Using these separate rollback files, you can delete only the nodes created in a specific secondary task.

## Setting the directory where the rollback CVS file is written

You can determine where the rollback CSV file is written by including the `rollback_dir` in your configuration. This overrides the default location defined in `input_dir`.

The rollback configuration file is always written to the Workbench working directory.

## Using rollback filename templates

For both the rollback config file and the rollback CSV file, you can define a template that provides two placeholders, `$config_filename` and `$input_csv_filename`, which contain the names of the `create` (or `create_from_files`) configuration file and the name of the input CSV file used in `create` tasks. You can embed these two placeholders in your template, which is then used to create the names of the two files. The templates for the two filenames are defined in two separate configuration settings:

For the rollback configuration file:

```
rollback_config_filename_template: my-custom_filename_$config_filename_$input_csv_filename
```
Assuming the configuration file for the `create` task that generates this rollback configuration has the name "mjtest.yml", and the `input_csv` filename has the filename "sample.csv", this template will result in a configuration file named `my-custom_filename_mjtest_sample.yml`. Note that the ".yml" extension is added automatically; you do not need to include it in your template.

For the rollback CSV file:
```
rollback_csv_filename_template: my_custom_rollback_filename_$config_filename_$input_csv_filename
```
Using the same `create` task configuration filename and input CSV filename as in the previous example, this template will result in a CSV file named `my_custom_rollback_filename_mjtest_sample.csv`. The ".csv" extension is added automatically; you do not need to include it in your template.

!!! warning
    Python's built-in templating system has a quirk where when a character that is valid in a Python variable name follows a template placeholder, that character is added to the template placeholder. If this happens to you, Workbench will exit with the error message "One or more parts of the configured rollback configuration filename template ([your template here]) need adjusting."

    You can work around this behavior by wrapping your template variable in `{}`. The example rollback config filename template above (`my-custom_filename_$config_filename_$input_csv_filename`) will trigger this error because the `_` following the `$config_filename` placeholder is valid in Python variable names. If you see this type of message, adjust your template to `my-custom_filename_${config_filename}_$input_csv_filename`.

## Adding a timestamp to the rollback filenames

By default, Workbench overwrites the rollback configuration and CSV files each time it runs, so these files only apply to the most recent `create` and `create_from_files` runs. If you add `timestamp_rollback: true` to your configuration file, a (to-the-second) timestamp will be added to the `rollback.yml` and corresponding `rollback.csv` files, for example, `rollback.2024_11_03_21_10_28.yml` and `rollback.2024_11_03_21_10_28.csv`. The name of the CSV is also written to `workbench.log`. Running `./workbench --config rollback.2024_11_03_21_10_28.yml` will delete the nodes identified in `input_data/rollback.2024_11_03_21_10_28.csv`.

Timestamps are added in the same way to custom rollback configuration and CSV filenames create using templates.

## Adding custom comments to your rollback configuration and CSV files

Workbench always adds to lines of comments to rollback configuration and CSV files, indicating when the files were generated and the names of the configuration and input CSV files they were generated from, like this:

```
# Generated by a "create" task started 2024:11:10 09:52:57 using
# config file "mjtest.yml" and input CSV "sample.csv".
```

You can add additional, custom comment lines by including the `rollback_file_comments` configuration setting in your `create` or `create_from_files` configuration, like this:

```
rollback_file_comments:
  - Keep this file! It might be useful if something goes wrong with this job.
  - Have a nice day!
```

This will result in the following comments in your rollback configuration and CSV files:

```
# Generated by a "create" task started 2024:11:10 09:52:57 using
# config file "mjtest.yml" and input CSV "sample.csv".
# Keep this file! It might be useful if something goes wrong with this job.
# Have a nice day!
```

