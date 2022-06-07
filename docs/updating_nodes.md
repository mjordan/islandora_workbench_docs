You can update existing nodes by providing a CSV file with a `node_id` column plus field data you want to update. The type of update is determined by the value of the `update_mode` configuration option:

* `replace` (the default) will replace all existing values in a field with the values in the input CSV.
* `append` will add values in the input CSV to any existing values in a field.
* `delete` will delete all values in a field.

Islandora Workbench will never allow a field to contain more values than the field's configuration allows. Attempts to update a field with more values than the maximum number allowed will result in the surplus values being ignored during the "update" task. If Workbench does this, it will write an entry to the log indicating it has done so.

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
If you want to append the values in the CSV to values that already exist in the target nodes, add the `update_mode` configuration option:

```yaml
task: update
validate_title_length: false
host: "http://localhost:8000"
username: admin
password: islandora
input_csv: update.csv
update_mode: append
```

Some things to note:

* The `update_mode` applies to all rows in your CSV; it cannot be specified for particular rows.
* Updates apply to entire fields. Workbench cannot replace individual values in field.
* Values in the `node_id` column can be numeric node IDs (e.g. `467`) or full URLs, including URL aliases.
* You should include `validate_title_length: false` in your update configuration file, unless you are updating node titles.
* If a node you are updating doesn't have a field named in your input CSV, Workbench will skip updating the node and add a log entry to that effect.
* For `update` tasks where the `update_mode` is "delete", it doesn't matter if the column(s) in the input CSV are blank or contain values - the values in the corresponding Drupal fields are deleted in both cases.
