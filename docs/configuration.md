## The configuration file

Workbench uses a YAML configuration whose location is indicated in the `--config` argument. This file defines the various options it will use to create, update, or delete Islandora content. The simplest configuration file needs only the following four options:

```yaml
task: create
host: "http://localhost:8000"
username: admin
password: islandora
```

In this example, the `task` being performed is creating nodes (and optionally media). Other tasks are `create_from_files`, `update`, `delete`, `add_media`, `update_media`, and `delete_media`. Some of the configuration settings documented below are used in all tasks, while others are only used in specific tasks.

## Configuration settings

The settings defined in a configuration file are documented in the tables below, grouped into broad functional categories for easier reference. The order of the options in the configuration file doesn't matter, and settings do not need to be grouped together in any specific way in the configuration file.

### Use of quotation marks

Generally speaking, you do not need to use quotation marks around values in your configuration file. You may wrap values in quotation marks if you wish, and many examples in this documentation do that (especially the `host` setting), but the only values that should not be wrapped in quotation marks are those that take `true` or `false` as values because in YAML, and many other computer/markup languages,`"true"` is a string (in this case, an English word that can mean many things) and `true` is a reserved symbol that can mean one thing and one thing only, the boolean opposite of `false` (I'm sorry for this explanation, I can't describe the distinction in any other way without writing a primer on symbolic logic).

For example, the following is a valid configuration file:

```yaml
task: create
host: http://localhost:8000
username: admin
password: islandora
nodes_only: true
csv_field_templates:
 - field_linked_agent: relators:aut:person:Jordan, Mark
 - field_model: 25
```

But the following version is not valid, since there are quotes around `"true"` in the `nodes_only` setting:

```yaml
task: create
host: http://localhost:8000
username: admin
password: islandora
nodes_only: "true"
csv_field_templates:
 - field_linked_agent: relators:aut:person:Jordan, Mark
 - field_model: 25
```

### Use of spaces and other syntactical features

Configuration setting names should start a new line and not have any leading spaces. The exception is illustrated in the values of the `csv_field_templates` setting in the above examples, where the setting's value is a list of other values. In this case the members of the list start with a dash and a space (`- `). The trailing space in these values is significant. (However, the leading space before the dash is insignificant, and is used for appearance only.) For example, this snippet is valid:

```yaml
csv_field_templates:
 - field_linked_agent: relators:aut:person:Jordan, Mark
 - field_model: 25
```

whereas this one is not:

```yaml
csv_field_templates:
 -field_linked_agent: relators:aut:person:Jordan, Mark
 -field_model: 25
```

Some setting values are represented in Workbench documentation using square brackets, like this one:

```yaml
export_csv_field_list: ['field_description', 'field_extent']
```

Strictly speaking, YAML lists can be represented as either a series of entries on their own lines that start with `- ` or as entries enclosed in `[` and `]`. It's best to follow the examples provided throughout the Workbench documentation.


## Config

!!swagger openapi.json!!


When you run Islandora Workbench with the `--check` argument, it will verify that all configuration options required for the current task are present, and if they aren't tell you so.

!!! note
    Islandora Workbench automatically converts any configuration keys to lowercase, e.g., `Task` is equivalent to `task`.

## Validating the syntax of the configuration file

When you run Workbench, it confirms that your configuration file is valid YAML. This is a syntax check only, not a content check. If the file is valid YAML, Workbench then goes on to perform a long list of application-specific [checks](/islandora_workbench_docs/check).

If this syntax check fails, some detail about the problem will be displayed to the user. The same information plus the entire Python stack trace is also logged to a file named "workbench.log" in the directory Islandora Workbench is run from. This file name is Workbench's default log file name, but in this case (validating the config file's YAML syntax), that file name is used regardless of the log file location defined in the configuration's `log_file_path` option. The reason the error is logged in the default location instead of the value in the configuration file (if one is present) is that the configuration file isn't valid YAML and therefore can't be parsed.

## Example configuration files

These examples provide inline annotations explaining why the settings are included in the configuration file. Blank rows/lines are included for readability.

### Create nodes only, no media

