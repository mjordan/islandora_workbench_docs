You can update media by providing a CSV file with a `media_id` column and other columns representing the fields of the media that you wish to update. Here is a very simple example CSV that updates the file attached to the media with ID 100:

```text
media_id,file
100,test.txt
```
Values in the `media_id` column can be numeric node IDs (as illustrated above), full URLs, or full URL aliases.

The minimum configuration file for "update_media" tasks looks like this (note the `task` option is `update_media`):

```yaml
task: update_media
host: http://islandora.dev
username: admin
password: password
input_csv: update_media.csv
media_type: file
```

`media_type` is required, and its value is the Drupal machine name of the type of media you are updating (e.g. `image`, `document`, `file`, `video`, etc.)

Currently, the `update_media` routine has support for the following operations:

- Updating files attached to media
- Updating the set of track files attached to media
- Updating the Media Use TIDs associated with media
- Updating the published status of media
- Updating custom fields of any supported field type

When updating field values on the media, the `update_mode` configuration option allows you to determine whether the field values are appended or deleted:

* `replace` (the default) will replace all existing values in a field with the values in the input CSV.
* `append` will add values in the input CSV to any existing values in a field.
* `delete` will delete all values in a field.

## Updating files attached to media
!!! note
    Updating files attached to media is currently only supported for media attached to a node.

!!! warning
    This operation will delete the existing file attached to the media and replace it with the file specified in the CSV file. The `update_mode` setting has no effect on replacing files.

To update the file attached to a media, you must provide a CSV file with, at minimum, a `media_id` column and a `file` column. The `media_id` column should contain the ID of the media you wish to update, and the `file` column should contain the path to the file you wish to attach to the media (always use `file` and not the media-type-specific file fieldname). Here is an example CSV that updates the file attached to the media with ID 100:

```text
media_id,file
100,test.txt
```

Values in the `file` column can be paths to files on the local filesystem, full URLs, or full URL aliases.

## Updating the track files attached to media
!!! note
    This functionality is currently only supported for media attached to a node.

!!! note
    This operation will delete all existing track files attached to the media and replace them with the track files specified in the CSV file.

To update the set of track files attached to a media, you must provide a CSV file with, at minimum, a `media_id` column and a column with a name that matches the `media_track_file_fields` setting in the configuration file. By default, the `media_track_file_fields` setting in the configuration file is set to `field_track` for both audio and video. If you have a custom setup that has a different machine name of the field on the media that holds the track file and need to override these defaults, you can do so using the `media_track_file_fields` configuration setting:

```text
media_track_file_fields:
 audio: field_my_custom_track_file_field
 mycustommmedia: field_another_custom_track_file_field
```

For the purposes of this example, we will assume that the `media_track_file_fields` setting is set to `field_track` for both audio and video.

The `media_id` column should contain the ID of the media you wish to update, and the `field_track` column should contain information related to the track files you wish to attach to the media. The `field_track` has a special format, which requires you to specify the following information separated by colons, in exactly the following order:
- The label for the track
- Its "kind" (one of "subtitles", "descriptions", "metadata", "captions", or "chapters")
- The language code for the track ("en", "fr", "es", etc.)
- The absolute path to the track file, which must have the extension ".vtt" (the extension may be in upper or lower case)

Here is an example CSV that updates the set of track files attached to the media with ID 100:

```text
media_id,field_track
100,English Subtitles:subtitles:en:/path/to/subtitles.vtt
```

You may also wish to attach multiple track files to a single media. To do this, you can specify items in the field_track column separated by the subdelimeter specified in the configuration file (by default, this is the pipe character, "|"). Here is an example CSV that updates the set of track files attached to the media with ID 100 to have multiple track files:

```text
media_id,field_track
100,English Subtitles:subtitles:en:/path/to/subtitles.vtt|French Subtitles:subtitles:fr:/path/to/french_subtitles.vtt
```

## Updating the media use TIDs associated with media

To update the Media Use TIDs associated with media, you must provide a CSV file with, at minimum, a `media_id` column and a `media_use_tid` column. The `media_id` column should contain the ID of the media you wish to update, and the `media_use_tid` column should contain the TID(s) of the media use term(s) you wish to associate with the media. If a value is not specified for the `media_use_tid` column in a particular row, the value for the `media_use_tid` setting in the configuration file (Service File by default) will be used.
Here is an example CSV that updates the Media Use TID associated with the media with ID 100:

```text
media_id,media_use_tid
100,17
```

You may also wish to associate multiple Media Use TIDs with a single media. To do this, you can specify items in the `media_use_tid` column separated by the subdelimeter specified in the configuration file (by default, this is the pipe character, "|"). Here is an example CSV that updates the Media Use TIDs associated with the media with ID 100 to have multiple Media Use TIDs:

```text
media_id,media_use_tid
100,17|18
```

Values in the `media_use_tid` column can be the taxonomy term ID of the media use or the taxonomy term URL alias.

## Updating the published status of media

To update the published status of media, you must provide a CSV file with, at minimum, a `media_id` column and a `status` column. The `media_id` column should contain the ID of the media you wish to update, and the `status` column should contain one of the following case-insensitive values:


- "1" or "True" (to publish the media)
- "0" or "False" (to unpublish the media)

Here is an example CSV that updates the published status of some media:

```text
media_id,status
100,tRuE
101,0
```

## Updating custom fields attached to media

To update custom fields attached to media, you must provide a CSV file with, at minimum, a `media_id` column and columns with the machine names of the fields you wish to update.

The `media_id` column should contain the ID of the media you wish to update, and the other columns should contain the values you wish to set for the fields. Here is an example CSV that updates the published status of some media:

```text
media_id,name,field_my_custom_field
100,My Media,My Custom Value
```

## Leaving fields unchanged

If you wish to leave a field unchanged, you can leave it blank in the column for that field. Here is an example CSV that updates the published status of some media and leaves others unchanged:

```text
media_id,status
100,1
101,
102,0
```

## Updating media using node IDs

If you want to update media from specific nodes without having to know the media IDs as described above, you can use the `update_media_by_node` task. The input CSV for this task is the same as the input CSVs described above, but using a `node_id` (containing node IDs, of course) column instead of a `media_id` column.


The configuration file for this task looks like this:

```yaml
task: update_media_by_node
host: https://islandora.dev
username: admin
password: password
input_csv: update_media_by_node.csv

media_type: file
# You must include at least one Media Use term in this setting, otherwise
# Workbench will not update any media on the associated node.
update_media_by_node_media_use_tids: ["Original file"]
```

The `media_type` and `update_media_by_node_media_use_tids` settings are required, since they are necessary to allow Workbench to specify which media on the nodes identified in the input CSV to update. You must include at least one value in this list. You can use Media Use term names, IDs, or URIs, and you can use multple terms. The media on each node matching the `media_type` value and one of the `update_media_by_node_media_use_tids` values will be updated.

Before using this option, consult your Islandora's Islandora Media Use vocabulary page at `/admin/structure/taxonomy/manage/islandora_media_use/overview` to get the term IDs you need to use.
