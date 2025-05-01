In `create` tasks, you can configure Workbench to automatically populate the `field_viewer_override` column in your input CSV using to two different criteria:

1. the extension of the file named in your CSV's `file` column, and
2. the value in your CSV's `field_model` column.

Using this configuration saves you the tedious task of manually populating the `field_viewer_override` field.

The following are examples of how the two settings, `field_viewer_override_models` and `field_viewer_override_extensions` work. Both settings take a list of mappings from terms in the Islandora Display vocabulary on the left to conditions on the right. The conditions are themselves lists of `field_model` values (for mappings in `field_viewer_override_models`) and file extensions (for mappings in `field_viewer_override_extensions`).

An example of using `field_viewer_override_models` is:

```yaml
field_viewer_override_models:
 - http://mozilla.github.io/pdf.js: ["Digital document"]
 - http://openseadragon.github.io: ["Image"]
```

These mappings will populate `field_viewer_override` in a row with "http://mozilla.github.io/pdf.js" if the row's `field_model` value is "Digital document", and with "http://openseadragon.github.io" if the `field_model` value is "Image".

An example of using `field_viewer_override_extensions` is:

```yaml
# Leading periods in the list of file extensions are ignored.
field_viewer_override_extensions:
 - http://mozilla.github.io/pdf.js: ["pdf"]
 - http://openseadragon.github.io: ["tiff",".tif"]
```

The mappings in this example will populate `field_viewer_override` in a row with "http://mozilla.github.io/pdf.js" if the row's `file` value is has an extension "pdf" and with "http://openseadragon.github.io" if the row's `file` value is either "tiff" or "tif". If the file identified in your input CSV's `file` column is a remote file (that is, starting with "http" or "https"), Workbench will get its extension from the file's MIME type provided by the remote web server. Note that leading periods on the extensions listed within this condition are ignored while matching.

Since Workbench simply copies the left-hand value in the mappings (i.e., the term) into the CSV row's `field_viewer_override` cell, you can use term URIs, term names, or term IDs from the Islandora Display terms. For example, this configuration works the same way as if it used term URIs instread of term names:

```yaml
# Leading periods in the list of file extensions are ignored.
field_viewer_override_extensions:
 - PDF.js: ["pdf"]
 - OpenSeadragon: ["tiff",".tif"]
```

A few notes about using these configuration settings:

 - If you include one or both of the configuration settings described above, Workbench will add the `field_viewer_override` column to your input CSV automatically if it is not already present.
 - If a `field_viewer_override` column is present in your input CSV, these configuration settings will populate the empty cells for rows that meet one of the conditions but leave any cells that are already populated intact.
 - If you include one or both of the configuration settings, Workbench will warn you during `--check` (both by printing a line of output and by writing a log entry) that 'You should review `"[path to your preprocessed input CSV file]"` to ensure that values in the "field_viewer_override" column have been correctly assigned based on your configuration settings.'
 - The configuration described here applies to rows in your input CSV. Setting the `field_viewer_override` values for book, newspaper, and other page-model content created using the "[Using subdirectories](/islandora_workbench_docs/paged_and_compound/#using-subdirectories)", where individual pages do not have rows in the input CSV, should still be used for Page-model items as described there. In other words, in the same configuration file you can use `paged_content_page_viewer_override` to define the viewer override for pages and also `field_viewer_override_models` to define the viewer override for the pages' parents described in the input CSV.
 - The strings in the lists on the right-hand side of the mappings are case insensitive. For example, both "Digital Document" and "Digital document" in the `field_model` column in your input CSV will match on the condition `["Digital document"]`, and "PDF" and "pdf" in the `file` column will match the condition `["pdf"]`
 - If you include both `field_viewer_override_models` and `field_viewer_override_extensions` settings in your configuration file, the mappings for `field_viewer_override_extensions` will pertain. If a row in the input CSV matches multiple conditions, the last match (using the order of the mappings in the configuration) pertains.
 - Populating the `field_viewer_override` CSV column as described here happens after [CSV field templates](/islandora_workbench_docs/field_templates/) and [CSV value templates](/islandora_workbench_docs/csv_value_templates/) are applied. Therefore, when configuring your `field_viewer_override_models` and `field_viewer_override_extensions` settings, assume that CSV values populated by the CSV field and CSV value templates will already exist in your CSV by the time your viewer override configurations are applied.