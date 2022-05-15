You can update existing nodes by providing a CSV file with a `node_id` column plus field data you want to update. Updating replaces existing values in the nodes' fields, it don't append new values (but see [this issue](https://github.com/mjordan/islandora_workbench/issues/47)). Values in the `node_id` column can be numeric node IDs (e.g. `467`) or full URLs, including URL aliases.

The column headings in the CSV file other than `node_id` must match machine names of fields that exist in the target Islandora content type. Only include fields that you want to update. Currently, text fields, ETDF fields, taxonomy fields, linked node fields (e.g. "Member of" for collection nodes), typed relation, link, and geolocation fields can be updated following the same conventions used when creating nodes as described in the "[Fields](/islandora_workbench_docs/fields/)" documentation.

For example, using the fields defined by the Islandora Defaults module for the "Repository Item" content type, your CSV file could look like this:

```text
node_id,field_description,field_rights,field_access_terms,field_member_of
100,This is my new title,I have changed my mind. This item is yours to keep.,27,45
```

The config file for update operations looks like this (note the `task` option is 'update'):

```yaml
task: update
validate_title_length: false
host: "http://localhost:8000"
username: admin
password: islandora
input_csv: update.csv
```

Note that you should include `validate_title_length: false` in your update configuration file, unless you are updating node titles.
