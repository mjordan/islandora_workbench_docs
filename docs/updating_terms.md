You can update existing taxonomy terms in an `update_terms` tasks by providing a CSV file with a `term_id` column plus field data you want to update. The type of update is determined by the value of the `update_mode` configuration option:

* `replace` (the default) will replace all existing values in a field with the values in the input CSV.
* `append` will add values in the input CSV to any existing values in a field.
* `delete` will delete all values in a field.

Islandora Workbench will never allow a field to contain more values than the field's configuration allows. Attempts to update a field with more values than the maximum number allowed will result in the surplus values being ignored during the "update" task. If Workbench does this, it will write an entry to the log indicating it has done so.

The column headings in the CSV file other than `term_id` must match either the machine names of fields that exist in the target Islandora content type, or their human-readable labels, with exceptions for the following fields: `term_name`, `description`, `parent`, and `weight` (more information about these fields is available in the "[Creating taxonomy terms](/islandora_workbench_docs/creating_taxonomy_terms/)" documentation). Only include fields that you want to update. Currently, fields with the data types as described in the "[Fields](/islandora_workbench_docs/fields/)" documentation can be updated.

For example, using the fields defined by the Islandora Defaults module for the "Person" vocabulary, your CSV file could look like this:

```text
term_id,term_name,description,field_authority_link
100,"Jordan, Mark",Mark Jordan's Person term.,http://viaf.org/viaf/106468077%%VIAF Record
```

The config file for update operations looks like this (note the `task` option is 'update'):

```yaml
task: update_terms
host: "http://localhost:8000"
username: admin
password: islandora
# vocab_id is required.
vocab_id: person
input_csv: update.csv
```
If you want to append the values in the CSV to values that already exist in the target nodes, add the `update_mode` configuration option:

```yaml
task: update_terms
host: "http://localhost:8000"
username: admin
password: islandora
vocab_id: person
input_csv: update.csv
update_mode: append
```

Some things to note:

* The `vocab_id` config setting is required.
* The `update_mode` applies to all rows in your CSV; it cannot be specified for particular rows.
* Updates apply to entire fields. Workbench cannot replace individual values in field.
* Values in the `term_id` column can be numeric term IDs (e.g. `467`) or string (e.g. `Dogs`). If a string, it must match the existing term identically other than for trailing and leading whitespace. In tasks where you want to *update* the values in `term_name`, you should use `term_id` to identify the term entity.
* For `update` tasks where the `update_mode` is "replace" or "append", blank/empty CSV values will do nothing; in other words, empty CSV values tell Workbench to *not* update the field.
* For `update` tasks where the `update_mode` is "delete", it doesn't matter if the column(s) in the input CSV are blank or contain values - the values in the corresponding Drupal fields are deleted in both cases.
