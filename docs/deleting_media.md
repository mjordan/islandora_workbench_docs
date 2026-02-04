### Deleting media using media IDs

!!! note
    Drupal version 10.0.0 and earlier does not allow a user to delete or modify media files unless the user originally created (or is the owner) of the file. This means that if you created a media using "user1" in your Workbench configuration file, only "user1" can delete or modify those files. For `delete_media` tasks, the value of `username` will need to be the same as the username used to create the media. If the username defined in a `delete_media` task is not the same as the Drupal user who owns the files, Drupal will return a `403` response, which you will see in your Workbench logs. Drupal version [10.1.0](https://www.drupal.org/project/drupal/issues/2949017) and higher allows users who don't own a file to delete it.

You can delete media and their associate files by providing a CSV file with a `media_id` column that contains the Drupal IDs of media you want to delete. For example, your CSV file could look like this:

```text
media_id
100
103
104
```

The config file looks like this (note the `task` option is 'delete_media'):

```yaml
task: delete_media
host: "http://localhost:8000"
username: admin
password: islandora
input_csv: delete_media.csv
```

### Deleting media using node IDs

If you want to delete media from specific nodes without having to know the media IDs as described above, you can use the `delete_media_by_node` task. This task takes a list of node IDs as input, like this:

```text
node_id
345
367
246
```

The configuration file for this task looks like this:

```yaml
task: delete_media_by_node
host: https://islandora.dev
username: admin
password: password
input_dir: input_data
input_csv: delete_node_media.csv
```

This configuration will delete all media attached to nodes 345, 367, and 246.

By default, all media attached to the specified nodes are deleted. A "delete_media_by_node" configuration file can include a `delete_media_by_node_media_use_tids` option that lets you specify a list of Islandora Media Use term IDs that a media must have to be deleted:

```yaml
delete_media_by_node_media_use_tids: [17, 1]
```

Before using this option, consult your Islandora's Islandora Media Use vocabulary page at `/admin/structure/taxonomy/manage/islandora_media_use/overview` to get the term IDs you need to use.
