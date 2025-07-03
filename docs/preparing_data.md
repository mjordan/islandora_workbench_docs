Islandora Workbench allows you to arrange your input data in a variety of ways. The two basic sets of data you need to prepare (depending on what task you are performing) are:

1. a CSV file, containing data that will populate node fields (or do other things depending on what task you are performing), described [here](/islandora_workbench_docs/fields/)
2. files that will be used as Drupal media.

The options for arranging your data are detailed below.

!!! note
    Some of Workbench's functionality depends on a specific directory structure not described here, for example "[Creating nodes from files](/islandora_workbench_docs/creating_nodes_from_files/)" and "[Creating paged, compound, and collection content](/islandora_workbench_docs/paged_and_compound/)." However, the information on this page applies to the vast majority of Workbench usage.

## Using an input directory

In this configuration, you define an input directory (identified by the `input_dir` config option) that contains a CSV file with field content (identified by the `input_csv` config option) and any accompanying media files you want to add to the newly created nodes:

```text
input_data/
├── image1.JPG
├── pic_saturday.jpg
├── image-27262.jpg
├── IMG_2958.JPG
├── someimage.jpg
└── metadata.csv
```

Here is the same input directory, with some explanation of how the files relate to each other:

```text
input_data/         <-- This is the directory named in the "input_dir" configuration setting.
├── image1.JPG      <-- This and the other JPEG files are named in the "file" column in the CSV file.
├── pic_saturday.jpg
├── image-27262.jpg
├── IMG_2958.JPG
├── someimage.jpg
└── metadata.csv    <-- This is the CSV file named in the "input_csv" configuration setting.
```


The names of the image/PDF/video/etc. files are included in the `file` column of the CSV file. Files with any extension that you can upload to Drupal are allowed. Islandora Workbench reads the CSV file and iterates through it, performing the current task for each record. In this configuration, files other than the CSV and your media files are allowed in this directory (although for some configurations, your input directory should not contain any files that are not going to be ingested).

This is Islandora Workbench's default configuration. If you do not specify an `input_dir` or an `input_csv`, as illustrated in following minimal configuration file, Workbench will assume your files are in a directory named "input_data" in the same directory as the Workbench script, and that within that directory, your CSV file is named "metadata.csv":

```yaml
task: create
host: "http://localhost:8000"
username: admin
password: islandora
```

Workbench ignores the other files in the input directory, and only looks for files in that directory if the filename alone (no directory component) is in `file` column.

```text
workbench           <-- The "workbench" script.
├── input_data/
   ├── image1.JPG
   ├── pic_saturday.jpg
   ├── image-27262.jpg
   ├── IMG_2958.JPG
   ├── someimage.jpg
   └── metadata.csv
```

For example, in this configuration, in the following "metadata.csv" file, Workbench looks for "image1.JPG", "image-27626.jpg", and "someimage.jpg" at "input_data/image1.JPG", "input_data/image1.JPG", and "input_data/someimage.jpg" respectively, relative to the location of the "workbench" script:

```text
id,file,title
001,image1.JPG,A very good file
0002,image-27262.jpg,My cat
003,someimage.jpg,My dog
```

Workbench complete ignores "pic_saturday.jpg" and "IMG_2958.JPG" because they are not named in any of the `file` columns in the "metadata.csv" file.

If the configuration file specified an `input_dir` value, or identified a CSV file in `input_csv`,  Workbench would use those values:

```yaml
task: create
host: "http://localhost:8000"
username: admin
password: islandora
input_dir: myfiles
input_csv: mymetadata.csv
```

```text
workbench         <-- The "workbench" script.
├── myfiles/
   ├── image1.JPG
   ├── pic_saturday.jpg
   ├── image-27262.jpg
   ├── IMG_2958.JPG
   ├── someimage.jpg
   └── mymetadata.csv
```

The value of `input_dir` doesn't need to be relative to the `workbench` script, it can be absolute:

