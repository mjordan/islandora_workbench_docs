# The CSV ID to node ID map

### What the map does, and how it works

By default, Workbench maintains a simple database that maps values in your CSV's ID column (or whatever column you define in the `id_field` config setting) to node IDs created in `create` tasks. Every time Workbench creates a node, it writes a row corresponding to that node to this database. Workbench subsequently reads this data while operating in [recovery mode](/islandora_workbench_docs/recovery_mode/), and uses it to determine the node ID of parent nodes named in the `parent_id` column when creating paged and compound content [across Workbench sessions](/islandora_workbench_docs/paged_and_compound/#creating-parentchild-relationships-across-workbench-sessions) and during [secondary tasks](/islandora_workbench_docs/paged_and_compound/#using-a-secondary-task).

You may find other uses for this data. Since it is stored in an embedded SQLite database, it can be queried using SQL, or can be dumped using into a CSV file using the `manage_csv_to_node_id_map.py` script provided in Workbench's `scripts` directory. Information on using this script is provided below.

!!! note
    You do not need to install anything extra for Workbench to create this database. Workbench provides a utility script, `manage_csv_to_node_id_map.py` (described below), for exporting and pruning the data. You only need to install the `sqlite3` client or a third-party utility if you want to access the database in ways that surpass the capabilities of the `manage_csv_to_node_id_map.py` script.

    A useful third-party tool for viewing and modifying SQLite databases is [DB Browser for SQLite](https://sqlitebrowser.org/). Here is a sample screenshot illustrating the CSV to node ID map database table in DB Browser for SQLite (CSV ID and node ID are the two right-most columns):

    ![CSV to node ID map sample data in DB Browser](images/sqlite_db_browser.png)

[Several configuration settings](/islandora_workbench_docs/configuration/#csv-id-to-node-id-map-settings) apply to the CSV ID to node ID map. The most important determine where the database file is located. By default, its value is `[your temporary directory]/csv_id_to_node_id_map.db`. Additional information on defining the database file's location is provided below.

!!! warning
    Some systems clear out their temporary directories on restart. You may want to define the path to your ID map database in the `csv_id_to_node_id_map_dir` configuration setting so it is stored in a location that will not get deleted on system restart, and, if you wish, define the name of the database file in the `csv_id_to_node_id_map_filename` configuration setting as well. More information on doing this is provided below.


The SQLite database contains one table, "csv_id_to_node_id_map". This table has five columns:

* `timestamp`: the current timestamp in `yyyy-mm-dd hh:mm:ss` format or a truncated version of that format
* `config_file`: the name of the Workbench configuration file active when the row was added
* `parent_csv_id`: if the node was created along with its parent, the parent's CSV ID
* `parent_node_id`: if the node was create along with its parent, the parent's node ID
* `csv_id`: the value in the node's CSV ID field (or in `create_from_files` tasks, which don't use an input CSV file, the filename); when creating paged content from directories using the `paged_content_from_directories: true` setting, the filename is recorded instead of an "id".
* `node_id`: the node's Drupal node ID

### Extracting data from and maintaining the map

As noted above, you can use SQLite to query the CSV ID to node ID map, but if you don't want to query the database directly, you can use `scripts/manage_csv_to_node_id_map.py` to:

* Export the contents of the entire database to a CSV file.
    * To do this, in the Workbench directory, run the script, specifying the path to the database file and the path to the CSV output: `python scripts/manage_csv_to_node_id_map.py --db_path /tmp/csv_id_to_node_id_map.db --csv_path /tmp/output.csv`
* Export the rows that have duplicate (i.e., identical) CSV ID values, or duplicate values in any specific field.
    * To limit the rows that are dumped to those with duplicate values in a specific database field, add the `--nonunique` argument and the name of the field, e.g., `--nonunique csv_id`. The resulting CSV will only contain those entries from your database.
* Delete entries from the database that have a specific value in their `config_file` column.
    * To do this, provide the script with the `--remove_entries_with_config_files` argument, e.g., `python scripts/manage_csv_to_node_id_map.py --db_path csv_id_to_node_id_map.db --remove_entries_with_config_files create.yml`. You can also specify a comma-separated list of config file names (for example `--remove_entries_with_config_files create.yml,create_testing.yml`) to remove all entries with those config file names with one command.
* Delete entries from the database create before a specific date.
    * To do this, provide the script with the `--remove_entries_before` argument, e.g., `python scripts/manage_csv_to_node_id_map.py --db_path csv_id_to_node_id_map.db --remove_entries_before "2023-05-29 19:17"`.
* Delete entries from the database created after a specific date.
    * To do this, provide the script with the `--remove_entries_after` argument, e.g., `python scripts/manage_csv_to_node_id_map.py --db_path csv_id_to_node_id_map.db --remove_entries_after "2023-05-29 19:17"`.


The value of the `--remove_entries_before` and `--remove_entries_after` arguments is a date string that can take the form `yyyy-mm-dd hh:mm:ss` or any truncated version of that format, e.g. `yyyy-mm-dd hh:mm`, `yyyy-mm-dd hh`, or `yyyy-mm-dd`. Any rows in the database table that have a `timestamp` value that matches the date value will be deleted from the database. Note that if your timestamp value has a space in it, you need to wrap it quotation marks as illustrated above; if you don't, the script will delete all the entries on the timestamp value before the space, in other words, that day.


### Defining the location of your CSV ID to node ID map file

As noted above, the default location for the CSV ID to node ID map database file is in the computer's temporary directory. It is a good idea to define the location of this file somewhere where 1) it will not be delete on system restart and 2) optionally, it is accessible by multiple Workbench users.

Some systems, for example Linux distibutions based on Ubuntu, wipe the `/tmp` directory on reboot. If your map database file is in that directory, all of its contents will be lost.

On many Windows computers, the system temporary directory is specific to each user (e.g. `c:\users\mjordan\AppData\Local\Temp`). If you are using Islandora Workbench in an environment where multiple users are using it to create content, you may want to have all users share a common map database file, for example on a shared Windows drive.

```
# '.' is the current Workbench directory, but any relative or absolute path is OK.
csv_id_to_node_id_map_dir: .
csv_id_to_node_id_map_filename: mymap.db
```

The value of `csv_id_to_node_id_map_dir` doesn't have to be `.` (Workbench's current directory). Any directory path is valid, as long as it exists and is writable by the user running Workbench. If you are working in a distributed environment, where multiple people are running Workbench, the directory and file need to be writable by all users who run Workbench.

You can easily move a database file into another directory (from the default temporary directory location) as long as you configure Workbench to use that new location going forward. You can even combine the data in multiple database files by exporting the data to CSV from a database and then importing it into the consolidated database using commonly available SQLite3 utilities.