```yaml
task: create
host: "http://localhost:8000"
username: admin
password: islandora

# This setting tells Workbench to create nodes with no media.
# Also, this tells --check to skip all validation of "file" locations.
# Other media settings, like "media_use_tid", are also ignored.
nodes_only: true
```

### Use a custom log file location

```yaml
task: create
host: "http://localhost:8000"
username: admin
password: islandora

# This setting tells Workbench to write its log file to the location specified
# instead of the default "workbench.log" within the directory Workbench is run from.
log_file_path: /home/mark/workbench_log.txt
```

### Include some CSV field templates

```yaml
task: create
host: "http://localhost:8000"
username: admin
password: islandora

# The values in this list of field templates are applied to every row in the
# input CSV file before the CSV file is used to populate Drupal fields. The
# field templates are also applied during the "--check" in order to validate
# the values of the fields.
csv_field_templates:
 - field_member_of: 205
 - field_model: 25
```

### Use a Google Sheet as input CSV

```yaml
task: create
host: "http://localhost:8000"
username: admin
password: islandora
input_csv: 'https://docs.google.com/spreadsheets/d/13Mw7gtBy1A3ZhYEAlBzmkswIdaZvX18xoRBxfbgxqWc/edit
# You only need to specify the google_sheets_gid option if the worksheet in the Google Sheet
# is not the default one.
google_sheets_gid: 1867618389
```

### Create nodes and media from files (no input CSV file)

```yaml
task: create_from_files
host: "http://localhost:8000"
username: admin
password: islandora

# The files to create the nodes from are in this directory.
input_dir: /tmp/sample_files

# This tells Workbench to write a CSV file containing node IDs of the
# created nodes, plus the field names used in the target content type
# ("islandora_object" by default).
output_csv: /tmp/sample_files.csv

# All nodes should get the "Model" value corresponding to this URI.
model: 'https://schema.org/DigitalDocument'
```

### Create taxonomy terms

```yaml
task: create_terms
host: "http://localhost:8000"
username: admin
password: islandora
input_csv: my_term_data.csv
vocab_id: myvocabulary
```

### Ignore some columns in your input CSV file

```yaml
task: create
host: "http://localhost:8000"
username: admin
password: islandora
input_csv: input.csv

# This tells Workbench to ignore the 'date_generated' and 'batch_id'
# columns in the input.csv file.
ignore_csv_columns: ['date_generated', 'batch_id']
```

### Generating sample Islandora content

```yaml
task: create_from_files
host: "http://localhost:8000"
username: admin
password: islandora
# This directory must match the on defined in the script's 'dest_dir' variable.
input_dir: /tmp/autogen_input
media_use_tid: 17
output_csv: /tmp/my_sample_content_csv.csv
model: http://purl.org/coar/resource_type/c_c513
# This is the script that generates the sample content.
bootstrap:
 - "/home/mark/Documents/hacking/workbench/generate_image_files.py"
```

### Running a post-action script

```yaml
ask: create
host: "http://localhost:8000"
username: admin
password: islandora
node_post_create: ['/home/mark/hacking/islandora_workbench/scripts/entity_post_task_example.py']
# node_post_update: ['/home/mark/hacking/islandora_workbench/scripts/entity_post_task_example.py']
# media_post_create: ['/home/mark/hacking/islandora_workbench/scripts/entity_post_task_example.py']
```

### Create nodes and media from files

```yaml
task: create_from_files
host: "http://localhost:8000"
username: admin
password: islandora
input_dir: path/to/files
models:
 - 'http://purl.org/coar/resource_type/c_1843': ['zip', 'tar', '']
 - 'https://schema.org/DigitalDocument': ['pdf', 'doc', 'docx', 'ppt', 'pptx']
 - 'http://purl.org/coar/resource_type/c_c513': ['tif', 'tiff', 'jp2', 'png', 'gif', 'jpg', 'jpeg']
 - 'http://purl.org/coar/resource_type/c_18cc': ['mp3', 'wav', 'aac']
 - 'http://purl.org/coar/resource_type/c_12ce': ['mp4']
```
