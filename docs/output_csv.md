Islandora Workbench can generate several different CSV files you might find useful.

### CSV file templates

!!! note
    This documentation describes creating CSV *file* templates. For information on CSV *field* templates, see the "[Using CSV field templates](/islandora_workbench_docs/field_templates/)" section.

You can generate a template CSV file by running Workbench with the `--get_csv_template` argument:

`./workbench --config config.yml --get_csv_template`

With this option, Workbench will fetch the field definitions for the content type named in your configuration's `content_type` option and save a CSV file with a column for each of the content type's fields. You can then populate this template with values you want to use in a `create` or `update` task. The template file is saved in the directory indicated in your configuration's `input_dir` option, using the filename defined in `input_csv` with `.csv_file_template` appended.

The template also contains three additional rows:

* human-readable label
* whether or not the field is required in your CSV for `create` tasks
* sample data
* number of values allowed (either a specific maximum number or 'unlimited')
* the name of the section in the documentation covering the field type

Here is a screenshot of this CSV file template loaded into a spreadsheet application:

![CSV file template](images/csv_file_template.png)

Note that the first column, and all the rows other than the field machine names, should be deleted before you use a populated version of this CSV file in a `create` task. Also, you can remove any columns you do not intend on populating:

![CSV file template ready to use](images/csv_file_template_ready_to_use.png)

### Creating a CSV file containing a row for every new node

In some instances, you may want to create stub nodes that only have a small subset of fields, and then populate the remaining fields later. To facilitate this type of workflow, Workbench provides an option to generate a simple CSV file containing a record for every newly created node. This file can then be used later in `update` tasks to add additional metadata or in `add_media` tasks to add media.

 You tell Workbench to generate this file by including the optional `output_csv` setting in your configuration file. If this setting is present, Workbench will write a CSV file at the specified location containing one record per node created. This CSV file contains the following fields:

 * `id` (or whatever column is specified in your `id_field` setting): the value in your input CSV file's ID field
 * `node_id`: the node ID for the newly created node
 * `uuid`: the new node's UUID
 * `status`: true if the node is published, False if it is unpublished
 * `title`: the node's title

 The file will also contain empty columns corresponding to all of the fields in the target content type. An example, generated from a 2-record input CSV file, looks like this (only left-most part of the spreadsheet shown):

 ![Output CSV](images/output_csv.png)

This CSV file is suitable as a template for subsequent `update` tasks, since it already contains the `node_id`s for all the stub nodes plus column headers for all of the fields in those nodes. You can remove from the template any columns you do not want to include in your `update` task. You can also use the node IDs in this file as the basis for later `add_media` tasks; all you will need to do is delete the other columns and add a `file` column containing the new nodes' corresponding filenames.

If you want to include in your output CSV all of the fields (and their values) from the input CSV, add `output_csv_include_input_csv: true` to your configuration file. This option is useful if you want a CSV that contains the node ID and a field such as `field_identifier` or other fields that contain local identifiers, DOIs, file paths, etc. If you use this option, all the fields from the input CSV are added to the output CSV; you cannot configure which fields are included.

### Exporting field data into a CSV file

In development. See [this Github issue](https://github.com/mjordan/islandora_workbench/issues/425).