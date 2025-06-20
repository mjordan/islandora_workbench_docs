
This page lists a number of things you can do to optimize your use of Islandora Workbench. Following these recommendations will decrease the liklihood that you will encounter problems, and increase value you get out of Workbench.

If you have found a way to improve your use of Workbench that you think others might want to know about, share it!

## Keep your copy of Workbench up to date

Islandora Workbench is under active development, both in terms of bug fixes and new features. It's good practice to [update your copy of Workbench](/islandora_workbench_docs/installation/#updating-islandora-workbench) every couple of weeks.

During `--check`, Workbench automatically checks to see if it is up to date, and logs the results. If you want to turn this off, include `check_for_workbench_updates: false` in your configuration file.

## Protect user credentials in your config files

Workbench requires the credentials of a Drupal user. You should consult with your local IT group to determine [which option](https://mjordan.github.io/islandora_workbench_docs/installation/#password-management) for managing the password of this user is their preference. You don't want Islandora Workbench to become a security vulnerability.

## Run `--check`

Running Workbench with its`--check` argument tells it to confirm that it can connect to Drupal, and to confirm that your input data is formatted as expected. It also performs validation on values in fields that have specific requirements, such as EDTF and typed relation fields. If `--check` passes, or only flags expected issues (such as files you know are missing), you reduce the likelihood that Workbench will encounter problems when you remove `--check` and commit your changes to your Islandora repository.

It can sometimes take several runs of `--check` for you to find and resolve all the issues it finds. The extra time this takes is worth it. You may want to try the `perform_soft_checks: true` config setting to tell `--check` to be more forgiving.

Don't forget to always look at your Workbench log file after running `--check`. It will likely contain useful detail that will help you identify potential problems.

## Run Workbench on a small sample first

Even if `--check` doesn't reveal any problems, running Workbench using a small set of sample input is a very good idea. Workbench provides a number of options for doing this, including processing only the first few rows of your input CSV, processing only the last few rows, processing specific rows, or processing only rows that meet a certain condtion. [This page](/islandora_workbench_docs/ignoring_csv_rows_and_columns/) provides more information on how to use these options.


## Break large jobs up into smaller jobs

Workbench is capable of processing a very large set of input data, but many users prefer to break up large jobs into several smaller subsets to decrease the chance of overloading their Drupal server, or to avoid possible network glitches, etc. Splitting up your input CSV into smaller files is one way of doing this, but Workbench's  "[Using CSV row ranges](https://mjordan.github.io/islandora_workbench_docs/ignoring_csv_rows_and_columns/#using-csv-row-ranges)" or "[Processing specific CSV rows](https://mjordan.github.io/islandora_workbench_docs/ignoring_csv_rows_and_columns/#processing-specific-csv-rows)" features let you use a single input CSV and define the subsets in your configuration file.

## Defer Solr indexing

Configuring Solr to not index newly added or updated items immediately can result in noticably faster `create` and `update` jobs. To do this, visit `/admin/config/search/search-api/index/default_solr_index/edit` and in the "Index options" fieldset, make sure "Index items immediately" is unchecked and save the form. Indexing will happen during cron runs. Thanks to [Born-Digital](https://www.born-digital.com/software/islandora) for suggesting this!

## Create a View that allows you to use the "Checking if nodes already exist" feature

Creating a View that you can use in conjunction with Workbench's `node_exists_verification_view_endpoint` config setting lets rerun the same job, using the same input CSV, multiple times without worrying about ingesting duplicate nodes. You will find this feature useful if your job crashes part way through. More information on creating this View, and confguring Workbench to use it, is available [here](/islandora_workbench_docs/checking_if_nodes_exist/).

## Use adaptive pause

[Adaptive pause](/islandora_workbench_docs/reducing_load/#adaptive-pause) is an effective way to decrease the likelihood that Workbench will time out due to excessive load on the Drupal server. Since it only pauses Workbench when needed, the few times it will introduce a small pause in execution time are well worth the increased reliability it introduces.

## Assume you will need the node IDs of items you create later

Having the node IDs generated during `create` tasks, and being able to relate them to a given Workbench job, is very useful, for example if you need to run an remedial `update` or `add_media` task on a large set of new nodes. Workbench provides a few ways to provide those node IDs.

### 1) The CSV ID to node ID map

Workbench's CSV ID to node ID map can make recovering from an interrupted `create` task very easy. You should configure your `create` tasks to use a directory for the map's database file that is *not* your system's temporary directory (which is the default). This is done using the `csv_id_to_node_id_map_dir` configuration setting. More information is available [here](/islandora_workbench_docs/csv_id_to_node_id_map/#defining-the-location-of-your-csv-id-to-node-id-map-file).


### 2) Rollback CSV files

You should always assume that you will need to roll back a set of nodes created during `create` tasks. Even if you don't need to do that, having the new node IDs easily accessible can be very convenient, for example if you need to do a remedial `update` task on the new nodes. Using the [rollback's configuration settings](/islandora_workbench_docs/rolling_back/), particulary the following two, can ensure that you have this data intact and ready to use:

- `timestamp_rollback: true`
- `rollback_file_include_node_info: true`

### 3) The `output_csv` setting

During `create` tasks, you can use the `output_csv` setting to tell Workbench to write a CSV file that contains the new node IDs. More information is available [here](/islandora_workbench_docs/generating_csv_files/#csv-file-containing-a-row-for-every-newly-created-node).

## Rely on recovery mode

Once you have configured Workbench to look for the CSV ID to node ID map in a [stable location](/islandora_workbench_docs/csv_id_to_node_id_map/#defining-the-location-of-your-csv-id-to-node-id-map-file), you ready to use [recovery mode](/islandora_workbench_docs/recovery_mode/)! You may never need to use it, but like insurance, you don't need it until you need it.

## Create local shortcuts and aliases

Some users create aliases for Workbench commands. How to do this depends on your operating system, but on Unix-based systems such as MacOS and Linux, a single command such as `alias wb='workbench --config=config.yml` can let you run `wb` many times without having to use the full command. Thanks to [rosiel](https://github.com/mjordan/islandora_workbench/issues/789#issuecomment-2189446853) for this tip.

## Specify configuration settings on the command line

Managing small changes in a configuration file over multiple runs of Workbench can be tedious. Some configuration settings can be [applied at the command line](/islandora_workbench_docs/configuration_settings_on_the_command_line/), which makes editing the configuration file unnecessary.


## Balance speed optimizations with convenience

Consider techniques for [speeding up Workbench](/islandora_workbench_docs/troubleshooting/#workbench-is-slow). However, they all come at the cost of some convenience. You decide which ones are worth it!


## Make Workbench tell you about its progress

Watching lines of output say that a node has been created is BORING. Try adding `show_percentage_of_csv_input_processed: true` or `progress_bar: true` to your config file to make using Workbench a little less boring.


## Use the `protected_vocabularies` setting

`allow_adding_terms` is an essential Workbench config setting. But, it can lead to problems. For example, you don't want a typo in the values in your `field_model` CSV column to create a new Islandora Model! To be very safe, you should include the `protected_vocabularies` setting in your configuration files to exclude which vocabularies Workbench will add new terms to. At Simon Fraser University Library, we use:

```yml
protected_vocabularies:
 - islandora_model
 - islandora_display
 - islandora_media_use
 - issuance_modes
 - resource_types
 - physical_form
 - genre
 - copyright
 - terms_of_use
 - concerns_corrections
```