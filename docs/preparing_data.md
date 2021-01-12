Islandora Workbench allows you to arrange your input data in a variety of ways. The two basic sets of data you need to prepare (depending on what task you are performing) are:

1. a CSV file, containing data that will populate node fields (or do other things depending on what task you are performing)
2. files that will be used as Drupal media.

The options for arranging your data are detailed below.

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


The names of the image/PDF/video/etc. files are included in the `file` column of the CSV file. Files with any extension that you can upload to Drupal are allowed. Islandora Workbench reads the CSV file and iterates throught it, performing the current task for each record. In this configuration, files other than the CSV and your media files are allowed in this directory (although for some configurations, your input directory should not contain any files that are not going to be ingested).

This is Islandora Workbench's default configuration. If you do not specify an `input_dir` or an `input_csv`, as illustrated in following minimal configuration file, Workbench will assume your files are in a directory named "input_data" in the same directory as the Workbench script, and that within that directory, your CSV file is named "metadata.csv":

```yaml
task: create
host: "http://localhost:8000"
username: admin
password: islandora
```

Workbench ignores the other files in the input directory, and only looks for files in that directory if the filenname alone (no directory component) is in `file` column.

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

Workbench complete igonores "pic_saturday.jpg" and "IMG_2958.JPG" because they are not named in any of the `file` columns in the "metadata.csv" file.

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

We saw in the previous section that the path specified in your configuration file's `input_dir` need to be relative to the location of the `workbench` script, it can be absolute. That is also true for both the value to `input_csv` and for the values in your input CSV's `file` column. 

You can also mix absolute and relative filenames in the same CSV file, but all relative filenames are considered to be in the directory named in `input_dir`. An example configuration file for this is:

```yaml
task: create
host: "http://localhost:8000"
username: admin
password: islandora
input_data: media_files
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

## Using URLs as file paths

In the `file` column, you can also use URLs to files, like this:

```text
id,file,title
001,http://www.mysite.com/file01.png,A very good file
0002,https://mycatssite.org/images/cat.jpg,My cat
003,dog.png,My dog
```

[More information](/fields/#values-in-the-file-field) is available on using URLs in your `file` column.

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

Islandora Workbench fetches a new copy of the CSV data every time it runs (even with the `--check` option), so if you make changes to the contents of that local file, the changes will be overwritten with the data from the Google spreadsheet the next time you run Workbench. If you don't want to overwrite your local copy of the data, rename the local CSV file manually before running Workbench, and update the `input_csv` option in your configuration file to use the name of the CSV file you copied.

!!! note
    Using a Google Sheet is currently the fastest and most convenient way of managing CSV data for use with Islandora Workbench. Since Sheets saves changes in realtime, and since Workbench fetches a fresh copy of the CSV data every time you run it, it's easy to iterate over changes in your CSV data by making changes to your data in Sheets, running Workbench (don't forget to use `--check` to identify any problems!), seeing the effects of your changes, [rolling back your nodes](/islandora_workbench_docs/rolling_back/), tweak your data in Sheets, and start a new cycle.

    If you are focused on perfecting your metadata an not media during this iterative process, you can save even more time by skipping the creation of media by including `nodes_only: true` in your configuration file.

Note that:

* You can use a Google spreadsheet in all tasks that use a CSV file as input.
* All of the columns required in a local CSV file are also required in the Google spreadsheet.
* The URL in the configuration file needs single or double quotes around it, like any other value that contains a colon.
* You can use either the URL you copy from your browser when you are viewing the spreadsheet (which ends in "/edit#gid=0" or something similar), or the "sharing" URL you copy into your clipboard from within the "Share" dialog box (which ends in "edit?usp=sharing"). Either is OK.
* The Google spreadsheet must be publicly readable, e.g. with "Anyone on the Internet with this link can view" permission.
* Spreadsheets work best for descriptive metadata if all cells are formatted as "Plain text". To do this in Google Sheets, select all cells, then choose the menu items Format > Number > Plain text *before adding any content to the cells*.
* The worksheet that the CSV data is taken from is the first one in the spreadsheet (i.e., the one named in the left-most tab).
* If the values in the `file` column of the spreadsheet are relative, they are assumed to point to files within your local `input_directory`, just like they do in a local CSV input file. However, you can also use abosolute file paths and URLs in the `file` column, as described above.

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
* The worksheet that the CSV data is taken from is the one named "Sheet1", unless you specify another worksheet using the `excel_worksheet` configuration option.
* If the values in the `file` column of the spreadsheet are relative, they are assumed to point to files within your local `input_directory`, just like they do in a local CSV input file. However, you can also use abosolute file paths and URLs in the `file` column, as described above.


## Blank or missing "file" values

By defualt, if the `file` value for a row is empty, Workbench's `--check` option will show an error. But, in some cases you may want to create a node but not add any media. If you add `allow_missing_files: true` to your config file for "create" tasks, you can leave the `file` cell in your CSV for that item empty.

## Creating nodes but not media

If you want to only create nodes and not media, you can do so by including `nodes_only: true` in your configuration file. More detail [is available](/islandora_workbench_docs/nodes_only/).
