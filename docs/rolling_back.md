In the `create` and `create_from_files` tasks, Workbench generates a configuration file and accompanying input CSV file in the format described in "Deleting nodes" documentation. These files allow you to easily roll back (i.e., delete) all the nodes and accompanying media you just created. Specifically, this configuration file defines a `delete` task. See the "[Deleting nodes](/islandora_workbench_docs/deleting_nodes/)" section for more information.

`--check` it will also write an entry in your log file indicating the location of the rollback config and CSV files, and it will test to ensure that the rollback config and CSV files can be written. If either one cannot, Workbench exists with an error.

!!! warning
    The rollback configuration file contains the username used in the `create` or `create_from_files` task that generates it. If you also want to include the accompanying password, add `include_password_in_rollback_config_file: true` to your configuration.

    Also note, rollback configuration and CSV files are overwritten every time you run a `create` or `create_from_files` task. It is highly recommended that you timestamp your rollback files (see below) to produce unique filenames.

By default, the configuration file is named "rollback.yml" and is written into the Workbench directory. The input CSV file is named "rollback.csv" and is written into the directory defined in your `input_dir` configuration setting. If either of these files exist, they are overwritten during the next `create` or `create_from_files` task.

You can also specify the relative (to workbench) or abolute path to your rollback config and CSV files, by including `rollback_config_file_path` and `rollback_csv_file_path`, respectively, in your configuration.

To roll back all the nodes and media you just created, run `./workbench --config rollback.yml`.

There are several configuration settings that let you control the names of these two files, and there is also an option to include comments in the files, as described below.

!!! note
    When secondary tasks are configured, each task will get its own rollback file. Each secondary task's rollback file will have a normalized version of the path to the task's configuration file appended to the rollback filename, e.g., `rollback.csv.home_mark_hacking_islandora_workbench_example_secondary_task`. Using these separate rollback files, you can delete only the nodes created in a specific secondary task.

## Setting the directory where the rollback CSV file is written

You can determine where the rollback CSV file is written by including the `rollback_dir` in your configuration. This overrides the default location defined in `input_dir`.

The rollback configuration file is always written to the Workbench working directory unless that behavior is overwritten by `rollback_config_file_path`.

## Using rollback filename templates

For both the rollback config file and the rollback CSV file, you can define a template that provides four placeholders:

* `$config_filename`, which contains the name of the `create` (or `create_from_files`) configuration file
* `$input_csv_filename`, which contain the name of the input CSV file (only available in `create` tasks)
* `csv_start_row`, which contains the value of the `csv_start_row` configuration setting, or "0" if none is set (only available in `create` tasks)
* `csv_stop_row`, which contains the value of the `csv_stop_row` configuration setting, or "None" if not set (only available in `create` tasks).

You can embed these placeholders in your template, which is then used to create the names of the two files. The templates for the two filenames are defined in two separate configuration settings. For the rollback configuration file, this setting is `rollback_config_filename_template`, e.g.:

```
rollback_config_filename_template: my-custom_filename_${config_filename}_$input_csv_filename
```
Assuming the configuration file for the `create` task that generates this rollback configuration has the name "mjtest.yml", and the `input_csv` filename has the filename "sample.csv", this template will result in a configuration file named `my-custom_filename_mjtest_sample.yml`. Note that the ".yml" extension is added automatically; you do not need to include it in your template.

For the rollback CSV file, the configuration setting is `rollback_csv_filename_template`, e.g.:

```
rollback_csv_filename_template: my_custom_rollback_filename_${config_filename}_$input_csv_filename
```

Using the same `create` task configuration filename and input CSV filename as in the previous example, this template will result in a CSV file named `my_custom_rollback_filename_mjtest_sample.csv`. The ".csv" extension is added automatically; you do not need to include it in your template.

!!! warning
    Python's built-in templating system has a quirk where when a character that is valid in a Python variable name follows a template placeholder, that character is added to the template placeholder. When templating filenames, the two most common characters of this type are `_` and `-`. When this happens, Workbench will exit with the error message "One or more parts of the configured rollback configuration filename template ([your template here]) need adjusting."

    You can work around this behavior by wrapping your template variable name, without the leading `$`, in `{}`. The example rollback config filename template above (`my-custom_filename_$config_filename_$input_csv_filename`) will trigger this error because the `_` following the `$config_filename` placeholder is valid in Python variable names. If you see this type of message, adjust your template to `my-custom_filename_${config_filename}_$input_csv_filename`.

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

## Including additional node-level data in the rollback CSV file

By default, rollback CSV files have only a single column, `node_id`. However, if you include `rollback_file_include_node_info: true` in your configuration, Workbench will add `id` (or whatever field is specified in the `id_field` config setting), `title`, `field_member_of`, and `file` columns containing relevant info for each created node.

You do not need to remove these columns from the rollback CSV file before using it.


## Rolling back a subset of the nodes identified in the rollback.csv file

If you want to roll back only a subset of nodes, simply remove the node IDs of the nodes you want to keep from the rollback.csv file, leaving the IDs of the nodes you want to delete.
