You can add media to existing nodes by providing a CSV file with a `node_id` column plus a `file` field that contains the name of the file you want to add:

```text
node_id,file
100,test.txt
```
Values in the `node_id` column can be numeric node IDs (as illustrated above), full URLs, or full URL aliases.

The config file for "add_media" tasks like this (note the `task` option is 'add_media'):

```yaml
task: add_media
host: "http://localhost:8000"
username: admin
password: islandora
input_csv: add_media.csv
# media_use_tid is optional, it defaults to "Original file".
media_use_tid: 21
```

This is the same configuration file using a term URI in `media_use_tid` rather than a term ID:

```yaml
task: add_media
host: "http://localhost:8000"
username: admin
password: islandora
input_csv: add_media.csv
media_use_tid: "http://pcdm.org/use#Transcript"
```

If you want to specify a media_use_tid per CSV row, you can include that column in your CSV:

```text
node_id,file,media_use_tid
100,test.txt,21
110,test2.txt,35
```

If you include `media_use_tid` values in your CSV file, they override the `media_use_tid` value set in your configuration file.
