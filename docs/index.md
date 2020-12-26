## Overview

Islandora Workbench is a command-line tool that allows creation, updating, and deletion of Islandora content from CSV data. Islandora Workbench is an alternative to using Drupal's built-in Migrate tools for [ingesting Islandora content from CSV files](https://github.com/Islandora/migrate_islandora_csv). Unlike the Migrate tools, Islandora Workbench can be run anywhere - it does not need to run on the Drupal server. Migrate, however, is much more flexible than Islandora Workbench, and can be extended using plugins in ways that Workbench cannot.


!!! note
    This tool is not related in any way to the Drupal contrib module called [Workbench](https://www.drupal.org/project/workbench).

## Features

* Allows creation of Islandora nodes and media, updating of nodes, and deletion of nodes and media from CSV files
* Allows creation of paged/compound content
* Can run from anywhere - it communicates with Drupal via HTTP interfaces
* Provides robust data validation functionality
* Supports a variety of Drupal entity field types (text, integer, term reference, typed relation, geolocation)
* Can provide a CSV file template based on Drupal content type
* Can use a Google Sheet instead of a local CSV file as input
* Allows assignment of Drupal vocabulary terms using term IDs, term names, or term URIs
* Allows creation of new taxonomy terms from CSV field data
* Allows the assignment of URL aliases
* Allows adding alt text to images
* Cross platform (written in Python)
* Well tested
* Well documented
* Provides both sensible default [configuration](/configuration/) values and rich configuation options for power users
* A companion project under development, [Islandora Workbench Desktop](https://github.com/mjordan/islandora_workbench_desktop), will add a graphical user interface that enables users not familiar or comfortable with the command line to use Workbench.

## Usage

`./workbench --config config.yml --check`

`--check` [validates](/check/) your configuration and input data. Typical output looks like:

```
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

If `--check` hasn't identified any problems, you can then rerun Islandora Workbench without the `--check` option to create the nodes:

`./workbench --config config.yml`

Workbench will then create a node and attached media for each record in your input CSV file. Typical output looks like:

```
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

## License

Islandora Workbench, and this documentation, are available under the terms of the Unlicense.

![The Unlicense](https://camo.githubusercontent.com/a0f44681d578ce545f4614325d26eac4036b273d21a61de5293af355cb969bac/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6c6963656e73652d556e6c6963656e73652d626c75652e737667)