```yaml
task: create
host: "http://localhost:8000"
username: admin
password: islandora
input_dir: /tmp/myfiles
```

```text
├── /tmp/myfiles/
   ├── image1.JPG
   ├── image-27262.jpg
   ├── someimage.jpg
   └── mymetadata.csv
```

```text
id,file,title
001,image1.JPG,A very good file
0002,image-27262.jpg,My cat
003,someimage.jpg,My dog
```

In this case, even though only the CSV `file` entries contain only filenames and no path information, Workbench looks for the image files at "/tmp/myfiles/image1.JPG", "/tmp/myfiles/image1.JPG", and "/tmp/myfiles/someimage.jpg".

## Using absolute file paths

We saw in the previous section that the path specified in your configuration file's `input_dir` configuration option need not be relative to the location of the `workbench` script, it can be absolute. That is also true for both the configuration value of `input_csv` and for the values in your input CSV's `file` column.

You can also mix absolute and relative filenames in the same CSV file, but all relative filenames are considered to be in the directory named in `input_dir`. An example configuration file for this is:

```yaml
task: create
host: "http://localhost:8000"
username: admin
password: islandora
input_dir: media_files
input_csv: /tmp/input.csv
```

And within the `file` column of the CSV, values like:

```text
id,file,title
001,/tmp/mydata/file01.png,A very good file
0002,/home/me/Documents/files/cat.jpg,My cat
003,dog.png,My dog
```

Notice that the `file` values in the first two rows are absolute, but the `file` value in the last row is relative. Workbench will look for that file at "media_files/dog.png".

!!! note
    In general, Workbench doesn't care if any file path used in configuration or CSV data is relative or absolute, but if it's relative, it's relative to the directory where the `workbench` script lives.

!!! note
    Most of the example paths used in this documentation are Linux paths. In general, paths on Mac computers look and work the same way. On Windows, relative paths and absolute paths like `'C:\Users\Mark\Downloads\myfile.pdf'` and UNC paths like `'\\some.windows.file.share.org\share_name\files\myfile.png'` work fine. These paths also work in Workbench configuration files in settings such as `input_dir`. Make sure all paths on Windows are wrapped in *single* (not double) quotes.

## Using URLs as file paths

In the `file` column, you can also use URLs to files, like this:

```text
id,file,title
001,http://www.mysite.com/file01.png,A very good file
0002,https://mycatssite.org/images/cat.jpg,My cat
003,dog.png,My dog
```

