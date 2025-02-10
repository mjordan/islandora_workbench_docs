You can define some configuration settings by including them as command-line arguments to Workbench. If the command-line setting exists in your configuration file, it will override the value in the file; if it doesn't exist in your configuration file, it is added to the configuration as if it were in the config file. The settings available as command-line arguements are:

- `input_dir`
- `input_csv`
- `google_sheets_gid`
- `log_file_path`
- `rollback_csv_file_path`
- `rollback_config_file_path`
- `csv_start_row`
- `csv_stop_row`
- `recovery_mode_starting_from_node_id`

In all cases, you need to prepend the setting with `--` to conform with Python's command-line argument syntax. For example, if you want to specify an input CSV file different from the one registered in your configuration file, include `--input_csv` as a command-line argument to Worbkench:

`python workbench --config example.yml --input_csv alternate.csv`

To specify a log file path, include `--log_file_path` as an argument:

`python workbench --config example.yml --check --log_file_path custom_log_during_check.log`
