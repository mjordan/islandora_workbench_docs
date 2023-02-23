!!! note
    This section describes using CSV *field* templates in your configuration file. For information on CSV *value* templates, see "[CSV value templates](/islandora_workbench_docs/csv_value_templates/)". For information on CSV *file* templates, see the "[CSV file templates](/islandora_workbench_docs/generating_csv_files/#csv-file-templates)" section.

In `create` and `update` tasks, you can configure field templates that are applied to each node as if the fields were present in your CSV file. The templates are configured in the `csv_field_templates` option. An example looks like this:

```text
csv_field_templates:
 - field_rights: "The author of this work dedicates any and all copyright interest to the public domain."
 - field_member_of: 205
 - field_model: 25
 - field_tags: 231|257
```

Values in CSV field templates are structured the same as field values in your CSV (e.g., in the example above, `field_tags` is multivalued), and are validated against Drupal's configuration in the same way that values present in your CSV are validated.

If a column with the field name used in a template is present in the CSV file, Workbench ignores the template and uses the data in the CSV file. If a column listed in the `ignore_csv_columns` [setting](/islandora_workbench_docs/ignoring_csv_rows_and_columns/#ignoring-csv-columns), the value from the template is used.
