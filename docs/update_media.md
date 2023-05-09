You can update media by providing a CSV file with a `media_id` column and other columns representing the fields of the media that you wish to update. Here is a very simple example CSV that updates the file attached to the media with ID 100:

```text
media_id,file
100,test.txt
```
Values in the `media_id` column can be numeric node IDs (as illustrated above), full URLs, or full URL aliases.

The minimum configuration file for "update_media" tasks looks like this (note the `task` option is 'update_media'):

```yaml
task: update_media
host: "http://localhost:8000"
username: admin
password: islandora
input_csv: update_media.csv
```

Currently, the `update_media` routine has support for the following operations:
- Updating files attached to media
- Updating the set of track files attached to media
- Updating the Media Use TIDs associated with media
- Updating the published status of media
- Updating any plain text fields attached to media (including custom ones!)

## Updating Files Attached to Media
**NOTE**: This functionality is currently only supported for media attached to a node.

**NOTE**: This operation will delete the existing file attached to the media and replace it with the file specified in the CSV file.

To update the file attached to a media, you must provide a CSV file with, at minimum, a `media_id` column and a `file` column. The `media_id` column should contain the ID of the media you wish to update, and the `file` column should contain the path to the file you wish to attach to the media. Here is an example CSV that updates the file attached to the media with ID 100:

```text
media_id,file
100,test.txt
```

Values in the `file` column can be paths to files on the local filesystem, full URLs, or full URL aliases.

## Updating the Set of Track Files Attached to Media
**NOTE**: This functionality is currently only supported for media attached to a node.

**NOTE**: This operation will delete all existing track files attached to the media and replace them with the track files specified in the CSV file.

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

## Updating the Media Use TIDs Associated with Media
To update the Media Use TIDs associated with media, you must provide a CSV file with, at minimum, a `media_id` column and a `media_use_tid` column. The `media_id` column should contain the ID of the media you wish to update, and the `media_use_tid` column should contain the TID(s) of the media use term(s) you wish to associate with the media. If a value is not specified for the `media_use_tid` is not specified in a particular row, the value for the `media_use_tid` setting in the configuration file (Service File by defualt) will be used.
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

## Updating the Published Status of Media
To update the published status of media, you must provide a CSV file with, at minimum, a `media_id` column and a `published` column. The `media_id` column should contain the ID of the media you wish to update, and the `published` column should contain one of the following case-insensitive values:


- "1" or "True" (to publish the media)
- "0" or "False" (to unpublish the media)

Here is an example CSV that updates the published status of some media:

```text
media_id,published
100,tRuE
101,0
```

## Updating Any Plain Text Fields Attached to Media
To update any plain text fields attached to media, you must provide a CSV file with, at minimum, a `media_id` column and columns with the machine names of the plain-text fields you wish to update. Note that a "plain text field" must have one of the following field types
- List (text)
- Text (formatted)
- Text (formatted, long)
- Text (formatted, long, with summary)
- Text (plain)
- Text (plain, long)

The `media_id` column should contain the ID of the media you wish to update, and the other columns should contain the values you wish to set for the fields. Here is an example CSV that updates the published status of some media:

```text
media_id,name,field_my_custom_field
100,My Media,My Custom Value
```

## Leaving Fields Unchanged
If you wish to leave a field unchanged, you can leave it blank in the column for that field. Here is an example CSV that updates the published status of some media and leaves others unchanged:

```text
media_id,published
100,1
101,
102,0
```