[More information](/islandora_workbench_docs/fields/#values-in-the-file-column) is available on using URLs in your `file` column.

## Using a local or remote .zip archive as input data

If you register the location of a local .zip archive or a remote (available over http(s)) zip archive in your configuration file, Workbench will unzip the contents of the archive into the directory defined in your `input_dir` setting:

```
input_data_zip_archives:
  - /tmp/mytest.zip
  - https://myremote.host.org/zips/another_zip.zip
```

The archive is unzipped with its internal directory structure intact; for example, if your zip has the following structure:

```
bpnichol
├── 003 Partial Side A.mp3
└── MsC12.Nichol.Tape15.mp3
```
and your `input_dir` value is "input_data", the archive will be unzipped into:

```
input_data/
   ├──bpnichol
      ├── 003 Partial Side A.mp3
      └── MsC12.Nichol.Tape15.mp3
```

In this case, your CSV `file` column values should include the intermediate directory's path, e.g. `bpnichol/003 Partial Side A.mp3`.

You can also include an input CSV in your zip archive if you want:

```
bpnichol
├── bpn_metadata.csv
├── 003 Partial Side A.mp3
└── MsC12.Nichol.Tape15.mp3
```
Within `input_data`, the unzipped content would look like:

```
input_data/
   ├──bpnichol
      ├── bpn_metadata.csv
      ├── 003 Partial Side A.mp3
      └── MsC12.Nichol.Tape15.mp3
```

Alternatively, all of your files can also be at the root of the zip archive. In that case, they would be unzipped into the directory named in your `input_dir` setting. A zip archive with this structure:

```
├── bpn_metadata.csv
├── 003 Partial Side A.mp3
└── MsC12.Nichol.Tape15.mp3
```
would be unzipped into:

```
input_data/
    ├── bpn_metadata.csv
    ├── 003 Partial Side A.mp3
    └── MsC12.Nichol.Tape15.mp3
```

If you are zipping up directories to create [paged content](/islandora_workbench_docs/paged_and_compound/#using-subdirectories), all of the directories containing page files should be at the root of your zip archive, with no intermediate parent directory:


```
├── rungh.csv
├── rungh_v2_n1-2
│   ├── Vol.2-1-2-001.tif
│   ├── Vol.2-1-2-002.tif
│   ├── Vol.2-1-2-003.tif
│   ├── Vol.2-1-2-004.tif
│   └── Vol.2-1-2-005.tif
└── rungh_v2_n3
    ├── Vol.2-3-01.tif
    ├── Vol.2-3-02.tif
    ├── Vol.2-3-03.tif
    ├── Vol.2-3-04.tif
    ├── Vol.2-3-05.tif
    └── Vol.2-3-07.tif
```

and your `input_dir` value is "input_data", the archive will be unzipped into:

```
input_data/
    ├── rungh.csv
    ├── rungh_v2_n1-2
    │   ├── Vol.2-1-2-001.tif
    │   ├── Vol.2-1-2-002.tif
    │   ├── Vol.2-1-2-003.tif
    │   ├── Vol.2-1-2-004.tif
    │   └── Vol.2-1-2-005.tif
    └── rungh_v2_n3
        ├── Vol.2-3-01.tif
        ├── Vol.2-3-02.tif
        ├── Vol.2-3-03.tif
        ├── Vol.2-3-04.tif
        ├── Vol.2-3-05.tif
        └── Vol.2-3-07.tif
```

This is because if you include `paged_content_from_directories: true` in your configuration file, Workbench looks within your `input_dir` for a directory named after the paged content item's `id` value, without an intermediate directory.

A few things to note if you are using a zip archive as your input data:

- Remote URLs to zip archives do not need to end in ".zip", but the remote files must be directly accessible for downloading without any authentication.
- You can register a single or multiple zip file in your `input_data_zip_archives` setting. Workbench doesn't check for the existence of files at extracted destination paths, so if a file with the same extracted path exists in more than one archive (or is already at a path the same as that of a file from an archive), the file from the last archive in the `input_data_zip_archives` list will overwrite existing files at the same path.
- You can include in your zip archive(s) any files that you want to put in the directory indicated in your `input_dir` config setting, including files named in your CSV `file` column, files named in columns defined by your `additional_files` configuration, or the CSV or Excel file named in your `input_csv` setting (as illustrated in the Rungh example above).
- Workbench will automatically delete the local downloaded copy of remote archive files after extracting them unless you add `delete_zip_archive_after_extraction: false` to your config file.

## Using a Google Sheet as the input CSV file

With this option, your configuration's `input_csv` option contains the URL to a publicly readable Google Sheet. To do this, simply provide the URL to the Google spreadsheet in your configuration file's `input_csv` option, like this:

```yaml
task: create
host: "http://localhost:8000"
username: admin
password: islandora
input_csv: 'https://docs.google.com/spreadsheets/d/13Mw7gtBy1A3ZhYEAlBzmkswIdaZvX18xoRBxfbgxqWc/edit
google_sheets_gid: 430969348
```

That's all you need to do. It's most reliable to have your Google Sheets URLs end in "/edit" and explicitly add the "gid" value using the `google_sheets_gid` config setting.

Every time Workbench runs, it fetches the CSV content of the spreadsheet and saves it to a local file in the directory named in your `input_directory` configuration option, and from that point onward in its execution, uses the locally saved version of the spreadsheet. The default filename for this CSV file is `google_sheet.csv` but you can change it if you need to by including the `google_sheets_csv_filename` option in your configuration file, e.g., `google_sheets_csv_filename: my_filename.csv`.

Islandora Workbench fetches a new copy of the CSV data every time it runs (even with the `--check` option), so if you make changes to the contents of that local file, the changes will be overwritten with the data from the Google spreadsheet the next time you run Workbench. If you don't want to overwrite your local copy of the data, rename the local CSV file manually before running Workbench, and update the `input_csv` option in your configuration file to use the name of the CSV file you copied.

!!! note
    Using a Google Sheet is currently the fastest and most convenient way of managing CSV data for use with Islandora Workbench. Since Sheets saves changes in realtime, and since Workbench fetches a fresh copy of the CSV data every time you run it, it's easy to iterate by making changes to your data in Sheets, running Workbench (don't forget to use `--check` first to identify any problems!), seeing the effects of your changes in the nodes you've just created, [rolling back your nodes](/islandora_workbench_docs/rolling_back/), tweaking your data in Sheets, and starting a new cycle.

    If you are focused on refining your CSV metadata, you can save time by skipping the creation of media by including `nodes_only: true` in your configuration file.

Some things to note about using Google Sheets:

* You can use a Google Sheet in all tasks that use a CSV file as input.
* All of the columns required in a local CSV file are also required in the Google spreadsheet.
* The URL in the configuration file needs single or double quotes around it, like any other value that contains a colon.
* You can use either the URL you copy from your browser when you are viewing the spreadsheet (which ends in "/edit#gid=0" or something similar), or the "sharing" URL you copy into your clipboard from within the "Share" dialog box (which ends in "edit?usp=sharing"). Either is OK, but, as noted above, it's best to have your URL end in "/edit" and explicitly state the gid using the `google_sheets_gid` config setting.
* The Google spreadsheet must be publicly readable, e.g. with "Anyone on the Internet with this link can view" permission.
* Spreadsheets work best for descriptive metadata if all cells are formatted as "Plain text". To do this in Google Sheets, select all cells, then choose the menu items Format > Number > Plain text *before adding any content to the cells*.
* If the values in the `file` column of the spreadsheet are relative, they are assumed to point to files within your local `input_directory`, just like they do in a local CSV input file. However, you can also use absolute file paths and URLs in the `file` column, as described above.

### Selecting a specific worksheet within a Google Sheet

Worksheets within a given Google Sheet are identified by a "gid". If a Sheet has only a single worksheet, its "gid" is "0" (zero):

`https://docs.google.com/spreadsheets/d/1RLrjb5BrlJNaasFIKrKV4l2rw/edit#gid=0`

If you add additional worksheets, they get a randomly generated "gid", such as "1094504353". You can see this "gid" in the URL when you are in the worksheet:

`https://docs.google.com/spreadsheets/d/1RLrjb5BrlJNaasFIKrKV4l2rw/edit#gid=1094504353`

By default, Workbench extracts CSV data from the worksheet with a "gid" of "0". If you want Workbench to extract the CSV data from a specific worksheet that is not the one with a "gid" of "0", specify the "gid" in your configuration file using the `google_sheets_gid` option, like this:

```yaml
task: create
host: "http://localhost:8000"
username: admin
password: islandora
input_csv: 'https://docs.google.com/spreadsheets/d/1RLrjb5BrlJNaasFIKrKV4l2rw/edit?usp=sharing'
google_sheets_gid: 1094504353
```

## Using an Excel file as the input CSV file

With this option, your configuration's `input_csv` option contains the filename of an Excel 2010 (or higher) file, like this:

```yaml
task: create
host: "http://localhost:8000"
username: admin
password: islandora
input_csv: my_file.xlsx
```

Islandora Workbench extracts the content of this file as CSV data, and uses that extracted data as its input the same way it would use a raw CSV file.

Note that:

* You can use an Excel file in all tasks that use a CSV file as input.
* All of the columns required in a local CSV file are also required in the Excel spreadsheet.
* Spreadsheets work best for descriptive metadata if all cells are formatted as "text". To do this, in Excel, select all cells, alt-click on the selected area, then choose the "Format Cells" context menu item. In the "Number" tab, choose "Text", then click on the "OK" button.
* If the values in the `file` column of the spreadsheet are relative, they are assumed to point to files within your local `input_directory`, just like they do in a local CSV input file. However, you can also use absolute file paths and URLs in the `file` column, as described above.

### Selecting a specific worksheet within an Excel file

* The worksheet that the CSV data is taken from is the one named "Sheet1", unless you specify another worksheet using the `excel_worksheet` configuration option. As with Google Sheets, you can tell Workbench to use a specific worksheet in an Excel file. Here is an example of a config file using that setting:


```yaml
task: create
host: "http://localhost:8000"
username: admin
password: islandora
input_csv: my_file.xlsx
excel_worksheet: Second sheet
```

## How Workbench cleans your input data

Regardless of whether your input data is raw CSV, a Google Sheet, or Excel, Workbench applies a small number of cleansing operations on it. These are:

* replaces smart/curly quotes (both double and single) with regular quotes
* replaces multiple whitespaces within strings with a single space
* removes leading and trailing spaces (including newlines).
* removes leading and trailing subdelimter characters (i.e., the value of the `subdelimiter` config setting, default of `|`).

If you do *not* want Workbench to do one or more of these cleanups, include the `clean_csv_values_skip` setting in your configuration, specifying in a list one or more of the following:

* `smart_quotes`
* `inside_spaces`
* `outside_spaces`
* `outside_subdelimiters`


An example of using this configuration setting is:

```
clean_csv_values_skip: ["smart_quotes", "inside_spaces"]
```

## When Workbench skips invalid CSV data

Running `--check` will tell you when any of the data in your CSV file is invalid, or in other words, does not conform to its target Drupal field's configuration and is likely to cause the creation/updating of content to fail. Currently, for the following types of fields, Workbench will validate CSV values and skip values that fail its validation tests, logging that it did so:

* text
* geolocation
* link

Work is [underway](https://github.com/mjordan/islandora_workbench/issues/424) to complete this feature, including skipping of invalid entity reference and typed relation fields.

## Blank or missing "file" values

By default, if the `file` value for a row is empty, Workbench's `--check` option will show an error. But, in some cases you may want to create nodes but not add any media. If you add `allow_missing_files: true` to your config file for "create" tasks, you can leave the `file` column in your CSV empty.

## Preparing to ingest large files

The maximum size of a file that can be ingested by Workbench is determined by server-side configuration settings in PHP, Drupal and Fedora. The settings that need to be adjusted are documented in the [Islandora documentation](https://islandora.github.io/documentation/user-documentation/uploading-large-files/). That documentation drew on the experience of Workbench users as shared in [this Github issue](https://github.com/mjordan/islandora_workbench/issues/353). If you need additional assistance configuring your server to ingest large files, ask for help on the Islandora Slack.

## Creating nodes but not media

If you want to only create nodes and not media, you can do so by including `nodes_only: true` in your configuration file. More detail [is available](/islandora_workbench_docs/nodes_only/).

## Encoding of text files

All text files used as input to Islandora Workbench, including CSV data, files that are going to be use to create "Extracted Text" media such as OCR/hOCR files, and media track files, must use a standard UTF-8 encoding. This is generally not a problem other than if you have created any of these files on Microsoft Windows. Even on Windows, UTF-8 files can contain a Microsoft-specifc feature called a "Byte-order mark", or "BOM". This type of UTF-8 file is not valid; only standard UTF-8 files are.
