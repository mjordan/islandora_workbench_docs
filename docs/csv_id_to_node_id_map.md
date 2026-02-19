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


The SQLite database contains one table, "csv_id_to_node_id_map". This table has seven columns:

* `timestamp`: the current timestamp in `yyyy-mm-dd hh:mm:ss` format or a truncated version of that format
* `config_file`: the name of the Workbench configuration file active when the row was added
* `parent_csv_id`: if the node was created during the same `create` job as its parent, the parent's CSV ID
* `parent_node_id`: if the node was created during the same `create` job its parent, the parent's node ID
* `csv_id`: the value in the node's CSV ID field (or in `create_from_files` tasks, which don't use an input CSV file, the filename); when creating paged content from directories using the `paged_content_from_directories: true` setting, the filename is recorded instead of an "id".
* `node_id`: the node's Drupal node ID
* `host`: the value of the `host` config setting at the time a row was added to the table. For rows added  prior to June 2025, this value will be blank/empty, or, if you are performing SQLite queries against this field, its value is `null`.

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

### "host" values in the map

!!! note
    The only situation where you should use the `csv_id_to_node_id_map_allowed_hosts` setting described in this section is if you have changed the host name of your Islandora server (e.g. from `https://myislandoraserver-dev` to `https://myislandoraserver`) and want to use node IDs created prior to the name change. Otherwise, you can ignore the `csv_id_to_node_id_map_allowed_hosts` setting.

Up until early June 2025, the CSV ID to node ID map contained node IDs but not the hostname of the Islandora repository those node IDs exist in. This lack of hostnames in the map means that if Workbench has been used with more than one host ("host" here means the hostname defined in your config file's `host` setting) there is a chance that a given node ID could exist in more than one Drupal instance. If the same node ID from separate Drupal instances is used as a parent node ID, that "member of" relationship would be entirely invalid -- the child's parent could be an unrelated node that happened to have one of the overlapping IDs.

No Workbench user has reported this situation, and Workbench uses the CSV ID to node ID map in ways that minimize the risk of it happening (e.g. during recovery mode by ignoring rows in the map that have a node ID lower than the one specified). Starting in June 2025, Workbench populates the CSV ID to node ID map with the value of the `host` configuration setting in order to further safeguard against overlapping node IDs. This change will be completely transparent to most users, but does introduce a couple of changes in behavior that you should be aware of:

- During `--check`, if Workbench finds any "host" values in your CSV ID to node ID map other than "" (empty) (which would be the case for all entries created prior to this change) or equal to your current confifguration's "host" setting, it will warn you that your map contains additional "host" values and ask you to review your log file. The log file will refer you to this page for more advice on what to do (see below).
- The message will look similar to `Warning: There are values for the "host" column in the CSV ID to node ID map at "./csv_id_to_node_id_map.db". Please see your workbench log for more information.`
- The referenced log entry will list the hostnames that are not either "" (empty) or the hostname identified in your current `host` config setting.

What this means:

- If you have not been using Workbench (or more specifically, not been using the same CSV ID to node ID map file) across multiple Drupal instances, you don't need to do anything. None of the node IDs in the map refer to another Drupal instance.
- If Workbench tells you there are "host" values in your map other than "empty" or the current value of your configuration's `host` setting, and you want Workbench to ignore the node IDs in the rows with those other hostnames (e.g., they are hostname for test or dev servers), you don't need to do anything.
- The only situation where you would need to use the `csv_id_to_node_id_map_allowed_hosts` config setting is if you *do* want to use the node IDs in the rows with the extra hostnames. For example, if you created content in a Drupal instance with the hostname `https://awesomeness-dev.net` prior to launch and then in preparation for launch changed that server's hostname to `https://awesomeness.net`. In this case, you would include in your config file the `csv_id_to_node_id_map_allowed_hosts` setting, listing all the hosts you want to include in the node ID lookups, e.g. `csv_id_to_node_id_map_allowed_hosts: ["", "https://awesomeness-dev.net", "https://awesomeness.net"]`. The `""` is for rows in the map created before June 2025; if you want to ignore rows added prior to then, you can omit it.

Note that:

- Including `csv_id_to_node_id_map_allowed_hosts` in your config file replaces the default value of `["", the value of the "host" configuration setting]`, so you should always include `""` (for empty "host" values in the map) plus your current `host` configuration setting value, plus any additional hostnames that apply.
- The hostnames are "or'ed" in the queries against tha CSV ID to node ID map, meaning that a match on any of the entries in the `csv_id_to_node_id_map_allowed_hosts` will be used in queries against the map.






