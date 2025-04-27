## Overview

[Islandora Workbench](https://github.com/mjordan/islandora_workbench) is a command-line tool that allows creation, updating, and deletion of Islandora content from CSV data. It is an alternative to using Drupal's built-in Migrate framework for [ingesting Islandora content from CSV files](https://github.com/Islandora/migrate_islandora_csv). Unlike the Migrate tools, Islandora Workbench can be run anywhere - it does not need to run on the Drupal server. Drupal's Migrate framework, however, is much more flexible than Islandora Workbench, and can be extended using plugins in ways that Workbench cannot.

Note that Islandora Workbench is not related in any way to the Drupal contrib module called [Workbench](https://www.drupal.org/project/workbench).

## Features

* Allows creation of Islandora nodes and media, updating of nodes, and deletion of nodes and media from CSV files
* Allows creation of paged/compound content
* Can run from anywhere - it does not need to be run from the Drupal server's command line
* Provides both sensible default configuration values and rich configuration options for power users
* Provides robust data validation functionality
* Supports a variety of Drupal entity field types (text, integer, term reference, typed relation, geolocation)
* Can generate a CSV file template based on Drupal content type
* Can generate a contact sheet from CSV data
* Can use a Google Sheet or an Excel file instead of a CSV file as input
* Allows assignment of Drupal vocabulary terms using term IDs, term names, or term URIs
* Allows creation of new taxonomy terms from CSV field data
* Allows the assignment of URL aliases
* Allows creation of URL redirects
* Allows adding alt text to images and updating of existing alt text
* Supports transmission fixity auditing for media files
* Cross platform (Windows, Mac, and Linux)
* Well documented
* Well tested

## Usage

Within the `islandora_workbench` directory, run the following command, providing the name of your configuration file ("config.yml" in this example):

`./workbench --config config.yml --check`

!!! note
    If you're on Windows, you will likely need to run Workbench by explicitly invoking Python, e.g. `python workbench --config config.yml --check` instead of using `./workbench` as illustrated above.

`--check` [validates](/islandora_workbench_docs/check/) your configuration and input data. Typical output looks like:

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

If your configuration file is not in the same directory as the `workbench` script, use its absolute path, e.g.:

`./workbench --config /home/mark/config.yml --check`

If `--check` hasn't identified any problems, you can then rerun Islandora Workbench without the `--check` option to create the nodes:

`./workbench --config config.yml`

Workbench will then create a node and attached media for each record in your input CSV file. Typical output looks like:

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

If you'd rather not see all this detail, you can set an option in your configuration file to see a progress bar instead:

```
 [==================================>      40.0%                                         ]
```


## License

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/88x31.png" /></a><br />Islandora Workbench's documentation is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>.

## Contributing

Contributions to this documentation are welcome. If you have a suggestion, please open an issue on the Islandora Workbench GitHub repository's [queue](https://github.com/mjordan/islandora_workbench/issues) and tag your issue "documentation".


## Origin story

I've been hacking content management in Islandora for a number of years, and was involved in (or was the principal maintainer of) a number of tools such as [Move to Islandora Kit](https://github.com/MarcusBarnes/mik), [Islandora Datastream CRUD](https://github.com/SFULibrary/islandora_datastream_crud), [Islandora Sample Content Generator](https://github.com/mjordan/islandora_scg), [Islandora Bulk Deleter](https://github.com/mjordan/islandora_bulk_delete) and [Islandora Get CSV](https://github.com/mjordan/islandora_get_csv).

When Drupal 8 came along, I [did some work](https://github.com/mjordan/drupal_field_limit_tester/blob/master/test_results/results.md) to understand the scalability of its REST interface and started playing around with ingesting and modifying content via REST. As a frequent and often frustrated user of the various Islandora 7.x "batch" modules, which for very large ingests required SSH access to the Drupal server's shell, I realized that REST-based tools offered a lot of potential for content management at scale, in scripted envrionments, and in distributed workflows.

The only existing way to perform batch content management in Drupal 8 was using its [Migrate](https://www.drupal.org/migrate) framework, which, while powerful, still required access to the Drupal server's shell. The Islandora community needed something simpler, yet more flexible in terms of where it could be run, than Migrate to empower a wider spectrum of users to manage content in their Islandora repositories than Migrate can.

That's where Islandora Workbench came from.




