!!! note
    This section describes using CSV *value* templates in your configuration file. For information on CSV *field* templates, see the "[CSV field templates](/islandora_workbench_docs/field_templates.md)". For information on CSV *file* templates, see the "[CSV file templates](/islandora_workbench_docs/generating_csv_files/#csv-file-templates)" section.

## Applying CSV value templates to rows in your input CSV

In `create` and `update` tasks, you can configure templates that are applied to all the values in a CSV column (or to each subvalue if you have multiple values in a single field) as if the templated text were present in the values in your CSV file. The templates are configured in the `csv_value_templates` option. An example looks like this:

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

This example configuration defines only one field/template pair, but `csv_value_templates` can define multiple field/template pairs, for example:

```yaml
csv_value_templates:
  - field_linked_agent: relators:aut:person:$csv_value
  - field_subject: subject:$csv_value
  - field_local_identifier: SFU-$uuid_string
```

The following template variables are available:

 * `$csv_value`: The verbatim string value of the field.
 * `$file`: The verbatim value of the `file` column in the row.
 * `$filename_without_extension`: The filename portion only (with no leading directory path or file extension) in the `file` column in the row.
 * `$weight`: The value of the `field_weight` column in the CSV, or for paged content created using the "[Using subdirectories](/islandora_workbench_docs/paged_and_compound/#using-subdirectories)" option, the sequence number embedded in the page's filenme.
 * `$random_alphanumeric_string`: A randomly generated string containing numbers and mixed-case letters. This string's default length is 5 characters, but this can be overridden by including `csv_value_templates_rand_length` in your configuration file, e.g. `csv_value_templates_rand_length: 10`.
 * `$random_number_string`: A randomly generated string containing only numbers. This string's default length is 5 characters, but this can be overridden by including `csv_value_templates_rand_length` in your configuration file, e.g. `csv_value_templates_rand_length: 10`.
 * `$uuid_string`: A valid version 4 UUID.

A few things to note about CSV value templates:

* The variable can be anywhere in your template (beginning, middle, or end).
* You can only define a single template for a given field in `csv_value_templates`, but you can include multiple variables in a single template. If multiple variables are present in a template, they are applied in the order listed above.
* If a CSV field contains multiple subvalues, the same template is applied to all subvalues in the field (as illustrated above).
* Values in the templated CSV output are validated against Drupal's configuration in the same way that values present in your CSV are validated.
* By default, CSV value templates won't be applied to empty fields. However, if you want a template to be applied to a field if that field is empty, you can include the `allow_csv_value_templates_if_field_empty` setting in your config file defining a list of column names. For example, `allow_csv_value_templates_if_field_empty: [field_identifier]` will apply any templates defined for `field_identifier` in your `csv_value_templates` setting, even if `field_identifier` is empty in your input CSV; for example, the following will apply the template defined in the above example configuration even if the named fields are empty:

```
allow_csv_value_templates_if_field_empty: ['field_local_identifier', 'field_subject']
```

## Applying CSV value templates to paged content

Paged content (or as sometimes referred to, children) created using the "[Using subdirectories](/islandora_workbench_docs/paged_and_compound/#using-subdirectories)" method do not have their own rows in input CSV files. Any fields that are configured to be "required" in the parent and child's content type are copied from the parent's CSV row and applied to all that parent's pages/children.

If you want to add non-required field data to pages/children, you can use CSV value templates to do that:

* the CSV row that is used as the source of `$csv_value` is the page's (or child's) parent row; in other words, the value of `$csv_value` is inherited from a page/child's parent row
* the `$file` variable is the name of the page/child's filename (and `$filename_without_extension` is derived from this value)
* the `$weight` variable is taken from the page/child's sequence indicator, e.g. a filename of `page-002.jpg` would result in a `$weight` value of "2".

If you want to apply CSV field templates to page/child items using this technique, register the templates in the `csv_value_templates_for_paged_content` config setting. The structure of the field-to-template mappings is identical to those used in the `csv_value_templates` setting as illustrated above. For example:

```yaml
csv_value_templates_for_paged_content:
  - field_linked_agent: relators:aut:person:$csv_value
  - field_edtf_date_issued: $csv_value
  - field_local_identifier: $csv_value-$weight
```

The template is applied to each value in the source field, so

Even though this section documents how to apply templates with variables, you can also apply "templates" to pages/child items that are complete values, that do not use variables. For example, if you want to add the term "Newspapers" to the `field_genre` field in each page in a newspaper issue, you can register that string as your "template", e.g.

```yaml
csv_value_templates_for_paged_content:
  - field_genre: Newspapers
```

If you use a template that contains no variables, you can use multiple hard-coded values (separated with the `|` or whatever you have configure in the `subdelimiter` setting) provided the target field supports multiple values:

```yaml
csv_value_templates_for_paged_content:
  - field_genre: Newspapers|Text
```

