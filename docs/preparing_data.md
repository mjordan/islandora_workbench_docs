Islandora Workbench allows for arranging your input data in a variety of ways. The two basic sets of data you need to preare (depending on what task you are performing) are 1) a CSV file and 2) files that will be used as Drupal media.

## Using an input directory

In this configuration, you define an input directory (identified by the `input_dir` config option) that contains a CSV file with field content (identified by the `input_csv` config option) and any accompanying media files you want to add to the newly created nodes:

```
your_folder/
├── image1.JPG
├── pic_saturday.jpg
├── image-27262.jpg
├── IMG_2958.JPG
├── someimage.jpg
└── metadata.csv
```

The names of the image/PDF/video/etc. files can take any form you want since they are included in the `file` column of the CSV file. Files of any extension are allowed.

This is Islandora Workbench's default configuration. If you do not specify an `input_dir` or an `input_csv`, as illustrated in this minimal configuration file:

```yaml
task: create
host: "http://localhost:8000"
username: admin
password: islandora
```

Workbench will assume your files are in a directory named "input_data" in the same directory as the Workbench script, and that within that directory, your CSV file is named "metadata.csv".

## Using absolute file paths

Both your input CSV file and the values in its `file` column can be absolute paths. In this configuration, your `input_csv` configuration option must specify the absolute path to your input CSV file, and all values within its `file` column may point to a file to be used as the corresponding node's media file. Relative filenames are also allowed, but they will be relative to the directory named in `input_dir`. An example configuration file for this is:

```yaml
task: create
host: "http://localhost:8000"
username: admin
password: islandora
input_csv: /tmp/input.csv
```

## Using a Google Sheet as the input CSV file

With this option, your configuration's `input_csv` option contains the URL to a publicly readable Google Sheet. To do this, simply provide the URL to the Google spreadsheet in your configuration file's `input_csv` option, like this:

```yaml
task: create
host: "http://localhost:8000"
username: admin
password: islandora
input_csv: 'https://docs.google.com/spreadsheets/d/13Mw7gtBy1A3ZhYEAlBzmkswIdaZvX18xoRBxfbgxqWc/edit#gid=0'
```

That's all you need to do. Every time Workbench runs, it fetches the CSV content of the spreadsheet and saves it to a local file in the directory named in your `input_directory` configuration option, and from that point onward in its execution, uses the locally saved version of the spreadsheet. The default filename for this CSV file is `google_sheet.csv` but you can change it if you need to by including the `google_sheets_csv_filename` option in your configuration file, e.g., `google_sheets_csv_filename: my_filename.csv`.

Islandora Workbench fetches a new copy of the CSV data every time it runs (even with the `--check` option), so if you make changes to the contents of that local file, the changes will be overwritten with the data from the Google spreadsheet the next time you run Workbench. If you don't want to overwrite your local copy of the data, rename the local CSV file manually before running Workbench, and update the `input_csv` option in your configuration file accordingly.

An advantage of this constant refreshing of the CSV data is that you can delete all the rows from the Google spreadsheet, repopulate them with new data, and use the same configuration later (or, at least use the same `input_csv` URL).

Note that:

* You can use a Google spreadsheet in all tasks that use a CSV file as input.
* All of the columns required in a local CSV file are also required in the Google spreadsheet.
* The URL in the configuration file needs single or double quotes around it, like any other value that contains a colon.
* You can use either the URL you copy from your browser when you are viewing the spreadsheet (which ends in "/edit#gid=0" or something similar), or the "sharing" URL you copy into your clipboard from within the "Share" dialog box (which ends in "edit?usp=sharing"). Either is OK.
* The Google spreadsheet must be publicly readable, e.g. with "Anyone on the Internet with this link can view" permission.
* Spreadsheets work best for descriptive metadata if all cells are formatted as "Plain text". To do this, in Sheets, select all cells, then choose the menu items Format > Number > Plain text *before adding any content to the cells*.
* The worksheet that the CSV data is taken from is the first one in the spreadsheet (i.e., the one named in the left-most tab).
* The values in the `file` column of the spreadsheet point to files within your local `input_directory`, just like they do in a local CSV input file.

## Blank or missing "file" values

By defualt, if the `file` value for a row is empty, Workbench's `--check` option will show an error. But, in some cases you may want to create a node but not add any media. If you add `allow_missing_files: true` to your config file for "create" tasks, you can leave the `file` cell in your CSV for that item empty.

## Creating nodes but not media

If you want to only create nodes and not media, you can do so by including `nodes_only: true` in your configuration file. More detail [is available](/nodes_only/).
