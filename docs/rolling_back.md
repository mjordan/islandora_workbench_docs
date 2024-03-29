In the `create` and `create_from_files` tasks, Workbench generates a `rollback.yml` configuration file and a `rollback.csv` file in the format described in "Deleting nodes" documentation. These files allow you to easily roll back (i.e., delete) all the nodes and accompanying media you just created. Specifically, this configuration file defines a `delete` task. See the "[Deleting nodes](/islandora_workbench_docs/deleting_nodes/)" section for more information.

To roll back all the nodes and media you just created, run `./workbench --config rollback.yml`.

By default, Workbench overwrites the rollback configuration and CSV files each time it runs, so these files only apply to the most recent `create` and `create_from_files` runs. If you add `timestamp_rollback: true` to your configuration file, a (to-the-second) timestamp will be added to the `rollback.yml` and corresponding `rollback.csv` files, for example, `rollback.2021_11_03_21_10_28.yml` and `rollback.2021_11_03_21_10_28.csv`. The name of the CSV is also written to `workbench.log`. Running `./workbench --config rollback.2021_11_03_21_10_28.yml` will delete the nodes identified in `input_data/rollback.2021_11_03_21_10_28.csv`.

The rollback CSV file is written to the location defined in the `input_dir` setting. If you want to define another directory to put it in, include the `rollback_dir` setting in your config file.

!!! note
    When secondary tasks are configured, each task will get its own rollback file. Each secondary task's rollback file will have a normalized version of the path to the task's configuration file appended to the rollback filename, e.g., `rollback.csv.home_mark_hacking_islandora_workbench_example_secondary_task`. Using these separate rollback files, you can delete only the nodes created in a specific secondary task.
