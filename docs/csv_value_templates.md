!!! note
    This section describes using CSV *value* templates in your configuration file. For information on CSV *field* templates, see the "[CSV field templates](/islandora_workbench_docs/field_templates.md)". For information on CSV *file* templates, see the "[CSV file templates](/islandora_workbench_docs/generating_csv_files/#csv-file-templates)" section.

In `create` and `update` tasks, you can configure templates that are applied to the value (or to each subvalue if you have multiple values in a single field) as if the templated text were present in the values in your CSV file. The templates are configured in the `csv_value_templates` option. An example looks like this:

```text
csv_value_templates:
  - field_linked_agent: relators:aut:person:$csv_value
```

For each value in the named CSV field (in this example, `field_linked_agent`), Workbench will apply the template text (in this example, `relators:aut:person:`) to the CSV value, represented in the template as `$csv_value`. An input CSV file that looks like this:

```text
file,id,title,field_model,field_linked_agent
IMG_2940.JPG,03,Looking across Burrard Inlet,25,"Jordan, Mark|Cantelo, M."
```

will have the template `relators:aut:person:$csv_value` applied to so it is converted to:

```text
file,id,title,field_model,field_linked_agent
IMG_2940.JPG,03,Looking across Burrard Inlet,25,"relators:aut:person:Jordan, Mark|relators:aut:person:Cantelo, M."
```

A few things to note about CSV value templates:

* `$csv_value` is the only variable available in a template.
* Your templated text can be appended or prepended to `$csv_value`.
* If a CSV field contains multiple subvalues, the same template is applied to all subvalues in the field (as illustrated above).
* You can only apply a single template to the value(s) in the configured field.
* Values in the templated CSV output are validated against Drupal's configuration in the same way that values present in your CSV are validated.

Finally, `csv_value_templates` can define multiple field/template pairs, for example:

```text
csv_value_templates:
  - field_linked_agent: relators:aut:person:$csv_value
  - field_local_identifier: SFU-$csv_value
```
 
