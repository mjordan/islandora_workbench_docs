
!!! note
    If you are encountering problems not described here, please [open an issue](https://github.com/mjordan/islandora_workbench/issues) and help improve Islandora Workbench!

## Workbench is failing to ingest some nodes and is leaving messages in the log mentioning HTTP response code 422.

This is probably caused by unexpected data in your CSV file that Workbench's `--check` validation is not finding. If you encounter these messages, please [open an issue](https://github.com/mjordan/islandora_workbench/issues) and share your CSV input file and configuration file so we can track down the problem.

## --check is telling me that one the rows in my CSV file has more columns than headers

The most likely problem is that one of your CSV values contains a comma but is not wrapped in double quotes. 

