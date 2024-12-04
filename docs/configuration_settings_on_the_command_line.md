You can override the following settings in configuration files by including them as command-line arguments to Workbench:

- `input_dir`
- `input_csv`
- `google_sheets_gid`
- `log_file_path`
- `rollback_csv_file_path`
- `rollback_config_file_path`
- `csv_start_row`
- `csv_stop_row`

In all cases, you need to add `--` to conform with Python's command-line argument syntax.

For example, if you want to specify an input CSV file different from the one registered in your configuration file, include `--input_csv` as a command-line argument to Worbkench:

`python workbench --config example.yml --input_csv alternate.csv`

To specify a log file path, include `--log_file_path` as an argument:

`python workbench --config example.yml --check --log_file_path custom_log_during_check.log`
