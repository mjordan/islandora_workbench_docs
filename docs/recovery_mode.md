# Recovering from interrupted "create" tasks

Islandora Workbench provides a way to recover from `create` tasks that fail part way through, whether the failure is caused by a fatal exception in Workbench, a network or Drupal server timeout, or as the result of [cancelling Workbench](/islandora_workbench_docs/cancelling). Using this "recovery mode" does not require any action _prior_ to the interruption; you only use it _after_ your job has been interrupted.

Assuming that your [CSV ID to node ID map](/islandora_workbench_docs/csv_id_to_node_id_map/) has been populuated during the `create` task that was interrupted (it will be by default), all you need to do to use recovery mode is to add a single configuration setting to the config file of the task that failed indicating the node ID of the _first_ node created during the failed job and then rerun the same job using the updated configuration file:

`recovery_mode_starting_from_node_id: 12678`

In this example, the first node created during the `create` job that failed has a node ID of 12687.

!!! note
    The first node ID in the failed job can be found in the first entry in your Workbench log.

When running in recovery mode, for each row in the input CSV, Workbench queries the CSV ID to node ID map using the row's "id" value. If it finds an entry in the map (because the node has already been created) it skips that row; if it doesn't find a row's ID in the map (because that item's entry in the map doesn't exist yet), it creates the node as usual using data in the CSV row. This works for input CSV where each row will produce one node, for compound items where the parent and all child items have their own rows and are linked via `parent_id` values, and for paged content created from directories. Because recovery mode is based on the existence of an item's entry in the CSV ID to node ID map, the same job can be interrupted and resumed multiple times until all the rows in the input CSV have corresponding entries in the map.

The node ID of the first node created during the successful part of the task, specified in the `recovery_mode_starting_from_node_id` configuration setting, tells Workbench  to filter out rows in its query against the map that may have older duplicate CSV IDs, added during previous, unrelated `create` tasks.

To summarize, to use recovery mode, you do not need to remove rows from the input CSV, remove files from paged content directories, or modify your input data in any other way. All you need to do is provide the node ID of the first node in the interrupted job using the `recovery_mode_starting_from_node_id` config setting.

!!! warning
    In order for recovery mode to work, the rows for the items successfully ingested during the interrupted `create` task need to be in the CSV ID to node ID map. To prepare for using recovery mode, you should move the map's database file out of its default location with your temporary directory into a location that will ensure that the file will not be deleted on system reboot. Information on doing this is available in the documentation for the [CSV ID to node ID map](/islandora_workbench_docs/csv_id_to_node_id_map/#defining-the-location-of-your-csv-id-to-node-id-map-file).

A few notes about recovery mode:

-  As an alternative to changing the configuration file of the failed job, you can also provide `recovery_mode_starting_from_node_id` as a command-line argument to Workbench, e.g. `./workbench --config mycreatejob.yml --recovery_mode_starting_from_node_id 12687`.
- In recovery mode, Workbench will create a new rollback configuration and CSV file using the same naming configuration but will add both a timestamp and "recovery_mode" to the filenames. This is ensure that you do not overwrite the rollback files created during the earlier, interrupted part of your job.
- In rare circumstances, a node may be created but not registered in the CSV ID to node ID map before Workbench stops executing. If this happens, the last node created prior to the interruption may be created again (duplicated) when you restart your job in recovery mode. It is always a good idea to check the "boundary" between the end of the interrupted part of the job and the start of the resumed recovery-mode part of the job, and if necessary, delete the duplicate node.
- A related caveat applies to media attached to a boundary node: if the job was interrupted before the media attached to a node were created, the media will not be created when you resume your job in recovery mode.
