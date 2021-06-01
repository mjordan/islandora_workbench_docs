Islandora Workbench comes with some sample data. Running

`./workbench --config create.yml --check`

will result in the following output:

```text
OK, connection to Drupal at http://localhost:8000 verified.
OK, configuration file has all required values (did not check for optional values).
OK, CSV file input_data/metadata.csv found.
OK, all 5 rows in the CSV file have the same number of columns as there are headers (5).
OK, CSV column headers match Drupal field names.
OK, required Drupal fields are present in the CSV file.
OK, term IDs/names in CSV file exist in their respective taxonomies.
OK, term IDs/names used in typed relation fields in the CSV file exist in their respective taxonomies.
OK, files named in the CSV "file" column are all present.
Configuration and input data appear to be valid.
```

Then running workbench Workbench without `--check` will result in something like:

```text
Node for 'Small boats in Havana Harbour' created at http://localhost:8000/node/52.
+File media for IMG_1410.tif created.
Node for 'Manhatten Island' created at http://localhost:8000/node/53.
+File media for IMG_2549.jp2 created.
Node for 'Looking across Burrard Inlet' created at http://localhost:8000/node/54.
+Image media for IMG_2940.JPG created.
Node for 'Amsterdam waterfront' created at http://localhost:8000/node/55.
+Image media for IMG_2958.JPG created.
Node for 'Alcatraz Island' created at http://localhost:8000/node/56.
+Image media for IMG_5083.JPG created.
```

