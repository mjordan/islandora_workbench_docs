# Recovering from interrupted "create" tasks

Islandora Workbench provides a way to recover from `create` tasks that fail part way through, whether the failure is caused by a fatal exception in Workbench, a network or Drupal server timeout, or as the result of [cancelling Workbench](/islandora_workbench_docs/cancelling).

Assuming that your CSV ID to node ID map has been populuated during the `create` task that was interrupted (and it will be by default), all you need to do to use "recovery mode" is to add a single configuration setting to the config file of the task that failed indicating the node ID of the _first_ node created during the failed job:

`recovery_mode_starting_from_node_id: 12678`

In this example, the first node created during the `create` job that failed has a node ID of 12687.

Essentially, in recovery mode, you rerun the interrupted `create` task but prior to ingesting a node, Workbench queries the [CSV ID to node ID map](/islandora_workbench_docs/csv_id_to_node_id_map/). If it finds a row in the map (using the row's "id" value) it skips recreating it; if it doesn't find a row's ID in the map, it creates the node as usual. This works for input CSV where each row will produce one node, for compound items where the parent and all child items have their own rows, and for paged content created from directories.

The node ID of the first node created during the successful part of the task tells Workbench to perform its queries for CSV IDs starting at that node ID. Otherwise, a duplicate CSV ID in the map would prevent Workbench from reliably querying for that ID. The first node ID in the failed job can be found in your Workbench log.

To summarize, to use recovery mode, you do not need to remove rows from the input CSV or modify your input data in any other way. All you need to do is provide the node ID of the first node in the interrupted job using the `recovery_mode_starting_from_node_id` config setting.

!!! note
    As an alternative to changing the configuration file of the failed job, you can also provide `recovery_mode_starting_from_node_id` as a command-line argument to Workbench, e.g. `./workbench --config mycreatejob.yml --recovery_mode_starting_from_node_id 12687`.

!!! note
    Also, In recovery mode, Workbench will create a new rollback configuration and CSV file using the same naming configuration but will add both a timestamp and "recovery_mode" to the filenames.

!!! warning
    In order for recovery mode to work, the rows for the items successfully ingested during the interrupted `create` task need to be in the CSV ID to node ID map. To prepare for using recovery mode, you should move the map's database file out of its default location with your temporary directory into a location that will ensure that the file will not be deleted on system reboot. Information on doing this is available in the documentation for the [CSV ID to node ID map](/islandora_workbench_docs/csv_id_to_node_id_map/#defining-the-location-of-your-csv-id-to-node-id-map-file).
